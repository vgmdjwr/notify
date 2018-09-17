# notify
进行消息通知，并确保送达

# 通信
所有通信方式全部使用http post，application使用 x-www-form-urlencoded ，数据发送前使用url encode方式转码

# 接收

```
Sender: 发送方
Timestamp: 发送时间戳
Data: 发送内容
Confirm: 接收方接到内容后，需回复的确认内容
Retry: 失败重试次数，默认为10
RetryMode: 重试方式
Receiver: 接收方
Extra: 额外数据，格式为kv键值对

Receiver 是http或https协议下完整URL
```

# 发送
发送时，会按照接收到的设定，只发送data里的内容。
如接收方并正确未返回确认信息，则进行重发。

# RetryMode
- 1000
时间间隔按照 15/15/30/30/60/60/120/120... 进行重试

- 1001
时间间隔按照 30/60/120/240... 进行重试

- 1002
时间间隔按照固定数据进行重试，需在Extra字段进行指定，内容为kv对，key值为interval，value为时间间隔，单位为秒
例：
  ```
  以固定30秒的时间间隔进行重试，最多重试次数为20次
  RetryMode: 1002
  Extra: interval=30
  Retry: 20
  ```
  
# 组件
- Redis，进行数据暂存，缓冲
- Mysql，对每次发送情况进行记录
- RabbitMQ，消息分发，提升并发量

# 扩展
- 进行短信推送
- 微信公众号推送

# TODO
- 公众号，订阅后，进行每日统计并推送
- 回调通知，对于接收者最终的接收情况，对发送者进行通知，本次通知只发送一次
- 自查，对于接收者的接收情况，发送者可以进行查询，建议最终重试时间过后，且回调通知未正常收到的情况下进行查询

