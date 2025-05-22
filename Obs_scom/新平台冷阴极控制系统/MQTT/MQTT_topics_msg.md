
以下是射线机控制系统各MQTT主题的详细JSON格式规范，包括请求和响应格式。


```mermaid
graph LR
    A[RS232硬件] <--> B[rs232.c] <--> C[uart-man] <--> D[handler app]

```

## 1. 系统控制类主题

### 1.1 配置射线机参数

**主题**: `xray/uart-man/cfg`

**请求格式**:
```json
{
  "cmd": "cfg",
  "params": {
    "voltage": 200.0,     // 电压值，范围160.0-200.0V
    "current": 2000.0,    // 电流值，范围200.0-2000.0mA
    "exposure_time": 1000, // 曝光时间，范围500-3000ms
    "interval_time": 500,  // 曝光间隔时间，范围10-10000ms
    "number": 1           // 曝光次数，范围1-50
  },
  "timestamp": "2025-03-17T10:30:00" // ISO 8601格式时间戳
}
```

**主题**: `xray/uart-man/cfg/rsp`

**响应格式**:
```json
{
  "cmd": "cfg",
  "result": 0,            // 0表示成功，非0表示错误码
  "timestamp": "2025-03-17T10:30:01"
}
```

### 1.2 查询射线机参数

**主题**: `xray/uart-man/query`

**请求格式**:
```json
{
  "cmd": "query",
  "timestamp": "2025-03-17T10:30:05"
}
```

**主题**: `xray/uart-man/query/rsp`

**响应格式**:
```json
{
  "cmd": "query",
  "result": 0,
  "params": {
    "voltage": 200.0,
    "current": 2000.0,
    "exposure_time": 1000,
    "interval_time": 500,
    "number": 1
  },
  "timestamp": "2025-03-17T10:30:06"
}
```

### 1.3 立即曝光命令

**主题**: `xray/uart-man/explosive`

**请求格式**:
```json
{
  "cmd": "opt",
  "timestamp": "2025-03-17T10:31:00"
}
```

**主题**: `xray/uart-man/explosive/rsp`

**响应格式**:
```json
{
  "cmd": "opt",
  "result": 0,
  "timestamp": "2025-03-17T10:31:01"
}
```

### 1.4 延迟曝光命令

**主题**: `xray/uart-man/delay`

**请求格式**:
```json
{
  "cmd": "delay",
  "countdown": 10      // 延迟时间，单位秒，范围0-60
  "timestamp": "2025-03-17T10:32:00"
}
```

**主题**: `xray/uart-man/delay/rsp`

**响应格式**:
```json
{
  "cmd": "delay",
  "result": 0,
  "timestamp": "2025-03-17T10:32:01"
}
```

### 1.5 紧急停止命令

**主题**: `xray/uart-man/stop`

**请求格式**:
```json
{
  "cmd": "emg_stop",
  "timestamp": "2025-03-17T10:33:00"
}
```

**主题**: `xray/uart-man/stop/rsp`

**响应格式**:
```json
{
  "cmd": "emg_stop",
  "result": 0,
  "timestamp": "2025-03-17T10:33:01"
}
```

### 1.6 版本查询

**主题**: `xray/uart-man/version`

**请求格式**:
```json
{
  "cmd": "version",
  "timestamp": "2025-03-17T10:34:00"
}
```

**主题**: `xray/uart-man/version/rsp`

**响应格式**:
```json
{
  "cmd": "version",
  "version": "V2.0",
  "timestamp": "2025-03-17T10:34:01"
}
```

### 1.7 时间更新

**主题**: `xray/uart-man/time`

**请求格式**:
```json
{
  "cmd": "time",
  "settime": "2025-03-17T10:35:00"  // ISO 8601格式时间
  "timestamp": "2025-03-17T10:35:00"
}
```

**主题**: `xray/uart-man/time/rsp`

**响应格式**:
```json
{
  "cmd": "time",
  "current_time": "2025-03-17T10:35:01",
  "timestamp": "2025-03-17T10:35:01"
}
```

## 2. X射线管控制类主题

### 2.1 Modbus命令

**主题**: `xray/modbus/command`

**请求格式**:
```json
{
  "device_addr": 1,      // 设备地址
  "cmd": 6,              // Modbus命令码
  "reg_addr": 0,         // 寄存器地址
  "data_num": 1,         // 数据数量
  "reg_data": [200],     // 寄存器数据
  "timestamp": "2025-03-17T10:36:00"
}
```

**主题**: `xray/modbus/response`

**响应格式**:
```json
{
  "device_addr": 1,
  "cmd": 6,
  "reg_addr": 0,
  "data_num": 1,
  "reg_data": [200],
  "result": 0,           // 0表示成功，非0表示错误码
  "timestamp": "2025-03-17T10:36:01"
}
```

## 3. 显示控制类主题

### 3.1 LCD电压显示

**主题**: `xray/display/lcd/voltage`

**发布格式**:
```json
{
  "voltage": 200.0,      // 电压值
  "timestamp": "2025-03-17T10:37:00"
}
```

### 3.2 LCD电流显示

**主题**: `xray/display/lcd/current`

**发布格式**:
```json
{
  "current": 2000.0,     // 电流值
  "timestamp": "2025-03-17T10:37:01"
}
```

### 3.3 LCD曝光时间显示

**主题**: `xray/display/lcd/exposure_time`

**发布格式**:
```json
{
  "exposure_time": 1000, // 曝光时间，单位ms
  "timestamp": "2025-03-17T10:37:02"
}
```

### 3.4 LCD倒计时显示

**主题**: `xray/display/lcd/countdown`

**发布格式**:
```json
{
  "countdown": 10,       // 倒计时时间，单位秒
  "timestamp": "2025-03-17T10:37:03"
}
```

### 3.5 LCD紧急停止状态显示

**主题**: `xray/display/lcd/emg_stop`

**发布格式**:
```json
{
  "emg_stop": "yes",     // "yes"表示紧急停止状态，"no"表示正常状态
  "timestamp": "2025-03-17T10:37:04"
}
```

### 3.6 LCD WiFi状态显示

**主题**: `xray/display/lcd/wifi`

**发布格式**:
```json
{
  "wifi_status": "connected", // "connected", "disconnected", "connecting"
  "signal_strength": 80,      // 信号强度百分比
  "timestamp": "2025-03-17T10:37:05"
}
```

### 3.7 LCD网络状态显示

**主题**: `xray/display/lcd/network`

**发布格式**:
```json
{
  "network_status": "online", // "online", "offline"
  "ip_address": "192.168.1.100",
  "timestamp": "2025-03-17T10:37:06"
}
```

### 3.8 LCD电池电量显示

**主题**: `xray/display/lcd/battery`

**发布格式**:
```json
{
  "battery_level": 75,   // 电池电量百分比
  "charging": false,     // 是否正在充电
  "timestamp": "2025-03-17T10:37:07"
}
```

## 4. 指示灯控制类主题

### 4.1 红色LED控制

**主题**: `xray/indicator/led/red`

**发布格式**:
```json
{
  "state": "blink",      // "on", "off", "blink"
  "duration": 2000,      // 持续时间，单位ms，0表示一直保持该状态
  "blink_interval": 500, // 闪烁间隔，单位ms
  "timestamp": "2025-03-17T10:38:00"
}
```

### 4.2 绿色LED控制

**主题**: `xray/indicator/led/green`

**发布格式**:
```json
{
  "state": "on",         // "on", "off", "blink"
  "duration": 0,         // 持续时间，单位ms，0表示一直保持该状态
  "blink_interval": 0,   // 闪烁间隔，单位ms
  "timestamp": "2025-03-17T10:38:01"
}
```

## 5. 蜂鸣器控制类主题

### 5.1 蜂鸣器控制

**主题**: `xray/indicator/beep`

**发布格式**:
```json
{
  "state": "on",         // "on", "off", "beep"
  "duration": 2000,      // 持续时间，单位ms
  "beep_interval": 500,  // beep模式下的间隔时间，单位ms
  "timestamp": "2025-03-17T10:39:00"
}
```

## 6. 系统监控类主题

### 6.1 电池电量监控

**主题**: `xray/monitor/battery`

**发布格式**:
```json
{
  "battery_level": 75,   // 电池电量百分比
  "charging": false,     // 是否正在充电
  "voltage": 3.7,        // 电池电压
  "low_battery": false,  // 是否低电量
  "timestamp": "2025-03-17T10:40:00"
}
```

### 6.2 CPU温度监控

**主题**: `xray/monitor/cpu_temp`

**发布格式**:
```json
{
  "cpu_temp": 45.5,      // CPU温度，单位摄氏度
  "timestamp": "2025-03-17T10:40:01"
}
```

## 7. 历史记录类主题

### 7.1 曝光历史记录

**主题**: `xray/record/exposure`

**发布格式**:
```json
{
  "record_id": "exp_20250317104100",
  "voltage": 200.0,
  "current": 2000.0,
  "exposure_time": 1000,
  "start_time": "2025-03-17T10:41:00",
  "end_time": "2025-03-17T10:41:01",
  "status": "completed", // "completed", "aborted", "failed"
  "error_code": 0,
  "timestamp": "2025-03-17T10:41:02"
}
```

### 7.2 配置历史记录

**主题**: `xray/record/config`

**发布格式**:
```json
{
  "record_id": "cfg_20250317104200",
  "voltage": 200.0,
  "current": 2000.0,
  "exposure_time": 1000,
  "interval_time": 500,
  "number": 1,
  "config_time": "2025-03-17T10:42:00",
  "user": "system",
  "source": "network",   // "network", "button", "system"
  "timestamp": "2025-03-17T10:42:01"
}
```

### 7.3 电池电量历史记录

**主题**: `xray/record/battery`

**发布格式**:
```json
{
  "record_id": "bat_20250317104300",
  "battery_level": 75,
  "voltage": 3.7,
  "charging": false,
  "record_time": "2025-03-17T10:43:00",
  "timestamp": "2025-03-17T10:43:01"
}
```

### 7.4 CPU温度历史记录

**主题**: `xray/record/cpu_temp`

**发布格式**:
```json
{
  "record_id": "cpu_20250317104400",
  "cpu_temp": 45.5,
  "record_time": "2025-03-17T10:44:00",
  "timestamp": "2025-03-17T10:44:01"
}
```

## 8. 按键控制类主题

### 8.1 选择按键状态

**主题**: `xray/key/select`

**发布格式**:
```json
{
  "key_state": "pressed", // "pressed", "released"
  "press_time": "2025-03-17T10:45:00",
  "timestamp": "2025-03-17T10:45:01"
}
```

### 8.2 延迟按键状态

**主题**: `xray/key/delay`

**发布格式**:
```json
{
  "key_state": "pressed", // "pressed", "released"
  "press_time": "2025-03-17T10:45:10",
  "timestamp": "2025-03-17T10:45:11"
}
```

### 8.3 停止按键状态

**主题**: `xray/key/stop`

**发布格式**:
```json
{
  "key_state": "pressed", // "pressed", "released"
  "press_time": "2025-03-17T10:45:20",
  "timestamp": "2025-03-17T10:45:21"
}
```

## 通用错误响应格式

对于所有可能返回错误的主题，错误响应格式如下：

```json
{
  "cmd": "命令名称",
  "result": 错误码,     // 非0表示错误
  "error": {
    "code": 错误码,
    "message": "错误描述"
  },
  "timestamp": "时间戳"
}
```

## 错误码定义

| 错误码 | 描述 |
|------|-----|
| 0 | 成功 |
| 1 | 一般错误 |
| 2 | 参数无效 |
| 3 | 设备忙 |
| 4 | 通信超时 |
| 5 | 设备未就绪 |
| 6 | 权限不足 |
| 7 | 资源不可用 |
| 8 | 内部错误 |
| 9 | 未知错误 |
| 10 | 电池电量不足 |
| 11 | 温度过高 |
| 12 | 紧急停止激活 |

以上JSON格式规范涵盖了射线机控制系统所有MQTT主题的数据格式，为系统改造提供了明确的接口定义。
