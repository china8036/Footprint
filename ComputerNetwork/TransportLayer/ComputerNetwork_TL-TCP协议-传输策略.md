- [计算机网络 - 传输层：Transport control protocol 传输策略](#计算机网络---传输层transport-control-protocol-传输策略)
  - [1. 可靠性保证：重传 (Retransmit)](#1-可靠性保证重传-retransmit)
    - [1.1. 计时器管理](#11-计时器管理)
      - [1.1.1. RFC 经典算法](#111-rfc-经典算法)
      - [1.1.2. Karn / Partridge 算法](#112-karn--partridge-算法)
      - [1.1.3. Jacobson / Karels 算法](#113-jacobson--karels-算法)
    - [1.2. 重传机制](#12-重传机制)
      - [1.2.1. 基本原则](#121-基本原则)
      - [1.2.2. 超时重传机制 (Timeout Retransmit)](#122-超时重传机制-timeout-retransmit)
      - [1.2.3. 快速重传机制 (Fast Retransmit)](#123-快速重传机制-fast-retransmit)
      - [1.2.4. 选择确认重传 (Selective Acknowledgment Retransmit)](#124-选择确认重传-selective-acknowledgment-retransmit)
      - [1.2.5. 重复选择确认重传 (Duplicate Selective Acknowledgment Retransmit)](#125-重复选择确认重传-duplicate-selective-acknowledgment-retransmit)
  - [2. 流量控制：滑动窗口协议](#2-流量控制滑动窗口协议)
    - [2.1. 相关字段](#21-相关字段)
    - [2.2. 基本原理](#22-基本原理)
    - [2.3. 作用](#23-作用)
      - [2.3.1. 提供面向流的可靠性](#231-提供面向流的可靠性)
      - [2.3.2. 提供流量控制](#232-提供流量控制)
      - [2.3.3. 解决延迟确认问题](#233-解决延迟确认问题)
  - [3. 拥塞控制 (Congestion Handling)](#3-拥塞控制-congestion-handling)
    - [3.1. 慢启动算法 (Slow Start)](#31-慢启动算法-slow-start)
    - [3.2. 拥塞避免算法 (Congestion Avoidance)](#32-拥塞避免算法-congestion-avoidance)
    - [3.3. 拥塞发生算法](#33-拥塞发生算法)
    - [3.4. 快速恢复算法 (Fast Recovery)](#34-快速恢复算法-fast-recovery)
  - [4. Refer Links](#4-refer-links)

# 计算机网络 - 传输层：Transport control protocol 传输策略

## 1. 可靠性保证：重传 (Retransmit)
 
可靠性是指需要保证数据确实到达目的地；如果未到达，能够发现并重传。因此，TCP 要保证所有的数据包都可以到达，必需要有重传机制。

### 1.1. 计时器管理

TCP 使用多个计时器来辅助其它工作的完成，其中重传计时器 (RTO, Retranmission Timeout) 用于协助实现 TCP 重传机制。当 TCP 实体发出一个段后，它同时启动一个重传计时器，若在该计时器超时前该段被确认，则计时器停止；若该计时器超时后仍没有收到确认，则会启动针对该段的重传机制，同时重新启动该计时器。

那么存在一个关键问题是：Timeout 时间应该设置为多长？
- 设长了，重发就慢，没有效率，性能差。
- 设短了，会导致可能实际上没有丢包就重发，且会增加网络拥塞，导致更多的超时，更多的超时导致更多的重发。
- 不同的网络环境传输质量差异很大，因此不能将 Timeout 设置为固定的值。

因此，RTO (Retransmission TimeOut) 的设置需要一个动态算法，它可以根据网络性能的连续测量情况，不断地调整超时间隔。为此，TCP 引入了 RTT (Round Trip Time)，即一个数据包从发出去到回来的时间，在发送端发包时记下 t0，然后接收端再把这个 ack 回来时再记一个 t1，于是 RTT = t1 – t0。

#### 1.1.1. RFC 经典算法

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

#### 1.1.2. Karn / Partridge 算法

针对 RFC 793 经典算法存在的问题，1987 年出现了 Karn / Partridge Algorithm，这个算法的最大特点是忽略重传，不把重传的 RTT 做采样。

只要一发生重传，就对现有的 RTO 值翻倍（Exponential backoff）。但很明显，这种死规矩对于一个需要估计比较准确的 RTT 也不靠谱。

#### 1.1.3. Jacobson / Karels 算法

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

### 1.2. 重传机制

接收端给发送端的 ACK 确认只会确认最后一个连续的包，例如，发送端发了 1,2,3,4,5 一共五份数据，接收端收到了 1，2，于是回 ack 3，然后收到了 4（注意此时 3 没收到），此时的 TCP 会怎么办？

具体的重传机制有以下几种。

#### 1.2.1. 基本原则

接收端确认的基本原则：由于 **SeqNum 和 ACK 是以字节数为单位，所以 ACK 的时候，不能跳着确认，只能确认最大的连续收到的包**，不然，发送端就以为之前的都收到了。

#### 1.2.2. 超时重传机制 (Timeout Retransmit)

超时重传机制指的是接收端不回 ACK，一直等待 3 的到来。而发送方发出一个报文分组时，会启动一个计时器，若计时器计数完毕，ACK 还未到达，发送方就会重传 3。接收方收到 3 后，才会 ACK 4，意味着 3 和 4 都收到了。

但是，这种方式存在比较严重的问题：由于接收端一直等待 3 的到来而不回复 ACK，所以会导致 4 和 5 即便已经收到了，而发送方也完全不知道，发送方可能会悲观地认为 4 和 5 也丢了，会导致 4 和 5 的重传，浪费网络资源。

对于发送方，有两种选择：
- 仅重传 timeout 的包，即第 3 份数据。这种方式节省了带宽，但是处理速度较慢。
- 重传 timeout 后所有的数据，即第 3，4，5 这三份数据。这种方式处理速度较快，但是浪费带宽，降低网络利用率。
总体来说都不好，因为都在等 timeout，而 timeout 可能会很长。

#### 1.2.3. 快速重传机制 (Fast Retransmit)

针对超市重传机制的需要持续等待 timeout 的问题，TCP 引入了 Fast Retransmit，不以时间驱动，而以数据驱动重传。在快速重传机制中，**如果包没有连续到达，就只 ACK 最后那个可能被丢了的包，如果发送方连续收到 3 次相同的 ACK，就马上重传**，避免了 timeout 的等待时间。

- 例：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/c9bcc65d5532ed212e7a832960b4dca2.jpg)

  发送方发出了 1，2，3，4，5 份数据，第一份先到送了，于是接收方 ACK 2，结果 2 因为某些原因没收到，3 到达了，于是还是 ACK 2，后面的 4 和 5 都到了，但是还是 ACK 2，因为 2 还是没有收到。于是发送端连续收到了三个 ACKnum = 2 的 ACK 包，知道了 2 已丢失，就马上重传 2。然后，接收端收到了 2，此时因为 3，4，5 都收到了，于是 直接 ACK 6。

Fast Retransmit 解决了 timeout 的问题，但它和 Timeout Retransmit 一样，面临是重传之前的一个分段还是重传所有的问题。对于上面的示例来说，是重传#2 呢还是重传#2，#3，#4，#5 呢？也许发送端发了 20 份数据，是#6，#10，#20 传来的呢。这样，发送端很有可能要重传从 2 到 20 的这堆数据（这就是某些 TCP 的实际的实现）。可见，这是一把双刃剑。

#### 1.2.4. 选择确认重传 (Selective Acknowledgment Retransmit)

Selective Acknowledgment (SACK) 在 [RFC 2018](http://tools.ietf.org/html/rfc2018) 中定义，是针对 Fast Retransmit 的改进。这种重传方式需要在 TCP Header 中设置 SACK 的字段，用于存放接收端已收到的分段序列号。

- 例：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/38841d17cf1c5afced8395078b90b1a4.jpg)

由于接收端永远不能把 SACK 的包标记为 ACK，在发送端就可以根据回传的 SACK 来知道哪些数据到了，哪些没有到。于是就优化了 Fast Retransmit 的算法。当然，这个协议需要两边都支持。在 Linux 下，可以通过 tcp_sack 参数打开这个功能（Linux 2.4 后默认打开）。

但这种方式存在安全问题：SACK 会消费发送方的资源，如果一个攻击者给数据发送方发一堆 SACK 的选项，这会导致发送方开始要重传甚至遍历已经发出的数据，这会消耗很多发送端的资源。

#### 1.2.5. 重复选择确认重传 (Duplicate Selective Acknowledgment Retransmit)

Duplicate SACK 又称 D-SACK，在 [RFC 2883](http://www.ietf.org/rfc/rfc2883.txt) 中定义，其主要使用了 SACK 来告诉发送方有哪些数据被重复接收了。

D-SACK 使用了 SACK 的第一个段来做标志：
- 如果 SACK 的第一个段的范围被 ACK 所覆盖，那么就是 D-SACK。
- 如果 SACK 的第一个段的范围被 SACK 的第二个段覆盖，那么就是 D-SACK。

D-SACK 优势：
- 可以让发送方知道，是发出去的包丢了，还是回来的 ACK 包丢了。
- 是不是自己的 timeout 太小了，导致重传。
- 网络上出现了先发的包后到的情况（又称 reordering）。
- 网络上是不是把我的数据包给复制了。

## 2. 流量控制：滑动窗口协议

流量控制指的是防止一个发送能力较强的发送端的数据将一个接收能力较差的接收端“淹没”的控制措施。

TCP 流量控制是通过一个可变大小的滑动窗口 (Sliding Window) 来实现的，除此之外，TCP 协议本身的操作都是围绕滑动窗口协议来进行的，因此：
> 如果你不了解 TCP 的滑动窗口这个事，你等于不了解 TCP 协议。

### 2.1. 相关字段

TCP 段中窗口的相关字段：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/c003e446f4caaff25b0a564033662896.jpg)

TCP 的 Window 是一个 16bit 位字段，它代表的是窗口的字节容量，也就是 TCP 的标准窗口最大为 2^16-1=65535 个字节。这个字段是接收端告诉发送端自己还有多少缓冲区可以接收数据。于是发送端就可以根据这个接收端的处理能力来发送数据，而不会导致接收端处理不过来。

另外在 TCP 的选项字段中还包含了一个 TCP 窗口扩大因子，option-kind 为 3，option-length 为 3 个字节，option-data 取值范围 0-14。窗口扩大因子用来扩大 TCP 窗口，可把原来 16bit 的窗口，扩大为 31bit。

### 2.2. 基本原理

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

### 2.3. 作用

#### 2.3.1. 提供面向流的可靠性
 
  - TCP 最基本的传输可靠性来源于“确认重传”机制，事实上 TCP 的滑动窗口的可靠性也是建立在“确认重传”基础上的。
  
  - 发送窗口**只有收到对端对于本段发送窗口内字节的 ACK 确认，才会移动发送窗口的左边界。**
  
  - 接收窗口只有在前面所有的段都确认的情况下才会移动左边界。**假设在前面还有字节未接收但收到后面字节的情况下，窗口不会移动，并不对后续字节确认，以此确保对方在未接收到 ACK 导致计时器超时后会对这些数据重传。**

    但存在一个缺陷是：因为它不会对每一个片段分别进行确认，这可能会导致其他实际上已经接收到的片段被重传。

#### 2.3.2. 提供流量控制
  
  在于基本的滑动窗口机制中，数据于接收时确认，但并不一定立即从缓存中传输出去。也就意味着当接收数据速度快于接收 TCP 处理速度时，缓存有可能被填满。因此，当这一情况发生时，接收设备需要调整窗口大小已防止缓存过载。

  窗口大小能够以这种方式管理连接两端设备数据流的速率，实际上 TCP 就是通过这种方式实现传输层流量控制任务的，即通过增加或缩小窗口大小，服务器和客户端能够确保对端发送数据的速度等同于处理速度。
  
  应用程序在需要（如内存不足）时，通过 API 通知 TCP 协议栈缩小 TCP 的接收窗口。然后 TCP 协议栈在下个段发送时包含新的窗口大小通知给对端，对端按通知的窗口来改变发送窗口，以此达到减缓发送速率的目的。

#### 2.3.3. 解决延迟确认问题

TCP 是面向数据流的协议，它将独立的字节数据当作流来处理。但实际上，一次发送一个字节并接收一次确认显然是不可行的，即使重叠传输（即不等待确认就发送下一个数据），速度也还是会非常缓慢。因此，为提高速度，TCP 实现采用了延迟确认的机制，即将确认和窗口更新延迟 50 毫秒，以获得一些数据免费搭载过去。

尽管通过延迟确认减少了接收端对网络的负载，但发送端总发送多个小数据包的方式仍非常低效。在这种模式下，可能会出现 Silly Window Syndrome （低能窗口综合症）。即如果接收方来不及取走 Receive Windows 里的数据，那么，就会导致发送方越来越小。到最后，如果接收方腾出几个字节并告诉发送方现在有几个字节的 window，而我们的发送方会义无反顾地发送这几个字节。

- Nagle 算法

  Nagle’s algorithm 是针对 sender 端的解决方法。它的思路非常简单：当数据每次以很少量的方式进入发送端时，发送端只是发送第一次到达的数据，然后将其它的字节缓冲起来，直到发送出去的那个数据包被确认，才将所有缓冲的字节放在一个 TCP 段中发送出去，并继续缓冲后续到达的字节，直到下一个段被确认。

  Nagle 算法默认是打开的，所以，对于一些需要小包场景的程序，如 telnet 或 ssh 这样的交互性比较强的程序，需要手动关闭这个算法。可以在 Socket 设置 `TCP_NODELAY` 选项来关闭这个算法。

  P.S. 参数 `TCP_CORK` 是更激进的 Nagle 算法，它完全禁止了小包发送，而参数 `TCP_NODELAY` 指定的 Nagle 算法没有禁止小包发送，只是禁止了大量的小包发送。

- Clark 方案

  David D Clark’s 方案是针对 receiver 端的解决方法。它的思路是禁止 receiver 端发送只有 1 个字节的窗口更新段，强制 receiver 端必须等待一段时间，直到有了一定数量的可用空间后再通告 sender 端。

Nagle 算法试图解决由于发送端应用每次向 TCP 传递一个字节而引起的问题，Clark 方案则试图解决由于接收端应用每次从 TCP 流种读取一个字节而引起的问题。这两种方案各自有效、相互补充，发送端的目标是不发送太小的数据段，而接收端也不要请求太小的段。

## 3. 拥塞控制 (Congestion Handling)

当提供给任何网络的负载超过它的负载能力时，拥塞便会产生，传输层接收到从网络层反馈来的拥塞信息后，应减慢它发送到网络层中的流量速率。拥塞控制 (Congestion Handling) 是传输层需要解决的问题之一，也是 TCP 的一个关键功能。

当网络上的延时突然增加时，如果 TCP 对这个事做出的应对只有重传数据，这将会导致网络的负担更重，于是会导致更大的延迟以及更多的丢包。如果一个网络内有成千上万的 TCP 连接都这么行事，那么马上就会形成“网络风暴”，TCP 这个协议就会拖垮整个网络。因此，[TCP 关于拥塞控制的设计理念](http://ee.lbl.gov/papers/congavoid.pdf) 是：**TCP 不是一个自私的协议，当拥塞发生的时候，要做自我牺牲。就像交通阻塞一样，每个车都应该把路让出来，而不要再去抢路了**。

TCP 拥塞控制主要是四个算法：
- 1988 年，TCP-Tahoe 提出了慢启动算法、拥塞避免算法、拥塞发生时的快速重传算法。
- 1990 年，TCP Reno 在 Tahoe 的基础上增加了快速恢复算法。

这些算法经过了很长时间的发展，到如今都还在优化中。

### 3.1. 慢启动算法 (Slow Start)

慢启动指的是刚刚加入网络的连接，一点一点地提速，不要一上来就像那些特权车一样霸道地把路占满。新同学上高速还是要慢一点，不要把已经在高速上的秩序给搞乱了。

1. 连接建立后先初始化 cwnd = 1 (cwnd 即 Congestion Window)，表明可以传一个 MSS 大小的数据。

1. 每当收到一个 ACK，cwnd++，呈线性上升。

1. 每当过了一个 RTT，cwnd = cwnd*2，呈指数让升。

1. 当 cwnd >= ssthresh (slow start threshold 即慢启动阈值) 时，就会进入“拥塞避免算法”。一般来说 ssthresh 的值是 65535 bytes。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/c13c8eec98d68c48aab0c6acfc0824c5.jpg)

因此，如果实际网速很快的话，ACK 也会返回得快，RTT 也会短，那么这个慢启动就一点也不慢。

P.S. Linux 3.0 后采用了这篇论文的建议——把 cwnd 初始化成了 10 个 MSS。 而 Linux 3.0 以前，比如 2.6，Linux 采用了 RFC3390，cwnd 是跟 MSS 的值来变的，如果 MSS< 1095，则 cwnd = 4；如果 MSS>2190，则 cwnd=2；其它情况下，则是 3。

### 3.2. 拥塞避免算法 (Congestion Avoidance)

当 cwnd >= ssthresh 时，就会进入“拥塞避免算法”，算法如下：
- 收到一个 ACK 时，cwnd = cwnd + 1/cwnd
- 当每过一个 RTT 时，cwnd = cwnd + 1

这样就可以避免增长过快导致网络拥塞，慢慢的增加调整到网络的最佳值。很明显，是一个线性上升的算法。

### 3.3. 拥塞发生算法

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

### 3.4. 快速恢复算法 (Fast Recovery)

快速恢复算法 (Fast Recovery) 是一种启发式机制，定义于 [RFC 5681](http://tools.ietf.org/html/rfc5681)，一般和快速重传同时使用。快速恢复算法认为，若还能收到 3 个 Duplicated Acks，说明网络也不那么糟糕，所以没有必要使用像 RTO 超时那么强烈。Fast Recovery 算法如下：

1. 进入 Fast Recovery 之前，cwnd 和 sshthresh 已被更新。
1. cwnd = sshthresh  + 3 * MSS （确认有 3 个数据包被收到了）
1. 重传 Duplicated ACKs 指定的数据包。
1. 如果再收到 duplicated Acks，那么 cwnd = cwnd +1。
1. 如果收到了新的 ACK，那么 cwnd = sshthresh，就进入了拥塞避免的算法。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/15/7c7ed038edf247fd39029d51b1041f16.jpg)

## 4. Refer Links

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