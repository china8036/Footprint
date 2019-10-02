- [Python 并发：多线程](#python-并发多线程)
  - [1. 底层实现模块：thread / _thread](#1-底层实现模块thread--_thread)
    - [1.1. thread](#11-thread)
      - [1.1.1. 基本概念](#111-基本概念)
      - [1.1.2. 组成结构](#112-组成结构)
      - [1.1.3. 实现分析](#113-实现分析)
    - [1.2. _thread](#12-_thread)
      - [1.2.1. 基本概念](#121-基本概念)
      - [1.2.2. 实现分析](#122-实现分析)
  - [2. 高级封装模块：threading](#2-高级封装模块threading)
    - [2.1. 基本概念](#21-基本概念)
    - [2.2. 版本差异](#22-版本差异)
    - [2.3. 辅助方法](#23-辅助方法)
    - [2.4. Thread Objects](#24-thread-objects)
    - [2.5. 线程间通信 (communication)](#25-线程间通信-communication)
      - [2.5.1. Event Objects](#251-event-objects)
    - [2.6. 线程间同步 (synchronization)](#26-线程间同步-synchronization)
      - [2.6.1. Lock Objects](#261-lock-objects)
      - [2.6.2. RLock Objects](#262-rlock-objects)
      - [2.6.3. Condition Objects](#263-condition-objects)
      - [2.6.4. Semaphore Objects](#264-semaphore-objects)
      - [2.6.5. BoundedSemaphore Objects](#265-boundedsemaphore-objects)
      - [2.6.6. Barrier Objects](#266-barrier-objects)
    - [2.7. Thread-Local Data](#27-thread-local-data)
    - [2.8. Timer](#28-timer)
  - [3. dummy 模块：dummy_thread / _dummy_thread / dummy_threading](#3-dummy-模块dummy_thread--_dummy_thread--dummy_threading)
  - [4. threading 封装模块：multiprocessing.dummy](#4-threading-封装模块multiprocessingdummy)
  - [5. 同步阻塞队列模块：Queue / queue](#5-同步阻塞队列模块queue--queue)
  - [6. 周期性调度模块：sched](#6-周期性调度模块sched)
  - [7. Refer Links](#7-refer-links)

# Python 并发：多线程

## 1. 底层实现模块：thread / _thread

`thread`(python 2.7) / `_thread`(Python3.7) 是一个 build-in 的模块，在解释器（如 cpython）中由底层代码（如 C++）实现，提供了多线程操作底层 API 的具体实现。

Python 的多线程是真正的 Posix Thread，底层基于 pthread 创建线程，而不是模拟出来的用户态线程。

### 1.1. thread

#### 1.1.1. 基本概念

[`thread`](https://docs.python.org/2.7/library/thread.html) module provides low-level primitives for working with multiple threads (also called light-weight processes or tasks) — multiple threads of control sharing their global data space.For synchronization, simple locks (also called mutexes or binary semaphores) are provided.

- The threading module provides an easier to use and higher-level threading API built on top of this module.

- The module is optional. It is supported on Windows, Linux, SGI IRIX, Solaris 2.x, as well as on systems that have a POSIX thread (a.k.a. “pthread”) implementation. For systems lacking the thread module, the dummy_thread module is available. It duplicates this module’s interface and can be used as a drop-in replacement.

- **The thread module has been renamed to _thread in Python 3**.

#### 1.1.2. 组成结构

**FUNCTIONS**：
- `thread.start_new_thread(function, args[, kwargs])`: Start a new thread and return its identifier. The thread executes the function function with the argument list args (which must be a tuple). The optional kwargs argument specifies a dictionary of keyword arguments. When the function returns, the thread silently exits.

  When the function terminates with an unhandled exception, a stack trace is printed and then the thread exits (but other threads continue to run).

- `thread.interrupt_main()`: Raise a `KeyboardInterrupt` exception in the main thread. A subthread can use this function to interrupt the main thread.

- `thread.exit()`: Raise the `SystemExit` exception. When not caught, this will cause the thread to exit silently. 等效于 `sys.exit()`.

- `thread.get_ident()`: Return the ‘thread identifier’ of the current thread. This is a nonzero integer. Its value has no direct meaning; it is intended as a magic cookie to be used e.g. to index a dictionary of thread-specific data. Thread identifiers may be recycled when a thread exits and another thread is created.

- `thread.stack_size([size])`: Return the thread stack size used when creating new threads. The optional size argument specifies the stack size to be used for subsequently created threads, and must be 0 (use platform or configured default) or a positive integer value of at least 32,768 (32kB). If size is not specified, 0 is used.

  If changing the thread stack size is unsupported, the error exception is raised. If the specified stack size is invalid, a ValueError is raised and the stack size is unmodified.

  32kB is currently the minimum supported stack size value to guarantee sufficient stack space for the interpreter itself. Note that some platforms may have particular restrictions on values for the stack size, such as requiring a minimum stack size > 32kB or requiring allocation in multiples of the system memory page size - platform documentation should be referred to for more information (4kB pages are common; using multiples of 4096 for the stack size is the suggested approach in the absence of more specific information). Availability: Windows, systems with POSIX threads.

- `thread.allocate_lock()`: Return a new lock object. Methods of locks are described below. The lock is initially unlocked.

**CLASSES**：
- lock
  - `lock.acquire([waitflag])`: Without the optional argument, this method acquires the lock unconditionally, **if necessary waiting until it is released by another thread** (only one thread at a time can acquire a lock — that’s their reason for existence).

    If the integer waitflag argument is present, the action depends on its value: if it is zero, the lock is only acquired if it can be acquired immediately without waiting, while if it is nonzero, the lock is acquired unconditionally as before.

    The return value is True if the lock is acquired successfully, False if not.

    **It is not possible to interrupt the acquire() method on a lock** — the KeyboardInterrupt exception will happen after the lock has been acquired.

  - `lock.release()`: Releases the lock. The lock must have been acquired earlier, but not necessarily by the same thread.

  - `lock.locked()`: Return the status of the lock: True if it has been acquired by some thread, False if not.

  - 除了使用 `release()` 和 `acquire()` 方法对 lock 对象进行操作，还可以使用使用 with 语句来操作 lock 对象：
    ```python
    import thread

    a_lock = thread.allocate_lock()
    with a_lock:
        print "a_lock is locked while this executes"
    ```

**NOTE**:

- 当存在多个线程时，Threads interact strangely with interrupts，如：
  - KeyboardInterrupt exception 会被 an arbitrary thread 接收到。
  - 当 signal 模块可用时，interrupts 总会被 main thread 接收到。

- 当主线程退出时，其他线程是否能继续运行是由具体系统实现来决定的：
  - On SGI IRIX using the native thread implementation, they survive.
  - On most other systems, they are killed without executing try … finally clauses or executing object destructors.

  When the main thread exits, it does not do any of its usual cleanup (except that try … finally clauses are honored), and the standard I/O files are not flushed.

#### 1.1.3. 实现分析

TODO:

```
This module provides primitive operations to write multi-threaded programs.
    The 'threading' module provides a more convenient interface.

    __builtin__.object
            lock
        exceptions.Exception(exceptions.BaseException)
            error
```

source code: cpython-2.7/Modules/threadmodule.c
```c
/* Lock objects */
typedef struct {
    PyObject_HEAD
    PyThread_type_lock lock_lock;
    PyObject *in_weakreflist;
} lockobject;
```

### 1.2. _thread

#### 1.2.1. 基本概念

Python2 中的 `thread` module 在 Python3 中更名为 [`_thread`](https://docs.python.org/3/library/_thread.html)。

```
This module provides primitive operations to write multi-threaded programs.
    The 'threading' module provides a more convenient interface.

    builtins.object
        RLock
        lock
```

NOTE：
- 相比 Python 2 中的 `thread` 模块，`_thread` 中添加了对可重入锁 `RLock` 的支持。

#### 1.2.2. 实现分析

TODO:

```
This module provides primitive operations to write multi-threaded programs.
    The 'threading' module provides a more convenient interface.

builtins.object
        RLock
        lock
```

source code: cpython-3.7/Modules/_threadmodule.c
```c
/* Lock objects */
typedef struct {
    PyObject_HEAD
    PyThread_type_lock lock_lock;
    PyObject *in_weakreflist;
} lockobject;

/* Recursive lock objects */
typedef struct {
    PyObject_HEAD
    PyThread_type_lock rlock_lock;
    unsigned long rlock_owner;
    unsigned long rlock_count;
    PyObject *in_weakreflist;
} rlockobject;
```

## 2. 高级封装模块：threading

### 2.1. 基本概念

Docs：
- https://github.com/python/cpython/blob/2.7/Lib/threading.py
- https://github.com/python/cpython/blob/3.7/Lib/threading.py

`threading` 模块是对 `thread`(python 2.7) / `_thread`(Python3.7) 模块的高级封装，实现了 Java 线程模型的子集。在绝大多数情况下，我们只需要使用 threading 这个高级模块即可实现多线程开发。

### 2.2. 版本差异

在 Python2 和 Python3 中， `threading` 模块存在一定差异：
- python2.7/threading.py
  ```
  threading - Thread module emulating a subset of Java's threading model.

    __builtin__.object
            thread._local
    _Verbose(__builtin__.object)
        Thread
    FUNCTIONS
      Lock = allocate_lock(...)
      RLock(*args, **kwargs)

      BoundedSemaphore(*args, **kwargs)
      Condition(*args, **kwargs)
      Event(*args, **kwargs)
      Semaphore(*args, **kwargs)
      Timer(*args, **kwargs)
      activeCount()
      active_count = activeCount()
      currentThread()
      current_thread = currentThread()
      enumerate()
      setprofile(func)
      settrace(func)
      stack_size(...)
  ```
  在 Python2 中，threading 模块以 FUNCTIONS 的形式提供了一系列用于线程间同步 (Synchronization) 的工厂方法，如 Condition、Semaphore、BoundedSemaphore 等。

- python3.7/threading.py
  ```
  threading - Thread module emulating a subset of Java's threading model.

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
      FUNCTIONS
        Lock = allocate_lock(...)
        RLock(*args, **kwargs)

        active_count()
        current_thread()
        enumerate()
        get_ident(...)
        main_thread()
        setprofile(func)
        settrace(func)
        stack_size(...)
  ```
  在 Python3 中，除了封装在底层 `_thread` 类中的 Lock 和 RLock 类，threading 模块将用于线程间同步的一系列工厂方法改为直接以 CLASS 的形式提供 API，如 Barrier、Condition、Semaphore 等。

### 2.3. 辅助方法

threading 模块中以模块方法的形式提供了一系列辅助线程操作的方法，这些方法都属于 executed atomically 的方法：

- `currentThread()` / `current_thread()`: Return the current Thread object, corresponding to the caller’s thread of control. If the caller’s thread of control was not created through the threading module, a dummy thread object with limited functionality is returned.

- `enumerate()`: Return a list of all Thread objects currently alive. The list includes daemonic threads, dummy thread objects created by current_thread(), and the main thread. It excludes terminated threads and threads that have not yet been started.

- `activeCount()` / `active_count()`: Return the number of Thread objects currently alive. The returned count is equal to the length of the list returned by enumerate().

- `settrace(func)`: Set a trace function for all threads started from the threading module. The func will be passed to sys.settrace() for each thread, before its run() method is called.

- `setprofile(func)`: Set a profile function for all threads started from the threading module. The func will be passed to sys.setprofile() for each thread, before its run() method is called.

- `stack_size([size])`: Return the thread stack size used when creating new threads.

  The optional size argument specifies the stack size to be used for subsequently created threads, and must be 0 (use platform or configured default) or a positive integer value of at least 32,768 (32 KiB). If size is not specified, 0 is used. If changing the thread stack size is unsupported, a ThreadError is raised. If the specified stack size is invalid, a ValueError is raised and the stack size is unmodified. 32kB is currently the minimum supported stack size value to guarantee sufficient stack space for the interpreter itself. Note that some platforms may have particular restrictions on values for the stack size, such as requiring a minimum stack size > 32kB or requiring allocation in multiples of the system memory page size - platform documentation should be referred to for more information (4kB pages are common; using multiples of 4096 for the stack size is the suggested approach in the absence of more specific information). Availability: Windows, systems with POSIX threads.

### 2.4. Thread Objects

A class that represents a thread of control. This class can be safely subclassed in a limited fashion.

- CONSTRUCTION
  ```python
  class threading.Thread(group=None, target=None, name=None, args=(), kwargs={}, *, daemon=None)¶
  ```
  Options:
  - `target`: 表示调用对象，你可以传入函数名。
  - `args`: 表示被调用对象的位置参数元组，比如 target 是函数 a，他有两个参数 m，n，那么 args 就传入 (m, n) 即可。Defaults to ().
  - `kwargs`: 表示调用对象的字典。Defaults to {}.
  - `name`: 是别名，相当于给这个线程取一个名字，如果不起名字 Python 就自动给线程命名为 Thread-1，Thread-2……，主线程实例的名叫 MainThread。
  - `group`: 分组，实际上不使用。

- API
  - `start(self)`: Once a thread object is created, **its activity must be started by calling the thread’s start() method**. This invokes the run() method in a separate thread of control. This method will raise a RuntimeError if called more than once on the same thread object.

  - `join(self, timeout=None)`: Other threads can call a thread’s join() method. This blocks the calling thread until the thread whose join() method is called is terminated.

    <!-- TODO: 思考 -- 待确认是否正确：与多进程不同，调用了线程的 start 之后，该线程会马上与主线程并行执行（而不是像多进程一样要等到父进程执行完毕后才开始子进程）。思考：这是因为创建和切换一个线程的开销要比创建进程的开销小的多，因此不会产生多进程中的那种父进程先执行完后再开始子进程的“错觉“。 -->

  - `getName(self)`: 返回线程的名字。

  - `isAlive(self)`: 布尔标志，表示这个线程是否还在运行中。

  - `daemon`: A boolean value indicating whether this thread is a daemon thread (True) or not (False).

    NOTE:
    - **This must be set before start() is called, otherwise RuntimeError is raised**. **Its initial value is inherited from the creating thread**; the main thread is not a daemon thread and therefore all threads created in the main thread default to daemon = False.

    - **当所有 non-daemon 线程退出后，整个 Python 进程就会退出，也就是说 daemon 线程会被强制退出**。Their resources (such as open files, database transactions, etc.) may not be released properly. **If you want your threads to stop gracefully, make them non-daemonic and use a suitable signalling mechanism such as an Event**.

    - 主线程指的是 Python 进程的 initial thread of control，**主线程不是 daemon 线程**。

    - There is the possibility that “dummy thread objects” are created. These are thread objects corresponding to “alien threads”, which are threads of control started outside the threading module, such as directly from C code. Dummy thread objects have limited functionality; they are always considered alive and daemonic, and cannot be `join()`ed. They are never deleted, since it is impossible to detect the termination of alien threads.

  - `setDaemon(self: daemonic)` / `isDaemon(self)`: getter/setter API for daemon.

  - `setName(self, name)`: 设置线程的名字。

NOTE
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

- 自定义 Thread 类

  与 multiprocessing 模块相同，除了使用构造方法创建一个新的进程 / 线程，还可以通过继承 Thread 类并 override 其中的 `__init__()` and `run()` methods，来创建一个新的线程。

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

### 2.5. 线程间通信 (communication)

在同一个进程内，多个线程共享该进程的内存地址空间，因此在 Python 多线程程序中，只需要把变量定义为全局变量，即可让多个线程共享访问。尽管如此，threading 模块还提供了简化多线程通信的便捷操作类。

#### 2.5.1. Event Objects

> This is one of the simplest mechanisms for communication between threads: one thread signals an event and other threads wait for it.

An event object manages an internal flag that can be set to true with the `set()` method and reset to false with the `clear()` method. The `wait()` method blocks until the flag is true.

- CONSTRUCTION
  - py2: `threading.Event()`: A factory function that returns a new event object.
  - py3: `class threading.Event`: Class implementing event objects.

- API

  The internal flag is initially false.
  - `is_set()` / `isSet()`: Return true if and only if the internal flag is true.
  - `set()`: Set the internal flag to true. **All threads waiting for it to become true are awakened**. Threads that call `wait()` once the flag is true will not block at all.
  - `clear()`: Reset the internal flag to false. Subsequently, threads calling `wait()` will block until `set()` is called to set the internal flag to true again.
  - `wait([timeout])`: Block until the internal flag is true. This method returns the internal flag on exit, so it will always return True except if a timeout is given and the operation times out.

### 2.6. 线程间同步 (synchronization)

All of the objects provided by this module that have acquire() and release() methods can be used as context managers for a with statement. The acquire() method will be called when the block is entered, and release() will be called when the block is exited.

Currently, Lock, RLock, Condition, Semaphore, and BoundedSemaphore objects may be used as with statement context managers. For example:
```python
import threading

some_rlock = threading.RLock()
with some_rlock:
    print "some_rlock is locked while this executes"
```

#### 2.6.1. Lock Objects

Once a thread has acquired it, subsequent attempts to acquire it block, until it is released; any thread may release it.

Locks also support the context management protocol.
<!-- TODO: with statment -->

- CONSTRUCTION
  - py2: `threading.Lock()`: A factory function that returns a new primitive lock object.
  - py3: `class threading.Lock`: The class implementing primitive lock objects.

- API

  All methods are executed atomically.
  - `Lock.acquire([blocking])`：获得线程的互斥锁。
  - `Lock.release()`：释放线程的互斥锁。

- IMPLEMENTATION
  ```python
  # lib/python2.7/threading.py
  # lib/python3.7/threading.py
  Lock = _allocate_lock
  _allocate_lock = thread.allocate_lock
  ```
  可见，`Lock()` 实际上是直接调用了底层 `thread` / `_thread` 模块的 `allocate_lock()` 来实现的。

- e.g.
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
  当多个线程同时执行 lock.acquire() 时，只有一个线程能成功地获取锁，然后继续执行代码，其他线程就继续等待直到获得锁为止。获得锁的线程用完后一定要释放锁，否则那些苦苦等待锁的线程将永远等待下去，成为死线程。为防止资源占用的操作出异常后 release 无法调用，而导致其它线程一直等待解锁而成为“死线程”，需要用 try...finally 来确保锁一定会被释放。

#### 2.6.2. RLock Objects

A reentrant lock must be released by the thread that acquired it. Once a thread has acquired a reentrant lock, the same thread may acquire it again without blocking; the thread must release it once for each time it has acquired it.

RLock（可重入锁）是一个可以被同一个线程请求多次的同步指令。RLock 使用了“拥有的线程”和“递归等级”的概念，处于锁定状态时，RLock 被某个线程拥有。拥有 RLock 的线程可以再次调用 acquire()，释放锁时需要调用 release() 相同次数。

可以认为 RLock 包含一个锁定池和一个初始值为 0 的计数器，每次成功调用 acquire()/release()，计数器将 +1/-1，为 0 时锁处于未锁定状态。

- CONSTRUCTION
  - py2: `threading.RLock()`: A factory function that returns a new reentrant lock object.
  - py3: `class threading.RLock`: This class implements reentrant lock objects.

- API
  - `acquire(blocking=True, timeout=-1)`: Acquire a lock, blocking or non-blocking.
  - `release()`: Release a lock, decrementing the recursion level.

- IMPLEMENTATION
  - py2

    由于在 Python2 的多线程底层 thread 模块中没有对 RLock 的实现，因此 threaidng.RLock 在 thread 上层实现了一个 `_RLock` class，实现了可重入锁的逻辑：
    ```python
    class _RLock(_Verbose):
        # pass
    # ...
    def RLock(*args, **kwargs):
        return _RLock(*args, **kwargs)
    ```

  - py3
    由于在 Python3 的多线程底层 _thread 模块中直接提供了 RLock 的实现，因此 threading.RLock 直接调用即可，如果调用失败才 rollback 到 threading 自己实现的 `_RLock`：
    ```python
    class _RLock(_Verbose):
        # pass
    # ...
    try:
        _CRLock = _thread.RLock
    except AttributeError:
        _CRLock = None
    # ...
    _PyRLock = _RLock
    # ...
    def RLock(*args, **kwargs):
        if _CRLock is None:
            return _PyRLock(*args, **kwargs)
        return _CRLock(*args, **kwargs)
    ```

- e.g.
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

#### 2.6.3. Condition Objects

A condition variable is always associated with some kind of lock; this can be passed in or one will be created by default. (Passing one in is useful when several condition variables must share the same lock.)

> A condition variable allows one or more threads to wait until they are notified by another thread.

- CONSTRUCTION
  - py2: `threading.Condition()`: A factory function that returns a new condition variable object.
  - py3: `class threading.Condition(lock=None)`: This class implements condition variable objects.

- API

  If the lock argument is given and not None, it must be a Lock or RLock object, and it is used as the underlying lock. Otherwise, a new RLock object is created and used as the underlying lock.

  - `acquire(*args)`: Acquire the underlying lock. This method calls the corresponding method on the underlying lock; the return value is whatever that method returns.
  - `release()`: Release the underlying lock. This method calls the corresponding method on the underlying lock; there is no return value.

  - `wait([timeout])`: Wait until notified or until a timeout occurs. If the calling thread has not acquired the lock when this method is called, a RuntimeError is raised.

    This method releases the underlying lock, and then blocks until it is awakened by a notify() or notifyAll() call for the same condition variable in another thread, or until the optional timeout occurs. Once awakened or timed out, it re-acquires the lock and returns.

  - `notify(n=1)`: By default, wake up one thread waiting on this condition, if any. If the calling thread has not acquired the lock when this method is called, a RuntimeError is raised. This method **wakes up at most n of the threads waiting for the condition variable**; it is a no-op if no threads are waiting.

  - `notify_all()` / `notifyAll()`: Wake up all threads waiting on this condition.

- e.g.

  a generic producer-consumer situation with unlimited buffer capacity:
  ```python
  # Consume one item
  cv.acquire()
  while not an_item_is_available():
      cv.wait()
  get_an_available_item()
  cv.release()

  # Produce one item
  cv.acquire()
  make_an_item_available()
  cv.notify()
  cv.release()
  ```

#### 2.6.4. Semaphore Objects

> A semaphore manages a counter representing the number of release() calls minus the number of acquire() calls, plus an initial value. The acquire() method blocks if necessary until it can return without making the counter negative. If not given, value defaults to 1.

Semaphores also support the context management protocol.

信号量 Semaphore 对象维护着一个内部计数器，调用 acquire() 方法时该计数器减 1，调用 release() 方法时该计数器加 1，适用于需要控制特定资源的并发访问线程数量的场合，如连接池，控制线程最大运行数目。

- CONSTRUCTION
  - py2: `threading.Semaphore([value])`: A factory function that returns a new semaphore object.
  - py3: `class threading.Semaphore(value=1)`: This class implements semaphore objects.

- API
  - `acquire([timeout])`: 请求 Semaphore。如果计数器为 0，将阻塞线程至同步阻塞状态；否则将计数器 -1 并立即返回。
  - `release()`: 释放 Semaphore，将计数器 +1，如果使用 BoundedSemaphore，还将进行释放次数检查。release() 方法不检查线程是否已获得 Semaphore。

- e.g.
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

#### 2.6.5. BoundedSemaphore Objects

> A bounded semaphore checks to make sure its current value doesn’t exceed its initial value. If it does, ValueError is raised. In most situations semaphores are used to guard resources with limited capacity. If the semaphore is released too many times it’s a sign of a bug. If not given, value defaults to 1.

有限信号量 BoundedSemaphore，可以确保 release() 方法的调用次数不能超过给定的初始信号量数值 Semaphore 对象可以调用任意次 release() 方法，而 BoundedSemaphore 对象可以保证计数器的值不超过特定的值。

- CONSTRUCTION
  - py2: `threading.BoundedSemaphore([value])`: A factory function that returns a new bounded semaphore object.
  - py3: `class threading.BoundedSemaphore(value=1)`: Class implementing bounded semaphore objects.

- e.g.
  ```python
  maxconnections = 5
  # ...
  pool_sema = BoundedSemaphore(value=maxconnections)
  with pool_sema:
      conn = connectdb()
      try:
          # ... use connection ...
      finally:
          conn.close()
  ```

#### 2.6.6. Barrier Objects

**Barrier is new in version py3.2**.

This class provides a simple synchronization primitive for use by a fixed number of threads that need to wait for each other. Each of the threads tries to pass the barrier by calling the wait() method and will block until all of the threads have made their wait() calls. At this point, the threads are released simultaneously.

The barrier can be reused any number of times for the same number of threads.

- CONSTRUCTION

  `class threading.Barrier(parties, action=None, timeout=None)`: Create a barrier object for parties number of threads. An action, when provided, is a callable to be called by one of the threads when they are released. timeout is the default timeout value if none is specified for the wait() method.

- API
  - `wait(timeout=None)`: Pass the barrier. When all the threads party to the barrier have called this function, they are all released simultaneously. If a timeout is provided, it is used in preference to any that was supplied to the class constructor.

  - `reset()`: Return the barrier to the default, empty state. Any threads waiting on it will receive the BrokenBarrierError exception.

  - `abort()`: Put the barrier into a broken state. This causes any active or future calls to wait() to fail with the BrokenBarrierError. Use this for example if one of the needs to abort, to avoid deadlocking the application.

  - `parties`: The number of threads required to pass the barrier.

  - `n_waiting`: The number of threads currently waiting in the barrier.

  - `broken`: A boolean that is True if the barrier is in the broken state.

- e.g.
  ```python
  b = Barrier(2, timeout=5)

  def server():
      start_server()
      b.wait()
      while True:
          connection = accept_connection()
          process_server_connection(connection)

  def client():
      b.wait()
      while True:
          connection = make_connection()
          process_client_connection(connection)
  ```

### 2.7. Thread-Local Data

在多线程环境下，每个线程都有自己的数据。一个线程使用自己的局部变量比使用全局变量好，因为局部变量只有线程自己能看见，不会影响其他线程，而全局变量的修改必须加锁。

为解决了参数在一个线程中各个函数之间互相传递的问题（线程内的“全局变量”），threading 模块提供了 Thread-Local Data 的封装类 threading.local。threading.local 对象虽然是全局变量，但每个线程都只能读写自己线程的独立副本，互不干扰。

- CONSTRUCTION

  `class threading.local`: A class that represents thread-local data. 底层基于 `_threading_local` 模块实现。

- e.g.
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

  t1 = threading.Thread(target=process_thread, args=('Alice',))
  t2 = threading.Thread(target=process_thread, args=('Bob',))
  t1.start()
  t2.start()
  t1.join()
  t2.join()
  ```

  全局变量 local_school 就是一个 ThreadLocal 对象，每个 Thread 对它都可以读写 student 属性，但互不影响。你可以把 local_school 看成全局变量，但每个属性如 local_school.student 都是线程的局部变量，可以任意读写而互不干扰，也不用管理锁的问题，ThreadLocal 内部会处理。

  可以理解为全局变量 local_school 是一个 dict，不但可以用 local_school.student，还可以绑定其他变量，如 local_school.teacher 等等。

- 应用：

  Thread-Local Data 最常用的地方就是为每个线程绑定一个数据库连接，HTTP 请求，用户身份信息等，这样一个线程的所有调用到的处理函数都可以非常方便地访问这些资源。

### 2.8. Timer

This class represents an action that should be run only after a certain amount of time has passed — a timer. **Timer is a subclass of Thread** and as such also functions as an example of creating custom threads.

Timers are started, as with threads, by calling their start() method. The timer can be stopped (before its action has begun) by calling the cancel() method. The interval the timer will wait before executing its action may not be exactly the same as the interval specified by the user.

- CONSTRUCTION

  `class threading.Timer(interval, function, args=[], kwargs={})`: A thread that executes a function after a specified interval has passed.

- API
  - `cancel()`: Stop the timer, and cancel the execution of the timer’s action. **This will only work if the timer is still in its waiting stage**.

- e.g.
  ```python
  def hello():
      print("hello, world")

  t = Timer(30.0, hello)
  t.start()  # after 30 seconds, "hello, world" will be printed
  ```

## 3. dummy 模块：dummy_thread / _dummy_thread / dummy_threading

- `dummy_thread`(Python2.7)

  [`dummy_thread`](https://docs.python.org/2.7/library/dummy_threading.html) module provides a duplicate interface to the threading module. It is meant to be imported when the thread module is not provided on a platform.
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
  NOTE: **Be careful to not use this module where deadlock might occur** from a thread being created that blocks waiting for another thread to be created. This often occurs with blocking I/O.

- `_dummy_thread`(Python 3.7)
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

- `dummy_threading`

  The `dummy_threading` module is provided for situations where threading cannot be used because thread is missing.
  ```
  dummy_threading — Drop-in replacement for the threading module.
      Faux ``threading`` version using ``dummy_thread`` instead of ``thread``.

      The module ``_dummy_threading`` is added to ``sys.modules`` in order
      to not have ``threading`` considered imported.  Had ``threading`` been
      directly imported it would have made all subsequent imports succeed
      regardless of whether ``thread`` was available which is not desired.

  ```

## 4. threading 封装模块：multiprocessing.dummy

> This package is intended to duplicate the functionality (and much of the API) of threading.py but uses processes instead of threads.

> A subpackage 'multiprocessing.dummy' has the same API but is a simple wrapper for 'threading'.

multiprocessing 在 python 中是一个多进程模块，而 multiprocessing.dummy 则是一个多线程模块，它是对 threading 模块的简单封装。

## 5. 同步阻塞队列模块：Queue / queue

Docs:
- https://docs.python.org/2.7/library/queue.html
- https://docs.python.org/3.7/library/queue.html

The `Queue` (py2) / `queue` (py3) module implements multi-producer, multi-consumer queues. It is especially useful in threaded programming when information must be exchanged safely between multiple threads. The Queue class in this module implements all the required locking semantics. It depends on the availability of thread support in Python, like `threading`.

- CONSTRUCTION
  - `class Queue.Queue(maxsize=0)`: Constructor for a **FIFO queue**. maxsize is an integer that sets the upperbound limit on the number of items that can be placed in the queue. Insertion will block once this size has been reached, until queue items are consumed. If maxsize is less than or equal to zero, the queue size is infinite.

  - `class Queue.LifoQueue(maxsize=0)`: Constructor for a **LIFO queue**. maxsize is an integer that sets the upperbound limit on the number of items that can be placed in the queue. Insertion will block once this size has been reached, until queue items are consumed. If maxsize is less than or equal to zero, the queue size is infinite.

  - `class Queue.PriorityQueue(maxsize=0)`: Constructor for a **priority queue**. maxsize is an integer that sets the upperbound limit on the number of items that can be placed in the queue. Insertion will block once this size has been reached, until queue items are consumed. If maxsize is less than or equal to zero, the queue size is infinite.

    The lowest valued entries are retrieved first (the lowest valued entry is the one returned by `sorted(list(entries))[0])`. A typical pattern for entries is a tuple in the form (priority_number, data).

  - `class queue.SimpleQueue`: (New in version py3.7.) Constructor for an **unbounded FIFO queue**. Simple queues lack advanced functionality such as task tracking.

- API

  Queue objects (Queue, LifoQueue, or PriorityQueue) provide the public methods described below:
  - `Queue.qsize()`: Return the approximate size of the queue. Note, qsize() > 0 doesn’t guarantee that a subsequent get() will not block, nor will qsize() < maxsize guarantee that put() will not block.

  - `Queue.empty()`: Return True if the queue is empty, False otherwise. If empty() returns True it doesn’t guarantee that a subsequent call to put() will not block. Similarly, if empty() returns False it doesn’t guarantee that a subsequent call to get() will not block.

  - `Queue.full()`: Return True if the queue is full, False otherwise. If full() returns True it doesn’t guarantee that a subsequent call to get() will not block. Similarly, if full() returns False it doesn’t guarantee that a subsequent call to put() will not block.

  - `Queue.put(item, block=True, timeout=None)`: Put item into the queue. If optional args block is true and timeout is None (the default), block if necessary until a free slot is available. If timeout is a positive number, it blocks at most timeout seconds and raises the Full exception if no free slot was available within that time. Otherwise (block is false), put an item on the queue if a free slot is immediately available, else raise the Full exception (timeout is ignored in that case).

  - `Queue.put_nowait(item)`: Equivalent to put(item, False).

  - `Queue.get(block=True, timeout=None)`: Remove and return an item from the queue. If optional args block is true and timeout is None (the default), block if necessary until an item is available. If timeout is a positive number, it blocks at most timeout seconds and raises the Empty exception if no item was available within that time. Otherwise (block is false), return an item if one is immediately available, else raise the Empty exception (timeout is ignored in that case).

    Prior to 3.0 on POSIX systems, and for all versions on Windows, if block is true and timeout is None, this operation goes into an uninterruptible wait on an underlying lock. This means that no exceptions can occur, and in particular a SIGINT will not trigger a KeyboardInterrupt.

  - `Queue.get_nowait()`: Equivalent to get(False).

  - `Queue.task_done()`: Indicate that a formerly enqueued task is complete. Used by queue consumer threads. For each `get()` used to fetch a task, a subsequent call to `task_done()` tells the queue that the processing on the task is complete.

    If a `join()` is currently blocking, it will resume when all items have been processed (meaning that a `task_done()` call was received for every item that had been `put()` into the queue).

  - `Queue.join()`: Blocks until all items in the queue have been gotten and processed.

    The count of unfinished tasks goes up whenever an item is added to the queue. The count goes down whenever a consumer thread calls `task_done()` to indicate that the item was retrieved and all work on it is complete. When the count of unfinished tasks drops to zero, `join()` unblocks.

- e.g.
  ```python
  def worker():
      while True:
          item = q.get()
          if item is None:
              break
          do_work(item)
          q.task_done()

  q = queue.Queue()
  threads = []
  for i in range(num_worker_threads):
      t = threading.Thread(target=worker)
      t.start()
      threads.append(t)

  for item in source():
      q.put(item)

  # block until all tasks are done
  q.join()

  # stop workers
  for i in range(num_worker_threads):
      q.put(None)
  for t in threads:
      t.join()
  ```

## 6. 周期性调度模块：sched

TODO: https://blog.csdn.net/Leonard_wang/article/details/54017537

Docs：
- https://docs.python.org/2.7/library/sched.html
- https://docs.python.org/3/library/sched.html



## 7. Refer Links

[Python Docs: 并发](https://docs.python.org/zh-cn/3/library/concurrency.html)

[菜鸟教程：Python 多线程](https://www.runoob.com/python/python-multithreading.html)

[Python线程指南](https://www.cnblogs.com/huxi/archive/2010/06/26/1765808.html)

TODO:

concurrent.futures.ThreadPoolExecutor 和 ProcessPoolExecutor