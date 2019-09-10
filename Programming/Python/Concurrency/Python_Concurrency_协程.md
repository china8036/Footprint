- [Python 多任务：协程](#python-多任务协程)
  - [1. asyncio 模块](#1-asyncio-模块)
    - [1.1. 版本差异](#11-版本差异)
    - [1.2. 使用](#12-使用)
    - [1.3. 其它方法](#13-其它方法)
  - [2. aiohttp 模块](#2-aiohttp-模块)
  - [3. Refer Links](#3-refer-links)

# Python 多任务：协程

Python 对协程的支持经历了很长的一段发展历程，其大概经历了如下阶段：
- python2.x：最初的生成器变形 yield/send，实现了协程的一部分但不完全
- python3.4：引入标准库 asynico，提供 @asyncio.coroutine 和 yield from，还是以生成器对象为基础
- python3.5：添加了 types.coroutine 装饰器以及 async/await 关键字，确定了协程的语法
- python3.6：将 asyncio 由临时版改为了稳定版
- 除 asyncio 外，tornado 和 gevent 都实现了异步编程的功能。

## 1. asyncio 模块

https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00144661533005329786387b5684be385062a121e834ac7000#0

https://github.com/ictar/python-doc/blob/master/Python%20Common/Python%20async-await%E6%95%99%E7%A8%8B.md

asyncio 的编程模型就是一个消息循环。我们从 asyncio 模块中直接获取一个 EventLoop 的引用，然后把需要执行的协程扔到 EventLoop 中执行，就实现了异步 IO。

### 1.1. 版本差异

Python3.4 和 Python3.5 中如何定义一个协程函数：
- python34
  ```python
  @asyncio.coroutine
  def py34_function():
      yield from work()
  ```

- python35
  ```python
  async def py35_function():
      await work()
  ```

  Python3.5 中定义协程更为简单了，但是实际上生成器和协程之间的差别变的更加明显了。

  NOTE
  - await 只能用于 async def 的函数中。
  - yield from 不能用于 async def 的函数中。
  - yield from 和 await 可以接受的对象是略有区别的，await 接受的对象必须是一个 awaitable 对象。什么是 awaitable 对象呢，就是一个实现了__await()__方法的对象，而且这个方法必须返回一个不是协程的迭代器。满足这两个条件，才算是一个 awaitable 对象，当然协程本身也是 awaitable 对象，因为 collections.abc.Coroutine 继承了 collections.abc.Awaitable。换句话说，await 后面可接受的对象有两种，分别是：协程和 awaitable 对象，当然协程也是 awaitable 对象。

- python36

  在 Python3.6 中，这种特性继续被发扬光大，现在可以在同一个函数体内使用 yield 和 await，而且除此之外，也可以在列表推导等地方使用 async for 或 await 语法。
  ```python
  result = [i async for i in aiter() if i % 2]
  result = [await func() for fun in funcs if await condition()]

  async def test(x, y):
      for i in range(y):
          yield i
          await asyncio.sleep(x)
  ```

### 1.2. 使用

e.g.
```python
import asyncio

# async def 关键字定义一个特殊函数 -- 协程对象，调用时实际上并不运行而是返回一个 coroutine 对象，将这个对象传给事件循环后才会运行；
async def wget(host):
    print('wget %s...' % host)
    conn = asyncio.open_connection(host, 80)
    reader, writer = await conn
    header = 'GET / HTTP/1.0\r\nHost:%s\r\n\r\n' % host
    writer.write(header.encode('utf-8'))
    await writer.drain()
    while True:
        line = await reader.readline()
        if line == b'\r\n':
            break
        print('%s header > %s' % (host, line.decode('utf-8').rstrip()))
    writer.close()

# 首先获取默认的时间循环，调度和运行异步任务，然后当循环结束运行的时候关闭循环
loop = asyncio.get_event_loop()
# 多个 coroutine 可以封装成一组 Task 然后并发执行
tasks = [wget(host) for host in ['www.sina.com.cn', 'www.sohu.com', 'www.163.com']]
# 该函数实际上是阻塞的，所以直到所有的一步方法完成后它才返回。由于我们只在一个线程中运行它，当循环正在运行的时候，是没有办法继续执行下去的
loop.run_until_complete(asyncio.wait(tasks))
# 关闭事件循环
loop.close()
```

await 后边可以接什么？

用于 await 语句的对象必须是 awaitable 的，awaitable 对象必须满足如下条件中其中之一（可以用`isinstance(gencoro, Awaitable)`进行判断）：
- A native coroutine object returned from a native coroutine function：原生协程对象。

- A generator-based coroutine object returned from a function decorated with types.coroutine()：types.coroutine() 修饰的基于生成器的协程对象，注意不是 Python3.4 中 asyncio.coroutine。

- An object with an await method returning an iterator：实现了 await method，并在其中返回了 iterator 的对象。

- 此外，基于生成器的 coroutine 对象（用 types.coroutine 和 asyncio.coroutine 修饰器修饰），虽然没有实现`__await__`方法，但是也被认为是 awaitable 的，可以被用于 await 语句。但是这种对象如果用上面的 isinstance 判断会返回 False。可以使用`inspect.isawaitable()`判断。

若 await 后边接了非法对象，会报错：`TypeError("object xxxx can't be used in 'await' expression")`

<!-- http://www.jianshu.com/p/b036e6e97c18 -->

### 1.3. 其它方法

asyncio.iscoroutine(obj)：确定一个对象是否为协程。

## 2. aiohttp 模块

https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014320981492785ba33cc96c524223b2ea4e444077708d000

asyncio 实现了 TCP、UDP、SSL 等协议，aiohttp 则是基于 asyncio 实现的 HTTP 框架。

https://python.freelycode.com/contribution/detail/201

https://github.com/ictar/python-doc/blob/master/Python%20Common/Python%20async-await%E6%95%99%E7%A8%8B.md

```python
import signal
import sys
import asyncio
import aiohttp
import json

loop = asyncio.get_event_loop()
client = aiohttp.ClientSession(loop=loop)

async def get_json(client, url):
    async with client.get(url) as response:
        assert response.status == 200
        return await response.read()

async def get_reddit_top(subreddit, client):
    data1 = await get_json(client, 'https://www.reddit.com/r/' + subreddit + '/top.json?sort=top&t=day&limit=5')

    j = json.loads(data1.decode('utf-8'))
    for i in j['data']['children']:
        score = i['data']['score']
        title = i['data']['title']
        link = i['data']['url']
        print(str(score) + ': ' + title + ' (' + link + ')')

    print('DONE:', subreddit + '\n')

def signal_handler(signal, frame):
    loop.stop()
    client.close()
    sys.exit(0)

signal.signal(signal.SIGINT, signal_handler)

asyncio.ensure_future(get_reddit_top('python', client))
asyncio.ensure_future(get_reddit_top('programming', client))
asyncio.ensure_future(get_reddit_top('compsci', client))
loop.run_forever()
```

## 3. Refer Links
