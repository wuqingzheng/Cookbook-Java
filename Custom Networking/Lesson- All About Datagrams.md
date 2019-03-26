### Lesson: All About Datagrams
### All About Datagrams
某些您编写的通过网络进行通信的应用程序不需要 TCP 提供的可靠的点对点通道。相反，您的应用程序可能会受益某种通信模式，该模式提供独立的信息包，其是否到达和到达顺序无法保证。

UDP 协议提供了一种网络通信模式，由此应用程序将数据包（称为数据报 datagrams）发送到另一个应用程序。数据报是通过网络发送的独立，自包含的消息，其是否到达、到达时间和内容不能得到保证。java.net 包中的 DatagramPacket 类和 DatagramSocket 类使用 UDP 实现与系统无关的数据报通信。

#### What Is a Datagram?
数据报是通过网络发送的独立、自包含的消息，其是否到达、到达时间和内容不能得到保证。

#### Writing a Datagram Client and Server
本节将向您介绍一个示例，其中包含两个使用数据报进行通信的 Java 程序。服务器端是一个报价服务器，监听其 DatagramSocket 并且向客户端发送报价单不论该客户端是否请求。客户端是一个简单的程序，它只是发出服务器的请求。

#### Broadcasting to Multiple Recipients
此部分修改报价服务器，以便报价服务器不会向单个请求的客户端发送报价，而是每分钟向正在监听的多个客户端广播报价，客户端程序必须相应地修改。

> Note:   
> 许多防火墙和路由器配置为不允许 UDP 数据包。如果连接到防火墙外的服务时遇到问题，或者客户端无法连接到您的服务，请询问系统管理员是否允许 UDP。


## 什么是 Datagram？
Clients and servers that communicate via a reliable channel, such as a TCP socket, have a dedicated point-to-point channel between themselves, or at least the illusion of one. To communicate, they establish a connection, transmit the data, and then close the connection. All data sent over the channel is received in the same order in which it was sent. This is guaranteed by the channel.

In contrast, applications that communicate via datagrams send and receive completely independent packets of information. These clients and servers do not have and do not need a dedicated point-to-point channel. The delivery of datagrams to their destinations is not guaranteed. Nor is the order of their arrival.

> Definition: 
> 
> A datagram is an independent, self-contained message sent over the network whose arrival, arrival time, and content are not guaranteed.

The java.net package contains three classes to help you write Java programs that use datagrams to send and receive packets over the network: DatagramSocket, DatagramPacket, and MulticastSocketAn application can send and receive DatagramPackets through a DatagramSocket. In addition, DatagramPackets can be broadcast to multiple recipients all listening to a MulticastSocket.