- [IO 模型](#io-%E6%A8%A1%E5%9E%8B)
    - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [1.1. 阻塞和非阻塞](#11-%E9%98%BB%E5%A1%9E%E5%92%8C%E9%9D%9E%E9%98%BB%E5%A1%9E)
        - [1.2. 同步和异步](#12-%E5%90%8C%E6%AD%A5%E5%92%8C%E5%BC%82%E6%AD%A5)
    - [2. IO 模式](#2-io-%E6%A8%A1%E5%BC%8F)
        - [2.1. Reactor 模式](#21-reactor-%E6%A8%A1%E5%BC%8F)
        - [2.2. Proactor 模式](#22-proactor-%E6%A8%A1%E5%BC%8F)
        - [2.3. Actor 模式](#23-actor-%E6%A8%A1%E5%BC%8F)
    - [3. IO 模型](#3-io-%E6%A8%A1%E5%9E%8B)
        - [3.1. 同步阻塞式 IO](#31-%E5%90%8C%E6%AD%A5%E9%98%BB%E5%A1%9E%E5%BC%8F-io)
        - [3.2. 同步非阻塞式 IO](#32-%E5%90%8C%E6%AD%A5%E9%9D%9E%E9%98%BB%E5%A1%9E%E5%BC%8F-io)
        - [3.3. 同步非阻塞式 IO / 事件驱动 IO / IO 多路复用](#33-%E5%90%8C%E6%AD%A5%E9%9D%9E%E9%98%BB%E5%A1%9E%E5%BC%8F-io--%E4%BA%8B%E4%BB%B6%E9%A9%B1%E5%8A%A8-io--io-%E5%A4%9A%E8%B7%AF%E5%A4%8D%E7%94%A8)
            - [3.3.1. select](#331-select)
                - [3.3.1.1. API](#3311-api)
                - [3.3.1.2. 实现原理](#3312-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
                - [3.3.1.3. 缺陷](#3313-%E7%BC%BA%E9%99%B7)
            - [3.3.2. poll](#332-poll)
                - [3.3.2.1. API](#3321-api)
                - [3.3.2.2. 使用示例](#3322-%E4%BD%BF%E7%94%A8%E7%A4%BA%E4%BE%8B)
            - [3.3.3. epoll](#333-epoll)
                - [3.3.3.1. API](#3331-api)
                - [3.3.3.2. 实现原理](#3332-%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
                    - [3.3.3.2.1. 存储结构](#33321-%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84)
                    - [3.3.3.2.2. 解决拷贝问题](#33322-%E8%A7%A3%E5%86%B3%E6%8B%B7%E8%B4%9D%E9%97%AE%E9%A2%98)
                    - [3.3.3.2.3. 解决遍历问题](#33323-%E8%A7%A3%E5%86%B3%E9%81%8D%E5%8E%86%E9%97%AE%E9%A2%98)
                    - [3.3.3.2.4. 实现流程](#33324-%E5%AE%9E%E7%8E%B0%E6%B5%81%E7%A8%8B)
                - [3.3.3.3. 工作模式](#3333-%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F)
                    - [3.3.3.3.1. Level Triggered 模式](#33331-level-triggered-%E6%A8%A1%E5%BC%8F)
                    - [3.3.3.3.2. Edge Triggered 模式](#33332-edge-triggered-%E6%A8%A1%E5%BC%8F)
                - [3.3.3.4. 使用示例](#3334-%E4%BD%BF%E7%94%A8%E7%A4%BA%E4%BE%8B)
            - [3.3.4. /dev/pool](#334-devpool)
            - [3.3.5. kqueue](#335-kqueue)
        - [3.4. 异步非阻塞式 IO / 异步 IO](#34-%E5%BC%82%E6%AD%A5%E9%9D%9E%E9%98%BB%E5%A1%9E%E5%BC%8F-io--%E5%BC%82%E6%AD%A5-io)
        - [3.5. 信号驱动式 IO](#35-%E4%BF%A1%E5%8F%B7%E9%A9%B1%E5%8A%A8%E5%BC%8F-io)
        - [3.6. 比较](#36-%E6%AF%94%E8%BE%83)
    - [4. Refer Links](#4-refer-links)

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

NOTE: **在处理 IO 的时候，阻塞和非阻塞都是同步 IO，只有使用了特殊的 API ( 如 POSIX 的 aio_* 系列函数 ) 才是异步 IO**。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/cb68a4980c64f35d6358a290ffbf26e4.jpg)

## 2. IO 模式

### 2.1. Reactor 模式

Reactor 实现了一个被动的事件分离和分发模型，服务等待请求事件的到来，再通过不受间断的同步处理事件，从而做出反应。适用于同时接收多个服务请求，并且依次**同步**的处理它们的事件驱动程序。

一般过程：
1. 向事件分发器注册事件回调 
1. 事件发生 
1. 事件分发器调用之前注册的函数 
1. 在回调函数中读取数据，对数据进行后续处理 

eg: Reactor 将 handle 放到 select()，等待可写就绪，然后调用 write() 写入数据，写完处理后续逻辑。

Reactor 模型实例：libevent/libev/libuv/Event Library in Redis/ACE/Select/Epoll/ZeroMQ。

优点：
- Reactor 实现相对简单，对于耗时短的处理场景处理高效。
- 操作系统可以在多个事件源上等待，并且避免了多线程编程相关的性能开销和编程复杂性。
- 事件的串行化对应用是透明的，可以顺序的同步执行而不需要加锁。
- 事务分离，将与应用无关的多路分解和分配机制和与应用相关的回调函数分离开来。

缺点：
- Reactor 处理耗时长的操作会造成事件分发的阻塞，影响到后续事件的处理。

### 2.2. Proactor 模式

Proactor 实现了一个主动的事件分离和分发模型，这种设计允许多个任务并发的执行，从而提高吞吐量，且可执行耗时长的任务（各个任务间互不影响）。适用于**异步**接收和同时处理多个服务请求的事件驱动程序。

一般过程：
1. 向事件分发器注册事件回调 
1. 事件发生 
1. **操作系统读取数据，并放入应用缓冲区，然后通知事件分发器** 
1. 事件分发器调用之前注册的函数 
1. 在回调函数中对数据进行后续处理 

eg: Proactor 调用 aoi_write 后立刻返回，由内核负责写操作，写完后调用相应的回调函数处理后续逻辑。

Proactor 模型实例：Boost.Asio/IOCP。

优点：
- 相比 Reactor，Proactor 性能更高，能够处理耗时长的并发场景。

缺点：
- Proactor 实现逻辑复杂，依赖操作系统对异步的支持，但目前实现了纯异步操作的操作系统少，其中如 windows IOCP，但由于其 windows 系统用于服务器的局限性，目前应用范围较小；而 Unix/Linux 系统对纯异步的支持有限，因此应用事件驱动的主流还是通过 select/epoll 来实现。

### 2.3. Actor 模式

Actor 模型是一个概念模型，被称为高并发事务的终极解决方案。它定义了一系列系统组件应该如何动作和交互的通用规则，实体之通过消息通讯，各自处理自己的数据，适用于处理并发计算的场景。

Actors 一大重要特征在于 actors 之间相互隔离，它们并不互相共享内存。这点区别于上述的对象。也就是说，一个 actor 能维持一个私有的状态，并且这个状态不可能被另一个 actor 所改变。

Actor 模型 = 数据 + 行为 + 消息。

**Actor 模型内部的状态由自己的行为维护，外部线程不能直接调用对象的行为，必须通过消息才能激发行为，这样就保证 Actor 内部数据只有被自己修改**。

Proactor 模型实例：Erlang/Skynet/Akka。

## 3. IO 模型

在 UNIX 中，有 5 种可用的 IO 模型：
- 同步阻塞式 IO (Blocking IO)
- 同步非阻塞式 IO / 轮询 (Non-blocking IO)
- 同步非阻塞式 IO / 事件驱动 IO / IO 多路复用 (IO Multiplexing) 
- 异步非阻塞式 IO / 异步 IO (Asynchronous IO) 
- 信号驱动式 IO (Signal Driven IO) (SIGIO)

NOTE: [不存在 “异步阻塞式 IO” 的说法](https://www.zhihu.com/question/65519203/answer/233433548)。

### 3.1. 同步阻塞式 IO

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

### 3.2. 同步非阻塞式 IO

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

### 3.3. 同步非阻塞式 IO / 事件驱动 IO / IO 多路复用

IO 多路复用是最常用的 IO 模型，像 Nginx，lighttd 等服务器软件都选用该模型。

IO 多路复用本质上也属于同步非阻塞 IO，它在其基础上，通过操作系统提供的多路复用器（**为实现 IO 多路复用，不同的操作系统都为用户态空间提供了不同的系统调用，如 select/poll/devpoll/epoll/kqueue**）实现在一个线程中监听多路通道数据 / 多个描述符的读写就绪情况，这样，多个描述符的 I/O 操作都能在一个线程内并发交替地顺序完成，这里的**复用的指是复用同一个线程。虽是同步非阻塞，但严格地来讲，只是把阻塞点改变了位置**。

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

用户线程需要首先在 Reactor 中注册一个事件处理器，然后 Reactor（相当于上文提到的 Selector）负责轮询各个通道是否有新的数据到来，当有新的数据到来时，Reactor 通过先前注册的事件处理器通知用户线程有数据可读，此时用户线程向内核发起读取 IO 数据的请求，用户线程阻塞直至数据读取完成。 

#### 3.3.1. select

[select](https://linux.die.net/man/2/select) 是用于 IO 多路复用的一个系统调用函数，**几乎所有平台都提供了该系统调用**。在 C 程序中，它定义于 sys/select.h 或 unistd.h 中。

**select() 的时间复杂度为 O(n)。随着监控的 socket 集合的增加性能线性下降，因此 select 并不适合用于大并发场景**。

##### 3.3.1.1. API

```c
#include <sys/select.h>
int select(int nfds, fd_set *readfds, fd_set *writefds,
           fd_set *exceptfds, struct timeval *timeout);
```
调用后 select 函数会阻塞，直到有描述符就绪（有数据 可读、可写、或者有 except），或者超时（timeout 指定等待时间，如果立即返回设为 null 即可），函数返回。当 select 函数返回后，可以 通过遍历 fdset，来找到就绪的描述符。
- Parameter Description:
	- `nfds`: sets 的文件描述符的最大值。
	- `readfds`: fd_set type 类型，只读的描述符集。
	- `writefds`: fd_set type 类型，只写的描述符集。
	- `exceptfds`: fd_set type 类型，错误的描述符集。
	- `timeout`: 超时等待时间。

- Return Value:
	
	返回描述符集的个数，如果超时返回为 0，错误则返回 -1。

##### 3.3.1.2. 实现原理

select 的调用过程如下所示：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/9/57875098d36a1f413b5d1a490913004b.jpg)

执行原理：

1. **使用 copy_from_user 从用户空间拷贝 fd_set 到内核空间**。

1. 遍历所有 fd，调用其对应的回调函数（对于 socket，这个 poll 方法是 `sock_poll`，sock_poll 根据情况会调用到 tcp_poll,udp_poll 或者 datagram_poll）。以 tcp_poll 为例，其核心实现就是 `__pollwait()`。

1. __pollwait 的主要工作就是把 current（当前进程）挂到设备的等待队列中，不同的设备有不同的等待队列，对于 tcp_poll 来说，其等待队列是 sk->sk_sleep（注意把进程挂到等待队列中并不代表进程已经睡眠了）。**在设备收到一条消息（网络设备）或填写完文件数据（磁盘设备）后，会唤醒设备等待队列上睡眠的进程**，这时 current 便被唤醒了。

1. poll 方法返回时会返回一个描述读写操作是否就绪的 mask 掩码，根据这个 mask 掩码给 fd_set 赋值。

1. 如果遍历完所有的 fd，还没有返回一个可读写的 mask 掩码，则会调用 schedule_timeout 使调用 select 的进程（也就是 current）进入睡眠。当设备驱动发生自身资源可读写后，会唤醒其等待队列上睡眠的进程。如果超过一定的超时时间（schedule_timeout 指定），还是没人唤醒，则调用 select 的进程会重新被唤醒获得 CPU，进而重新遍历 fd，判断有没有就绪的 fd。

1. **把 fd_set 从内核空间拷贝到用户空间**。

##### 3.3.1.3. 缺陷

select 实现了对 IO 多路复用的支持，但其存在以下缺陷：

- 限制问题：select 支持的文件描述符数量（即被监控的 fds 集合）太小了，只有 1024。

- 性能问题：每次调用 select，都需要把 fd 集合从用户态**拷贝**到内核态，这个开销在 fd 很多时会很大。

- 性能问题：每次调用 select 都需要在内核**遍历**所有传递进来的 fd，这个开销在 fd 很多时也会很大。

#### 3.3.2. poll

对于 select 遗留的 3 个问题，[poll](https://linux.die.net/man/2/poll) 函数只解决了文件描述符的限制问题。poll 通过改变 fds 集合的描述方式，使用 pollfd 结构而不是 select 中的 fd_set 结构，使得 poll 支持的 fds 集合限制远大于 select 的 1024。

**poll() 的时间复杂度与 select() 相同，都为 O(n)。随着监控的 socket 集合的增加性能线性下降，因此 poll 同样不适合用于大并发场景**。

##### 3.3.2.1. API

```c
#include <sys/poll. h>
int poll(struct pollfd *fds,nfds_t nfds, int timeout);
```

不同于 select 使用三个位图来表示三个 fdset 的方式，poll 使用一个 pollfd 的指针实现。pollfd 结构定义如下：
```c
struct pollfd {
    int fd;         		/* 文件描述符 */
    short events;         	/* 等待的事件 */
    short revents;       	/* 实际发生了的事件 */
};
```
每一个 pollfd 结构体指定了一个被监视的文件描述符，可以传递多个结构体，指示 poll() 监视多个文件描述符。每个结构体的 events 域是监视该文件描述符的事件掩码，由用户来设置这个域。revents 域是文件描述符的操作结果事件掩码。内核在调用返回时设置这个域。events 域中请求的任何事件都可能在 revents 域中返回。

##### 3.3.2.2. 使用示例

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <assert.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <poll.h>

#define MAX_FD_NUM 1024  
#define MAXLEN 1024  

int buf_len = 0;

int main(int argc,char* argv[])
{
    int i = 0;
    printf("server start up\n");

    if(argc <= 2)
    {
        printf("usage:%s ip port\n",basename(argv[0]));
        return 1;
    }

    const char* ip = argv[1];
    int port = atoi(argv[2]);

    // 创建 socket  (TIP/IP 协议族，流式 socket)  
    int server_sockfd = socket(PF_INET,SOCK_STREAM,0);

    //TCP/IP 协议族的 socket 地址结构体  
    struct sockaddr_in server_addr;
    bzero(&server_addr, sizeof(server_addr));
    server_addr.sin_family = AF_INET;          //TCP/IPv4 的地址族  
    // 将 IP 地址字符串转换为二进制的整数并赋给 addr.sin_server_addr  
    inet_pton(AF_INET, ip, &server_addr.sin_addr);      
    // server_add.sin_addr = inet_addr(ip);
    // 端口，host to net，将主机字节序（小端）转换为网络字节序（大端）  
    server_addr.sin_port = htons(port);      

    // 将文件描述符 sock 和 socket 地址关联，仅服务端需要，客户端自动绑定地址  
    // 注意需要强制转换为 struct sockaddr*  
    int ret = bind(server_sockfd, 
        (struct sockaddr*)&server_addr, sizeof(server_addr));
    assert(ret != -1);

    // 监听  
    ret = listen(server_sockfd, MAX_FD_NUM - 1);
    assert(ret != -1);

    // 等待客户端做些连接等相关工作  
    sleep(3);

    // 客户端地址信息  
    struct sockaddr_in client_addr;
    socklen_t client_addr_len = sizeof(struct sockaddr_in);

    //poll fds  
    struct pollfd pollfdArry[MAX_FD_NUM];
    for (i = 0; i < MAX_FD_NUM; ++i)
    {
        pollfdArry[i].fd = -1;
    }

    //insert the server socket fd  
    pollfdArry[0].fd = server_sockfd;
    pollfdArry[0].events = POLLIN;

    int cur_fd_num = 1;
    char buf[MAXLEN]={0};

    while (1)
    {
        int nready = poll(pollfdArry, cur_fd_num, -1);

        //server socket fd  
        if(pollfdArry[0].revents & POLLIN)
        {
            if(cur_fd_num > MAX_FD_NUM)
            {
                printf("socket num to much\n");
            }
            else
            {
                // 接受连接，并将被接受的远端 sock 地址信息保存在第二个参数中  
                // 只是从监听队列取出连接，即使客户端已经断开网络连接也会 accept 成功  
                int client_sockfd = accept(server_sockfd,
                    (struct sockaddr*)&client_addr,&client_addr_len);

                if(client_sockfd < 0)
                {
                   perror("accept");
                }
                else
                {
                    //inet_ntoa(struct addr_in) 将 IP 地址转换为字符串并返回  
                    printf("accept client_addr %s\n",
                        inet_ntoa(client_addr.sin_addr));

                    for(i=0;i<MAX_FD_NUM;++i)
                    {
                        if(pollfdArry[i].fd == -1)
                        {
                            pollfdArry[i].fd = client_sockfd;
                            pollfdArry[i].events = POLLIN;
                            cur_fd_num++;
                            break;
                        }
                    }
                }
            }

            if(--nready <= 0)
            {
                continue;
            }
        }

        for(i=0;i<MAX_FD_NUM;++i)
        {
            if(pollfdArry[i].fd < 0)
            {
                continue;
            }

            // 处理读
            if(pollfdArry[i].revents & (POLLIN |POLLERR))
            {

                int n = recv(pollfdArry[i].fd,buf,MAXLEN,0);

                if(n < 0)
                {
                    if(ECONNRESET == errno)
                    {
                        close(pollfdArry[i].fd);
                        pollfdArry[i].fd = -1;
                        cur_fd_num--;
                    }
                    else
                    {
                        perror("recv");
                    }
                }
                else if(n == 0)
                {
                    close(pollfdArry[i].fd);
                    pollfdArry[i].fd = -1;
                    cur_fd_num--;
                }
                else
                {
                    pollfdArry[i].events = POLLIN | POLLOUT | POLLERR;
                    buf_len = n;
                }

            }

            // 处理写
            else if (pollfdArry[i].revents & POLLOUT)
            {
                //!> 读出来的写进去
                write(pollfdArry[i].fd, buf, buf_len);        
            }

            if(--nready)
            {
                break;
            }
        }
    }

    // 关闭连接，实际只是 socket 的引用 -1, 必须引用为 0 才会真正关闭  
    for(i=0;i<MAX_FD_NUM;++i)
    {
        if(pollfdArry[i].fd != -1)
        {
            close(pollfdArry[i].fd);
        }
    }

    return 0;
}
```

#### 3.3.3. epoll

epoll 从 **Linux 2.5.44** 开始引入，是 select/poll 的升级版本，其**设计目的旨在取代原有的 POSIX select() 与 poll() 系统调用，解决 select/poll 中存在的缺陷**。

**epoll 函数的时间复杂度都为 O(logn)**。

##### 3.3.3.1. API

epoll 实际上是一个模块，由 3 个系统调用组成：[epoll_create](https://linux.die.net/man/2/epoll_create)、[epoll_ctl](https://linux.die.net/man/2/epoll_ctl) 和 [epoll_wait](https://linux.die.net/man/2/epoll_wait)。

- epoll_create
	```c
	#include <sys/epoll.h>
	int epoll_create(int size); 
	```
	创建一个 epoll 的句柄，size 用来告诉内核这个监听的数目一共有多大，它并不是限制了 epoll 所能监听的描述符最大个数，只是对内核初始分配内部数据结构的一个建议。
	
	需要注意的是，当创建好 epoll 句柄后，它就是会占用一个 fd 值，在 Linux 下如果查看 /proc/ 进程 id/fd/，是能够看到这个 fd 的，所以在使用完 epoll 后，必须调用 close() 关闭，否则可能导致 fd 被耗尽。

- epoll_ctl
	```c
	#include <sys/epoll.h>
	int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event); 
	```
	epoll_ctl 是 epoll 的事件注册函数。
	- Parameter Description:
		- 第一个参数是 epoll_create() 的返回值。
		- 第二个参数表示动作，用三个宏来表示：
			- `EPOLL_CTL_ADD`: 注册新的 fd 到 epfd 中。
			- `EPOLL_CTL_MOD`: 修改已经注册的 fd 的监听事件。
			- `EPOLL_CTL_DEL`: 从 epfd 中删除一个 fd。
		- 第三个参数是需要监听的 fd，就是我们的 socket。
		- 第四个参数是告诉内核需要监听什么事，struct epoll_event 结构如下：
			```c
			typedef union epoll_data {
					void *ptr;
					int fd;
					__uint32_t u32;
					__uint64_t u64;
			} epoll_data_t;

			struct epoll_event {
					__uint32_t events; /* Epoll events */
					epoll_data_t data; /* User data variable */
			};
			```
			events 可以是以下几个宏的集合：（前三种是最常见的)
			- `EPOLLIN`: 表示对应的文件描述符可以读（包括对端 SOCKET 正常关闭）。
			- `EPOLLOUT`: 表示对应的文件描述符可以写。
			- `EPOLLERR`: 表示对应的文件描述符发生错误。
			- `EPOLLPRI`: 表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）。
			- `EPOLLHUP`: 表示对应的文件描述符被挂断。
			- `EPOLLET`: 将 EPOLL 设为边缘触发 (Edge Triggered) 模式，这是相对于水平触发 (Level Triggered) 来说的。
			- `EPOLLONESHOT`: 只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个 socket 的话，需要再次把这个 socket 加入到 EPOLL 队列里。

	- Return Value
		
		When successful, epoll_ctl() returns zero. When an error occurs, epoll_ctl() returns -1 and errno is set appropriately.

- epoll_wait
	```c
	#include <sys/epoll.h>
	int epoll_wait(int epfd, struct epoll_event *events,
								int maxevents, int timeout);
	```
	等待事件的产生。参数 events 用来从内核得到事件的集合，maxevents 告之内核这个 events 有多大，这个 maxevents 的值不能大于创建 epoll_create() 时的 size，参数 timeout 是超时时间（毫秒，0 会立即返回，-1 是永久阻塞）。
	
	该函数返回需要处理的事件数目，如返回 0 表示已超时。

	- Return 
		
		When successful, epoll_wait() returns the number of file descriptors ready for the requested I/O, or zero if no file descriptor became ready during the requested timeout milliseconds. When an error occurs, epoll_wait() returns -1 and errno is set appropriately.

##### 3.3.3.2. 实现原理

###### 3.3.3.2.1. 存储结构

为解决 select/poll 遗留的性能问题，epoll 模块中主要定义了 2 种结构：
- 存放文件资源描述符 (fds) 的数据结构：**哈希表 (Linux < 2.6); 红黑树 (Linux > 2.6)**.
- 存放就绪状态的文件资源描述符 (fds) 的数据结构：一个**双向链表 (ready_list)**.

由于 epoll 通过 epoll_ctl 来对监控的 fds 集合来进行增、删、改，那么必会涉及到 fd 的快速查找问题，因此**需要一个低时间复杂度的增、删、改、查的数据结构来组织被监控的 fds 集合**：
- 在 linux 2.6.8 之前的内核，epoll 使用 HashTable 来组织 fds 集合，因此在创建 epoll fd 的时候，epoll 需要初始化 hash 的大小。于是 epoll_create(int size) 有一个参数 size，以便内核根据 size 的大小来分配 hash 的大小。
- 在 linux 2.6.8 之后的内核中，epoll 使用红黑树来组织监控的 fds 集合，因此 `epoll_create(int size)` 的参数 size 实际上已经没有意义了。TODO: 为什么要用红黑树代替哈希表？

###### 3.3.3.2.2. 解决拷贝问题

对于 IO 多路复用，有两件事是必须要做的（对于监控可读事件而言)：
1. 准备好需要监控的 fds 集合。
1. 探测并返回 fds 集合中哪些 fd 可读了。

在 select / poll 函数中，每次调用 select 或 poll 都在重复地准备（集中处理) 整个需要监控的 fds 集合。然而对于频繁调用的 select / poll 而言，fds 集合的变化频率要低得多，完全没必要每次都重新准备（集中处理) 整个 fds 集合。

因此，**epoll 将高频调用的 epoll_wait 和低频的 epoll_ctl 隔离开**：
- 对于 epoll_ctl 调用，通过 (EPOLL_CTL_ADD、EPOLL_CTL_MOD、EPOLL_CTL_DEL) 三个操作来分散对需要监控的 fds 集合的修改，做到了有变化才变更，**将 select 或 poll 高频、大块内存拷贝（集中处理) 变成 epoll_ctl 的低频、小块内存的拷贝（分散处理)，避免了大量的内存拷贝**。
- 对于 epoll_wait 调用，针对高频的可读就绪的 fd 集合返回的拷贝问题，**epoll 通过内核与用户空间 mmap（内存映射) 同一块内存来解决，使得这块物理内存对内核和对用户均可见，减少用户态和内核态之间的数据交换**。

###### 3.3.3.2.3. 解决遍历问题

在 select/poll 函数中，每次返回后我们都需要遍历所有注册的文件描述符，来获取其中的少数就绪 fd，大量消耗了性能。为了做到只遍历就绪的 fd，我们需要有个地方来组织那些已经就绪的 fd。

**为此，epoll 巧妙的引入一个中间层解决了大量监控 socket 的无效遍历问题 -- 使用一个双向链表 (ready_list) 用于存储准备就绪的事件**，当 epoll_wait 调用时，仅仅观察这个 list 链表里有没有数据即可。有数据就返回，没有数据就 sleep，等到 timeout 时间到后即使链表没数据也返回。

那么，这个就绪链表是怎么维护的呢？
- **当我们执行 epoll_create 时，在创建红黑树 / 哈希表的同时还创建了就绪链表**。
- **当我们执行 epoll_ctl 时**，除了把 socket 放到 epoll 文件系统里文件对象对应的哈希表 / 红黑树上之外，还**会给内核中断处理程序注册一个回调函数，告诉内核，如果这个句柄的中断到了，就把它放到准备就绪链表里**。因此，当一个 socket 上有数据到了，内核在把网卡上的数据 copy 到内核中后就来把 socket 插入到准备就绪链表里了。TODO:

###### 3.3.3.2.4. 实现流程

1. 执行 epoll_create 时，创建了哈希表 / 红黑树和就绪链表。

1. 执行 epoll_ctl 时，如果增加 socket 句柄，则检查在哈希表 / 红黑树中是否存在，存在立即返回，不存在则添加到树干上，然后向内核注册回调函数，用于当中断事件来临时，向准备就绪链表中插入数据。

1. 执行 epoll_wait 时，检查准备就绪链表里的数据，有数据就直接返回；没有数据就 sleep，等到 timeout 时间到后即使链表没数据也返回。

##### 3.3.3.3. 工作模式

###### 3.3.3.3.1. Level Triggered 模式

**LT 模式（水平触发模式）下，若一个句柄上的事件一次没有处理完，会在以后调用 epoll_wait 时每一次都返回这个句柄**。

- .socket 接收缓冲区不为空，有数据可读，则读事件一直触发。

- .socket 发送缓冲区不满可以继续写入数据，则写事件一直触发。

- 符合思维习惯，epoll_wait 返回的事件就是 socket 的状态。

###### 3.3.3.3.2. Edge Triggered 模式

**ET 模式（边缘触发模式）下，若一个句柄上的事件一次没有处理完，在以后调用 epoll_wait 时不会再返回这个句柄**。

- .socket 的接收缓冲区状态变化时触发读事件，即空的接收缓冲区刚接收到数据时触发读事件。

- .socket 的发送缓冲区状态变化时触发写事件，即满的缓冲区刚空出空间时触发读事件。

- 仅在缓冲区状态变化时触发事件，比如数据缓冲去从无到有的时候（不可读 - 可读)。

**ET 模式在很大程度上减少了 epoll 事件被重复触发的次数，因此效率要比 LT 模式高**。epoll 工作在 ET 模式的时候，必须使用非阻塞套接口，以避免由于一个文件句柄的阻塞读 / 阻塞写操作把处理多个文件描述符的任务饿死。

###### 3.3.3.3.3. 实现原理

LT, ET 这件事怎么做到的呢？当一个 socket 句柄上有事件时，内核会把该句柄插入上面所说的准备就绪 list 链表，这时我们调用 epoll_wait，会把准备就绪的 socket 拷贝到用户态内存，然后清空准备就绪 list 链表，最后，epoll_wait 干了件事，就是检查这些 socket，如果不是 ET 模式（就是 LT 模式的句柄了），并且这些 socket 上确实有未处理的事件时，又把该句柄放回到刚刚清空的准备就绪链表了。所以，非 ET 的句柄，只要它上面还有事件，epoll_wait 每次都会返回。而 ET 模式的句柄，除非有新中断到，即使 socket 上的事件没有处理完，也是不会次次从 epoll_wait 返回的。（从上面这段，可以看出，LT 还有个回放的过程，低效了）

##### 3.3.3.4. 使用示例

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <stdlib.h>
#include <assert.h>
#include <stdio.h>
#include <string.h>
#include <errno.h>
#include <sys/epoll.h>

#define MAX_FD_NUM 1024  
#define MAXLEN 1024  

int buf_len = 0;

int main(int argc,char* argv[])
{
    int i = 0;
    printf("server start up\n");

    if(argc <= 2)
    {
        printf("usage:%s ip port\n",basename(argv[0]));
        return 1;
    }

    const char* ip = argv[1];
    int port = atoi(argv[2]);

    int server_sockfd = socket(PF_INET,SOCK_STREAM,0);

    struct sockaddr_in server_addr;
    bzero(&server_addr, sizeof(server_addr));
    server_addr.sin_family = AF_INET;
    inet_pton(AF_INET, ip, &server_addr.sin_addr);
    server_addr.sin_port = htons(port);

    int ret = bind(server_sockfd, (struct sockaddr*)&server_addr, sizeof(server_addr));
    assert(ret != -1);

    ret = listen(server_sockfd, MAX_FD_NUM - 1);
    assert(ret != -1);

    struct sockaddr_in client_addr;
    socklen_t client_addr_len = sizeof(struct sockaddr_in);

    // 创建一个 epfd，并且把 server_sockfd 注册到这个 epfd 上。
    int epfd = epoll_create(1024);
    struct epoll_event ev,events[20];
    ev.data.fd = server_sockfd;
    ev.events = EPOLLIN;
    epoll_ctl(epfd, EPOLL_CTL_ADD, server_sockfd, &ev);

    int cur_fd_num = 1;
    char buf[MAXLEN]={0};

    while (1) {
        // nReady 就是 events 数组的长度。
        int nready = epoll_wait(epfd, events, 20, 50);

        int i = 0;
        for (; i < nready; i++) {
            if (events[i].data.fd == server_sockfd) {
                int client_sockfd = accept(server_sockfd,(struct sockaddr*)&client_addr,&client_addr_len);

                if(client_sockfd < 0) {
                    perror("accept");
                }
                else {
                    printf("accept client_addr %s\n",inet_ntoa(client_addr.sin_addr));
                    ev.data.fd = client_sockfd;
                    ev.events=EPOLLIN;
                    epoll_ctl(epfd, EPOLL_CTL_ADD, client_sockfd, &ev);
                }
            }
            else if (events[i].events & EPOLLIN) {
                int connfd = events[i].data.fd;
                int n = recv(connfd, buf, MAXLEN, 0);
                if(n < 0) {
                    if(ECONNRESET == errno) {
                        close(connfd);
                        epoll_ctl(epfd, EPOLL_CTL_DEL, connfd, 0);
                    }
                    else {
                        perror("recv");
                    }
                }

                printf("receive %s", buf);
                buf_len = n;

                ev.data.fd = connfd;
                ev.events = EPOLLOUT;
                epoll_ctl(epfd, EPOLL_CTL_MOD, connfd, &ev);
            }
            else if (events[i].events & EPOLLOUT) {
                int connfd = events[i].data.fd;
                write(connfd, buf, buf_len);

                ev.data.fd = connfd;
                ev.events = EPOLLIN;
                epoll_ctl(epfd, EPOLL_CTL_MOD, connfd, &ev);
            }
        }
    }

    return 0;
}
```

#### 3.3.4. /dev/pool

以 Solaris 平台为主

#### 3.3.5. kqueue

以 BSD 平台为主

### 3.4. 异步非阻塞式 IO / 异步 IO

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

**相比于 IO 多路复用模型，异步 IO 并不十分常用，不少高性能并发服务程序使用 IO 多路复用模型 + 多线程任务处理的架构基本可以满足需求，况且目前操作系统对异步 IO 的支持并非特别完善**。

目前常用的异步 IO 分两种：POSIX IO 和 libaio：
- POSIX IO 是 glibc 实现和维护的使用线程 + 阻塞调用来模拟的异步 IO，不需要内核支持。这种异步 IO 的实现方式占用线程资源而且受可用线程的数量限制。
- [libaio](http://oss.oracle.com/projects/libaio-oracle/) 由 Oracle 维护，是真正的原生的内核级别的异步 IO，IO 请求完全由底层自由调度。因此，效率较前者能高出很多。需要注意的是，linux 内核 2.6 版本以后有了对内核级别的异步 IO 支持，另外，想要使用该种方式的文件必须支持以 O_DIRECT 标志打开，然而并不是所有的文件系统都支持。如果你没有使用 O_DIRECT 打开文件，它可能仍然“工作”，但它可能不是异步完成的，而是变为了阻塞的。

TODO:
- POSIX AIO
	
	[使用异步 I/O 大大提高应用程序的性能](https://www.ibm.com/developerworks/cn/linux/l-async/index.html)

- libevent

- Windows IOCP

  [基于 Proactor 模式的 IOCP 和基于 Reactor 模式的 epoll/kqueue 哪个效率更高？](https://www.zhihu.com/question/27008712)

### 3.5. 信号驱动式 IO

信号驱动式 IO 模型用得比较少，操作流程如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/11c13a199d218fe184ceb2e4d42002bd.jpg)

为了使用该 I/O 模型，需要开启套接字的信号驱动 I/O 功能，并通过 sigaction 系统调用安装一个信号处理函数。sigaction 函数立即返回，我们的进程继续工作，即进程没有被阻塞。当数据报准备好时，内核会为该进程产生一个 SIGIO 信号，这样我们可以在信号处理函数中调用 recvfrom 读取数据报，也可以在主循环中读取数据报。无论如何处理 SIGIO 信号，这种模型的优势在于等待数据报到达期间不被阻塞。

相比异步 IO 模型，信号驱动 IO 是由内核通知用线程何时可以启动一个 IO 操作，而异步 IO 模型是由内核通知用户线程 IO 操作何时已完成。

### 3.6. 比较

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/6/1/17891efd2acda172e74d6be8764685be.jpg)

## 4. Refer Links

[BIO，NIO 和 AIO 模型介绍](https://blog.insanecoder.top/java-bio-nio-aio/)

[高性能 IO 模型浅析](http://www.cnblogs.com/fanzhidongyzby/p/4098546.html)

[I/O 多路复用技术（multiplexing）是什么？](https://www.zhihu.com/question/28594409)

[怎样理解阻塞非阻塞与同步异步的区别？](https://www.zhihu.com/question/19732473/answer/88599695)

[I/O 模型简述](http://www.coolblog.xyz/2018/02/08/IO%E6%A8%A1%E5%9E%8B%E7%AE%80%E8%BF%B0/)

[Wikipedia epoll](https://zh.wikipedia.org/wiki/epoll)

[Wikipedia select](https://zh.wikipedia.org/wiki/Select_(Unix))

[Wikipedia IOCP](https://zh.wikipedia.org/wiki/IOCP)

[大话 Select、Poll、Epoll](https://cloud.tencent.com/developer/article/1005481)

[linux 下非阻塞 io 库 epoll](https://zhuanlan.zhihu.com/p/27050330)

[谈谈 epoll 实现原理](http://luodw.cc/2016/01/24/epoll/)

[linux 内核 poll/select/epoll 实现剖析](http://watter1985.iteye.com/blog/1614039)

[epoll 或者 kqueue 的原理是什么？](https://www.zhihu.com/question/20122137)

[IO 设计模式：Actor、Reactor、Proactor](https://www.cnblogs.com/losophy/p/9202815.html)

[为什么 Actor 模型是高并发事务的终极解决方案？](https://www.jdon.com/45728)

[如何深刻理解 reactor 和 proactor？](https://www.zhihu.com/question/26943938)

[epoll 的边沿触发模式 (ET) 真的比水平触发模式 (LT) 快吗？](https://www.zhihu.com/question/20502870)