# 目的
实现灾备，数据备份和故障转移
读写分离，主负责写操作，从负责读操作
# 配置实现
## 简单的主从打通
先通过链接[Mysql主从复制搭建及详解](https://blog.csdn.net/hsd2012/article/details/51251051)，实现最简单的主从复制
- 主节点配置binlog和serverid；从节点配置serverid，在从节点的MySQL环境中指定master地址、binlog、pos；start slave后就打通了。（前提需要slave已经创建库和表）
- 存在的问题：缺少专门用于replication slave的账户；缺少binlog写入频率的配置；缺少主从超时配置
## 简单的双主打通
双主打通的原理其实就是两个DB节点即使master又是slave，互为主从，所以两个节点都要配置binlog、serverid，同时指定对方host、binlog、pos，start slave。这里额外需要增加auto_increment，防止双主的主键冲突。
没有在本机验证[高可用的Mysql双机热备(Mysql_HA)](https://blog.csdn.net/xuejiazhi/article/details/8941156)，不过逻辑和参数可参考，内部还包含keepalived的结合。

# 原理
原理：[MySql 主从复制及配置实现](https://segmentfault.com/a/1190000008942618)
# 带来的问题
## 双主的VIP实现-keepalived
根据[Keepalived 原理](https://www.cnblogs.com/andy6/p/7019054.html)我们知道Keepalived是linux系统上的一个应用，通过VRRP协议解决负载均衡的问题。
VRRP协议(Virtual Router Redundancy Protocol)允许多个路由器组成一个虚拟路由器，申请自己的虚拟ip、虚拟mac并广播。
常用的部署形态是：两个Keepalived节点，一主一备共用一个虚拟ip，主节点对real ip地址健康做探测，同时转发流量到real ip。当主节点挂掉，备节点快速抢占，保证VIP服务的高可用和连续性。对外只提供一个vip，外界无感知。
关于原理对照着[](https://www.jianshu.com/p/4b46586e79aa)
# 解决方案&优化