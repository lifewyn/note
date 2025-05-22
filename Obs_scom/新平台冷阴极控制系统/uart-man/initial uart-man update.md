---
time: 2025-03-31
tags:
  - C
---


I'll reimplement the uart-man.c file according to the updated flowchart, where we directly handle the command without JSON validation steps.

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

/* MQTT配置 */
#define MQTT_SERVER_ADDR    "tcp://localhost:1883"
#define MQTT_CLIENT_ID      "uart_man_client"
#define MQTT_TOPIC_SEND     "device/xray/control"
#define MQTT_TOPIC_RECV     "device/xray/response"
#define MQTT_QOS            1
#define MQTT_TIMEOUT        10000L  /* 10秒超时 */

/* 消息队列配置 */
#define MQ_MAX_MSG_NUM      10      /* 消息队列最大消息数量 */

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
* @brief 处理命令并创建MQTT消息
* 
* @param json_data 接收到的JSON数据
* @return 需要发送的MQTT消息JSON字符串，需要调用者释放
*/
static char *handle_command(const char *json_data)
{
    cJSON *root = NULL;
    cJSON *cfg_obj = NULL;
    cJSON *mqtt_root = NULL;
    cJSON *params = NULL;
    char time_str[32];
    char *mqtt_payload = NULL;
    
    /* 解析接收的JSON数据 */
    root = cJSON_Parse(json_data);
    if (!root) {
        syslog(LOG_ERR, "JSON解析失败");
        return NULL;
    }
    
    /* 查找cfg对象 */
    cfg_obj = cJSON_GetObjectItem(root, "cfg");
    if (!cfg_obj) {
        syslog(LOG_ERR, "未找到cfg命令");
        cJSON_Delete(root);
        return NULL;
    }
    
    /* 提取参数 */
    double vol_kV = cJSON_GetObjectItem(cfg_obj, "vol_kV") ? 
                    cJSON_GetObjectItem(cfg_obj, "vol_kV")->valuedouble : 0;
    double cur_uA = cJSON_GetObjectItem(cfg_obj, "cur_uA") ? 
                    cJSON_GetObjectItem(cfg_obj, "cur_uA")->valuedouble : 0;
    int time_ms = cJSON_GetObjectItem(cfg_obj, "time_ms") ? 
                 cJSON_GetObjectItem(cfg_obj, "time_ms")->valueint : 0;
    int gap_s = cJSON_GetObjectItem(cfg_obj, "gap_s") ? 
               cJSON_GetObjectItem(cfg_obj, "gap_s")->valueint : 0;
    int num = cJSON_GetObjectItem(cfg_obj, "num") ? 
             cJSON_GetObjectItem(cfg_obj, "num")->valueint : 0;
    
    /* 创建MQTT消息 */
    mqtt_root = cJSON_CreateObject();
    if (!mqtt_root) {
        syslog(LOG_ERR, "创建MQTT JSON对象失败");
        cJSON_Delete(root);
        return NULL;
    }
    
    /* 获取当前时间 */
    get_iso8601_time(time_str, sizeof(time_str));
    
    /* 构建MQTT消息结构 */
    cJSON_AddStringToObject(mqtt_root, "cmd", "cfg");
    
    params = cJSON_CreateObject();
    cJSON_AddItemToObject(mqtt_root, "params", params);
    
    cJSON_AddNumberToObject(params, "voltage", vol_kV);
    cJSON_AddNumberToObject(params, "current", cur_uA);
    cJSON_AddNumberToObject(params, "exposure_time", time_ms);
    cJSON_AddNumberToObject(params, "interval_time", gap_s * 1000);  /* 转换为毫秒 */
    cJSON_AddNumberToObject(params, "number", num);
    
    cJSON_AddStringToObject(mqtt_root, "timestamp", time_str);
    
    /* 转换为字符串 */
    mqtt_payload = cJSON_PrintUnformatted(mqtt_root);
    
    /* 清理 */
    cJSON_Delete(root);
    cJSON_Delete(mqtt_root);
    
    return mqtt_payload;
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
    
    /* 订阅MQTT响应主题 */
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
        /* 清空数据 */
        memset(&uart_data, 0, sizeof(uart_data_t));
        memset(rx_data, 0, sizeof(rx_data));
        memset(tx_data, 0, sizeof(tx_data));
        tx_len = 0;
        
        /* 等待来自rs232.c的POSIX消息 */
        ret = mq_receive(mq_recv_fd, (char *)&uart_data, sizeof(uart_data_t), NULL);
        if (ret < 0) {
            syslog(LOG_ERR, "从消息队列接收数据失败: %s", strerror(errno));
            continue;
        }
        
        /* 拷贝接收buffer */
        if (uart_data.length > 0 && uart_data.length <= RS232_BUFFER_SIZE) {
            memcpy(rx_data, uart_data.data, uart_data.length);
            rx_data[uart_data.length] = '\0';  /* 确保字符串结束 */
            syslog(LOG_DEBUG, "接收到RS232数据: %s", rx_data);
        } else {
            syslog(LOG_ERR, "接收到的数据长度无效: %ld", uart_data.length);
            continue;
        }
        
        /* 判断命令类型并调用命令处理函数 */
        mqtt_payload = handle_command((const char *)rx_data);
        if (!mqtt_payload) {
            syslog(LOG_ERR, "处理命令失败");
            continue;
        }
        
        syslog(LOG_DEBUG, "准备发送MQTT消息: %s", mqtt_payload);
        
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
            syslog(LOG_ERR, "MQTT消息发送完成等待失败，错误码: %d", mqtt_rc);
        } else {
            syslog(LOG_DEBUG, "MQTT消息发送成功");
        }
        
        free(mqtt_payload);
        
        /* 等待MQTT消息 */
        mqtt_rc = MQTTClient_receive(mqtt_client, &mqtt_received_topic, &mqtt_received_topicLen, 
                                   &mqtt_received_message, 5000);
        if (mqtt_rc == MQTTCLIENT_SUCCESS && mqtt_received_message) {
            /* 判断返回值并构建发送buffer */
            if (mqtt_received_message->payloadlen > 0) {
                if (mqtt_received_message->payloadlen < RS232_BUFFER_SIZE) {
                    /* 直接复制接收到的数据 */
                    memcpy(tx_data, mqtt_received_message->payload, mqtt_received_message->payloadlen);
                    tx_len = mqtt_received_message->payloadlen;
                    
                    /* 确保结束符 */
                    tx_data[tx_len] = '\0';
                    
                    syslog(LOG_DEBUG, "收到MQTT响应: %s", (char *)tx_data);
                    
                    /* 构建POSIX消息 */
                    memset(&uart_data, 0, sizeof(uart_data_t));
                    memcpy(uart_data.data, tx_data, tx_len);
                    uart_data.length = tx_len;
                    
                    /* 发送消息给rs232.c */
                    if (mq_send(mq_send_fd, (char *)&uart_data, sizeof(uart_data_t), 0) < 0) {
                        syslog(LOG_ERR, "发送数据到消息队列失败: %s", strerror(errno));
                    } else {
                        syslog(LOG_DEBUG, "成功发送响应到RS232，长度: %ld", tx_len);
                    }
                } else {
                    syslog(LOG_ERR, "接收到的MQTT消息太大: %d bytes", mqtt_received_message->payloadlen);
                }
            }
            
            /* 释放MQTT消息资源 */
            MQTTClient_freeMessage(&mqtt_received_message);
            MQTTClient_free(mqtt_received_topic);
        } else {
            syslog(LOG_WARNING, "等待MQTT响应超时或错误，错误码: %d", mqtt_rc);
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
Let's update the Makefile to reflect any changes in the dependencies if needed:

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
I've reimplemented the uart-man.c program according to the simplified flow diagram you provided. Here's an overview of what the updated implementation does:

1. **Initialization**
   - Sets up POSIX message queues for communication with rs232.c
   - Initializes the MQTT client and connects to the broker
   - Subscribes to the MQTT response topic

2. **Main Loop Process Flow**
   - Clears all data buffers at the start of each cycle
   - Waits for and receives POSIX messages from rs232.c
   - Copies the received data to a local buffer
   - Processes the command directly without extensive validation
   - Converts the command format to the MQTT format
   - Sends the MQTT message to the broker
   - Waits for a response from the MQTT broker
   - When a response is received, copies it directly to the transmit buffer
   - Constructs a POSIX message and sends it back to rs232.c
   - Loops back to the beginning

Key differences in this implementation:

1. **Simplified Command Handling**: Removed the multi-step validation process and directly processes the command
2. **Direct Response Handling**: Instead of parsing the MQTT response in detail, it directly forwards the response to rs232.c
3. **Removed Error Handling Complexity**: Simplified error handling to basic logging and continuation

This implementation follows your updated flowchart exactly, providing a more streamlined process for handling communication between rs232.c and MQTT.

To build the program, you can still use the same Makefile with the command `make`. The dependencies remain the same as they're all still required for the core functionality.
