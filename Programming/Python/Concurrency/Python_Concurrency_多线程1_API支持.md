- [Python 并发：多线程](#python-并发多线程)
  - [1. 底层实现模块：thread / _thread](#1-底层实现模块thread--_thread)
    - [1.1. thread](#11-thread)
    - [1.2. _thread](#12-_thread)
  - [2. 高级封装模块：threading](#2-高级封装模块threading)
    - [2.1. 模块方法](#21-模块方法)
    - [2.2. Thread 类](#22-thread-类)
      - [2.2.1. 构造方法](#221-构造方法)
      - [2.2.2. 方法](#222-方法)
      - [2.2.3. 自定义 Thread 类](#223-自定义-thread-类)
    - [2.3. 线程间同步](#23-线程间同步)
      - [2.3.1. Lock 类](#231-lock-类)
      - [2.3.2. RLock 类](#232-rlock-类)
      - [2.3.3. Condition 类](#233-condition-类)
      - [2.3.4. Semaphore 类 / BoundedSemaphore 类](#234-semaphore-类--boundedsemaphore-类)
      - [2.3.5. ThreadLocal 类](#235-threadlocal-类)
  - [3. dummy 模块：dummy_thread / _dummy_thread / dummy_threading](#3-dummy-模块dummy_thread--_dummy_thread--dummy_threading)
  - [4. multiprocessing 线程模块](#4-multiprocessing-线程模块)
    - [4.1. threading 封装模块：multiprocessing.dummy](#41-threading-封装模块multiprocessingdummy)
    - [4.2. 线程池模块：multiprocessing.pool.ThreadPool](#42-线程池模块multiprocessingpoolthreadpool)
  - [5. Refer Links](#5-refer-links)

# Python 并发：多线程

## 1. 底层实现模块：thread / _thread

`thread`(python 2.7) / `_thread`(Python3.7) 是一个 build-in 的模块，在解释器（如 cpython）中由底层代码（如 C++）实现，提供了多线程操作底层 API 的具体实现。

Python 的多线程是真正的 Posix Thread，底层基于 pthread 创建线程，而不是模拟出来的用户态线程。

### 1.1. thread

```
This module provides primitive operations to write multi-threaded programs.
    The 'threading' module provides a more convenient interface.

    __builtin__.object
        lock
    exceptions.Exception(exceptions.BaseException)
        error
```

### 1.2. _thread

https://docs.python.org/3/library/_thread.html

```
This module provides primitive operations to write multi-threaded programs.
    The 'threading' module provides a more convenient interface.

    builtins.object
        RLock
        lock
```
<!-- TODO: 相比 thread 增加了读写锁？ -->

## 2. 高级封装模块：threading

threading 是对 `thread`(python 2.7) / `_thread`(Python3.7) 模块的高级封装，实现了 Java 线程模型的子集。在绝大多数情况下，我们只需要使用 threading 这个高级模块即可实现多线程开发。

```
threading - Thread module emulating a subset of Java's threading model.
    Higher-level threading interface

    builtins.Exception(builtins.BaseException)
        builtins.RuntimeError
            BrokenBarrierError
    builtins.object
        _thread._local
        Barrier
        Condition
        Event
        Semaphore
            BoundedSemaphore
        Thread
            Timer
```

### 2.1. 模块方法

- `currentThread()`: 返回当前的线程变量。

- `enumerate()`: 返回一个包含正在运行的线程的 list。正在运行指线程启动后、结束前，不包括启动前和终止后的线程。

- `activeCount()`: 返回正在运行的线程数量，与 len(threading.enumerate()) 有相同的结果。

### 2.2. Thread 类

#### 2.2.1. 构造方法

```python
Thread([group [, target [, name [, args [, kwargs]]]]])
# 参数说明：
# target 表示调用对象，你可以传入函数名
# args 表示被调用对象的位置参数元组，比如 target 是函数 a，他有两个参数 m，n，那么 args 就传入 (m, n) 即可
# kwargs 表示调用对象的字典
# name 是别名，相当于给这个线程取一个名字，如果不起名字 Python 就自动给线程命名为 Thread-1，Thread-2……，主线程实例的名叫 MainThread
# group 分组，实际上不使用
```

#### 2.2.2. 方法

- `start(self)`: 开始线程执行。

- `join(self, timeout=None)`: 阻塞主线程，直到调用该方法的线程执行结束，才返回继续运行主线程。如果给出 timeout，则最多阻塞 timeout 秒。

  <!-- TODO: 思考 -- 待确认是否正确：与多进程不同，调用了线程的 start 之后，该线程会马上与主线程并行执行（而不是像多进程一样要等到父进程执行完毕后才开始子进程）。思考：这是因为创建和切换一个线程的开销要比创建进程的开销小的多，因此不会产生多进程中的那种父进程先执行完后再开始子进程的“错觉“。 -->

- `getName(self)`: 返回线程的名字。

- `isAlive(self)`: 布尔标志，表示这个线程是否还在运行中。

- `isDaemon(self)`: 返回线程的 daemon 布尔标志。

- `setDaemon(self: daemonic)`: 当我们在程序运行中，执行一个主线程，如果主线程又创建一个子线程，主线程和子线程就分兵两路，当主线程完成想退出时，会检验子线程是否完成。如果子线程未完成，则主线程会等待子线程完成后再退出。但是有时候我们需要的是，只要主线程完成了，不管子线程是否完成，都要和主线程一起退出，这时就可以用 setDaemon(True) 将主线程设置为守护线程，则主线程退出时，其它所有线程也都会退出。

- `setName(self, name)`: 设置线程的名字。

- 获取线程返回值

  有时候，我们往往需要获取每个子线程的返回值。然而通过调用普通函数，获取 return 值的方式在多线程中并不适用。因此需要一种新的方式去获取子线程返回值。

  e.g.
  ```python
  import threading

  class test(threading.Thread):
      def __init__(self):
          threading.Thread.__init__(self)

      def run(self):
          self.tag = 1

      def get_result(self):
          if self.tag == 1:
              return True
          else:
              return False
  f = test()
  f.start()
  while f.isAlive():
      continue
  print(f.get_result())
  ```

#### 2.2.3. 自定义 Thread 类

与 multiprocessing 模块相同，除了使用构造方法创建一个新的进程 / 线程，还可以通过继承 Thread 类，来创建一个新的线程。

e.g.
```python
class MyThread(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self);

    def run(self):
        print "I am %s" %self.name

if __name__ == "__main__":
    for thread in range(0, 5):
        t = MyThread()
        t.start()
```

### 2.3. 线程间同步

#### 2.3.1. Lock 类

e.g.
```python
balance = 0
def change_it(n):
    # 先存后取，结果应该为 0:
    global balance
    balance = balance + n
    balance = balance - n

def run_thread(n):
    for i in range(1000000):
        change_it(n)

t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(8,))
t1.start()
t2.start()
t1.join()
t2.join()
print(balance)
```
由于线程的调度是由操作系统决定的，当 t1、t2 交替执行时，只要循环次数足够多，balance 的结果就不一定是 0 了。

如果我们要确保 balance 计算正确，就要给 change_it() 上一把锁，当某个线程开始执行 change_it() 时，我们说，该线程因为获得了锁，因此其他线程不能同时执行 change_it()，只能等待，直到锁被释放后，获得该锁以后才能改。由于锁只有一个，无论多少线程，同一时刻最多只有一个线程持有该锁，所以，不会造成修改的冲突。创建一个锁就是通过 threading.Lock() 来实现：

- 构造
  ```python
  lock = threading.Lock()
  ```

- 方法
  - `acquire()`：给线程加上互斥锁。
  - `release()`：释放线程的互斥锁。

e.g.
```python
balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁
        lock.acquire()
        try:
            # 执行边界资源的操作
            change_it(n)
        finally:
            # 执行完毕后一定要释放锁
            lock.release()
```
当多个线程同时执行 lock.acquire() 时，只有一个线程能成功地获取锁，然后继续执行代码，其他线程就继续等待直到获得锁为止。

获得锁的线程用完后一定要释放锁，否则那些苦苦等待锁的线程将永远等待下去，成为死线程。

为防止资源占用的操作出异常后 release 无法调用，而导致其它线程一直等待解锁而成为“死线程”，需要用 try...finally 来确保锁一定会被释放。

锁的好处就是确保了某段关键代码只能由一个线程从头到尾完整地执行，坏处当然也很多，首先是阻止了多线程并发执行，包含锁的某段代码实际上只能以单线程的串行模式执行，效率就大大地下降了。其次，由于可以存在多个锁，不同的线程持有不同的锁，并试图获取对方持有的锁时，可能会造成死锁，导致多个线程全部挂起，既不能执行，也无法结束，只能靠操作系统强制终止。

#### 2.3.2. RLock 类

RLock（可重入锁）是一个可以被同一个线程请求多次的同步指令。RLock 使用了“拥有的线程”和“递归等级”的概念，处于锁定状态时，RLock 被某个线程拥有。拥有 RLock 的线程可以再次调用 acquire()，释放锁时需要调用 release() 相同次数。

可以认为 RLock 包含一个锁定池和一个初始值为 0 的计数器，每次成功调用 acquire()/release()，计数器将 +1/-1，为 0 时锁处于未锁定状态。

e.g.
```python
rlock = threading.RLock()

def func():
    # 第一次请求锁定
    print '%s acquire lock...' % threading.currentThread().getName()
    if rlock.acquire():
        print '%s get the lock.' % threading.currentThread().getName()
        time.sleep(2)

        # 第二次请求锁定
        print '%s acquire lock again...' % threading.currentThread().getName()
        if rlock.acquire():
            print '%s get the lock.' % threading.currentThread().getName()
            time.sleep(2)

        # 第一次释放锁
        print '%s release lock...' % threading.currentThread().getName()
        rlock.release()
        time.sleep(2)

        # 第二次释放锁
        print '%s release lock...' % threading.currentThread().getName()
        rlock.release()

t1 = threading.Thread(target=func)
t2 = threading.Thread(target=func)
t3 = threading.Thread(target=func)
t1.start()
t2.start()
t3.start()
```

#### 2.3.3. Condition 类

https://www.cnblogs.com/huxi/archive/2010/06/26/1765808.html

#### 2.3.4. Semaphore 类 / BoundedSemaphore 类

信号量 Semaphore 对象维护着一个内部计数器，调用 acquire() 方法时该计数器减 1，调用 release() 方法时该计数器加 1，适用于需要控制特定资源的并发访问线程数量的场合，如连接池，控制线程最大运行数目。

调用 acquire() 方法时，如果计数器已经为 0 则阻塞当前线程，直到有其他线程调用了 release() 方法，所以计数器的值永远不会小于 0。

有限信号量 BoundedSemaphore，可以确保 release() 方法的调用次数不能超过给定的初始信号量数值 Semaphore 对象可以调用任意次 release() 方法，而 BoundedSemaphore 对象可以保证计数器的值不超过特定的值。

方法
- `acquire([timeout])`: 请求 Semaphore。如果计数器为 0，将阻塞线程至同步阻塞状态；否则将计数器 -1 并立即返回。
- `release()`: 释放 Semaphore，将计数器 +1，如果使用 BoundedSemaphore，还将进行释放次数检查。release() 方法不检查线程是否已获得 Semaphore。

e.g.
```python
def worker(value):
    # 线程启动时间
    start = time()
    with sema:
        # 获取资源访问权限的时间
        end = time()
        # 冒号后面是该线程等待的时间
        print(value, ':', end - start)
        sleep(randrange(5))
# 同一时刻最多允许 2 个线程访问特定资源
sema = BoundedSemaphore(2)
# 创建并启动 10 个线程
for i in range(10):
    t = Thread(target=worker, args=(i,))
    t.start()
```

#### 2.3.5. ThreadLocal 类

在多线程环境下，每个线程都有自己的数据。一个线程使用自己的局部变量比使用全局变量好，因为局部变量只有线程自己能看见，不会影响其他线程，而全局变量的修改必须加锁。

为解决了参数在一个线程中各个函数之间互相传递的问题，threading 模块有了 ThreadLocal 类。

ThreadLocal 对象虽然是全局变量，但每个线程都只能读写自己线程的独立副本，互不干扰。

e.g.
```python
import threading

# 创建全局 ThreadLocal 对象：
local_school = threading.local()

def process_student():
    # 获取当前线程关联的 student:
    std = local_school.student
    print('Hello, %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):
    # 绑定 ThreadLocal 的 student:
    local_school.student = name
    process_student()

t1 = threading.Thread(target= process_thread, args=('Alice',))
t2 = threading.Thread(target= process_thread, args=('Bob',))
t1.start()
t2.start()
t1.join()
t2.join()
```

全局变量 local_school 就是一个 ThreadLocal 对象，每个 Thread 对它都可以读写 student 属性，但互不影响。你可以把 local_school 看成全局变量，但每个属性如 local_school.student 都是线程的局部变量，可以任意读写而互不干扰，也不用管理锁的问题，ThreadLocal 内部会处理。

可以理解为全局变量 local_school 是一个 dict，不但可以用 local_school.student，还可以绑定其他变量，如 local_school.teacher 等等。

应用：
ThreadLocal 最常用的地方就是为每个线程绑定一个数据库连接，HTTP 请求，用户身份信息等，这样一个线程的所有调用到的处理函数都可以非常方便地访问这些资源。

## 3. dummy 模块：dummy_thread / _dummy_thread / dummy_threading

- dummy_thread(Python2.7)
  ```
  dummy_thread — Drop-in replacement for the thread module

  Meant to be used as a brain-dead substitute so that threaded code does
      not need to be rewritten for when the thread module is not present.

      Suggested usage is::

          try:
              import thread
          except ImportError:
              import dummy_thread as thread

      __builtin__.object
              LockType
          exceptions.Exception(exceptions.BaseException)
              error
  ```

- _dummy_thread(Python 3.7)
  ```
  _dummy_thread - Drop-in replacement for the thread module.

  Meant to be used as a brain-dead substitute so that threaded code does
      not need to be rewritten for when the thread module is not present.

      Suggested usage is::

          try:
              import _thread
          except ImportError:
              import _dummy_thread as _thread

      builtins.Exception(builtins.BaseException)
              builtins.RuntimeError
          builtins.object
              LockType
                  RLock
  ```

- dummy_threading
  ```
  dummy_threading — Drop-in replacement for the threading module.
      Faux ``threading`` version using ``dummy_thread`` instead of ``thread``.

      The module ``_dummy_threading`` is added to ``sys.modules`` in order
      to not have ``threading`` considered imported.  Had ``threading`` been
      directly imported it would have made all subsequent imports succeed
      regardless of whether ``thread`` was available which is not desired.

  ```

## 4. multiprocessing 线程模块

> This package is intended to duplicate the functionality (and much of the API) of threading.py but uses processes instead of threads.

### 4.1. threading 封装模块：multiprocessing.dummy

> A subpackage 'multiprocessing.dummy' has the same API but is a simple wrapper for 'threading'.

multiprocessing 在 python 中是一个多进程模块，而 multiprocessing.dummy 则是一个多线程模块，它是对 threading 模块的简单封装。

### 4.2. 线程池模块：multiprocessing.pool.ThreadPool

线程池，用法类似 multiprocessing.Pool 进程池。

e.g.
```python
import urllib2
from multiprocessing.dummy import Pool as ThreadPool
urls = [
        ...
		...
        ]
pool = ThreadPool(4)
results = pool.map(urllib2.urlopen, urls)
pool.close()
pool.join()
```

## 5. Refer Links

[Python Docs: 并发](https://docs.python.org/zh-cn/3/library/concurrency.html)

[菜鸟教程：Python 多线程](https://www.runoob.com/python/python-multithreading.html)

TODO:

ThreadLocal： https://www.liaoxuefeng.com/wiki/1016959663602400/1017630786314240

concurrent.futures.ThreadPoolExecutor 和 ProcessPoolExecutor