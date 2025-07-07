



```c
#include <stdio.h>

#include <stdlib.h>

#include <signal.h>

#include <unistd.h>

#include <syslog.h>

#include <time.h>

#include <string.h>

#include <cjson/cJSON.h>

  

#include "mqtt_lib.h"

#include "mqtt_config.h"

#include "mqtt_topics.h"

  

/* 信号相关全局变量 */

volatile sig_atomic_t running = 1;

  

/* 信号处理函数 */

void signal_handler(int sig)

{

    if (sig == SIGINT || sig == SIGTERM)

    {

        running = 0;

    }

}

  

/* 获取当前时间戳字符串 */

char* get_timestamp(void)

{

    time_t now = 0;

    struct tm *timeinfo = NULL;

    static char buffer[30] = {0};

    time(&now);

    timeinfo = localtime(&now);

    if (timeinfo == NULL)

    {

        return NULL;

    }

    strftime(buffer, sizeof(buffer), "%Y-%m-%dT%H:%M:%S", timeinfo);

    return buffer;

}

  

int main(int argc, char *argv[])

{

    int rc = 0;

    mqtt_handle_t handle = {0};

    mqtt_config_t config = {

        .clientId = "tube_heat_timer",

        .topic = NULL,

        .rsp_topic = NULL,

        .qos = MQTT_QOS,

        .timeout = MQTT_TIMEOUT,

        .retry_count = MQTT_RETRY_COUNT,

        .reconnect_delay = MQTT_RECONNECT_DELAY

    };

  

    /* 设置信号处理 */

    signal(SIGINT, signal_handler);

    signal(SIGTERM, signal_handler);

  

    // 打开系统日志

    openlog("sc_tube_heat_timer", LOG_PID | LOG_CONS, LOG_USER);

    syslog(LOG_INFO, "启动sc_tube_heat_timer进程");

  

    if (daemon(0, 0) == -1)

    {

        syslog(LOG_ERR, "daemon: %m");

        exit(EXIT_FAILURE);

    }

    /* 初始化MQTT */

    rc = mqtt_init(&handle, &config);

    if (rc != 0)

    {

        syslog(LOG_ERR, "MQTT初始化失败，错误码: %d", rc);

        return EXIT_FAILURE;

    }

    /* 连接MQTT服务器 */

    rc = mqtt_connect(&handle);

    if (rc != 0)

    {

        syslog(LOG_ERR, "连接MQTT服务器失败，错误码: %d", rc);

        return EXIT_FAILURE;

    }

    syslog(LOG_INFO, "sc_tube_heat_timer进程启动，开始发送定时脉冲...");

    /* 主循环 - 定时发送脉冲消息 */

    while (running)

    {

        // 创建状态消息

        cJSON *root = cJSON_CreateObject();

  

        if (root)

        {

            cJSON_AddStringToObject(root, "timestamp", get_timestamp());

  

            char *json_string = cJSON_Print(root);

            if (json_string)

            {

                int rc = mqtt_publish(&handle, MQTT_TOPIC_HEAT_DECREASE_PULSE, json_string);

                if (rc == 0)

                {

                    syslog(LOG_DEBUG, "发送定时脉冲消息成功");

                }

                else

                {

                    syslog(LOG_ERR, "发送定时脉冲消息失败，错误码: %d", rc);

                }

  

                free(json_string);

            }

  

            cJSON_Delete(root);

        }

  

        // 休眠1秒

        sleep(1);

    }

  

    /* 断开连接 */

    mqtt_disconnect(&handle);

    syslog(LOG_INFO, "sc_tube_heat_timer进程已关闭");

    return EXIT_SUCCESS;

}
```

