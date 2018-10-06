- [计算机网络 - 传输层：Transport control protocol 其它问题](#计算机网络---传输层transport-control-protocol-其它问题)
  - [1. Linux 下单机 TCP 连接的最大并发量](#1-linux-下单机-tcp-连接的最大并发量)
    - [1.1. 理论最大并发量](#11-理论最大并发量)
    - [1.2. 实际最大并发量](#12-实际最大并发量)
  - [2. TCP 拆包粘包问题](#2-tcp-拆包粘包问题)
    - [2.1. 问题描述](#21-问题描述)
    - [2.2. 解决方法](#22-解决方法)
  - [3. TCP 半开连接问题](#3-tcp-半开连接问题)
    - [3.1. 问题描述](#31-问题描述)
    - [3.2. 处理方案](#32-处理方案)
  - [8. Refer Links](#8-refer-links)

# 计算机网络 - 传输层：Transport control protocol 其它问题

## 1. Linux 下单机 TCP 连接的最大并发量

### 1.1. 理论最大并发量

在 TCP 应用中，Server 事先在某个固定端口监听，Client 主动发起连接，经过三路握手后建立 TCP 连接。系统用一个 4 四元组来唯一标识一个 TCP 连接：`{local ip, local port,remote ip,remote port}`。

- Client 端最大 TCP 并发量

  Client 每次发起 TCP 连接请求创建 Socket 时，除非绑定指定的端口，否则系统会自动选取一个空闲的本地端口（local port）。该端口是独占的，不能和其它 TCP 连接共享，因此，**Client 端的 TCP 连接数取决于可用的端口数量**。

  TCP 端口的数据类型是 unsigned short，因此本地端口个数最大为 65536，端口 0 有特殊含义不能使用，因此可用的端口最多为 65535。所以在全部作为 TCP Client 端的情况下，**一个 client 最大 TCP 连接数为 65535，这些连接可以连到不同的 Server ip**。

- Server 端最大 TCP 并发量

  虽然现在的集群，分布式技术可以为我们将并发负载分担在多台服务器上，那我们只需要扩展出数十台电脑就可以解决问题，但是我们更希望能更大的挖掘单台服务器的资源，先努力垂直扩展，再进行水平扩展，这样可以有效的节省服务器相关的开支（硬件资源，机房，运维，电力其实也是一笔不小的开支）。那么到底一台服务器能够支持多少 TCP 并发连接呢？

  Server 通常固定在某个本地端口上监听，等待 Client 的连接请求。不考虑地址重用（unix 的 SO_REUSEADDR 选项）的情况下，即使 Server 端有多个 ip，本地监听端口也是独占的，因此 Server 端的 TCP 连接 4 元组中，只有 remote ip (Client ip) 和 remote port (Client port) 是可变的。也就是说，**最大 TCP 连接并发量为 `number of Client ip * number of Client port`**。

  **对于 IPv4，不考虑 ip 地址分类等因素，最大 TCP 连接数为 `2^32 (number of Client ip) * 2^16 (number of Client port)`，也就是 Server 端单机最大 TCP 连接数约为 `2^48`**。

### 1.2. 实际最大并发量

- 文件句柄限制

  在 Linux 中每个 TCP 连接都需要占用一个文件描述符，一旦所有文件描述符都被占用，新的连接到来时会返回错误 `Socket/File:Can't open so many files`。因此，操作系统的文件句柄限制，是实际应用中影响 TCP 连接并发量的因素之一。

  - 进程限制

    执行 `ulimit -n` 输出 1024，说明对于一个进程，默认情况下最多只能打开 1024 个文件，所以如果采用默认配置，最多也就可以并发上千个 TCP 连接。

    - 临时修改：`ulimit -n 1000000`，但是这种临时修改只对当前登录用户目前的使用环境有效，系统重启或用户退出后就会失效。

    - 永久修改：编辑 `/etc/security/limits.conf` 文件， 修改后内容为
      ```
      * soft nofile 1000000
      * hard nofile 1000000
      ```
      或编辑 /etc/rc.local，在其后添加如下内容
      ```
      ulimit -SHn 1000000
      ```

  - 全局限制

    执行 `cat /proc/sys/fs/file-nr` 输出 `9344 0 592026`，含义分别为：
    - 已经分配的文件句柄数。
    - 已经分配但没有使用的文件句柄数。
    - 最大文件句柄数。
    
    可通过修改 `/etc/sysctl.conf` 文件来增大最大文件句柄数：
    ```
    fs.file-max = 1000000
    net.ipv4.ip_conntrack_max = 1000000
    net.ipv4.netfilter.ip_conntrack_max = 1000000
    ```

- 内存限制

  每个进程 / 线程都需要占用一定的内存。

- 带宽限制

## 2. TCP 拆包粘包问题

### 2.1. 问题描述

- UDP 是基于报文发送的，从 UDP 的帧结构可以看出，在 UDP 首部采用了 16bit 来指示 UDP 数据报文的长度，因此在应用层能很好的将不同的数据报文区分开，从而避免粘包和拆包的问题。
- TCP 是基于字节流的，虽然应用层和 TCP 传输层之间的数据交互是大小不等的数据块，但是 TCP 把这些数据块仅仅看成一连串无结构的字节流，没有边界。另外从 TCP 的帧结构也可以看出，在 TCP 的首部没有表示数据长度的字段，基于上面两点，在使用 TCP 传输数据时，才有粘包或者拆包现象发生的可能。

### 2.2. 解决方法

粘包与拆包是由于 TCP 协议是字节流协议，没有记录边界所导致的，所以如何确定一个完整的业务包就由应用层来处理了（这就是分包机制，本质上就是要在应用层维护消息与消息的边界）。

分包机制一般有两个通用的解决方法：

- 特殊字符控制，例如 FTP 协议。
- 在包头首部添加数据包的长度，例如 HTTP 协议。

## 3. TCP 半开连接问题

### 3.1. 问题描述

当客户端与服务器建立起正常的 TCP 连接后，如果客户主机掉线（网线断开、电源掉电、或系统崩溃等），服务端进程将永远不会知道（通过常用的 select/epoll 都监测不到断开或错误事件），如果不主动处理或重启系统的话对于服务端来说会一直维持着这个连接，这种情况就是半开连接，浪费了服务器端可用的文件描述符。

### 3.2. 处理方案

- 利用操作系统在传输层进行心跳检查

  TCP Socket 的系统 API 中存在保持存活的选项 SO_KEEPALIVE，通过设置该参数，如果在两个小时之内在该套接字的任何一个方向上都没数据交换，TCP 就自动给对端发送一个保持存活探测分节，如果此 TCP 探测分节的响应为 RST，说明对端已经崩溃且已经重新启动，该套接字的待处理错误被置为 ECONNRESET，套接字本身则被关闭。如果没有对此 TCP 探测分节的任何响应，该套接字的处理错误就被置为 ETIMEOUT，套接字本身则被关闭。

  这样确实可以处理 TCP 半开连接的问题，但是该方式存在以下问题：
  - 大多数内核是基于整个内核而不是基于每个套接字维护这些时间参数的，因此如果把无活动周期从两小时改为（比如）2 分钟，那将影响到该主机上所有开启了此选项的套接字。
  - SO_KEEPALIVE 由操作系统负责探查，因此即便是进程死锁或有其他异常，操作系统也会正常收发 TCP keepalive 消息，而对方无法得知这一异常。在这种情况下，SO_KEEPALIVE 参数明显失去了心跳检测应有的作用。

- 在应用层自己实现心跳检测

  可以在应用层模拟 SO_KEEPALIVE 的方式，用心跳包来模拟保活探测分节，例如[使用 libevent 进行检测](http://blog.51cto.com/yaocoder/1349003)。

  由于服务器通常要承担成千上万的并发连接，所以可以将心跳逻辑部署在客户端，由客户端在应用层进行心跳来模拟保活探测分节。
  - 客户端发送心跳包后，若多次收不到服务器的响应，则说明与此次连接产生了问题，因此可终止此 TCP 连接。
  - 服务端可监测所有客户端的 Socket 连接，若在一定时间间隔内未收到来自某个客户端的任何心跳包，则可以终止此 TCP 连接，这样就有效避免了 TCP 半开连接的情况。

## 8. Refer Links

[网络编程释疑之：TCP 协议的“流”特性](http://blog.51cto.com/yaocoder/1339958)

[TCP 半开连接问题](http://blog.51cto.com/yaocoder/1309358)

[关于应用层解决拆包粘包问题？](https://www.zhihu.com/question/37023914)

[TCP 粘包，拆包及解决方法](https://blog.insanecoder.top/tcp-packet-splice-and-split-issue/)

[Linux 中，一个端口能够接受 TCP 连接数的理论上限是？](https://www.nowcoder.com/questionTerminal/997f45dddc9b4c8ea7da76288aa439d1)

[单机最大 tcp 连接数](http://wanshi.iteye.com/blog/1256282)

[单台服务器上的并发 TCP 连接数可以有多少？](http://blog.51cto.com/yaocoder/1312821)

[The C10K problem](http://www.kegel.com/c10k.html)

[tcp 的可靠性到底指的是什么？](https://www.zhihu.com/question/49596182)

[TCP 协议存在那些缺陷？](https://www.zhihu.com/question/47560918)

[TCP 和 Udp 的区别是什么？](https://www.zhihu.com/question/47378601)

<!-- TODO: -->

[TCP 协议专场问题](https://zhuanlan.zhihu.com/p/30032980)