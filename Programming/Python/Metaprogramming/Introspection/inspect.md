
# Python 反射 / 自省

## 1

https://www.ibm.com/developerworks/cn/linux/l-pyint/

https://kb.cnblogs.com/page/87128/

有时候我们会碰到这样的需求，需要执行对象的某个方法，或是需要对对象的某个字段赋值，而方法名或是字段名在编码代码时并不能确定，需要通过参数传递字符串的形式输入。举个具体的例子：当我们需要实现一个通用的 DBM 框架时，可能需要对数据对象的字段赋值，但我们无法预知用到这个框架的数据对象都有些什么字段，换言之，我们在写框架的时候需要通过某种机制访问未知的属性。

这个机制被称为反射（反过来让对象告诉我们他是什么），或是自省（让对象自己告诉我们他是什么，好吧我承认括号里是我瞎掰的 - -#），用于实现在运行时获取未知对象的信息。反射是个很吓唬人的名词，听起来高深莫测，在一般的编程语言里反射相对其他概念来说稍显复杂，一般来说都是作为高级主题来讲；但在 Python 中反射非常简单，用起来几乎感觉不到与其他的代码有区别，使用反射获取到的函数和方法可以像平常一样加上括号直接调用，获取到类后可以直接构造实例；不过获取到的字段不能直接赋值，因为拿到的其实是另一个指向同一个地方的引用，赋值只能改变当前的这个引用而已。

## 2

https://blog.csdn.net/freeking101/article/details/52458647

https://www.cnblogs.com/yaohong/p/8874154.html

利用 inspect 模块可以获取类中的 docs, 类名，类以及类的代码，是否存在的模块等。

## 3

https://www.cnblogs.com/vipchenwei/p/6991209.html

https://www.jianshu.com/p/628f61f01a54

1. getattr() 函数是 Python 自省的核心函数，具体使用大体如下：
class A:
def __init__(self):
self.name = 'zhangjing'
#self.age='24'
def method(self):
print"method print"

Instance = A()
print getattr(Instance , 'name, 'not find') #如果 Instance 对象中有属性 name 则打印 self.name 的值，否则打印'not find'
print getattr(Instance , 'age', 'not find') #如果 Instance 对象中有属性 age 则打印 self.age 的值，否则打印'not find'
print getattr(a, 'method', 'default') #如果有方法 method，否则打印其地址，否则打印 default
print getattr(a, 'method', 'default')() #如果有方法 method，运行函数并打印 None 否则打印 default

2. hasattr(object, name)

说明：判断对象 object 是否包含名为 name 的特性（hasattr 是通过调用 getattr(ojbect, name) 是否抛出异常来实现的）

3. setattr(object, name, value)

这是相对应的 getattr()。参数是一个对象，一个字符串和一个任意值。字符串可能会列出一个现有的属性或一个新的属性。这个函数将值赋给属性的。该对象允许它提供。例如，setattr(x,“foobar”,123) 相当于 x.foobar = 123。

4. delattr(object, name)

与 setattr() 相关的一组函数。参数是由一个对象（记住 python 中一切皆是对象) 和一个字符串组成的。string 参数必须是对象属性名之一。该函数删除该 obj 的一个由 string 指定的属性。delattr(x, 'foobar')=del x.foobar