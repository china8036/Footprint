- [Java 网络编程：TCP 编程](#java-网络编程tcp-编程)
  - [1. 基本 API](#1-基本-api)
    - [1.1. ServerSocket](#11-serversocket)
    - [1.2. Socket](#12-socket)
  - [2. 加入多线程](#2-加入多线程)
  - [3. 非阻塞通信](#3-非阻塞通信)
  - [4. Refer Links](#4-refer-links)

# Java 网络编程：TCP 编程

## 1. 基本 API

Java 对基于 TCP 协议的网络通信提供了良好的封装，Java 使用 Socket 对象来代表两端的通信端口，并通过 Socket 产生 IO 流来进行网络通信。

在两台计算机之间使用套接字建立 TCP 连接时包含以下步骤：
1. 服务器实例化一个指定端口的 ServerSocket 对象，表示通过服务器上的端口通信。
1. 服务器调用 ServerSocket 类的 accept() 方法，该方法将一直阻塞当前线程，直到客户端连接到服务器上给定的端口时该方法返回一个对应客户端连接的 Socket 对象。
1. 服务器正在等待时，一个客户端实例化一个 Socket 对象，指定服务器名称和端口号来请求连接。
1. Socket 类的构造函数试图将客户端连接指定的服务器和端口号。如果通信被建立，则在客户端创建一个 Socket 对象能够与服务器进行通信。
1. 在服务器端，accept() 方法返回服务器上一个新的 socket 引用，该 socket 连接到客户端的 socket。
1. 连接建立后，通过使用 I/O 流在进行通信，每一个 socket 都有一个输出流和一个输入流，客户端的输出流连接到服务器端的输入流，而客户端的输入流连接到服务器端的输出流。

### 1.1. ServerSocket

Java 中能接受其它通信实体连接请求的类是 java.net.[ServerSocket](https://docs.oracle.com/javase/9/docs/api/java/net/ServerSocket.html)，ServerSocket 对象用于监听来自客户端的 Socket 连接，如果没有连接，它将一直处于等待状态。

ServerSocket 包含以下构造器，如果 ServerSocket 构造方法没有抛出异常，就意味着你的应用程序已经成功绑定到指定的端口：
- `ServerSocket(int port)`: 创建绑定到特定端口的服务器套接字。
- `ServerSocket(int port, int backlog)`: 利用指定的 backlog 创建服务器套接字并将其绑定到指定的本地端口号。
- `ServerSocket(int port, int backlog, InetAddress address)`: 使用指定的端口、侦听 backlog 和要绑定到的本地 IP 地址创建服务器。
- `ServerSocket()`: 创建非绑定服务器的套接字。

ServerSocket 使用以下方法来监听客户端的连接请求：
- `Socket accept​()`: 该方法将一直阻塞当前线程，直到接收到一个来自客户端 Socket 的连接请求时，该方法不再阻塞并返回一个与客户端 Socket 对应的 Socket 引用对象。

当不再需要监听 Socket 连接时，可调用以下方法关闭 ServerSocket，断开应用程序与指定端口的绑定：
- `void	close​()`

ServerSocket 还提供了以下常用 API：
- `int getLocalPort()`: 返回此套接字在其上侦听的端口。
- `void setSoTimeout(int timeout)`: 通过指定超时值启用 / 禁用 SO_TIMEOUT，以毫秒为单位。
- `void bind(SocketAddress host, int backlog)`: 将 ServerSocket 绑定到特定地址（IP 地址和端口号）。

### 1.2. Socket

java.net.[Socket](https://docs.oracle.com/javase/9/docs/api/java/net/Socket.html) 类代表客户端和服务器都用来互相沟通的套接字。

- 服务器端可通过 accept() 方法的返回值获取 Socket 对象，而客户端获取 Socket 对象则需要使用以下 Socket 构造器：
  - `Socket(String/InetAddress remoteAddress, int remotePort)`: 创建一个流套接字，并将其连接到指定主机上的指定端口。该构造器没有指定本地地址、本地端口，因此默认使用本地主机的默认 IP 地址、系统动态分配的端口。
  - `Socket(String/InetAddress remoteAddress, int remotePort, InetAddress localAddress, int localPort)`: 创建一个套接字，并将其连接到指定远程主机上的指定远程端口。同时指定了本地主机 IP 地址和本地端口，适用于本地主机有多个 IP 地址的情形。
  - `Socket()`: 通过系统默认类型的 SocketImpl 创建未连接的套接字。
  - `Socket​(Proxy proxy)`: 通过代理创建未连接的套接字。

  当上述构造器被调用时，创建 Socket 对象的同时会连接到指定的远程服务器，使得服务器端 ServerSocket 对象的 accept() 方法返回一个 Socket 实例，于是服务器端与客户端就产生了一对互相连接的 Socket。

- 此后，由于 TCP 是一个双向的通信协议，数据可以通过两个数据流在同一时间发送，因此程序无须再区分服务端和客户端的概念。而是可以平等的通过 Socket 对象的 IO 流进行通信。
  - `InputStream getInputStream()`: 返回此套接字的输入流，程序可以从该输入流中获取通信对方的数据。
  - `OutputStream getOutputStream()`: 返回此套接字的输出流，程序可以通过该输出流向通信对方输出数据。

- 当对 Socket 对象的 InputStream 或 OutputStream 操作结束后，就会产生这样一个问题：如何表示输入 / 输出数据已经完毕？
  - 可直接关闭 InputStream 或 OutputStream，可表示 IO 操作已完毕，但当关闭 OutputStream 时，其对应的 Socket 连接也将随之关闭，导致程序无法再从该 Socket 的 InputStream 中获取数据。
  - 可调用 Socket 对象提供的 2 个半关闭方法，即只关闭 Socket 的输入流 / 输出流而不关闭 Socket 连接，用以表示输出数据完毕：
    - `void	shutdownInput​()`: 关闭该 Socket 连接的输入流，程序还可以通过该 Socket 的输出流输出数据。
    - `void	shutdownOutput​()`: 关闭该 Socket 连接的输出流，程序还可以通过该 Socket 的输入流读取数据。
    程序可通过以下方法判断 Socket 连接是否处于半关闭状态：
    - `boolean	isInputShutdown​()`: 判断该 Socket 连接是否处于半读状态。
    - `boolean	isOutputShutdown​()`: 判断该 Socket 连接是否处于半写状态。
    NOTE:
    - 即使先后调用了 Socket 对象的 shutdownInput​ 和 shutdownOutput​ 方法，该 Socket 实例依然没有关闭，只是既无法输出数据也不能读取数据而已。
    - 当调用 Socket 对象的 shutdownInput​和 shutdownOutput​方法后，该 Socket 就无法再次打开输入流 / 输出流，因此这种做法通常不适合保持持久通信状态的交互式应用（如 RTMP 协议），只适用于一站式的通信协议（如 HTTP 协议）。

- Socket 对象提供了设置超时时长的方法：
  - 设置读写操作的超时限制：
    - `void	setSoTimeout​(int timeout)`: 设置超时时长后，若在使用 Socket 进行读写操作完成之前超出了该时间限制，则这些操作就会抛出 SocketTimeoutException 异常。
  - 设置连接服务器的超时限制：
    - 由于 Socket 对象的构造器中没有关于 timeout 的参数，因此可先创建一个无连接的 Socket，再通过接受 timeout 参数的 `void	connect​(SocketAddress endpoint, int timeout)` 方法来手动连接服务器：
      ```java
      Socket s = new Socket();
      s.connect(new InetSocketAddress(host, port), 10000); // 设置 10 秒超时
      ```

## 2. 加入多线程

在使用单线程 IO 模型建立 TCP 连接时：
- 服务器端在处理一个 Socket 连接时将无法处理其它客户端的连接请求，因此，服务器端应为每一个 Socket 连接单独启动一个线程，专用于处理与该客户端的通信逻辑。
- 客户端在读取服务器数据时同样会被阻塞，直到服务器数据 IO 完成，而由于网络因素不稳定等情况，阻塞时间无法确定。因此，客户端应单独启动一个线程，专门用于读取服务器数据。

eg:
```java
public class MyServer {
  	// 定义保存所有 Socket 的 ArrayList
	  public static ArrayList<Socket> socketList = new ArrayList<>();
	  public static void main(String[] args) throws IOException {
        ServerSocket ss = new ServerSocket(30000);
        while(true) {
          Socket s = ss.accept();
          socketList.add(s);
          // 每当客户端连接后启动一条 ServerThread 线程为该客户端服务
          new Thread(new ServerThread(s)).start();
        }
	  }
}
// 负责处理每个线程通信的线程类
public class ServerThread implements Runnable {
    Socket s = null; // 定义当前线程所处理的 Socket
    BufferedReader br = null; // 该线程所处理的 Socket 所对应的输入流
    public ServerThread(Socket s) throws IOException {
        this.s = s;
        br = new BufferedReader(new InputStreamReader(s.getInputStream())); // 初始化该 Socket 对应的输入流
    }
    @Override
	  public void run() {
        try {
            String content = null;
            // 采用循环不断从 Socket 中读取客户端发送过来的数据
            while ((content = readFromClient()) != null) {
                // 遍历 socketList 中的每个 Socket，将读到的内容向每个 Socket 发送一次
                for (Socket s : MyServer.socketList) {
                  PrintStream ps = new PrintStream(s.getOutputStream());
                  ps.println(content);
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    // 定义读取客户端数据的方法
    private String readFromClient() {
        try {
            return br.readLine();
        } catch (IOException e) { // 如果捕捉到异常，表明该 Socket 对应的客户端已经关闭
            MyServer.socketList.remove(s);
        }
        return null;
    }
}
public class MyClient {
    public static void main(String[] args)throws Exception {
        Socket s = new Socket("127.0.0.1" , 30000);
        // 客户端启动 ClientThread 线程不断读取来自服务器的数据
        new Thread(new ClientThread(s)).start();   // ①
        // 获取该 Socket 对应的输出流
        PrintStream ps = new PrintStream(s.getOutputStream());
        String line = null;
        // 不断读取键盘输入
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        while ((line = br.readLine()) != null) {
            // 将用户的键盘输入内容写入 Socket 对应的输出流
            ps.println(line);
        }
    }
}
public class ClientThread implements Runnable {
    // 该线程负责处理的 Socket
    private Socket s;
    // 该线程所处理的 Socket 所对应的输入流
    BufferedReader br = null;
    public ClientThread(Socket s) throws IOException {
        this.s = s;
        br = new BufferedReader(new InputStreamReader(s.getInputStream()));
    }
    @Override
    public void run() {
        try {
          String content = null;
          // 不断读取 Socket 输入流中的内容，并将这些内容打印输出
          while ((content = br.readLine()) != null) {
            System.out.println(content);
          }
        } catch (Exception e) {
          e.printStackTrace();
        }
    }
}
```

## 3. 非阻塞通信

TODO:

## 4. Refer Links

[网络编程释疑之：同步，异步，阻塞，非阻塞](http://blog.51cto.com/yaocoder/1308899)