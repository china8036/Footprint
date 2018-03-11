- [Java 集合：概述](#java-%E9%9B%86%E5%90%88%EF%BC%9A%E6%A6%82%E8%BF%B0)
  - [1. 设计思想](#1-%E8%AE%BE%E8%AE%A1%E6%80%9D%E6%83%B3)
  - [2. 类谱图](#2-%E7%B1%BB%E8%B0%B1%E5%9B%BE)
  - [3. 与 JCF 相关的其它接口](#3-%E4%B8%8E-jcf-%E7%9B%B8%E5%85%B3%E7%9A%84%E5%85%B6%E5%AE%83%E6%8E%A5%E5%8F%A3)
    - [3.1. Iterable 接口](#31-iterable-%E6%8E%A5%E5%8F%A3)
      - [3.1.1. forEach() 方法](#311-foreach-%E6%96%B9%E6%B3%95)
      - [3.1.2. Iterator 接口](#312-iterator-%E6%8E%A5%E5%8F%A3)
    - [3.2. Predicate 接口](#32-predicate-%E6%8E%A5%E5%8F%A3)
    - [3.3. Stream 接口](#33-stream-%E6%8E%A5%E5%8F%A3)
  - [4. Refer Links](#4-refer-links)

# Java 集合：概述

Java 在 `java.util.*` 中为开发者提供了包含了一系列常用数据结构及其操作的工具包，即 Java 集合框架 (Java Collections Framework, JCF)。

## 1. 设计思想

- 接口与实现分离
  
  与现在数据结构类库常见的情况一样，Java 集合类库将接口与实现分离。同一个接口可以有多种实现方式，便于开发者选择最适合自己的实现类进行使用。一旦想要改变，由于实现自同一个接口，所包含的方法及方法调用结果完全相同，因此只需修改类对象的构造处，即可轻松选用另外一种实现方式。

  在集合类谱图中，还有一组以 `Abstract` 开头命名的类，这些类是为类库实现者设计的。其中默认实现了接口的大部分方法，开发者只需继承这些抽象类，便可只对个别方法进行重新实现，而不用实现接口的所有方法。
  

- 适配器设计模式

  Java 集合框架中大量使用了适配器模式进行实现。

## 2. 类谱图

![image](/resources/images/java-collection.png)

Java 集合框架中有两个基本接口：Collection 和 Map，除此之外还有一些与 Java 集合框架相关的可用于辅助操作的其它接口。

## 3. 与 JCF 相关的其它接口

### 3.1. Iterable 接口

[Iterable](https://docs.oracle.com/javase/9/docs/api/java/lang/Iterable.html) 接口用于表示可被迭代的对象，该接口只包含了 1 个成员变量和 2 个成员方法：
- `Iterator<T> iterator()`
- `default void forEach(Consumer<? super T> action) {}`
- `default Spliterator<T> spliterator() {}`

#### 3.1.1. forEach() 方法

`forEach(Consumer<? super T> action)`方法是 Java 8 为 Iterable 接口新增的默认方法，由于 Collection 接口扩展了 Iterable 接口，因此任何 Collection 集合都可以直接调用该方法。

该方法所需参数的类型是一个函数式接口 Consumer，可以使用 Lambda 表达式。当调用该方法时，程序会依次将集合元素传给 Consumer 的 accept() 方法。

#### 3.1.2. Iterator 接口

[Iterator](https://docs.oracle.com/javase/9/docs/api/java/util/Iterator.html) 接口也是 Java 集合框架的成员，Iterator 对象也称为迭代器，用于对象的迭代。

Iterator 接口实际上是 Java 早期 Enumeration 接口的“升级版”。

- 基本 API

  - `boolean	hasNext​()`

    判断集合的“光标”当前所在位置后是否还有元素。

  - `E	next​()`

    Java 迭代器可以认为位于两个元素之间，每次调用 next 方法时，会将集合的“光标”向前移动一个位置，并返回移动时越过的元素的引用。若到达集合的末尾，将抛出一个异常。因此，一般在调用 next 方法之前需要使用 hasNext 进行判断。

  - `default void	remove​()`

    删除上次调用 next 方法时返回的元素。

    next 方法与 remove 方法的调用有相互依赖性。若调用 remove 之前没有调用 next 将会抛出一个异常。

  - `default void	forEachRemaining​(Consumer<? super E> action)`

    该方法从 Java SE 8 开始加入，可通过为该方法提供一个 Lambda 表达式，对迭代器的每一个元素调用该 Lambda 表达式进行处理。

### 3.2. Predicate 接口

TODO:

### 3.3. Stream 接口

TODO:

## 4. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)