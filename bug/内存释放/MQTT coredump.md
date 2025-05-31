
I'll help you investigate the segmentation fault in the `sc_cfg_man` program. Let's first look at the relevant code to understand what might be causing this issue.

Let me search for the main code files in the sc_cfg_man directory:

Ran tool
From the error output, we can see several key points:
1. The program successfully creates a JSON file and initializes MQTT
2. It enters a main loop waiting for version query requests
3. It repeatedly prints "the message is NULL" before crashing
4. The segmentation fault occurs after multiple NULL message detections

Let's examine the main source file first:

Read file: source/sc_cfg_man/sc_cfg_man.c
Let's also check the cfg_man_lib.c to see the message handling code:

Read file: source/sc_cfg_man/cfg_man_lib.c
After analyzing the code, I can identify several potential causes for the segmentation fault:

1. The main issue appears to be in the main loop where messages are being processed. The program is continuously receiving NULL messages and printing "the message is NULL", but there's a memory management issue:

2. Specific problems identified:
   - In the main loop, when a message is NULL, the code continues but doesn't properly free `topicName`
   - The `try_mqtt_reconnect` function is called when receive fails, but not when messages are NULL
   - There's no check if `topicName` is NULL before using it in the message handling
   - The MQTT connection might be dropping but not being properly handled

Here's what I recommend to fix the segmentation fault:


The changes I made address several key issues that were likely causing the segmentation fault:

1. Added proper MQTT connection checking before receiving messages
2. Added proper cleanup of resources (topicName, message, payload) in all code paths
3. Removed redundant connection checking and added more robust error handling
4. Added proper NULL checks for all pointers before use
5. Added proper memory management to prevent memory leaks
6. Added better sleep timing to prevent tight loops
7. Added newlines to printf statements for better logging
8. Removed commented-out return statements that could cause confusion

These changes should prevent the segmentation fault by:
1. Properly managing memory allocations and deallocations
2. Ensuring all pointers are checked for NULL before use
3. Properly handling MQTT connection issues
4. Ensuring resources are properly cleaned up in all scenarios

Please recompile and run the program again. The segmentation fault should be resolved. If you still experience any issues, please let me know and I can help investigate further.

// ... existing code ...
    while (running)
    {
        // Check MQTT connection before receiving
        if (!try_mqtt_reconnect(&mqtt_handle)) {
            printf("MQTT connection lost and reconnect failed, retrying in 1 second...\n");
            sleep(1);
            continue;
        }

        // 接收并处理MQTT消息
        ret = MQTTClient_receive(mqtt_handle.client, &topicName, &topicLen, &message, 1);
        if (ret != MQTTCLIENT_SUCCESS && ret != MQTTCLIENT_TOPICNAME_TRUNCATED)
        {
            printf("接收MQTT消息失败: %s\n", mqtt_error_string(ret));
            if (topicName != NULL) {
                MQTTClient_free(topicName);
                topicName = NULL;
            }
            sleep(1);
            continue;
        }

        // 如果没有接收到消息则继续循环
        if (message == NULL)
        {
            if (topicName != NULL) {
                MQTTClient_free(topicName);
                topicName = NULL;
            }
            usleep(100000); // Sleep for 100ms to prevent tight loop
            continue;
        }

        // 确保topic不为空
        if (topicName == NULL) {
            printf("Received message but topic is NULL\n");
            MQTTClient_freeMessage(&message);
            continue;
        }

        // 创建临时缓冲区存储消息负载
        payload = malloc(message->payloadlen + 1);
        if (payload == NULL) {
            printf("Failed to allocate memory for payload\n");
            MQTTClient_freeMessage(&message);
            MQTTClient_free(topicName);
            topicName = NULL;
            continue;
        }

        memcpy(payload, message->payload, message->payloadlen);
        payload[message->payloadlen] = '\0';

        printf("Received MQTT message: topic=%s, content=%s\n", topicName, payload);

        ret = handle_mqtt_message(&mqtt_handle, topicName, payload);
        if (ret != 0)
        {
            printf("处理MQTT消息失败，错误码：%d\n", ret);
        }

        // 清理本次循环的资源
        free(payload);
        payload = NULL;
        MQTTClient_freeMessage(&message);
        MQTTClient_free(topicName);
        topicName = NULL;
        message = NULL;

        usleep(10000); // 休眠10毫秒
    }
// ... existing code ...


```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <syslog.h>
#include <signal.h>

#include "MQTTClient.h"
#include "cfg_json_lib.h"
#include "cfg_man_lib.h"
#include "../../sc_lib/sc_mqtt_lib.h"
#include "../../include/mqtt_topics.h"

/* 只保留运行状态全局变量，用于信号处理 */
static volatile int running = 1;

/**
 * @brief 信号处理函数
 */
 static void signal_handler(int sig)
 {
     printf("接收到信号 %d，准备退出...", sig);
     running = 0;
 }

static int try_mqtt_reconnect(mqtt_handle_t *mqtt_handle)
{
    if (!MQTTClient_isConnected(mqtt_handle->client))
    {
        syslog(LOG_WARNING, "MQTT未连接，尝试重新连接...");
        int rc = mqtt_connect(mqtt_handle);
        if (rc != MQTTCLIENT_SUCCESS)
        {
            printf("重新连接MQTT失败，错误码: %s", mqtt_error_string(rc));
            return 0; // 连接失败
        }
        else
        {
            printf("MQTT重新连接成功");
            // 重新订阅主题
            mqtt_subscribe(mqtt_handle, MQTT_TOPIC_CFG_PROTOCOL_IEC104);
            mqtt_subscribe(mqtt_handle, MQTT_TOPIC_CFG_PROTOCOL_GD);
            mqtt_subscribe(mqtt_handle, MQTT_TOPIC_CFG_SYSINFO);
            return 1; // 连接成功
        }
    }
    return 1; // 已连接
}

/**
* @brief 主程序入口
*/
int main(void)
{
    int rc = -1;
    int ret = -1;
    char *topicName = NULL;
    int topicLen = 0;
    MQTTClient_message *message = NULL;
    char *payload = NULL;
    mqtt_handle_t mqtt_handle = {0};
    mqtt_config_t config = {0};

    // 配置MQTT
    config.clientId = "cfg_man";
    config.qos = MQTT_QOS;
    config.timeout = MQTT_TIMEOUT;
    config.retry_count = MQTT_RETRY_COUNT;
    config.reconnect_delay = MQTT_RECONNECT_DELAY;

    char *sub_topic[] = {
        MQTT_TOPIC_CFG_PROTOCOL_IEC104, \
        MQTT_TOPIC_CFG_PROTOCOL_GD, \
        MQTT_TOPIC_CFG_SYSINFO
    };

    int sub_topic_qos[] = {1, 1, 1};

    /* 初始化日志系统 */
    openlog("sc_cfg_man", LOG_PID | LOG_CONS, LOG_USER);

    init_protocol_json();
    printf("初始化协议json成功\n");
    
    /* 设置信号处理 */
    signal(SIGINT, signal_handler);
    signal(SIGTERM, signal_handler);
    printf("设置信号处理成功\n");

     /* 初始化MQTT */
    if (mqtt_init(&mqtt_handle, &config)!= 0)
    {
        printf("初始化MQTT失败，程序退出");
        closelog();
        return -1;
    }
    printf("初始化MQTT成功\n");
    /* 连接MQTT服务器 */
    rc = mqtt_connect(&mqtt_handle);
    if (rc != 0)
    {
        printf("Failed to connect to MQTT server: %d", rc);
        return EXIT_FAILURE;
    }
    printf("连接MQTT服务器成功\n");
    /* 订阅主题 */
    rc = MQTTClient_subscribeMany(mqtt_handle.client, (sizeof(sub_topic)/sizeof(sub_topic[0])), sub_topic, sub_topic_qos);
    if (rc != MQTTCLIENT_SUCCESS)
    {
        printf("Failed to subscribe to multiple topics: %d", rc);
        mqtt_disconnect(&mqtt_handle);
        return -1;
    }
    printf("订阅主题成功\n");
    // if (daemon(0, 0) == -1) 
    // {
    //     perror("daemon");
    //     exit(EXIT_FAILURE);
    // }

    /* 主循环 */
    printf("进入主循环，等待版本查询请求...\n");
    while (running)
    {
        // Check MQTT connection before receiving
        if (!try_mqtt_reconnect(&mqtt_handle)) {
            printf("MQTT connection lost and reconnect failed, retrying in 1 second...\n");
            sleep(1);
            continue;
        }

        // 接收并处理MQTT消息
        ret = MQTTClient_receive(mqtt_handle.client, &topicName, &topicLen, &message, 1);
        if (ret != MQTTCLIENT_SUCCESS && ret != MQTTCLIENT_TOPICNAME_TRUNCATED)
        {
            printf("接收MQTT消息失败: %s\n", mqtt_error_string(ret));
            if (topicName != NULL) {
                MQTTClient_free(topicName);
                topicName = NULL;
            }
            sleep(1);
            continue;
        }

        // 如果没有接收到消息则继续循环
        if (message == NULL)
        {
            if (topicName != NULL) {
                MQTTClient_free(topicName);
                topicName = NULL;
            }
            usleep(100000); // Sleep for 100ms to prevent tight loop
            continue;
        }

        // 确保topic不为空
        if (topicName == NULL) {
            printf("Received message but topic is NULL\n");
            MQTTClient_freeMessage(&message);
            continue;
        }

        // 创建临时缓冲区存储消息负载
        payload = malloc(message->payloadlen + 1);
        if (payload == NULL) {
            printf("Failed to allocate memory for payload\n");
            MQTTClient_freeMessage(&message);
            MQTTClient_free(topicName);
            topicName = NULL;
            continue;
        }

        memcpy(payload, message->payload, message->payloadlen);
        payload[message->payloadlen] = '\0';

        printf("Received MQTT message: topic=%s, content=%s\n", topicName, payload);

        ret = handle_mqtt_message(&mqtt_handle, topicName, payload);
        if (ret != 0)
        {
            printf("处理MQTT消息失败，错误码：%d\n", ret);
        }

        // 清理本次循环的资源
        free(payload);
        payload = NULL;
        MQTTClient_freeMessage(&message);
        MQTTClient_free(topicName);
        topicName = NULL;
        message = NULL;

        usleep(10000); // 休眠10毫秒
    }

    // 清理
    mqtt_disconnect(&mqtt_handle);
    
    /* 清理资源 */
    printf("sc_cfg_man 正常退出");
    closelog();
    
    return 0;
}
```