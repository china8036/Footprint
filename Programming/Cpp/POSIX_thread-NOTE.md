- [POSIX Thread Note](#posix-thread-note)
  - [1. 创建线程](#1-%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B)
  - [2. 管理线程](#2-%E7%AE%A1%E7%90%86%E7%BA%BF%E7%A8%8B)
    - [2.1. 互斥锁 Mutex](#21-%E4%BA%92%E6%96%A5%E9%94%81-mutex)
    - [2.2. 条件变量 Condition Variable](#22-%E6%9D%A1%E4%BB%B6%E5%8F%98%E9%87%8F-condition-variable)
    - [2.3. 信号量](#23-%E4%BF%A1%E5%8F%B7%E9%87%8F)
  - [3. 退出线程](#3-%E9%80%80%E5%87%BA%E7%BA%BF%E7%A8%8B)
  - [4. 其它 API](#4-%E5%85%B6%E5%AE%83-api)
  - [5. Refer Links](#5-refer-links)

# POSIX Thread Note

Pthreads 是 IEEE（电子和电气工程师协会）委员会开发的一组线程接口，负责指定便携式操作系统接口（POSIX）. Pthreads 中的 P 表示 POSIX，实际上，Pthreads 有时候也代表 POSIX 线程。

POSIX 委员会定义了一系列基本功能和数据结构，希望能够被大量厂商采用，以便线程代码能够轻松地在各种操作系统上移植。委员会的梦想由 UNIX 厂商实现了，他们都广泛 Pthreads . 最著名的例外就是 Sun，它继续采用 Solaris 线程作为其主要线程 API。

在 Linux 环境下，可以在 Shell 中通过 man 查询到 Pthreads 的部分函数命令，如： man pthread_create。编写 Linux 下的多线程程序，需要使用头文件 pthread.h，连接时需要使用库 libpthread.a。

Linux 下 pthread 的实现是通过系统调用 clone() 来实现的。clone() 是 Linux 所特有的系统调用，它的使用方式类似 fork。

## 1. 创建线程

```cpp
int pthread_create (pthread_t *thread, pthread_attr_t *attr, void *(*start_routine)(void *), void *arg)
```
若创建成功，返回 0；若出错，则返回错误代码。

参数说明：
- thread：存放线程标识符的变量。每个线程都有一个在进程中唯一的线程标识符，用一个数据类型 pthread_t 表示，该数据类型在 Linux 中就是一个无符号长整型数据。但这个参数不是由用户指定的，而是由 `pthread_create` 函数在创建时将新的线程的标识符放到这个变量中。

- attr：指定线程的属性，可以用 NULL 表示默认属性。

- `start_routine`：指定线程开始运行的函数。

- arg：`start_routine` 所需要的参数，是一个无类型指针。

默认地，线程在被创建时要被赋予一定的属性，这个属性存放在数据类型 `pthread_attr_t` 中，它包含了线程的调度策略，堆栈的相关信息，join or detach 的状态等。

`pthread_attr_init` 和 `pthread_attr_destroy` 函数分别用来创建和销毁 `pthread_attr_t`

## 2. 管理线程

### 2.1. 互斥锁 Mutex

使用互斥锁的一般步骤是：

- 创建一个互斥锁，即声明一个 pthread_mutex_t 类型的数据，然后初始化，只有初始化之后才能使用；

- 多个线程尝试锁定这个互斥锁；

- 只有一个成功锁定互斥锁，成为互斥锁的拥有者，然后进行一些指令；

- 拥有者解锁互斥锁；

- 其他线程尝试锁定这个互斥锁，重复上面的过程；

- 最后互斥锁被显式地调用 pthread_mutex_destroy 来进行销毁。

API：
- `pthread_mutex_init (mutex,attr)`
- `pthread_mutex_destroy (pthread_mutex_t *mutex)`
- `pthread_mutexattr_init (attr)`
- `pthread_mutexattr_destroy (attr)`
- `phtread_mutex_lock(pthread_mutex_t *mutex)`
- `phtread_mutex_trylock(pthread_mutex_t *mutex)`
- `phtread_mutex_unlock(pthread_mutex_t *mutex)`

有两种方式初始化一个互斥锁：
- 第一种，利用已经定义的常量初始化，例如 `pthread_mutex_t mymutex = PTHREAD_MUTEX_INITIALIZER;`
- 第二种方式是调用 `pthread_mutex_init (mutex,attr)` 进行初始化。

当多个线程同时去锁定同一个互斥锁时，失败的那些线程，如果是用 `pthread_mutex_lock` 函数，那么会被阻塞，直到这个互斥锁被解锁，它们再继续竞争；如果是用 `pthread_mutex_trylock` 函数，那么失败者只会返回一个错误。

最后需要指出的是，保护共享数据是程序员的责任。程序员要负责所有可以访问该数据的线程都使用 mutex 这种机制，否则，不使用 mutex 的线程还是有可能对数据造成破坏。

### 2.2. 条件变量 Condition Variable

互斥锁只有两种状态，这限制了它的用途。条件变量允许线程在阻塞的时候等待另一个线程发送的信号，当收到信号后，阻塞的线程就被唤醒并试图锁定与之相关的互斥锁。条件变量要和互斥锁结合使用。

### 2.3. 信号量

信号量本质上是一个非负的整数计数器，它被用来控制对公共资源的访问。当公共资源增加时，调用函数`sem_post（）`增加信号量。只有当信号量值大于 0 时，才能使用公共资源，使用后，函数`sem_wait（）`减少信号量。函数`sem_trywait（）`和函数`pthread_ mutex_trylock（）`起同样的作用，它是函数`sem_wait（）`的非阻塞版本。下面我们逐个介绍和信号量有关的一些函数，它们都在头文件`/usr/include/semaphore.h`中定义。

信号量的数据类型为结构`sem_t`，它本质上是一个长整型的数。函数 sem_init（）用来初始化一个信号量。它的原型为：
```cpp
extern int sem_init __P ((sem_t *__sem, int __pshared, unsigned int __value));
```
sem 为指向信号量结构的一个指针；pshared 不为０时此信号量在进程间共享，否则只能为当前进程的所有线程共享；value 给出了信号量的初始值。

API：
- `sem_post( sem_t *sem )`：用来增加信号量的值。当有线程阻塞在这个信号量上时，调用这个函数会使其中的一个线程不在阻塞，选择机制同样是由线程的调度策略决定的。
- `sem_wait( sem_t *sem )`：被用来阻塞当前线程直到信号量 sem 的值大于 0，解除阻塞后将 sem 的值减一，表明公共资源经使用后减少。
- `sem_trywait ( sem_t *sem )`：是函数 sem_wait（）的非阻塞版本，它直接将信号量 sem 的值减一。
- `sem_destroy(sem_t *sem)`：用来释放信号量 sem。

## 3. 退出线程

一般情况下，线程在其主体函数执行退出的时候会自动终止，但同时也可以因为接收到另一个线程发来的终止（取消）请求而强制终止。

pthread API:

- `void pthread_exit(void *retval)`：退出当前线程。

- `int pthread_join(pthread_t thread, void **retval)`：阻塞当前线程，直到指定的目标线程结束退出。

  - 终止的线程所占用的资源不会随着线程的结束而归还系统，而是仍为线程所在的进程持有。在 Linux 中，默认情况下是在一个线程被创建后，必须使用此函数对创建的线程进行资源回收，但是可以设置 Threads attributes 来设置当一个线程结束时，直接回收此线程所占用的系统资源。因此，一个可`pthread_join`的线程所占用的资源仅当有线程对其执行了`pthread_join`后才会释放，因此为了防止内存泄漏，所有线程终止时，要么已经被设置为 DETACHED 状态，要么使用`pthread_join`来回收资源。

  - 一个线程不能被多个线程等待。否则第一个收到信号的线程成功返回。其余调用 pthread_join 的线程返回错误码`ESRCH  No thread with the ID thread could be found.`

- `int pthread_detach(pthread_t thread)`：在线程设置为 joinable 后，可以调用`pthread_detach()`使之成为 detached。但是相反的操作则不可以。还有，如果线程已经调用`pthread_join()`后，则再调用`pthread_detach()`则不会有任何效果。

- `pthread_cancel()`：发送终止信号给 thread 线程，如果成功则返回 0，否则为非 0 值。发送成功并不意味着 thread 会终止。

NOTE：
- 在主线程的 main 函数中 return，则主线程退出，且整个进程也会终止，此时进程中的所有线程也将终止。因此要避免 main 函数过早结束。

- 在主线程中调用 pthread_exit, 则仅仅是主线程结束，进程不会结束，进程内的其他线程也不会结束，知道所有线程结束，进程才会终止。

- 在任何一个线程中调用 exit 函数都会导致整个进程结束。进程一旦结束，那么进程中的所有线程都将结束。

## 4. 其它 API

- pthread_self ()：返回线程的 thread ID

- pthread_equal (thread1,thread2)：比较两个线程的 ID, 如果不同则返回 0, 否则返回一个非零值。

## 5. Refer Links

[pthread.h Document](http://pubs.opengroup.org/onlinepubs/7908799/xsh/pthread.h.html)

[POSIX Threads Programming](https://computing.llnl.gov/tutorials/pthreads/)

http://blog.csdn.net/modiziri/article/details/41960179

https://hanbingyan.github.io/2016/03/07/pthread_on_linux/#section-3
