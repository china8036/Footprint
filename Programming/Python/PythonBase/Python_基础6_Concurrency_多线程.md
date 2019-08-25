- [Python 多任务：多线程](#python-多任务多线程)
  - [1. threading 模块](#1-threading-模块)
    - [1.1. Thread 类](#11-thread-类)
      - [1.1.1. 构造方法](#111-构造方法)
      - [1.1.2. 方法](#112-方法)
      - [1.1.3. 自定义 Thread 类](#113-自定义-thread-类)
    - [1.2. 进程间同步](#12-进程间同步)
      - [1.2.1. Lock 类](#121-lock-类)
      - [1.2.2. RLock 类](#122-rlock-类)
      - [1.2.3. Condition 类](#123-condition-类)
      - [1.2.4. Semaphore 类 / BoundedSemaphore 类](#124-semaphore-类--boundedsemaphore-类)
      - [1.2.5. ThreadLocal 类](#125-threadlocal-类)
      - [1.2.6. 其它方法](#126-其它方法)
    - [1.3. multiprocessing.dummy 模块](#13-multiprocessingdummy-模块)
    - [1.4. multiprocessing.pool.ThreadPool 模块](#14-multiprocessingpoolthreadpool-模块)
    - [1.5. GIL 全局解释器锁](#15-gil-全局解释器锁)
  - [2. Refer Links](#2-refer-links)

# Python 多任务：多线程

Python 的线程是真正的 Posix Thread，而不是模拟出来的线程。

Python 的标准库提供了两个模块：_thread 和 threading._thread 是低级模块，threading 是高级模块，对_thread 进行了封装。绝大多数情况下，我们只需要使用 threading 这个高级模块。

## 1. threading 模块

### 1.1. Thread 类

#### 1.1.1. 构造方法

```python
Thread([group [, target [, name [, args [, kwargs]]]]])
# 参数说明：
# target 表示调用对象，你可以传入函数名
# args 表示被调用对象的位置参数元组，比如 target 是函数 a，他有两个参数 m，n，那么 args 就传入 (m, n) 即可
# kwargs 表示调用对象的字典
# name 是别名，相当于给这个线程取一个名字，如果不起名字 Python 就自动给线程命名为 Thread-1，Thread-2……，主线程实例的名叫 MainThread
# group 分组，实际上不使用
```

#### 1.1.2. 方法

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

#### 1.1.3. 自定义 Thread 类

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

### 1.2. 进程间同步

#### 1.2.1. Lock 类

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

#### 1.2.2. RLock 类

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

#### 1.2.3. Condition 类

https://www.cnblogs.com/huxi/archive/2010/06/26/1765808.html

#### 1.2.4. Semaphore 类 / BoundedSemaphore 类

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

#### 1.2.5. ThreadLocal 类

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

#### 1.2.6. 其它方法

1)	current_thread()：返回当前的线程对象；

### 1.3. multiprocessing.dummy 模块

multiprocessing 在 python 中是一个多进程模块，而 multiprocessing.dummy 则是一个多线程模块。

### 1.4. multiprocessing.pool.ThreadPool 模块

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

### 1.5. GIL 全局解释器锁

GIL 的全称是 Global Interpreter Lock（全局解释器锁)，来源是 python 设计之初的考虑，为了数据安全所做的决定。

Python 的线程虽然是真正的线程，但解释器执行代码时，有一个 GIL 锁：Global Interpreter Lock（全局解释器锁)，任何 Python 线程执行前，必须先获得 GIL 锁，然后，当计时器超时后，解释器就自动释放 GIL 锁，让别的线程有机会执行。这个 GIL 全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在 Python 中只能交替执行，即使 100 个线程跑在 100 核 CPU 上，也只能用到 1 个核。

在 Python 多线程下，每个线程的执行方式：
1. 获取 GIL
1. 执行代码直到 sleep 或者是 python 虚拟机将其挂起。
1. 释放 GIL

验证：

用 Python 写个死循环：
```python
def loop():
    x = 0
    while True:
        x = x ^ 1

for i in range(multiprocessing.cpu_count()):
    t = threading.Thread(target=loop)
    t.start()
```
启动与 CPU 核心数量相同的 N 个线程，在 4 核 CPU 上可以监控到 CPU 占用率仅有 102%，也就是仅使用了一核。

GIL 是 Python 解释器设计的历史遗留问题，通常我们用的解释器是官方实现的 CPython，要真正利用多核，除非重写一个不带 GIL 的解释器。

所以，在 Python 中，可以使用多线程，但不要指望能有效利用多核。如果一定要通过多线程利用多核，那只能通过 C 扩展来实现，不过这样就失去了 Python 简单易用的特点。

Python 虽然不能利用多线程实现多核任务，但可以通过多进程实现多核任务。多个 Python 进程有各自独立的 GIL 锁，互不影响。

由于 GIL 的存在，python 中的多线程其实并不是真正的多线程，并不能做到充分利用多核 CPU 资源。

在 Python 中，线程不能加速受 CPU 限制的任务，原因是标准 Python 系统中使用了全局解释器锁（GIL）。 GIL 的作用是避免 Python 解释器中的线程问题，但是实际上会让多线程程序运行速度比对应的单线程版本甚至是多进程版本更慢。

## 2. Refer Links