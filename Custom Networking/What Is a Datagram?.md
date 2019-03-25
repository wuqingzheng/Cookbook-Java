### What Is a Datagram?
通过可靠通道（例如 TCP 套接字）进行通信的客户端和服务器之间有一个专用的点对点通道。要进行通信，它们会建立连接，传输数据，然后关闭连接。通过信道发送的所有数据的接收顺序与发送的顺序相同。这是由通道保证的。

相反，通过数据报通信的应用程序发送和接收完全独立的信息包。这些客户端和服务器没有也不需要专用的点对点通道。无法保证将数据报传送到目的地。他们到达的顺序也不是。

> 定义：  
> 数据报是通过网络发送的独立、自包含的消息，其是否到达、到达时间和内容都是不能保证的。

java.net package 包含三个类来帮助你编写使用 datagrams 在网络上发送和接收数据包的 Java 程序：DatagramSocket，DatagramPacket 和 MulticastSocket。应用程序可以通过 DatagramSocket 发送和接收 DatagramPacket。此外，DatagramPackets 可以广播给所有收听 MulticastSocket 的多个收件人。

