---
title: TCP协议的三次握手和四次挥手
date: 2017-07-13 21:13:04
tags: [TCP,计算机网络]
categories: 基础知识
---
TCP（Transmission Control Protocol 传输控制协议）是一种面向连接的、可靠的、基于字节流的传输层通信协议。
<!--more-->
# OSI七层模型与TCP/IP四层模型
![TCP协议的三次握手和四次挥手1](http://ofolh8dcq.bkt.clouddn.com/TCP%E5%8D%8F%E8%AE%AE%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%92%8C%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B1.jpg)

# TCP报文格式
![TCP协议的三次握手和四次挥手2](http://ofolh8dcq.bkt.clouddn.com/TCP%E5%8D%8F%E8%AE%AE%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%92%8C%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B2.png)

- 源端口：16位的源端口字段包含初始化通信的端口号。源端口和IP地址的作用是标识报文的返回地址。
- 目的端口：16位的目的端口字段定义传输的目的。这个端口指明接收方计算机上的应用程序接口。
- 序列号：表示本报文段所发送数据的第一个字节的编号。
- 确认号：表示接收方期望收到发送方下一个报文段的第一个字节数据的编号。
- 数据偏移：占4比特，表示数据开端的处所离TCP报文段的肇端处有多远，这实际上就是TCP报文段首部的长度。
- ACK（Acknowledgement field significant,确认字段标志）：取1时表示应答字段有效，也即TCP应答号将包含在TCP段中，为0则反之。
- SYN（Synchronize sequence numbers,同步序列号）：表示同步序号，用来建立连接。
- FIN（No more data from sender）:表示发送端已经发送到数据末尾，数据传送完成，发送FIN标志位的TCP段，连接将被断开。
- 校验和（Checksum）:TCP头包括16位的校验和字段用于错误检查。源主机基于部分IP头信息，TCP头和数据内容计算一个校验和，目的主机也要进行相同的计算，如果收到的内容没有错误过，两个计算应该完全一样，从而证明数据的有效性。

# TCP连接建立——三次握手
![TCP协议的三次握手和四次挥手3](http://ofolh8dcq.bkt.clouddn.com/TCP%E5%8D%8F%E8%AE%AE%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%92%8C%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B3.png)

三次握手过程：
1. 客户端发送SYN标志位为1，Sequence Number为x的连接请求报文段，然后客户端进入SYN_SEND状态，等待服务器的确认响应；
2. 服务器收到客户端的连接请求，对这个SYN报文段进行确认，然后发送Acknowledgment Number为x+1(Sequence Number+1)，SYN标志位和ACK标志位均为1，Sequence Number为y的报文段（即SYN+ACK报文段）给客户端，此时服务器进入SYN_RECV状态；
3. 客户端收到服务器的SYN+ACK报文段，确认ACK后，发送Acknowledgment Number为y+1，SYN标志位为0，ACK标志位为1的报文段，发送完成后，客户端和服务器端都进入ESTABLISHED状态，完成TCP三次握手，客户端和服务器端成功地建立连接，可以开始传输数据了。

# TCP连接释放——四次挥手
![TCP协议的三次握手和四次挥手4](http://ofolh8dcq.bkt.clouddn.com/TCP%E5%8D%8F%E8%AE%AE%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%92%8C%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B4.png)

四次挥手过程：
1. 客户端发送Sequence Number为x+2，Acknowledgment Number为y+1的FIN报文段，客户端进入FIN_WAIT_1状态，即告诉服务端没有数据需要传输了，请求关闭连接；
2. 服务端收到客户端的FIN报文段后，向客户端应答一个Acknowledgment Number为Sequence Number+1的ACK报文段，即应答客户端你的请求我收到了，但是我还没准备好，请等待我的关闭请求。客户端收到后进入FIN\_WAIT\_2状态；
3. 服务端完成数据传输后向客户端发送Sequence Number为y+1的FIN报文段，请求关闭连接，服务器进入LAST_ACK状态；
4. 客户端收到服务端的FIN报文段后，向服务端应答一个Acknowledgment Number为Sequence Number+1的ACK报文段，然后客户端进入TIME_WAIT状态；服务端收到客户端的ACK报文段后关闭连接进入CLOSED状态，客户端等待2MSL后依然没有收到回复，则证明服务端已正常关闭，客户端此时关闭连接进入CLOSED状态。

# FAQ
## 为什么建立连接是三次握手，而关闭连接却是四次挥手呢？
这是因为服务端在LISTEN状态下，收到建立连接请求的SYN报文后，把ACK和SYN放在一个报文里发送给客户端。而关闭连接时，当收到对方的FIN报文时，仅仅表示对方不再发送数据了但是还能接收数据，己方也未必全部数据都发送给对方了，所以己方可以立即close，也可以发送一些数据给对方后，再发送FIN报文给对方来表示同意现在关闭连接，因此，己方ACK和FIN一般都会分开发送。
## 为什么TIME_WAIT状态需要经过2MSL(最大报文段生存时间)才能返回到CLOSE状态？
原因有二：
1. 保证TCP协议的全双工连接能够可靠关闭
如果Client直接CLOSED了，那么由于IP协议的不可靠性或者是其它网络原因，导致Server没有收到Client最后回复的ACK。那么Server就会在超时之后继续发送FIN，此时由于Client已经CLOSED了，就找不到与重发的FIN对应的连接，最后Server就会收到RST而不是ACK，Server就会以为是连接错误把问题报告给高层。这样的情况虽然不会造成数据丢失，但是却导致TCP协议不符合可靠连接的要求。所以，Client不是直接进入CLOSED，而是要保持TIME_WAIT，当再次收到FIN的时候，能够保证对方收到ACK，最后正确的关闭连接。
2. 保证这次连接的重复数据段从网络中消失
如果Client直接CLOSED，然后又再向Server发起一个新连接，我们不能保证这个新连接与刚关闭的连接的端口号是不同的。也就是说有可能新连接和老连接的端口号是相同的。一般来说不会发生什么问题，但是还是有特殊情况出现：假设新连接和已经关闭的老连接端口号是一样的，如果前一次连接的某些数据仍然滞留在网络中，这些延迟数据在建立新连接之后才到达Server，由于新连接和老连接的端口号是一样的，又因为TCP协议判断不同连接的依据是socket pair，于是，TCP协议就认为那个延迟的数据是属于新连接的，这样就和真正的新连接的数据包发生混淆了。所以TCP连接还要在TIME_WAIT状态等待2倍MSL，这样可以保证本次连接的所有数据都从网络中消失。





---------------------
> [tcp报文格式](http://www.cnblogs.com/xinyuyuanm/archive/2013/04/17/3026279.html)
[TCP的三次握手和四次挥手](http://www.jianshu.com/p/f7d1010fa603)
[TCP协议详解](http://www.jianshu.com/p/ef892323e68f)

