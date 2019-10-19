- [Python 并发：多进程](#python-并发多进程)
  - [1. 底层实现模块：_multiprocessing 模块](#1-底层实现模块_multiprocessing-模块)
  - [2. 高级封装模块：multiprocessing 模块](#2-高级封装模块multiprocessing-模块)
    - [2.1. 基本概念](#21-基本概念)
    - [2.2. 进程上下文 / 启动模式](#22-进程上下文--启动模式)
    - [2.3. 辅助方法](#23-辅助方法)
    - [2.4. Process Objects](#24-process-objects)
    - [2.5. 进程间同步 (synchronization)](#25-进程间同步-synchronization)
      - [2.5.1. Lock Objects](#251-lock-objects)
      - [2.5.2. RLock Objects](#252-rlock-objects)
      - [2.5.3. Semaphore Objects](#253-semaphore-objects)
      - [2.5.4. BoundedSemaphore Objects](#254-boundedsemaphore-objects)
      - [2.5.5. Condition Objects](#255-condition-objects)
      - [2.5.6. Event Objects](#256-event-objects)
      - [2.5.7. Barrier Objects](#257-barrier-objects)
    - [2.6. 进程间通信 (IPC)](#26-进程间通信-ipc)
      - [2.6.1. 阻塞同步队列：Queue Objects](#261-阻塞同步队列queue-objects)
      - [2.6.2. 管道：Pipe Objects](#262-管道pipe-objects)
      - [2.6.3. Shared memory](#263-shared-memory)
        - [2.6.3.1. multiprocessing.Value Object](#2631-multiprocessingvalue-object)
        - [2.6.3.2. multiprocessing.Array Object](#2632-multiprocessingarray-object)
        - [2.6.3.3. multiprocessing.SharedMemory Modules](#2633-multiprocessingsharedmemory-modules)
      - [2.6.4. Server process: Manager](#264-server-process-manager)
  - [3. subprocess 模块](#3-subprocess-模块)
  - [4. signal 模块](#4-signal-模块)
  - [5. mmap 模块](#5-mmap-模块)
  - [6. socket 模块](#6-socket-模块)
  - [7. ssl 模块](#7-ssl-模块)
  - [8. 进程池支持模块](#8-进程池支持模块)
    - [8.1. multiprocessing.Pool](#81-multiprocessingpool)
    - [8.2. concurrent.futures.ProcessPoolExecutor](#82-concurrentfuturesprocesspoolexecutor)
  - [9. Refer Links](#9-refer-links)

# Python 并发：多进程

## 1. 底层实现模块：_multiprocessing 模块

`_multiprocessing` 模块是 Python 中 build-in 的多进程底层实现模块，在解释器（如 cpython）中由底层代码（如 C++）实现。

`/multiprocessing/forking.py` 、`/multiprocessing/queues.py`、 `/multiprocessing/synchronize.py` 等子模块都引用了 `_multiprocessing` build-in 模块作为底层实现。

source code: cpython/Modules/_multiprocessing/multiprocessing.c

## 2. 高级封装模块：multiprocessing 模块

Docs:
- https://docs.python.org/2.7/library/multiprocessing.html
- https://docs.python.org/3/library/multiprocessing.html

### 2.1. 基本概念

> This package is intended to duplicate the functionality (and much of the API) of threading.py but uses processes instead of threads.  A subpackage 'multiprocessing.dummy' has the same API but is a simple wrapper for 'threading'. It runs on both Unix and Windows.

`multiprocessing` 模块是一个跨平台的多进程模块，底层基于 `_multiprocessing` 模块实现，提供了对 both local and remote concurrency 的支持。`multiprocessing` 模块包含绝大部分 `threading` 模块中的 API 的“进程”版本，但比 `threading` 模块中支持更多特性（如 Pool 支持）。

此外，`multiprocessing` 模块中的 `multiprocessing.dummy` 子模块是对 `threading` 模块的简单封装，因此也就是说，**`multiprocessing` 模块一定程度上“屏蔽”了 `threading` 模块，可以通过 `multiprocessing` 模块实现包括多线程和多进程的多种并发操作**。

### 2.2. 进程上下文 / 启动模式

[Contexts and start methods](https://docs.python.org/3/library/multiprocessing.html#contexts-and-start-methods)

根据具体平台 / 系统的不同，`multiprocessing` 模块支持 3 种进程的启动模式：
- **spawn**

  The parent process starts a fresh python interpreter process. **The child process will only inherit those resources necessary to run the process objects `run()` method**. In particular, unnecessary file descriptors and handles from the parent process will not be inherited.

  Starting a process using this method is rather slow compared to using fork or forkserver.

  **Available on Unix and Windows. The default on Windows**.

- **fork**

  The parent process uses `os.fork()` to fork the Python interpreter. The child process, when it begins, is effectively identical to the parent process. **All resources of the parent are inherited by the child process**.

  Note: **Safely forking a multithreaded process is problematic**.

  **Available on Unix only. The default on Unix**. 因此，在 Unix 系统中，`multiprocessing` 模块默认都是使用 `os.fork()` 来创建一个新的进程，也就是说所有创建的进程都是父进程的子进程。

- **forkserver**

  When the program starts and selects the forkserver start method, a server process is started. From then on, whenever a new process is needed, the parent process connects to the server and requests that it fork a new process. The fork server process is single threaded so it is safe for it to use `os.fork()`. No unnecessary resources are inherited.

  **Available on Unix platforms which support passing file descriptors over Unix pipes**.

`multiprocessing` 模块支持对默认启动模式进行修改，通过 `multiprocessing.set_start_method()` 方法，在 `if __name__ == '__main__'` 指定 spawn/fork/forkserver 即可：
```python
import multiprocessing as mp

def foo(q):
    q.put('hello')

if __name__ == '__main__':
    mp.set_start_method('spawn')
    q = mp.Queue()
    p = mp.Process(target=foo, args=(q,))
    p.start()
    print(q.get())
    p.join()
```

NOTE:
- `multiprocessing.set_start_method()` should not be used more than once in the program.

### 2.3. 辅助方法

- `multiprocessing.active_children()`：以 list 结构返回目前所有的运行的 Process 对象。

- `multiprocessing.cpu_count()`：返回当前机器的逻辑 CPU 核心数量，即物理核数 x cpu 数量 x 超线程数。May raise NotImplementedError.

- `multiprocessing.current_process()`：返回当前的进程对象。

- `multiprocessing.freeze_support()`: Add support for when a program which uses multiprocessing has been frozen to produce a Windows executable. (Has been tested with py2exe, PyInstaller and cx_Freeze.)
  ```python
  from multiprocessing import Process, freeze_support

  def f():
      print 'hello world!'

  if __name__ == '__main__':
      freeze_support()
      Process(target=f).start()
  ```
  If the `freeze_support()` line is omitted then trying to run the frozen executable will raise RuntimeError.

- `multiprocessing.set_executable()`: Sets the path of the Python interpreter to use when starting a child process. (By default sys.executable is used). Embedders will probably need to do some thing like

### 2.4. Process Objects

在 multiprocessing 中，每一个进程都用一个 Process 类来表示。

- CONSTRUCTION:

  ```python
  Process([group [, target [, name [, args [, kwargs]]]]])
  ```
  Options:
  - `target`: 表示调用对象，你可以传入函数名。
  - `args`: 表示被调用对象的位置参数元组，比如 target 是函数 a，他有两个参数 m，n，那么 args 就传入 (m, n) 即可。
  - `kwargs`: 表示调用对象的字典。
  - `name`: 是别名，相当于给这个进程取一个名字，如果不起名字 Python 就自动给线程命名为 Process-1，Process-2 等。
  - `group`: 分组，实际上不使用。

  e.g.
  ```python
  def process(num):
      time.sleep(num)
      print('Process:', num)

  if __name__ == '__main__':
      for i in range(5):		# 创建并启动 5 个进程
          p = multiprocessing.Process(target=process, args=(i,))
          p.start()

      print('CPU number:' + str(multiprocessing.cpu_count()))
      for p in multiprocessing.active_children():
          print('Child process name: ' + p.name + ' id: ' + str(p.pid))
      print('Process Ended')
  ```
  输出结果：
  ```
  Process: 0
  CPU number:8
  Child process name: Process-2 id: 9641
  Child process name: Process-4 id: 9643
  Child process name: Process-5 id: 9644
  Child process name: Process-3 id: 9642
  Process Ended
  Process: 1
  Process: 2
  Process: 3
  Process: 4
  ```

- API

  - `name`: 进程名

  - `pid`: Return the process ID. Before the process is spawned, this will be None.

  - `deamon`: 默认为 False；如果设置子进程为 True，当父进程结束后，子进程会自动被终止。可以有效的防止孤儿进程。

  - `exitcode`: The child’s exit code. This will be None if the process has not yet terminated. A negative value -N indicates that the child was terminated by signal N.

  - `start()`：开始执行 Process 对象的进程，进程执行完毕后自动销毁然后返回父进程。

    source code:

    multiprocessing/process.py
    ```python
    def _cleanup():
        # check for processes which have finished
        # Hint: All completed processes which have not yet been joined will be joined. Avoid a zombie process
        for p in list(_current_process._children):
            if p._popen.poll() is not None:
                _current_process._children.discard(p)
    class Process:
        def start(self):
            '''
            Start child process
            '''
            assert self._popen is None, 'cannot start a process twice'
            assert self._parent_pid == os.getpid(), \
                  'can only start a process object created by current process'
            assert not _current_process._daemonic, \
                  'daemonic processes are not allowed to have children'
            _cleanup()  # Avoid a zombie process
            if self._Popen is not None:
                Popen = self._Popen
            else:
                from .forking import Popen
            self._popen = Popen(self)  # 通过 Popen 对象来创建子进程并执行 run 方法
            # Avoid a refcycle if the target function holds an indirect
            # reference to the process object (see bpo-30775)
            del self._target, self._args, self._kwargs
            _current_process._children.add(self)

        def _bootstrap(self):
            from . import util
            global _current_process

            try:
                self._children = set()
                self._counter = itertools.count(1)
                try:
                    sys.stdin.close()
                    sys.stdin = open(os.devnull)
                except (OSError, ValueError):
                    pass
                _current_process = self
                util._finalizer_registry.clear()
                util._run_after_forkers()
                util.info('child process calling self.run()')
                try:
                    self.run()  # 此处调用了 run 方法，执行子进程的逻辑
                    exitcode = 0
                finally:
                    util._exit_function()
            except SystemExit, e:
              # ......
    ```
    multiprocessing/forking.py
    ```python
    class Popen(object):
        # Popen 对象封装了在不同平台中创建子进程的具体实现
        def __init__(self, process_obj):
            sys.stdout.flush()
            sys.stderr.flush()
            self.returncode = None

            self.pid = os.fork()
            if self.pid == 0:
                if 'random' in sys.modules:
                    import random
                    random.seed()
                code = process_obj._bootstrap()
                sys.stdout.flush()
                sys.stderr.flush()
                os._exit(code)
    ```
    NOTE
    - 同一个进程不能调用多次 start()，否则会 raise Exception。
    - 只能在当前进程内创建新的子进程，而不能在子进程中再创建子进程。
    - 只有非 daemon 进程才运行创建子进程。

  - `join([timeout])`：阻塞当前进程，直到调用 join 方法的 Process Object 执行完，再继续执行当前进程。

    source code: multiprocessing/process.py
    ```python
    class Process:
        def join(self, timeout=None):
            '''
            Wait until child process terminates
            '''
            assert self._parent_pid == os.getpid(), 'can only join a child process

            assert self._popen is not None, 'can only join a started process'
            res = self._popen.wait(timeout)
            if res is not None:
                _current_process._children.discard(self)
    ```
    NOTE: 对于所有调用了 `start()` 的 Process 对象，good practice 是为每个 Process 对象都调用 `join()` 方法。

    > On Unix when a process finishes but has not been joined it becomes a zombie. There should never be very many because **each time a new process starts (or `active_children()` is called) all completed processes which have not yet been joined will be joined. Also calling a finished process’s `Process.is_alive` will join the process**. Even so it is probably good practice to explicitly join all the processes that you start.

  - `terminate()`：Terminate the process. On Unix this is done using the `SIGTERM` signal; on Windows `TerminateProcess()` is used. Note that exit handlers and finally clauses, etc., will not be executed.

    NOTE
    - 如果在调用 terminate 前没有调用 join 方法先将主进程阻塞，可能会导致子进程还未执行就被 terminate。
    - 当调用这个函数的时候，子进程运行逻辑中的 exit 和 finally 代码段将不会执行。
    - 若有嵌套子进程，会导致调用该方法的进程的的子孙进程不会被终结，而是成为孤儿进程。
    - 若调用 terminate 方法的子进程使用了 shared resources，会造成这部分 resources 不再可用，可能会影响相关进程的作业，因此 it is probably best to only consider **using Process.terminate on processes which never use any shared resources**.

  - `is_alive()`：返回该进程是否存活。

  - `active_children()`：以 Process 对象的 list 返回目前所有的运行的进程。

  - `cpu_count()`：返回当前机器的 CPU 核心数量。

  - Customized Process

    通过继承 Process 类，可以对进程类进行自定义，只需将要在进程中执行的代码放在 run() 方法的实现即可。同时，可以把一些方法独立的写在每个类里封装好，等用的时候直接初始化一个类运行即可。

    e.g.
    ```python
    class MyProcess(Process):
        def __init__(self, loop):
            Process.__init__(self)
            self.loop = loop

        def run(self):
            for count in range(self.loop):
                time.sleep(1)
                print('Pid: ' + str(self.pid) + ' LoopCount: ' + str(count))

    if __name__ == '__main__':
        for i in range(2, 5):
            p = MyProcess(i)
            p.daemon = True # 父进程结束后会终止子进程
            p.start()
            p.join() # 父进程结束后会等待子进程执行完毕后再终止子进程
        print 'Main process Ended!'
    ```

NOTE
- 获取进程返回值

  由于进程间内存空间相互独立，因此要获取子进程的执行返回值需要通过进程间通信来完成。例：
  ```python
  def worker(procnum, return_dict):
      '''worker function'''
      print str(procnum) + ' represent!'
      return_dict[procnum] = procnum

  if __name__ == '__main__':
      manager = Manager()
      return_dict = manager.dict()
      jobs = []
      for i in range(5):
          p = multiprocessing.Process(target=worker, args=(i,return_dict))
          jobs.append(p)
          p.start()

      for proc in jobs:
          proc.join()
      # 最后的结果是多个进程返回值的集合
      print return_dict.values()
  ```

- On Unix a child process can make use of a shared resource created in a parent process using a global resource. However, **it is better to pass the object as an argument to the constructor for the child process**. Apart from making the code (potentially) **compatible with Windows** this also ensures that as long as **the child process is still alive the object will not be garbage collected in the parent process**. This might be important if some resource is freed when the object is garbage collected in the parent process.

  ```python
  from multiprocessing import Process, Lock

  def f():
      ... do something using "lock" ...

  if __name__ == '__main__':
      lock = Lock()
      for i in range(10):
          Process(target=f).start()
  ```
  should be rewritten as
  ```python
  from multiprocessing import Process, Lock

  def f(l):
      ... do something using "l" ...

  if __name__ == '__main__':
      lock = Lock()
      for i in range(10):
          Process(target=f, args=(lock,)).start()
  ```

- 在 Windows 平台上，以下代码会 raise RuntimeError:
  ```python
  from multiprocessing import Process

  def foo():
      print 'hello'

  p = Process(target=foo)
  p.start()
  ```
  Instead one should protect the “entry point” of the program by using `if __name__ == '__main__':` as follows:
  ```python
  from multiprocessing import Process, freeze_support

  def foo():
      print 'hello'

  if __name__ == '__main__':
      freeze_support()  # can be omitted if the program will be run normally instead of frozen
      p = Process(target=foo)
      p.start()
  ```
  This allows the newly spawned Python interpreter to safely import the module and then run the module’s `foo()` function. Similar restrictions apply if a pool or manager is created in the main module.

### 2.5. 进程间同步 (synchronization)

source code: lib/python/multiprocessing/synchronize.py

> multiprocessing contains equivalents of all the synchronization primitives from threading.

#### 2.5.1. Lock Objects

常用方法：
- `acquire()`：给进程加上互斥锁。

- `release()`：释放进程的互斥锁。

e.g.
```python
class MyProcess(Process):
    def __init__(self, loop, lock):
        Process.__init__(self)
        self.loop = loop
        self.lock = lock

    def run(self):
        for count in range(self.loop):
            time.sleep(0.1)
			# 在 print 方法的前后分别添加了获得锁和释放锁的操作，这样就能保证在同一时间只有一个 print 操作
            self.lock.acquire() # 加锁
            print('Pid: ' + str(self.pid) + ' LoopCount: ' + str(count)) # 执行占用边界资源的操作
            self.lock.release() # 解锁
if __name__ == '__main__':
    lock = Lock()
    for i in range(10, 15):
        p = MyProcess(i, lock)
        p.start()
```

#### 2.5.2. RLock Objects

#### 2.5.3. Semaphore Objects

信号量，是在进程同步过程中一个比较重要的角色。可以控制临界资源的数量，保证各个进程之间的互斥和同步。

Semaphore 用来控制对临界资源的访问数量，例如池的最大连接数。

信号量的使用主要是用来保护共享资源，使得资源在一个时刻只有一个进程（线程）所拥有。信号量的值为正的时候，说明它空闲。所测试的线程可以锁定而使用它。若为 0，说明它被占用，测试的线程要进入睡眠队列中，等待被唤醒。

e.g.

进程之间利用 Semaphore 做到同步和互斥，以及控制临界资源数量
```python
buffer = Queue(10)
empty = Semaphore(2)
full = Semaphore(0)
lock = Lock()

class Consumer(Process):
    def run(self):
        global buffer, empty, full, lock
        while True:
            full.acquire()
            lock.acquire()
            buffer.get()
            print('Consumer pop an element')
            time.sleep(1)
            lock.release()
            empty.release()
class Producer(Process):
    def run(self):
        global buffer, empty, full, lock
        while True:
            empty.acquire()
            lock.acquire()
            buffer.put(1)
            print('Producer append an element')
            time.sleep(1)
            lock.release()
            full.release()
if __name__ == '__main__':
    p = Producer()
    c = Consumer()
    p.daemon = c.daemon = True
    p.start()
    c.start()
    p.join()
    c.join()
    print 'Ended!'

# 定义了两个进程类，一个是消费者，一个是生产者
# 定义了一个共享队列，利用了 Queue 数据结构，然后定义了两个信号量，一个代表缓冲区空余数，一个表示缓冲区占用数
# 生产者 Producer 使用 empty.acquire() 方法来占用一个缓冲区位置，然后缓冲区空闲区大小减小 1，接下来进行加锁，对缓冲区进行操作。然后释放锁，然后让代表占用的缓冲区位置数量 +1，消费者则相反
```
输出：
```
Producer append an element
Consumer pop an element
Producer append an element
Consumer pop an element
Producer append an element
Consumer pop an element
Producer append an element
```
#### 2.5.4. BoundedSemaphore Objects

#### 2.5.5. Condition Objects

#### 2.5.6. Event Objects

#### 2.5.7. Barrier Objects

### 2.6. 进程间通信 (IPC)

Process 之间肯定是需要通信的，操作系统提供了很多机制来实现进程间的通信。Python 的 multiprocessing 模块包装了底层的机制，提供了 Queue、Pipes 等多种方式来交换数据。

NOTE:
- 在实际的应用开发中，应尽可能的避免在多个进程之间传递大量的数据。
- 在需要 IPC 的场景中，应尽可能的使用 queues or pipes，而不是直接使用来自 `threading` 模块的底层 synchronization primitives。

#### 2.6.1. 阻塞同步队列：Queue Objects

multiprocessing.Queue（不是 queue.Queue），是一种多线程优先队列。它允许多个进程读和写，可以作为进程间通信的共享队列使用。

如果你把 Queue 换成普通的 list，是完全起不到效果的。即使在一个进程中改变了这个 list，在另一个进程也不能获取到它的状态。

- 构造方法

  ```
  mutiprocessing.Queue(maxsize)
  # maxsize 表示队列中可以存放对象的最大数量
  ```

- 常用方法
  - `Queue.empty()`: 如果队列为空，返回 True, 反之 False。

  - `Queue.full()`: 如果队列满了，返回 True, 反之 False。

  - `Queue.put(obj[, block[, timeout]])`: 添加元素到队列；。

  - `Queue.get([block[, timeout]])`: 返回队列中的一个元素后将其从队列中删除。

    NOTE: put 方法和 get 方法有两个参数，blocked 和 timeout，分别表示是否阻塞和超时时间。默认 blocked 是 true，即阻塞式。
    - 当一个队列为空的时候如果再用 get 取则会阻塞后无法返回，所以这时候就需要吧 blocked 设置为 false，即非阻塞式，实际上它就会调用 get_nowait() 方法，此时还需要设置一个超时时间，在这么长的时间内还没有取到队列元素，那就抛出 Queue.Empty 异常。
    - 当一个队列为满的时候如果再用 put 放则会阻塞，所以这时候就需要吧 blocked 设置为 false，即非阻塞式，实际上它就会调用 put_nowait() 方法，此时还需要设置一个超时时间，在这么长的时间内还没有放进去元素，那就抛出 Queue.Full 异常。

  - `Queue.get_nowait()`: 非阻塞的获取队列元素，相当 Queue.get(False)。

  - `Queue.put_nowait(item)`: 非阻塞的添加元素到队列中，相当 Queue.put(item, False)。

  - `Queue.qsize()`: 返回队列的大小 ，不过在 Mac OS 上没法运行。

e.g.
```python
# 写数据进程执行的代码
def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码
def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s from queue.' % value)

if __name__ == '__main__':
    # 父进程创建 Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    pw.start()
    pr.start()
    # 等待 pw 结束：
    pw.join()
    # pr 进程里是死循环，无法等待其结束，只能强行终止：
    pr.terminate()
```

#### 2.6.2. 管道：Pipe Objects

Pipe 管道可以是单向 (half-duplex)，也可以是双向 (duplex)。我们通过 mutiprocessing.Pipe(duplex=False) 创建单向管道 （默认为双向)。一个进程从 PIPE 一端输入对象，然后被 PIPE 另一端的进程接收，单向管道只允许管道一端的进程输入，而双向管道则允许从两端输入。

e.g.
```python
#声明了一个默认为双向的管道，然后将管道的两端分别传给两个进程。两个进程互相收发。
class Consumer(Process):
    def __init__(self, pipe):
        Process.__init__(self)
        self.pipe = pipe
    def run(self):
        self.pipe.send('Consumer Words')
        print 'Consumer Received:', self.pipe.recv()
class Producer(Process):
    def __init__(self, pipe):
        Process.__init__(self)
        self.pipe = pipe
    def run(self):
        print 'Producer Received:', self.pipe.recv()
        self.pipe.send('Producer Words')

if __name__ == '__main__':
    pipe = Pipe()
    p = Producer(pipe[0])
    c = Consumer(pipe[1])
    p.daemon = c.daemon = True
    p.start()
    c.start()
    p.join()
    c.join()
print 'Ended!'
```

输出结果：
```
Producer Received: Consumer Words
Consumer Received: Producer Words
Ended!
```

#### 2.6.3. Shared memory

TODO:

https://docs.python.org/2.7/library/multiprocessing.html#shared-ctypes-objects

https://my.oschina.net/dragondjf/blog/169321

https://blog.csdn.net/kongxx/article/details/77764878

https://zhuanlan.zhihu.com/p/41172891

##### 2.6.3.1. multiprocessing.Value Object

使用 Value Objects 可以将一个值存放在共享内存中。

- CONSTRUCTION

  `multiprocessing.Value(typecode_or_type, *args[, lock])`: Return a `ctypes` object allocated from shared memory. By default the return value is actually a synchronized wrapper for the object.

  Options:
  - `typecode_or_type`: determines the type of the returned object: it is either a `ctypes` type or a one character typecode of the kind used by the [`array`](https://docs.python.org/3/library/array.html) module.

    | Type code | C Type             | Python Type       | Minimum size in bytes |
    | --------- | ------------------ | ----------------- | --------------------- |
    | `'b'`     | signed char        | int               | 1                     |
    | `'B'`     | unsigned char      | int               | 1                     |
    | `'u'`     | Py_UNICODE         | Unicode character | 2                     |
    | `'h'`     | signed short       | int               | 2                     |
    | `'H'`     | unsigned short     | int               | 2                     |
    | `'i'`     | signed int         | int               | 2                     |
    | `'I'`     | unsigned int       | int               | 2                     |
    | `'l'`     | signed long        | int               | 4                     |
    | `'L'`     | unsigned long      | int               | 4                     |
    | `'q'`     | signed long long   | int               | 8                     |
    | `'Q'`     | unsigned long long | int               | 8                     |
    | `'f'`     | float              | float             | 4                     |
    | `'d'`     | double             | float             | 8                     |

  - `*args`: is passed on to the constructor for the type.

  - `lock`:
    - **If lock is True (the default)** then a new recursive lock object is created to synchronize access to the value. If lock is a Lock or RLock object then that will be used to synchronize access to the value.
    - If lock is False then access to the returned object will not be automatically protected by a lock, so it will not necessarily be “process-safe”.

- e.g.

  ```python
  from multiprocessing import Process, Value
  import time
  import random

  def save_money(money):
      for i in range(100):
          time.sleep(0.1)
          money.value += random.randint(1,200)

  def take_money(money):
      for i in range(100):
          time.sleep(0.1)
          money.value -= random.randint(1,150)

  if __name__ == '__main__':
      # money 为共享内存对象，给他一个初始值 2000，类型为 int 型“i”，相当于开辟了一个空间，同时绑定值 2000
      money = Value('i',2000)
      d = Process(target=save_money,args=(money,))
      w = Process(target=take_money,args=(money,))
      d.start()
      w.start()
      d.join()
      w.join()

      print(money.value)
  ```

- IMPLEMENT

  `multiprocessing/__init__.py`
  ```python
  def Value(typecode_or_type, *args, **kwds):
      '''
      Returns a synchronized shared object
      '''
      from multiprocessing.sharedctypes import Value
      return Value(typecode_or_type, *args, **kwds)
  ```

  `multiprocessing/sharedctypes.py`
  ```python
  prop_cache = {}
  template = '''
  def get%s(self):
      self.acquire()
      try:
          return self._obj.%s
      finally:
          self.release()
  def set%s(self, value):
      self.acquire()
      try:
          self._obj.%s = value
      finally:
          self.release()
  %s = property(get%s, set%s)
  '''

  def make_property(name):
      try:
          return prop_cache[name]
      except KeyError:
          d = {}
          exec template % ((name,)*7) in d
          prop_cache[name] = d[name]
          return d[name]

  # Synchronized wrapper
  class SynchronizedBase(object):
      def __init__(self, obj, lock=None):
          self._obj = obj
          self._lock = lock or RLock()
          self.acquire = self._lock.acquire
          self.release = self._lock.release
      def __reduce__(self):
          assert_spawning(self)
          return synchronized, (self._obj, self._lock)
      def get_obj(self):
          return self._obj
      def get_lock(self):
          return self._lock
      def __repr__(self):
          return '<%s wrapper for %s>' % (type(self).__name__, self._obj)
  class Synchronized(SynchronizedBase):
      value = make_property('value')

  def synchronized(obj, lock=None):
      assert not isinstance(obj, SynchronizedBase), 'object already synchronized'

      if isinstance(obj, ctypes._SimpleCData):
          return Synchronized(obj, lock)
      elif isinstance(obj, ctypes.Array):
          if obj._type_ is ctypes.c_char:
              return SynchronizedString(obj, lock)
          return SynchronizedArray(obj, lock)
      else:
          cls = type(obj)
          try:
              scls = class_cache[cls]
          except KeyError:
              names = [field[0] for field in cls._fields_]
              d = dict((name, make_property(name)) for name in names)
              classname = 'Synchronized' + cls.__name__
              scls = class_cache[cls] = type(classname, (SynchronizedBase,), d)
          return scls(obj, lock)

  def _new_value(type_):
      size = ctypes.sizeof(type_)
      wrapper = heap.BufferWrapper(size)
      return rebuild_ctype(type_, wrapper, None)

  def RawValue(typecode_or_type, *args):
      '''
      Returns a ctypes object allocated from shared memory
      '''
      type_ = typecode_to_type.get(typecode_or_type, typecode_or_type)
      obj = _new_value(type_)
      ctypes.memset(ctypes.addressof(obj), 0, ctypes.sizeof(obj))
      obj.__init__(*args)
      return obj

  def Value(typecode_or_type, *args, **kwds):
      '''
      Return a synchronization wrapper for a Value
      '''
      lock = kwds.pop('lock', None)
      if kwds:
          raise ValueError('unrecognized keyword argument(s): %s' % kwds.keys())
      obj = RawValue(typecode_or_type, *args)
      if lock is False:
          return obj
      if lock in (True, None):  # 此处可见，Value 对象默认带 lock，除非指定 lock=False
          lock = RLock()
      if not hasattr(lock, 'acquire'):
          raise AttributeError("'%r' has no method 'acquire'" % lock)
      return synchronized(obj, lock)
  ```

  `multiprocessing/heap.py`
  ```python
  class Heap(object):
      # ......
      def _malloc(self, size):
        # returns a large enough block -- it might be much larger
        i = bisect.bisect_left(self._lengths, size)
        if i == len(self._lengths):
            length = self._roundup(max(self._size, size), mmap.PAGESIZE)
            self._size *= 2
            info('allocating a new mmap of length %d', length)
            arena = Arena(length)
            self._arenas.append(arena)
            return (arena, 0, length)
        else:
            length = self._lengths[i]
            seq = self._len_to_seq[length]
            block = seq.pop()
            if not seq:
                del self._len_to_seq[length], self._lengths[i]

        (arena, start, stop) = block
        del self._start_to_block[(arena, start)]
        del self._stop_to_block[(arena, stop)]
        return block
      def malloc(self, size):
          # return a block of right size (possibly rounded up)
          assert 0 <= size < sys.maxint
          if os.getpid() != self._lastpid:
              self.__init__()                     # reinitialize after fork
          self._lock.acquire()
          self._free_pending_blocks()
          try:
              size = self._roundup(max(size,1), self._alignment)
              (arena, start, stop) = self._malloc(size)
              new_stop = start + size
              if new_stop < stop:
                  self._free((arena, new_stop, stop))
              block = (arena, start, new_stop)
              self._allocated_blocks.add(block)
              return block
          finally:
              self._lock.release()

  class BufferWrapper(object):
      _heap = Heap()
      def __init__(self, size):
          assert 0 <= size < sys.maxint
          block = BufferWrapper._heap.malloc(size)
          self._state = (block, size)
          Finalize(self, BufferWrapper._heap.free, args=(block,))

  ```
  可见，`Value` 所创建的共享内存在底层实现是调用了 `mmap` 模块进行内存的分配。

##### 2.6.3.2. multiprocessing.Array Object

使用 Array Objects 可以将多个数据存放在内存中，但要求数据类型一致。

- CONSTRUCTION

  `multiprocessing.Array(typecode_or_type, size_or_initializer, *, lock=True)`: Return a `ctypes` array allocated from shared memory. By default the return value is actually a synchronized wrapper for the array.

  Option:
  - `typecode_or_type`: determines the type of the elements of the returned array: it is either a ctypes type or a one character typecode of the kind used by the array module.
  - `size_or_initializer`:
    - If size_or_initializer is an integer, then it determines the length of the array, and the array will be initially zeroed.
    - Otherwise, size_or_initializer is a sequence which is used to initialize the array and whose length determines the length of the array.
  - `lock`:
    - **If lock is True (the default)** then a new lock object is created to synchronize access to the value. If lock is a Lock or RLock object then that will be used to synchronize access to the value.
    - If lock is False then access to the returned object will not be automatically protected by a lock, so it will not necessarily be “process-safe”.

  NOTE
  ```python
  array = mp.Array('i', [1, 2, 3, 4])
  ```
  这里的 Array 和 numpy 中的不同，它只能是一维的，不能是多维的。同样和 Value 一样，需要定义数据形式，否则会报错。

  错误形式
  ```python
  array = mp.Array('i', [[1, 2], [3, 4]]) # 2 维 list
  ```
  运行结果：
  """
  TypeError: an integer is required
  """

- e.g.
  ```python
  from multiprocessing import Process,Array
  import time

  def fun(m,n):
      for i in range(n):
          m[i]=i

  m = Array('i',5)

  p = Process(target= fun,args=(m,5))
  p.start()

  time.sleep(1)
  for i in m:
      print(i)

  p.join()
  ```

##### 2.6.3.3. multiprocessing.SharedMemory Modules

Python 在 2019-02-25 释出了 python 3.8 早期预览版 `3.8.0a2`，其中新增了 `multiprocessing.SharedMemory` 用以支持共享内存，大大提高多进程之间通信效率。

#### 2.6.4. Server process: Manager

> Managers provide a way to create data which can be shared between different processes.
>
> A manager object controls a server process which manages shared objects. Other processes can access the shared objects by using proxies.

`Managers` 模块是对进程间共享数据的高级封装，支持 `dict` / `list` / `Lock` / `Value` / `Array` / `Semaphore` 等多种共享数据类型。

- CONSTRUCTION

  `multiprocessing.Manager()`: Returns a started `SyncManager` object which can be used for sharing objects between processes. The returned manager object corresponds to a spawned child process and has methods which will create shared objects and return corresponding proxies. Manager processes will be shutdown as soon as they are garbage collected or their parent process exits.

- API
  - `class multiprocessing.managers.BaseManager([address[, authkey]])`: Create a BaseManager object.
    - Options:
      - `address` is the address on which the manager process listens for new connections. If address is None then an arbitrary one is chosen.
      - `authkey` is the authentication key which will be used to check the validity of incoming connections to the server process. If authkey is None then current_process().authkey. Otherwise authkey is used and it must be a string.

    - `start([initializer[, initargs]])`: Start a subprocess to start the manager. If initializer is not None then the subprocess will call initializer(*initargs) when it starts.
    - `get_server()`: Returns a Server object which represents the actual server under the control of the Manager.
    - `connect()`: Connect a local manager object to a remote manager process.
    - `shutdown()`: Stop the process used by the manager.
    - `register(typeid[, callable[, proxytype[, exposed[, method_to_typeid[, create_method]]]]])`: A classmethod which can be used for registering a type or callable with the manager class.

  - `class multiprocessing.managers.SyncManager`: A subclass of BaseManager which can be used for the synchronization of processes. Objects of this type are returned by `multiprocessing.Manager()`.
    - `BoundedSemaphore([value])`: Create a shared threading.BoundedSemaphore object and return a proxy for it.
    - `Condition([lock])`: Create a shared threading.Condition object and return a proxy for it.
    - `Event()`: Create a shared threading.Event object and return a proxy for it.
    - `Lock()`: Create a shared threading.Lock object and return a proxy for it.
    - `Namespace()`: Create a shared Namespace object and return a proxy for it.
    - `Queue([maxsize])`: Create a shared Queue.Queue object and return a proxy for it.
    - `RLock()`: Create a shared threading.RLock object and return a proxy for it.
    - `Semaphore([value])`: Create a shared threading.Semaphore object and return a proxy for it.
    - `Array(typecode, sequence)`: Create an array and return a proxy for it.
    - `Value(typecode, value)`: Create an object with a writable value attribute and return a proxy for it.
    - `dict()` / `dict(mapping)` / `dict(sequence)`: Create a shared dict object and return a proxy for it.
    - `list()` / `list(sequence)`: Create a shared list object and return a proxy for it.

      NOTE: Modifications to mutable values or items in dict and list proxies will not be propagated through the manager, because **the proxy has no way of knowing when its values or items are modified**. To modify such an item, you can re-assign the modified object to the container proxy:
      ```python
      # create a list proxy and append a mutable object (a dictionary)
      lproxy = manager.list()
      lproxy.append({})
      # now mutate the dictionary
      d = lproxy[0]
      d['a'] = 1
      d['b'] = 2
      # at this point, the changes to d are not yet synced, but by reassigning the dictionary, the proxy is notified of the change
      lproxy[0] = d
      ```

  - `class multiprocessing.managers.Namespace`: A type that can register with SyncManager. A namespace object has no public methods, but does have writable attributes. Its representation shows the values of its attributes.

  - Customized managers

    To create one’s own manager, one creates a subclass of BaseManager and uses the register() classmethod to register new types or callables with the manager class.
    ```python
    from multiprocessing.managers import BaseManager

    class MathsClass(object):
        def add(self, x, y):
            return x + y
        def mul(self, x, y):
            return x * y

    class MyManager(BaseManager):
        pass

    MyManager.register('Maths', MathsClass)

    if __name__ == '__main__':
        manager = MyManager()
        manager.start()
        maths = manager.Maths()
        print maths.add(4, 3)         # prints 7
        print maths.mul(7, 8)         # prints 56
    ```

  - Remote manager

    It is possible to run a manager server on one machine and have clients use it from other machines (assuming that the firewalls involved allow it).

- e.g.
  ```python
  from multiprocessing import Process, Manager

  def f1(ns, l):
      ns.x += 1
      l.append(2)

  def f2(ns, l):
      ns.x -= 2
      l.append(3)

  if __name__ == '__main__':
      manager = Manager()
      ns = manager.Namespace()
      l = manager.list([1])
      ns.x = 0
      p1 = Process(target=f1, args=(ns, l))
      p2 = Process(target=f2, args=(ns, l))
      p1.start()
      p2.start()
      p1.join()
      p2.join()
      print(ns, l)
  ```
  打印结果：
  ```
  Namespace(x=-1) [1, 2, 3]
  ```

- IMPLEMENT

  `multiprocessing/__init__.py`
  ```python
  def Manager():
      '''
      Returns a manager associated with a running server process

      The managers methods such as `Lock()`, `Condition()` and `Queue()`
      can be used to create shared objects.
      '''
      from multiprocessing.managers import SyncManager
      m = SyncManager()
      m.start()
      return m
  ```
  `multiprocessing/managers.py`
  ```python

  ```

## 3. subprocess 模块

## 4. signal 模块

## 5. mmap 模块

## 6. socket 模块

## 7. ssl 模块

## 8. 进程池支持模块

### 8.1. multiprocessing.Pool

> One can create a pool of processes which will carry out tasks submitted to it with the Pool class.

在利用 Python 进行系统管理的时候，特别是同时操作多个文件目录，或者远程控制多台主机，并行操作可以节约大量的时间。当被操作对象数目不大时，可以直接利用 multiprocessing 中的 Process 动态成生多个进程，十几个还好，但如果是上百个，上千个目标，手动的去限制进程数量却又太过繁琐，此时可以发挥进程池的功效。

Pool 可以提供指定数量的进程，供用户调用，**当有新的请求提交到 pool 中时，如果池还没有满，那么就会创建一个新的进程用来执行该请求；但如果池中的进程数已经达到规定最大值，那么该请求就会等待，直到池中有进程结束，才会创建新的进程来它**。

Pool 的用法有阻塞和非阻塞两种方式。

-  CONSTRUCTION

  `class multiprocessing.Pool([processes[, initializer[, initargs[, maxtasksperchild]]]])`: A process pool object which controls a pool of worker processes to which jobs can be submitted. It supports asynchronous results with timeouts and callbacks and has a parallel map implementation.

  Options:
  - `processes`: is the number of worker processes to use. If processes is None then the number returned by `cpu_count()` is used. **在 Pool 对象创建时，就会把 processes 个进程全部创建好**。
  - `initializer`: If initializer is not None then each worker process will call `initializer(*initargs)` when it starts.
  - `maxtasksperchild`: is the number of tasks a worker process can complete before it will exit and be replaced with a fresh worker process, to enable unused resources to be freed. The default maxtasksperchild is None, which means worker processes will live as long as the pool.

    NOTE: A frequent pattern found in other systems (such as Apache, mod_wsgi, etc) to free resources held by workers is **to allow a worker within a pool to complete only a set amount of work before being exiting**, being cleaned up and a new process spawned to replace the old one.

- API
  - `apply_async(func[, args[, kwds[, callback]]])`:
    - 向进程池中添加进程，它是非阻塞的，即能同时运行多个进程，同时运行的最大进程数在创建进程池时指定，如果池中的正在运行的进程数已经达到最大值，那么还没有执行的进程就会等待，直到池中有进程结束，才能进入进程池运行。
    - 返回值：如果在函数 func 中返回一个值，那么 `apply_async(func, (msg, ))` 的返回值 func 返回值的 ApplyResult 对象（注意是对象，不是值本身）。
    - Callback: If callback is specified then it should be a callable which accepts a single argument. When the result becomes ready callback is applied to it (unless the call failed).

    e.g.
    ```python
    import multiprocessing
    import time

    def func(msg):
        return multiprocessing.current_process().name + '-' + msg

    if __name__ == "__main__":
        pool = multiprocessing.Pool(processes=4) # 创建 4 个进程
        results = []
        for i in xrange(10):
            msg = "hello %d" %(i)
            results.append(pool.apply_async(func, (msg, )))
        pool.close() # 关闭进程池，表示不能再往进程池中添加进程，需要在 join 之前调用
        pool.join() # 等待进程池中的所有进程执行完毕
        print ("Sub-process(es) done.")
        for res in results:
            print (res.get())
    ```

    e.g. 把耗时间（阻塞）的任务放到进程池中，然后指定回调函数（主进程负责执行）
    ```python
    from multiprocessing import Pool
    import requests
    import json
    import os

    def get_page(url):
        print('《进程 %s> get %s' %(os.getpid(),url))
        respone=requests.get(url)
        if respone.status_code == 200:
            return {'url':url,'text':respone.text}

    def pasrse_page(res):
        print('< 进程 %s> parse %s' %(os.getpid(),res['url']))
        parse_res='url:<%s> size:[%s]\n' %(res['url'],len(res['text']))
        with open('db.txt','a') as f:
            f.write(parse_res)

    if __name__ == '__main__':
        urls=[
            'https://www.baidu.com',
            'https://www.python.org',
            'http://www.sina.com.cn/'
        ]
        p=Pool(3)
        res_l=[]
        for url in urls:
            res=p.apply_async(get_page,args=(url,),callback=pasrse_page)
            res_l.append(res)

        p.close()
        p.join()
        print([res.get() for res in res_l]) # 拿到的是 get_page 的结果，其实完全没必要拿该结果，该结果已经传给回调函数处理了
    ```

  - `apply(func[, args[, kwds]])` 向进程池中添加进程，它是阻塞的，即需要串行的运行每个进程。`apply_async()` is better suited for performing work in parallel.

    source code:
    ```python
    # multiprocessing/pool.py
    def apply(self, func, args=(), kwds={}):
        '''
        Equivalent of `apply()` builtin
        '''
        assert self._state == RUN
        return self.apply_async(func, args, kwds).get()
    ```

    e.g.
    ```python
    def function(index):
        print('Start process: ', index)
        time.sleep(3)
        print('End process', index)

    if __name__ == '__main__':
        pool = Pool(processes=3)
        for i in range(4):
            pool.apply(function, (i,)) # 每个线程被添加到线程池后即执行
        print("Started processes")
        pool.close()
        pool.join()
    print("Subprocess done.")
    ```
    输出结果：
    ```
    Start process:  0
    End process 0
    Start process:  1
    End process 1
    Start process:  2
    End process 2
    Start process:  3
    End process 3
    Started processes
    Subprocess done.
    ```

    P.S. 在 Python 语言设计之初，需要通过 build-in 方法 apply 来调用用户自定义的函数：`apply(f,args,kwargs)`，随着语言的完善，后来才支持了直接使用函数名来调用函数：`f(*args,**kwargs)`。因此，multiprocessing.Pool 模块的 apply 方法实际上是 tries to provide a similar interface.

  - `map_async(func, iterable[, chunksize[, callback]])`: A variant of the `map()` method which returns a result object.
    - 向进程池中添加进程，会将 iterable 中的各个元素作为参数分别传给每个进程的 func。它是阻塞的，即只能同时运行一个进程，还没有执行的进程就会等待，直到池中有进程结束，才能进入进程池运行。
    - 返回值：如果在函数 func 中返回一个值，那么 `apply_async(func, (msg, ))` 的返回值 func 返回值的 MapResult 对象（注意是对象，不是值本身）。
    - Callback: If callback is specified then it should be a callable which accepts a single argument. When the result becomes ready callback is applied to it (unless the call failed). callback should complete immediately since otherwise the thread which handles the results will get blocked.

  - `map(func, iterable[, chunksize])` It blocks until the result is ready.

    source code:
    ```python
    # multiprocessing/pool.py
    def map(self, func, iterable, chunksize=None):
        '''
        Equivalent of `map()` builtin
        '''
        assert self._state == RUN
        return self.map_async(func, iterable, chunksize).get()
    ```

    P.S. 虽然 iterable 是一个迭代器，但在实际使用中，必须在整个队列都就绪后，程序才会开始运行子进程。

    e.g.
    ```python
    def function(index):
        print('Start process: ', index)
        time.sleep(3)
        print('End process', index)

    if __name__ == '__main__':
        pool = Pool(processes=4)
        index = [1, 2, 3, 4]
        pool.map(func=function, iterable=index)
    ```
    运行结果：
    ```
    Start process:  1
    Start process:  2
    Start process:  3
    Start process:  4
    End process 1
    End process 2
    End process 3
    End process 4
    ```

    NOTE: **map 和 apply 的比较**：
    - `pool.apply(f, args)`: f is only **executed in ONE of the workers of the pool**. So ONE of the processes in the pool will run f(args).
    - `pool.map(f, iterable)`: This method chops the iterable into a number of chunks which it submits to the process pool as separate tasks. So you **take advantage of all the processes in the pool**.

  - `imap(func, iterable[, chunksize])`: A lazier version of map(). The chunksize argument is the same as the one used by the map() method. For very long iterables using a large value for chunksize can make the job complete much faster than using the default value of 1.

  - `imap_unordered(func, iterable[, chunksize])`: The same as imap() except that the ordering of the results from the returned iterator should be considered arbitrary. (Only when there is only one worker process is the order guaranteed to be “correct”.)

  - `close()`: Prevents any more tasks from being submitted to the pool. Once all the tasks have been completed the worker processes will exit. 调用 join() 之前必须先调用 close()，调用 close() 之后就不能继续添加新的 Process 了。

  - `terminate()`: Stops the worker processes immediately without completing outstanding work. When the pool object is garbage collected terminate() will be called immediately.

  - `join()`: Wait for the worker processes to exit. One must call close() or terminate() before using join().

  - `class multiprocessing.pool.AsyncResult`: The class of the result returned by `Pool.apply_async()` and `Pool.map_async()`.

    - `get([timeout])`: Return the result when it arrives. If timeout is not None and the result does not arrive within timeout seconds then multiprocessing.TimeoutError is raised. If the remote call raised an exception then that exception will be reraised by get().

    - `wait([timeout])`: Wait until the result is available or until timeout seconds pass.

    - `ready()`: Return whether the call has completed.

    - `successful()`: Return whether the call completed without raising an exception. Will raise AssertionError if the result is not ready.

    e.g.
    ```python
    from multiprocessing import Pool
    import time

    def f(x):
        return x*x

    if __name__ == '__main__':
        pool = Pool(processes=4)              # start 4 worker processes

        result = pool.apply_async(f, (10,))   # evaluate "f(10)" asynchronously in a single process
        print result.get(timeout=1)           # prints "100" unless your computer is *very* slow

        print pool.map(f, range(10))          # prints "[0, 1, 4,..., 81]"

        it = pool.imap(f, range(10))
        print it.next()                       # prints "0"
        print it.next()                       # prints "1"
        print it.next(timeout=1)              # prints "4" unless your computer is *very* slow

        result = pool.apply_async(time.sleep, (10,))
        print result.get(timeout=1)           # raises multiprocessing.TimeoutError
    ```

### 8.2. concurrent.futures.ProcessPoolExecutor

New in version 3.2.

## 9. Refer Links

[莫烦：共享内存 shared memory](https://morvanzhou.github.io/tutorials/python-basic/multiprocessing/6-shared-memory/)

[python 基于 mmap 模块的 jsonmmap 实现本地多进程内存共享](https://my.oschina.net/dragondjf/blog/169321)

[python 学习笔记——多进程中共享内存 Value & Array](https://www.cnblogs.com/gengyi/p/8661235.html)

[Why is communication via shared memory so much slower than via queues?](https://stackoverflow.com/questions/25271723/why-is-communication-via-shared-memory-so-much-slower-than-via-queues)

[廖雪峰：python 分布式多进程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431929340191970154d52b9d484b88a7b343708fcc60000)

[Python 程序中的进程操作 - 进程池 (multiprocess.Pool）](https://www.cnblogs.com/nickchen121/p/11130258.html)

[multiprocessing.Pool: When to use apply, apply_async or map?](https://stackoverflow.com/questions/8533318/multiprocessing-pool-when-to-use-apply-apply-async-or-map)

TODO:

[How does multiprocessing.Manager() work in python?](https://stackoverflow.com/questions/9436757/how-does-multiprocessing-manager-work-in-python)