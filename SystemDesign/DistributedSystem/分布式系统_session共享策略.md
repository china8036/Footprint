- [分布式系统的 session 共享机制](#分布式系统的-session-共享机制)
  - [1. Session 保持（黏性 session）](#1-session-保持黏性-session)
  - [2. Session 复制](#2-session-复制)
  - [3. Session 共享](#3-session-共享)
  - [4. Refer Links](#4-refer-links)

# 分布式系统的 session 共享机制

在搭建完集群或者分布式环境之后，如果不做任何处理的话，网站将频繁的出现用户未登录的现象。比如：集群中有 A、B 两台服务器，用户第一次访问网站时，Nginx 将用户请求分发到 A 服务器，这时 A 服务器给用户创建了一个 Session，当用户第二次访问网站时，假设 Nginx 将用户请求分发到了 B 服务器上，而这时 B 服务器并不存在用户的 Session，所以就会出现用户未登录的情况，这对用户来说是不可忍受的。

因此，在搭建集群 / 分布式环境之后，必须考虑的一个问题就是用户访问产生的 session 如何处理，即 session 的共享机制。

处理 Session 的方式大致分为三种：Session 保持（黏性 Session）、Session 复制、Session 共享。

## 1. Session 保持（黏性 session）

Session 保持（会话保持）就是将用户锁定到某一个服务器上。比如上面说的例子，用户第一次请求时，负载均衡器（Nginx）将用户的请求分发到了 A 服务器上，如果负载均衡器（Nginx）设置了 Session 保持的话，那么用户以后的每次请求都会分发到 A 服务器上，相当于把用户和 A 服务器粘到了一块，这就是 Session 保持的原理。Session 保持方案在所有的负载均衡器都有对应的实现。而且这是在负载均衡这一层就可以解决 Session 问题。

优点：非常简单，不需要对 session 做任何处理。

缺点：
- 负责不绝对均衡了：由于使用了 Session 保持，很显然就无法保证负载绝对的均衡。
- 缺乏容错性：如果后端某台服务器宕机，那么这台服务器的 Session 丢失，被分配到这台服务请求的用户还是需要重新登录，所以没有彻底的解决问题。

适用场景：发生故障对客户产生的影响较小；服务器发生故障是低概率事件。

实现方式：以 Nginx 为例，在 upstream 模块配置 ip_hash 属性即可实现粘性 Session。

## 2. Session 复制

针对 Session 保持的容错性缺点，我们可以在所有服务器上都保存一份用户的 Session 信息。这种将每个服务器中的 Session 信息复制到其它服务器上的处理办法就称为会话复制。当任何一台服务器上的 session 发生改变时，该节点会把 session 的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要 session，以此来保证 Session 同步。

优点：可容错，各个服务器间的 Session 能够实时响应。

缺点：
- 将 session 广播同步给成员，会对网络负荷造成一定压力；
- 如果 session 量大的话可能会造成网络堵塞，拖慢服务器性能，特别是 session 保存的对象较大，并且对象变化较快时；从而也使应用水平扩展受到限制；
- session 内容的序列化，也消耗了系统的性能；
- 实现方式局限，必须在同一种组件之间实现（比如用的 tomcat，则必须全部用 tomcat）。

实现方式：tomcat 本身已支持该功能，可以参考官方文档。而且这种处理方式，**在大的集群中也不推荐生产环境使用**。

Tomcat 的会话复制分为两种：
- 全局复制 (DeltaManager)：复制会话中的变更信息到集群中的所有其他节点。
- 非全局复制（BackupManager）：它会把 Session 复制给一个指定的备份节点。

## 3. Session 共享

Session 共享的实现方式有很多种，比如 memcached、Redis、DB 等；核心思想是修改 tomcat 的 session 存储机制，使之能够把 session 序列化，然后存放到 memcached 中。

使用 Session 共享可以分为两种机制：
- 黏性 Session 处理方式：
  
  Tomcat 本地 Session 为主 Session，Memcached 中的 Session 为备 Session。用户访问时首先在 tomcat 中创建 session，然后将 session 复制一份放到对应的 memcahed 上。memcache 只起备份作用，读写都在 tomcat 上。当某一个 tomcat 挂掉后，集群将用户的访问定位到备 tomcat 上，然后根据 cookie 中存储的 SessionId 找 session，找不到时，再去相应的 memcached 上取 session，找到之后将其复制到备 tomcat 上。

- 非黏性 Session 处理方式：
  
  Tomcat 本地 Session 为中转 Session，Memcached 为主备 Session。创建的 session 都往 memcached 上写，读取都从 memcached 读取，tomcat 本身不存储 session。

实现方式：[Tomcat+Nginx+MSM+memcached](https://my.oschina.net/bgq365/blog/870569) 

[关于 tomcat 集群中 session 共享的三种方法](http://ezerg.iteye.com/blog/2068557) 
- 使用 filter 方法存储
- 使用 tomcat session manager 方法存储
- 使用 terracotta 服务器共享

## 4. Refer Links
