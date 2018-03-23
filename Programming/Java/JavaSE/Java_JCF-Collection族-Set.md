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
    - [4. SortedSet 接口 & NavigableSet 接口 & TreeSet 类](#4-sortedset-%E6%8E%A5%E5%8F%A3-navigableset-%E6%8E%A5%E5%8F%A3-treeset-%E7%B1%BB)
        - [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [4.2. 常用 API](#42-%E5%B8%B8%E7%94%A8-api)
        - [4.3. 源码分析](#43-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
    - [5. 为什么 JCF 中不提供 WeakHashSet 类？](#5-%E4%B8%BA%E4%BB%80%E4%B9%88-jcf-%E4%B8%AD%E4%B8%8D%E6%8F%90%E4%BE%9B-weakhashset-%E7%B1%BB%EF%BC%9F)
    - [6. EnumSet 类](#6-enumset-%E7%B1%BB)
        - [6.1. 基本概念](#61-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [6.2. 常用 API](#62-%E5%B8%B8%E7%94%A8-api)
    - [7. Refer Links](#7-refer-links)
  
# Java 集合：Collection 族 - Set

![image](/resources/images/set-api-class-diagram.png)

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

## 4. SortedSet 接口 & NavigableSet 接口 & TreeSet 类

### 4.1. 基本概念

树集 [TreeSet](https://docs.oracle.com/javase/9/docs/api/java/util/TreeSet.html) 与散列表相似，但有所改进。它是一个有序的集合，可以以任意顺序将元素插入到集合中，但在对集合进行遍历时，每个值将自动按照排序后的顺序呈现。

树集中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

**若不需要对集合的元素进行排序，那么应该选择散列集作为存储结构，可避免用于排序的开销。**

### 4.2. 常用 API

### 4.3. 源码分析

类似于 HashMap 和 HashSet 之间的关系，HashSet 底层依赖于 HashMap 实现，TreeSet 底层则采用一个 NavigableMap 来保存 TreeSet 集合的元素。但实际上，由于 NavigableMap 只是一个接口，因此底层依然是使用 TreeMap 来包含 Set 集合中的所有元素。

```java
// 使用 NavigableMap 的 key 来保存 Set 集合的元素
private transient NavigableMap<E,Object> m;

// 使用一个 PRESENT 作为 Map 集合的所有 value
private static final Object PRESENT = new Object();

// 包访问权限的构造器，以指定的 NavigableMap 对象创建 Set 集合
TreeSet(NavigableMap<E,Object> m) {
    this.m = m;
}

// 以自然顺序方式创建一个新的 TreeMap, 根据该 TreeSet 创建一个 TreeSet
public TreeSet() {
    this(new TreeMap<E,Object>());
}

// 以定制顺序方式创建一个新的 TreeMap, 根据该 TreeSet 创建一个 TreeSet
public TreeSet(Comparator<? super E> comparator) {
    this(new TreeMap<>(comparator));
}

public TreeSet(Collection<? extends E> c) {
    this();
    addAll(c);
}

public TreeSet(SortedSet<E> s) {
    this(s.comparator());
    addAll(s);
}
```

与 HashSet 完全类似的是，TreeSet 里绝大部分方法都是直接调用 TreeMap 的方法来实现的。

## 5. 为什么 JCF 中不提供 WeakHashSet 类？

https://stackoverflow.com/questions/4062919/why-does-exist-weakhashmap-but-absent-weakset

在 JCF 中，基本上大部分的 Map 接口的实现类都一个与之对应的 Set 实现类，其基于 Map 接口实现。但为什么 WeakHashMap 没有与之对应的 WeakHashSet 呢？

事实上，可以通过以下方法创建一个 WeakHashSet 实例：
```java
Set<Object> weakHashSet = Collections.newSetFromMap(
		new WeakHashMap<Object, Boolean>());
```
> Why there is no specific class for such stuff? 
> It's easy to imagine why the maintainers of java.util might have wanted to stop having to provide dual Map and Set versions of everything they do, and opted to just provide newSetFromMap() instead.

## 6. EnumSet 类

### 6.1. 基本概念

[EnumSet](https://docs.oracle.com/javase/9/docs/api/java/util/EnumSet.html) 是一个专门为枚举类设计的集合类，EnumSet 中的所有元素都必须是指定枚举类型的枚举值，该枚举类型在创建 EnumSet 时显式或隐式的指定。

EnumSet 的集合元素也是有序的，EnumSet 以枚举值在 Enum 类中的定义顺序来决定元素的顺序。

EnumSet 在内部以位向量的形式存储，这种存储形式非常高效紧凑，因此 EnumSet 对象占用内存很小，而且运行效率很好。尤其进行批量操作（如 containsAll()、retainAll() 方法）时，如果其参数也是 EnumSet 集合，则该批量操作的执行速度会非常快。

EnumSet 不允许加入 null 元素，如果试图加入 null 元素，将会导致 NullPointerException 异常。

### 6.2. 常用 API

EnumSet 类没有提供任何 public 的构造器来创建该类的实例，程序应该通过它提供的类方法来创建 EnumSet 对象：
- `static <E extends Enum<E>> EnumSet<E>	allOf​(Class<E> elementType)`: Creates an enum set containing all of the elements in the specified element type.
- `static <E extends Enum<E>> EnumSet<E>	complementOf​(EnumSet<E> s)`: Creates an enum set with the same element type as the specified enum set, initially containing all the elements of this type that are not contained in the specified set.
- `static <E extends Enum<E>> EnumSet<E>	copyOf​(Collection<E> c)`: Creates an enum set initialized from the specified collection.
- `static <E extends Enum<E>> EnumSet<E>	noneOf​(Class<E> elementType)`: Creates an empty enum set with the specified element type.
- `static <E extends Enum<E>> EnumSet<E>	of​(E first, E... rest)`: Creates an enum set initially containing the specified elements.
- `static <E extends Enum<E>> EnumSet<E>	range​(E from, E to)`: Creates an enum set initially containing all of the elements in the range defined by the two specified endpoints.

NOTE: 当试图复制一个 Collection 中的元素来创建 EnumSet 实例时，必须保证 Collection 中的所有元素都是同一个枚举类的枚举值。

## 7. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)

[容器源码分析之 HashSet （三)](http://blog.csdn.net/qq_33394088/article/details/79032972)