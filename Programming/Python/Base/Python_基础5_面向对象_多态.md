- [Python 基础：面向对象 - 多态](#python-基础面向对象---多态)
  - [1. “开闭”原则](#1-开闭原则)
  - [2. 鸭子类型](#2-鸭子类型)
  - [3. Refer Links](#3-refer-links)

# Python 基础：面向对象 - 多态

例：
```python
class Animal(object):
    def run(self):
        print('Animal is running...')
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')

def run_twice(animal):
    animal.run()
```
当我们传入 Animal 的实例时：
```python
>>> run_twice(Animal())
Animal is running...
```
当我们传入 Dog 的实例时：
```python
>>> run_twice(Dog())
Dog is running...
```
当我们传入 Cat 的实例时：
```python
>>> run_twice(Cat())
Cat is running...
```

## 1. “开闭”原则

比如：
```python
class Animal(object):
    def run(self):
        print('Animal is running...')

class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
```

多态的好处就是，当我们需要传入 Dog、Cat、Tortoise……时，我们只需要接收 Animal 类型就可以了，因为 Dog、Cat、Tortoise……都是 Animal 类型，然后，按照 Animal 类型进行操作即可。由于 Animal 类型有 run() 方法，因此，传入的任意类型，只要是 Animal 类或者子类，就会自动调用实际类型的 run() 方法，这就是多态的意思：

对于一个变量，我们只需要知道它是 Animal 类型，无需确切地知道它的子类型，就可以放心地调用 run() 方法，而具体调用的 run() 方法是作用在 Animal、Dog、Cat 还是 Tortoise 对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种 Animal 的子类时，只要确保 run() 方法编写正确，不用管原来的代码是如何调用的。

这就是著名的“开闭”原则：

1)	对扩展开放：允许新增 Animal 子类；

2)	对修改封闭：不需要修改依赖 Animal 类型的 run_twice() 等函数；

## 2. 鸭子类型

对于静态语言（例如 Java）来说，如果需要传入 Animal 类型，则传入的对象必须是 Animal 类型或者它的子类，否则，将无法调用 run() 方法。

对于 Python 这样的动态语言来说，则不一定需要传入 Animal 类型。我们只需要保证传入的对象有一个 run() 方法就可以了：
```python
class Timer(object):
    def run(self):
        print('Start...')
```
这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。

<!-- TODO: 理解：看有没有操作的方法，而不看类的类型是什么 -- 弱化了类型的概念和限制 -- 动态语言的特点 -- “看实力，不看学历” -->

Python 的 “file-like object“ 就是一种鸭子类型。对真正的文件对象，它有一个 read() 方法，返回其内容。但是，许多对象，只要有 read() 方法，都被视为 “file-like object“。许多函数接收的参数就是“file-like object“，你不一定要传入真正的文件对象，完全可以传入任何实现了 read() 方法的对象。

例如：

要实现一个读取图像的功能：
```python
def readImage(fp):
    if hasattr(fp, 'read'):
        return readData(fp)
return None
```
根据鸭子类型，有 read() 方法，不代表该 fp 对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要 read() 方法返回的是有效的图像数据，就不影响读取图像的功能；

## 3. Refer Links
