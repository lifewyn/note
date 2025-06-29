



## 整体架构

```mermaid
graph TB
    subgraph "ETH-App"
        A1[Socket接收]
        A2[Socket发送]
    end
    
    subgraph "ETH-Man"
        B1[数据包解析]
        B2[协议识别]
        B3[数据分发]
        B4[连接管理]
    end
    
    subgraph "MQTT Broker"
        C1[Topic: embedded/network/eth/data]
        C2[Topic: embedded/network/eth/command]
        C3[Topic: embedded/network/eth/status]
    end
    
    subgraph "IEC104-Man"
        D1[IEC104协议栈]
        D2[ASDU处理]
        D3[传感器数据管理]
        D4[命令处理]
    end
    
    A1 --> B1
    B1 --> B2
    B2 --> B3
    B3 --> C1
    C1 --> D1
    D1 --> D2
    D2 --> D3
    D3 --> C2
    C2 --> B4
    B4 --> A2
```

## 详细数据交流逻辑

### 1. 数据上行流程（主站 → 传感器数据）

```mermaid
sequenceDiagram
    participant Master as 主站
    participant ETH_App as ETH-App
    participant ETH_Man as ETH-Man
    participant MQTT as MQTT Broker
    participant IEC104 as IEC104-Man
    participant HAL as HAL

    Master->>ETH_App: TCP数据包
    ETH_App->>ETH_Man: 原始网络数据
    ETH_Man->>ETH_Man: 解析数据包
    ETH_Man->>ETH_Man: 识别为IEC104协议
    ETH_Man->>MQTT: 发布 embedded/network/eth/data
    MQTT->>IEC104: 订阅数据
    IEC104->>IEC104: 解析IEC104协议
    IEC104->>IEC104: 处理ASDU
    Note over IEC104: 如果是查询命令
    IEC104->>HAL: 读取传感器数据
    HAL->>IEC104: 传感器数据
    IEC104->>IEC104: 封装ASDU响应
    IEC104->>MQTT: 发布 embedded/network/eth/command
    MQTT->>ETH_Man: 订阅命令
    ETH_Man->>ETH_App: 发送响应数据
    ETH_App->>Master: TCP响应
```

### 2. 数据下行流程（传感器数据 → 主站）

```mermaid
sequenceDiagram
    participant HAL as HAL
    participant IEC104 as IEC104-Man
    participant MQTT as MQTT Broker
    participant ETH_Man as ETH-Man
    participant ETH_App as ETH-App
    participant Master as 主站

    HAL->>IEC104: 传感器数据变化
    IEC104->>IEC104: 检查是否需要上报
    IEC104->>IEC104: 封装ASDU数据
    IEC104->>MQTT: 发布 embedded/network/eth/data
    MQTT->>ETH_Man: 订阅数据
    ETH_Man->>ETH_Man: 检查连接状态
    ETH_Man->>ETH_App: 发送数据包
    ETH_App->>Master: TCP发送
```

### 3. 控制命令流程（主站 → 硬件控制）

```mermaid
sequenceDiagram
    participant Master as 主站
    participant ETH_App as ETH-App
    participant ETH_Man as ETH-Man
    participant MQTT as MQTT Broker
    participant IEC104 as IEC104-Man
    participant HAL as HAL

    Master->>ETH_App: 控制命令
    ETH_App->>ETH_Man: 原始数据包
    ETH_Man->>ETH_Man: 协议识别
    ETH_Man->>MQTT: 发布 embedded/network/eth/data
    MQTT->>IEC104: 订阅数据
    IEC104->>IEC104: 解析控制命令
    IEC104->>IEC104: 验证命令有效性
    IEC104->>HAL: 执行硬件控制
    HAL->>IEC104: 执行结果
    IEC104->>IEC104: 生成确认响应
    IEC104->>MQTT: 发布 embedded/network/eth/command
    MQTT->>ETH_Man: 订阅命令
    ETH_Man->>ETH_App: 发送确认
    ETH_App->>Master: TCP确认
```

## MQTT Topic 设计

### 核心Topics
```
embedded/network/eth/data      # 网络数据交换
embedded/network/eth/command   # 命令响应
embedded/network/eth/status    # 状态信息
```

### 消息格式

#### 网络数据消息
```json
{
    "topic": "embedded/network/eth/data",
    "payload": {
        "direction": "in",           // in: 接收, out: 发送
        "timestamp": 1640995200,
        "source": "192.168.1.100:2404",
        "protocol": "IEC104",
        "data": {
            "length": 256,
            "type": "I-Frame",
            "content": "68 01 00 01 00 01 01 01 01 01..."
        }
    }
}
```

#### 命令响应消息
```json
{
    "topic": "embedded/network/eth/command",
    "payload": {
        "direction": "out",
        "timestamp": 1640995200,
        "target": "192.168.1.100:2404",
        "protocol": "IEC104",
        "command": {
            "type": "response",
            "asdu_type": "M_ME_TE_1",
            "ioa": 1001,
            "value": 25.6,
            "quality": "GOOD"
        }
    }
}
```

#### 状态消息
```json
{
    "topic": "embedded/network/eth/status",
    "payload": {
        "timestamp": 1640995200,
        "connection": {
            "status": "connected",
            "remote_ip": "192.168.1.100",
            "remote_port": 2404,
            "uptime": 3600
        },
        "statistics": {
            "bytes_sent": 10240,
            "bytes_received": 8192,
            "packets_sent": 100,
            "packets_received": 80
        }
    }
}
```

## 数据流向图

```mermaid
graph LR
    subgraph "数据上行"
        A1[主站数据] --> A2[ETH-App接收]
        A2 --> A3[ETH-Man解析]
        A3 --> A4[MQTT发布]
        A4 --> A5[IEC104-Man处理]
        A5 --> A6[生成响应]
        A6 --> A7[MQTT发布]
        A7 --> A8[ETH-Man发送]
        A8 --> A9[ETH-App发送]
        A9 --> A10[主站接收]
    end
    
    subgraph "数据下行"
        B1[传感器数据] --> B2[IEC104-Man封装]
        B2 --> B3[MQTT发布]
        B3 --> B4[ETH-Man发送]
        B4 --> B5[ETH-App发送]
        B5 --> B6[主站接收]
    end
```

## 错误处理机制

```mermaid
graph TD
    subgraph "错误检测"
        A1[网络连接错误]
        A2[协议解析错误]
        A3[数据格式错误]
        A4[硬件通信错误]
    end
    
    subgraph "错误处理"
        B1[错误日志记录]
        B2[错误状态发布]
        B3[重连机制]
        B4[降级处理]
    end
    
    subgraph "MQTT错误处理"
        C1[发布错误状态]
        C2[订阅错误处理]
        C3[连接恢复]
    end
    
    A1 --> B1
    A2 --> B1
    A3 --> B1
    A4 --> B1
    
    B1 --> B2
    B2 --> C1
    C1 --> C2
    C2 --> B3
    B3 --> C3
```

## 性能优化

```mermaid
graph LR
    subgraph "数据缓冲"
        B1[接收缓冲区]
        B2[发送缓冲区]
        B3[MQTT消息缓冲]
    end
    
    subgraph "并发处理"
        C1[多线程处理]
        C2[异步I/O]
        C3[事件驱动]
    end
    
    subgraph "缓存机制"
        D1[连接状态缓存]
        D2[协议状态缓存]
        D3[数据缓存]
    end
    
    B1 --> C1
    B2 --> C2
    B3 --> C3
    
    C1 --> D1
    C2 --> D2
    C3 --> D3
```

这个重新梳理的逻辑清晰地展示了ETH-Man和IEC104-Man之间通过MQTT Broker的数据交流机制，包括数据上行、下行、控制命令和错误处理等完整流程。