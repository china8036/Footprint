
AIO 的实现流程，相比 NIO 有了不少的简化：
1. 创建 AsynchronousServerSocketChannel，绑定监听端口
1. 调用 AsynchronousServerSocketChannel 的 accpet 方法，传入自己实现的 CompletionHandler。包括上一步，都是非阻塞的
1. 连接传入，回调 CompletionHandler 的 completed 方法，在里面，调用 AsynchronousSocketChannel 的 read 方法，传入负责处理数据的 CompletionHandler。
1. 数据就绪，触发负责处理数据的 CompletionHandler 的 completed 方法。继续做下一步处理即可。
1. 写入操作类似，也需要传入 CompletionHandler。



todo:
《疯狂Java讲义》P709 P792
https://zhuanlan.zhihu.com/p/27451893
https://zhuanlan.zhihu.com/p/27466965
https://zhuanlan.zhihu.com/p/27484203

https://zhuanlan.zhihu.com/p/27519705
https://zhuanlan.zhihu.com/p/27572086
https://zhuanlan.zhihu.com/p/27590299
