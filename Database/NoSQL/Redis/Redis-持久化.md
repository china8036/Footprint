- [Redis 持久化](#redis-持久化)
  - [5. Refer Links](#5-refer-links)

# Redis 持久化

Redis 也提供了持久化的选项，这些选项可以让用户将自己的数据保存到磁盘上面进行存储。根据实际情况，可以每隔一定时间将数据集导出到磁盘（快照），或者追加到命令日志中（AOF 只追加文件），他会在执行写命令时，将被执行的写命令复制到硬盘里面。您也可以关闭持久化功能，将 Redis 作为一个高效的网络的缓存数据功能使用。

- 全量持久化 -- BGSAVE
- 增量持久化 -- AOF

bgsave 做镜像全量持久化，aof 做增量持久化。因为 bgsave 会耗费较长时间，不够实时，在停机的时候会导致大量丢失数据，所以需要 aof 来配合使用。在 redis 实例重启时，会使用 bgsave 持久化文件重新构建内存，再使用 aof 重放近期的操作指令来实现完整恢复重启之前的状态。

对方追问那如果突然机器掉电会怎样？取决于 aof 日志 sync 属性的配置，如果不要求性能，在每条写指令时都 sync 一下磁盘，就不会丢失数据。但是在高性能的要求下每次都 sync 是不现实的，一般都使用定时 sync，比如 1s1 次，这个时候最多就会丢失 1s 的数据。

对方追问 bgsave 的原理是什么？你给出两个词汇就可以了，fork 和 cow。fork 是指 redis 通过创建子进程来进行 bgsave 操作，cow 指的是 copy on write，子进程创建后，父子进程共享数据段，父进程继续提供读写服务，写脏的页面数据会逐渐和子进程分离开来。

Redis 分别提供了 RDB 和 AOF 两种持久化机制：
- RDB 将数据库的快照（snapshot）以二进制的方式保存到磁盘中。
- AOF 则以协议文本的方式，将所有对数据库进行过写入的命令（及其参数）记录到 AOF 文件，以此达到记录数据库状态的目的。

## 5. Refer Links

[深入学习 Redis（2）：持久化](https://www.cnblogs.com/kismetv/p/9137897.html)

[redis 持久化——RDB、AOF](https://lanjingling.github.io/2015/11/16/redis-chijiuhua/)

[Redis 设计与实现：AOF](https://redisbook.readthedocs.io/en/latest/internal/aof.html)

[Redis 设计与实现：RDB](https://redisbook.readthedocs.io/en/latest/internal/rdb.html)