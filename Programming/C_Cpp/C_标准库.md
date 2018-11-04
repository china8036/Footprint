- [C 标准库](#c-%E6%A0%87%E5%87%86%E5%BA%93)
    - [1. 基础概念](#1-%E5%9F%BA%E7%A1%80%E6%A6%82%E5%BF%B5)
    - [2. 时间操作](#2-%E6%97%B6%E9%97%B4%E6%93%8D%E4%BD%9C)
    - [3. 暂停](#3-%E6%9A%82%E5%81%9C)
        - [3.1. 睡眠 / 挂起 / 闲等待](#31-%E7%9D%A1%E7%9C%A0--%E6%8C%82%E8%B5%B7--%E9%97%B2%E7%AD%89%E5%BE%85)
            - [3.1.1. sleep](#311-sleep)
            - [3.1.2. usleep](#312-usleep)
            - [3.1.3. nanosleep](#313-nanosleep)
            - [3.1.4. ssleep / msleep](#314-ssleep--msleep)
        - [3.2. 忙等待](#32-%E5%BF%99%E7%AD%89%E5%BE%85)
            - [3.2.1. udelay](#321-udelay)
            - [3.2.2. mdelay](#322-mdelay)
            - [3.2.3. ndelay](#323-ndelay)
    - [4. Refer Links](#4-refer-links)

# C 标准库

## 1. 基础概念

- C 标准库是在操作系统 API 上加入独特的算法封装成标准接口的库，使用 C 标准库可以屏蔽底层实现细节，比如 fopen 这样的函数，在 Windows 上通过调用 CreateFileEx 实现，在 linux 上通过调用 open 系统调用实现。不仅是包装，还在上层使用独特的算法提供了用户态缓冲区的功能。

  ![image](http://img.cdn.firejq.com/jpg/2018/7/15/93c9bcf552eebbcbfd9cddc468d772ce.jpg)

- glibc && c standard library (cstdlib) && c++ standard library

  - C 标准库

    C 标准库也称为 ISO C 库，是用于完成诸如输入 / 输出处理、字符串处理、内存管理、数学计算和许多其他操作系统服务等任务的宏、类型和函数的集合。它是在 C 标准中（例如 C11 标准）中定义的。其内容分布在不同的头文件中，比如上面我所提到的 math.h。

  - C++ 标准库
    
    和 C 标准库的概念类似，但仅针对 C ++。C++ 标准库是一组 C++ 模板类，它提供了通用的编程数据结构和函数，如链表、堆、数组、算法、迭代器和任何其他你可以想到的 C++ 组件。C ++ 标准库也包含了 C 标准库，并在 C++ 标准中进行了定义（例如 C++ 11 标准）。

## 2. 时间操作

https://blog.csdn.net/CAIYUNFREEDOM/article/details/75388111

获取微秒级时间：
```cpp
long getCurrentTime()
{
    struct timeval tv{};
    gettimeofday(&tv, NULL);
    return tv.tv_sec * 1000000 + tv.tv_usec;
}
```

获取纳秒级时间：
<!-- TODO:https://www.cnblogs.com/kekukele/p/3662816.html -->
```cpp
long getCurrentTime()
{
    struct timespec tn{};
    clock_gettime(CLOCK_REALTIME, &tn);
    return tn.tv_sec * 1000000000 + tn.tv_nsec;
}
```

## 3. 暂停

### 3.1. 睡眠 / 挂起 / 闲等待

进程被挂起后：
- 进程将暂停执行。
- 进程会释放 CPU，因此挂起过程会涉及上下文切换。若原本处于就绪状态，则该进程此时暂不接受调度。
- 挂起的进程通过“对换”技术被换出到外存（磁盘）中。

#### 3.1.1. sleep

http://man7.org/linux/man-pages/man3/sleep.3.html

https://stackoverflow.com/questions/175882/whats-the-algorithm-behind-sleep

https://en.wikipedia.org/wiki/Sleep_(system_call)

> On Linux, sleep() is implemented via nanosleep.

Linux 中并没有提供系统调用 sleep()，sleep() 是在库函数中通过 nanosleep 实现的。

> On some systems, sleep() may be implemented using alarm() and SIGALRM (POSIX.1 permits this); mixing calls to alarm() and sleep() is a bad idea.
> 
> Using longjmp() from a signal handler or modifying the handling of SIGALRM while sleeping will cause undefined results.

Sleep 是通过调用 alarm() 来设定报警时间，调用 sigsuspend() 将进程挂起在信号 SIGALARM 上，sleep() 只能精确到秒级上。

sleep() 的实现分为三步：
1. 注册一个信号 signal(SIGALRM,handler)。接收内核给出的一个信号。
1. 调用 alarm() 函数。
1. pause() 挂起进程。

实例代码：
```cpp
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void wakeUp()
{
    printf("please wakeup!!/n");
}

int main() 
{
    printf("you have 4 s sleep!/n");
    signal(SIGALRM,wakeUp);
    alarm(4); 
    pause();
    printf("good morning!/n");
    return EXIT_SUCCESS;
}
```
其中，alarm() 也是通过定时器实现的，但是其精度只精确到秒级，另外，它设置的定时器执行函数是在指定时间向当前进程发送 SIGALRM 信号。

关键的是 pause(). 当执行到这个函数的时候，当前进程被挂起，等时钟 alarm 函数 4 秒之后，内核发送一个 SIGALRM 信号。导致控制从 pause 函数转到信号的处理函数。信号处理函数中的代码被执行，然后控制返回。当信号被处理完毕之后，pause 函数返回，进程继续。

#### 3.1.2. usleep

http://man7.org/linux/man-pages/man3/usleep.3.html

```cpp
int usleep(useconds_t usec);
```

usleep() 有有很大的问题
- 在一些平台下不是线程安全，如 HP-UX 以及 Linux
- usleep() 会影响信号
- 在很多平台，如 HP-UX 以及某些 Linux 下，当参数的值必须小于 1 * 1000 * 1000 也就是 1 秒，否则该函数会报错，并且立即返回。

大部分平台的帮助文档已经明确说了，该函数是已经被舍弃的函数。

#### 3.1.3. nanosleep

http://man7.org/linux/man-pages/man2/nanosleep.2.html

https://stackoverflow.com/questions/1157209/is-there-an-alternative-sleep-function-in-c-to-milliseconds

```cpp
#include <time.h>

int nanosleep(const struct timespec *req, struct timespec *rem);
```

> Compared to sleep() and usleep(), nanosleep() has the following advantages: it provides a higher resolution for specifying the sleep interval; POSIX.1 explicitly specifies that it does not interact with signals; and it makes the task of resuming a sleep that has been interrupted by a signal handler easier.

nanosleep() 则是 Linux 中的系统调用，它是使用定时器来实现的，该调用使调用进程睡眠，并往定时器队列上加入一个 timer_list 型定时器，time_list 结构里包括唤醒时间以及唤醒后执行的函数，通过 nanosleep() 加入的定时器的执行函数仅仅完成唤醒当前进程的功能。系统通过一定的机制定时检查这些队列（比如通过系统调用陷入核心后，从核心返回用户态前，要检查当前进程的时间片是否已经耗尽，如果是则调用 schedule() 函数重新调度，该函数中就会检查定时器队列，另外慢中断返回前也会做此检查），如果定时时间已超过，则执行定时器指定的函数唤醒调用进程。当然，由于系统时间片可能丢失，所以 nanosleep() 精度也不是很高。

> usleep() has since been deprecated and subsequently removed from POSIX; for new code, nanosleep() is preferred.

nanosleep() 函数没有 usleep() 的那些缺点，它的精度是纳秒级。在 Solaris 的多线程环境下编译器会自动把 usleep() 连接成 nanosleep()。

eg:
```cpp
struct timespec request{0, 20 * 1000}, remaining{};
//    request.tv_sec = 0;
//    request.tv_nsec = 20 * 1000; // 20ms
nanosleep(&request, &remaining);
```

#### 3.1.4. ssleep / msleep

https://stackoverflow.com/questions/3544978/where-is-msleep-declared

- void ssleep(unsigned int seconds) -- second delay
- void msleep(unsigned int msecs) -- millisecond delay

### 3.2. 忙等待

暂停进程但不释放 CPU。

#### 3.2.1. udelay

#### 3.2.2. mdelay

#### 3.2.3. ndelay

## 4. Refer Links

TODO:

[c 标准库和操作系统 api 的关系是怎样的？](https://www.zhihu.com/question/46763480)

[C 标准库和 Linux 系统 glibc(C 运行库) 的关系？](https://www.zhihu.com/question/49945649)

[什么是 C 和 C ++ 标准库？](https://www.oschina.net/translate/c-c-standard-library)

https://blog.csdn.net/zephyr_be_brave/article/details/8847319

[how to achieve microsecond sleep in kernel ?](https://www.linuxquestions.org/questions/linux-kernel-70/how-to-achieve-microsecond-sleep-in-kernel-916541/)