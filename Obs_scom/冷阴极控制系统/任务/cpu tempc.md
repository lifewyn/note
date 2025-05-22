---
time: 2025-02-19
tags:
  - C
  - scom
---



好的，让我分析一下cpu_tempc.c的主要问题：

- 内存安全和参数检测:
    
    cpu_tempc.c+4
    
     */
    
    void static fill_cpu_temp_msg(struct sc_queue_msg * pointer_msg_content, float cpu_temp)
    
    {
    
        pointer_msg_content->msg_type = SEND_MSG_TYPE;
    
        time(&(pointer_msg_content->sc_msg_obj.msg_time));
    
        pointer_msg_content->sc_msg_obj.msg_content_obj.tempc_obj.msg_sender = CPU_TEMPC;
    
     */
    
    void static fill_cpu_temp_msg(struct sc_queue_msg * pointer_msg_content, float cpu_temp)
    
    {
    
        if (pointer_msg_content == NULL) {
    
            syslog(LOG_ERR, "Invalid pointer in fill_cpu_temp_msg");
    
            return;
    
        }
    
        pointer_msg_content->msg_type = SEND_MSG_TYPE;
    
        time(&(pointer_msg_content->sc_msg_obj.msg_time));
    
        pointer_msg_content->sc_msg_obj.msg_content_obj.tempc_obj.msg_sender = CPU_TEMPC;
    

- 缓冲区溢出风险:
    
    cpu_tempc.c+16-11
    
     */
    
    int static get_cpu_temp(float *cpu_temp)
    
    {
    
        if(cpu_temp == NULL)
    
        {
    
            syslog(LOG_INFO, "%s:The cpu_temp pointer variable is null!",__FUNCTION__);
    
            return -1;
    
        }
    
        FILE *file = fopen(TEMPC_DEV, "r");
    
        if (file == NULL) 
    
        {
    
            perror("Unable to open file /sys/class/thermal/thermal_zone0/temp");
    
        if(cpu_temp == NULL)
    
        {
    
            syslog(LOG_INFO, "%s:The cpu_temp pointer variable is null!",__FUNCTION__);
    
        if (file == NULL) 
    
        {
    
            perror("Unable to open file /sys/class/thermal/thermal_zone0/temp");
    
     */
    
    int static get_cpu_temp(float *cpu_temp)
    
    {
    
        if(cpu_temp == NULL) {
    
            syslog(LOG_ERR, "NULL pointer in get_cpu_temp");
    
            return -1;
    
        }
    
        FILE *file = fopen(TEMPC_DEV, "r");
    

- 函数返回值检查:
    
    cpu_tempc.c+23-10
    
        float cpu_temp = 0;
    
        int cpu_temp_queue_log_id = -1;
    
        struct sc_queue_msg cpu_temp_send_data;
    
        memset(&cpu_temp_send_data, 0, sizeof(struct sc_queue_msg));
    
        int cpu_temp_queue_log_id = -1;
    
        cpu_temp_queue_log_id = get_sc_msg_queue_id(RECORD_REC_QUEUE_KEY);
    
        float cpu_temp = 0;
    
        int ret;
    
        // 检查消息队列创建结果
    
        int cpu_temp_queue_log_id = get_sc_msg_queue_id(RECORD_REC_QUEUE_KEY);
    
        if (cpu_temp_queue_log_id < 0) {
    
            syslog(LOG_ERR, "Failed to create message queue");
    
            return EXIT_FAILURE;
    
        }
    
        struct sc_queue_msg cpu_temp_send_data;
    
        memset(&cpu_temp_send_data, 0, sizeof(struct sc_queue_msg));
    
    3 hidden lines
    

- 添加资源清理:
    
    cpu_tempc.c+15
    
    #include <syslog.h>
    
    #include <sys/types.h>
    
    #include <unistd.h>
    
    #include "../../../../../../../buildroot/buildroot-201902/scom/sc_msg_lib/sc_msg.h"
    
    int static get_cpu_temp(float *cpu_temp);
    
    void static fill_cpu_temp_msg(struct sc_queue_msg * pointer_msg_content, float cpu_temp);
    
    int main()
    
    #include <syslog.h>
    
    #include <sys/types.h>
    
    #include <unistd.h>
    
    #include <signal.h>
    
    #include "../../../../../../../buildroot/buildroot-201902/scom/sc_msg_lib/sc_msg.h"
    
    int static get_cpu_temp(float *cpu_temp);
    
    void static fill_cpu_temp_msg(struct sc_queue_msg * pointer_msg_content, float cpu_temp);
    
    static void cleanup_resources(int queue_id);
    
    int main()
    
    3 hidden lines
    
    17 hidden lines
    
    18 hidden lines
    

主要问题和改进建议：

- 内存安全:

- 添加NULL指针检查

- 添加适当的内存初始化

- 添加资源清理函数

- 缓冲区溢出:

- 使用固定大小的缓冲区

- 使用更安全的字符串处理函数

- 添加长度检查

- 参数检测:

- 添加指针有效性检查

- 添加参数范围检查

- 添加返回值检查

- 函数返回值:

- 所有关键操作添加错误检查

- 适当的错误处理和日志记录

- 统一的返回值约定

你觉得这些改进合理吗？需要我详细解释某个部分吗？

