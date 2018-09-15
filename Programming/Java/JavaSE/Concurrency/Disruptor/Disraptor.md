- [Disraptor](#disraptor)
  - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
  - [2. 设计特性](#2-%E8%AE%BE%E8%AE%A1%E7%89%B9%E6%80%A7)
  - [3. 工作过程](#3-%E5%B7%A5%E4%BD%9C%E8%BF%87%E7%A8%8B)
  - [4. 应用](#4-%E5%BA%94%E7%94%A8)
  - [5. Refer Links](#5-refer-links)

# Disraptor

TODO:

https://www.zhihu.com/question/23235063

https://tech.meituan.com/disruptor.html

http://ifeve.com/locks-are-bad/

http://ifeve.com/disruptor-cacheline-padding/

http://ifeve.com/disruptor-memory-barrier/

http://ifeve.com/dissecting-disruptor-whats-so-special/

https://zhuanlan.zhihu.com/p/21355046

## 1. 基本概念

它是英国外汇交易公司 LMAX 开发的一个高性能队列，研发的初衷是解决内存队列的延迟问题。基于 Disruptor 开发的系统单线程能支撑每秒 600 万订单。

目前，包括 Apache Storm、Log4j2 在内的很多知名项目都应用了 Disruptor 以获取高性能。在楼主公司内部使用 Disruptor 与 Netty 结合用来做 GPS 实时数据的处理，性能相当强悍。

Disruptor 是特别适用于对时间高度敏感的多线程应用。如果服务对时间不敏感完全可以不用 disruptor 而只用 array blocking queue. 再如果废了好大劲挣回来 30 毫秒，结果被一个数据库连接耗掉 1 秒，也没必要用。所以搞清楚适用的环境很重要。

## 2. 设计特性

Disruptor 通过以下设计来解决队列速度慢的问题：

- 环形数组结构
  
  为了避免垃圾回收，采用数组而非链表。因为，数组对处理器的缓存机制更加友好。

- 元素位置定位
  
  数组长度 2^n，通过位运算，加快定位的速度。下标采取递增的形式。不用担心 index 溢出的问题。index 是 long 类型，即使 100 万 QPS 的处理速度，也需要 30 万年才能用完。

- 无锁设计
  
  每个生产者或者消费者线程，会先申请可以操作的元素在数组中的位置，申请到之后，直接在该位置写入或者读取数据。整个过程通过原子变量 CAS，保证操作的线程安全。

- 针对伪共享问题的优化
  
  Disruptor 消除这个问题，至少对于缓存行大小是 64 字节或更少的处理器架构来说是这样的（有可能处理器的缓存行是 128 字节，那么使用 64 字节填充还是会存在伪共享问题），通过增加补全来确保 ring buffer 的序列号不会和其他东西同时存在于一个缓存行中。

## 3. 工作过程

https://tech.meituan.com/disruptor.html

## 4. 应用

Log4j 2 相对于 Log4j 1 最大的优势在于多线程并发场景下性能更优。该特性源自于 Log4j 2 的异步模式采用了 Disruptor 来处理。

## 5. Refer Links
