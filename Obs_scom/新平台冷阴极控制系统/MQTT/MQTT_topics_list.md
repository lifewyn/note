
## MQTT主题列表


| 主题类别 |                         主题名称 |                     发布者 |         订阅者 | 数据格式 |                   描述 |
| ---: | ---------------------------: | ----------------------: | ----------: | ---: | -------------------: |
|      |          `xray/uart-man/cfg` |               uart-man, |       cfg进程 | JSON | 配置射线机参数(电压、电流、曝光时间等) |
|      |      `xray/uart-man/cfg/rsp` |                   cfg进程 |   uart-man, | JSON |             配置参数响应结果 |
|      |        `xray/uart-man/query` |               uart-man, |     fetch进程 | JSON |            查询射线机当前参数 |
|      |    `xray/uart-man/query/rsp` |                 fetch进程 |   uart-man, | JSON |             查询参数响应结果 |
|      |          `xray/uart-man/opt` |               uart-man, | explosive进程 | JSON |               立即曝光命令 |
|      |      `xray/uart-man/opt/rsp` |             explosive进程 |   uart-man, | JSON |             曝光命令响应结果 |
|      |     `xray/uart-man/emg_stop` |  uart-man, , stop_key进程 |  stop_cmd进程 | JSON |               紧急停止命令 |
|      | `xray/uart-man/emg_stop/rsp` |              stop_cmd进程 |   uart-man, | JSON |             紧急停止响应结果 |
|      |        `xray/uart-man/delay` | uart-man, , delay_key进程 | delay_cmd进程 | JSON |               延迟曝光命令 |
|      |    `xray/uart-man/delay/rsp` |             delay_cmd进程 |   uart-man, | JSON |             延迟曝光响应结果 |
|      |      `xray/uart-man/version` |               uart-man, |        系统进程 | JSON |               查询系统版本 |
|      |  `xray/uart-man/version/rsp` |                    系统进程 |   uart-man, | JSON |               系统版本响应 |
|      |         `xray/uart-man/time` |               uart-man, |        系统进程 | JSON |               更新系统时间 |
|      |     `xray/uart-man/time/rsp` |                    系统进程 |   uart-man, | JSON |               时间更新响应 |

## MQTT服务质量(QoS)建议

| 主题类别 | QoS级别 | 说明 |
| --- | --- | --- |
| 系统控制命令 | QoS 2 | 确保命令只被处理一次，不丢失也不重复 |
| X射线管控制 | QoS 2 | 确保射线管命令准确无误地传递 |
| 显示控制 | QoS 0 | 显示数据可接受偶尔丢失 |
| 指示灯控制 | QoS 0 | 指示灯状态可接受偶尔丢失 |
| 蜂鸣器控制 | QoS 1 | 确保蜂鸣器命令至少被处理一次 |
| 系统监控 | QoS 0 | 监控数据可接受偶尔丢失 |
| 历史记录 | QoS 1 | 确保记录数据至少被保存一次 |
| 按键控制 | QoS 1 | 确保按键状态至少被处理一次 |

## 实现建议

1. 使用Mosquitto作为MQTT Broker，轻量级且适合嵌入式系统
2. 使用Eclipse Paho MQTT C客户端库进行MQTT通信
3. 为每个进程创建独立的MQTT客户端连接
4. 实现消息持久化机制，确保系统重启后能恢复关键配置
5. 添加消息认证和TLS加密，提高系统安全性
6. 实现心跳机制，监控各进程状态

这个MQTT主题设计充分考虑了射线机控制系统的功能需求，将原有的消息队列通信方式转换为MQTT发布/订阅模式，实现了系统各模块的解耦，提高了系统的可靠性和可扩展性。
