- [IO 模型](#io)
	- [1. 基本概念](#1)
		- [1.1. 阻塞和非阻塞](#11)
		- [1.2. 同步和异步](#12)
	- [2. IO modules](#2-io-modules)
		- [2.1. 同步阻塞式 IO](#21--io)
		- [2.2. 同步非阻塞式 IO](#22--io)
		- [2.3. 同步非阻塞式 IO / 事件驱动 IO / IO 多路复用](#23--io----io---io)
		- [2.4. 异步非阻塞式 IO / 异步 IO](#24--io----io)
		- [2.5. 信号驱动式 IO](#25--io)
		- [2.6. 比较](#26)
	- [3. Refer Links](#3-refer-links)

# IO 模型

## 1. 基本概念

IO 操作实际上可以分为两步：
1. 发起 IO 请求（即查询目标资源就绪状态）。
2. 实际的 IO 操作（即从内核向进程复制数据）。

### 1.1. 阻塞和非阻塞

**如果在第一步发起 IO 请求时发生阻塞，那么这个 IO 就可以说阻塞的，否则是非阻塞的**。

阻塞和非阻塞描述的是用户线程调用内核 IO 操作的方式，是指在用户程序查询 IO 就绪状态时（比如查询 IO 是否有数据），用户程序对 IO 不同的就绪状态所表现出来的不同形式。以读取数据为例，当 IO 没有数据可供读取时，如果是阻塞 IO，程序会一直阻塞直至 IO 有数据，如果是非阻塞 IO，程序会直接返回错误码说当前没有数据，请稍后再来查询。

你打电话问书店老板有没有《分布式系统》这本书，你如果是阻塞式调用，你会一直把自己“挂起”，直到得到这本书有没有的结果，如果是非阻塞式调用，你不管老板有没有告诉你，你自己先一边去玩了， 当然你也要偶尔过几分钟 check 一下老板有没有返回结果。

### 1.2. 同步和异步

**如果在第二步实际 IO 操作时发生阻塞，那么这个 IO 就是同步的，否则就是异步的**。

同步和异步描述的是用户线程与内核的交互方式，是由在进行实际的 IO 操作时，用户程序是否等待数据操作完成来决定。还是以读取数据为例，如果是同步 IO，用户程序会等待读取数据完成，在此期间这个线程什么也不做，如果是异步 IO，用户程序可以去作别的事情，内核在完成数据读取后，会以回调的方式通知用户程序。

你打电话问书店老板有没有《分布式系统》这本书，如果是同步通信机制，书店老板会说，你稍等，”我查一下"，然后开始查啊查，等查好了（可能是 5 秒，也可能是一天）告诉你结果（返回结果）。而异步通信机制，书店老板直接告诉你我查一下啊，查好了打电话给你，然后直接挂电话了（不返回结果）。然后查好了，他会主动打电话给你。在这里老板通过“回电”这种方式来回调。

NOTE: 在处理 IO 的时候，阻塞和非阻塞都是同步 IO。只有使用了特殊的 API ( 如 POSIX 的 aio_* 系列函数 ) 才是异步 IO。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/cb68a4980c64f35d6358a290ffbf26e4.jpg)

## 2. IO modules

在 UNIX 中，有 5 种可用的 IO 模型：
- 同步阻塞式 IO (Blocking IO)
- 同步非阻塞式 IO / 轮询 (Non-blocking IO)
- 同步非阻塞式 IO / 事件驱动 IO / IO 多路复用 (IO Multiplexing) 
- 异步非阻塞式 IO / 异步 IO (Asynchronous IO) 
- 信号驱动式 IO (Signal Driven IO) (SIGIO)

NOTE: [不存在 “异步阻塞式 IO” 的说法](https://www.zhihu.com/question/65519203/answer/233433548)。

### 2.1. 同步阻塞式 IO

同步阻塞 IO 模型是最简单的 IO 模型，默认情况下创建的所有 socket 都是阻塞的。

同步阻塞 IO 模型比较简单，操作流程如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/44971233fd8eba6fae383ba9fb5a586c.jpg)

伪代码：
```
{
    read(socket, buffer);
    process(buffer);
}
```

用户线程通过系统调用 read 发起 IO 读操作，由用户空间转到内核空间。内核等到数据包到达后，然后将接收的数据拷贝到用户空间，完成 read 操作。

在整个 IO 请求的过程中，用户线程是被阻塞的，这导致用户在发起 IO 请求时，不能做任何事情，对 CPU 的资源利用率很低。

### 2.2. 同步非阻塞式 IO

同步非阻塞 IO 模型，在同步阻塞 IO 的基础上，将 socket 设置为 NONBLOCK。

同步非阻塞 IO 模型中，用户线程可以在发起 IO 请求后立即返回，操作流程如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/0136acbda8621455165d9c327817cc8e.jpg)

伪代码：
```
{
    while(read(socket, buffer) != SUCCESS);
        process(buffer);
}
```

用户发起 read 请求，这时候如果 IO 数据没有准备好，那么 read 函数立即返回，并未读取到任何数据。这样是不会阻塞线程了，但是用户线程需要不停的去轮询是否有数据到来，当有数据到来时，用户程序再次发起读取数据操作，这个时候用户线程会发生阻塞（因为是同步的模型），直至数据从 IO 拷贝至 buffer 操作完成，用户线程继续处理读取到的数据。

在整个 IO 操作的过程中，虽然用户线程每次发起 IO 请求后可以立即返回，但是为了等到数据，仍需要不断地轮询、重复请求，消耗了大量的 CPU 的资源。一般很少直接使用这种模型，而是在其他 IO 模型中使用非阻塞 IO 这一特性。

### 2.3. 同步非阻塞式 IO / 事件驱动 IO / IO 多路复用

IO 多路复用是最常用的 IO 模型，像 Nginx，lighttd 等服务器软件都选用该模型。

IO 多路复用本质上也属于同步非阻塞 IO，它在其基础上，通过操作系统提供的多路复用器（如系统调用 select/poll/devpoll/epoll/kqueue/IOCP）实现在一个线程中监听多路通道数据 / 多个描述符的读写就绪情况，这样，多个描述符的 I/O 操作都能在一个线程内并发交替地顺序完成，这里的复用的指是复用同一个线程。虽是同步非阻塞，但严格地来讲，只是把阻塞点改变了位置。

IO 多路复用的操作流程如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/f8964dea87820cb2414727d92eae61e0.jpg)

伪代码：
```
{
    select(socket);
    while(1) {
        sockets = select();
        for(socket in sockets) {
            if(can_read(socket)) {
                read(socket, buffer);
                process(buffer);
            }
        }
    }
}
```

用户线程首先需要把需要监听的 socket 注册至 selector，然后 selector 负责去轮询各个 socket 通道是否有数据到来，一旦有数据到来，select 返回，最后用户程序读取 IO 数据，这个过程仍然是用户线程阻塞直至数据读取完成。在整个过程中，只在调用 select、poll、epoll 的时候才会阻塞，读写操作是不会阻塞的，从而整个进程或者线程就被充分利用起来。

IO 多路复用采用 **Reactor 设计模式**（事件驱动模式）:

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/65bad25a2277c4ab4028d661827a4132.jpg)

用户线程需要首先在 Reactor 中注册一个事件处理器，然后 Reactor（相当于上文提到的 selector）负责轮询各个通道是否有新的数据到来，当有新的数据到来时，Reactor 通过先前注册的事件处理器通知用户线程有数据可读，此时用户线程向内核发起读取 IO 数据的请求，用户线程阻塞直至数据读取完成。 

### 2.4. 异步非阻塞式 IO / 异步 IO

异步 IO 模型由 POSIX 规范定义，需要操作系统更强的支持，它采用 **Proactor 设计模式**，操作流程如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/76a2c346cbf2a69a4a50e39a0f8a4c20.jpg)

伪代码：
```
void UserCompletionHandler::handle_event(buffer) {
    process(buffer);
}
{
    aio_read(socket, new UserCompletionHandler);
}
```

跟 Reactor 模式一样，用户线程首先也需要向 Proactor 注册一个事件处理器，然后告诉内核要读取 IO 数据，这个时候用户线程就去做别的事情了，剩下监听 IO 数据以及 IO 数据的读取都由内核来完成，完成之后内核通过用户线程注册的事件处理器通知用户线程，“数据已经读取完成，你可以对这些数据做你自己的处理了”，最后用户线程拿到数据开始做自己的处理。

相比于 IO 多路复用模型，异步 IO 并不十分常用，不少高性能并发服务程序使用 IO 多路复用模型 + 多线程任务处理的架构基本可以满足需求，况且目前操作系统对异步 IO 的支持并非特别完善。

### 2.5. 信号驱动式 IO

信号驱动式 IO 模型用得比较少，操作流程如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/11c13a199d218fe184ceb2e4d42002bd.jpg)

为了使用该 I/O 模型，需要开启套接字的信号驱动 I/O 功能，并通过 sigaction 系统调用安装一个信号处理函数。sigaction 函数立即返回，我们的进程继续工作，即进程没有被阻塞。当数据报准备好时，内核会为该进程产生一个 SIGIO 信号，这样我们可以在信号处理函数中调用 recvfrom 读取数据报，也可以在主循环中读取数据报。无论如何处理 SIGIO 信号，这种模型的优势在于等待数据报到达期间不被阻塞。

相比异步 IO 模型，信号驱动 IO 是由内核通知用线程何时可以启动一个 IO 操作，而异步 IO 模型是由内核通知用户线程 IO 操作何时已完成。

### 2.6. 比较

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/17891efd2acda172e74d6be8764685be.jpg)

## 3. Refer Links

[BIO，NIO 和 AIO 模型介绍](https://blog.insanecoder.top/java-bio-nio-aio/)

[高性能 IO 模型浅析](http://www.cnblogs.com/fanzhidongyzby/p/4098546.html)

[I/O 多路复用技术（multiplexing）是什么？](https://www.zhihu.com/question/28594409)

[怎样理解阻塞非阻塞与同步异步的区别？](https://www.zhihu.com/question/19732473/answer/88599695)

[I/O 模型简述](http://www.coolblog.xyz/2018/02/08/IO%E6%A8%A1%E5%9E%8B%E7%AE%80%E8%BF%B0/)