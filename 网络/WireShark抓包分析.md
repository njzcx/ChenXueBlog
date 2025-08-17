>  原文链接：https://www.cnblogs.com/linyfeng/p/9496126.html

## 1. 获取抓包文件

1. 本机如有WireShark，可选择网卡后，输入抓包过滤条件（路径：Capture->Capture Filters。用于在抓取数据包前设置），启动Capture抓包
2. 如是Linux服务器，可使用tcpdump抓包并导出抓包文件，具体命令（[官网链接](https://www.wireshark.org/docs/wsug_html_chunked/AppToolstcpdump.html)）：

```git
# Older versions of tcpdump truncate packets to 68 or 96 bytes.
# If this is the case, use -s to capture full-sized packets
tcpdump -i <interface> -s 65535 -w <file>
```

## 2. 导入WireShark分析

1. WireShark界面说明

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711355810656-16db2c0a-f876-4627-86da-a8acfca3ed81.png)

2. 无论是本地或是文件导入，在WireShark都需要**过滤条件（分为 抓包过滤和显示过滤）**，对所需的流量进行分析，抓包过滤路径Capture->Capture Filters，显示过滤路径Analyze->Display Filters。

3. 抓包过滤条件常用组合有：类型(host、net、port)、方向(src、dst)、协议(ether、ip、tcp、udp、http、icmp、ftp等)、逻辑运算符(&&、||、!)。

1. **协议过滤**

- tcp，只显示TCP协议的数据包列表
- http，只查看HTTP协议的数据包列表
- icmp，只显示ICMP协议的数据包列表

1. **ip过滤**

- host 192.168.1.104
- src host 192.168.1.104
- dst host 192.168.1.104

1. **端口过滤**

- port 80
- src port 80
- dst port 80

1. **逻辑运算过滤**

- src host 192.168.1.104 && dst port 80，抓取主机地址为192.168.1.80 且 目的端口为80的数据包
- !broadcast，不抓取广播数据包

1. 显示过滤条件常用（当过滤框输入xx.时会自动提示可用选项）：

1. **协议过滤**：等同于抓包过滤
2. **ip过滤**

- ip.src == 192.168.1.104
- ip.dst == 192.168.1.104
- ip.addr == 192.168.1.104，显示源IP地址或目标IP地址为192.168.1.104的数据包列表

1. **端口过滤**

- tcp.port == 80
- tcp.srcport == 80
- tcp.dstport == 80

1. **逻辑运算过滤（and、or、not）**

- ip.addr == 192.0.2.1 and icmp：表示只显示ICMP协议，且源或目标主机IP为192.0.2.1的包

1. **协议过滤（可自己尝试）**

- http.request.method=="GET",   只显示HTTP GET方法的

1. **内容过滤**

- 右键数据包->Apply as Filter->Selected，只显示此过滤数据
- data contains "abcd"，过滤出data数据包中包含"abcd"内容的包

1. 数据包列表中不同协议使用了不同颜色做区分，具体颜色含义见View->Coloring Rules

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711355480742-6360c566-4aac-4fb3-a0e1-891b000be76a.png)

1. 数据包列表中的time默认是相对时间，可调整数据包列表中时间戳显示格式，调整方法为View ->Time Display Format -> Date and Time of Day。
2. 数据包详细信息，该面板是最重要的，用来查看协议中的每一个字段。各行信息分别为

1. Frame: 物理层的数据帧概况
2. Ethernet II: 数据链路层以太网帧头部信息
3. Internet Protocol Version 4: 网络层IP包头部信息
4. Transmission Control Protocol:  传输层的数据段头部信息，此处是TCP
5. Hypertext Transfer Protocol:  应用层的信息，此处是HTTP协议

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711355929347-618c2fef-9ea8-4720-85c6-9d375b3a1a39.png)

TCP包字段报文格式

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711356300285-19fb1a29-3842-4d1e-b47c-af07448f8028.png)

1. 数据字节：此包的字节16进制信息，深入分析会用到，常规用的比较少
2. 数据统计：展示所有包数量、显示的包数量、丢包数量

## 3. 分析原理

### a. TCP三次握手

三次握手原理

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711358540201-04c8c61b-2543-4e70-a491-f39169e58fc2.png)

wireshark截获到了三次握手的三个数据包。第四个包才是HTTP的， 这说明HTTP的确是使用TCP建立连接的

- TCP FLAGS字段标识：SYN表示建立连接，FIN表示关闭连接，ACK表示响应，PSH表示有DATA数据传输，RST表示连接重置

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711359910275-0b89720f-5c77-4504-9ee3-ad0e35ce7ae2.png)

**第一次握手数据包**：客户端发送一个TCP，标志位为SYN，序列号为0， 代表客户端请求建立连接

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711360002402-6d8fd34c-7697-4707-b391-1e99f17c6d30.png)

数据包的关键属性如下：

- Seq = 0 ：初始建立连接值为0，数据包的相对序列号从0开始，表示当前还没有发送数据
- Ack =0：初始建立连接值为0，已经收到包的数量，表示当前没有接收到数据
- SYN ：标志位，表示请求建立连接

**第二次握手的数据包**：服务器发回确认包, 标志位为 SYN,ACK. 将确认序号(ACK)设置为客户端的SYN加1

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711360124291-0d116b9e-f60d-46e4-980e-599a131e1619.png)

数据包的关键属性如下：

- [SYN + ACK]: 标志位，同意建立连接，并回送SYN+ACK

- Seq = 0 ：初始建立值为0，表示当前还没有发送数据

- Ack = 1：表示当前端成功接收的数据位数，虽然客户端没有发送任何有效数据，确认号还是被加1，因为包含SYN或FIN标志位。（并不会对有效数据的计数产生影响，因为含有SYN或FIN标志位的包并不携带有效数据）

**第三次握手的数据包**：客户端再次发送确认包(ACK) ，SYN标志位为0，ACK标志位为1.并且把服务器发来ACK的序号字段+1,放在确定字段中发送给对方.并且在数据段放写ISN的+1

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711360207781-c6597f95-7302-4e93-b8d6-d11343da75d5.png)

数据包的关键属性如下：

- ACK ：标志位，表示已经收到记录
- Seq = 1 ：表示当前已经发送1个数据
- Ack = 1 : 表示当前端成功接收的数据位数，虽然服务端没有发送任何有效数据，确认号还是被加1，因为包含SYN或FIN标志位（并不会对有效数据的计数产生影响，因为含有SYN或FIN标志位的包并不携带有效数据)

### b. Seq和Ack规则

**传输阶段**

Seq=上次Seq+上次Len （或 Seq=对方Ack ，握手和传输交界阶段规则不准确）

Ack=对方Seq+对方Len

**握手阶段（握手SYN、挥手FIN）**

Seq=对方Ack （或 Seq=上次Seq+1 ，握手和传输交界阶段规则不准确）

Ack=对方Seq+1

> +1的原因是握手SYN和挥手FIN Len为0，无法区分

3次握手阶段Seq和Ack分别是：0,0 -> 0,1 -> 1,1

4次挥手阶段Seq和Ack分别是：1,1 -> 1,2 -> 1,2 -> 2,2

**总结**

握手阶段，我方Seq和Ack = 对方Ack和Seq+1，即每次握手交互都是Seq,Ack翻转后+1

传输阶段，我方Seq和Ack = 我方上次Seq+Len和对方Seq+Len

一次Http请求

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711443542160-1114764c-8d61-481b-ac05-7d3d16e1384a.png)

一次Https请求

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/网络/images/WireShark抓包分析/1711446906522-c95d5510-7724-4216-8007-0dc38848ca9d.png)