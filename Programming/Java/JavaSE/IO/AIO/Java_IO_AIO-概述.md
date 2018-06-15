- [Java AIO 概述](#java-aio- 概述)
  - [Refer Links](#refer-links)

# Java AIO 概述

Java 7 的 NIO.2 在 java.nio.channels 包下增加了多个以 Asynchronous 开头的 Channel 接口和类，提供了基于异步 Channel 的 IO 支持，这种异步 Channel 的 IO 机制也称为异步 IO (Asynchronous IO)。

AIO 的实现流程，相比 NIO 有了不少的简化：
1. 创建 AsynchronousServerSocketChannel，绑定监听端口
1. 调用 AsynchronousServerSocketChannel 的 accpet 方法，传入自己实现的 CompletionHandler。包括上一步，都是非阻塞的
1. 连接传入，回调 CompletionHandler 的 completed 方法，在里面，调用 AsynchronousSocketChannel 的 read 方法，传入负责处理数据的 CompletionHandler。
1. 数据就绪，触发负责处理数据的 CompletionHandler 的 completed 方法。继续做下一步处理即可。
1. 写入操作类似，也需要传入 CompletionHandler。

## Refer Links
todo:

[Java NIO(8) : 异步模型之状态机](https://zhuanlan.zhihu.com/p/27451893)

[Java NIO(9) : 状态机](https://zhuanlan.zhihu.com/p/27466965)

[Java NIO(10): 异步模型之 Callback](https://zhuanlan.zhihu.com/p/27484203)

[协程，高并发 IO 的终级杀器 (1)](https://zhuanlan.zhihu.com/p/27519705)

[协程，高并发 IO 的终极杀器 (2)](https://zhuanlan.zhihu.com/p/27572086)

[协程，高并发 IO 终极杀器 (3)](https://zhuanlan.zhihu.com/p/27590299)
