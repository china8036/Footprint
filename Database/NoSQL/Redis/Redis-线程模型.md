- [Redis 线程模型](#redis-线程模型)
  - [5. Refer Links](#5-refer-links)

# Redis 线程模型

NOTE
- Redis的单线程模型，指的只是在处理网络请求的时候Redis只有一个线程来处理，而一个 Redis Server 实例运行的时候肯定是不止一个线程的，如Redis进行持久化的时候会以子进程或者子线程的方式执行。


## 5. Refer Links

[Redis 网络架构及单线程模型](http://blog.jobbole.com/100079/)

[Redis 和 I/O 多路复用](https://draveness.me/redis-io-multiplexing)