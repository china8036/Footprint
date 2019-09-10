- [Python 多任务：多进程](#python-多任务多进程)
  - [1. os.fork() 系统调用](#1-osfork-系统调用)
  - [2. multiprocessing 模块](#2-multiprocessing-模块)
    - [2.1. Process 类](#21-process-类)
      - [2.1.1. 构造方法](#211-构造方法)
      - [2.1.2. 属性](#212-属性)
      - [2.1.3. 方法](#213-方法)
      - [2.1.4. 自定义 Process 类](#214-自定义-process-类)
    - [2.2. Pool 类](#22-pool-类)
      - [2.2.1. 构造方法](#221-构造方法)
      - [2.2.2. 方法](#222-方法)
    - [2.3. 进程间同步类](#23-进程间同步类)
      - [2.3.1. Lock 类](#231-lock-类)
      - [2.3.2. Semaphore 类](#232-semaphore-类)
    - [2.4. 进程间通信 (IPC) 类](#24-进程间通信-ipc-类)
      - [2.4.1. Queue 类](#241-queue-类)
      - [2.4.2. Pipe 类](#242-pipe-类)
    - [2.5. 其它模块方法](#25-其它模块方法)
    - [2.6. managers 子模块](#26-managers-子模块)
  - [3. Refer Links](#3-refer-links)

# Python 多任务：多进程

## 1. os.fork() 系统调用

Unix/Linux 操作系统提供了一个 fork() 系统调用，它非常特殊。普通的函数调用，调用一次，返回一次，但是 fork() 调用一次，返回两次，因为操作系统自动把当前进程（称为父进程）复制了一份（称为子进程），然后，分别在父进程和子进程内返回。子进程永远返回 0，而父进程返回子进程的 ID。这样做的理由是，一个父进程可以 fork 出很多子进程，所以，父进程要记下每个子进程的 ID，而子进程只需要调用 getppid() 就可以拿到父进程的 ID。

Python 的 os 模块封装了常见的系统调用，其中就包括 fork，可以在 Python 程序中轻松创建子进程：
```python
import os
print('Process (%s) start...' % os.getpid())
# Only works on Unix/Linux/Mac:
pid = os.fork()
if pid == 0:
    print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid()))
else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))
```

运行结果：
```
Process (876) start...
I (876) just created a child process (877).
I am child process (877) and my parent is 876.
```

应用：一个进程在接到新任务时就可以复制出一个子进程来处理新任务，常见的 Apache 服务器就是由父进程监听端口，每当有新的 http 请求时，就 fork 出子进程来处理新的 http 请求。

缺陷：由于 Windows 系统没有提供 fork() 系统调用，os.fork() 在 Windows 上无法运行。

## 2. multiprocessing 模块

Python 是跨平台的，自然也应该提供一个跨平台的多进程支持，multiprocessing 模块就是跨平台版本的多进程模块。

另外，multiprocessing 的执行需要“模拟”出 fork 的效果，即父进程所有 Python 对象都必须通过 pickle 序列化再传到子进程去，所以，如果 multiprocessing 在 Windows 下调用失败了，要先考虑是不是 pickle 失败了。

### 2.1. Process 类

在 multiprocessing 中，每一个进程都用一个 Process 类来表示。

#### 2.1.1. 构造方法

```python
Process([group [, target [, name [, args [, kwargs]]]]])
# 参数说明：
# target 表示调用对象，你可以传入函数名
# args 表示被调用对象的位置参数元组，比如 target 是函数 a，他有两个参数 m，n，那么 args 就传入 (m, n) 即可
# kwargs 表示调用对象的字典
# name 是别名，相当于给这个进程取一个名字，如果不起名字 Python 就自动给线程命名为 Process-1，Process-2……
# group 分组，实际上不使用
```

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

#### 2.1.2. 属性

- name：进程名

- pid：进程 id

- deamon：默认为 False；每个线程都可以单独设置它的属性，如果设置为 True，当父进程结束后，子进程会自动被终止。这样可以有效防止无控制地生成子进程。如果这样写了，你在关闭这个主程序运行时，就无需额外担心子进程有没有被关闭了

#### 2.1.3. 方法

- `start()`：<!-- TODO: 在父进程所有代码执行完毕后 -->，开始执行 Process 对象的进程；每个进程最多调用一次；进程执行完毕后自动销毁然后返回父进程。

- `join([timeout])`：阻塞父进程，直到调用 join 方法的那个子进程执行完，再继续执行当前进程，通常用于进程间的同步。

  NOTE
  - 一旦某一个子进程调用了 join 方法，将父进程阻塞了，此时所有已经调用 start 的子进程都会开始并行执行，直到调用 join 的子进程返回了，主进程才继续执行（此时若其它子进程还没执行完毕，则主进程会与它们并行执行）。
  - 若不调用 join 方法，会将父进程的所有代码执行完毕后，再并行的执行所有已经调用了 start 方法的子进程。

- `terminate()`：强制终止调用该方法的子进程，通常用于子进程死循环的终止。

  NOTE
  - 如果在调用 terminate 前没有调用 join 方法先将主进程阻塞，可能会导致子进程还未执行就被 terminate。
  - 当调用这个函数的时候，子进程运行逻辑中的 exit 和 finally 代码段将不会执行。
  - 若有嵌套子进程，会导致调用该方法的进程的的子孙进程不会被终结，而是成为孤儿进程。

  <!-- TODO: 思考 -- 待确认是否正确：执行顺序的问题：真的是父进程所有代码执行过一遍之后，再开始并行执行调用了 start 的子进程？或者说主进程是和所有子进程并行执行的，造成这种错觉的原因是因为创建核开始执行子进程需要时间开销，所以在调用了 start 之后，时间开销还没结束，还没真正开始执行之前，主进程就已经运行完所有代码了？所以，正因为这样，才需要调用 join 方法先将主进程阻塞？ -->

- `is_alive()`：返回该进程是否存活。

- `active_children()`：以 Process 对象的 list 返回目前所有的运行的进程。

- `cpu_count()`：返回当前机器的 CPU 核心数量。

#### 2.1.4. 自定义 Process 类

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

### 2.2. Pool 类

在利用 Python 进行系统管理的时候，特别是同时操作多个文件目录，或者远程控制多台主机，并行操作可以节约大量的时间。当被操作对象数目不大时，可以直接利用 multiprocessing 中的 Process 动态成生多个进程，十几个还好，但如果是上百个，上千个目标，手动的去限制进程数量却又太过繁琐，此时可以发挥进程池的功效。

Pool 可以提供指定数量的进程，供用户调用，**当有新的请求提交到 pool 中时，如果池还没有满，那么就会创建一个新的进程用来执行该请求；但如果池中的进程数已经达到规定最大值，那么该请求就会等待，直到池中有进程结束，才会创建新的进程来它**。

Pool 的用法有阻塞和非阻塞两种方式。

#### 2.2.1. 构造方法

```python
pool = Pool(processes=3)
# processes 参数指定进程池的最大进程数，默认大小是 CPU 的核数
```

#### 2.2.2. 方法

- `apply_async(func[, args[, kwds[, callback]]])` 向进程池中添加进程，它是非阻塞的，即能同时运行多个进程，同时运行的最大进程数在创建进程池时指定，如果池中的正在运行的进程数已经达到最大值，那么还没有执行的进程就会等待，直到池中有进程结束，才能进入进程池运行；

  e.g.
  ```python
  def function(index):
      print('Start process: ', index)
      time.sleep(3)
      print('End process', index)

  if __name__ == '__main__':
      pool = Pool(processes=3)
      for i in range(4):
          pool.apply_async(function, (i,))

      print("Started processes")
      pool.close() # 调用 close() 后，才开始执行线程池中的线程
      pool.join()
      print("Subprocess done.")
  ```
  输出结果：
  ```
  Started processes
  Start process:  0
  Start process:  1
  Start process:  2
  End process 0
  Start process:  3
  End process 1
  End process 2
  End process 3
  Subprocess done.
  ```

- `apply(func[, args[, kwds]])` 向进程池中添加进程，它是阻塞的，即需要串行的运行每个进程。

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

- `close()` 关闭 pool，使其不在接受新的任务。

  调用 join() 之前必须先调用 close()，调用 close() 之后就不能继续添加新的 Process 了

- `terminate()` 结束工作进程，不在处理未完成的任务。

- `join()` 主进程阻塞，等待子进程的退出， join 方法要在 close 或 terminate 之后使用。

  对 Pool 对象调用 join() 方法会等待所有子进程执行完毕

- `map(func, iterable)` 会使主进程阻塞直到最后一个子进程返回结果。在进程池中添加非阻塞进程，将 iterable 中的各个元素分别作为参数在每个进程中调用 fun 函数，也就是说，iterable 对象有几个元素，就会创建几个进程。

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

### 2.3. 进程间同步类

#### 2.3.1. Lock 类

使用多进程执行多任务并行时，可能会出现因多个进程同时占用资源而导致的不可预测错误。如：两个进程同时进行了输出，结果第一个进程的换行没有来得及输出，第二个进程就输出了结果。所以导致一些排版的问题。

为避免这样的问题，应使得在某一时间，只能一个进程输出，其他进程等待。等刚才那个进程输出完毕之后，另一个进程再进行输出，即使用 “互斥锁”。这种“互斥锁”可以通过进程锁 Lock 类来实现，在一个进程输出时，加锁，其他进程等待。等此进程执行结束后，释放锁，其他进程可以进行输出。

在访问临界资源时，使用 Lock 就可以避免进程同时占用资源而导致的一些资源争用问题。

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

#### 2.3.2. Semaphore 类

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

### 2.4. 进程间通信 (IPC) 类

Process 之间肯定是需要通信的，操作系统提供了很多机制来实现进程间的通信。Python 的 multiprocessing 模块包装了底层的机制，提供了 Queue、Pipes 等多种方式来交换数据。

#### 2.4.1. Queue 类

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

#### 2.4.2. Pipe 类

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

### 2.5. 其它模块方法

- `multiprocessing.active_children()`：以 Process 对象的 list 返回目前所有的运行的进程。

- `multiprocessing.cpu_count()`：返回当前机器的逻辑 CPU 核心数量，即物理核数 x cpu 数量 x 超线程数。

- `multiprocessing.current_process()`：返回当前的进程对象。

### 2.6. managers 子模块

TODO: 分布式进程：

http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431929340191970154d52b9d484b88a7b343708fcc60000

## 3. Refer Links
