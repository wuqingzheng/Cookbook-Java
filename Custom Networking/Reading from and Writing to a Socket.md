### Reading from and Writing to a Socket
让我们看一个简单的例子，说明程序如何使用 Socket 类建立与服务器程序的连接，然后，客户端如何通过套接字向服务器发送数据和从服务器接收数据。

示例程序实现了一个连接到 echo 服务器的 EchoClient 客户端。echo 服务器从其客户端接收数据并将其回送。该 EchoServer 示例实现了一个 echo 服务器。（或者，客户端可以连接到支持回声协议的任何主机。）

该 EchoClient 示例创建一个套接字，从而获得与 echo 服务器的连接。它在标准输入流上读取用户的输入，然后通过将文本写入套接字将该文本转发到 echo 服务器，服务器通过套接字将输入回送给客户端，客户端程序读取并显示从服务器传回给它的数据。

请注意，该 EchoClient 示例的写入和读取都通过其套接字，从而向 echo 服务器发送数据和从 echo 服务器接收数据。

让我们来看看程序并研究其有趣的部分，EchoClient示例中 try-with-resources 语句中的以下语句至关重要。这些行在客户端和服务器之间建立套接字连接，并且在套接字上打开 PrintWriter 和BufferedReader：

```
String hostName = args[0];
int portNumber = Integer.parseInt(args[1]);

try (
    Socket echoSocket = new Socket(hostName, portNumber);
    PrintWriter out = new PrintWriter(echoSocket.getOutputStream(), true);
    BufferedReader in = new BufferedReader(new InputStreamReader(echoSocket.getInputStream()));
    BufferedReader stdIn = new BufferedReader(new InputStreamReader(System.in))
)
```

try-with resources 语句中的第一个语句创建一个新 Socket 对象并为其命名 echoSocket，此处使用的 Socket 构造函数需要你想要连接的计算机的名称和端口号。示例程序使用第一个命令行参数作为计算机的名称（主机名），使用第二个命令行参数作为端口号。在计算机上运行此程序时，请确保您使用的主机名是要连接的计算机的完全限定IP名称，例如，如果您的 echo 服务器正在计算机 echoserver.example.com 上运行并且正在侦听端口号 7，那么如果要将该 EchoServer 示例用作 echo 服务器，首先请从计算机 echoserver.example.com 运行以下命令：

```java EchoServer 7```

然后，使用以下命令运行该 EchoClient 示例：

```java EchoClient echoserver.example.com 7```

try-with resources 语句中的第二个语句获取套接字的输出流并在其上打开一个 PrintWriter，类似地，第三个语句获取套接字的输入流并在其上打开一个 PrintWriter，该示例使用读取器和写入器，以便它可以在套接字上写入 Unicode 字符。

要通过套接字将数据发送到服务器，该 EchoClient 示例需要写入 PrintWriter。要获取服务器的响应，EchoClient 从 BufferedReader 对象 stdIn 中读取，该对象在 try-with resources 语句的第四个语句中创建，如果您还不熟悉 Java 平台的 I/O 类，你不妨读一读 Basic I/O。

该程序的下一个有趣部分是 while 循环。循环从标准输入流一次读取一行，并通过将其写入连接到套接字的 PrintWriter 立即将其发送到服务器：

```
String userInput;
while ((userInput = stdIn.readLine()) != null) {
    out.println(userInput);
    System.out.println("echo: " + in.readLine());
}
```

while 循环中的最后一个语句从连接到套接字的 BufferedReader 读取一行信息。该 readLine 方法等待，直到服务器将信息回送回 EchoClient。当 readline 返回时，EchoClient 打印信息到标准输出。

该 while 循环继续，直到用户类型输入结束的字符。也就是说，该 EchoClient 示例读取来自用户的输入，将其发送到 Echo 服务器，从服务器获取响应并显示它，直到输入结束。（可以通过按 Ctrl-C 输入结束字符。）然后 while 循环终止，Java 运行时自动关闭连接到套接字和标准输入流的读取器和写入器，并关闭连接到服务器的套接字。Java 运行时会自动关闭这些资源，因为它们是在 try-with resources 语句中创建的。Java运行时以与创建它们相反的顺序关闭这些资源。（这很好，因为连接到套接字的流应该在套接字本身关闭之前关闭。）

此客户端程序简单明了，因为 echo 服务器实现了一个简单的协议。客户端将文本发送到服务器，服务器回送它。当您的客户端程序与更复杂的服务器（如 HTTP 服务器）通信时，您的客户端程序也会更复杂。但是，基础知识与此计划中的基本内容大致相同：

1. Open a socket.
2. Open an input stream and output stream to the socket.
3. Read from and write to the stream according to the server's protocol.
4. Close the streams.
5. Close the socket.

只有步骤 3 因客户端而异，具体取决于服务器。其他步骤基本保持不变。