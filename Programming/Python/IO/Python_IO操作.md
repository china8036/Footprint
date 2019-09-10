- [Python IO 操作](#python-io-操作)
  - [1. 文件操作](#1-文件操作)
    - [1.1. 读文件](#11-读文件)
    - [1.2. 写文件](#12-写文件)
    - [1.3. 批量文件读写](#13-批量文件读写)
  - [2. Refer Links](#2-refer-links)

# Python IO 操作

TODO: https://docs.python.org/3/tutorial/inputoutput.html

## 1. 文件操作

https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files

读写文件是最常见的 IO 操作。Python 内置了读写文件的函数，用法和 C 是兼容的。

读写文件前，我们先必须了解一下，在磁盘上读写文件的功能都是由操作系统提供的，现代操作系统不允许普通的程序直接操作磁盘，所以，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。

### 1.1. 读文件

首先要以读文件的模式打开一个文件对象，使用 Python 内置的 open() 函数，传入文件名和标示符：
```python
>>> f = open('/Users/michael/test.txt', 'r')
标示符'r'表示读，这样，我们就成功地打开了一个文件。
```
如果文件不存在，open() 函数就会抛出一个 IOError 的错误，并且给出错误码和详细的信息告诉你文件不存在。

如果文件打开成功，调用 read() 方法可以一次读取文件的全部内容，Python 把内容读到内存，用一个 str 对象表示：
```python
>>> f.read()
'Hello, world!'
```
也可以调用 readlines() 一次读取所有内容并按行返回 list：
```python
for line in f.readlines():
  print(line.strip()) # 把末尾的'\n'删掉
```

[强悍的 Python —— 读取大文件](https://blog.csdn.net/lanchunhui/article/details/51581540)

对于大文件的读取，由于调用 read() 或 readlines() 方法会一次性读取文件的全部内容，如果文件有 10G，内存就爆了，因此在这种情况下有以下选择：
- 反复调用 read(size) 方法，每次最多读取 size 个字节的内容。
- 反复调用 readline() 每次读取一行内容。
- 通过 iter & yield 将大文件分割成若干小文件处理，处理完每个小文件后释放该部分内存：
  ```python
  def read_in_chunks(filePath, chunk_size=1024*1024):
      """
      Lazy function (generator) to read a file piece by piece.
      Default chunk size: 1M
      You can set your own chunk size
      """
      file_object = open(filePath)
      while True:
          chunk_data = file_object.read(chunk_size)
          if not chunk_data:
              break
          yield chunk_data
  if __name__ == "__main__":
      filePath = './path/filename'
      for chunk in read_in_chunks(filePath):
          process(chunk) # <do something with chunk>
  ```
- 通过 with 语句实现流式读取（pythonic）：
  ```python
  #If the file is line based
  with open(...) as f:
      for line in f:
          process(line) # <do something with line>
  ```
  for line in f 文件对象 f 视为一个迭代器，会自动的采用缓冲 IO 和内存管理，所以不必担心大文件。

最后一步是调用 close() 方法关闭文件。文件使用完毕后必须关闭，因为文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的：
```python
>>> f.close()
```

由于文件读写时都有可能产生 IOError，一旦出错，后面的 f.close() 就不会调用。所以，为了保证无论是否出错都能正确地关闭文件，我们可以使用 try ... finally 来实现：
```python
try:
  f = open('/path/to/file', 'r')
  print(f.read())
finally:
  if f:
    f.close()
```

为进一步简化操作，Python 引入了 with 语句来自动帮我们调用 close() 方法：
```python
with open('/path/to/file', 'r') as f:
  print(f.read())
```
这和前面的 try ... finally 是一样的，但是代码更佳简洁，并且不必调用 f.close() 方法。

### 1.2. 写文件

写文件和读文件是一样的，唯一区别是调用 open() 函数时，传入标识符'w'或者'wb'表示写文本文件或写二进制文件：
```python
>>> f = open('/Users/michael/test.txt', 'w')
>>> f.write('Hello, world!')
>>> f.close()
```
要写入特定编码的文本文件，请给 open() 函数传入 encoding 参数，将字符串自动转换成指定编码。例如，读取 GBK 编码的文件：
```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
>>> f.read()
'测试'
```
遇到有些编码不规范的文件，你可能会遇到 UnicodeDecodeError，因为在文本文件中可能夹杂了一些非法编码的字符。遇到这种情况，open() 函数还接收一个 errors 参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：
```python
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```

你可以反复调用 write() 来写入文件，但是务必要调用 f.close() 来关闭文件。当我们写文件时，操作系统往往不会立刻把数据写入磁盘，而是放到内存缓存起来，空闲的时候再慢慢写入。只有调用 close() 方法时，操作系统才保证把没有写入的数据全部写入磁盘。忘记调用 close() 的后果是数据可能只写了一部分到磁盘，剩下的丢失了。因此，为简化操作，推荐使用 with open 语句来进行文件的读写：
```python
with open('/Users/michael/test.txt', 'w') as f:
  f.write('Hello, world!')
```

### 1.3. 批量文件读写

对于多个文件的读写，一般有以下两种方式：
- 嵌套式
```python
with open('/home/xbwang/Desktop/output_measures.txt','r') as f:
    with open('/home/xbwang/Desktop/output_measures2.txt','r') as f1:
        with open('/home/xbwang/Desktop/output_output_bk.txt','r') as f2:
　　　　　　　........
```

- 先后式
```python
with open('/home/xbwang/Desktop/output_measures.txt','r') as f:
........
with open('/home/xbwang/Desktop/output_measures2.txt','r') as f1:
........
with open('/home/xbwang/Desktop/output_output_bk.txt','r') as f2:
........
```

## 2. Refer Links
