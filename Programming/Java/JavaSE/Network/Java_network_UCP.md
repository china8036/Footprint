- [Java 网络编程：UDP 编程](#java-udp)
	- [1. DatagramSocket](#1-datagramsocket)
	- [2. DatagramPacket](#2-datagrampacket)
	- [3. MulticastSocket](#3-multicastsocket)
	- [4. Refer Links](#4-refer-links)

# Java 网络编程：UDP 编程

Java 使用 DaatagramSocket 代表 UDP 协议的 Socket，使用 DatagramPacket 来代表发送、接收的数据报，DaatagramSocket 接收和发送的数据都是通过 DatagramPacket 对象完成的。

## 1. DatagramSocket

java.net.[DatagramSocket](https://docs.oracle.com/javase/9/docs/api/java/net/DatagramSocket.html) 代表 UDP 协议的 Socket，它本身只是码头，只负责接收和发送数据报，而不维护状态、不能产生 IO 流。

DatagramSocket 提供了以下构造器：
- `DatagramSocket​()`: 创建一个 DatagramSocket 实例，并将该对象绑定到本机默认 IP 地址、本机可用的随机端口。
- `DatagramSocket​(int port)`: 创建一个 DatagramSocket 实例，并将该对象绑定到本机默认 IP 地址、本机指定的端口。
- `DatagramSocket​(int port, InetAddress laddr)`: 创建一个 DatagramSocket 实例，并将该对象绑定到本机指定的 IP 地址、本机指定的端口。

获取 DatagramSocket​对象实例后，就可以通过以下方法来接收和发送数据：
- `void	receive​(DatagramPacket p)`: 从该 DatagramSocket 对象中接收数据报。在接受数据之前，应构造一个包含了空数组的 DatagramPacket 对象，然后调用 DatagramSocket​的 receive 方法等待数据报的到来，receive 方法会阻塞当前线程直到接收到一个数据报位置。
- `void	send​(DatagramPacket p)`: 以该 DatagramSocket 对象向外发送数据报。可知，DatagramSocket 在发送数据报时，并不知道要将该数据报发送到哪里，而是由 DatagramPacket 自身决定目的地的。在发送数据之前，应创建一个包含了要发送内容和目标 IP 地址、端口号的 DatagramPacket 对象，然后调用 DatagramSocket​的 send 方法来发送数据报。

## 2. DatagramPacket

java.net.[DatagramPacket](https://docs.oracle.com/javase/9/docs/api/java/net/DatagramPacket.html) 代表了 UDP 协议的数据报。

DatagramPacket 提供了以下构造器：
- `DatagramPacket​(byte[] buf, int length)`: 以一个空数组来创建 DatagramPacket 对象，该对象的作用是接收 DatagramSocket 中的数据。
- `DatagramPacket​(byte[] buf, int offset, int length, InetAddress address, int port)`: 以一个包含数据的数组来创建 DatagramPacket 对象，同时指定目的地的 IP 地址和端口，作为要发送的数据报。
- `DatagramPacket​(byte[] buf, int offset, int length)`: 以一个空数组来创建 DatagramPacket 对象，并指定接收到的数据放入 buf 数组中时从 offset 开始，最多放 length 个字节。
- `DatagramPacket​(byte[] buf, int length, InetAddress address, int port)`: 创建一个用于发送的 DatagramPacket 对象，指定发送 buf 数组中从 offset 开始，总共 length 个字节的部分内容。

NOTE: UDP 数据报文一次传送的最大数据为**65507**个字节，因此接收方应该提供一个有足够大的缓存空间的 DatagramPacket 实例，以完整地存放调用 receive() 方法时应用程序协议所允许的最大长度的消息。

DatagramPacket 还提供了以下方法来获取 DatagramPacket 对象的关键信息：
- `InetAddress	getAddress​()`: 若程序准备发送此数据报，该方法返回此数据报的目标主机的 IP 地址。若程序刚接收该数据报，该方法返回该数据报发送主机的 IP 地址。
- `int	getPort​()`: 若程序准备发送此数据报，该方法返回此数据报的目标主机的端口。若程序刚接收该数据报，该方法返回该数据报发送主机的端口。
- `SocketAddress	getSocketAddress​()`: 若程序准备发送此数据报，该方法返回此数据报的目标主机的 SocketAddress 对象。若程序刚接收该数据报，该方法返回该数据报发送主机的 SocketAddress 对象。

## 3. MulticastSocket

DatagramSocket ​只允许将数据报发送给指定的目标地址，但若需要将 UDP 数据报发送给多个地址，使用 DatagramSocket​将会非常繁琐。因此，Java 为 UDP 协议提供了 [MulticastSocket](https://docs.oracle.com/javase/9/docs/api/java/net/MulticastSocket.html) 类，它是 DatagramSocke 类的扩展类，通过该类可将数据报以广播方式发送到多个客户端，同时也可以接收其它主机的广播信息。

IP 协议为多点广播提供了一批特殊的 IP 地址：224.0.0.0 ~ 239.255.255.255，当 MulticastSocket 将一个 DatagramPacket 发送到此类 IP 地址时，该数据报将被自动广播到加入该地址的所有 MulticastSocket。

MulticastSocket 提供了以下构造器：
- `MulticastSocket​()`: 使用本机默认地址、随机端口来创建一个 MulticastSocket​对象。
- `MulticastSocket​(int port)`: 使用本机默认地址、指定端口来创建一个 MulticastSocket​对象。
- `MulticastSocket​(SocketAddress bindaddr)`: 使用本机指定地址、指定端口来创建一个 MulticastSocket​对象。

创建 MulticastSocket​ 对象后，需要将该对象加入指定的多点广播地址 (224.0.0.0 ~ 239.255.255.255)：
- `void	joinGroup​(InetAddress mcastaddr)`: Joins a multicast group.
- `void	joinGroup​(SocketAddress mcastaddr, NetworkInterface netIf)`: Joins the specified multicast group at the specified interface.
- `void	leaveGroup​(InetAddress mcastaddr)`: Leave a multicast group.
- `void	leaveGroup​(SocketAddress mcastaddr, NetworkInterface netIf)`: Leave a multicast group on a specified local interface.

MulticastSocket​用于发送、接收数据报的方法与 DatagramSocket 完全相同：
- `void send(DatagramPacket p)`: 以此套接字发送数据报。
- `void receive(DatagramPacket p)`: 从此套接字接收数据报。

MulticastSocket​对象增加了以下方法：
- `void	setTimeToLive​(int ttl)`: 该 ttl 参数用于设置数据报最多可跨过多少个网络。
  - 若 ttl 为 0，则指定数据报只能停留在本地主机。
  - 若 ttl 为 1，则指定数据报发送到本地局域网，这也是默认值。
  - 若 ttl 为 32，则指定数据报只能发送到本站点的网络中。
  - 若 ttl 为 64，则指定数据报应保留在本地区。
  - 若 ttl 为 128，则指定数据报应保留在本大洲。
  - 若 ttl 为 255，则指定数据报可发送到任何地方。
- `int getTimeToLive()`

eg:
```java
try {
    String message[] = {"这是一条广播信息"}; 
    int port = 9876; // 组播的端口  
    InetAddress group = InetAddress.getByName("230.198.112.0"); // 设置广播组地址 224.0.0.0 ~ 239.255.255.255
    MulticastSocket mutiSocket = new MulticastSocket(port); // 广播套接字将在 port 端口广播  
    mutiSocket.setTimeToLive(1); // 可省略  
    mutiSocket.joinGroup(group); // 加入组播地址 
} catch (Exception e) {
    System.out.println("Error:" + e);  
}  
     
while (true) {  
    try {  
        DatagramPacket packet = null;  
        for (String msg : message) { // 循环发送每条广播信息
            byte buff[] = msg.getBytes();  
            packet = new DatagramPacket(buff, buff.length,group,port);  
            System.out.println(new String(buff));  
            mutiSocket.send(packet);  
            sleep(2000);  
        }  
    } catch (Exception e) {  
        System.out.println("Error:" + e);  
    }  
}  
```

eg: 当开发一个群聊系统时，可通过 MulticastSocket 周期性向广播地址发送在线信息，并且将所有用户的 MulticastSocket 加入到该广播地址中，则每个用户都可以接收到其它用户广播的在线信息。如果系统经过一段时间没有收到某个用户广播的在线信息，就从用户列表中将该用户删除。

## 4. Refer Links

[【Java TCP/IP Socket】UDP Socket（含代码）](https://blog.csdn.net/ns_code/article/details/14128987)

<!-- todo: -->
[udp 穿透简单讲解和实现 (Java)](http://www.cnblogs.com/wunaozai/p/5545150.html)