

```c
#include <stdio.h>

#include <stdlib.h>

#include <string.h>

#include <syslog.h>

#include <unistd.h>

#include <MQTTClient.h>

  

int main()

{

    MQTTClient client;

    MQTTClient_connectOptions conn_opts = MQTTClient_connectOptions_initializer;

    int rc;

  

    // 打开系统日志

    openlog("mqtt_test", LOG_PID | LOG_CONS, LOG_USER);

  

    // 创建MQTT客户端

    rc = MQTTClient_create(&client, "tcp://127.0.0.1:1883", "test_client",

                          MQTTCLIENT_PERSISTENCE_NONE, NULL);

    if (rc != MQTTCLIENT_SUCCESS)

    {

        syslog(LOG_ERR, "创建MQTT客户端失败: %d", rc);

        return -1;

    }

  

    // 设置连接选项

    conn_opts.keepAliveInterval = 180;

    conn_opts.cleansession = 1;

    conn_opts.connectTimeout = 5;

  

    // 尝试连接

    syslog(LOG_INFO, "尝试连接到MQTT服务器...");

    rc = MQTTClient_connect(client, &conn_opts);

    if (rc != MQTTCLIENT_SUCCESS)

    {

        syslog(LOG_ERR, "连接失败: %d", rc);

        MQTTClient_destroy(&client);

        return -1;

    }

  

    syslog(LOG_INFO, "MQTT连接成功!");

    // 断开连接

    MQTTClient_disconnect(client, 10000);

    MQTTClient_destroy(&client);

    syslog(LOG_INFO, "MQTT测试完成");

    return 0;

}
```

