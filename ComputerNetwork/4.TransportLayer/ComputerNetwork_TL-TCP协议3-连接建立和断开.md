- [计算机网络 - 传输层：Transport control protocol 基本操作](#计算机网络---传输层transport-control-protocol-基本操作)
  - [1. TCP 状态机](#1-tcp-状态机)
  - [2. 建立连接：三次握手](#2-建立连接三次握手)
    - [2.1. 过程](#21-过程)
    - [2.2. 结果](#22-结果)
    - [2.3. Sequence Number 计算](#23-sequence-number-计算)
    - [2.4. SYN Flood](#24-syn-flood)
    - [2.5. 为什么握手的次数是三次？](#25-为什么握手的次数是三次)
  - [3. 终止连接：四次挥手](#3-终止连接四次挥手)
    - [3.1. TIME_WAIT 状态的意义](#31-time_wait-状态的意义)
  - [4. Refer Links](#4-refer-links)

# 计算机网络 - 传输层：Transport control protocol 基本操作

## 1. TCP 状态机

从本质上讲，网络传输是没有连接的，包括 TCP 也是一样。**TCP 所谓的“面向连接”，实际上只不过是在通讯的双方共同维护一个 TCP 有限状态机，从而使得双方好像保持了“连接”**。  

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/cfe08663096766a102cc20bce4b35c65.jpg)

**TCP 有限状态机包含了 11 种状态**：
- CLOSED: 
- LISTEN: 
- SYN-SENT: 
- SYN-RECEIVED: 
- ESTABLISHED: 
- CLOSE-WAIT: 
- LAST-ACK: 
- FIN-WAIT-1: 
- FIN-WAIT-2: 
- CLOSING: 
- TIME-WAIT: 

统计当前各种状态的连接的数量：
```shell
netstat -pantu | awk '/^tcp/{++S[$(NF-1)]} END{for(a in S) print a, S[a]}'
```
运行结果：
```
LAST_ACK 14
SYN_RECV 348
ESTABLISHED 70
FIN_WAIT1 229
FIN_WAIT2 30
CLOSING 33
TIME_WAIT 18122
```

## 2. 建立连接：三次握手

为获得 TCP 服务，必须显式地在一台机器的套接字和另一台机器的套接字之间建立一个连接。TCP 连接的建立采用三次握手 (Three-way Handshake) 的过程，即整个过程由发送方请求建立连接、接收方确认、发送方再发送关于确认的确认三个过程组成。**所谓三次握手，是指建立一个 TCP 连接时，需要客户端和服务器总共发送 3 个包**。

在 socket 编程中，客户端执行 connect() 时就将触发三次握手的过程。

### 2.1. 过程

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/fd3130ffeb4e2d19650a461c6a531107.jpg)

- 第一次握手 (SYN=1, seq=x):

  客户端发送一个 SYN 标志位置 1 的 TCP 包，指明客户端请求连接的服务器的端口，以及初始序列号 X（保存在包头的 Sequence Number 字段里）。
  
  <!-- TODO: x 是怎么得到的？ -->
  
  发送完毕后，客户端进入 `SYN_SEND` 状态。

  eg: 此时 Flags 为 0x002，二进制是 0000 0010，即表示 SYN 有效

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/b1cc07bf1d11a4c10762c79021dd50ec.jpg)

- 第二次握手 (SYN=1, ACK=1, seq=y, ACKnum=x+1):

  服务器收到第一个握手包后，检查是否已有一个进程在目标端口上监听：
  - 若没有，则服务器会发送一个 RST 包作为响应，拒绝客户端的连接请求。
  - 若有，则服务器会发送 SYN + ACK 包应答，其中 SYN 标志位和 ACK 标志位均置 1。服务器端选择自己 ISN 序列号，放到 Sequence Number 字段里，同时将 Acknowledgement Number 设置为客户的 Sequence Number 加 1，即 X+1。 

  发送完毕后，服务器端进入 `SYN_RCVD` 状态。

  eg: 此时 Flags 为 0x012，二进制是 0001 0010，即表示有 SYN 和 ACK 有效

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/ee5f160bacb26ceea749f7316d5a7418.jpg)

- 第三次握手 (ACK=1，ACKnum=y+1):

  客户端收到第二个握手包后，发送 ACK 应答，其中 SYN 标志位为 0，ACK 标志位为 1。并把服务器发来 Sequence Number 字段 + 1，放在 Acknowledgement Number 字段中发送给对方。
  
  <!-- TODO: SEQ=? -->
  
  发送完毕后，客户端进入 `ESTABLISHED` 状态，当服务器端接收到这个包时，也进入 `ESTABLISHED` 状态。

  eg: 此时 Flags 为 0x010，二进制是 0001 0000，即表示 ACK 有效

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/f4bf935e29020f47e7a13cfd8dddbc6f.jpg)

至此 TCP 三次握手结束，连接建立，客户端与服务器可以开始传送数据。

### 2.2. 结果

- 连接服务器指定端口，建立 TCP 连接。

- 同步双方的序列号和确认号，即通知对方自己的 ISN (Inital Sequence Number)，并将本地 ACKnum 设置为对方 ISN 的值加一。

  发送方和接收方必须就它们将要为数据流中的字节指定的 sequence number 达成一致。这一过程称为同步。

- 交换 TCP 窗口大小信息。

### 2.3. Sequence Number 计算

根据 RFC 793，Sequence Number 计算过程如下：

- ISN 初始化
  
  **ISN 会和一个虚拟的时钟绑定在一起，这个时钟每 4 微秒会对 ISN 做加一操作，直到超过 2^32，又从 0 开始，即一个 ISN 的周期大约是 4.55 个小时。**因此，只要 TCP Segment 在网络上的最大存活时间 MSL (Maximum Segment Lifetime) 小于 4.55 小时，那么 ISN 就不会被重用。

  NOTE:
  - **MSL 在 RFC 1122 上建议是 2 分钟，但实际上，在 Linux 系统中 MSL 被设置为 30s；在 Windows 系统中 MSL 被设置为 240s 即 4 min**。
  - ISN 不可使用 Hard Code。例如，连接建好后始终用 1 来做 ISN，如果 client 发了 30 个 segment 过去，但是网络断了，于是 client 重连，又用了 1 做 ISN，但是之前连接的那些包到了，就被当成了新连接的包。此时，client 的 Sequence Number 可能是 3，而 Server 端认为 client 端的这个号是 30 了。

- Sequence Number

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/883579859015d43c2dd2c54a1cd30d5e.jpg)

  可以看到，三次握手建立 TCP 连接后，SeqNum 的增加是和传输的字节数相关的。三次握手后，来了两个 Len:1440 的包，而第二个包的 SeqNum 就成了 1441。然后第一个 ACK 回的是 1441，表示第一个 1440 收到了。

  注意：如果你用 Wireshark 抓包程序看 3 次握手，你会发现 SeqNum 总是为 0，但事实上不是这样的。Wireshark 为了显示更友好，使用了相对序号 (Relative SeqNum)，你只要在右键菜单中的 protocol preference 中取消掉就可以看到 Absolute SeqNum 了。

### 2.4. SYN Flood

在第二次握手时，服务器发送 SYN-ACK 之后，在收到客户端的 ACK 之前服务器处于 SYN_RCVD 状态，此时的 TCP 连接称为半连接 (half-open connect)。

服务器端会保存半连接 TCP 的序号，如果服务器端在一定时间内没有收到客户端回来的 ACK，那么服务端会向该序号的 TCP 连接重新发送 SYN-ACK：
- 在 Linux 下，默认重试次数为 5 次，重试的间隔时间从 1s 开始每次都翻倍，5 次的重试时间间隔为 1s, 2s, 4s, 8s, 16s，总共 31s，第 5 次发出后还要等 32s 才知道第 5 次也超时了，所以，总共需要 `1s + 2s + 4s+ 8s+ 16s + 32s` = **63s**，TCP 才会最终断开这个连接。

因此，这意味着一个恶意的发送端很容易占据服务器的资源，使用 SYN Flood 攻击。攻击客户端只需在短时间内伪造大量不存在的 IP 地址，向服务器不断地发送 SYN 包，服务器回复确认包，并等待客户的确认。由于源地址是不存在的，服务器需要不断的重发直至超时，这些伪造的 SYN 包将长时间占用未连接队列，正常的 SYN 请求被丢弃，导致目标系统运行缓慢，严重者会引起网络堵塞甚至系统瘫痪。SYN 攻击是一种典型的 DoS/DDoS 攻击。

- 如何检测？

  当你在服务器上看到大量的半连接状态时，且源 IP 地址分布随机，基本上可以断定这是 SYN 攻击。在 Linux/Unix 上可以使用系统自带的 netstats 命令来检测 SYN 攻击。

- 如何防御？

  SYN 攻击不能完全被阻止，除非将 TCP 协议重新设计。因此，我们所做的是尽可能的减轻 SYN 攻击的危害，常见的防御 SYN 攻击的方法有如下几种：
  - 缩短超时（SYN Timeout）时间。
  - 通过 tcp_synack_retries 参数减少重试次数。
  - 通过 tcp_max_syn_backlog 参数增加最大半连接数。
  - 通过 tcp_abort_on_overflow 参数定义超过服务器能力时的行为（如直接拒绝连接）。
  - 过滤网关防护。
  - 使用 SYN Cookies 技术：
    
    主机不记忆 TCP 半连接信息，而是选择一个加密生成的序号 x，将它放在出境段中。当接收到客户端 ACK 包（ACKNum=x+1），主机运行相同的加密函数，只要该函数的输入是已知的，就能重新生成正确的序号。

    但应注意，由于 SYN Cookies 是妥协的 TCP 传输，并不严谨，因此 SYN Cookies 技术不可用于大负载的连接情况。

### 2.5. 为什么握手的次数是三次？

为什么 TCP 握手的次数是三次？

TCP 作为一种可靠传输控制协议，其核心思想：既要保证数据可靠传输，又要提高传输的效率。三次握手正是**可靠性和效率的权衡**。

- 如果是两次握手：

  1. A 发送同步信号 SYN + A 的 Initial sequence number。
  1. B 发送同步信号 SYN + B 的 Initial sequence number + B 的 ACK sequence number。

  容易看出，这样的握手机制不具备可靠性。虽然 A 与 B 就 A 的初始序列号达成了一致，但是 B 无法知道 A 是否已经接收到自己的同步信号，如果这个同步信号丢失了，A 和 B 就 B 的初始序列号将无法达成一致。因此，A 在接收到 B 的 SYN 信号后应立即响应 ACK，也就成了三次握手机制。

- 如果是四次握手：

  1. A 发送 SYN 报文给 B，这是第一次报文交互。
  1. B 发送 ACK 确认 A 的 SYN 报文，这是第二次报文交互。
  1. B 发送自己的 SYN 报文给 A，这是第三次报文交互。
  1. A 需要 ACK 确认 B 的 SYN 报文，这是第四次报文交互。

  这样的握手机制虽然保证了可靠性，但是传输效率却较低。报文 2、3 为何要分开发送呢？增加了延迟的同时还白白浪费了网络的带宽，因此完全可以将 2、3 步骤合并起来，同样也就成了三次握手机制。

- 握手次数超过 3 次后，可靠性都是一样的。TODO:

综上，只有三次握手，才是 TCP 传输可靠性和传输效率的权衡保证。

## 3. 终止连接：四次挥手

TCP 的连接的终止过程总共需要发送四个包，因此称为四次挥手 (Four-way handshake)，也叫做改进的三次握手。由于 TCP 连接是全双工的，客户端和服务器处于完全平等的地位，因此客户端或服务器均可主动发起挥手动作。

在 socket 编程中，任何一方执行 close() 操作即可触发挥手操作。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/a7b9d01d939f512eae557bd6040e2b8a.jpg)

- 第一次挥手 (FIN=1，seq=x)

  假设客户端想要关闭连接，则客户端首先应发送一个 FIN 标志位置为 1 的 FIN 包，表示自己已经没有数据可以发送了，但是仍然可以接受数据。

  <!-- todo: 所谓的半关闭状态？ -->
  
  发送完毕后，客户端进入 `FIN_WAIT_1` 状态。

- 第二次挥手 (ACK=1，ACKnum=x+1)

  服务器端确认客户端的 FIN 包，发送一个确认包，表明自己接受到了客户端关闭连接的请求，但还没有做好关闭连接的准备。

  发送完毕后，服务器端进入 `CLOSE_WAIT` 状态，客户端接收到这个确认包之后，进入 `FIN_WAIT_2` 状态，等待服务器端关闭连接。

- 第三次挥手 (FIN=1，seq=y)

  服务器端做好关闭连接的准备后，向客户端发送结束连接请求的 FIN 包，即 FIN flag 置为 1。

  发送完毕后，服务器端进入 `LAST_ACK` 状态，等待来自客户端的最后一个 ACK。若等待超时都没有收到 ACK，就会触发重发 FIN。

- 第四次挥手 (ACK=1，ACKnum=y+1)

  客户端接收到来自服务器端的关闭请求，发送一个 ACK 包，并进入 `TIME_WAIT` 状态，等待可能出现的要求重传 ACK 包的情况。

  服务器端接收到这个确认包之后，关闭连接，进入 `CLOSED` 状态。

  客户端等待了某个固定时间（即 2 Maximum Segment Lifetime 即 2MSL）之后，若没有收到服务器端的 ACK，即可认为服务器端已经正常关闭连接，于是自己也关闭连接，进入 `CLOSED` 状态。

### 3.1. TIME_WAIT 状态的意义

[TIME_WAIT and its design implications for protocols and scalable client server systems](http://www.serverframework.com/asynchronousevents/2011/01/time-wait-and-its-design-implications-for-protocols-and-scalable-servers.html)

- 为什么要有 TIME_WAIT，而不直接转成 CLOSED 状态呢？TIME_WAIT 状态存在的意义？
  - 实现 TCP 全双工连接的可靠释放。

    TCP 协议在关闭连接的四次握手过程中，最终的 ACK 是由主动关闭连接的一端（A 端）发出的，如果这个 ACK 丢失，对方（B 端）将重发最终的 FIN，因此 A 端必须维护状态信息（TIME_WAIT）允许它重发最终的 ACK。**如果 A 端不维持 TIME_WAIT 状态，而是直接进入 CLOSED 状态，那么 A 端收到重发的 ACK 后将响应 RST，进而 B 端收到 RST 后将解释成一个错误**（在 java 中会抛出 connection reset 的 SocketException)，然而这事实上只是正常的关闭连接过程，并非异常。

    因而，要实现 TCP 全双工连接的正常终止，必须处理终止过程中四个阶段中任何一个阶段握手包的丢失情况，因此主动关闭连接的 A 端必须维持 TIME_WAIT 状态 。

  - 为旧的数据包在网络因过期而消失留足时间，避免前后两次连接数据错乱的情况。

    TCP 分节可能由于路由器异常而“迷途”，在迷途期间，TCP 发送端可能因确认超时而重发这个分节，迷途的分节在路由器修复后也会被送到最终目的地，这个迟到的迷途分节到达时就可能会引起问题。在关闭“前一个连接”之后，如果马上又重新建立起一个相同的 IP 和端口之间的“新连接”，“前一个连接”的迷途重复分组就可能在“前一个连接”终止后到达，被“新连接”收到了。
    
    为了避免这个情况，TCP 协议不允许处于 TIME_WAIT 状态的连接启动一个新的可用连接，即**使 TIME_WAIT 状态持续 2MSL，就可以保证当成功建立一个新 TCP 连接的时候，来自旧连接重复分组已经在网络中消逝，避免 2 次连接的数据发生混乱的情况**。

- 为什么等待时间设置为 2MSL？
  
  TIME_WAIT 的等待时间需要确保有足够的时间让对方收到了 ACK。如果被动关闭的那方没有收到 ACK，就会重发 FIN，一来一去正好是 2 MSL。

- 怎么查看 TIME_WAIT 状态的具体持续时间？
  ```shell
  cat /proc/sys/net/ipv4/tcp_fin_timeout
  ```
  或者
  ```shell
  sysctl net.ipv4.tcp_fin_timeout
  ```
  通过查看 TIME_WAIT 状态的具体时间，也可间接的查看系统 MSL 的具体时间设置（除以 2 即可）。

- TIME_WAIT 状态可能产生什么危害？
  - 对于大多数网络服务，一般是由客户端主动断开连接，因此 TIME_WAIT 状态发生在客户端，不会造成危害。
  - 但如果是由服务器主动关闭 TCP 连接，将导致服务器端存在大量的处于 TIME_WAIT 状态的 socket, 甚至可能比处于 ESTABLISHED 状态下的 socket 还要多。每一个状态非 CLOSED 的 socket 连接都至少需要占用一个五元组表项、一个文件描述符 (fd)，而由于 TIME_WAIT 状态下的 socket 不能被回收使用，因此这将严重影响服务器的处理能力，甚至耗尽可用的 socket，拒绝服务。

- TIME_WAIT 状态如何避免？

  - 设置 TIME_WAIT 状态的 TCP 连接端口可重用
  
    服务器可以设置 SO_REUSEADDR 套接字选项来通知内核，如果端口忙，但 TCP 连接位于 TIME_WAIT 状态时，可以重用端口。在一个非常有用的场景就是，如果你的服务器程序停止后想立即重启，而新的套接字依旧希望使用同一端口，此时 SO_REUSEADDR 选项就可以避免 TIME_WAIT 状态。

  - 修改 / 缩短 TIME_WAIT 状态的持续时间
    - 在 Linux 下可通过修改 `/etc/sysctl.conf` 中的内核参数 `net.ipv4.tcp_fin_timeout` 来实现。
    - 在 Windows 下可通过在 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters, 添加名为 TcpTimedWaitDelay 的 DWORD 键来实现。

  - ip_conntrack
  - tcp_tw_recycle
  - tcp_tw_reuse
  - tcp_max_tw_buckets

## 4. Refer Links

[TCP 协议：笔试面试知识点整理](https://hit-alibaba.github.io/interview/basic/network/TCP.html)

[TCP 为什么是三次握手，而不是两次或四次？](https://www.zhihu.com/question/24853633)

[车小胖谈网络：TCP 为什么是三次握手，而不是两次或四次？](https://mp.weixin.qq.com/s?__biz=MzIxNTM3NDE2Nw==&mid=2247483950&idx=1&sn=32c0fab8adbe4c3fd9cc8be905d50e37&chksm=97980216a0ef8b00c1a04f87af0df15be8b657e8a68be3ea3849e139f765e156cc5c19a1aba0&mpshare=1&scene=1&srcid=0913DRksfDipGDgIbRUggOP3#rd)

[面试总结之 time_wait 状态产生的原因，危害，如何避免](https://blog.csdn.net/u013616945/article/details/77510925)

[大量 TIME_WAIT 解决办法](http://coolnull.com/3605.html)

[再叙 TIME_WAIT](https://huoding.com/2013/12/31/316)