- [POSIX Thread](#posix-thread)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 两大保证](#2-%E4%B8%A4%E5%A4%A7%E4%BF%9D%E8%AF%81)
	- [3. 创建线程](#3-%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B)
	- [4. 线程同步](#4-%E7%BA%BF%E7%A8%8B%E5%90%8C%E6%AD%A5)
		- [4.1. 互斥锁 Mutex Lock](#41-%E4%BA%92%E6%96%A5%E9%94%81-mutex-lock)
		- [4.2. 自旋锁 Spin Lock](#42-%E8%87%AA%E6%97%8B%E9%94%81-spin-lock)
		- [4.3. 读写锁 Reader-Writter Lock](#43-%E8%AF%BB%E5%86%99%E9%94%81-reader-writter-lock)
		- [4.4. 条件变量 Condition Variable](#44-%E6%9D%A1%E4%BB%B6%E5%8F%98%E9%87%8F-condition-variable)
		- [4.5. 信号量 Semaphore](#45-%E4%BF%A1%E5%8F%B7%E9%87%8F-semaphore)
		- [4.6. Barriers](#46-barriers)
	- [5. 退出线程](#5-%E9%80%80%E5%87%BA%E7%BA%BF%E7%A8%8B)
	- [6. 线程回调](#6-%E7%BA%BF%E7%A8%8B%E5%9B%9E%E8%B0%83)
	- [7. 其它 API](#7-%E5%85%B6%E5%AE%83-api)
	- [9. Refer Links](#9-refer-links)

# POSIX Thread

## 1. 基本概念

**POSIX 线程**（英语：POSIX Threads，常被缩写为 Pthreads）是 [POSIX](https://zh.wikipedia.org/wiki/POSIX) 的[线程](https://zh.wikipedia.org/wiki/%E7%BA%BF%E7%A8%8B) 标准，定义了创建和操纵线程的一套 [API](https://zh.wikipedia.org/wiki/Application_programming_interface)。 实现 POSIX 线程标准的库常被称作**Pthreads**，一般用于 [Unix-like](https://zh.wikipedia.org/wiki/Unix-like) POSIX 系统，如 [Linux](https://zh.wikipedia.org/wiki/Linux)、 [Solaris](https://zh.wikipedia.org/wiki/Solaris)。但是 [Microsoft Windows](https://zh.wikipedia.org/wiki/Microsoft_Windows) 上的实现也存在，例如直接使用 Windows API 实现的第三方库 pthreads-w32；而利用 Windows 的 SFU/SUA 子系统，则可以使用微软提供的一部分原生 POSIX API。 

Pthreads 是 IEEE（电子和电气工程师协会）委员会开发的一组线程接口，负责指定便携式操作系统接口（POSIX）. Pthreads 中的 P 表示 POSIX，实际上，Pthreads 有时候也代表 POSIX 线程。

POSIX 委员会定义了一系列基本功能和数据结构，希望能够被大量厂商采用，以便线程代码能够轻松地在各种操作系统上移植。委员会的梦想由 UNIX 厂商实现了，他们都广泛 Pthreads . 最著名的例外就是 Sun，它继续采用 Solaris 线程作为其主要线程 API。

在 Linux 环境下，可以在 Shell 中通过 man 查询到 Pthreads 的部分函数命令，如： man pthread_create。编写 Linux 下的多线程程序，需要使用头文件 pthread.h，连接时需要使用库 libpthread.a。

**Linux 下 pthread 的实现是通过系统调用 clone() 来实现的**。clone() 是 Linux 所特有的系统调用，它的使用方式类似 fork。

Pthreads 定义了一套 C 语言的类型、函数与常量，它以`pthread.h`头文件和一个线程库实现。Pthreads API 中大致共有 100 个函数调用，全都以`pthread_`开头。

POSIX 的 [Semaphore](https://zh.wikipedia.org/wiki/%E4%BF%A1%E8%99%9F%E6%A8%99) API 可以和 Pthreads 协同工作，但这并不是 Pthreads 的标准。因而这部分 API 是以`sem_`打头，而非`pthread_`。 

## 2. 两大保证

为了解决各线程同时访问一段内存，**pthread 库提供了原子访问和内存可见性两大保证**，这里举一个可见性原则：当线程 A 修改变量后调用 pthread_unlock mutex，且线程 B 成功 pthread_lock mutex 后，对线程 A 之前的变量修改是立即可见的。

但 mutex 往往也会造成性能过低，当临界区过大会限制线程的并发，而临界区过小会造成上下文的频繁切换（此时应考虑 adaptive mutex)。而随着硬件的发展，并行编程变得越来越重要。

## 3. 创建线程

```cpp
int pthread_create (pthread_t *thread, pthread_attr_t *attr, void *(*start_routine)(void *), void *arg)
```
若创建成功，返回 0；若出错，则返回错误代码。

参数说明：
- thread：存放线程标识符的变量。每个线程都有一个在进程中唯一的线程标识符，用一个数据类型 pthread_t 表示，该数据类型在 Linux 中就是一个无符号长整型数据。但这个参数不是由用户指定的，而是由 `pthread_create` 函数在创建时将新的线程的标识符放到这个变量中。

- attr：指定线程的属性（封装在 `pthread_attr_t` 中），可以用 NULL 表示默认属性。

	默认地，线程在被创建时要被赋予一定的属性，这个属性存放在数据类型 `pthread_attr_t` 中，它包含了线程的调度策略，堆栈的相关信息，join or detach 的状态等。

	`pthread_attr_init` 和 `pthread_attr_destroy` 函数分别用来创建和销毁 `pthread_attr_t`

- `start_routine`：指定线程开始运行的函数。

- arg：`start_routine` 所需要的参数，是一个无类型指针。

eg:
```c
#include <pthread.h> 
#include <sched.h> 
 
//...  
 
pthread_t ThreadA;  
pthread_attr_t SchedAttr;  
sched_param SchedParam;  
int MidPriority,MaxPriority,MinPriority;  
 
int main(int argc, char *argv[])  
{  
   //...  
 
   // Step 1: initialize attribute object  
   pthread_attr_init(&SchedAttr);  
 
   // Step 2: retrieve min and max priority values for scheduling policy  
   MinPriority = sched_get_priority_max(SCHED_RR);  
   MaxPriority = sched_get_priority_min(SCHED_RR);  
 
   // Step 3: calculate priority value  
   MidPriority = (MaxPriority + MinPriority)/2;  
 
   // Step 4: assign priority value to sched_param structure  
   SchedParam.sched_priority = MidPriority;  
 
   // Step 5: set attribute object with scheduling parameter  
   pthread_attr_setschedparam(&SchedAttr,&SchedParam);  
 
   // Step 6: set scheduling attributes to be determined by attribute object  
   pthread_attr_setinheritsched(&SchedAttr,PTHREAD_EXPLICIT_SCHED);  
 
   // Step 7: set scheduling policy  
   pthread_attr_setschedpolicy(&SchedAttr,SCHED_RR);  
 
   // Step 8: create thread with scheduling attribute object  
   pthread_create(&ThreadA,&SchedAttr,task1,NULL);  
 
   //...  
} 
```

## 4. 线程同步

线程同步 (Thread Synchronization) 是并行编程中非常重要的通讯手段，其中最典型的应用就是用 Pthreads 提供的锁机制 (lock) 来对多个线程之间共享的临界区 (Critical Section) 进行保护。

Pthreads 提供了多种锁机制：
- Mutex（互斥锁）: `pthread_mutex_***`
- Spin lock（自旋锁）: `pthread_spin_***`
- Condition Variable（条件变量）: `pthread_con_***`
- Read/Write lock（读写锁）: `pthread_rwlock_***`

### 4.1. 互斥锁 Mutex Lock

Mutual-Exclude Lock 是使用最广泛的一种同步机制，被这个锁保护的临界区就只允许一个线程进入，其它线程如果没有获得锁权限，那就只能在外面等着。

使用互斥锁的一般步骤是：
1. 创建一个互斥锁，即声明一个 pthread_mutex_t 类型的数据，然后初始化，只有初始化之后才能使用。
1. 多个线程尝试锁定这个互斥锁。
1. 只有一个成功锁定互斥锁，成为互斥锁的拥有者，然后进行一些指令。
1. 拥有者解锁互斥锁。
1. 其他线程尝试锁定这个互斥锁，重复上面的过程。
1. 最后互斥锁被显式地调用 pthread_mutex_destroy 来进行销毁。

相关 API：
- `PTHREAD_MUTEX_INITIALIZER`: 用于静态的 mutex 的初始化，采用默认的 attr。比如：`static pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;`。
- `pthread_mutex_init (mutex,attr)`: 用于动态的初始化。
- `pthread_mutex_destroy (pthread_mutex_t *mutex)`
- `phtread_mutex_lock(pthread_mutex_t *mutex)`
- `phtread_mutex_trylock(pthread_mutex_t *mutex)`: 当多个线程同时去锁定同一个互斥锁时，失败的那些线程，如果是用 `pthread_mutex_lock` 函数，那么会被阻塞，直到这个互斥锁被解锁，它们再继续竞争；如果是用 `pthread_mutex_trylock` 函数，那么失败者只会返回一个错误。
- `phtread_mutex_unlock(pthread_mutex_t *mutex)`
- `pthread_mutexattr_init (attr)`
- `pthread_mutexattr_destroy (attr)`

有两种方式初始化一个互斥锁：
- 第一种：利用已经定义的常量初始化，例如 `pthread_mutex_t mymutex = PTHREAD_MUTEX_INITIALIZER;`。如果没有特殊的配置要求的话，使用宏比较好，因为它比较快。
- 第二种：调用 `pthread_mutex_init (mutex,attr)` 进行初始化。

基本上所有的问题都可以用互斥的方案去解决，但不要不管什么情况都用互斥，都能采用这种方案不代表都适合采用这种方案。mutex 影响的面比较大，本来不需要通过互斥就能让线程进入临界区，但用了互斥方案之后，就使这样的线程不得不等待互斥锁的释放，比如多资源分配，线程步调通知等。如果是读多写少的场合，就比较适合读写锁 (reader/writter lock)，如果临界区比较短，就适合空转锁 (pin lock)。

互斥锁分为 3 种：
- 快速互斥锁
- 递归互斥锁
- 检测互斥锁

TODO: https://zh.wikipedia.org/wiki/%E4%BA%92%E6%96%A5%E9%94%81

互斥锁的实现，维护了一个等待队列和一个引用计数器 futex(fast Userspace mutexes)，当获取锁之前，先对引用计数器减 1 操作，如果为非负，则可以获取锁进入临界区，否则需要将该任务挂在该等待对列上。
- Lock 操作如下：
  1. 执行一个原子操作，对整数减一并返回结果。
  1. 如果结果为 -1，说明 critical region 内没有其它线程，继续执行进入 critical region。
  1. 如果结果小于 -1，将当前线程放入等待队列。
- Unlock 操作如下：
	1. 执行一个原子操作，对整数加一并返回结果。
	1. 如果结果为 0，说明没有其它线程被阻塞。
	1. 如果结果小于 0，将等待队列中的一个线程执行放入 kernel scheduler 的 ready 队列（唤醒该线程）。

### 4.2. 自旋锁 Spin Lock

Spin Lock 在第一次申请加锁失败的时候，先不着急切换 context，空转一段时间。如果锁在短时间内又可用了，那么就避免了 context 切换的开销，CPU 浪费的时间也不多。空转一段时间之后发现还是不能申请加锁成功，那么就有很大概率在将来的不短的一段时间里面加锁也不成功，那么就把线程挂起，把轮询用的 CPU 时间释放出来给别的地方用。

事实上，spin lock 在实现的时候，有一个 `__pthread_spin_count` 限制，如果空转次数超过这个限制，线程依旧会挂起（__shed_yield）。

空转锁锁非常适合临界区非常短的场合，或者实时性要求比较高的场合。由于临界区短，线程需要等待的时间也短，即便轮询浪费 CPU 资源，也浪费不了多少，还省了 context 切换的开销。由于实时性要求比较高，来不及等待 context 切换的时间，那就只能浪费 CPU 资源在那儿轮询了。

相关 API:
- `PTHREAD_SPINLOCK_INITIALIZER`
- `int pthread_spin_init (__pthread_spinlock_t *__lock, int __pshared);`
- `int pthread_spin_destroy (__pthread_spinlock_t *__lock);`
- `int pthread_spin_trylock (__pthread_spinlock_t *__lock);`
- `int pthread_spin_unlock (__pthread_spinlock_t *__lock);`
- `int pthread_spin_lock (__pthread_spinlock_t *__lock);`

与 Mutex 对比：
- Mutex 适合对锁操作非常频繁的场景，并且具有更好的适应性。尽管相比 spin lock 它会花费更多的开销（主要是上下文切换），但是它能适合实际开发中复杂的应用场景，在保证一定性能的前提下提供更大的灵活度。

- spin lock 的 lock/unlock 性能更好（花费更少的 cpu 指令)，但是它只适应用于临界区运行时间很短的场景。而在实际软件开发中，除非程序员对自己的程序的锁操作行为非常的了解，否则使用 spin lock 不是一个好主意（通常一个多线程程序中对锁的操作有数以万次，如果失败的锁操作 (contended lock requests) 过多的话就会浪费很多的时间进行空等待)。

- 更保险的方法或许是先（保守的）使用 Mutex，然后如果对性能还有进一步的需求，可以尝试使用 spin lock 进行调优。

### 4.3. 读写锁 Reader-Writter Lock

Reader-Writter Lock 的特性是这样的，当一个线程加了读锁访问临界区，另外一个线程也想访问临界区读取数据的时候，也可以加一个读锁，这样另外一个线程就能够成功进入临界区进行读操作了。此时读锁线程有两个。当第三个线程需要进行写操作时，它需要加一个写锁，这个写锁只有在读锁的拥有者为 0 时才有效。也就是等前两个读线程都释放读锁之后，第三个线程就能进去写了。总结一下就是，读写锁里，读锁能允许多个线程同时去读，但是写锁在同一时刻只允许一个线程去写。

相关 API：
- `PTHREAD_RWLOCK_INITIALIZER`

- `int pthread_rwlock_init(pthread_rwlock_t *restrict rwlock, const pthread_rwlockattr_t *restrict attr);`
- `int pthread_rwlock_destroy(pthread_rwlock_t *rwlock);`

- `int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock);`
- `int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock);`

- `int pthread_rwlock_wrlock(pthread_rwlock_t *rwlock);`
- `int pthread_rwlock_trywrlock(pthread_rwlock_t *rwlock);`

- `int pthread_rwlock_unlock(pthread_rwlock_t *rwlock);`

- `int pthread_rwlock_timedrdlock_np(pthread_rwlock_t *rwlock, const struct timespec *deltatime);` 
- `int pthread_rwlock_timedwrlock_np(pthread_rwlock_t *rwlock, const struct timespec *deltatime);`

需要注意的是，这样的锁建立之后一定要设置优先级，不然就容易出现写线程饥饿。而且读写锁适合读多写少的情况，如果读、写一样多，那这时候还是用 mutex 锁比较合理。

### 4.4. 条件变量 Condition Variable

TODO: https://blog.csdn.net/maopig/article/details/61616785

条件变量能在合适的时候唤醒正在等待的线程，即允许线程在阻塞的时候等待另一个线程发送的信号，当收到信号后，阻塞的线程就被唤醒并试图锁定与之相关的互斥锁。条件变量必须要和互斥锁结合使用。

相关 API：
- `PTHREAD_COND_INITIALIZER`
- `int pthread_cond_init(pthread_cond_t *restrict cond, const pthread_condattr_t *restrict attr);`
- `int pthread_cond_destroy(pthread_cond_t *cond);`

- 发送信号
	- `int pthread_cond_signal(pthread_cond_t *cond);`
	- `int pthread_cond_broadcast(pthread_cond_t *cond);`

- 阻塞等待
	- `int pthread_cond_wait(pthread_cond_t *restrict cond, pthread_mutex_t *restrict mutex);`
  - `int pthread_cond_timedwait(pthread_cond_t *restrict cond, pthread_mutex_t *restrict mutex, const struct timespec *restrict abstime);`

	NOTE: pthread_cond_wait 必须与 mutex 协同使用，当线程调用 pthread_cond_wait 时，会先 unlock mutex lock，然后再进入 waiting 阻塞状态；直到被 pthread_cond_signal 唤醒，再重新 lock mutex。

```cpp
class Condition4 : public ConditionBase
{
public:
    Condition4()
        : signal_(false)
    {
    }

    void wait()
    {
        pthread_mutex_lock(&mutex_);
        while (!signal_)
        {
            pthread_cond_wait(&cond_, &mutex_);
        }
        signal_ = false;
        pthread_mutex_unlock(&mutex_);
    }

    void wakeup()
    {
        pthread_mutex_lock(&mutex_);
        signal_ = true;
        pthread_cond_signal(&cond_);
        pthread_mutex_unlock(&mutex_);
    }

private:
    bool signal_;
};
```

### 4.5. 信号量 Semaphore

信号量本质上是一个非负的整数计数器，它被用来控制对公共资源的访问。

当公共资源增加时，调用函数`sem_post（）`增加信号量。只有当信号量值大于 0 时，才能使用公共资源。
使用后，函数`sem_wait（）`减少信号量。函数`sem_trywait（）`和函数`pthread_ mutex_trylock（）`起同样的作用，它是函数`sem_wait（）`的非阻塞版本。

信号量的数据类型为结构`sem_t`，它本质上是一个长整型的数。函数 sem_init（）用来初始化一个信号量。它的原型为：
```cpp
extern int sem_init __P ((sem_t *__sem, int __pshared, unsigned int __value));
```
sem 为指向信号量结构的一个指针；pshared 不为 0 时此信号量在进程间共享，否则只能为当前进程的所有线程共享；value 给出了信号量的初始值。

相关 API：
- `int sem_destroy(sem_t *sem);`
- `int sem_init(sem_t *sem, int pshared, unsigned int value);`

- `int sem_wait(sem_t *sem);`: 如果 sempahore 的数值还够，那就 semaphore 数值减 1，然后进入临界区，也就是 P 操作。
- `int sem_post(sem_t *sem);`: 该函数会给 semphore 的值加 1，也就是 V 操作。
- `sem_trywait ( sem_t *sem )`: 该函数是 sem_wait（）的非阻塞版本，它直接将信号量 sem 的值减一。

- `int sem_getvalue(sem_t *sem, int *valp);`: 该函数把 semaphore 的值通过你传进去的指针告诉你，而不是用这个函数的返回值告诉你。

### 4.6. Barriers

场景：超大数组排序的时候，可以采用多线程的方案来排序。比如开 10 个线程分别排这个超大数组的 10 个部分。必须要这 10 个线程都完成了各自的排序，你才能进行后续的归并操作。先完成的线程会挂起等待，直到所有线程都完成之后，才唤醒所有等待的线程。

Barrier 可以理解成一个 mile stone。当一个线程率先跑到 mile stone 的时候，就先等待。当其他线程都到位之后，再从等待状态唤醒，继续做后面的事情。

相关 API:
- `int pthread_barrier_init(pthread_barrier_t *barrier, const pthread_barrierattr_t *restrict attr, unsigned count);`
- `int pthread_barrier_destroy(pthread_barrier_t *barrier);`
- `int pthread_barrier_wait(pthread_barrier_t *barrier);`: 该函数在唤醒之后，会有两种返回值：0 或者 PTHREAD_BARRIER_SERIAL_THREAD，在众多线程中只会有主线程在唤醒时得到 PTHREAD_BARRIER_SERIAL_THREAD 的返回，其他返回都是 0。

## 5. 退出线程

一般情况下，线程在其主体函数执行退出的时候会自动终止，但同时也可以因为接收到另一个线程发来的终止（取消）请求而强制终止。

pthread API:

- `void pthread_exit(void *retval)`：退出当前线程。

- `int pthread_join(pthread_t thread, void **retval)`：阻塞当前线程，直到指定的目标线程结束退出。

  - 终止的线程所占用的资源不会随着线程的结束而归还系统，而是仍为线程所在的进程持有。在 Linux 中，默认情况下是在一个线程被创建后，必须使用此函数对创建的线程进行资源回收，但是可以设置 Threads attributes 来设置当一个线程结束时，直接回收此线程所占用的系统资源。因此，一个可`pthread_join`的线程所占用的资源仅当有线程对其执行了`pthread_join`后才会释放，因此为了防止内存泄漏，所有线程终止时，要么已经被设置为 DETACHED 状态，要么使用`pthread_join`来回收资源。

  - 一个线程不能被多个线程等待。否则第一个收到信号的线程成功返回。其余调用 pthread_join 的线程返回错误码`ESRCH  No thread with the ID thread could be found.`

- `int pthread_detach(pthread_t thread)`：在线程设置为 joinable 后，可以调用`pthread_detach()`使之成为 detached。但是相反的操作则不可以。还有，如果线程已经调用`pthread_join()`后，则再调用`pthread_detach()`则不会有任何效果。

- `pthread_cancel()`：发送终止信号给 thread 线程，如果成功则返回 0，否则为非 0 值。发送成功并不意味着 thread 会终止。

NOTE：
- **在主线程的 main 函数中 return，则主线程退出，且整个进程也会终止，此时进程中的所有线程也将终止**。因此要避免 main 函数过早结束。

- 在主线程中调用 pthread_exit, 则**仅仅是主线程结束，进程不会结束，进程内的其他线程也不会结束，直到所有线程结束，进程才会终止**。

- **在任何一个线程中调用 exit 函数都会导致整个进程结束**。进程一旦结束，那么进程中的所有线程都将结束。

## 6. 线程回调

线程允许在退出的时候，调用一些回调方法。

- `pthread_cleanup_push(void (*callback)(void *), void *arg)`: 
- `pthread_cleanup_pop(int execute)`: 

callback 只有在以下情况下才会被调用：
- 线程通过 pthread_exit() 函数退出。
- 线程被 pthread_cancel() 取消。
- pthread_cleanup_pop(int execute) 时，execute 传了一个非 0 值。

## 7. 其它 API

- `pthread_self()`：返回线程的 thread ID。

- `pthread_equal(thread1, thread2)`：比较两个线程的 ID, 如果不同则返回 0, 否则返回一个非零值。

## 9. Refer Links

[pthread.h Document](http://pubs.opengroup.org/onlinepubs/7908799/xsh/pthread.h.html)

[Wikipedia POSIX 线程](https://zh.wikipedia.org/wiki/POSIX%E7%BA%BF%E7%A8%8B)

[POSIX Threads Programming](https://computing.llnl.gov/tutorials/pthreads/)

[《C++ 多核高级编程》6.7.3 设置线程调度和优先级](http://book.51cto.com/art/201006/206983.htm)

[Pthreads 并行编程之 spin lock 与 mutex 性能对比分析](http://www.parallellabs.com/2010/01/31/pthreads-programming-spin-lock-vs-mutex-performance-analysis/)

[Docs: pthread_spin_lock_***](https://computing.llnl.gov/tutorials/pthreads/man/pthread_spin_lock.txt)

[Docs: pthread_mutex_***](https://computing.llnl.gov/tutorials/pthreads/man/pthread_mutex_lock.txt)

[Docs: pthread_cond_***](https://computing.llnl.gov/tutorials/pthreads/man/pthread_cond_signal.txt)

[C++ 11 线程、锁和条件变量](https://fzheng.me/2016/08/11/cpp11-multi-thread/)

[pthread 的各种同步机制](https://casatwy.com/pthreadde-ge-chong-tong-bu-ji-zhi.html)

[条件变量的陷阱与思考](https://originlee.com/2015/01/21/trick-in-conditon-variable/)

[为什么条件锁会产生虚假唤醒现象（spurious wakeup）？](https://www.zhihu.com/question/271521213)

[pthread_cond_wait 为什么需要传递 mutex 参数？](https://www.zhihu.com/question/24116967)

[深入理解 pthread_cond_wait、pthread_cond_signal](https://blog.csdn.net/YEYUANGEN/article/details/37593533)

[生产者消费者模型（Linux 系统下的两种实现方法）](https://blog.csdn.net/yusiguyuan/article/details/48265205)

[多线程编程指南  > 第 4 章 用同步对象编程  > 使用条件变量](https://docs.oracle.com/cd/E19253-01/819-7051/sync-41991/index.html)

TODO:
http://blog.csdn.net/modiziri/article/details/41960179

https://hanbingyan.github.io/2016/03/07/pthread_on_linux/#section-3

https://www.zhihu.com/question/24116967