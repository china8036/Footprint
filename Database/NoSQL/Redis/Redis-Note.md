- [Redis Note](#redis-note)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [定义](#%E5%AE%9A%E4%B9%89)
    - [优势](#%E4%BC%98%E5%8A%BF)
    - [数据类型](#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
    - [应用场景](#%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)
  - [安装](#%E5%AE%89%E8%A3%85)
    - [Windows](#windows)
    - [Linux](#linux)
  - [使用](#%E4%BD%BF%E7%94%A8)
    - [服务端启动和客户端连接](#%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%90%AF%E5%8A%A8%E5%92%8C%E5%AE%A2%E6%88%B7%E7%AB%AF%E8%BF%9E%E6%8E%A5)
    - [配置](#%E9%85%8D%E7%BD%AE)
    - [命令](#%E5%91%BD%E4%BB%A4)
  - [客户端](#%E5%AE%A2%E6%88%B7%E7%AB%AF)
  - [Refer Links](#refer-links)

# Redis Note

## 概述

### 定义

> Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs and geospatial indexes with radius queries. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.


Redis 是一个开源的、使用 ANSI C 语言编写、支持网络、可基于内存存储、可持久化存储的高性能日志型键值对数据库；

与 memcached 一样，为了保证效率，数据都是缓存在内存中。区别的是 redis 会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了 master-slave（主从) 同步；

redis 的出现，很大程度补偿了 memcached 这类 key/value 存储的不足，在部分场合可以对关系数据库起到很好的补充作用；

Redis 支持主从同步。数据可以从主服务器向任意数量的从服务器上同步，从服务器可以是关联其他从服务器的主服务器。这使得 Redis 可执行单层树复制。存盘可以有意无意的对数据进行写操作。由于完全实现了发布 / 订阅机制，使得从数据库在任何地方同步树时，可订阅一个频道并接收主服务器完整的消息发布记录。同步对读取操作的可扩展性和数据冗余很有帮助；

### 优势

- 性能极高 – Redis 能读的速度是 110000 次 /s, 写的速度是 81000 次 /s；
- 丰富的数据类型 – Redis 支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作；
- 原子 – Redis 的所有操作都是原子性的，同时 Redis 还支持对几个操作全并后的原子性执行；
- 丰富的特性 – Redis 还支持 publish/subscribe, 通知，key 过期等等特性；

### 数据类型

Redis 支持的键值数据类型：
- 字符串 string；
- 列表 list；
- 散列 hash；
- 集合 set；
- 有序集合 zset；

### 应用场景

- 缓存；
- 任务队列；
- 应用排行榜；
- 网站访问统计；
- 数据过期处理；
- 分布式集群架构中的 session 分离；

## 安装

### Windows

Redis 官方没有提供 Windows 版本的发行版，但 Microsoft 维护了 Windows 版本的 Redis；

下载：https://github.com/MicrosoftArchive/redis/releases 

安装后会自动将路径添加至环境变量；

验证安装成功：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/15/c63121f7fbc91c31a1ac1ead9e4c5e61.jpg)

### Linux

https://redis.io/topics/quickstart

<!-- TODO: -->

## 使用

### 服务端启动和客户端连接

启动 redis 服务端：
```shell
redis-server redis.windows.conf
```

连接 redis 服务端：
```shell
redis-cli -h 127.0.0.1 -p 6379
```

### 配置

```
daemonize 如果需要在后台运行，把该项改为 yes

pidfile 配置多个 pid 的地址，默认在 /var/run/redis.pid

bind 绑定 ip，设置后只接受自该 ip 的请求

port 监听端口，默认为 6379

timeout 设置客户端连接时的超时时间，单位为秒

loglevel 分为 4 级，debug、verbose、notice、warning

logfile 配置 log 文件地址

databases 设置数据库的个数，默认使用的数据库为 0

save   设置 redis 进行数据库镜像的频率，保存快照的频率。  
    第一个参数表示多长时间，第二个表示执行多少次写操作。
    在一定时间内执行一定数量的写操作时，自动保存快照。可设置多个条件。

rdbcompression 在进行镜像备份时，是否进行压缩

Dbfilename 镜像备份文件的文件名

Dir 数据库镜像备份的文件放置路径

Slaveof 设置数据库为其他数据库的从数据库 

Masterauth 主数据库连接需要的密码验证

Requirepass 设置登录时需要使用的密码

Maxclients 限制同时连接的客户数量

Maxmemory 设置 redis 能够使用的最大内存

Appendonly 开启 append only 模式

appendfsync 设置对 appendonly.aof 文件同步的频率

vm-enabled 是否虚拟内存的支持

vm-swap-file 设置虚拟内存的交换文件路径

vm-max-memory 设置 redis 使用的最大物理内存大小

vm-page-size 设置虚拟内存的页大小

vm-pages 设置交换文件的总 page 数量

vm-max-threads 设置 VMIO 同时使用的线程数量

glueoutputbuf 把小的输出缓存存放在一起

hash-max-zipmap-entries 设置 hash 的临界值

activerehashing 重新 hash
```

### 命令




## 客户端

- python：[redis-py](https://github.com/andymccurdy/redis-py)，[文档](https://redis-py.readthedocs.io/en/latest/)
  
  教程: 
  - http://lawtech0902.com/2017/03/27/Redis-2/
  - http://python.jobbole.com/87305/



## Refer Links

[redis 官方文档](https://redis.io/documentation)

[redis 命令官方手册](https://redis.io/commands)

redis 命令中文手册：
- http://redisdoc.com/
- http://www.redis.cn/commands.html
