

## 数据流向架构

```mermaid
graph LR
    subgraph "ETH-Man 网络管理层"
        A1[接收网络数据]
        A2[协议识别]
        A3[数据分发]
        A4[连接管理]
    end
    
    subgraph "MQTT Broker"
        B1[Topic: /network/iec104]
        B2[Topic: /network/status]
        B3[Topic: /network/command]
    end
    
    subgraph "IEC104-Man 协议管理层"
        C1[协议栈处理]
        C2[ASDU解析/封装]
        C3[传感器数据管理]
        C4[命令处理]
    end
    
    A1 --> A2
    A2 --> A3
    A3 --> B1
    B1 --> C1
    C1 --> C2
    C2 --> C3
    C3 --> B1
    B1 --> A4
```

## 详细数据交流逻辑

### 1. 数据上行流程（传感器数据 → 主站）

```mermaid
sequenceDiagram
    participant HAL as HAL
    participant IEC104 as IEC104-Man
    participant MQTT as MQTT Broker
    participant ETH as ETH-Man
    participant ETH_App as ETH-App
    participant Master as 主站

    HAL->>IEC104: 传感器数据
    IEC104->>IEC104: 封装ASDU
    IEC104->>MQTT: 发布 /network/iec104/data
    MQTT->>ETH: 订阅数据
    ETH->>ETH: 协议识别(IEC104)
    ETH->>ETH_App: 发送数据包
    ETH_App->>Master: TCP发送
```

### 2. 数据下行流程（主站命令 → 硬件）

```mermaid
sequenceDiagram
    participant Master as 主站
    participant ETH_App as ETH-App
    participant ETH as ETH-Man
    participant MQTT as MQTT Broker
    participant IEC104 as IEC104-Man
    participant HAL as HAL

    Master->>ETH_App: 控制命令
    ETH_App->>ETH: 原始数据包
    ETH->>ETH: 协议识别(IEC104)
    ETH->>MQTT: 发布 /network/iec104/command
    MQTT->>IEC104: 订阅命令
    IEC104->>IEC104: 解析ASDU
    IEC104->>HAL: 执行控制命令
```

## MQTT Topic 设计

```mermaid
graph TD
    subgraph "MQTT Topic 结构"
        T1[/network/iec104/data]
        T2[/network/iec104/command]
        T3[/network/iec104/status]
        T4[/network/iec104/heartbeat]
        T5[/network/iec104/config]
    end
    
    subgraph "数据流向"
        D1[传感器数据] --> T1
        T1 --> D2[网络发送]
        D3[主站命令] --> T2
        T2 --> D4[硬件控制]
        D5[连接状态] --> T3
        D6[心跳包] --> T4
        D7[配置更新] --> T5
    end
```

## 消息格式设计

### 数据上行消息格式
```json
{
    "topic": "/network/iec104/data",
    "payload": {
        "protocol": "IEC104",
        "type": "data",
        "timestamp": 1640995200,
        "data": {
            "ioa": 1001,
            "value": 25.6,
            "quality": "GOOD",
            "type": "M_ME_TE_1"
        }
    }
}
```

### 数据下行消息格式
```json
{
    "topic": "/network/iec104/command",
    "payload": {
        "protocol": "IEC104",
        "type": "command",
        "timestamp": 1640995200,
        "data": {
            "ioa": 2001,
            "command": "C_SC_NA_1",
            "value": true,
            "cot": "ACTIVATION"
        }
    }
}
```

## 状态管理

```mermaid
graph LR
    subgraph "ETH-Man 状态"
        E1[连接状态]
        E2[协议状态]
        E3[数据统计]
    end
    
    subgraph "IEC104-Man 状态"
        I1[协议连接]
        I2[ASDU状态]
        I3[传感器状态]
    end
    
    subgraph "状态同步"
        S1[MQTT状态发布]
        S2[状态订阅]
        S3[状态同步]
    end
    
    E1 --> S1
    E2 --> S1
    E3 --> S1
    I1 --> S1
    I2 --> S1
    I3 --> S1
    
    S1 --> S2
    S2 --> S3
    S3 --> E1
    S3 --> I1
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
    
    subgraph "恢复机制"
        C1[自动重连]
        C2[数据重传]
        C3[状态恢复]
        C4[配置重载]
    end
    
    A1 --> B1
    A2 --> B1
    A3 --> B1
    A4 --> B1
    
    B1 --> B2
    B2 --> B3
    B3 --> C1
    
    C1 --> C2
    C2 --> C3
    C3 --> C4
```

## 性能优化

```mermaid
graph LR
    subgraph "数据缓冲"
        B1[接收缓冲区]
        B2[发送缓冲区]
        B3[处理缓冲区]
    end
    
    subgraph "并发处理"
        C1[多线程处理]
        C2[异步I/O]
        C3[事件驱动]
    end
    
    subgraph "缓存机制"
        D1[数据缓存]
        D2[状态缓存]
        D3[配置缓存]
    end
    
    B1 --> C1
    B2 --> C2
    B3 --> C3
    
    C1 --> D1
    C2 --> D2
    C3 --> D3
```

这个设计通过MQTT Broker实现了ETH-Man和IEC104-Man之间的松耦合通信，支持双向数据流、状态同步和错误处理。