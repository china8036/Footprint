- [Java 集合：Collection 族 - Set](#java-%E9%9B%86%E5%90%88%EF%BC%9Acollection-%E6%97%8F---set)
  - [1. Set 接口](#1-set-%E6%8E%A5%E5%8F%A3)
    - [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [1.2. 基本 API](#12-%E5%9F%BA%E6%9C%AC-api)
  - [2. HashSet 类](#2-hashset-%E7%B1%BB)
    - [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [2.2. 常用 API](#22-%E5%B8%B8%E7%94%A8-api)
      - [2.2.1. 构造器](#221-%E6%9E%84%E9%80%A0%E5%99%A8)
      - [2.2.2. 添加 & 删除元素](#222-%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
      - [2.2.3. 访问元素](#223-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
      - [2.2.4. 其它](#224-%E5%85%B6%E5%AE%83)
    - [2.3. 源码分析](#23-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
      - [2.3.1. 关键属性](#231-%E5%85%B3%E9%94%AE%E5%B1%9E%E6%80%A7)
      - [2.3.2. 初始化](#232-%E5%88%9D%E5%A7%8B%E5%8C%96)
      - [2.3.3. 添加元素](#233-%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
      - [2.3.4. 删除元素](#234-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
      - [2.3.5. 访问元素](#235-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
  - [3. LinkedHashSet 类](#3-linkedhashset-%E7%B1%BB)
    - [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [3.2. 常用 API](#32-%E5%B8%B8%E7%94%A8-api)
    - [3.3. 源码分析](#33-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
      - [3.3.1. 类定义](#331-%E7%B1%BB%E5%AE%9A%E4%B9%89)
      - [3.3.2. 初始化](#332-%E5%88%9D%E5%A7%8B%E5%8C%96)
  - [4. SortedSet 接口 & TreeSet 类](#4-sortedset-%E6%8E%A5%E5%8F%A3-treeset-%E7%B1%BB)
  - [5. EnumSet 类](#5-enumset-%E7%B1%BB)
  - [6. Refer Links](#6-refer-links)
  
# Java 集合：Collection 族 - Set

## 1. Set 接口

### 1.1. 基本概念

[Set](https://docs.oracle.com/javase/9/docs/api/java/util/Set.html) 接口是 Collection 接口的扩展，但与 Collection 接口基本相同，没有添加任何额外的方法，只是行为实现略有不同。

- 特点
  - 不能保证元素的排列顺序。顺序可能与添加的顺序不同，也可能发生变化。
  - 元素值不可重复。在一个 Set 集合中不允许存在值相同的元素。

- 优点
  - 快速查找元素，利于搜索

- 缺点
  - 无法控制元素出现的次序

### 1.2. 基本 API

Set 接口提供的方法的行为相比 Collection 接口有更严谨的定义：
- `boolean	add​(E e)`

  添加元素时不允许添加重复的元素。若添加失败，返回 false。

- `boolean	equals​(Object o)`
  
  只要两个集合包含的元素相同就返回 true，不要求有相同的顺序。

- `int	hashCode​()`

  要保证包含相同元素的两个集合会得到相同的散列码。

## 2. HashSet 类

### 2.1. 基本概念

[HashSet](https://docs.oracle.com/javase/9/docs/api/java/util/HashSet.html) 类是 Set 接口的典型实现，大多数时候使用 Set 集合时都是使用 HashSet 实现类。HashSet 实现了基于散列表的集合，因此具有很好的存取和查找性能。

特点：
- HashSet 是非线程安全的。若有多个线程同时修改 HashSet，必须通过代码保证其同步。
- HashSet 中的元素值可以为 null。

### 2.2. 常用 API

#### 2.2.1. 构造器

- `HashSet​()`

  构造一个空的散列表。

- `HashSet​(Collection<? extends E> c)`

  构造一个散列集，并将所给集合中的所有集合添加到该散列集中。

- `HashSet​(int initialCapacity)`

  构造一个空的具有指定容量 / 桶数的散列表。

- `HashSet​(int initialCapacity, float loadFactor)`

  构造一个空的具有指定容量 / 桶数和装填因子的散列表。

#### 2.2.2. 添加 & 删除元素

- `boolean	add​(E e)`

- `boolean	remove​(Object o)`

#### 2.2.3. 访问元素

```java
Set<String> words = new HashSet<>();
//...
Iterator iter = words.iterator();
for (int i =1; i <= 20 && iter.hasNext(); i ++) {
  System.out.println(iter.next());
}
```

#### 2.2.4. 其它

- `boolean	contains​(Object o)`：用于快速查看某个元素是否已经出现在散列集中。该方法不会查看集合中的所有元素，而只会在某个桶中查找元素。

### 2.3. 源码分析

在实现上，HashSet 实际上是对 HashMap 的简单包装，对 HashSet 的函数调用都会转换成相应的 HashMap 方法，因此 HashSet 的实现非常简单，代码只有三百多行。

#### 2.3.1. 关键属性

```java
// 内部维护的 HashMap
private transient HashMap<E,Object> map;  

// 作为每次插入 HashMap 时的 value
private static final Object PRESENT = new Object();
```

#### 2.3.2. 初始化

HashSet 的初始化实际上都是对内部 HashMap 的初始化。
```java
public HashSet() {
    map = new HashMap<>();
}

public HashSet(Collection<? extends E> c) {
    map = new HashMap<>(Math.max((int) (c.size()/.75f) + 1, 16));
    addAll(c);
}

public HashSet(int initialCapacity, float loadFactor) {
    map = new HashMap<>(initialCapacity, loadFactor);
}

public HashSet(int initialCapacity) {
    map = new HashMap<>(initialCapacity);
}

HashSet(int initialCapacity, float loadFactor, boolean dummy) {
    map = new LinkedHashMap<>(initialCapacity, loadFactor);
}
```

#### 2.3.3. 添加元素

HashSet 不允许添加重复的元素，这一特点的实现，是利用了 HashMap 中 Key 的唯一性，来保证 HashSet 中不出现重复值。

当使用 add 方法将新的对象添加到 HashSet 当中时，实际上是将该新对象作为底层所维护的 HashMap 对象的 Key，而 value 则都是同一个内部 Object 对象。

```java
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
```

#### 2.3.4. 删除元素

```java
public boolean remove(Object o) {
    return map.remove(o)==PRESENT;
}
```

#### 2.3.5. 访问元素

```java
public Iterator<E> iterator() {
    return map.keySet().iterator();
}
```

## 3. LinkedHashSet 类

### 3.1. 基本概念

[LinkedHashMap](https://docs.oracle.com/javase/9/docs/api/java/util/LinkedHashSet.html) 类是 HashSet 的扩展类，它同样是根据元素的 hash 值来决定元素的存储位置，但不同之处在于 LinkedHashSet 同时使用链表维护元素的插入次序，使得遍历集合元素时，LinkedHashMap 将会按照元素的添加顺序来访问集合中的元素。

由于 LinkedHashMap 需要维护元素的插入顺序，因此性能略低于 HashSet，但由于它以双向链表维护着内部顺序，所以在迭代访问 Set 中的全部元素时将会比 HashSet 有更快的迭代速度。

### 3.2. 常用 API

LinkedHashSet 的 API 与 HashSet 的 API 完全相同。

### 3.3. 源码分析

LinkedHashSet 通过继承 HashSet，虽然在 LinkedHashSet 源代码中没有出现任何 LinkedHashMap 相关代码，但实际上 LinkedHashSet 借助 HashSet 的构造器构造了一个 LinkedHashMap，以简单明了的方式来实现了其自身的所有功能。其实现代码只提供了四个构造方法，不到二百行。

#### 3.3.1. 类定义

```java
public class LinkedHashSet<E>
    extends HashSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
```

#### 3.3.2. 初始化

LinkedHashSet 的构造方法都是直接调用父类 HashSet 的三参构造方法进行对象初始化：
```java
public LinkedHashSet(int initialCapacity, float loadFactor) {
    super(initialCapacity, loadFactor, true);
}

public LinkedHashSet(int initialCapacity) {
    super(initialCapacity, .75f, true);
}

public LinkedHashSet() {
    super(16, .75f, true);
}

public LinkedHashSet(Collection<? extends E> c) {
    super(Math.max(2*c.size(), 11), .75f, true);
    addAll(c);
}
```

HashMap 中：
```java
// 该构造方法为包访问权限，并未对外公开，可见是专用于 LinkedHashSet 的构造方法
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
    map = new LinkedHashMap<>(initialCapacity, loadFactor);
}
```

## 4. SortedSet 接口 & TreeSet 类

树集 [TreeSet](https://docs.oracle.com/javase/9/docs/api/java/util/TreeSet.html) 与散列表相似，但有所改进。它是一个有序的集合，可以以任意顺序将元素插入到集合中，但在对集合进行遍历时，每个值将自动按照排序后的顺序呈现。

TreeSet 采用红黑树实现，每次将一个元素添加到树中，都将被放置在正确排序的位置上。因此，迭代器总是以排好序的顺序访问各个元素。若树中包含 n 个元素，则查找新元素位置的操作需要 log2(n) 次比较。因此，将一个元素添加到树中要比添加到散列集中慢，但相比检查数组或链表中的重复元素还是快很多。

树集中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

**若不需要对集合的元素进行排序，那么应该选择散列集作为存储结构，可避免用于排序的开销。**

## 5. EnumSet 类

## 6. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)

[容器源码分析之 HashSet （三)](http://blog.csdn.net/qq_33394088/article/details/79032972)