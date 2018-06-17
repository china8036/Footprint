- [计算机网络 - 传输层：Transport control protocol 段格式](#计算机网络---传输层transport-control-protocol-段格式)
  - [1. TCP 载荷](#1-tcp-载荷)
  - [2. TCP 头部](#2-tcp-头部)
  - [3. Refer Links](#3-refer-links)

# 计算机网络 - 传输层：Transport control protocol 段格式

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/13/b95e2c758894d9ff79f145930435be7d.jpg)

## 1. TCP 载荷

- TCP 载荷的理论上限为 65495 bytes。

  IP 数据包的载荷理论上限是 65515 bytes，因此 TCP 段的大小不能超过这个上限。TCP 头为 20 字节，因此 TCP 报文段的最大负载是为 65515 - 20 = 65495 字节。

- TCP 载荷为空的 TCP 段也是合法的，通常被用于确认和控制消息。

## 2. TCP 头部

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/13/f3d9ab773180d961376364a8c32fbd33.jpg)

- Source port (16 bits)

- Destination port (16 bits)

- Sequence number (32 bits): 包序号，用于解决网络包乱序 (reordering) 问题。Sequence Number 作为数据通信的序号，以保证应用层接收到的数据不会因为网络上的传输的问题而乱序（TCP 会用这个序号来还原数据）。万一发生丢包，也可以知道丢失的是哪一个包。
  - 若 SYN flag 为 1，表示要求对方进行同步，则 Sequence number 代表初始序列号，响应的 ACK 包中的 acknowledged number 值为该 Sequence number 加一。
  - 若 SYN flag 为 0，表示不要求对方进行同步，则 Sequence number 代表此次会话中 TCP 段的第一个数据字节的累积序列号。

- Acknowledgment number (32 bits): 确认号，用于解决丢包问题。
  > If the ACK flag is set then the value of this field is the next sequence number that the sender of the ACK is expecting. This acknowledges receipt of all prior bytes (if any). The first ACK sent by each end acknowledges the other end's initial sequence number itself, but no data.
  **每收到一个 SYN/FIN flag 为 1 的 TCP 段 A，都需要响应一个 ACK flag 为 1 的 TCP 段 B，并将 B 中的 Acknowledgment number 设置为 A 的 Sequence number + 1.**

- Data offset / TCP header length (4 bits)

  指明了 TCP header 包含多少个 32 位的字。这个字段实际上指明了数据部门在 TCP 段内的起始位置。

- Reserved (**3 bits**)
  
  3 bits 没有被使用的保留位置，通常设置为 0，用于将来修正协议的错误，而 30 年来 TCP 协议没有使用该字段也说明了 TCP 协议自始至终运行良好。

- Flags (**9 bits**): 包含了 9 个 1 bit 的标志位，用于指定包的类型，操作 TCP 状态机。
  - `NS (1 bit)`: 隐私保护 (experimental: see RFC 3540).
  
  - `CWR (1 bit)`: 用于 ECN 拥塞控制。Congestion Window Reduced (CWR) flag is set by the sending host to indicate that it received a TCP segment with the ECE flag set and had responded in congestion control mechanism (added to header by RFC 3168).
  
  - `ECE (1 bit)`: 用于 ECN 拥塞控制。ECN-Echo has a dual role, depending on the value of the SYN flag. It indicates:
    - If the SYN flag is set (1), that the TCP peer is ECN capable.
    - If the SYN flag is clear (0), that a packet with Congestion Experienced flag set (ECN=11) in IP header was received during normal transmission (added to header by RFC 3168). This serves as an indication of network congestion (or impending congestion) to the TCP sender.
  
  - `URG (1 bit)`: 表示 TCP 包的紧急指针域，用来保证 TCP 连接不被中断，并且督促中间层设备要尽快处理这些数据 ACK (1 bit)。
  
  - `ACK (1 bit)`: 几乎所有情况下该标志位是置位的，表示确认号字段是有效的。TCP 报头内的确认编号栏内包含的确认编号 (w+1，Figure-1) 为下一个预期的序列编号，同时提示远端系统已经成功接收所有数据。
  
  - `PSH (1 bit)`: 表示强制发送操作，即在数据包到达接收端以后，立即传送给应用程序，而不是在缓冲区中排队。
    
    当一个应用将数据传递给 TCP 时，由于 TCP 是面向字节流的传输，因此这部分数据可能立即被发送出去，也可能被缓冲起来等待收集到足够的数据后再发送，这完全由 TCP 程序自己决定。但对于某些应用程序，它们希望数据能够立即发送（如交互式游戏程序），为强制将数据发送出去，TCP 提供了 PUSH 标志位，将该位置 1 后可要求 TCP 不延迟传输。
  
  - `RST (1 bit)`: 表示连接重置。用来重置那些产生错误的连接，也被用来拒绝错误和非法的数据包。
  
  - `SYN (1 bit)`: 表示同步序号，用于建立连接过程。SYN 标志位和 ACK 标志位搭配使用，当连接请求的时候，SYN=1，ACK=0；连接被相应的时候，SYN=1，ACK= 1。本质上，SYN 位用于同时表示 CONNECTION REQUEST 和 CONNECTION ACCEPTED，然后进一步用 ACK 位来区分这 2 种情况。
    
    SYN 位还经常被用来进行端口扫描。扫描者发送一个只有 SYN 置位的数据包，如果对方主机响应了一个数据包回来，就表明这台主机存在这个端口。但由于这种扫描方式只是进行 TCP 三次握手的第一次握手，因此这种扫描的成功表示被扫描的机器不很安全，一台安全的主机将应该强制要求一个连接严格的进行 TCP 的三次握手。

  - `FIN (1 bit)`: 表示发送端已经达到数据末尾，也就是说双方的数据传送完成，用于终止一个连接。SYN 和 FIN 段都有序号，从而保证了这 2 种段以正确的顺序被处理。

    FIN 位也经常被用于进行端口扫描。当一个 FIN 标志的 TCP 数据包发送到一台计算机的特定端口，如果这台计算机响应了这个数据，并且反馈回来一个 RST 标志的 TCP 包，就表明这台计算机上没有打开这个端口，但是这台计算机是存在的；如果这台计算机没有反馈回来任何数据包，这就表明，这台被扫描的计算机存在这个端口。

- Window size (16 bits): 用于流量控制。
  
  window size 指定了从被确认的字节起可以接收多少个字节，即此段的发送者当前愿意接收的窗口大小单位的数量。
  
  window size 为 0 是合法的，说明当前已接收了 `Acknowledgment number - 1` 个字节，但接收端无法再消耗数据，因此不希望再接收数据。当接收端希望接收数据时，可以发送一个具有同样 Acknowledgment number 但 window size 不为 0 的通知。

- Checksum (16 bits)
  
  > The 16-bit checksum field is used for error-checking of the header, the Payload and a Pseudo-Header. The Pseudo-Header consist of the Source IP Address, the Destination IP Address, the protocol number for the TCP-Protocol (0x0006) and the length of the TCP-Headers including Payload (in Bytes).

  校验和提供了额外的可靠性，校验范围包括头、数据、概念性伪头。

- Urgent pointer (16 bits)
  
  紧急指针指向从当前序号开始找到紧急数据的字节偏移量，URGENT 标志位现已很少被使用，但仍保留在 TCP 协议中。

  当一个具有较高优先级数据的应用立即被处理时，发送端程序把一些控制信息放入数据流中，并将它连同 URGENT 标志位一起传递给 TCP，这一事件将导致 TCP 停止累积数据，将该连接上已有的所有数据立即传输出去。

- Options: 可选字段，最多 40 字节。每个选项的开始是 1 字节的 kind 字段，说明选项的类型。
  - 0: 选项表结束（1 字节）。
  - 1: 无操作（1 字节）用于选项字段之间的字边界对齐。
  - 2: 最大报文段长度（4 字节，Maximum Segment Size，MSS）通常在建立连接而设置 SYN 标志的数据包中指明这个选项，指明本端所能接收的最大长度的报文段。通常将 MSS 设置为（MTU-40）字节，携带 TCP 报文段的 IP 数据报的长度就不会超过 MTU，从而避免本机发生 IP 分片。只能出现在同步报文段中，否则将被忽略。
  - 3: 窗口扩大因子（4 字节，wscale），取值 0-14。用来把 TCP 的窗口的值左移的位数，使窗口值乘倍。只能出现在同步报文段中，否则将被忽略。这是因为现在的 TCP 接收数据缓冲区（接收窗口）的长度通常大于 65535 字节。
  - 4: sackOK 发送端支持并同意使用 SACK 选项。
  - 5: SACK 是对 ACK 的补充，使得接收端可以告诉发送端以及接收到段的序号范围。
  - 8: 时间戳（10 字节，TCP Timestamps Option，TSopt）。
    - 发送端的时间戳（Timestamp Value field，TSval，4 字节）。
    - 时间戳回显应答（Timestamp Echo Reply field，TSecr，4 字节）。

## 3. Refer Links
