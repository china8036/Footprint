- [Python 并发：多线程 - 线程池](#python-并发多线程---线程池)
    - [4.2. 线程池模块：multiprocessing.pool.ThreadPool](#42-线程池模块multiprocessingpoolthreadpool)
  - [5. Refer Links](#5-refer-links)

# Python 并发：多线程 - 线程池


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

TODO:

https://www.google.com/search?q=concurrent.futures

http://lovesoo.org/analysis-of-asynchronous-concurrent-python-module-concurrent-futures.html?lwdkry=laqpw&medqri=iqeqw

https://juejin.im/post/5b1e36476fb9a01e4a6e02e4#heading-2

concurrent.futures.ThreadPoolExecutor 和 ProcessPoolExecutor