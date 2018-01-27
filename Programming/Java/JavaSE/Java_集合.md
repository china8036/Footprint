- [Java 集合](#java-%E9%9B%86%E5%90%88)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [设计思想](#%E8%AE%BE%E8%AE%A1%E6%80%9D%E6%83%B3)
    - [类谱图](#%E7%B1%BB%E8%B0%B1%E5%9B%BE)
    - [Collection](#collection)
    - [Iterator](#iterator)
    - [Map](#map)
  - [List](#list)
    - [ListIterator](#listiterator)
    - [LinkedList](#linkedlist)
      - [构造器](#%E6%9E%84%E9%80%A0%E5%99%A8)
      - [添加元素](#%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
    - [访问元素](#%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
    - [ArrayList](#arraylist)
      - [构造器](#%E6%9E%84%E9%80%A0%E5%99%A8)
      - [添加 & 删除元素](#%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
      - [访问元素](#%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
      - [转换为数组](#%E8%BD%AC%E6%8D%A2%E4%B8%BA%E6%95%B0%E7%BB%84)
      - [其它](#%E5%85%B6%E5%AE%83)
  - [Set](#set)
    - [HashSet](#hashset)
    - [SortedSet 接口 &TreeSet 类](#sortedset-%E6%8E%A5%E5%8F%A3-treeset-%E7%B1%BB)
  - [Queue](#queue)
    - [LinkedList](#linkedlist)
    - [PriorityQueue](#priorityqueue)
  - [Map](#map)
    - [HashMap](#hashmap)
    - [SortedMap 接口 &TreeMap 类](#sortedmap-%E6%8E%A5%E5%8F%A3-treemap-%E7%B1%BB)
    - [WeakHashMap](#weakhashmap)
  - [集合工具类：Arrays 和 Collections](#%E9%9B%86%E5%90%88%E5%B7%A5%E5%85%B7%E7%B1%BB%EF%BC%9Aarrays-%E5%92%8C-collections)
  - [Refer Links](#refer-links)

# Java 集合

## 概述

Java 在 `java.util.*` 中为开发者提供了包含了一系列常用数据结构及其操作的工具包，即 Java 集合框架。

### 设计思想

- 接口与实现分离
  
  与现在数据结构类库常见的情况一样，Java 集合类库将接口与实现分离。同一个接口可以有多种实现方式，便于开发者选择最适合自己的实现类进行使用。一旦想要改变，由于实现自同一个接口，所包含的方法及方法调用结果完全相同，因此只需修改类对象的构造处，即可轻松选用另外一种实现方式。

  在集合类谱图中，还有一组以 `Abstract` 开头命名的类，这些类是为类库实现者设计的。其中默认实现了接口的大部分方法，开发者只需继承这些抽象类，便可只对个别方法进行重新实现，而不用实现接口的所有方法。
  

- 适配器设计模式

  Java 集合框架中大量使用了适配器模式进行实现。

### 类谱图

![image](/resources/images/java-collections.png)

集合有两个基本接口：Collection 和 Map。

### Collection 

Java 类库中，集合类最基本的接口是 [Collection](https://docs.oracle.com/javase/9/docs/api/java/util/Collection.html) 接口，用于存储相同类型的元素。

- 基本 API

  - `int	size​()`

    返回当前存储在集合中的元素个数。

  - `boolean	isEmpty​()`

    判断集合是否为空。

  - `boolean	contains​(Object o)`

    判断集合是否包含一个与对象 o 相等的元素。

  - `boolean	add​(E e)`
    
    向集合中添加元素。若操作改变了集合则返回 true，若集合没有发生变化则返回 false。

  - `Iterator<E>	iterator​()`

    返回一个实现了 Iterator 接口的对象，通过此迭代器对象可以依次访问集合中的元素。

  - `boolean	remove​(Object o)`

    删除集合中等于对象 o 的对象。若该方法使集合发生了改变则返回 true。

  - `default boolean	removeIf​(Predicate<? super E> filter)`

    删除 filter 返回 true 的所有元素。若该方法使集合发生了改变则返回 true。

  - `boolean	retainAll​(Collection<?> c)`

    删除集合中所有与 other 集合不同的元素。若该方法使集合发生了改变则返回 true。

  - `Object[]	toArray​()`
  
    返回集合的对象数组，并填入 arrayToFill 中。若 arrayToFill 足够大，则剩余空间补 null；否则分配一个新的数组，其长度等于集合的大小，并重新填充集合元素。

### Iterator

[Iterator](https://docs.oracle.com/javase/9/docs/api/java/util/Iterator.html) 接口用于对象的迭代。

任何实现了 Iterator 接口的类，都可以用 for each 循环进行遍历。Collection 接口扩展了 Iterator 接口，因此对于 Java 集合类库中的任何类都可以使用 for each 循环进行遍历。

元素被访问的顺序取决于集合类型。若对 List 进行迭代，则迭代器将从索引 0 处开始，每迭代一次索引值加 1；若对 Set 进行迭代，虽能确定最终结果能遍历所有元素，但无法预知元素被访问的顺序。

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

    该方法从 Java SE 8 开始加入，可通过为该方法提供一个 lambda 表达式，对迭代器的每一个元素调用该 lambda 表达式进行处理。
    ```java
    todo:
    ```

### Map

[Map](https://docs.oracle.com/javase/9/docs/api/java/util/Map.html) 是 Java 集合类库中的两个基本接口之一。

不同于 Collection 接口，Map 用于存储键值对的映射。因此，其 API 与 Collection 存在显著差别：
- `V	put​(K key, V value)`

  不同于 Collection 中添加元素的 add 方法，Map 中使用 put 添加元素，包含了一对键值对。

- `V	get​(Object key)`

  通过 key 从 Map 获取键值。

## List

[List](https://docs.oracle.com/javase/9/docs/api/java/util/List.html) 是一个有序集合，元素会添加到容器中的特定位置。

可以采用两种方式访问 List 元素：
- 使用整数索引访问（随机访问）
  
  List 接口中提供了多个基于随机访问的 API：
  - `void	add​(int index, E element)`
  - `E	remove​(int index)`
  - `E	get​(int index)`
  - `E	set​(int index, E element)`
  <!-- `boolean	add​(E e)`
  `boolean	remove​(Object o)`  TODO: 这两个的实现源码是顺序访问还是随机访问的？ -->

- 使用迭代器访问（顺序访问）
  
  List 接口的顺序访问需要使用 ListIterator 接口。

P.S. 

core Java 10th
> 集合框架的这个方面设计得很不好。实际中有两种有序集合，其性能有着很大差异。数组适合随机访问，但链表适合迭代器顺序访问。若能提供两个接口就会容易一些了。

可通过工具类 RandomAccess 来判断一个集合是否支持高效的随机访问：
```java
if (c instanceof RandomAccess) {
  // 支持 
} else {
  // 不支持
}
```

### ListIterator

[ListIterator](https://docs.oracle.com/javase/9/docs/api/java/util/ListIterator.html) 接口是 Iterator 的子接口，是用于顺序访问的迭代器，主要用在链表结构中。

- `void	add​(E e)`：在迭代器前添加元素。

- `boolean	hasPrevious​()`

- `E	previous​()`：返回越过的对象。

  NOTE：调用 next 后再调用 remove 方法，会删除迭代器左侧的元素，但调用 previous 后再调用 remove 方法，会删除迭代器右侧的元素，并且同样不能连续调用两次 remove。

- `int	previousIndex​()`：返回上一个元素的索引。

- `int	nextIndex​()`：返回下一个元素的索引。

- `void	set​(E e)`

### LinkedList

在 Java 中，[LinkedList](https://docs.oracle.com/javase/9/docs/api/java/util/LinkedList.html) 是链表的数据结构封装，而且 Java 中所有的链表都是双向链表。

#### 构造器

- `LinkedList​()`

  构造一个空链表。

- `LinkedList​(Collection<? extends E> c)`

  构造一个链表，并将所给集合中的所有元素添加到这个链表中。

#### 添加元素

LinkedList 提供了	`add​(E e)` 方法将对象添加到链表的尾部，但对于链表中间位置的元素添加，需要使用迭代器 ListIterator 中的 add 方法。

- `ListIterator<E>	listIterator​(int index)`：返回一个 ListIterator 迭代器。

P.S. 并非所有的集合都需要使用迭代器来插入中间位置的元素，比如无序的 Set 结构，其中元素完全无序。因此，Java 集合框架的设计者不在 Iterator 接口中定义 add 方法，而是在 Iterator 的子接口 ListIterator 中才定义了 add 方法，更具实际意义。

### 访问元素

LinkedList 提供了一个用于访问元素的 get 方法，但该方法的实现效率很低。

虽然 get 方法做了优化（索引大于 size/2 时，从链表尾部开始搜索元素），但是，若要访问第 n 个元素，就需要从链表头部开始，越过 n-1 个元素，没有任何捷径可走，且 LinkedList 对象访问元素时不做任何缓存位置信息的操作。

因此，若需要使用该方法访问元素，应考虑是否更换所用的数据结构。

core java
> 我们建议避免使用以整数索引表示链表中位置的所有方法。若需要对集合进行随机访问，就使用数组或 ArrayList，不要使用链表。

### ArrayList

Java 中允许创建数组的长度为变量，即允许在运行时动态确定数组的大小，但这并没有解决运行时动态改变数组大小的问题。一旦确定了数组的大小，再改变它就不太容易了（比如通过 Arrays.copyOf 进行扩容）。因此，Java 提供了 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html) 类。

ArrayList 是一个采用类型参数的泛型类，在添加或删除数组元素时，它具有自动调节数组容量的功能。

#### 构造器

ArrayList 由三种构造方式：

- `ArrayList​()`：构造一个空的数组列表。
  ```java
  ArrayList<Employee> staff = new ArrayList<>(); 
  ```

- `ArrayList​(int initialCapacity)`：构造一个具有初始容量的数组列表。
  ```java
  ArrayList<Employee> staff = new ArrayList<>(100); 
  ```
  NOTE：数组列表的容量与普通数组的大小是有区别的。普通数组的大小，指的是数组初始化时已分配了 100 个元素的存储空间可供使用；数组列表的容量，指的是该数组列表拥有保存 100 个元素的潜力（实际上还可以通过重新分配空间从而超过 100 个），但是在完成初始化时，数组列表根本不含有任何元素空间。

- `ArrayList​(Collection<? extends E> c)`：以集合元素作为初始值构造一个数组列表。

NOTE: 尖括号内不可使用基本数据类型。

#### 添加 & 删除元素

`void	add​(E element)`：可为数组列表创建一块新的存储空间并添加元素。若调用 add 时内部数组已满，数组列表将自动创建一个更大的数组，并将所有的对象从较小的数组中拷贝到一个较大的数组中。

`void	add​(int index, E element)`：在数组指定位置插入元素。为插入该元素，位于 index 之后的所有元素都要向后移动一个位置，若插入的元素导致数组列表大小超过了容量，数组列表会被重新分配存储空间，在原来基础上增大一半。

`E	remove​(int index)`：删除数组列表中指定位置的元素。位于 index 之后的元素都向前移动一个位置，并且数组大小减一。

`boolean	remove​(Object o)`：删除数组列表中值为 o 的元素。

NOTE：

对数组进行插入或删除元素的操作效率都比较低。对小型数组而言，这部分效率的损失还不必担心；但对于大型数组，频繁的在数组的中间位置插入、删除元素，将耗费大量时间成本在元素的移动上，事实上这种情况下应该使用链表作为数据结构存储。

#### 访问元素

数组列表的自动扩展容量带来了便利，但也增加了访问元素时语法的复杂性。因为 ArrayList 不是 Java 程序设计语言的一部分，只是由其他人编写并包含在标准库中的实用工具类，而 Java 中不支持重载运算符的特性，因此在 ArrayList 中访问元素无法使用普通数组的`[]`格式，而需要使用 get 和 set 方法进行访问。

`E	get​(int index)`：获取数组列表指定位置的元素。

`E	set​(int index, E element)`: 设置数组列表指定位置的元素的值。该方法不会创建新的存储空间，因此不可用于添加新元素。

#### 转换为数组

`Object[]	toArray​()`：使用该方法将数组列表中的数组元素拷贝到一个数组中。

#### 其它

`int	size​()`：返回数组列表的元素个数。

`void	ensureCapacity​(int minCapacity)`：若能够知道数组可能存储的元素数量，可在填充数组之前调用该方法。该方法会分配一个包含 minCapacity 个对象的内部数组，再调用 minCapacity 次 add 方法，而不用重新分配空间。

`void	trimToSize​()`：当确认数组列表的大小将不再发生变化，可以调用该方法，将存储区域的大小调整为当前元素数量所需要的存储空间数目，多余的空间由垃圾回收器回收。需要注意的是，一旦调用该方法整理数组列表的大小后，添加新元素就需要耗费时间再次移动存储快，因此应该在确认不会再添加任何元素时，才调用该方法。

## Set

[Set](https://docs.oracle.com/javase/9/docs/api/java/util/Set.html) 是一个无序、元素不可以重复的集合，其方法的行为相比 Collection 接口有更严谨的定义：
- `boolean	add​(E e)`

  添加元素时不允许添加重复的元素。

- `boolean	equals​(Object o)`
  
  只要两个集合包含的元素相同就返回 true，不要求有相同的顺序。

- `int	hashCode​()`

  要保证包含相同元素的两个集合会得到相同的散列码。

### HashSet

### SortedSet 接口 &TreeSet 类

## Queue

### LinkedList

### PriorityQueue

## Map

### HashMap

### SortedMap 接口 &TreeMap 类

### WeakHashMap

## 集合工具类：Arrays 和 Collections

## Refer Links

[java 集合系列](http://blog.csdn.net/u010648555/article/details/56049460)