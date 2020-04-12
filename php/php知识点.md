[TOC]
#php 输出缓冲区(output buffer)
主要作用就是将数据传输一次性发送而不是一个字节一个字节的发送，减少层与层之间的数据交换次数对性能提升有帮助
#网络协议
![](./networkPotocol.png)
#TCP
TCP属于`传输层`，应用层将`数据流`传输给TCP,TCP将数据流分区成`报文段`，之后把结果包发给IP层。IP通过网络发送给另一端的TCP层。另一端的TCP层接收到数据包，将状态结果发送回去，如果没有发送回去，那么TCP发送端又会再发送一次给接收端`数据包`。
#TCP三次`握手`建立连接(three-way handshake)
![](./tcp-hand-shake.png)
1.Step 1 (SYN):
First client sends a TCP segment with SYN(Synchronize Sequence Number) control bit (synchronize) set.
2.Step 2 (SYN + ACK): 
If server receives client’s data (Yay!!), it sends ACK(acknowledgement) along with its own SYN request.
3.Step 3 (ACK) : 
Client sends ACK(acknowledgement). Connection is established.
#TCP四次`挥手`关闭连接(four-way handshake)
1.A发送给B关闭连接
2.B发送确认消息给A
3.B发送给A关闭连接
4.A发送确认消息给B
#为什么建立连接握手三次而关闭连接需要握手四次？
前提A先发送关闭信息给B
因为`关闭连接`的时候按照`建立连接`的时候一样来，将会丢失数据，万一B传输数据给A呢？这时候A还能接收B发送来的数据，B已经不接A的数据了，直接把`关闭`的信号和`确认`的信号一起发送，A将会停止接收数据。