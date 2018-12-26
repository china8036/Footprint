- [Tomcat 并发处理](#tomcat-%E5%B9%B6%E5%8F%91%E5%A4%84%E7%90%86)
  - [1. 并发处理机制](#1-%E5%B9%B6%E5%8F%91%E5%A4%84%E7%90%86%E6%9C%BA%E5%88%B6)
  - [2. 提高并发能力的方法](#2-%E6%8F%90%E9%AB%98%E5%B9%B6%E5%8F%91%E8%83%BD%E5%8A%9B%E7%9A%84%E6%96%B9%E6%B3%95)
    - [2.1. 使用 NIO](#21-%E4%BD%BF%E7%94%A8-nio)
    - [2.2. 加大 tomcat 连接数](#22-%E5%8A%A0%E5%A4%A7-tomcat-%E8%BF%9E%E6%8E%A5%E6%95%B0)
    - [2.3. 加大 tomcat 可用内存](#23-%E5%8A%A0%E5%A4%A7-tomcat-%E5%8F%AF%E7%94%A8%E5%86%85%E5%AD%98)
  - [3. Refer Links](#3-refer-links)

# Tomcat 并发处理

## 1. 并发处理机制

TODO:

## 2. 提高并发能力的方法

### 2.1. 使用 NIO

Tomcat 的最大并发数是可以配置的，实际运用中，最大并发数与硬件性能和 CPU 数量都有很大关系的。更好的硬件，更多的处理器都会使 Tomcat 支持更多的并发。

Tomcat 默认的 HTTP 实现是采用阻塞式的 Socket 通信，每个请求都需要创建一个线程处理。这种模式下的并发量受到线程数的限制，但对于 Tomcat 来说几乎没有 BUG 存在了。

Tomcat 还可以配置 NIO 方式的 Socket 通信，在性能上高于阻塞式的，每个请求也不需要创建一个线程进行处理，并发能力比前者高。但没有阻塞式的成熟。

### 2.2. 加大 tomcat 连接数

在 tomcat 配置文件 server.xml 中的<Connector ... />配置中，和连接数相关的参数有：
- minProcessors：最小空闲连接线程数，用于提高系统处理性能，默认值为 10
- maxProcessors：最大连接线程数，即：并发处理的最大请求数，默认值为 75
- acceptCount：允许的最大连接数，应大于等于 maxProcessors，默认值为 100
- enableLookups：是否反查域名，取值为：true 或 false。为了提高处理能力，应设置为 false
- connectionTimeout：网络连接超时，单位：毫秒。设置为 0 表示永不超时，这样设置有隐患的。通常可设置为 30000 毫秒。

其中和最大连接数相关的参数为 maxProcessors 和 acceptCount。如果要加大并发连接数，应同时加大这两个参数。

web server 允许的最大连接数还受制于操作系统的内核参数设置，通常 Windows 是 2000 个左右，Linux 是 1000 个左右。

对于其他端口的侦听配置，以此类推。

### 2.3. 加大 tomcat 可用内存

## 3. Refer Links

[Tomcat 是如何处理并发请求的？](https://www.zhihu.com/question/20194686)

[Tomcat 的性能与最大并发](http://www.pinhuba.com/javaweb/101216.htm)

[【Linux】I/O 工作模型及 Web 服务器原理](https://www.cnblogs.com/hukey/p/5470318.html)
