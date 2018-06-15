- [计算机网络 - 传输层：Transport control protocol](#计算机网络---传输层transport-control-protocol)
  - [1. 基本概念](#1-基本概念)
    - [1.1. 两个经典问题](#11-两个经典问题)
      - [1.1.1. 拜占庭将军问题](#111-拜占庭将军问题)
      - [1.1.2. 两军问题](#112-两军问题)
    - [1.2. TCP 协议概述](#12-tcp-协议概述)
  - [2. TCP 段格式](#2-tcp-段格式)
    - [2.1. TCP 载荷](#21-tcp-载荷)
    - [2.2. TCP 头部](#22-tcp-头部)
  - [3. 基本操作](#3-基本操作)
    - [3.1. TCP 分段 (TCP Segment)](#31-tcp-分段-tcp-segment)
      - [3.1.1. MSS](#311-mss)
      - [3.1.2. TCP 分段](#312-tcp-分段)
    - [3.2. TCP 状态机](#32-tcp-状态机)
    - [3.3. 建立连接：三次握手](#33-建立连接三次握手)
      - [3.3.1. 过程](#331-过程)
      - [3.3.2. 结果](#332-结果)
      - [3.3.3. Sequence Number 计算](#333-sequence-number-计算)
      - [3.3.4. SYN Flood](#334-syn-flood)
    - [3.4. 终止连接：四次挥手](#34-终止连接四次挥手)
      - [3.4.1. TIME_WAIT 状态的意义](#341-time_wait-状态的意义)
  - [4. TCP KeepAlive 机制](#4-tcp-keepalive-机制)
    - [4.1. TCP 断连问题](#41-tcp-断连问题)
    - [4.2. 基本原理](#42-基本原理)
    - [4.3. KeepAlive 缺陷](#43-keepalive-缺陷)
    - [4.4. TCP KeepALive 与应用层心跳](#44-tcp-keepalive-与应用层心跳)
  - [5. 可靠性保证：重传 (Retransmit)](#5-可靠性保证重传-retransmit)
    - [5.1. 计时器管理](#51-计时器管理)
      - [5.1.1. RFC 经典算法](#511-rfc-经典算法)
      - [5.1.2. Karn / Partridge 算法](#512-karn--partridge-算法)
      - [5.1.3. Jacobson / Karels 算法](#513-jacobson--karels-算法)
    - [5.2. 重传机制](#52-重传机制)
      - [5.2.1. 基本原则](#521-基本原则)
      - [5.2.2. 超时重传机制 (Timeout Retransmit)](#522-超时重传机制-timeout-retransmit)
      - [5.2.3. 快速重传机制 (Fast Retransmit)](#523-快速重传机制-fast-retransmit)
      - [5.2.4. 选择确认重传 (Selective Acknowledgment Retransmit)](#524-选择确认重传-selective-acknowledgment-retransmit)
      - [5.2.5. 重复选择确认重传 (Duplicate Selective Acknowledgment Retransmit)](#525-重复选择确认重传-duplicate-selective-acknowledgment-retransmit)
  - [6. 流量控制：滑动窗口协议](#6-流量控制滑动窗口协议)
    - [6.1. 相关字段](#61-相关字段)
    - [6.2. 基本原理](#62-基本原理)
    - [6.3. 作用](#63-作用)
      - [6.3.1. 提供面向流的可靠性](#631-提供面向流的可靠性)
      - [6.3.2. 提供流量控制](#632-提供流量控制)
      - [6.3.3. 解决延迟确认问题](#633-解决延迟确认问题)
  - [7. 拥塞控制 (Congestion Handling)](#7-拥塞控制-congestion-handling)
    - [7.1. 慢启动算法 (Slow Start)](#71-慢启动算法-slow-start)
    - [7.2. 拥塞避免算法 (Congestion Avoidance)](#72-拥塞避免算法-congestion-avoidance)
    - [7.3. 拥塞发生算法](#73-拥塞发生算法)
    - [7.4. 快速恢复算法 (Fast Recovery)](#74-快速恢复算法-fast-recovery)
  - [8. 其它问题](#8-其它问题)
    - [8.1. Linux 下单机 TCP 连接的最大并发量](#81-linux-下单机-tcp-连接的最大并发量)
      - [8.1.1. 理论最大并发量](#811-理论最大并发量)
      - [8.1.2. 实际最大并发量](#812-实际最大并发量)
    - [8.2. TCP 拆包粘包问题](#82-tcp-拆包粘包问题)
      - [8.2.1. 问题描述](#821-问题描述)
      - [8.2.2. 解决方法](#822-解决方法)
    - [8.3. TCP 半开连接问题](#83-tcp-半开连接问题)
      - [8.3.1. 问题描述](#831-问题描述)
      - [8.3.2. 处理方案](#832-处理方案)
    - [8.4. TCP 与 UDP 的区别](#84-tcp-与-udp-的区别)
  - [9. Refer Links](#9-refer-links)

# 计算机网络 - 传输层：Transport control protocol

## 1. 基本概念

### 1.1. 两个经典问题

#### 1.1.1. 拜占庭将军问题

拜占庭将军问题深入探讨：http://www.8btc.com/baizhantingjiangjun 

问题描述：

拜占庭帝国想要进攻一个强大的敌人，为此派出了 10 支军队去包围这个敌人。这个敌人虽不比拜占庭帝国，但也足以抵御 5 支常规拜占庭军队的同时袭击。基于一些原因，这 10 支军队不能集合在一起单点突破，必须在分开的包围状态下同时攻击。他们任一支军队单独进攻都毫无胜算，除非有至少 6 支军队同时袭击才能攻下敌国。他们分散在敌国的四周，依靠通信兵相互通信来协商进攻意向及进攻时间。

困扰这些将军的问题是，他们不确定他们中是否有叛徒，叛徒可能擅自变更进攻意向或者进攻时间。在这种状态下，拜占庭将军们能否找到一种分布式的协议来让他们能够远程协商，从而赢取战斗？

#### 1.1.2. 两军问题

https://www.cnblogs.com/charlesblc/p/6271472.html 

两军问题 / 两军对垒问题 (two-army problem)(two Generals' Problem)，是计算机领域的一个思想实验，用来阐述**在一个不可靠的通信链路上试图通过通信以达成一致是存在缺陷的和困难的，是在计算机通信领域首个被证明无解的问题，由此也可推论出，随机通信失败条件下的“拜占庭将军问题”也同样无解**。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/11/4b151e199e3ca4121a5ba119660253cc.jpg)

- 问题描述：

  一支白军被围困在一个山谷中，山谷的两侧是蓝军。困在山谷中的白军人数多余山谷两侧的任意一支蓝军，而少于两支蓝军的中和。若一支蓝军对白军单独发起进攻，则必败无疑；但若两支蓝军同时发起进攻，则可取胜。两只蓝军希望同时发起进攻，这样他们就要传递消息，以便确定发起进攻的具体时间。假设他们只能派遣士兵穿越白军所在的山谷（唯一的通信信道）来传递消息，那么在穿越山谷是，士兵有可能被俘虏，从而造成消息的丢失。现在的问题是：如何通信，以便蓝军必胜。

- 设计：
  - 假设一支蓝军指挥官发出消息：“我建议在明天佛晓发起进攻，请确认。”如果消息到达了另一支蓝军，其指挥官同意这一建议，并且他的回信也安全的送到，那么能否进攻呢？不能。这是一个两步握手协议，因为该指挥官无法知道他的回信是否安全送到了，所以，他不能发起进攻；
  - 改进协议，将两步握手协议改为三步握手协议，这样，最初提出建议的指挥官必须确认对该建议的应答信息。假如信息没有丢失，并收到确认消息，则他必须将收到的确认信息告诉对方，从而完成三步握手协议。
  - 然而，这样他就无法知道消息是否被对方收到，因此，他不能发起进攻。那么采用四步握手协议会如何呢？结果仍被证明是于事无补。

- 结论是：不存在使红军必胜的通信约定（协议）。最后信息的发送者，永远无法知道这个信息是否到达。

两军问题的根本问题在于信道的不可靠，反过来说，如果传递消息的信道是可靠的，两军问题可解。然而，并不存在这样一种信道，所以两军问题在经典情境下是不可解的。

但可以通过一种相对可靠的方式来解决大部分情形，即 TCP 协议。

### 1.2. TCP 协议概述

[传输控制协议 (Transmission Control Protocol, TCP)](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) **是一种面向连接的、可靠的、基于端到端字节流的传输层通信协议，是专门为了在不可靠的互联网络上提供可靠的端到端字节流而设计的传输协议**，由 IETF 的 [RFC 793](http://tools.ietf.org/html/rfc793) 定义。对于大多数 Internet 应用来说，它们需要可靠的、按序传递交的传输特性，这种情况下就应该使用 TCP 协议。

- TCP 传输实体

  支持 TCP 的机器都有一个 **TCP 传输实体**，或者是用户进程或者是操作系统内核，都可以管理 TCP 流和跟 IP 层的接口：
  - TCP 实体接收本地进程的用户数据流，将其分割成不超过 64 kB 的分片（实际上通过以太网传输要考虑 MTU 限制，去掉 IP 头和 TCP 头，通常不超过 1460 字节），每个分片 / 分段以单独的 IP 数据包形式发送。
  - 当包含 TCP 数据段的报文到达某台机器的时候，被提交给该机器的 TCP 传输实体，传输实体再将其重构出原始的字节流。

- TCP 连接的全球唯一标识：
  - 三元组：协议 + 本地地址 + 本地端口号。
  - 五元组：协议 + 本地地址 + 本地端口号 + 远端地址 + 远端端口号。

- TCP 协议基本特点

  - TCP 的一个关键特征，也是主导整个协议设计的特征是 **TCP 连接上的每个字节都有它自己独有的 32 位序号**。数据包携带的 32 位序号可用在一个方向上的滑动窗口以及另一个方向上的确认。

  - 发送端和接收端的 TCP 实体以段的形式交换数据，TCP 段由一个固定的 20 bytes 头和随后 0 个或多个数据字节构成。

  - TCP 连接**传输的是一个字节流而不是一个消息流**，端到端之间不保留消息的边界，因此 TCP 传输可能会出现 TCP 粘包问题。
  
  - TCP 段的实际大小由 TCP 应用程序决定，它可以将多次写操作中的数据累积后放在一个段中发送，也可以将一次写操作中的数据分割到多个段中发送。限制一个 TCP 段实际大小的因素有 2 个：
    - IP 数据包的载荷理论上限是 65515 bytes，因此 TCP 段的大小不能超过这个上限。TCP 头为 20 字节，因此 TCP 报文段的最大负载是为 65515 - 20 = 65495 字节。
    - 每个网络都有 MTU 限制，发送端和接收端的每个段必须适合 MTU 才能以单个不分段的数据包发送和接收，因此对于传输层有相应的 MSS 限制。

  - 所有的 TCP 连接是**全双工**的（同时双向传输）和**端到端**的（每条连接只有两个端点）。

  - **在一个 TCP 连接中，仅有两方进行彼此通信，因此 TCP 不支持组播和广播**。

  - **TCP 通信建立在面向连接的基础上，实现了一种"虚电路"的概念**。双方通信之前，需要先建立一条连接，然后双方就可以在其上发送数据流。这种数据交换方式能提高效率，但事先建立连接和事后拆除连接需要开销。
    <!-- todo: -->
  - TCP 使用校验和，确认和重传机制来保证可靠传输。

  - TCP 给数据分节进行排序，并使用累积确认保证数据的顺序不变和非重复。

  - TCP 使用滑动窗口机制来实现流量控制，通过具有动态窗口大小的滑动窗口协议进行拥塞控制。
    
    当发送端传送一个 TCP 段时，会启动一个计时器。当该 TCP 段到达接收方时，接收端的 TCP 实体返回一个携带了确认号和剩余窗口大小的段（若有数据要发送则包含数据，否则直接发送不包含数据的 TCP 段），并且确认号的值等于接收端期望接受的下一个序号。如果发送方的计时器在确认段到达之前超时，则发送端会再次发送原来的段。

  - TCP 并不能保证数据一定会被对方接收到，因为在不可靠的信道上这是不可能的。TCP 能够做到的是，如果有可能，就把数据递送到接收方，否则就（通过放弃重传并且中断连接）通知用户。因此**准确说 TCP 也不是 100% 可靠的协议，它所能提供的是数据的可靠递送或故障的可靠通知**。

## 2. TCP 段格式

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/13/b95e2c758894d9ff79f145930435be7d.jpg)

### 2.1. TCP 载荷

- TCP 载荷的理论上限为 65495 bytes。

  IP 数据包的载荷理论上限是 65515 bytes，因此 TCP 段的大小不能超过这个上限。TCP 头为 20 字节，因此 TCP 报文段的最大负载是为 65515 - 20 = 65495 字节。

- TCP 载荷为空的 TCP 段也是合法的，通常被用于确认和控制消息。

### 2.2. TCP 头部

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

- Reserved (3 bits)
  
  3 bits 没有被使用的保留位置，通常设置为 0，用于将来修正协议的错误，而 30 年来 TCP 协议没有使用该字段也说明了 TCP 协议自始至终运行良好。

- Flags (9 bits): 包含了 8 个 1 bit 的标志位，指定包的类型，操作 TCP 状态机。
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

## 3. 基本操作

### 3.1. TCP 分段 (TCP Segment)

#### 3.1.1. MSS

最大分段大小 (Maximum Segment Size, MSS) 是指 TCP 数据包每次能够传输的最大数据分段（不包括 TCP Header），单位为字节。

```
                         _____________MTU______________          
                        |                              |
--------------------------------------------------------
| MAC 头 14 字节 | IP 头 20 字节 | TCP 头 20 字节 | 数据 |
--------------------------------------------------------
                                                |      |
                                                ---MSS--
```

TCP 协议中 MSS 的计算方法：
> MSS = MTU - IP Header - TCP Header
eg:
- 以太网 MSS = 1500 - 20 -20 = 1460 bytes. 
- IEEE 802.3/802.2 MSS = 1492 - 20 - 20 = 1452 bytes.

#### 3.1.2. TCP 分段

TCP 如何避免 IP 分片？

为避免 IP 分片，达到最佳的传输效能，TCP 会先进行 MSS 协商，并对超过 MSS 的数据流进行 TCP 分段。

- MSS 协商

  TCP 协议在建立连接的时候通常先要协商双方的 MSS 值，通讯双方会根据双方提供的 MSS 值的最小值确定为这次连接的 MSS 值。

  TCP 在三次握手的第一个 SYN 消息中有一个选项 option 4，通告双方的 MSS，并会选择较小的一方作为这个 TCP 连接的协定 MSS，即 MSS = minimum{MSSs, MSSr}。从而使得双向通信都可以避免因为 IP 包太大引起的分片。
  
  但实际上，MSS 协商只能解决源主机、目的主机之间 first hop 的 MTU 问题，可以保证第一跳不分片，但是不能克服路径中有更小的 MTU 而造成的分片（但这是少数情况）。

- TCP 分段

  - 一旦传递给 TCP 层的数据流超过了 MSS，则在传输层会对 TCP 包进行分段，以避免在 IP 层发生分片行为。通过 TCP 分段，TCP 将通信过程中发生 IP 分片的机会大幅度降低。
  - 而对于 UDP / ICMP 协议，由于不具备分段机制，因此只能由 IP 层进行 IP 分片。
    - 在普通的局域网环境下，UDP 的数据最大为 1472 字节最好（避免分片重组）。
    - Internet 中的路由器可能有设置成不同的值（小于默认值），Internet 上的标准 MTU 值为 576，所以 Internet 的 UDP 编程时数据长度最好在 576－20－8＝548 字节以内。
  
### 3.2. TCP 状态机

从本质上讲，网络传输是没有连接的，包括 TCP 也是一样。TCP 所谓的“面向连接”，实际上只不过是在通讯的双方共同维护一个 TCP 有限状态机，从而使得双方好像保持了“连接”。  

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/cfe08663096766a102cc20bce4b35c65.jpg)

TCP 有限状态机包含了 11 种状态：
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

### 3.3. 建立连接：三次握手

为获得 TCP 服务，必须显式地在一台机器的套接字和另一台机器的套接字之间建立一个连接。TCP 连接的建立采用三次握手 (Three-way Handshake) 的过程，即整个过程由发送方请求建立连接、接收方确认、发送方再发送关于确认的确认三个过程组成。**所谓三次握手，是指建立一个 TCP 连接时，需要客户端和服务器总共发送 3 个包**。

在 socket 编程中，客户端执行 connect() 时就将触发三次握手的过程。

#### 3.3.1. 过程

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/fd3130ffeb4e2d19650a461c6a531107.jpg)

- 第一次握手 (SYN=1, seq=x):

  客户端发送一个 SYN 标志位置 1 的 TCP 包，指明客户端请求连接的服务器的端口，以及初始序列号 X（保存在包头的 Sequence Number 字段里）。
  <!-- todo: x 是怎么得到的？ -->
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
  <!-- todo: seq=? -->
  发送完毕后，客户端进入 `ESTABLISHED` 状态，当服务器端接收到这个包时，也进入 `ESTABLISHED` 状态。
  
  eg: 此时 Flags 为 0x010，二进制是 0001 0000，即表示 ACK 有效

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/f4bf935e29020f47e7a13cfd8dddbc6f.jpg)

至此 TCP 三次握手结束，连接建立，客户端与服务器可以开始传送数据。

#### 3.3.2. 结果

- 连接服务器指定端口，建立 TCP 连接。

- 同步双方的序列号和确认号，即通知对方自己的 ISN (Inital Sequence Number)，并将本地 ACKnum 设置为对方 ISN 的值加一。

  发送方和接收方必须就它们将要为数据流中的字节指定的 sequence number 达成一致。这一过程称为同步。

- 交换 TCP 窗口大小信息。

#### 3.3.3. Sequence Number 计算

根据 RFC 793，Sequence Number 计算过程如下：

- ISN 初始化
  
  **ISN 会和一个虚拟的时钟绑定在一起，这个时钟每 4 微秒会对 ISN 做加一操作，直到超过 2^32，又从 0 开始，即一个 ISN 的周期大约是 4.55 个小时。**因此，只要 TCP Segment 在网络上的最大存活时间 MSL (Maximum Segment Lifetime) 小于 4.55 小时，那么 ISN 就不会被重用。

  NOTE:
  - 实际上，在 Linux 系统中 MSL 被设置为 30s。
  - ISN 不可使用 Hard Code。例如，连接建好后始终用 1 来做 ISN，如果 client 发了 30 个 segment 过去，但是网络断了，于是 client 重连，又用了 1 做 ISN，但是之前连接的那些包到了，就被当成了新连接的包。此时，client 的 Sequence Number 可能是 3，而 Server 端认为 client 端的这个号是 30 了。

- Sequence Number

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/14/883579859015d43c2dd2c54a1cd30d5e.jpg)

  可以看到，三次握手建立 TCP 连接后，SeqNum 的增加是和传输的字节数相关的。三次握手后，来了两个 Len:1440 的包，而第二个包的 SeqNum 就成了 1441。然后第一个 ACK 回的是 1441，表示第一个 1440 收到了。

  注意：如果你用 Wireshark 抓包程序看 3 次握手，你会发现 SeqNum 总是为 0，但事实上不是这样的。Wireshark 为了显示更友好，使用了相对序号 (Relative SeqNum)，你只要在右键菜单中的 protocol preference 中取消掉就可以看到 Absolute SeqNum 了。

#### 3.3.4. SYN Flood

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

### 3.4. 终止连接：四次挥手

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

#### 3.4.1. TIME_WAIT 状态的意义

[TIME_WAIT and its design implications for protocols and scalable client server systems](http://www.serverframework.com/asynchronousevents/2011/01/time-wait-and-its-design-implications-for-protocols-and-scalable-servers.html)

为什么要有 TIME_WAIT，而不直接转成 CLOSED 状态呢？
- 提供足够的时间让这个连接不会跟后面的连接混在一起（有些路由器会缓存 IP 数据包，如果连接被重用了，那么这些延迟收到的包就有可能会跟新连接混在一起）。

为什么等待时间设置为 2MSL？
- TIME_WAIT 的等待时间需要确保有足够的时间让对方收到了 ACK。如果被动关闭的那方没有收到 ACK，就会重发 FIN，一来一去正好是 2 MSL。

## 4. TCP KeepAlive 机制

### 4.1. TCP 断连问题

[TCP 连接断连问题剖析](https://www.ibm.com/developerworks/cn/aix/library/0808_zhengyong_tcp/index.html)

[网络编程释疑之：TCP 连接拔掉网线后会发生什么](http://blog.51cto.com/yaocoder/1589919)

> 当客户端与服务器建立起正常的 TCP 连接后，如果客户主机掉线（网线断开、电源掉电、或系统崩溃等），服务端进程将永远不会知道（通过常用的 select/epoll 都监测不到断开或错误事件）。

因此，如果在 Server-Client 已建立了长连接的期间，断开客户端与服务器的物理连接，可能存在以下情况：

- 如果断开的时间短暂，在服务端 SO_KEEPALIVE 设定的探测时间间隔内，并且两端在此期间没有任何针对此长连接的网络操作，也就是说断开期间不足以让服务端“发现”客户端的连接出错了。因此，当连上网线后此 TCP 连接可以自动恢复，继续进行正常的网络操作。

- 如果断开的时间很长，超出了 SO_KEEPALIVE 设定的探测时间间隔，或者两端期间在此有了任何针对此长连接的网络操作，也就是说断开期间服务端“发现”了客户端的连接出错了，于是服务端会抛弃这个连接。因此，当连上网线时就会出现 ETIMEDOUT 或者 ECONNRESET 的错误，必须重新建立一个新的长连接进行网络操作。

### 4.2. 基本原理

由于 TCP 断连问题，在长时间无数据交互的时间段内，交互双方都有可能出现掉电、死机、异常重启等各种意外，当这些意外发生之后，这些 TCP 连接并未来得及正常释放，在软件层面上，连接的另一方并不知道对端的情况，它会一直维护这个连接，长时间的积累会导致非常多的半打开连接，造成端系统资源的消耗和浪费。

为解决这个问题，在传输层 TCP 提供了 [KeepAlive 机制](http://www.tldp.org/HOWTO/html_single/TCP-Keepalive-HOWTO/)，主流的操作系统内核都支持了这个机制的实现。

TCP KeepAlive 的基本原理是，隔一段时间给连接对端发送一个探测包，如果收到对方回应的 ACK，则认为连接还是存活的，在超过一定重试次数之后还是没有收到对方的回应，则丢弃该 TCP 连接。

HTTP 应用层协议中的 KeepAlive 字段就是基于 TCP 的这个机制实现的，且几乎所有的浏览器都默认会使用 TCP 的这个机制，重用一个 TCP 连接来处理多个 HTTP 请求，然后让客户端去断链接（浏览器可能会非常贪婪，他们不到万不得已不会主动断连接）。

### 4.3. KeepAlive 缺陷

TCP KeepAlive 机制主要存在以下 2 个问题：

- TCP KeepAlive 监测的方式是发送一个 probe 包，会给网络带来额外的流量。

- TCP KeepAlive 检查不到机器断电、网线拔出、防火墙这些断线情况，只能在内核层级监测连接的存活与否，而连接的存活不一定代表服务的可用。本质上来说，KeepAlive 是用来检测长时间不活跃的连接的，所以，不适合用来及时检测连接的状态。
  
  例如，当一个服务器 CPU 进程服务器占用达到 100%，已经卡死不能响应请求了，但 TCP KeepAlive 依然会认为连接是存活的。因此 TCP KeepAlive 对于应用层程序的价值相对较小，**需要做连接保活的应用层程序（例如 QQ 等长连接需求的程序），一般需要在应用层实现自己的心跳检测和保活功能，并在获知了断线之后，可以按照服务器逻辑进行相应操作（如断线后的数据清理、重新连接等）。**。

  <!-- [既然 TCP 中有心跳检测（KeepAlive 机制）了，为什么还要在应用层做心跳检测？](https://www.cnblogs.com/1wen/p/5808276.html ) -->

  <!-- 回答：https://blog.coderzh.com/2015/03/05/WhyHeartBeatNeeded/  -->

  <!-- http://blog.csdn.net/hengyunabc/article/details/44310193 -->

  <!-- [服务端主动发送心跳包，还是客户端发送比较好？](https://www.zhihu.com/question/35896874) -->

### 4.4. TCP KeepALive 与应用层心跳

**心跳包机制一般两种：TCP 自带的 keep-alive 和应用层的心跳包探活。**那到底使用 tcp 的 keep-alive 还是应用层自己实现的心跳机制呢？

- TCP KeepALive 机制占用带宽少，不用开发人员实现。但在有代理的网络下，不一定能探活到服务器，除非反向代理也有相应的心跳机制。
- 应用层心跳能携带更多状态，但需要开发人员自己实现。

## 5. 可靠性保证：重传 (Retransmit)
 
可靠性是指需要保证数据确实到达目的地；如果未到达，能够发现并重传。因此，TCP 要保证所有的数据包都可以到达，必需要有重传机制。

### 5.1. 计时器管理

TCP 使用多个计时器来辅助其它工作的完成，其中重传计时器 (RTO, Retranmission Timeout) 用于协助实现 TCP 重传机制。当 TCP 实体发出一个段后，它同时启动一个重传计时器，若在该计时器超时前该段被确认，则计时器停止；若该计时器超时后仍没有收到确认，则会启动针对该段的重传机制，同时重新启动该计时器。

那么存在一个关键问题是：Timeout 时间应该设置为多长？
- 设长了，重发就慢，没有效率，性能差。
- 设短了，会导致可能实际上没有丢包就重发，且会增加网络拥塞，导致更多的超时，更多的超时导致更多的重发。
- 不同的网络环境传输质量差异很大，因此不能将 Timeout 设置为固定的值。

因此，RTO (Retransmission TimeOut) 的设置需要一个动态算法，它可以根据网络性能的连续测量情况，不断地调整超时间隔。为此，TCP 引入了 RTT (Round Trip Time)，即一个数据包从发出去到回来的时间，在发送端发包时记下 t0，然后接收端再把这个 ack 回来时再记一个 t1，于是 RTT = t1 – t0。

#### 5.1.1. RFC 经典算法

在 [RFC 793](http://tools.ietf.org/html/rfc793) 中定义了设置 RTO 的经典算法：

1. 首先，先采样 RTT，记下最近几次的 RTT 值。

1. 然后做平滑计算求出 SRTT (Smoothed RTT)。公式为：（Exponential weighted moving average，指数加权移动平均）
    ```
    SRTT = (α * SRTT) + ((1- α) * RTT) (0.8 <= α <= 0.9)
    ```
1. 计算 RTO。公式为：
    ```
    RTO = min[UBOUND,  max[LBOUND, (β * SRTT)]] (1.3 <= β <= 2.0)
    ```
    其中：
    - UBOUND 是最大的 timeout 时间，上限值
    - LBOUND 是最小的 timeout 时间，下限值

这个算法在重传的时候会出现一个严重的问题：你是用第一次发数据的时间和 ACk 回来的时间做 RTT 样本值，还是用重传的时间和 ACK 回来的时间做 RTT 样本值？
- 情况（a）是 ack 没回来，所以重传。如果你计算第一次发送和 ACK 的时间，那么，明显算大了。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/77afb57b33741ab30554e1626e270066.jpg)

- 情况（b）是 ack 回来慢了，但是导致了重传，但刚重传不一会儿，之前 ACK 就回来了。如果你是算重传的时间和 ACK 回来的时间的差，就会算短了。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/5f9fda5c3546abed428558f5a8ae2f2a.jpg)

#### 5.1.2. Karn / Partridge 算法

针对 RFC 793 经典算法存在的问题，1987 年出现了 Karn / Partridge Algorithm，这个算法的最大特点是忽略重传，不把重传的 RTT 做采样。

只要一发生重传，就对现有的 RTO 值翻倍（Exponential backoff）。但很明显，这种死规矩对于一个需要估计比较准确的 RTT 也不靠谱。

#### 5.1.3. Jacobson / Karels 算法

使用 Exponential weighted moving average 计算 RTO 存在一个问题：如果 RTT 有一个大的波动的话，很难被发现，因为被平滑掉了。

针对这个问题，1988 年又出现了 Jacobson / Karels Algorithm，这个算法引入了最新的 RTT 的采样和平滑过的 SRTT 的差距做因子来计算。

Jacobson / Karels Algorithm 公式如下：（其中的 DevRTT 是 Deviation RTT 的意思）
```
计算 SRTT:
SRTT = SRTT + α (RTT – SRTT)
计算平滑 RTT 和真实的差距（加权移动平均）:
DevRTT = (1-β)*DevRTT + β*(|RTT-SRTT|)
神一样的公式：
RTO= µ * SRTT + ∂ *DevRTT
```
在 Linux 下，α = 0.125，β = 0.25， μ = 1，∂ = 4 。

这也是如今 TCP 协议中真正采用的 RTO 计算算法 (Linux 的源代码在：tcp_rtt_estimator)。

### 5.2. 重传机制

接收端给发送端的 ACK 确认只会确认最后一个连续的包，例如，发送端发了 1,2,3,4,5 一共五份数据，接收端收到了 1，2，于是回 ack 3，然后收到了 4（注意此时 3 没收到），此时的 TCP 会怎么办？

具体的重传机制有以下几种。

#### 5.2.1. 基本原则

接收端确认的基本原则：由于 **SeqNum 和 ACK 是以字节数为单位，所以 ACK 的时候，不能跳着确认，只能确认最大的连续收到的包**，不然，发送端就以为之前的都收到了。

#### 5.2.2. 超时重传机制 (Timeout Retransmit)

超时重传机制指的是接收端不回 ACK，一直等待 3 的到来。而发送方发出一个报文分组时，会启动一个计时器，若计时器计数完毕，ACK 还未到达，发送方就会重传 3。接收方收到 3 后，才会 ACK 4，意味着 3 和 4 都收到了。

但是，这种方式存在比较严重的问题：由于接收端一直等待 3 的到来而不回复 ACK，所以会导致 4 和 5 即便已经收到了，而发送方也完全不知道，发送方可能会悲观地认为 4 和 5 也丢了，会导致 4 和 5 的重传，浪费网络资源。

对于发送方，有两种选择：
- 仅重传 timeout 的包，即第 3 份数据。这种方式节省了带宽，但是处理速度较慢。
- 重传 timeout 后所有的数据，即第 3，4，5 这三份数据。这种方式处理速度较快，但是浪费带宽，降低网络利用率。
总体来说都不好，因为都在等 timeout，而 timeout 可能会很长。

#### 5.2.3. 快速重传机制 (Fast Retransmit)

针对超市重传机制的需要持续等待 timeout 的问题，TCP 引入了 Fast Retransmit，不以时间驱动，而以数据驱动重传。在快速重传机制中，**如果包没有连续到达，就只 ACK 最后那个可能被丢了的包，如果发送方连续收到 3 次相同的 ACK，就马上重传**，避免了 timeout 的等待时间。

- 例：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/c9bcc65d5532ed212e7a832960b4dca2.jpg)

  发送方发出了 1，2，3，4，5 份数据，第一份先到送了，于是接收方 ACK 2，结果 2 因为某些原因没收到，3 到达了，于是还是 ACK 2，后面的 4 和 5 都到了，但是还是 ACK 2，因为 2 还是没有收到。于是发送端连续收到了三个 ACKnum = 2 的 ACK 包，知道了 2 已丢失，就马上重传 2。然后，接收端收到了 2，此时因为 3，4，5 都收到了，于是 直接 ACK 6。

Fast Retransmit 解决了 timeout 的问题，但它和 Timeout Retransmit 一样，面临是重传之前的一个分段还是重传所有的问题。对于上面的示例来说，是重传#2 呢还是重传#2，#3，#4，#5 呢？也许发送端发了 20 份数据，是#6，#10，#20 传来的呢。这样，发送端很有可能要重传从 2 到 20 的这堆数据（这就是某些 TCP 的实际的实现）。可见，这是一把双刃剑。

#### 5.2.4. 选择确认重传 (Selective Acknowledgment Retransmit)

Selective Acknowledgment (SACK) 在 [RFC 2018](http://tools.ietf.org/html/rfc2018) 中定义，是针对 Fast Retransmit 的改进。这种重传方式需要在 TCP Header 中设置 SACK 的字段，用于存放接收端已收到的分段序列号。

- 例：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/38841d17cf1c5afced8395078b90b1a4.jpg)

由于接收端永远不能把 SACK 的包标记为 ACK，在发送端就可以根据回传的 SACK 来知道哪些数据到了，哪些没有到。于是就优化了 Fast Retransmit 的算法。当然，这个协议需要两边都支持。在 Linux 下，可以通过 tcp_sack 参数打开这个功能（Linux 2.4 后默认打开）。

但这种方式存在安全问题：SACK 会消费发送方的资源，如果一个攻击者给数据发送方发一堆 SACK 的选项，这会导致发送方开始要重传甚至遍历已经发出的数据，这会消耗很多发送端的资源。

#### 5.2.5. 重复选择确认重传 (Duplicate Selective Acknowledgment Retransmit)

Duplicate SACK 又称 D-SACK，在 [RFC 2883](http://www.ietf.org/rfc/rfc2883.txt) 中定义，其主要使用了 SACK 来告诉发送方有哪些数据被重复接收了。

D-SACK 使用了 SACK 的第一个段来做标志：
- 如果 SACK 的第一个段的范围被 ACK 所覆盖，那么就是 D-SACK。
- 如果 SACK 的第一个段的范围被 SACK 的第二个段覆盖，那么就是 D-SACK。

D-SACK 优势：
- 可以让发送方知道，是发出去的包丢了，还是回来的 ACK 包丢了。
- 是不是自己的 timeout 太小了，导致重传。
- 网络上出现了先发的包后到的情况（又称 reordering）。
- 网络上是不是把我的数据包给复制了。

## 6. 流量控制：滑动窗口协议

流量控制指的是防止一个发送能力较强的发送端的数据将一个接收能力较差的接收端“淹没”的控制措施。

TCP 流量控制是通过一个可变大小的滑动窗口 (Sliding Window) 来实现的，除此之外，TCP 协议本身的操作都是围绕滑动窗口协议来进行的，因此：
> 如果你不了解 TCP 的滑动窗口这个事，你等于不了解 TCP 协议。

### 6.1. 相关字段

TCP 段中窗口的相关字段：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/c003e446f4caaff25b0a564033662896.jpg)

TCP 的 Window 是一个 16bit 位字段，它代表的是窗口的字节容量，也就是 TCP 的标准窗口最大为 2^16-1=65535 个字节。这个字段是接收端告诉发送端自己还有多少缓冲区可以接收数据。于是发送端就可以根据这个接收端的处理能力来发送数据，而不会导致接收端处理不过来。

另外在 TCP 的选项字段中还包含了一个 TCP 窗口扩大因子，option-kind 为 3，option-length 为 3 个字节，option-data 取值范围 0-14。窗口扩大因子用来扩大 TCP 窗口，可把原来 16bit 的窗口，扩大为 31bit。

### 6.2. 基本原理

TCP 的滑动窗口分为接收窗口和发送窗口：
- 发送窗口

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/fce737911a5228e54a0cd401e1fd98d1.jpg)

  对于 TCP 会话的发送方，任何时候在其发送缓存内的数据都可以分为 4 类：
  - 已经发送并得到对端 ACK 的：数据流中最早的字节已经发送并得到确认。
  - 已经发送但还未收到对端 ACK 的：发送方在确认之前，不认为这些数据已经被处理。
  - 未发送但对端允许发送的：设备尚未将数据发出，但接收方根据最近一次关于发送方一次要发送多少字节确认自己有足够空间，发送方会立即尝试发送。
  - 未发送且对端不允许发送：由于接收方 not ready，还不允许将这部分数据发出。
  其中，“已经发送但还未收到对端 ACK 的”和“未发送但对端允许发送的”这两部分数据结合起来就称为发送窗口。

  当收到接收方新的 ACK（对于发送窗口中后续字节的确认）时，窗口向后至 ACKnum。例：收到 ACKnum=36 时：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/fbf0312af887af483a60b290c347b733.jpg)

  发送窗口的大小取决于 TCP 连接对方通告的接收窗口大小，要求两者保持相同。

- 接收窗口

  对于 TCP 的接收方，在某一时刻在它的接收缓存内存在 3 种：
  - 已接收
  - 未接收准备接收
  - 未接收并未准备接收
  - （由于 ACK 直接由 TCP 协议栈回复，默认无应用延迟，不存在“已接收未回复 ACK”）
  其中，“未接收准备接收”部分就称为接收窗口，用于向对端通知本地希望一次接收多少字节的数据。

  接收窗口大小取决于应用、系统、硬件的限制（TCP 传输速率不能大于应用的数据处理速率）。

- 关系

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/eb21c6b4a08419d2f193f88fa8828766.jpg)

  TCP 是双工的协议，会话的双方都可以同时接收、发送数据。因此，TCP 会话的双方都各自维护一个发送窗口和一个接收窗口。

### 6.3. 作用

#### 6.3.1. 提供面向流的可靠性
 
  - TCP 最基本的传输可靠性来源于“确认重传”机制，事实上 TCP 的滑动窗口的可靠性也是建立在“确认重传”基础上的。
  
  - 发送窗口**只有收到对端对于本段发送窗口内字节的 ACK 确认，才会移动发送窗口的左边界。**
  
  - 接收窗口只有在前面所有的段都确认的情况下才会移动左边界。**假设在前面还有字节未接收但收到后面字节的情况下，窗口不会移动，并不对后续字节确认，以此确保对方在未接收到 ACK 导致计时器超时后会对这些数据重传。**

    但存在一个缺陷是：因为它不会对每一个片段分别进行确认，这可能会导致其他实际上已经接收到的片段被重传。

#### 6.3.2. 提供流量控制
  
  在于基本的滑动窗口机制中，数据于接收时确认，但并不一定立即从缓存中传输出去。也就意味着当接收数据速度快于接收 TCP 处理速度时，缓存有可能被填满。因此，当这一情况发生时，接收设备需要调整窗口大小已防止缓存过载。

  窗口大小能够以这种方式管理连接两端设备数据流的速率，实际上 TCP 就是通过这种方式实现传输层流量控制任务的，即通过增加或缩小窗口大小，服务器和客户端能够确保对端发送数据的速度等同于处理速度。
  
  应用程序在需要（如内存不足）时，通过 API 通知 TCP 协议栈缩小 TCP 的接收窗口。然后 TCP 协议栈在下个段发送时包含新的窗口大小通知给对端，对端按通知的窗口来改变发送窗口，以此达到减缓发送速率的目的。

#### 6.3.3. 解决延迟确认问题

TCP 是面向数据流的协议，它将独立的字节数据当作流来处理。但实际上，一次发送一个字节并接收一次确认显然是不可行的，即使重叠传输（即不等待确认就发送下一个数据），速度也还是会非常缓慢。因此，为提高速度，TCP 实现采用了延迟确认的机制，即将确认和窗口更新延迟 50 毫秒，以获得一些数据免费搭载过去。

尽管通过延迟确认减少了接收端对网络的负载，但发送端总发送多个小数据包的方式仍非常低效。在这种模式下，可能会出现 Silly Window Syndrome （低能窗口综合症）。即如果接收方来不及取走 Receive Windows 里的数据，那么，就会导致发送方越来越小。到最后，如果接收方腾出几个字节并告诉发送方现在有几个字节的 window，而我们的发送方会义无反顾地发送这几个字节。

- Nagle 算法

  Nagle’s algorithm 是针对 sender 端的解决方法。它的思路非常简单：当数据每次以很少量的方式进入发送端时，发送端只是发送第一次到达的数据，然后将其它的字节缓冲起来，直到发送出去的那个数据包被确认，才将所有缓冲的字节放在一个 TCP 段中发送出去，并继续缓冲后续到达的字节，直到下一个段被确认。

  Nagle 算法默认是打开的，所以，对于一些需要小包场景的程序，如 telnet 或 ssh 这样的交互性比较强的程序，需要手动关闭这个算法。可以在 Socket 设置 `TCP_NODELAY` 选项来关闭这个算法。

  P.S. 参数 `TCP_CORK` 是更激进的 Nagle 算法，它完全禁止了小包发送，而参数 `TCP_NODELAY` 指定的 Nagle 算法没有禁止小包发送，只是禁止了大量的小包发送。

- Clark 方案

  David D Clark’s 方案是针对 receiver 端的解决方法。它的思路是禁止 receiver 端发送只有 1 个字节的窗口更新段，强制 receiver 端必须等待一段时间，直到有了一定数量的可用空间后再通告 sender 端。

Nagle 算法试图解决由于发送端应用每次向 TCP 传递一个字节而引起的问题，Clark 方案则试图解决由于接收端应用每次从 TCP 流种读取一个字节而引起的问题。这两种方案各自有效、相互补充，发送端的目标是不发送太小的数据段，而接收端也不要请求太小的段。

## 7. 拥塞控制 (Congestion Handling)

当提供给任何网络的负载超过它的负载能力时，拥塞便会产生，传输层接收到从网络层反馈来的拥塞信息后，应减慢它发送到网络层中的流量速率。拥塞控制 (Congestion Handling) 是传输层需要解决的问题之一，也是 TCP 的一个关键功能。

当网络上的延时突然增加时，如果 TCP 对这个事做出的应对只有重传数据，这将会导致网络的负担更重，于是会导致更大的延迟以及更多的丢包。如果一个网络内有成千上万的 TCP 连接都这么行事，那么马上就会形成“网络风暴”，TCP 这个协议就会拖垮整个网络。因此，[TCP 关于拥塞控制的设计理念](http://ee.lbl.gov/papers/congavoid.pdf) 是：**TCP 不是一个自私的协议，当拥塞发生的时候，要做自我牺牲。就像交通阻塞一样，每个车都应该把路让出来，而不要再去抢路了**。

TCP 拥塞控制主要是四个算法：
- 1988 年，TCP-Tahoe 提出了慢启动算法、拥塞避免算法、拥塞发生时的快速重传算法。
- 1990 年，TCP Reno 在 Tahoe 的基础上增加了快速恢复算法。

这些算法经过了很长时间的发展，到如今都还在优化中。

### 7.1. 慢启动算法 (Slow Start)

慢启动指的是刚刚加入网络的连接，一点一点地提速，不要一上来就像那些特权车一样霸道地把路占满。新同学上高速还是要慢一点，不要把已经在高速上的秩序给搞乱了。

1. 连接建立后先初始化 cwnd = 1 (cwnd 即 Congestion Window)，表明可以传一个 MSS 大小的数据。

1. 每当收到一个 ACK，cwnd++，呈线性上升。

1. 每当过了一个 RTT，cwnd = cwnd*2，呈指数让升。

1. 当 cwnd >= ssthresh (slow start threshold 即慢启动阈值) 时，就会进入“拥塞避免算法”。一般来说 ssthresh 的值是 65535 bytes。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/c13c8eec98d68c48aab0c6acfc0824c5.jpg)

因此，如果实际网速很快的话，ACK 也会返回得快，RTT 也会短，那么这个慢启动就一点也不慢。

P.S. Linux 3.0 后采用了这篇论文的建议——把 cwnd 初始化成了 10 个 MSS。 而 Linux 3.0 以前，比如 2.6，Linux 采用了 RFC3390，cwnd 是跟 MSS 的值来变的，如果 MSS< 1095，则 cwnd = 4；如果 MSS>2190，则 cwnd=2；其它情况下，则是 3。

### 7.2. 拥塞避免算法 (Congestion Avoidance)

当 cwnd >= ssthresh 时，就会进入“拥塞避免算法”，算法如下：
- 收到一个 ACK 时，cwnd = cwnd + 1/cwnd
- 当每过一个 RTT 时，cwnd = cwnd + 1

这样就可以避免增长过快导致网络拥塞，慢慢的增加调整到网络的最佳值。很明显，是一个线性上升的算法。

### 7.3. 拥塞发生算法

若拥塞已经在网络中发生，数据包开始丢失，TCP 通过动态调整慢启动阈值来控制拥塞。存在 2 种情况：
- 等到 RTO 超时，重传数据包。TCP 认为这种情况太糟糕，因此做出的反应也很强烈：
  - sshthresh =  cwnd /2
  - cwnd 重置为 1
  - 进入慢启动过程
- Fast Retransmit 算法，也就是在收到 3 个 duplicate ACK 时就开启重传，而不用等到 RTO 超时。
  - cwnd = cwnd /2
  - sshthresh = cwnd
  - 进入快速恢复算法 Fast Recovery

可以看到 RTO 超时后，sshthresh 会变成 cwnd 的一半，这意味着，如果 cwnd<=sshthresh 时出现的丢包，那么 TCP 的 sshthresh 就会减了一半，然后等 cwnd 又很快地以指数级增涨爬到这个地方时，就会成慢慢的线性增涨。TCP 就是通过这种强烈地震荡快速而小心得找到网站流量的平衡点。

### 7.4. 快速恢复算法 (Fast Recovery)

快速恢复算法 (Fast Recovery) 是一种启发式机制，定义于 [RFC 5681](http://tools.ietf.org/html/rfc5681)，一般和快速重传同时使用。快速恢复算法认为，若还能收到 3 个 Duplicated Acks，说明网络也不那么糟糕，所以没有必要使用像 RTO 超时那么强烈。Fast Recovery 算法如下：

1. 进入 Fast Recovery 之前，cwnd 和 sshthresh 已被更新。
1. cwnd = sshthresh  + 3 * MSS （确认有 3 个数据包被收到了）
1. 重传 Duplicated ACKs 指定的数据包。
1. 如果再收到 duplicated Acks，那么 cwnd = cwnd +1。
1. 如果收到了新的 ACK，那么 cwnd = sshthresh，就进入了拥塞避免的算法。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/7c7ed038edf247fd39029d51b1041f16.jpg)

## 8. 其它问题

### 8.1. Linux 下单机 TCP 连接的最大并发量

[Linux 中，一个端口能够接受 TCP 连接数的理论上限是？](https://www.nowcoder.com/questionTerminal/997f45dddc9b4c8ea7da76288aa439d1)

[单机最大 tcp 连接数](http://wanshi.iteye.com/blog/1256282)

[单台服务器上的并发 TCP 连接数可以有多少？](http://blog.51cto.com/yaocoder/1312821)

[The C10K problem](http://www.kegel.com/c10k.html)

#### 8.1.1. 理论最大并发量

在 TCP 应用中，Server 事先在某个固定端口监听，Client 主动发起连接，经过三路握手后建立 TCP 连接。系统用一个 4 四元组来唯一标识一个 TCP 连接：`{local ip, local port,remote ip,remote port}`。

- Client 端最大 TCP 并发量

  Client 每次发起 TCP 连接请求创建 Socket 时，除非绑定指定的端口，否则系统会自动选取一个空闲的本地端口（local port）。该端口是独占的，不能和其它 TCP 连接共享，因此，Client 端的 TCP 连接数取决于可用的端口数量。

  TCP 端口的数据类型是 unsigned short，因此本地端口个数最大为 65536，端口 0 有特殊含义不能使用，即可用的端口最多为 65535。所以在全部作为 TCP Client 端的情况下，一个 client 最大 TCP 连接数为 65535，这些连接可以连到不同的 Server ip。

- Server 端最大 TCP 并发量

  虽然现在的集群，分布式技术可以为我们将并发负载分担在多台服务器上，那我们只需要扩展出数十台电脑就可以解决问题，但是我们更希望能更大的挖掘单台服务器的资源，先努力垂直扩展，再进行水平扩展，这样可以有效的节省服务器相关的开支（硬件资源，机房，运维，电力其实也是一笔不小的开支）。那么到底一台服务器能够支持多少 TCP 并发连接呢？

  Server 通常固定在某个本地端口上监听，等待 Client 的连接请求。不考虑地址重用（unix 的 SO_REUSEADDR 选项）的情况下，即使 Server 端有多个 ip，本地监听端口也是独占的，因此 Server 端的 TCP 连接 4 元组中，只有 remote ip (Client ip) 和 remote port (Client port) 是可变的。也就是说，最大 TCP 连接并发量为 `number of Client ip * number of Client port`。

  对于 IPv4，不考虑 ip 地址分类等因素，最大 TCP 连接数为 `2^32 (number of Client ip) * 2^16 (number of Client port)`，也就是 Server 端单机最大 TCP 连接数约为 `2^48`。

#### 8.1.2. 实际最大并发量

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
    
    可通过修改 /etc/sysctl.conf 文件来增大最大文件句柄数：
    ```
    fs.file-max = 1000000
    net.ipv4.ip_conntrack_max = 1000000
    net.ipv4.netfilter.ip_conntrack_max = 1000000
    ```

- 内存限制

  每个进程 / 线程都需要占用一定的内存。

- 带宽限制

### 8.2. TCP 拆包粘包问题

https://www.zhihu.com/question/37023914

https://blog.insanecoder.top/tcp-packet-splice-and-split-issue/

#### 8.2.1. 问题描述

- UDP 是基于报文发送的，从 UDP 的帧结构可以看出，在 UDP 首部采用了 16bit 来指示 UDP 数据报文的长度，因此在应用层能很好的将不同的数据报文区分开，从而避免粘包和拆包的问题。
- TCP 是基于字节流的，虽然应用层和 TCP 传输层之间的数据交互是大小不等的数据块，但是 TCP 把这些数据块仅仅看成一连串无结构的字节流，没有边界。另外从 TCP 的帧结构也可以看出，在 TCP 的首部没有表示数据长度的字段，基于上面两点，在使用 TCP 传输数据时，才有粘包或者拆包现象发生的可能。

#### 8.2.2. 解决方法

粘包与拆包是由于 TCP 协议是字节流协议，没有记录边界所导致的，所以如何确定一个完整的业务包就由应用层来处理了。
（这就是分包机制，本质上就是要在应用层维护消息与消息的边界）
分包机制一般有两个通用的解决方法：
- 特殊字符控制，例如 FTP 协议。
- 在包头首都添加数据包的长度，例如 HTTP 协议。

### 8.3. TCP 半开连接问题

[TCP 半开连接问题](http://blog.51cto.com/yaocoder/1309358)

#### 8.3.1. 问题描述

当客户端与服务器建立起正常的 TCP 连接后，如果客户主机掉线（网线断开、电源掉电、或系统崩溃等），服务端进程将永远不会知道（通过常用的 select/epoll 都监测不到断开或错误事件），如果不主动处理或重启系统的话对于服务端来说会一直维持着这个连接，这种情况就是半开连接，浪费了服务器端可用的文件描述符。

#### 8.3.2. 处理方案

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

### 8.4. TCP 与 UDP 的区别

[网络编程释疑之：TCP 协议的“流”特性](http://blog.51cto.com/yaocoder/1339958)

- TCP 是面向连接的，UDP 是无连接的。

- TCP 是面向数据流的协议，UDP 是面向数据报的协议。

  TCP 的分包 / 粘包问题也就是 TCP 协议的“流”特性造成的。

  假设客户端一次完全发送这么一串字符 str = "hello world!"到服务端，在服务端一次 read，并且 read 长度的参数大于 strlen(str) 的情况下，用 TCP 和 UDP 协议会有什么区别？

  在网络没有出问题的情况下，根据系统内核的套接字缓冲区是否充足：
  - 用 UDP 协议发送的话，在服务端要么是什么也收不到；要么是全部收到了"hello world!"这个字符串。
  - 用 TCP 协议发送的话，有可能一次 read 只是得到了"hello world!"的部分字符，经过多次 read 累积缓冲区才能收到整个字符串；有可能一次全部收到。

  举一个例子类比一下，我们要把一个空碗接满水，我们可以一次倒入也可以分多次倒入。但是我们要把一个馒头完整的放进另一个碗中，你的选择只有放一次，要么放得下要么放不下。

- TCP 是可靠的，UDP 是不可靠的。

<!-- - todo: -->

## 9. Refer Links

[TCP 协议：笔试面试知识点整理](https://hit-alibaba.github.io/interview/basic/network/TCP.html)

[TCP 为什么是三次握手，而不是两次或四次？](https://www.zhihu.com/question/24853633)

[TCP 的那些事儿（上）](https://coolshell.cn/articles/11564.html)

[TCP 的那些事儿（下）](https://coolshell.cn/articles/11609.html)

[TCP 协议的滑动窗口具体是怎样控制流量的？](https://www.zhihu.com/question/32255109)

[网络基本功（八）：细说 TCP 滑动窗口](https://wizardforcel.gitbooks.io/network-basic/content/7.html)

[网络基本功（九）：细说 TCP 重传](https://wizardforcel.gitbooks.io/network-basic/content/8.html)

[网络基本功（十）：细说 TCP 确认机制](https://wizardforcel.gitbooks.io/network-basic/content/9.html)

[网络基本功（十一）：TCP 窗口调整与流控](https://wizardforcel.gitbooks.io/network-basic/content/10.html)

[阮一峰：TCP 协议简介](http://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html)