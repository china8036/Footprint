- [Redis 面试 FAQ](#redis-面试-faq)
  - [1. Redis 与 Memcached 的区别？](#1-redis-与-memcached-的区别)
  - [2. Redis 为什么这么快？](#2-redis-为什么这么快)
  - [3. Redis 怎么利用多核 CPU？](#3-redis-怎么利用多核-cpu)
  - [Redis 的同步机制？](#redis-的同步机制)
  - [4. Refer Links](#4-refer-links)

# Redis 面试 FAQ

## 1. Redis 与 Memcached 的区别？

- Redis 采用的是基于内存的采用的是单进程单线程模型的 KV 数据库，由 C 语言编写；Memcached 采用的是基于内存的单进程多线程模型的 KV 数据库。
- Redis 支持2种持久化方式（RDB和AOF），重启的时候可以自动加载进行使用；Memcached 不支持持久化。
- Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，zset，hash等数据结构的存储；Memcached仅支持简单的数据类型 String。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/12/ed838eae55678c60681b805a56f49a77.jpg)

## 2. Redis 为什么这么快？

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/12/a956b81c077f187f3e0a63a8f53fb8ea.jpg)

[官方提供的数据](https://redis.io/topics/benchmarks) 是可以达到 100000+ 的 QPS（每秒内查询次数）。

为什么这么快？

- 完全基于内存，绝大部分请求是纯粹的内存操作，非常快速。
- 数据结构简单，对数据操作也简单，Redis 中的数据结构是专门进行设计的。
- 采用单线程，避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗。
- 使用同步非阻塞的多路复用 I/O 模型。
- 一般的系统调用系统函数的话，会浪费一定的时间去移动和请求，而 Redis 直接自己构建了 VM 机制。

## 3. Redis 怎么利用多核 CPU？

Redis is single threaded. How can I exploit multiple CPU / cores?

> It's not very frequent that CPU becomes your bottleneck with Redis, as usually Redis is either memory or network bound. For instance, using pipelining Redis running on an average Linux system can deliver even 1 million requests per second, so if your application mainly uses O(N) or O(log(N)) commands, it is hardly going to use too much CPU.
>
> However, to maximize CPU usage you can start multiple instances of Redis in the same box and treat them as different servers. At some point a single box may not be enough anyway, so if you want to use multiple CPUs you can start thinking of some way to shard earlier.
> 
> You can find more information about using multiple Redis instances in the Partitioning page.
> 
> However with Redis 4.0 we started to make Redis more threaded. For now this is limited to deleting objects in the background, and to blocking commands implemented via Redis modules. For the next releases, the plan is to make Redis more and more threaded.

单进程单线程的 Redis 已经非常快了，并且其瓶颈主要在于内存大小和网络带宽，而不是 CPU。如果为利用多核 CPU 而将 Redis 改造为多线程，反而会带来 CPU 上下文切换、并发 race condition 等问题。

但是，可以在同一个多核的服务器中，可以**启动多个 Redis-server 实例，组成 master-master 或者 master-slave 的形式**，需要执行耗时的命令时可以完全在 slave 进行，从而避免执行耗时命令时网络服务的并发下降。由于是单线程模型，Redis 更喜欢大缓存快速 CPU 而不是多核，因此可进一步为各个实例绑定独立的 CPU，避免不必要的 CPU 切换。

## Redis 的同步机制？

Redis 可以使用主从同步，从从同步。第一次同步时，主节点做一次 bgsave，并同时将后续修改操作记录到内存 buffer，待完成后将 rdb 文件全量同步到复制节点，复制节点接受完成后将 rdb 镜像加载到内存。加载完成后，再通知主节点将期间修改的操作记录同步到复制节点进行重放就完成了同步过程。

## 4. Refer Links

[为什么说 Redis 是单线程的以及 Redis 为什么这么快！](https://blog.csdn.net/xlgen157387/article/details/79470556)

[Redis FAQ](https://redis.io/topics/faq)

[redis 面试问题（一）](https://www.nowcoder.com/discuss/92610)

[redis 面试问题（二）](https://www.nowcoder.com/discuss/92611)

[天下无难试之 Redis 面试题刁难大全](https://zhuanlan.zhihu.com/p/32540678)

[面试中关于 Redis 的问题看这篇就够了](https://juejin.im/post/5ad6e4066fb9a028d82c4b66)