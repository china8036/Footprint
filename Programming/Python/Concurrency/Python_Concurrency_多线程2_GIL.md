- [Python 并发：多线程 - Global Interpreter Lock](#python-并发多线程---global-interpreter-lock)
  - [5. Refer Links](#5-refer-links)

# Python 并发：多线程 - Global Interpreter Lock

https://www.zhihu.com/question/56170408

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

## 5. Refer Links

TODO:

https://www.zhihu.com/question/27245271