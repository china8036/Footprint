- [MySQL 通讯协议](#mysql)
  - [1. 基本概念](#1)
  - [2. 连接方式](#2)
  - [3. 通讯过程](#3)
  - [4. 报文内容](#4)
  - [5. Refer Links](#5-refer-links)

# MySQL 通讯协议
<!-- todo: 待补充 -->
## 1. 基本概念

> The MySQL protocol is used between MySQL Clients and a MySQL Server. It is implemented by:
> 
> Connectors (Connector/C, Connector/J, and so forth)
> 
> MySQL Proxy
> 
> Communication between master and slave replication servers
> 
> The protocol supports these features:
> 
> Transparent encryption using SSL
> 
> Transparent compression using Compression
> 
> A Connection Phase where capabilities and authentication data are exchanged
> 
> A Command Phase which supports the needs of Section 14.7, “Prepared Statements” and Section 14.8, “Stored Procedures”

## 2. 连接方式

到 MySQL5.7 为止，总共有 5 种连接方式，分别是 TCP/IP，TLS/SSL，Unix Sockets，Shared Memory，Named pipes.
- TCP/IP 方式：默认开启，所有系统都支持，通过 `–skip-networking=yes` 开启，可通过 `–port –bind-address` 配置参数。
- TLS/SSL 方式：默认开启，所有系统都支持，通过 `–ssl=yes` 开启，可通过	`–ssl-* options` 配置参数。
- Unix Sockets 方式：默认开启，仅类 Unix 系统支持，仅支持本机通讯，通过设置`–socket=<empty>` 关闭，可通过	`–socket=socket path` 配置参数。
- Shared Memory 方式：默认关闭，仅 Windows 系统支持，仅支持本机通讯，通过设置 `–shared-memory=on/off` 开启和关闭，可通过 `–shared-memory-base-name=<name>` 配置参数。
- Named pipes 方式：默认关闭，仅 Windows 系统支持，通过设置 `–enable-named-pipe=on/off` 开启和关闭，可通过	`–socket=<name>` 配置参数。

例 1：通过 `mysql -uroot` 连接 MySQL，在 unix 系统上默认使用 Unix Sockets，在非 unix 系统上默认使用 TCP/IP。

例 2：通过 `mysql --protocol=tcp -uroot` 或 `mysql -P3306 -h127.0.0.1 -uroot` 连接 MySQL，则指定使用 TCP/IP。

例 3：通过 `mysql --protocol=tcp -uroot --ssl=on` 或 `mysql -P3306 -h127.0.0.1 -uroot --ssl=on` 连接 MySQL，则指定使用 TLS/SSL。

Q&A：
- 我们如何选择使用哪一种连接方式呢？
  - 若是你能确定程序和数据库在同一台机子（类 Unix 系统) 上，推荐使用 Unix Sockets，因为它效率更高。
  - 若数据库分布在不同的机子上，且能确保连接安全或者安全性要求不是那么高，推荐使用 TCP/IP，反之使用 TLS/SSL。

- Unix socket 方式登陆与 TCP 方式登陆有什么区别和联系？

  Unix socket 是实现进程间通信的一种方式，mysql 支持利用 Unix socket 来实现客户端 - 服务端的通信，但要求客户端和服务端在同一台机器上。对于 unix socket 而言，同样也是一种套接字，监听线程会同时监听 TCP socket 和 Unix socket，接受到请求然后处理，后续的处理逻辑都是一致的，只不过底层通信方式不一样罢了。
  ```
  mysql  -h127.0.0.1 –P3306 –uxxx –pxxx 【TCP 通讯方式】
  mysql  -uxxx –pxxx –S/usr/mysql/mysql.sock  【unix socket 通信方式】
  ```

- 监听 socket 是否与通信 socket 公用一个端口？

  服务端一直有一个监听 socket 在 3306 端口监听，等待新进来的客户请求，一旦一个请求过来，服务端会重新创建一个新的通信 socket，这个新的 socket 专门用于与这个客户通信，而监听 socket 则继续监听。虽然是 2 个套接字，但监听 socket 和通信 socket 都是同一个端口，通过 netstat 可以确认这个问题。

- <!-- todo: JDBC 建立连接时能否控制使用哪一种？ -->

## 3. 通讯过程

MySQL 的客户端与服务器之间支持多种通讯方式，其中最广泛使用的是 TCP 通讯，因此，以下介绍 TCP 方式的通讯过程。

服务器启动后，会使用 TCP 监听一个本地端口，当客户端的连接请求到达时，就会执行三段握手以及 MySQL 的权限验证；验证成功后，客户端开始发送请求，服务器会以响应的报文格式返回数据；当客户端发送完成后，会发送一个特殊的报文，告知服务器已结束会话。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/18/fc9c7c477d8102c71b9ad466405cdf38.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/18/dd9a8f12a7fac1c8c02b9d7a143cc140.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/18/819920ea246cc6ee6f3ff0b5d60f69e9.jpg)

分为以下几个阶段：
1. 三次握手建立 TCP 连接。

1. 认证阶段 (auth-phase):
    1. server -> client: send **Handshake Initialization Packet**, which contains the version, some flags and a password challenge.
    1. client -> server: send **Client Authentication Packet**, with username, some flags and the response to the challenge.
    1. server -> client: As the client provided the right password and the flags are fine, the server responds with a **OK_Packet**. That closes auth-phase and switches to the command-phase.

1. 命令执行阶段 (command-phase):
    1. client -> server: 发送 SQL 包，即 Client 将执行命令编码成 Server 要求的格式传输给 Server 端执行。
    1. server -> client: 执行并返回 resultset 包，Client 端根据相应的数据包格式解析获得所需的数据。

1. 断开连接
    - 客户端执行正常退出指令：客户端发送 **Quit Packet**，然后执行 TCP 四次挥手结束连接。
    - 客户端异常关闭：客户端发送 RST 标志的 TCP 包，通讯直接结束。

对于其它连接方式，除了第一个阶段建立连接和最后一个阶段断开连接和 TCP 方式不同外，其它的通讯过程大体上是相同的。

## 4. 报文内容

todo:

https://jin-yang.github.io/post/mysql-protocol.html

http://www.godpan.me/2017/11/10/mysql-protocol.html

## 5. Refer Links

[为什么数据库连接很消耗资源](https://blog.csdn.net/lmy86263/article/details/76165714)

[MySQL 通讯协议](https://jin-yang.github.io/post/mysql-protocol.html)

[深度解析 mysql 登录原理](http://www.cnblogs.com/cchust/p/5025880.html)

[MySQL 网络协议分析](http://www.godpan.me/2017/11/10/mysql-protocol.html)

[【Source Code Documentation】MySQL 8.0.11 Client/Server Protocol ](https://dev.mysql.com/doc/dev/mysql-server/8.0.11/PAGE_PROTOCOL.html)

[【MySQL Internals Manual 】Chapter 14 MySQL Client/Server Protocol](https://dev.mysql.com/doc/internals/en/client-server-protocol.html)

<!--  
todo:
https://wenku.baidu.com/view/793dc7dad15abe23482f4dba.html
https://wenku.baidu.com/view/3bb97a1dc5da50e2524d7ff7.html

-->