

```c:uart-man.c
/**

 * @file uart-man.c

 * @brief UART Manager Process

 *

 * Handles communication between rs232.c (via POSIX message queues) and MQTT.

 * - Receives JSON messages from rs232.c, extracts data, and sends to MQTT.

 * - Receives MQTT messages (time updates), formats them into JSON, and sends to rs232.c.

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

#include <pthread.h>

#include <cjson/cJSON.h>

#include <MQTTClient.h>

#include "buildroot/buildroot-201902/scom/include/rs232.h" // Include the header defining uart_data_t and MQ names

  

/* MQTT Configuration */

#define MQTT_BROKER_ADDRESS "tcp://localhost:1883" // Replace with your MQTT broker address

#define MQTT_CLIENT_ID      "uart-man-client"

#define MQTT_TIME_TOPIC     "xray/uart-man/time"     // Topic to publish time updates from rs232

#define MQTT_TIME_RSP_TOPIC "xray/uart-man/time/rsp" // Topic to subscribe for time responses from MQTT

#define MQTT_TIMEOUT_MS     5000 // 5 seconds

  

/* POSIX Message Queue Configuration */

#define MQ_MAX_MSG_NUM      10              /* Matches rs232.c */

#define MQ_MSG_PRIO         0               /* Default message priority */

  

/* Global Variables */

volatile sig_atomic_t keep_running = 1;

MQTTClient mqtt_client;

mqd_t mq_rs232_to_uart = -1; // MQ for receiving from rs232.c

mqd_t mq_uart_to_rs232 = -1; // MQ for sending to rs232.c

  

/**

 * @brief Signal handler for graceful shutdown.

 * @param signum Signal number.

 */

void signal_handler(int signum)

{

    syslog(LOG_INFO, "Received signal %d, shutting down...", signum);

    keep_running = 0;

}

  

/**

 * @brief Get current time in ISO 8601 format.

 * @param buffer Buffer to store the timestamp.

 * @param buffer_size Size of the buffer.

 */

void get_iso8601_time(char *buffer, size_t buffer_size)

{

    time_t now = time(NULL);

    struct tm *t = gmtime(&now); // Or localtime(&now) if you prefer local time

    if (t == NULL)

    {

        perror("gmtime/localtime failed");

        snprintf(buffer, buffer_size, "TIME_ERROR");

        return;

    }

    strftime(buffer, buffer_size, "%Y-%m-%dT%H:%M:%SZ", t); // ISO 8601 UTC format

}

  

/**

 * @brief Initialize MQTT client connection and subscription.

 * @return 0 on success, -1 on failure.

 */

int init_mqtt()

{

    MQTTClient_connectOptions conn_opts = MQTTClient_connectOptions_initializer;

    int rc;

  

    rc = MQTTClient_create(&mqtt_client, MQTT_BROKER_ADDRESS, MQTT_CLIENT_ID,

                           MQTTCLIENT_PERSISTENCE_NONE, NULL);

    if (rc != MQTTCLIENT_SUCCESS)

    {

        syslog(LOG_ERR, "Failed to create MQTT client, return code %d", rc);

        return -1;

    }

  

    conn_opts.keepAliveInterval = 20;

    conn_opts.cleansession = 1;

  

    syslog(LOG_INFO, "Connecting to MQTT broker: %s", MQTT_BROKER_ADDRESS);

    rc = MQTTClient_connect(mqtt_client, &conn_opts);

    if (rc != MQTTCLIENT_SUCCESS)

    {

        syslog(LOG_ERR, "Failed to connect to MQTT broker, return code %d", rc);

        MQTTClient_destroy(&mqtt_client);

        return -1;

    }

    syslog(LOG_INFO, "Successfully connected to MQTT broker.");

  

    // Subscribe to the response topic

    syslog(LOG_INFO, "Subscribing to MQTT topic: %s", MQTT_TIME_RSP_TOPIC);

    rc = MQTTClient_subscribe(mqtt_client, MQTT_TIME_RSP_TOPIC, 1); // QoS 1

    if (rc != MQTTCLIENT_SUCCESS)

    {

        syslog(LOG_ERR, "Failed to subscribe to %s, return code %d", MQTT_TIME_RSP_TOPIC, rc);

        MQTTClient_disconnect(mqtt_client, 10000);

        MQTTClient_destroy(&mqtt_client);

        return -1;

    }

    syslog(LOG_INFO, "Successfully subscribed to %s", MQTT_TIME_RSP_TOPIC);

  

    return 0;

}

  

/**

 * @brief Initialize POSIX message queues.

 * @return 0 on success, -1 on failure.

 */

int init_queues()

{

    struct mq_attr attr;

  

    /* Configure queue attributes */

    attr.mq_flags = 0;          // Blocking receive/send

    attr.mq_maxmsg = MQ_MAX_MSG_NUM;

    attr.mq_msgsize = sizeof(uart_data_t); // Use the size defined in rs232.h

    attr.mq_curmsgs = 0;

  

    /* Open queue for receiving messages FROM rs232.c */

    mq_rs232_to_uart = mq_open(MQ_RS232_TO_UART, O_RDONLY | O_CREAT | O_NONBLOCK, 0666, &attr);

    if (mq_rs232_to_uart == (mqd_t)-1)

    {

        syslog(LOG_ERR, "Failed to open queue %s: %s", MQ_RS232_TO_UART, strerror(errno));

        return -1;

    }

    syslog(LOG_INFO, "Message queue %s opened for reading.", MQ_RS232_TO_UART);

  
  

    /* Open queue for sending messages TO rs232.c */

    // Note: Create if not exists, attributes might be ignored if queue already exists

    mq_uart_to_rs232 = mq_open(MQ_UART_TO_RS232, O_WRONLY | O_CREAT, 0666, &attr);

     if (mq_uart_to_rs232 == (mqd_t)-1)

    {

        syslog(LOG_ERR, "Failed to open queue %s: %s", MQ_UART_TO_RS232, strerror(errno));

        mq_close(mq_rs232_to_uart); // Clean up the first queue

        return -1;

    }

    syslog(LOG_INFO, "Message queue %s opened for writing.", MQ_UART_TO_RS232);

  

    return 0;

}

  

/**

 * @brief Handles JSON message received from rs232.c

 * Parses the JSON, extracts data, and publishes to MQTT.

 * @param rx_buffer Received data buffer containing JSON string.

 * @param rx_len Length of the received data.

 */

void handle_rs232_message(const uint8_t *rx_buffer, size_t rx_len)

{

    cJSON *root = NULL;

    cJSON *cmd_item = NULL;

    cJSON *settime_item = NULL;

    char timestamp[64];

    char *mqtt_payload = NULL;

    int rc;

  

    syslog(LOG_DEBUG, "Processing message from rs232.c: %.*s", (int)rx_len, rx_buffer);

  

    // Null-terminate the received data to safely parse as string

    char *json_string = malloc(rx_len + 1);

    if (!json_string)

    {

        syslog(LOG_ERR, "Failed to allocate memory for JSON string buffer");

        return;

    }

    memcpy(json_string, rx_buffer, rx_len);

    json_string[rx_len] = '\0';

  

    root = cJSON_Parse(json_string);

    free(json_string); // Free the temporary buffer

  

    if (!root)

    {

        syslog(LOG_ERR, "Failed to parse JSON from rs232.c: %s", cJSON_GetErrorPtr());

        return;

    }

  

    cmd_item = cJSON_GetObjectItemCaseSensitive(root, "cmd");

    if (!cJSON_IsString(cmd_item) || (cmd_item->valuestring == NULL))

    {

        syslog(LOG_ERR, "JSON from rs232.c missing or invalid 'cmd' field.");

        cJSON_Delete(root);

        return;

    }

  

    // Handle "time" command specifically

    if (strcmp(cmd_item->valuestring, "time") == 0)

    {

        settime_item = cJSON_GetObjectItemCaseSensitive(root, "settime");

        if (!cJSON_IsString(settime_item) || (settime_item->valuestring == NULL))

        {

            syslog(LOG_ERR, "JSON 'time' command missing or invalid 'settime' field.");

            cJSON_Delete(root);

            return;

        }

  

        // Add current timestamp to the JSON object before sending to MQTT

        get_iso8601_time(timestamp, sizeof(timestamp));

        cJSON_AddStringToObject(root, "timestamp", timestamp);

  

        mqtt_payload = cJSON_PrintUnformatted(root);

        if (!mqtt_payload)

        {

             syslog(LOG_ERR, "Failed to generate MQTT payload string.");

             cJSON_Delete(root);

             return;

        }

  

        syslog(LOG_DEBUG, "Publishing to MQTT topic %s: %s", MQTT_TIME_TOPIC, mqtt_payload);

  

        MQTTClient_message pubmsg = MQTTClient_message_initializer;

        pubmsg.payload = mqtt_payload;

        pubmsg.payloadlen = (int)strlen(mqtt_payload);

        pubmsg.qos = 1;

        pubmsg.retained = 0;

        rc = MQTTClient_publishMessage(mqtt_client, MQTT_TIME_TOPIC, &pubmsg, NULL);

  

        if (rc != MQTTCLIENT_SUCCESS)

        {

            syslog(LOG_ERR, "Failed to publish MQTT message, return code %d", rc);

        }

        else

        {

            syslog(LOG_INFO, "Successfully published time update to MQTT.");

        }

  

        free(mqtt_payload); // Free memory allocated by cJSON_PrintUnformatted

    }

    else

    {

        syslog(LOG_WARNING, "Received unknown command from rs232.c: %s", cmd_item->valuestring);

        // Handle other potential commands if necessary

    }

  

    cJSON_Delete(root);

}

  

/**

 * @brief Handles MQTT message received (subscribed topic).

 * Parses the JSON, formats it, and sends to rs232.c via POSIX queue.

 * @param payload Received MQTT payload.

 * @param payloadlen Length of the payload.

 */

void handle_mqtt_message(const void *payload, int payloadlen)

{

    cJSON *root = NULL;

    cJSON *cmd_item = NULL;

    cJSON *current_time_item = NULL;

    cJSON *timestamp_item = NULL;

    char *json_to_send_str = NULL;

    uart_data_t tx_msg;

    int rc;

  

    syslog(LOG_DEBUG, "Processing message from MQTT: %.*s", payloadlen, (const char *)payload);

  

    // Null-terminate payload for safe parsing

    char *json_string = malloc(payloadlen + 1);

     if (!json_string) {

        syslog(LOG_ERR, "Failed to allocate memory for MQTT JSON string buffer");

        return;

    }

    memcpy(json_string, payload, payloadlen);

    json_string[payloadlen] = '\0';

  

    root = cJSON_Parse(json_string);

    free(json_string);

  

    if (!root)

    {

        syslog(LOG_ERR, "Failed to parse JSON from MQTT: %s", cJSON_GetErrorPtr());

        return;

    }

  

    cmd_item = cJSON_GetObjectItemCaseSensitive(root, "cmd");

    current_time_item = cJSON_GetObjectItemCaseSensitive(root, "current_time");

    timestamp_item = cJSON_GetObjectItemCaseSensitive(root, "timestamp"); // Although rs232 might not use it, we forward it

  

    // Basic validation for the time response format

    if (!cJSON_IsString(cmd_item) || (cmd_item->valuestring == NULL) || strcmp(cmd_item->valuestring, "time") != 0)

    {

        syslog(LOG_ERR, "MQTT JSON missing or invalid 'cmd' field (expected 'time').");

        cJSON_Delete(root);

        return;

    }

     if (!cJSON_IsString(current_time_item) || (current_time_item->valuestring == NULL))

     {

        syslog(LOG_ERR, "MQTT JSON missing or invalid 'current_time' field.");

        cJSON_Delete(root);

        return;

     }

      if (!cJSON_IsString(timestamp_item) || (timestamp_item->valuestring == NULL))

     {

        syslog(LOG_ERR, "MQTT JSON missing or invalid 'timestamp' field.");

        cJSON_Delete(root);

        return;

     }

  

    // Prepare the JSON structure to send to rs232.c

    // rs232.c expects: {"time": "YYYY-MM-DDTHH:MM:SSZ"}

    cJSON *json_to_send = cJSON_CreateObject();

    if (!json_to_send) {

        syslog(LOG_ERR, "Failed to create JSON object for rs232.c");

        cJSON_Delete(root);

        return;

    }

    // Use the 'current_time' value from MQTT response for the 'time' field for rs232.c

    cJSON_AddStringToObject(json_to_send, "time", current_time_item->valuestring);

  
  

    json_to_send_str = cJSON_PrintUnformatted(json_to_send);

    cJSON_Delete(json_to_send); // Clean up intermediate JSON object

    cJSON_Delete(root);        // Clean up parsed MQTT JSON

  

    if (!json_to_send_str) {

        syslog(LOG_ERR, "Failed to generate JSON string for rs232.c");

        return;

    }

  

    // Prepare message for POSIX queue

    memset(&tx_msg, 0, sizeof(tx_msg));

    tx_msg.length = strlen(json_to_send_str);

    if (tx_msg.length >= RS232_BUFFER_SIZE)

    {

        syslog(LOG_ERR, "JSON message for rs232.c too large (%zu bytes), max is %d", tx_msg.length, RS232_BUFFER_SIZE -1);

        free(json_to_send_str);

        return;

    }

    memcpy(tx_msg.data, json_to_send_str, tx_msg.length);

    // tx_msg.data is already null-terminated implicitly by memset if length < RS232_BUFFER_SIZE

  

    syslog(LOG_DEBUG, "Sending to queue %s: %s", MQ_UART_TO_RS232, json_to_send_str);

  

    // Send the message to rs232.c

    rc = mq_send(mq_uart_to_rs232, (const char *)&tx_msg, sizeof(uart_data_t), MQ_MSG_PRIO);

    if (rc == -1)

    {

        syslog(LOG_ERR, "Failed to send message to queue %s: %s", MQ_UART_TO_RS232, strerror(errno));

    } else {

        syslog(LOG_INFO, "Successfully sent time response to rs232.c");

    }

  

    free(json_to_send_str);

}

  
  

/**

 * @brief Main function.

 */

int main(void)

{

    struct sigaction sa;

    uart_data_t rx_msg; // Buffer for receiving messages from rs232.c

    ssize_t bytes_read;

    int rc;

  

    /* Initialize syslog */

    openlog("uart-man", LOG_PID | LOG_CONS, LOG_USER);

    syslog(LOG_INFO, "uart-man process starting...");

  

    /* Setup signal handling */

    memset(&sa, 0, sizeof(sa));

    sa.sa_handler = signal_handler;

    sigaction(SIGINT, &sa, NULL);

    sigaction(SIGTERM, &sa, NULL);

  

    /* Initialize POSIX Message Queues */

    if (init_queues() != 0)

    {

        syslog(LOG_ERR, "Failed to initialize message queues. Exiting.");

        closelog();

        return EXIT_FAILURE;

    }

  

    /* Initialize MQTT Client */

    if (init_mqtt() != 0)

    {

        syslog(LOG_ERR, "Failed to initialize MQTT client. Exiting.");

        if (mq_rs232_to_uart != -1) mq_close(mq_rs232_to_uart);

        if (mq_uart_to_rs232 != -1) mq_close(mq_uart_to_rs232);

        closelog();

        return EXIT_FAILURE;

    }

  

    syslog(LOG_INFO, "Initialization complete. Entering main loop.");

  

    /* Main Loop */

    while (keep_running)

    {

        // 1. Check for messages from rs232.c (non-blocking read)

        memset(&rx_msg, 0, sizeof(rx_msg)); // Clear buffer before receiving

        bytes_read = mq_receive(mq_rs232_to_uart, (char *)&rx_msg, sizeof(uart_data_t), NULL);

  

        if (bytes_read >= 0)

        {

            // Successfully received a message from rs232.c

            if (rx_msg.length > 0)

            {

                 syslog(LOG_DEBUG, "Received %ld bytes (data length %zu) from queue %s.", bytes_read, rx_msg.length, MQ_RS232_TO_UART);

                 handle_rs232_message(rx_msg.data, rx_msg.length);

            } else {

                 syslog(LOG_WARNING, "Received message from %s with zero length data.", MQ_RS232_TO_UART);

            }

  

        }

        else

        {

            // mq_receive failed

            if (errno != EAGAIN) // EAGAIN means queue was empty (non-blocking mode) - this is expected

            {

                 syslog(LOG_ERR, "Error receiving from queue %s: %s", MQ_RS232_TO_UART, strerror(errno));

                 // Consider adding a small delay or different error handling here if needed

                 usleep(10000); // Avoid busy-spinning on persistent errors

            }

             // else: Queue was empty, continue loop

        }

  
  

        // 2. Check for messages from MQTT (using MQTTClient_receive)

        char *topicName = NULL;

        int topicLen;

        MQTTClient_message *message = NULL;

  

        // Use timeout to avoid blocking indefinitely, allowing rs232 messages check

        rc = MQTTClient_receive(mqtt_client, &topicName, &topicLen, &message, 100); // 100ms timeout

  

        if (rc == MQTTCLIENT_SUCCESS && message != NULL)

        {

             syslog(LOG_DEBUG, "Received MQTT message on topic: %.*s", topicLen, topicName);

             if (strncmp(topicName, MQTT_TIME_RSP_TOPIC, topicLen) == 0) {

                handle_mqtt_message(message->payload, message->payloadlen);

             } else {

                 syslog(LOG_WARNING, "Received MQTT message on unexpected topic: %.*s", topicLen, topicName);

             }

  

             MQTTClient_freeMessage(&message);

             MQTTClient_free(topicName);

        }

        else if (rc != MQTTCLIENT_SUCCESS && rc != MQTTCLIENT_TOPICNAME_TRUNCATED) // Ignore timeout error for normal operation

        {

             // Handle actual MQTT errors if necessary

             syslog(LOG_ERR, "MQTTClient_receive error: %d", rc);

             // Potentially try to reconnect or handle the error appropriately

             // Example: if rc indicates connection lost, try reconnecting

             if (!MQTTClient_isConnected(mqtt_client)) {

                 syslog(LOG_WARNING, "MQTT connection lost. Attempting to reconnect...");

                 // Add reconnection logic here if needed

                 sleep(5); // Wait before retrying

             } else {

                 usleep(10000); // Small delay on other errors

             }

        }

        // else: Timeout occurred (rc == MQTTCLIENT_SUCCESS but message == NULL) or topic truncated, continue loop

  

        // Small delay to prevent busy-waiting if neither queue nor MQTT has data

         usleep(10000); // 10ms delay - adjust as needed

  

    } /* End main loop */

  
  

    /* Cleanup */

    syslog(LOG_INFO, "Shutting down...");

  

    if (MQTTClient_isConnected(mqtt_client))

    {

        syslog(LOG_INFO, "Unsubscribing from %s", MQTT_TIME_RSP_TOPIC);

        MQTTClient_unsubscribe(mqtt_client, MQTT_TIME_RSP_TOPIC);

        syslog(LOG_INFO, "Disconnecting from MQTT broker...");

        MQTTClient_disconnect(mqtt_client, 10000); // 10 second timeout for disconnect

    }

    MQTTClient_destroy(&mqtt_client);

    syslog(LOG_INFO, "MQTT client destroyed.");

  

    if (mq_rs232_to_uart != (mqd_t)-1)

    {

        mq_close(mq_rs232_to_uart);

        // Optional: mq_unlink(MQ_RS232_TO_UART); // Use unlink only if this process is solely responsible for queue lifetime

        syslog(LOG_INFO, "Closed queue %s", MQ_RS232_TO_UART);

    }

     if (mq_uart_to_rs232 != (mqd_t)-1)

    {

        mq_close(mq_uart_to_rs232);

        // Optional: mq_unlink(MQ_UART_TO_RS232);

        syslog(LOG_INFO, "Closed queue %s", MQ_UART_TO_RS232);

    }

  

    closelog();

    printf("uart-man process terminated gracefully.

");

    return EXIT_SUCCESS;

}
```
