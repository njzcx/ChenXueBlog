# 设计一个MQ需要考虑
## 1、远程通信功能
（1）逻辑通信协议：STOMP, AMQP, MQTT, Openwire, SSL, and WebSockets <br>
（2）物理通信协议：TCP、HTTP

## 2、目标功能
（1）支持哪几种消息发送模式(P2P、Pub/Sub) <br>
（2）消息接收模型(推、拉) <br>
（3）消息投递策略(at-most-once、at-least-once、exactly-once) <br>
（4）消息事务支持？ <br>
（5）消息顺序性? <br>
（6）JMS API支持？（这个不是很关键） <br>
（7）管理API：JMX还是REST? <br>

## 3、分布式方面考量
（1）高可用（High Availability）：Master/Slave？Failover？失败重连？ <br>
（2）高吞吐（High Throughout）：消息吞吐量、扩容（横向纵向扩展、负载均衡）？ <br>
（3）高一致（High Persistence）：消息持久化、不重发？ <br>