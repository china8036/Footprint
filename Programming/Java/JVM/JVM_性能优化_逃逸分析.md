- [JVM 逃逸分析](#jvm-逃逸分析)
  - [1. 栈上分配](#1-栈上分配)
  - [2. 应用](#2-应用)
  - [3. HotSpot 实现](#3-hotspot-实现)
  - [4. Refer Links](#4-refer-links)

# JVM 逃逸分析

逃逸分析是目前 Java 虚拟机中比较前沿的优化技术，它并不是直接优化代码的手段，而是为其他优化手段提供依据的分析技术。

当一个对象在方法中被定义后，如果该对象还被外部方法所引用，例如作为调用参数传递到其他方法中，这种行为称为**方法逃逸**。

当一个对象在方法中被定义后，如果该对象还被外部线程访问到，例如赋值被类变量或可以在其他线程中访问的实例变量，这种行为称为**线程逃逸**。

## 1. 栈上分配

- C++
  - new：堆上分配，需要手动释放对象实例的内存。
  - 直接调用构造函数：栈上分配，方法结束时自动调用析构函数。
- Java

## 2. 应用

- 分析对象作用域。

- 堆分配对象变成栈分配对象。一个方法当中的对象，对象的引用没有发生逃逸，那么这个方法可能会被分配在栈内存上而非常见的堆内存上。
  ```Java
  class Main {
    public static void main(String[] args) {
      example();
    }
    public static void example() {
      Foo foo = new Foo(); //alloc
      Bar bar = new Bar(); //alloc
      bar.setFoo(foo);
    }
  }
  
  class Foo {}
  
  class Bar {
    private Foo foo;
    public void setFoo(Foo foo) {
      this.foo = foo;
    }
  }
  ```
  在这个例子当中，我们创建了两个对象，Foo 对象和 Bar 对象，同时我们把 Foo 对象的应用赋值给了 Bar 对象的方法。此时，如果 Bar 对在堆上就会引起 Foo 对象的逃逸，但是，在本例当中，编译器通过逃逸分析，可以知道 Bar 对象没有逃出 example() 方法，因此这也意味着 Foo 也没有逃出 example 方法。因此，编译器可以将这两个对象分配到栈上。

- 标量替换：逃逸分析方法如果发现对象的内存存储结构不需要连续进行的话，就可以将对象的部分甚至全部都保存在 CPU 寄存器内，这样能大大提高访问速度。
   
- 同步消除：线程同步的代价是相当高的，同步的后果是降低并发性和性能。逃逸分析可以判断出某个对象是否始终只被一个线程访问，如果只被一个线程访问，那么对该对象的同步操作就可以转化成没有同步保护的操作，这样就能大大提高并发程度和性能。

## 3. HotSpot 实现

说了逃逸分析的这些作用，那么 Java 虚拟机是否有对对象做逃逸分析呢？答案是否。

关于逃逸分析的论文在 1999 年就已经发表，但直到 Sun JDK 1.6 才实现了逃逸分析，而且直到现在这项优化尚未足够成熟，仍有很大的改进余地。不成熟的原因主要是不能保证逃逸分析的性能收益必定高于它的消耗。因为逃逸分析本身就是一个高耗时的过程，假如分析的结果是没有几个不逃逸的对象，那么这个分析所花费时候比优化所减少的时间更长，这是得不偿失的。

所以目前虚拟机只能采用不那么准确，但时间压力相对较小的算法来完成逃逸分析。还有一点是，基于逃逸分析的一些优化手段，如上面提到的“栈上分配”，由于 HotSpot 虚拟机目前的实现方式导致栈上分配实现起来比较复杂，因此在 HotSpot 中暂时还没有做这项优化。

TODO:

[Java 中的逃逸分析和 TLAB 以及 Java 对象分配](https://blog.csdn.net/yangzl2008/article/details/43202969)

Java 在 Java SE 6u23 以及以后的版本中支持并默认开启了逃逸分析的选项。Java 的 HotSpot JIT 编译器，能够在方法重载或者动态加载代码的时候对代码进行逃逸分析，同时 Java 对象在堆上分配和内置线程的特点使得逃逸分析成 Java 的重要功能。

## 4. Refer Links

TODO:

[Java 是否可以栈上分配对象内存？为什么？](https://juejin.im/post/5ae727dd6fb9a07aa54216fc)

[JVM 中 TLAB 区 (Thread Local Allocation Buffer)](https://blog.csdn.net/nangeali/article/details/81866132?utm_source=blogkpcl7)

[深入分析 JVM 逃逸分析对性能的影响](https://www.jianshu.com/p/04fcd0ea5af7)

[JVM 的栈上分配与逃逸分析 (Escape Analysis）](https://blog.csdn.net/blueheart20/article/details/52050545)