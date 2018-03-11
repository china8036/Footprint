- [Java 集合：Map 族](#java-%E9%9B%86%E5%90%88%EF%BC%9Amap-%E6%97%8F)
  - [1. Map 接口](#1-map-%E6%8E%A5%E5%8F%A3)
  - [2. HashMap 接口](#2-hashmap-%E6%8E%A5%E5%8F%A3)
  - [3. SortedMap 接口 & TreeMap 类](#3-sortedmap-%E6%8E%A5%E5%8F%A3-treemap-%E7%B1%BB)
  - [4. WeakHashMap](#4-weakhashmap)
  - [5. Refer Links](#5-refer-links)

# Java 集合：Map 族

## 1. Map 接口

[Map](https://docs.oracle.com/javase/9/docs/api/java/util/Map.html) 是 Java 集合类库中的两个基本接口之一。

不同于 Collection 接口，Map 用于存储键值对的映射。因此，其 API 与 Collection 存在显著差别：
- `V	put​(K key, V value)`

  不同于 Collection 中添加元素的 add 方法，Map 中使用 put 添加元素，包含了一对键值对。

  NOTE：**键可以为 null，键值不可以为 null**。

- `default V	putIfAbsent​(K key, V value)`

  只有当键存在时，才会放入一个值。

- `V	get​(Object key)`

  通过 key 从 Map 获取键值。

- `default V	getOrDefault​(Object key, V defaultValue)`

  通过 key 从 Map 获取键值。若在映射中未找到这个键，则返回 defaultValue。

- `default void	forEach​(BiConsumer<? super K,? super V> action)`

  该方法从 Java SE 8 开始加入，可通过为该方法提供一个 lambda 表达式，对迭代器的每一个元素调用该 lambda 表达式进行处理。
  ```java
  maps.forEach(k, v) ->
    System,out,println(k, v);
  ```

- `boolean	containsKey​(Object key`

  若映射中已存在该键，返回 true。

- `boolean	containsValue​(Object value)`

  若映射中已存在该键值，返回 true。

Java 中为映射提供了两个通用的实现：HashMap 和 TreeMap。

## 2. HashMap 接口

散列映射 [HashMap](https://docs.oracle.com/javase/9/docs/api/java/util/HashMap.html) 对键进行散列，并将其组织成搜索树。散列函数只能作用于键。

构造器
- `HashMap​()`
- `HashMap​(int initialCapacity)`
- `HashMap​(int initialCapacity, float loadFactor)`

## 3. SortedMap 接口 & TreeMap 类

树映射 [TreeMap](https://docs.oracle.com/javase/9/docs/api/java/util/TreeMap.html) 用键的整体顺序对元素进行排序，并将其组织成搜索树。比较函数只能作用于键。

若不需要排序，应选择 HashMap，避免用于排序的开销。

构造器
- `TreeMap​()	`
- `TreeMap​(Comparator<? super K> comparator)	`
- `TreeMap​(Map<? extends K,? extends V> m)	`
- `TreeMap​(SortedMap<K,? extends V> m)`

常用 API
- `Comparator<? super K>	comparator​()`

  返回对键的比较器。如果键使用 Comparable 接口的 compareTo 方法进行比较，返回 null。

- `K	firstKey​()`

  返回映射中的最小元素。

- `K	lastKey​()`

  返回映射中的最大元素。

- `Map.Entry<K,V>	firstEntry​()`

## 4. WeakHashMap

对于 HashMap 存在这样的问题：由于 GC 跟踪活动的对象，只要映射对象是活动的，其中所有的桶也是活动的，因此即时一条映射的键值已不再使用了，GC 也不会回收，这导致了开发者需要负责从长期存活的映射表中删除无用的值。

为解决这个问题，Java 集合框架提供了 [WeakHashMap](https://docs.oracle.com/javase/9/docs/api/java/util/WeakHashMap.html)。当对键的唯一引用来自散列条目时，WeakHashMap 将与 GC 协同工作一起删除键值对。

WeakHashMap 使用弱引用保存键。WeakReference 对象将引用保存到另外一个对象中，即散列键。若某个对象只能由 WeakReference 引用，GC 仍然会回收它，但要将引用该对象的 WeakReference 放入队列中。WeakHashMap 会周期性检查队列，找出新添加的弱引用并删除其对应的条目。

## 5. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)