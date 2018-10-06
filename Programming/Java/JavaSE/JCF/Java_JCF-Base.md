- [Java 集合：概述](#java-集合概述)
  - [1. 设计思想](#1-设计思想)
  - [2. 类谱图](#2-类谱图)
  - [3. 其它](#3-其它)
    - [3.1. 元素都是对象](#31-元素都是对象)
    - [3.2. API 对比](#32-api-对比)
    - [3.3. 快速失败机制](#33-快速失败机制)
    - [3.4. 存储 null 值对比](#34-存储-null-值对比)
    - [3.5. 遍历方法](#35-遍历方法)
      - [3.5.1. 遍历 Collection](#351-遍历-collection)
      - [3.5.2. 遍历 Map](#352-遍历-map)
  - [4. Refer Links](#4-refer-links)

# Java 集合：概述

Java 在 `java.util.*` 中为开发者提供了包含了一系列常用数据结构及其操作的工具包，即 Java 集合框架 (Java Collections Framework, JCF)。

为处理在多线程环境下的并发安全问题，从 Java 5 开始，在 java.util.concurrent 包下增加提供了一系列多线程支持的集合类。

## 1. 设计思想

- 接口与实现分离
  
  与现在数据结构类库常见的情况一样，Java 集合类库将接口与实现分离。同一个接口可以有多种实现方式，便于开发者选择最适合自己的实现类进行使用。一旦想要改变，由于实现自同一个接口，所包含的方法及方法调用结果完全相同，因此只需修改类对象的构造处，即可轻松选用另外一种实现方式。

  在集合类谱图中，还有一组以 `Abstract` 开头命名的类，这些类是为类库实现者设计的。其中默认实现了接口的大部分方法，开发者只需继承这些抽象类，便可只对个别方法进行重新实现，而不用实现接口的所有方法。

- 适配器设计模式

  Java 集合框架中大量使用了适配器模式进行实现。

## 2. 类谱图

![image](/resources/images/java-collections-framework.png)

Java 集合框架中有两个基本接口：Collection 和 Map。

## 3. 其它

### 3.1. 元素都是对象

集合类与数组不同，数组的元素既可以是基本类型的值，也可以是对象的引用；而集合中只能保存对象的引用，不能保存基本数据类型的值。

### 3.2. API 对比

- 统一 API

  - JCF 中的所有类都支持以下两个构造器：
    - `X()`: 空构造器，构造一个空的集合。
    - `X(Collection<? extends E> c)`: 使用指定集合构造一个新的集合。
  - `int size()`: 返回集合元素数量。
  - `boolean isEmpty()`: 返回集合是否为空。
  - `Object clone​()`: 返回集合对象的复制。

  - Collection 统一 API( 定义在 AbstractCollection 中 ):
    - `boolean contains(Object o)`:
    - `Object[] toArray()`:
    - `boolean add(E e)`:
    - `boolean addAll(Collection<? extends E> c)`:
    - `boolean remove(Object o)`: 
    - `boolean removeAll(Collection<?> c)`: 
  
  - Map 统一 API( 定义在 AbstractMap 中 ):
    - `V get(Object key)`: 
    - `V put(K key, V value)`: 
    - `void putAll(Map<? extends K, ? extends V> m)`:
    - `V remove(Object key)`:
    - `boolean containsValue(Object value)`: 
    - `boolean containsKey(Object key)`: 

- 特有 API

  - 栈结构
    - 基于双向链表实现：LinkedList
    - 基于循环数组实现：ArrayDeque
      - `void push(E e)`/`void addFirst(E e)`: 将指定元素压入栈。栈为满时抛出异常。
      - `E pop()`/`E removeFirst()`: 返回栈顶元素，并将栈顶元素移除。栈为空时抛出异常。
      - `E element()`/`E getFirst()`: 返回栈顶元素但不移除。栈为空时抛出 NoSuchElementException 异常。

  - 队列结构
    - 基于双向链表实现：LinkedList
    - 基于循环数组实现：ArrayDeque
      - `boolean add(E e)`/`void addLast(E e)`: 将指定元素加入队列尾部。队列为满时抛出异常。
      - `E remove()`/`E removeFirst()`: 返回队列头部元素，并将队列头部元素移除。队列为空时抛出异常。
      - `E element()`/`E getFirst()`: 返回队列头部元素但不移除。队列为空时抛出 NoSuchElementException 异常。

  - 优先队列结构
    - 基于最小堆实现：PriorityQueue
      - `boolean add(E e)`/`boolean offer(E e)`: 将指定元素加入队列。
      - `E poll​()`: **返回队列头部元素，并将队列头部元素移除**。队列为空时返回 null。
      - `E peek()`: 返回队列头部元素但不移除。队列为空时返回 null。
    
  - map 结构
    - 基于哈希表实现：HashMap
    - 基于红黑树实现：TreeMap

  - set 结构
    - 基于哈希表实现：HashSet
    - 基于红黑树实现：TreeSet

  - 可变数组结构
    - ArrayList

  - 双向链表结构
    - LinkedList

### 3.3. 快速失败机制

TODO:

在 Java 集合框架中，很多类都实现了快速失败机制。该机制被触发时，会抛出并发修改异常 `java.lang.ConcurrentModificationException`，以避免程序在将来不确定的时间里出现不确定的行为。

### 3.4. 存储 null 值对比

对于不允许为 null 的容器，添加元素为 null 时会抛出 `java.lang.NullPointerException` 异常。

- Map
  - Hashtable: Key 不允许为 null，Value 不允许为 null。
  - ConcurrentMap (ConcurrentHashMap/ConcurrentSkipListMap): Key 不允许为 null，Value 不允许为 null。
  - TreeMap: Key 不允许为 null，Value 允许为 null。
  - EnumMap: Key 不允许为 null，Value 允许为 null。
  - HashMap/LinkedHashMap: Key 允许为 null（Hash 函数将 null 作为 0 处理），Value 允许为 null。
- Set: 
  - TreeSet: 不允许存储 null。
  - EnumSet: 不允许存储 null。
  - HashSet/LinkedHashSet: 允许存储 null，但只能存储一个，即使添加多个也只能存储一个。
- List (ArrayList/LinkedList/Vector/Stack): 允许存储 null，添加几个，存储几个。
- Queue (PriorityQueue/ArrayDeque): 不允许存储 null。

P.S. [为什么线程不安全的 HashMap 允许 K/V 为 null 而线程安全的 Hashtable/ConcurrentHashMaps 都不允许？](https://stackoverflow.com/questions/698638/why-does-concurrenthashmap-prevent-null-keys-and-values)
> The main one is that if map.get(key) returns null, you can't detect whether the key explicitly maps to null vs the key isn't mapped. In a non-concurrent map, you can check this via map.contains(key), but in a concurrent one, the map might have changed between calls.

### 3.5. 遍历方法

#### 3.5.1. 遍历 Collection

Collection 集合元素被访问的顺序取决于集合类型。

- 若对 List 进行迭代，则迭代器将从索引 0 处开始，每迭代一次索引值加 1。
- 若对 Set 进行迭代，虽能确定最终结果能遍历所有元素，但无法预知元素被访问的顺序。

Collection 集合的遍历主要有以下方法：
- 通过 Iterable 接口的 forEach 方法使用 Lambda 表达式遍历集合
  ```java
  @Test
  public void test() {
    Collection<String> books = new HashSet<>();
    books.add("书 A");
    books.add("书 B");
    books.add("书 C");
    books.forEach(elemet -> System.out.println(elemet));
  }
  ```

- 通过 Iterator 接口遍历集合
  
  任何实现了 Iterable 接口的类，都包含了与之绑定的成员变量 Iterator，由于 Collection 接口扩展了 Iterable 接口，因此任何 Collection 集合包含了与之绑定的迭代器 Iterator，从而可以通过与 Iterator 接口相关的以下几种方法来遍历集合：

  - 使用 Iterator 迭代器遍历集合
    ```java
    @Test
    public void test() {
      Collection<String> books = new HashSet<>();
      books.add("书 A");
      books.add("书 B");
      books.add("书 C");

      Iterator<String> iterator = books.iterator();
      System.out.println(books.size());
      while (iterator.hasNext()) {
        System.out.println(iterator.next());
        iterator.remove();
      }
      System.out.println(books.size());
    }
    ```
    NOTE: 

    <!-- TODO: 详细了解 fail-fast 机制 -->
    
    Iterator 迭代器采用 **fail-fast 机制**，一旦在迭代过程中检测到该集合被修改，程序会立即抛出 java.util.ConcurrentModificationException 异常，而不是显示修改后的结果，从而**避免共享资源而引发的线程安全问题**。

    因此，**当使用 Iterator 迭代器迭代访问集合元素时，除了可以通过 Iterator 接口的 remove() 方法删除上一次 next() 方法返回的集合元素，不能通过其它任何方式改变集合中的元素，否则将会引发 ConcurrentModificationException 异常**。

  - 使用 Java 5 提供的 foreach 循环遍历集合
    ```java
    @Test
    public void test() {
      Collection<String> books = new HashSet<>();
      books.add("书 A");
      books.add("书 B");
      books.add("书 C");

      for (String book : books) {
        System.out.println(book);
      }
    }
    ```
    foreach循环编译后实际上也是基于Iterator 迭代器遍历集合。

  - 通过 Iterator 接口的 forEachRemaining​() 方法使用 Lambda 表达式遍历集合
    ```java
    @Test
    public void test() {
      Collection<String> books = new HashSet<>();
      books.add("书 A");
      books.add("书 B");
      books.add("书 C");

      Iterator<String> iterator = books.iterator();
      iterator.forEachRemaining(elemet -> System.out.println(elemet));
    }
    ```

- 通过 Java 8 提供的 Stream 接口使用 Lambda 表达式遍历集合 
  ```java
  @Test
  public void test() {
    Collection<String> books = new HashSet<>();
    books.add("书 A");
    books.add("书 B");
    books.add("书 C");
  
    books.stream().forEach(element -> System.out.println(element));
  }
  ```

#### 3.5.2. 遍历 Map

Map 集合的遍历主要有以下方法：
- 通过 Map 接口的 forEach 方法使用 lambda 表达式遍历 Map
  ```java
  Map<String, String> map = new HashMap<String, String>();
  // ...
  map.forEach((k, v) -> {
    System.out.print(k + "=" + v + " ");
  });
  ```

- 通过 `public Set<Map.Entry<K,V>> entrySet()` 返回的 Key-Value Set （无重复元素）遍历 Map
  
  Map 接口中的 `entrySet()` 方法返回一个 `Set<Map.Entry<K,V>>` 类型的 Set 集合，通过该 Set 集合的迭代器 Iterator，可使用以下几种方法来遍历集合：
  - 使用 Iterator 迭代器遍历集合
    ```java
    Map<String, String> map = new HashMap<String, String>();
    // ...
    Iterator<Map.Entry<String, String>> iterator = map.entrySet().iterator();
    while (iterator.hasNext()) {
      Map.Entry<String, String> entry = iterator.next();
      entry.getKey();
      entry.getValue();
    }
    ```

  - 使用 Java 5 提供的 foreach 循环遍历集合
    ```java
    Map<String, String> map = new HashMap<String, String>();
    // ...
    for (Entry<String, String> entry : map.entrySet()) {
      entry.getKey();
      entry.getValue();
    }
    ```
    foreach循环编译后实际上也是基于Iterator 迭代器遍历集合。

  - 通过 Iterator 接口的 forEachRemaining​() 方法使用 Lambda 表达式遍历集合
    ```java
    Map<String, String> map = new HashMap<String, String>();
    // ...
    Iterator<String> iterator = map.entrySet().iterator();
    iterator.forEachRemaining((k, v) -> System.out.println(k + "=" + v));
    ```

- 通过 `public Set<K> keySet()` 返回的 Key Set （无重复元素）遍历 Map

  Map 接口中的 `keySet()` 方法返回一个 `Set<K>` 类型的 Set 集合，通过该 Set 集合的迭代器 Iterator，可使用以下几种方法来遍历集合：
  - Iterator 迭代器
    ```java
    Map<String, String> map = new HashMap<String, String>();
    Iterator<K> iterator = map.keySet().iterator();
    while (iterator.hasNext()) {
      String key = iterator.next();
      map.get(key);
    }
    ```
    
  - forEach
    ```java
    Map<String, String> map = new HashMap<String, String>();
    for (String key : map.keySet()) {
      map.get(key);
    }
    ```
    foreach循环编译后实际上也是基于Iterator 迭代器遍历集合。

  - forEachRemaining
    ```java
    Map<String, String> map = new HashMap<String, String>();
    // ...
    Iterator<String> iterator = map.keySet().iterator();
    iterator.forEachRemaining(key -> System.out.println(key + "=" + map.get(key)));
    ```

- 通过 `public Collection<V> values()` 返回的 Value List （有重复元素）遍历 Map

  Map 接口中的 `values()` 方法返回一个 `Collection<V>` 类型的 Collection 集合，通过该 Collection 集合的迭代器 Iterator，可使用以下几种方法来遍历集合：
  - Iterator 迭代器
  - forEach
  - forEachRemaining

## 4. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)

[高度注意 Map 类集合 K/V 能不能存储 null 值的情况](https://blog.csdn.net/suyujiezhang/article/details/78815112)

[HashMap循环遍历方式及其性能对比](http://www.trinea.cn/android/hashmap-loop-performance/)
