---
time: 2025-03-31
tags:
  - C
---


I'll create uart-man.c for you based on the provided requirements and flow chart.

```c:uart-man.c
/**
* @file uart-man.c
* @brief UART管理进程
* 
* 该进程负责接收rs232进程通过POSIX消息队列发送的数据，转换为MQTT消息发送出去，
* 同时接收MQTT消息，转换后通过POSIX消息队列发送给rs232进程。
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <mqueue.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <syslog.h>
#include <time.h>
#include <signal.h>
#include <cjson/cJSON.h>
#include <MQTTClient.h>

/* 包含rs232.h头文件 */
#include "include/rs232.h"

/* MQTT配置 */
#define MQTT_SERVER_ADDR    "tcp://localhost:1883"
#define MQTT_CLIENT_ID      "uart_man_client"
#define MQTT_TOPIC_SEND     "device/xray/control"
#define MQTT_TOPIC_RECV     "device/xray/response"
#define MQTT_QOS            1
#define MQTT_TIMEOUT        10000L  /* 10秒超时 */

/* 消息队列配置 */
#define MQ_MAX_MSG_NUM      10      /* 消息队列最大消息数量 */

/* 参数范围定义 */
#define VOLTAGE_MIN         160.0
#define VOLTAGE_MAX         200.0
#define CURRENT_MIN         200.0
#define CURRENT_MAX         2000.0
#define EXPOSURE_TIME_MIN   500
#define EXPOSURE_TIME_MAX   3000
#define INTERVAL_TIME_MIN   10
#define INTERVAL_TIME_MAX   10000
#define NUMBER_MIN          1
#define NUMBER_MAX          50

/* 全局变量 */
static volatile int running = 1;  /* 运行标志 */
static MQTTClient mqtt_client;    /* MQTT客户端 */

/**
* @brief 信号处理函数
* 
* @param sig 信号值
*/
static void signal_handler(int sig)
{
    running = 0;
    MQTTClient_disconnect(mqtt_client, 1000);
    syslog(LOG_INFO, "接收到信号 %d，准备退出...", sig);
}

/**
* @brief 获取当前时间的ISO 8601格式字符串
* 
* @param buffer 输出缓冲区
* @param size 缓冲区大小
*/
static void get_iso8601_time(char *buffer, size_t size)
{
    time_t now;
    struct tm *tm_info;
    
    time(&now);
    tm_info = localtime(&now);
    
    strftime(buffer, size, "%Y-%m-%dT%H:%M:%S", tm_info);
}

/**
* @brief 构建错误响应JSON
* 
* @param error_msg 错误消息
* @return 错误JSON字符串，需要调用者释放
*/
static char *build_error_json(const char *error_msg)
{
    cJSON *root = cJSON_CreateObject();
    char time_str[32];
    char *json_str = NULL;
    
    if (!root) {
        return NULL;
    }
    
    get_iso8601_time(time_str, sizeof(time_str));
    
    cJSON_AddStringToObject(root, "status", "error");
    cJSON_AddStringToObject(root, "message", error_msg);
    cJSON_AddStringToObject(root, "timestamp", time_str);
    
    json_str = cJSON_PrintUnformatted(root);
    cJSON_Delete(root);
    
    return json_str;
}

/**
* @brief 处理配置命令
* 
* @param cfg_obj JSON配置对象
* @return 处理结果JSON字符串，需要调用者释放
*/
static char *handle_cfg_command(cJSON *cfg_obj)
{
    cJSON *root = NULL;
    cJSON *params = NULL;
    char time_str[32];
    double voltage, current;
    int exposure_time, interval_time, number;
    char *json_str = NULL;
    
    /* 解析RS232发来的JSON数据 */
    double vol_kV = cJSON_GetObjectItem(cfg_obj, "vol_kV") ? cJSON_GetObjectItem(cfg_obj, "vol_kV")->valuedouble : 0;
    double cur_uA = cJSON_GetObjectItem(cfg_obj, "cur_uA") ? cJSON_GetObjectItem(cfg_obj, "cur_uA")->valuedouble : 0;
    int time_ms = cJSON_GetObjectItem(cfg_obj, "time_ms") ? cJSON_GetObjectItem(cfg_obj, "time_ms")->valueint : 0;
    int gap_s = cJSON_GetObjectItem(cfg_obj, "gap_s") ? cJSON_GetObjectItem(cfg_obj, "gap_s")->valueint : 0;
    int num = cJSON_GetObjectItem(cfg_obj, "num") ? cJSON_GetObjectItem(cfg_obj, "num")->valueint : 0;
    
    /* 转换参数 */
    voltage = vol_kV;
    current = cur_uA;
    exposure_time = time_ms;
    interval_time = gap_s * 1000; /* 转换为毫秒 */
    number = num;
    
    /* 检查参数范围 */
    if (voltage < VOLTAGE_MIN || voltage > VOLTAGE_MAX ||
        current < CURRENT_MIN || current > CURRENT_MAX ||
        exposure_time < EXPOSURE_TIME_MIN || exposure_time > EXPOSURE_TIME_MAX ||
        interval_time < INTERVAL_TIME_MIN || interval_time > INTERVAL_TIME_MAX ||
        number < NUMBER_MIN || number > NUMBER_MAX) {
        return build_error_json("参数超出有效范围");
    }
    
    /* 构建MQTT消息 */
    root = cJSON_CreateObject();
    if (!root) {
        return build_error_json("创建JSON对象失败");
    }
    
    get_iso8601_time(time_str, sizeof(time_str));
    
    cJSON_AddStringToObject(root, "cmd", "cfg");
    
    params = cJSON_CreateObject();
    cJSON_AddItemToObject(root, "params", params);
    
    cJSON_AddNumberToObject(params, "voltage", voltage);
    cJSON_AddNumberToObject(params, "current", current);
    cJSON_AddNumberToObject(params, "exposure_time", exposure_time);
    cJSON_AddNumberToObject(params, "interval_time", interval_time);
    cJSON_AddNumberToObject(params, "number", number);
    
    cJSON_AddStringToObject(root, "timestamp", time_str);
    
    json_str = cJSON_PrintUnformatted(root);
    cJSON_Delete(root);
    
    return json_str;
}

/**
* @brief 处理接收到的RS232 JSON数据
* 
* @param json_data JSON数据字符串
* @return 处理结果JSON字符串，需要调用者释放
*/
static char *process_rs232_json(const char *json_data)
{
    cJSON *root = NULL;
    cJSON *cfg_obj = NULL;
    char *result = NULL;
    
    /* 解析JSON */
    root = cJSON_Parse(json_data);
    if (!root) {
        return build_error_json("JSON解析失败");
    }
    
    /* 检查cfg命令 */
    cfg_obj = cJSON_GetObjectItem(root, "cfg");
    if (cfg_obj) {
        result = handle_cfg_command(cfg_obj);
    } else {
        result = build_error_json("不支持的命令");
    }
    
    cJSON_Delete(root);
    return result;
}

/**
* @brief 解析MQTT响应消息
* 
* @param mqtt_msg MQTT消息
* @param tx_data 发送缓冲区
* @param tx_len 发送长度
* @return 成功返回0，失败返回-1
*/
static int parse_mqtt_response(const char *mqtt_msg, uint8_t *tx_data, size_t *tx_len)
{
    cJSON *root = NULL;
    cJSON *status_obj = NULL;
    char *json_str = NULL;
    
    /* 解析JSON */
    root = cJSON_Parse(mqtt_msg);
    if (!root) {
        syslog(LOG_ERR, "MQTT响应JSON解析失败");
        return -1;
    }
    
    /* 获取状态 */
    status_obj = cJSON_GetObjectItem(root, "status");
    if (!status_obj) {
        syslog(LOG_ERR, "MQTT响应缺少状态字段");
        cJSON_Delete(root);
        return -1;
    }
    
    /* 简单地将整个MQTT响应传递回去 - 可以根据需要修改此处逻辑 */
    json_str = cJSON_PrintUnformatted(root);
    if (json_str) {
        size_t len = strlen(json_str);
        if (len < RS232_BUFFER_SIZE) {
            memcpy(tx_data, json_str, len);
            *tx_len = len;
            free(json_str);
        } else {
            syslog(LOG_ERR, "MQTT响应太大，无法放入发送缓冲区");
            free(json_str);
            cJSON_Delete(root);
            return -1;
        }
    } else {
        syslog(LOG_ERR, "无法序列化MQTT响应JSON");
        cJSON_Delete(root);
        return -1;
    }
    
    cJSON_Delete(root);
    return 0;
}

/**
* @brief 主函数
* 
* @return 程序退出码
*/
int main(void)
{
    struct mq_attr attr;
    mqd_t mq_recv_fd, mq_send_fd;
    uart_data_t uart_data;
    uint8_t rx_data[RS232_BUFFER_SIZE] = {0};
    uint8_t tx_data[RS232_BUFFER_SIZE] = {0};
    size_t tx_len = 0;
    int ret;
    char *mqtt_payload = NULL;
    MQTTClient_connectOptions conn_opts = MQTTClient_connectOptions_initializer;
    MQTTClient_message mqtt_message = MQTTClient_message_initializer;
    MQTTClient_deliveryToken token;
    int mqtt_rc;
    char *mqtt_received_topic = NULL;
    int mqtt_received_topicLen = 0;
    MQTTClient_message *mqtt_received_message = NULL;
    struct sigaction sa;
    
    /* 初始化syslog */
    openlog("uart_man", LOG_PID, LOG_USER);
    
    syslog(LOG_INFO, "UART管理进程启动");
    
    /* 设置信号处理 */
    memset(&sa, 0, sizeof(sa));
    sa.sa_handler = signal_handler;
    sigaction(SIGINT, &sa, NULL);
    sigaction(SIGTERM, &sa, NULL);
    
    /* 设置消息队列属性 */
    attr.mq_flags = 0;
    attr.mq_maxmsg = MQ_MAX_MSG_NUM;
    attr.mq_msgsize = sizeof(uart_data_t);
    attr.mq_curmsgs = 0;
    
    /* 打开接收消息队列 */
    mq_recv_fd = mq_open(MQ_RS232_TO_UART, O_RDONLY | O_CREAT, 0666, &attr);
    if (mq_recv_fd == (mqd_t)-1) {
        syslog(LOG_ERR, "打开接收消息队列失败: %s", strerror(errno));
        closelog();
        return EXIT_FAILURE;
    }
    
    /* 打开发送消息队列 */
    mq_send_fd = mq_open(MQ_UART_TO_RS232, O_WRONLY | O_CREAT, 0666, &attr);
    if (mq_send_fd == (mqd_t)-1) {
        syslog(LOG_ERR, "打开发送消息队列失败: %s", strerror(errno));
        mq_close(mq_recv_fd);
        closelog();
        return EXIT_FAILURE;
    }
    
    /* 创建MQTT客户端 */
    mqtt_rc = MQTTClient_create(&mqtt_client, MQTT_SERVER_ADDR, MQTT_CLIENT_ID,
                               MQTTCLIENT_PERSISTENCE_NONE, NULL);
    if (mqtt_rc != MQTTCLIENT_SUCCESS) {
        syslog(LOG_ERR, "创建MQTT客户端失败，错误码: %d", mqtt_rc);
        mq_close(mq_recv_fd);
        mq_close(mq_send_fd);
        closelog();
        return EXIT_FAILURE;
    }
    
    /* 设置MQTT连接选项 */
    conn_opts.keepAliveInterval = 20;
    conn_opts.cleansession = 1;
    
    /* 连接MQTT服务器 */
    mqtt_rc = MQTTClient_connect(mqtt_client, &conn_opts);
    if (mqtt_rc != MQTTCLIENT_SUCCESS) {
        syslog(LOG_ERR, "连接MQTT服务器失败，错误码: %d", mqtt_rc);
        MQTTClient_destroy(&mqtt_client);
        mq_close(mq_recv_fd);
        mq_close(mq_send_fd);
        closelog();
        return EXIT_FAILURE;
    }
    
    /* 订阅MQTT主题 */
    mqtt_rc = MQTTClient_subscribe(mqtt_client, MQTT_TOPIC_RECV, MQTT_QOS);
    if (mqtt_rc != MQTTCLIENT_SUCCESS) {
        syslog(LOG_ERR, "订阅MQTT主题失败，错误码: %d", mqtt_rc);
        MQTTClient_disconnect(mqtt_client, 10000);
        MQTTClient_destroy(&mqtt_client);
        mq_close(mq_recv_fd);
        mq_close(mq_send_fd);
        closelog();
        return EXIT_FAILURE;
    }
    
    syslog(LOG_INFO, "UART管理进程初始化完成");
    
    /* 主循环 */
    while (running) {
        /* 清空数据缓冲区 */
        memset(&uart_data, 0, sizeof(uart_data_t));
        memset(rx_data, 0, sizeof(rx_data));
        memset(tx_data, 0, sizeof(tx_data));
        tx_len = 0;
        
        /* 从POSIX消息队列接收数据 */
        ret = mq_receive(mq_recv_fd, (char *)&uart_data, sizeof(uart_data_t), NULL);
        if (ret < 0) {
            syslog(LOG_ERR, "从消息队列接收数据失败: %s", strerror(errno));
            continue;
        }
        
        syslog(LOG_DEBUG, "从消息队列接收数据，长度: %ld", uart_data.length);
        
        /* 拷贝接收缓冲区 */
        if (uart_data.length > 0 && uart_data.length <= RS232_BUFFER_SIZE) {
            memcpy(rx_data, uart_data.data, uart_data.length);
            rx_data[uart_data.length] = '\0'; /* 确保字符串结束 */
        } else {
            syslog(LOG_ERR, "接收到的数据长度无效: %ld", uart_data.length);
            continue;
        }
        
        /* 检查JSON数据格式并处理 */
        mqtt_payload = process_rs232_json((const char *)rx_data);
        if (!mqtt_payload) {
            syslog(LOG_ERR, "处理接收数据失败");
            continue;
        }
        
        syslog(LOG_DEBUG, "处理后的MQTT消息: %s", mqtt_payload);
        
        /* 发送MQTT消息 */
        mqtt_message.payload = mqtt_payload;
        mqtt_message.payloadlen = (int)strlen(mqtt_payload);
        mqtt_message.qos = MQTT_QOS;
        mqtt_message.retained = 0;
        
        mqtt_rc = MQTTClient_publishMessage(mqtt_client, MQTT_TOPIC_SEND, &mqtt_message, &token);
        if (mqtt_rc != MQTTCLIENT_SUCCESS) {
            syslog(LOG_ERR, "发送MQTT消息失败，错误码: %d", mqtt_rc);
            free(mqtt_payload);
            continue;
        }
        
        /* 等待发送完成 */
        mqtt_rc = MQTTClient_waitForCompletion(mqtt_client, token, MQTT_TIMEOUT);
        if (mqtt_rc != MQTTCLIENT_SUCCESS) {
            syslog(LOG_ERR, "等待MQTT消息发送完成失败，错误码: %d", mqtt_rc);
        } else {
            syslog(LOG_DEBUG, "MQTT消息发送成功");
        }
        
        free(mqtt_payload);
        
        /* 等待MQTT响应 */
        mqtt_rc = MQTTClient_receive(mqtt_client, &mqtt_received_topic, &mqtt_received_topicLen, 
                                    &mqtt_received_message, 5000);
        if (mqtt_rc == MQTTCLIENT_SUCCESS && mqtt_received_message) {
            /* 处理MQTT响应 */
            if (mqtt_received_message->payloadlen > 0) {
                char *response_data = malloc(mqtt_received_message->payloadlen + 1);
                if (response_data) {
                    memcpy(response_data, mqtt_received_message->payload, mqtt_received_message->payloadlen);
                    response_data[mqtt_received_message->payloadlen] = '\0';
                    
                    syslog(LOG_DEBUG, "收到MQTT响应: %s", response_data);
                    
                    /* 解析MQTT响应并构建发送缓冲区 */
                    if (parse_mqtt_response(response_data, tx_data, &tx_len) == 0) {
                        /* 构建IPC消息并发送给rs232 */
                        memset(&uart_data, 0, sizeof(uart_data_t));
                        memcpy(uart_data.data, tx_data, tx_len);
                        uart_data.length = tx_len;
                        
                        if (mq_send(mq_send_fd, (char *)&uart_data, sizeof(uart_data_t), 0) == -1) {
                            syslog(LOG_ERR, "发送数据到消息队列失败: %s", strerror(errno));
                        } else {
                            syslog(LOG_DEBUG, "发送响应到rs232，长度: %ld", tx_len);
                        }
                    }
                    
                    free(response_data);
                }
            }
            
            /* 释放MQTT消息资源 */
            MQTTClient_freeMessage(&mqtt_received_message);
            MQTTClient_free(mqtt_received_topic);
        } else if (mqtt_rc != MQTTCLIENT_SUCCESS) {
            syslog(LOG_WARNING, "等待MQTT响应超时或失败，错误码: %d", mqtt_rc);
            
            /* 构建错误响应并发送回rs232 */
            mqtt_payload = build_error_json("MQTT响应超时");
            if (mqtt_payload) {
                tx_len = strlen(mqtt_payload);
                if (tx_len < RS232_BUFFER_SIZE) {
                    memcpy(tx_data, mqtt_payload, tx_len);
                    
                    memset(&uart_data, 0, sizeof(uart_data_t));
                    memcpy(uart_data.data, tx_data, tx_len);
                    uart_data.length = tx_len;
                    
                    if (mq_send(mq_send_fd, (char *)&uart_data, sizeof(uart_data_t), 0) == -1) {
                        syslog(LOG_ERR, "发送错误响应到消息队列失败: %s", strerror(errno));
                    }
                    
                    free(mqtt_payload);
                }
            }
        }
    }
    
    /* 清理资源 */
    MQTTClient_unsubscribe(mqtt_client, MQTT_TOPIC_RECV);
    MQTTClient_disconnect(mqtt_client, 10000);
    MQTTClient_destroy(&mqtt_client);
    mq_close(mq_recv_fd);
    mq_close(mq_send_fd);
    
    syslog(LOG_INFO, "UART管理进程正常退出");
    closelog();
    
    return EXIT_SUCCESS;
}
```
Now let's create a header file for the uart-man process to ensure it has the correct includes and definitions:

Read file: include/rs232.h
Let's check the correct path:

Read file: buildroot/buildroot-201902/scom/include/rs232.h
Let's create a Makefile for building the uart-man process:

```makefile:Makefile
CC = gcc
CFLAGS = -Wall -Wextra -g -I buildroot/buildroot-201902/scom/include
LDFLAGS = -lrt -lpthread -lcjson -lpaho-mqtt3c

SRCS = uart-man.c
OBJS = $(SRCS:.c=.o)
TARGET = uart-man

.PHONY: all clean

all: $(TARGET)

$(TARGET): $(OBJS)
	$(CC) $^ -o $@ $(LDFLAGS)

%.o: %.c
	$(CC) -c $< -o $@ $(CFLAGS)

clean:
	rm -f $(OBJS) $(TARGET)
```
Let's update the includes in the uart-man.c file to point to the correct path for the rs232.h:

```c:uart-man.c
/**
* @file uart-man.c
* @brief UART管理进程
* 
* 该进程负责接收rs232进程通过POSIX消息队列发送的数据，转换为MQTT消息发送出去，
* 同时接收MQTT消息，转换后通过POSIX消息队列发送给rs232进程。
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <mqueue.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <syslog.h>
#include <time.h>
#include <signal.h>
#include <cjson/cJSON.h>
#include <MQTTClient.h>

/* 包含rs232.h头文件 */
#include "buildroot/buildroot-201902/scom/include/rs232.h"

// ... existing code ...
```
The uart-man.c implementation is now complete. This file:

1. Creates a process that acts as an intermediary between the RS232 hardware and the MQTT network
2. Receives JSON data from the RS232 hardware via POSIX message queues
3. Validates the parameters and converts them to the MQTT format
4. Sends the converted data to the MQTT broker
5. Waits for MQTT responses and forwards them back to the RS232 hardware

Here's a summary of the key functions in the implementation:

1. `signal_handler`: Handles system signals for clean program termination
2. `get_iso8601_time`: Generates ISO 8601 format timestamps
3. `build_error_json`: Creates error response JSON messages
4. `handle_cfg_command`: Processes configuration commands from RS232
5. `process_rs232_json`: Validates and processes incoming JSON data
6. `parse_mqtt_response`: Parses responses from MQTT for sending back to RS232

The main function:
1. Initializes message queues, MQTT client, and signal handlers
2. Enters the main loop where it:
   - Waits for messages from the RS232 process
   - Validates JSON data
   - Converts data to MQTT format
   - Sends MQTT messages
   - Receives MQTT responses
   - Forwards responses back to RS232

To build and run the program:
1. Use the provided Makefile with the command `make`
2. Run the executable with `./uart-man`

The implementation follows the flowchart you provided and handles all the required processing of data between the RS232 hardware and MQTT.
