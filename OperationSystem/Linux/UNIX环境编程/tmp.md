- [OTHERS](#others)
  - [1. UNIX 信号机制](#1-unix-信号机制)
  - [2. 可重入函数 & 不可重入函数](#2-可重入函数--不可重入函数)
  - [3. Linux C 头文件](#3-linux-c-头文件)
  - [4. libc & glibc & glib](#4-libc--glibc--glib)
  - [5. Core Dump](#5-core-dump)
  - [6. Segmentation fault](#6-segmentation-fault)
  - [7. System V & BSD & Posix](#7-system-v--bsd--posix)
  - [8. 绑定 CPU](#8-绑定-cpu)
    - [8.1. 进程绑定 CPU](#81-进程绑定-cpu)
      - [8.1.1. taskset 命令](#811-taskset-命令)
      - [8.1.2. sched_setaffinity 系统调用](#812-sched_setaffinity-系统调用)
      - [8.1.3. cpuset 命令](#813-cpuset-命令)
    - [8.2. 线程绑定 CPU](#82-线程绑定-cpu)
  - [9. 调试](#9-调试)
    - [9.1. ldd](#91-ldd)
  - [10. Refer Links](#10-refer-links)

# OTHERS

TODO:

## 1. UNIX 信号机制

https://www.cnblogs.com/suzhou/p/4488618.html

https://blog.csdn.net/thanksgining/article/details/41824475

https://www.google.com/search?q=unix+%E4%BF%A1%E5%8F%B7+%E5%8E%9F%E7%90%86

## 2. 可重入函数 & 不可重入函数

https://www.cnblogs.com/parrynee/archive/2010/01/29/1659071.html

https://blog.csdn.net/DLUTBruceZhang/article/details/8817587

所谓可重入是指一个可以被多个任务调用的过程，任务在调用时不必担心数据是否会出错。不可重入函数在实时系统设计中被视为不安全函数。

如何写出可重入的函数？在函数体内不访问那些全局变量，不使用静态局部变量，坚持只使用缺省态（auto）局部变量，写出的函数就将是可重入的。如果必须访问全局变量，记住利用互斥信号量来保护全局变量。或者调用该函数前关中断，调用后再开中断。

可重入函数可以被一个以上的任务调用，而不必担心数据被破坏。**可重入函数任何时候都可以被中断，一段时间以后又可以运行，而相应的数据不会丢失**。可重入函数或者只使用局部变量，即保存在 CPU 寄存器中或堆栈中；或者使用予以保护的全局变量。

## 3. Linux C 头文件

https://www.cnblogs.com/myitm/archive/2012/12/25/2832347.html

https://segmentfault.com/q/1010000007410418

https://www.zhihu.com/question/22209705

https://www.zhihu.com/question/21914131

> math.h 应该是声明，实现应该在 libm.so 中，然后你可以查找一下 libm.so 包的包名，发现它位于 libc6-dev 这个包，然后查找对应的源代码，如果是 Debian/Ubuntu 可以用 apt-get source libc6-dev 来下载源代码到当前目录，之后便可以查看源代码了。

![image](http://img.cdn.firejq.com/jpg/2018/7/10/e34964015714a67c399a4dd11c303efd.jpg)

## 4. libc & glibc & glib

https://blog.csdn.net/yasi_xi/article/details/9899599

## 5. Core Dump

https://www.zhihu.com/question/28521838

https://www.cnblogs.com/hazir/p/linxu_core_dump.html

http://km.oa.com/articles/show/131498?kmref=search&from_page=1&no=1

## 6. Segmentation fault

http://www.cnblogs.com/panfeng412/archive/2011/11/06/segmentation-fault-in-linux.html

## 7. System V & BSD & Posix

https://blog.csdn.net/qq_29344757/article/details/78657874

## 8. 绑定 CPU

https://www.zhihu.com/question/25520997

所谓绑核，其实就是设定某个进程 / 线程与某个 CPU 核的亲和力（affinity）。设定以后，Linux 调度器就会让这个进程 / 线程只在所绑定的核上面去运行。但并不是说该进程 / 线程就独占这个 CPU 的核，其他的进程 / 线程还是可以在这个核上面运行的。如果想要实现某个进程 / 线程独占某个核，就要使用 cpuset 命令去实现。其实，很多情况下，为了提高性能，Linux 调度器会自动的实现尽量让某个进程 / 线程在同样的 CPU 上去运行。所以，除非必须，我们没有必要显式的去进程绑核操作。

线程绑定的主要目的是提高线程访问 cpu 的 cache（缓存）命中率，从而提高程序的并行性能。线程绑定的并行优化程度和服务器架构有密切关系。

传统的多核运算是使用 SMP(Symmetric Multi-Processor ) 模式：将多个处理器与一个集中的存储器和 I/O 总线相连。所有处理器只能访问同一个物理存储器，因此 SMP 系统有时也被称为一致存储器访问（UMA）结构体系，一致性意指无论在什么时候，处理器只能为内存的每个数据保持或共享唯一一个数值。很显然，SMP 的缺点是可伸缩性有限，因为在存储器和 I/O 接口达到饱和的时候，增加处理器并不能获得更高的性能。

NUMA 模式是一种分布式存储器访问方式，处理器可以同时访问不同的存储器地址，大幅度提高并行性。 NUMA 模式下，处理器被划分成多个"节点"（node）， 每个节点被分配有的本地存储器空间。 所有节点中的处理器都可以访问全部的系统物理存储器，但是访问本节点内的存储器所需要的时间，比访问某些远程节点内的存储器所花的时间要少得多。因此 NUMA 架构相比 SMP 架构上使用线程绑定的方式更能提高并行效率。

### 8.1. 进程绑定 CPU

#### 8.1.1. taskset 命令

https://blog.csdn.net/SilentOB/article/details/73123792

https://www.cnblogs.com/liuhao/archive/2012/06/21/2558069.html

#### 8.1.2. sched_setaffinity 系统调用

sched_setaffinity 系统调用，可以指定进程运行的 CPU 实例。

https://linux.die.net/man/2/sched_setaffinity

> The affinity mask is actually a per-thread attribute that can be adjusted independently for each of the threads in a thread group. The value returned from a call to gettid(2) can be passed in the argument pid. Specifying pid as 0 will set the attribute for the calling thread, and passing the value returned from a call to getpid(2) will set the attribute for the main thread of the thread group. (If you are using the POSIX threads API, then use pthread_setaffinity_np(3) instead of sched_setaffinity().)

https://stackoverflow.com/questions/5578965/if-i-do-sched-setaffinity-in-a-process-do-the-threads-spawned-by-it-get-affecte

> A call to sched_setaffinity() affects only a single thread. A thread created with pthread_create() inherits the CPU affinity mask of its parent.
>
> This means that if you change the affinity of the current thread after creating other threads, their affinity will remain the default; but if you do it in the reverse order, they will inherit the altered affinity set.

#### 8.1.3. cpuset 命令

### 8.2. 线程绑定 CPU

线程亲和性的设置和获取主要通过 pthread 的两个函数来实现：
```cpp
int pthread_setaffinity_np(pthread_t thread, size_t cpusetsize，const cpu_set_t *cpuset);
int pthread_getaffinity_np(pthread_t thread, size_t cpusetsize, cpu_set_t *cpuset);
```

## 9. 调试

### 9.1. ldd

https://www.nowcoder.com/questionTerminal/b9aa6f39ea3f47d0a0d409b6eceb2eee?source=relative

## 10. Refer Links