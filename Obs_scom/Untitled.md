

flowchart TD
    A[开始] --> B[解析命令行参数]
    B --> C[设置信号处理函数]
    C --> D[创建应用上下文]
    D --> E[初始化消息队列]
    E --> F{消息队列初始化成功?}
    F -->|否| G[报错并退出]
    F -->|是| H[初始化RS485串口]
    H --> I{RS485初始化成功?}
    I -->|否| J[报错并清理资源]
    I -->|是| K[主循环开始]
    
    K --> L{检查IEC到RS485队列}
    L --> M{有命令消息?}
    M -->|是| N[处理命令消息]
    M -->|否| O{检查RS485串口数据}
    N --> O
    
    O --> P{有数据?}
    P -->|是| Q[处理RS485数据并发送到IEC]
    P -->|否| R[短暂休眠]
    Q --> R
    
    R --> S{运行标志为true?}
    S -->|是| L
    S -->|否| T[清理资源]
    T --> U[结束]




```mermaid
sequenceDiagram
    participant 主站 as 主站
    participant IEC as IEC60870进程
    participant RS485 as RS485进程
    participant 设备 as 现场设备

    %% 总招数据流
    主站->>IEC: 总招命令
    IEC->>RS485: 数据请求
    RS485->>设备: 读取命令
    设备->>RS485: 返回数据
    RS485->>IEC: 传输数据
    IEC->>主站: I帧响应

    %% 控制命令流
    主站->>IEC: 点控命令
    IEC->>RS485: 控制请求
    RS485->>设备: 执行控制
    IEC->>主站: 命令确认

    %% 主动上报流
    设备->>RS485: 主动上报数据
    RS485->>IEC: 传输数据
    Note over IEC: process_pending_rs485_messages
```



```mermaid
flowchart TD
    A[开始] --> B[设置信号处理函数]
    B --> C[创建应用上下文]
    C --> D[打开消息队列]
    D --> E{消息队列打开成功?}
    E -->|否| F[报错并退出]
    E -->|是| G[创建IEC60870从站]
    
    G --> H[配置从站参数]
    H --> I[设置连接回调]
    I --> J[设置ASDU回调]
    J --> K[启动从站]
    
    K --> L{从站启动成功?}
    L -->|否| M[报错并清理资源]
    L -->|是| N[主循环开始]
    
    N --> O[处理待处理的RS485消息]
    O --> P[短暂休眠]
    P --> Q{运行标志为true?}
    Q -->|是| N
    Q -->|否| R[清理资源]
    R --> S[结束]
    
    %% 回调函数（异步执行）
    T[总招命令回调] --> T1[向RS485请求数据]
    T1 --> T2[等待RS485响应]
    T2 --> T3[格式化数据返回主站]
    
    U[点控命令回调] --> U1[转换为RS485命令]
    U1 --> U2[发送命令到RS485]
```



# interrogationHandler 函数流程图（Markdown格式）

## 基本流程

```mermaid
flowchart TD
    A[开始] --> B[接收总招命令]
    B --> C{检查上下文参数}
    C -->|无效| C1[返回false]
    C -->|有效| D{检查总招组号QOI}
    
    D -->|不是20| D1[发送否定确认]
    D -->|是20| E[发送肯定确认ACT_CON]
    
    E --> F[准备查询RS485设备的命令]
    F --> G[发送命令到RS485进程]
    G --> H[等待RS485数据返回<br>超时设置为2秒]
    
    H --> I{接收数据成功?}
    I -->|成功| J[处理接收到的数据]
    I -->|超时或失败| K[准备默认值]
    
    J --> L[创建ASDU响应]
    K --> L
    
    L --> M[添加测量值信息对象]
    M --> N[发送ASDU到主站]
    N --> O[创建状态信息ASDU]
    O --> P[添加状态点信息对象]
    P --> Q[发送状态ASDU到主站]
    Q --> R[发送总招命令终止ACT_TERM]
    R --> S[返回true]
    
    D1 --> S
    C1 --> T[结束]
    S --> T
```

## 详细数据处理流程

```mermaid
flowchart TD
    A[接收RS485数据] --> B{数据接收成功?}
    
    B -->|成功| C[提取温度数据]
    B -->|失败| D[使用无效品质位<br>创建默认值]
    
    C --> C1{数据长度>=2?}
    C1 -->|是| C2[提取温度值<br>data[0]<<8 | data[1]]
    C1 -->|否| C3[使用默认温度值]
    
    C2 --> E[创建测量值对象<br>IOA=100]
    C3 --> E
    D --> E
    
    E --> F{数据长度>2?}
    F -->|是| G[添加第二个测量值<br>IOA=101]
    F -->|否| H[只使用第一个测量值]
    
    G --> I{数据长度>3?}
    I -->|是| J[添加第三个测量值<br>IOA=102]
    I -->|否| K[只使用前两个测量值]
    
    H --> L[创建状态点信息<br>IOA=200]
    J --> L
    K --> L
    
    L --> M[返回数据到主站]
```

## 与RS485进程交互流程

```mermaid
sequenceDiagram
    participant 主站 as 主站
    participant IH as interrogationHandler
    participant IEC as IEC60870进程
    participant MQ as 消息队列
    participant RS485 as RS485进程
    
    主站->>IH: 总招命令<br>qoi=20
    IH->>主站: ACT_CON确认
    
    IH->>MQ: 发送RS485查询命令
    Note over IH: RS485Message<br>{isCommand=1}
    
    MQ->>RS485: 传递命令
    RS485->>RS485: 发送到设备
    
    Note over IH: 启动超时等待<br>timeout=2秒
    
    RS485->>MQ: 返回数据
    Note over RS485: RS485Message<br>{isCommand=0}
    
    MQ->>IH: 获取数据
    
    alt 成功接收数据
        IH->>IH: 解析数据
        IH->>主站: 包含实际值的ASDU
    else 超时或失败
        IH->>主站: 包含默认值的ASDU<br>品质位=INVALID
    end
    
    IH->>主站: 包含状态的ASDU
    IH->>主站: ACT_TERM终止
```

```mermaid
flowchart TD
    A[回调函数] -->|接收命令| B[任务队列]
    B --> C[工作线程池]
    C -->|线程1| D[处理命令]
    C -->|线程2| E[处理命令]
    C -->|线程3| F[处理命令]
    D --> G[返回结果]
    E --> G
    F --> G
```


```mermaid
flowchart LR
    A[主线程] --> B[库回调处理]
    B --> C{命令分发}
    C -->|总招命令| D[总招处理线程]
    C -->|控制命令| E[控制处理线程]
    C -->|时钟同步| F[时钟同步线程]
    D --> G[与RS485通信]
    E --> G
```

```mermaid
sequenceDiagram
    participant 主站 as 主站
    participant 回调 as 回调函数
    participant 线程 as 处理线程
    participant RS485 as RS485进程
    
    主站->>回调: 总招命令
    回调->>主站: 立即返回确认
    回调->>线程: 创建处理线程
    线程->>RS485: 请求数据
    RS485->>线程: 返回数据
    线程->>主站: 发送数据帧
```

```mermaid
flowchart TD
    A[主线程] --> B[初始化资源]
    B --> C[启动接收线程]
    B --> D[启动发送线程]
    
    C -->|接收线程| E[监听RS485数据]
    E --> F[处理接收到的数据]
    F --> G[存入共享数据结构]
    G --> E
    
    D -->|发送线程| H[监听发送队列]
    H --> I[处理待发送命令]
    I --> J[发送到RS485]
    J --> H
```



```mermaid
classDiagram
    class 主线程 {
        +初始化资源()
        +创建线程()
        +处理回调()
        +清理资源()
    }
    
    class 接收线程 {
        -消息队列
        +运行()
        +处理RS485数据()
        +通知回调处理器()
    }
    
    class 发送线程 {
        -命令队列
        +运行()
        +等待命令()
        +发送命令到RS485()
    }
    
    class 共享数据 {
        -数据缓冲区
        -互斥锁
        +存入数据()
        +获取数据()
    }
    
    主线程 --> 接收线程: 创建
    主线程 --> 发送线程: 创建
    接收线程 --> 共享数据: 读写
    发送线程 --> 共享数据: 读写
```


```mermaid
flowchart TD
    A[主线程] -->|初始化| B[IEC104从站服务]
    B --> C[启动命令处理线程]
    B --> D[启动响应发送线程]
    
    C -->|命令处理线程| E[监听IEC104主站命令]
    E -->|总招命令| F1[解析总招命令]
    E -->|控制命令| F2[解析控制命令]
    E -->|时钟同步| F3[解析时钟命令]
    F1 --> G1[生成查询请求]
    F2 --> G2[生成控制请求]
    F3 --> G3[处理时钟同步]
    G1 --> H[请求队列]
    G2 --> H
    G3 --> H
    
    D -->|响应发送线程| I[监听响应队列]
    I --> J[格式化响应数据]
    J --> K[发送IEC104帧]
    K --> I
    
    H --> L[RS485进程]
    L --> M[响应队列]
    M --> I
```



```mermaid
classDiagram
    class IEC104主线程 {
        +启动服务()
        +初始化回调()
        +注册命令处理器()
    }
    
    class 命令处理线程 {
        -命令队列
        +运行()
        +总招命令处理()
        +控制命令处理()
        +时钟命令处理()
    }
    
    class 响应发送线程 {
        -响应队列
        +运行()
        +格式化数据()
        +构建响应帧()
        +发送响应()
    }
    
    class 回调分发器 {
        +接收命令()
        +放入命令队列()
        +发出命令处理通知()
    }
    
    IEC104主线程 --> 命令处理线程: 创建
    IEC104主线程 --> 响应发送线程: 创建
    IEC104主线程 --> 回调分发器: 创建
    命令处理线程 ..> 响应发送线程: 通过队列通信
```
