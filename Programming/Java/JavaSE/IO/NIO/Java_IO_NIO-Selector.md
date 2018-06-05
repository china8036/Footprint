
# Java IO NIO: Selector

NIO 的实现流程，类似于 select：
1. 创建 ServerSocketChannel 监听客户端连接并绑定监听端口，设置为非阻塞模式。
1. 创建 Reactor 线程，创建多路复用器 (Selector) 并启动线程。
1. 将 ServerSocketChannel 注册到 Reactor 线程的 Selector 上。监听 accept 事件。
1. Selector 在线程 run 方法中无线循环轮询准备就绪的 Key。
1. Selector 监听到新的客户端接入，处理新的请求，完成 tcp 三次握手，建立物理连接。
1. 将新的客户端连接注册到 Selector 上，监听读操作。读取客户端发送的网络消息。
1. 客户端发送的数据就绪则读取客户端请求，进行处理。

## Refer Links

todo: 
《疯狂Java讲义》P787

[Java NIO之选择器](http://www.coolblog.xyz/2018/04/03/Java-NIO%E4%B9%8B%E9%80%89%E6%8B%A9%E5%99%A8/)

[基于 Java NIO 实现简单的 HTTP 服务器](http://www.coolblog.xyz/2018/04/04/%E5%9F%BA%E4%BA%8E-Java-NIO-%E5%AE%9E%E7%8E%B0%E7%AE%80%E5%8D%95%E7%9A%84-HTTP-%E6%9C%8D%E5%8A%A1%E5%99%A8/)

http://ifeve.com/selectors/ 

https://zhuanlan.zhihu.com/p/27419141

https://zhuanlan.zhihu.com/p/27434028

https://zhuanlan.zhihu.com/p/27441342?group_id=859562548406677504



todo: 先搞懂 select/epoll，然后看下面这个问题

https://www.zhihu.com/question/23084473/answer/25133963

目前的服务器的确都是使用io多路复用技术来处理多个连接，也就是在单个线程中监听所有连接的事件，但注意：**实际对这些连接事件的具体处理操作，主流的方式是异步的**。也就是说当使用io多路复用获取到这些连接事件后，并不处理它，而是把它投递到一任务队列中，由一个线程池来专门负责处理这个队列，也就是生产者消费者模式。而不是同步处理直到处理完毕返回，否则，万一不幸的是某一个事件的处理操作长时间阻塞，这得导致后面很多事件超时。

当然，纯粹用io多路复用处理所有请求而且性能非常好的例子也有——redis。因为它的所有的读写请求都是在内存中操作，不存在阻塞超时，也用不上任务队列+线程池的组合。

