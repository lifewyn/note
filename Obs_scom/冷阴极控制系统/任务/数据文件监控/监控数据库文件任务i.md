---
time: 2025-02-11
tags:
  - C
  - scom
---
--- 
# 一、需求




---
# 二、Cursor修改以及意见

Ah, now I understand you want to check the database file size instead of disk space. Here's the implementation:

```c
// Add to the defines section
#define JSON_RECORD_STRING "record"

// In create_json_rsp_string function, modify/add the RECORD_ENMU case:
        case RECORD_ENMU:
            if(result_ret >= 0)
            {
                char size_str[32];
                if (result_ret >= 1024) {
                    snprintf(size_str, sizeof(size_str), "%dM", result_ret / 1024);
                } else {
                    snprintf(size_str, sizeof(size_str), "%dK", result_ret);
                }
                cJSON_AddStringToObject(root, JSON_RECORD_STRING, size_str);
            }
            else
            {
                cJSON_AddStringToObject(root, JSON_RECORD_STRING, "error");
            }
            break;
```

```c
// Add these includes if not already present
#include <sys/stat.h>
#include "db_lib.h"

// Add function prototype
int handle_record(struct sc_json_content * json_rec_content_pointer);

// In the switch statement in main(), add:
case RECORD_ENMU:
    syslog(LOG_INFO, "*******************record handler\n");
    handle_ret = handle_record(&json_rec_content);
    // handle_ret will be file size in KB, or -1 for error
    break;

// Add the new handler function
int handle_record(struct sc_json_content * json_rec_content_pointer)
{
    struct stat st;
    
    // Get database file stats
    if (stat(DATABASE_PATH, &st) != 0) {
        syslog(LOG_ERR, "Failed to get database file stats: %s\n", strerror(errno));
        return -1;
    }

    // Convert size to KB and round up
    int size_kb = (st.st_size + 1023) / 1024;  // Round up to nearest KB
    syslog(LOG_INFO, "Database file size: %d KB\n", size_kb);

    return size_kb;  // Return size in KB
}
```

This implementation:

1. Uses the existing JSON parsing infrastructure to handle {"record": 1}
2. Checks the size of /xray.db file
3. Returns the size in a human-readable format:
   - For files < 1MB: returns {"record": "96K"} (example)
   - For files ≥ 1MB: returns {"record": "1M"} (example)
4. Keeps message queue handling in the main loop
5. Uses the existing DATABASE_PATH define from db_lib.h

The flow will be:
1. Receives {"record": 1} from UART
2. Parses JSON using existing infrastructure
3. Identifies as RECORD_ENMU type
4. Calls handle_record() which checks database file size
5. Creates response JSON using existing create_json_rsp_string()
6. Main loop sends response through RS232 queue

The handle_record function returns:
- Positive number: file size in KB
- -1: if there was an error accessing the file

The response formatting is handled in create_json_rsp_string, which converts the KB value to either KB or MB format as appropriate.

All message queue operations remain in the main loop, maintaining clean separation of concerns.

--- 
# 三、测试结果

### 1. 文件存在
#### （1）文件属性

![[Pasted image 20250211134235.png]]
#### （2）运行结果

![[Pasted image 20250211134259.png]]

#### （3）log显示

![[Pasted image 20250211134832.png]]

### 2. 文件不存在
#### （1）文件属性

![[Pasted image 20250211134427.png]]
#### （2）运行结果

![[Pasted image 20250211134454.png]]

#### （3）log显示

![[Pasted image 20250211134534.png]]

### 3. 错误情况
#### （1）异常消息
{"record":20}

#### （2）运行结果

![[Pasted image 20250212112255.png]]

