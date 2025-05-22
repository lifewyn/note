



### 1. 配置参数命令 (cfg)-66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant Cfg as cfg进程
    participant Xray485 as xray485进程
    participant LCD as lcd进程
    participant Record as record进程

    Client->>uartMan: {"cmd":"cfg", "params":{"voltage":220, "current":100, ...}}
    uartMan->>Cfg: xray/uart-man/cfg
    Cfg->>Xray485: xray/cfg/modbus
    Xray485-->>Cfg: xray/cfg/modbus/rsp

    Cfg-->>uartMan: xray/uart-man/cfg/rsp
    uartMan-->>Client: 配置结果返回
    
    par 更新显示和记录
        Cfg->>LCD: xray/cfg/lcd
        Cfg->>Record: xray/cfg/record
    end
```

### 2. 查询参数命令 (query)-66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant Fetch as fetch进程
    participant Record as record进程

    Client->>uartMan: {"cmd":"query"}
    uartMan->>Fetch: xray/uart-man/query
    Fetch->>Record: xray/query/record
    Record-->>Fetch: xray/query/record/rsp
    Fetch-->>uartMan: xray/uart-man/query/rsp
    uartMan-->>Client: 查询结果返回
```

### 3. 曝光命令 (explosive)-66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant Explosive as explosive进程
    participant Xray485 as xray485进程
    participant LED as led进程
    participant Beep as beep进程
    participant Power as power进程
    participant Record as record进程
    

    Client->>uartMan: {"cmd":"explosive", "params":{"start":true}}
    uartMan->>Explosive: xray/uart-man/explosive
    Explosive->>Xray485: xray/explosive/modbus
    Xray485-->>Explosive: xray/explosive/modbus/rsp
    
    Explosive-->>uartMan: xray/uart-man/explosive/rsp
    uartMan-->>Client: 曝光结果返回

    par 指示和记录	    
        Explosive->>LED: xray/indicator/led/red
        Explosive->>Beep: xray/indicator/beep
        Explosive->>Record: xray/record/exposure
        
	    Explosive->>Xray485: xray/explosive/modbus/tube
	    Xray485-->>Explosive: xray/explosive/modbus/tube/rsp

		Explosive->>Power: xray/explosive/power
		Power-->>Explosive: xray/explosive/power/rsp

	    Explosive->>Record: xray/record/exposure/info
    end
    
	Explosive->>Explosive: wait for 2s

    par 指示和记录	    
	    Explosive->>Xray485: xray/explosive/modbus/tube
	    Xray485-->>Explosive: xray/explosive/modbus/tube/rsp

		Explosive->>Power: xray/explosive/power
		Power-->>Explosive: xray/explosive/power/rsp

	    Explosive->>Record: xray/record/exposure/info
    end


```

### 4. 紧急停止命令 (stop_msg)--66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant StopCmd as stop_cmd进程
    participant Xray485 as xray485进程
    participant LCD as lcd进程
    participant LED as led进程
    participant Beep as beep进程
    participant Delay as delay进程
    participant Record as record进程

    Client->>uartMan: {"cmd":"emg_stop"}
    uartMan->>StopCmd: xray/uart-man/emg_stop
    StopCmd->>Xray485: xray/stop/modbus
    Xray485-->>StopCmd: xray/stop/modbus/rsp
    StopCmd->>LCD: xray/stop/lcd

	note over LED : send signal
	note over Beep : send signal
	note over Delay : send signal
	note over Record : record the stop history

    StopCmd-->>uartMan: xray/uart-man/emg_stop/rsp
    uartMan-->>Client: 停止结果返回

```

### 5. 紧急停止按键 (stop_key)--66
```mermaid
sequenceDiagram
    participant Stopkey as stop_key进程
    participant Xray485 as xray485进程
    participant LCD as lcd进程
    participant LED as led进程
    participant Beep as beep进程
    participant Delay as delay进程
    participant Record as record进程

    Stopkey->>Xray485: xray/stop/modbus
    Xray485-->>Stopkey: xray/stop/modbus/rsp
    Stopkey->>LCD: xray/stop/lcd

	note over LED : send signal
	note over Beep : send signal
	note over Delay : send signal
	note over Record : record the stop history

```

### 5. 延迟曝光命令 (delay-msg)--66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant DelayCmd as delay_cmd进程
    participant LCD as lcd进程
    participant LED as led进程
    participant Beep as beep进程
    participant Explosive as explosive进程

    Client->>uartMan: {"cmd":"delay", "params":{"time":5000}}
    uartMan->>DelayCmd: xray/uart-man/delay
    DelayCmd->>LCD: xray/delay/lcd/
    DelayCmd->>LED: xray/indicator/led/red
    DelayCmd->>Beep: xray/indicator/beep
    DelayCmd->>DelayCmd: part delay time 
    DelayCmd->>Explosive:xray/delay/explosive
    Explosive->>DelayCmd:xray/delay/explosive/rsp
    DelayCmd->>LCD: xray/delay/lcd/recover
    
    DelayCmd-->>uartMan: xray/uart-man/delay/rsp
    uartMan-->>Client: 延迟曝光结果返回
```
### 6. 延迟曝光按键 (delay-key)--66
```mermaid
sequenceDiagram
    participant Delay-key as 延时按键
    participant LCD as lcd进程
    participant LED as led进程
    participant Beep as beep进程
    participant Explosive as explosive进程

    Delay-key->>LCD: xray/delay/lcd/
    Delay-key->>LED: xray/indicator/led/red
    Delay-key->>Beep: xray/indicator/beep
    Delay-key->>Delay-key: part delay time 
    Delay-key->>Explosive:xray/delay/explosive
    Explosive->>Delay-key:xray/delay/explosive/rsp
    Delay-key->>LCD: xray/delay/lcd/recover
```
### 7. 版本查询命令 (version)--66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant System as version进程

    Client->>uartMan: {"cmd":"version"}
    uartMan->>System: xray/control/version
    System-->>uartMan: xray/control/version/rsp
    uartMan-->>Client: 版本信息返回
```

### 8. 时间设置命令 (time)--66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant System as time进程

    Client->>uartMan: {"cmd":"time", "params":{"date":"2025-4-5T15:19:20", "time":"10:00:00"}}
    uartMan->>System: xray/control/time
    System-->>uartMan: xray/control/time/rsp
    uartMan-->>Client: 时间设置结果返回
```

### 9.  查询电池电量(power)--66
```mermaid
sequenceDiagram
    participant Client as 客户端
    participant uartMan as uart-man进程
    participant Power as power进程

    Client->>uartMan: {"cmd":"time", "params":{"date":"2023-08-01", "time":"10:00:00"}}
    uartMan->>Power: xray/uart-man/power
    Power-->>uartMan: xray/uart-man/power/rsp
    uartMan-->>Client: 电量查询返回
```


这样分开展示每个命令和模块的时序图，可以更清晰地看到：
1. 每个命令的具体执行流程
2. 各个模块之间的交互关系
3. 数据流向和响应机制
4. 实时显示和记录的处理方式
5. 系统监控的持续运行机制

每个模块都通过MQTT主题进行通信，保证了系统的模块化和可维护性。
