- [计算机网络 - 传输层：Transport control protocol 传输策略：拥塞控制](#计算机网络---传输层transport-control-protocol-传输策略拥塞控制)
  - [1. 历史发展](#1-历史发展)
  - [2. 慢启动算法 (Slow Start)](#2-慢启动算法-slow-start)
  - [3. 拥塞避免算法 (Congestion Avoidance)](#3-拥塞避免算法-congestion-avoidance)
  - [4. 拥塞发生算法](#4-拥塞发生算法)
  - [5. 快速恢复算法 (Fast Recovery)](#5-快速恢复算法-fast-recovery)
  - [6. Refer Links](#6-refer-links)

# 计算机网络 - 传输层：Transport control protocol 传输策略：拥塞控制

当提供给任何网络的负载超过它的负载能力时，拥塞便会产生，传输层接收到从网络层反馈来的拥塞信息后，应减慢它发送到网络层中的流量速率。拥塞控制 (Congestion Handling) 是传输层需要解决的问题之一，也是 TCP 的一个关键功能。

当网络上的延时突然增加时，如果 TCP 对这个事做出的应对只有重传数据，这将会导致网络的负担更重，于是会导致更大的延迟以及更多的丢包。如果一个网络内有成千上万的 TCP 连接都这么行事，那么马上就会形成“网络风暴”，TCP 这个协议就会拖垮整个网络。因此，[TCP 关于拥塞控制的设计理念](http://ee.lbl.gov/papers/congavoid.pdf) 是：**TCP 不是一个自私的协议，当拥塞发生的时候，要做自我牺牲。就像交通阻塞一样，每个车都应该把路让出来，而不要再去抢路了**。

<!-- TODO: 网络特点：每个节点都是盲人，无法获知全局信息；拥塞控制都是自组织的。一切都是自动完成的，没有控制中心，没有开始和结束。 -->

<!-- TODO: AIMD (Additive Increase Multiplicative Decrease) 和式增加，积式减少：当 TCP 发送方感受到端到端路径无拥塞时就线性的增加其发送速度，当察觉到路径拥塞时就乘性减小其发送速度。
当 TCP 发送端收到 ACK，并且没有检测到丢包事件时，拥塞窗口加 1；当 TCP 发送端检测到丢包事件后，拥塞窗口除以 2。？ -->

<!-- TODO: 说到 TCP 原理，一般的人谈传输效率，也就是吞吐率，了解的人谈公平性，以及收敛性。 -->

<!--
https://blog.csdn.net/dog250/article/details/90340322
https://blog.csdn.net/dog250/article/details/90446782
http://blog.chinaunix.net/uid-28387257-id-4543179.html
https://blog.sometimesnaive.org/article/8

https://allen-kevin.github.io/2017/05/14/%E6%B7%B7%E5%90%88%E6%85%A2%E5%90%AF%E5%8A%A8%E7%AE%97%E6%B3%95/
-->

## 1. 历史发展

TCP 拥塞控制的各个阶段主要使用了以下四个算法：
- 1988 年，TCP Tahoe 提出了**慢启动算法**、**拥塞避免算法**、拥塞发生时的**快速重传算法**。
- 1990 年，TCP Reno 在 Tahoe 的基础上增加了**快速恢复算法**。

**这些算法经过了很长时间的发展，到如今都还在优化中**。

## 2. 慢启动算法 (Slow Start)

> 新同学上高速还是要慢一点，不要把已经在高速上的秩序给搞乱了。

慢启动指的是刚刚加入网络的连接，一点一点地提速，不要一上来就像那些特权车一样霸道地把路占满。

1. 连接建立后先初始化 cwnd = 1 (cwnd 即 Congestion Window)，表明可以传一个 MSS 大小的数据。

1. 每当收到一个 ACK，cwnd++，呈线性上升；每当过了一个 RTT（也就是说，收到了 cwnd 个 ACK），cwnd = cwnd + cwnd = cwnd*2，呈指数让升。

1. 当 cwnd >= ssthresh (slow start threshold，慢启动阈值) 时，就会进入“拥塞避免算法”。**一般来说 ssthresh 的值是 65535 bytes**。

![image](http://img.cdn.firejq.com/jpg/2018/6/15/c13c8eec98d68c48aab0c6acfc0824c5.jpg)

因此，如果实际网速很快的话，ACK 也会返回得快，RTT 也会短，那么这个慢启动就一点也不慢。

P.S. Linux 3.0 后把 cwnd 初始化成了 10 个 MSS，而 Linux 3.0 以前，比如 2.6，Linux 采用了 RFC3390，cwnd 是跟 MSS 的值来变的，如果 MSS < 1095，则 cwnd = 4；如果 MSS > 2190，则 cwnd=2；其它情况下，则是 3。

<!-- ![image](http://img.cdn.firejq.com/jpg/2019/9/24/dff9af258430688c252c663ff4195a49.jpg) -->

<!-- https://www.zhihu.com/question/24886217 -->

<!-- https://www.tomorrow.wiki/archives/2072 -->

## 3. 拥塞避免算法 (Congestion Avoidance)

当 cwnd >= ssthresh 时，就会进入“拥塞避免算法”，算法如下：
- 收到一个 ACK 时，cwnd = cwnd + 1/cwnd
- 当每过一个 RTT 时，cwnd = cwnd + 1

**这样就可以避免增长过快导致网络拥塞，慢慢的增加调整到网络的最佳值。很明显，是一个线性上升的算法**。

<!-- TODO: 为什么最终会稳定？ -->

## 4. 拥塞发生算法

若拥塞已经在网络中发生，数据包开始丢失，TCP 会通过动态调整慢启动阈值来控制拥塞。存在 2 种情况：
- 等到 RTO 超时，重传数据包。TCP 认为这种情况太糟糕，因此做出的反应也很强烈：
  - sshthresh =  cwnd /2
  - cwnd 重置为 1
  - 进入慢启动过程
- Fast Retransmit 算法，也就是在收到 3 个 duplicate ACK 时就开启重传，而不用等到 RTO 超时。
  - cwnd = cwnd /2
  - sshthresh = cwnd
  - 进入快速恢复算法 Fast Recovery

可以看到 **RTO 超时后，sshthresh 会变成 cwnd 的一半**，这意味着，如果 cwnd <= sshthresh 时出现的丢包，那么 TCP 的 sshthresh 就会减了一半，然后等 cwnd 又很快地以指数级增涨爬到这个地方时，就会成慢慢的线性增涨。**TCP 就是通过这种强烈的震荡，快速而小心得找到网站流量的平衡点**。

## 5. 快速恢复算法 (Fast Recovery)

快速恢复算法 (Fast Recovery) 是一种启发式机制，定义于 [RFC 5681](http://tools.ietf.org/html/rfc5681) 中，一般和快速重传同时使用。**快速恢复算法认为，若还能收到 3 个 Duplicated Acks，说明网络也不那么糟糕，所以没有必要使用像 RTO 超时那么强烈**。

Fast Recovery 算法如下：
1. 进入 Fast Recovery 之前，cwnd 和 sshthresh 已被更新。
1. cwnd = sshthresh  + 3 * MSS （确认有 3 个数据包被收到了）
1. 重传 Duplicated ACKs 指定的数据包。
1. 如果再收到 duplicated Acks，那么 cwnd = cwnd +1。
1. 如果收到了新的 ACK，那么 cwnd = sshthresh，就进入了拥塞避免的算法。

![image](http://img.cdn.firejq.com/jpg/2018/6/15/7c7ed038edf247fd39029d51b1041f16.jpg)

## 6. Refer Links

[TCP 协议：笔试面试知识点整理](https://hit-alibaba.github.io/interview/basic/network/TCP.html)

[TCP 为什么是三次握手，而不是两次或四次？](https://www.zhihu.com/question/24853633)

[TCP 的那些事儿（上）](https://coolshell.cn/articles/11564.html)

[TCP 的那些事儿（下）](https://coolshell.cn/articles/11609.html)

[阮一峰：TCP 协议简介](http://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html)