- [Java 集合](#java-%E9%9B%86%E5%90%88)
  - [1. 概述](#1-%E6%A6%82%E8%BF%B0)
    - [1.1. 设计思想](#11-%E8%AE%BE%E8%AE%A1%E6%80%9D%E6%83%B3)
    - [1.2. 类谱图](#12-%E7%B1%BB%E8%B0%B1%E5%9B%BE)
  - [2. Collection](#2-collection)
  - [3. Iterator](#3-iterator)
  - [4. List](#4-list)
    - [4.1. ListIterator](#41-listiterator)
    - [4.2. LinkedList](#42-linkedlist)
      - [4.2.1. 构造器](#421-%E6%9E%84%E9%80%A0%E5%99%A8)
      - [4.2.2. 添加元素](#422-%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
    - [4.3. 访问元素](#43-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
    - [4.4. ArrayList](#44-arraylist)
      - [4.4.1. 构造器](#441-%E6%9E%84%E9%80%A0%E5%99%A8)
      - [4.4.2. 添加 & 删除元素](#442-%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
      - [4.4.3. 访问元素](#443-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
      - [4.4.4. 转换为数组](#444-%E8%BD%AC%E6%8D%A2%E4%B8%BA%E6%95%B0%E7%BB%84)
      - [4.4.5. 其它](#445-%E5%85%B6%E5%AE%83)
  - [5. Set](#5-set)
    - [5.1. HashSet](#51-hashset)
      - [5.1.1. 构造器](#511-%E6%9E%84%E9%80%A0%E5%99%A8)
      - [5.1.2. 添加 & 删除元素](#512-%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
      - [5.1.3. 访问元素](#513-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
      - [5.1.4. 其它](#514-%E5%85%B6%E5%AE%83)
    - [5.2. SortedSet 接口 & TreeSet 类](#52-sortedset-%E6%8E%A5%E5%8F%A3-treeset-%E7%B1%BB)
  - [6. Queue](#6-queue)
    - [6.1. PriorityQueue](#61-priorityqueue)
    - [6.2. Deque](#62-deque)
    - [6.3. ArrayDeque](#63-arraydeque)
    - [6.4. LinkedList](#64-linkedlist)
  - [7. Map](#7-map)
    - [7.1. HashMap](#71-hashmap)
    - [7.2. SortedMap 接口 & TreeMap 类](#72-sortedmap-%E6%8E%A5%E5%8F%A3-treemap-%E7%B1%BB)
    - [7.3. WeakHashMap](#73-weakhashmap)
  - [8. 算法包装类 Collections](#8-%E7%AE%97%E6%B3%95%E5%8C%85%E8%A3%85%E7%B1%BB-collections)
    - [8.1. 排序](#81-%E6%8E%92%E5%BA%8F)
    - [8.2. 混排](#82-%E6%B7%B7%E6%8E%92)
    - [8.3. 搜索](#83-%E6%90%9C%E7%B4%A2)
    - [8.4. 其它](#84-%E5%85%B6%E5%AE%83)
  - [9. Refer Links](#9-refer-links)

# Java 集合

## 1. 概述

Java 在 `java.util.*` 中为开发者提供了包含了一系列常用数据结构及其操作的工具包，即 Java 集合框架。

### 1.1. 设计思想

- 接口与实现分离
  
  与现在数据结构类库常见的情况一样，Java 集合类库将接口与实现分离。同一个接口可以有多种实现方式，便于开发者选择最适合自己的实现类进行使用。一旦想要改变，由于实现自同一个接口，所包含的方法及方法调用结果完全相同，因此只需修改类对象的构造处，即可轻松选用另外一种实现方式。

  在集合类谱图中，还有一组以 `Abstract` 开头命名的类，这些类是为类库实现者设计的。其中默认实现了接口的大部分方法，开发者只需继承这些抽象类，便可只对个别方法进行重新实现，而不用实现接口的所有方法。
  

- 适配器设计模式

  Java 集合框架中大量使用了适配器模式进行实现。

### 1.2. 类谱图

![image](/resources/images/java-collection.png)

集合有两个基本接口：Collection 和 Map。

## 2. Collection

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

## 3. Iterator

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

## 4. List

[List](https://docs.oracle.com/javase/9/docs/api/java/util/List.html) 是一个有序集合，元素会添加到容器中的特定位置。

优点
- 元素按顺序排列
优点
- 搜索时需要遍历元素，查找速度较慢

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

### 4.1. ListIterator

[ListIterator](https://docs.oracle.com/javase/9/docs/api/java/util/ListIterator.html) 接口是 Iterator 的子接口，是用于顺序访问的迭代器，主要用在链表结构中。

- `void	add​(E e)`：在迭代器前添加元素。

- `boolean	hasPrevious​()`

- `E	previous​()`：返回越过的对象。

  NOTE：调用 next 后再调用 remove 方法，会删除迭代器左侧的元素，但调用 previous 后再调用 remove 方法，会删除迭代器右侧的元素，并且同样不能连续调用两次 remove。

- `int	previousIndex​()`：返回上一个元素的索引。

- `int	nextIndex​()`：返回下一个元素的索引。

- `void	set​(E e)`

### 4.2. LinkedList

在 Java 中，[LinkedList](https://docs.oracle.com/javase/9/docs/api/java/util/LinkedList.html) 是链表的数据结构封装，而且 Java 中所有的链表都是双向链表。

优点
- 便于在集合中间位置插入元素
缺点
- 访问每个元素只能从头搜索，随机访问效率极低

#### 4.2.1. 构造器

- `LinkedList​()`

  构造一个空链表。

- `LinkedList​(Collection<? extends E> c)`

  构造一个链表，并将所给集合中的所有元素添加到这个链表中。

#### 4.2.2. 添加元素

LinkedList 提供了	`add​(E e)` 方法将对象添加到链表的尾部，但对于链表中间位置的元素添加，需要使用迭代器 ListIterator 中的 add 方法。

- `ListIterator<E>	listIterator​(int index)`：返回一个 ListIterator 迭代器。

P.S. 并非所有的集合都需要使用迭代器来插入中间位置的元素，比如无序的 Set 结构，其中元素完全无序。因此，Java 集合框架的设计者不在 Iterator 接口中定义 add 方法，而是在 Iterator 的子接口 ListIterator 中才定义了 add 方法，更具实际意义。

### 4.3. 访问元素

LinkedList 提供了一个用于访问元素的 get 方法，但该方法的实现效率很低。

虽然 get 方法做了优化（索引大于 size/2 时，从链表尾部开始搜索元素），但是，若要访问第 n 个元素，就需要从链表头部开始，越过 n-1 个元素，没有任何捷径可走，且 LinkedList 对象访问元素时不做任何缓存位置信息的操作。

因此，若需要使用该方法访问元素，应考虑是否更换所用的数据结构。

core java
> 我们建议避免使用以整数索引表示链表中位置的所有方法。若需要对集合进行随机访问，就使用数组或 ArrayList，不要使用链表。

### 4.4. ArrayList

Java 中允许创建数组的长度为变量，即允许在运行时动态确定数组的大小，但这并没有解决运行时动态改变数组大小的问题。一旦确定了数组的大小，再改变它就不太容易了（比如通过 Arrays.copyOf 进行扩容）。因此，Java 提供了 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html) 类。

ArrayList 是一个采用类型参数的泛型类，在添加或删除数组元素时，它具有自动调节数组容量的功能。

优点
- 随机访问元素效率高
缺点
- 在集合中间位置插入元素时需要移动该元素后的所有元素，效率低

#### 4.4.1. 构造器

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

#### 4.4.2. 添加 & 删除元素

`void	add​(E element)`：可为数组列表创建一块新的存储空间并添加元素。若调用 add 时内部数组已满，数组列表将自动创建一个更大的数组，并将所有的对象从较小的数组中拷贝到一个较大的数组中。

`void	add​(int index, E element)`：在数组指定位置插入元素。为插入该元素，位于 index 之后的所有元素都要向后移动一个位置，若插入的元素导致数组列表大小超过了容量，数组列表会被重新分配存储空间，在原来基础上增大一半。

`E	remove​(int index)`：删除数组列表中指定位置的元素。位于 index 之后的元素都向前移动一个位置，并且数组大小减一。

`boolean	remove​(Object o)`：删除数组列表中值为 o 的元素。

NOTE：

对数组进行插入或删除元素的操作效率都比较低。对小型数组而言，这部分效率的损失还不必担心；但对于大型数组，频繁的在数组的中间位置插入、删除元素，将耗费大量时间成本在元素的移动上，事实上这种情况下应该使用链表作为数据结构存储。

#### 4.4.3. 访问元素

数组列表的自动扩展容量带来了便利，但也增加了访问元素时语法的复杂性。因为 ArrayList 不是 Java 程序设计语言的一部分，只是由其他人编写并包含在标准库中的实用工具类，而 Java 中不支持重载运算符的特性，因此在 ArrayList 中访问元素无法使用普通数组的`[]`格式，而需要使用 get 和 set 方法进行访问。

`E	get​(int index)`：获取数组列表指定位置的元素。

`E	set​(int index, E element)`: 设置数组列表指定位置的元素的值。该方法不会创建新的存储空间，因此不可用于添加新元素。

#### 4.4.4. 转换为数组

`Object[]	toArray​()`：使用该方法将数组列表中的数组元素拷贝到一个数组中。

#### 4.4.5. 其它

`int	size​()`：返回数组列表的元素个数。

`void	ensureCapacity​(int minCapacity)`：若能够知道数组可能存储的元素数量，可在填充数组之前调用该方法。该方法会分配一个包含 minCapacity 个对象的内部数组，再调用 minCapacity 次 add 方法，而不用重新分配空间。

`void	trimToSize​()`：当确认数组列表的大小将不再发生变化，可以调用该方法，将存储区域的大小调整为当前元素数量所需要的存储空间数目，多余的空间由垃圾回收器回收。需要注意的是，一旦调用该方法整理数组列表的大小后，添加新元素就需要耗费时间再次移动存储快，因此应该在确认不会再添加任何元素时，才调用该方法。

## 5. Set

[Set](https://docs.oracle.com/javase/9/docs/api/java/util/Set.html) 是一种无序、元素不可以重复的集合。

优点
- 快速查找元素，利于搜索
缺点
- 无法控制元素出现的次序

Set 结构提供的方法的行为相比 Collection 接口有更严谨的定义：
- `boolean	add​(E e)`

  添加元素时不允许添加重复的元素。

- `boolean	equals​(Object o)`
  
  只要两个集合包含的元素相同就返回 true，不要求有相同的顺序。

- `int	hashCode​()`

  要保证包含相同元素的两个集合会得到相同的散列码。

### 5.1. HashSet

Java 集合类库中提供了 [HashSet](https://docs.oracle.com/javase/9/docs/api/java/util/HashSet.html) 类，它实现了基于散列表的集合。

在 Java 种散列表使用链表数组实现，每个列表称为桶。要想查找表中元素的位置，需要先计算其散列码，然后与桶的总数取余，所得到的结果就是保存这个元素的桶的索引。例，某个对象散列码为 76268，且有 128 个桶，则对象应保存在第 108 号桶中（76268%128=108）。当桶被占满（散列冲突）时，需要用新对象与桶中的所有对象进行比较，查看这个对象是否已存在。

P.S. 在 Java 8 中，桶满时会从链表变成平衡二叉树。

桶数是指用于手机具有相同散列值的桶的数目，若大致知道最终有多少个元素要插入散列表中，就可以设置桶数。通常会将桶数设置为预计元素个数的 75%~150%。为防止键的聚集，最好将桶数设置素数。HashSet 中使用的桶数是 2 的幂，默认值为 16（为表大小提供的任何值都将被自动转换为 2 的下一个幂）。

当表太满时，就需要对散列表进行再散列。装填因子决定何时对表进行再散列，例如装填因子为 0.75 时，若表中超过 75% 的位置已填入元素，这个表就会以双倍的桶数自动进行再散列。对于大多数应用，装填因子设置为 0.75 是比较合理的。

散列将元素分散在散列表的各个位置上，若元素的散列码发生了变化，则元素在数据结构中的位置也会发生变化。

#### 5.1.1. 构造器

- `HashSet​()`

  构造一个空的散列表。

- `HashSet​(Collection<? extends E> c)`

  构造一个散列集，并将所给集合中的所有集合添加到该散列集中。

- `HashSet​(int initialCapacity)`

  构造一个空的具有指定容量 / 桶数的散列表。

- `HashSet​(int initialCapacity, float loadFactor)`

  构造一个空的具有指定容量 / 桶数和装填因子的散列表。

#### 5.1.2. 添加 & 删除元素

- `boolean	add​(E e)

- `boolean	remove​(Object o)

#### 5.1.3. 访问元素

散列表迭代器将依次访问所有的桶，但由于散列将元素分散在表中的各个位置上，因此在散列表中访问元素的顺序是完全随机的。

```java
Set<String> words = new HashSet<>();
//...
Iterator iter = words.iterator();
for (int i =1; i <= 20 && iter.hasNext(); i ++) {
  System.out.println(iter.next());
}
```

#### 5.1.4. 其它

`boolean	contains​(Object o)`：用于快速查看某个元素是否已经出现在散列集中。该方法不会查看集合中的所有元素，而只会在某个桶中查找元素。

### 5.2. SortedSet 接口 & TreeSet 类

[TreeSet](https://docs.oracle.com/javase/9/docs/api/java/util/TreeSet.html) 与散列表相似，但有所改进。

树集 (TreeSet) 是一个有序的集合，可以以任意顺序将元素插入到集合中，但在对集合进行遍历时，每个值将自动按照排序后的顺序呈现。

P.S. TreeSet 采用红黑树实现，每次将一个元素添加到树中，都将被放置在正确排序的位置上。因此，迭代器总是以排好序的顺序访问各个元素。若树中包含 n 个元素，则查找新元素位置的操作需要 log2(n) 次比较。因此，将一个元素添加到树中要比添加到散列集中慢，但相比检查数组或链表中的重复元素还是快很多。

树集中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

若不需要对集合的元素进行排序，那么选择散列集作为存储结构，可避免用于排序的开销。

## 6. Queue

队列 [Queue](https://docs.oracle.com/javase/9/docs/api/java/util/Queue.html) 接口支持有效地在集合的尾部添加元素、在头部删除元素，但不支持在集合的中间位置添加元素。

常用 API
- Insert：将给定元素添加到队列末尾。
  - `boolean	add​(E e)`：若队列已满，抛出一个异常。
  
  - `boolean	offer​(E e)`：若队列已满，返回 false。
  
- Remove：删除并返回队列头部的元素。
  - `E	remove​()`：若队列为空，抛出一个异常。
  
  - `E	poll​()`：若队列为空，返回 false。

- Examine：返回队列头部的元素但不删除。
  - `E	element​()`：若队列为空，抛出一个异常。

  - `E	peek​()`：若队列为空，返回 null。

### 6.1. PriorityQueue

优先级队列 [PriorityQueue](https://docs.oracle.com/javase/9/docs/api/java/util/PriorityQueue.html) 是 Queue 接口的实现类，队列中的元素可以按照任意的顺序插入，却总是按照排序的顺序进行检索。

优先级队列使用堆作为数据结构实现，因此不需要对所有的元素进行排序。每次调用 remove 方法，都会删除当前优先级队列中最小的元素；每次调用 add 方法，都会将最小的元素移动到根。

同 TreeSet 一样，优先级队列中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

优先级队列的典型应用场景是任务的调度。任务以随机顺序添加到任务队列中，但每一次进行任务调度时，都会将一个优先级最高的元素从队列中删除并开始执行（可设置优先级为 1 时最高）。

- 构造器
  - `PriorityQueue​()`
  - `PriorityQueue​(int initialCapacity)`	
  - `PriorityQueue​(int initialCapacity, Comparator<? super E> comparator)`

### 6.2. Deque

双端队列（double ended queue） [Deque](https://docs.oracle.com/javase/9/docs/api/java/util/Deque.html) 接口是 Queue 接口的扩展，支持有效地在头部和尾部同时添加或删除元素，但同样不支持在集合的中间位置添加元素。它既可以当作栈使用，也可以当作队列使用。

ArrayDeque 和 LinkedList 都是该接口的实现类，但官方更推荐使用 AarryDeque 用作栈和队列。

常用 API
- Insert：将给定元素添加到双端队列的头部或末尾。
  - `void	addFirst​(E e)`
  - `void	addLast​(E e)`
  - `boolean	offerFirst​(E e)`
  - `boolean	offerLast​(E e)`
- Remove：删除并返回队列头部或尾部的元素。
  - `E	removeFirst​()`
  - `E	removeLast​()`
  - `E	pollFirst​()`
  - `E	pollLast​()`
- Examine：返回双端队列头部或尾部的元素但不删除。
  - `E	getFirst​()`
  - `E	getLast​()`
  - `E	peekFirst​()`
  - `E	peekLast​()`

### 6.3. ArrayDeque

当需要使用栈时，Java 已不推荐使用 Stack，而是推荐使用更高效的 [ArrayDeque](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayDeque.html)。

ArrayDeque 依赖于可变数组实现，作为栈来使用时，效率要高于 Stack；作为队列使用时，效率相较于基于双向链表的 LinkedList 也要更好一些。

ArrayDeque 不支持为 null 的元素。

- 构造器
  - `ArrayDeque​()`
  - `ArrayDeque​(int numElements)`：用给定的初始容量构造一个无限双端队列。

### 6.4. LinkedList

## 7. Map

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

### 7.1. HashMap

散列映射 [HashMap](https://docs.oracle.com/javase/9/docs/api/java/util/HashMap.html) 对键进行散列，并将其组织成搜索树。散列函数只能作用于键。

构造器
- `HashMap​()`
- `HashMap​(int initialCapacity)`
- `HashMap​(int initialCapacity, float loadFactor)`

### 7.2. SortedMap 接口 & TreeMap 类

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

### 7.3. WeakHashMap

对于 HashMap 存在这样的问题：由于 GC 跟踪活动的对象，只要映射对象是活动的，其中所有的桶也是活动的，因此即时一条映射的键值已不再使用了，GC 也不会回收，这导致了开发者需要负责从长期存活的映射表中删除无用的值。

为解决这个问题，Java 集合框架提供了 [WeakHashMap](https://docs.oracle.com/javase/9/docs/api/java/util/WeakHashMap.html)。当对键的唯一引用来自散列条目时，WeakHashMap 将与 GC 协同工作一起删除键值对。

WeakHashMap 使用弱引用保存键。WeakReference 对象将引用保存到另外一个对象中，即散列键。若某个对象只能由 WeakReference 引用，GC 仍然会回收它，但要将引用该对象的 WeakReference 放入队列中。WeakHashMap 会周期性检查队列，找出新添加的弱引用并删除其对应的条目。

## 8. 算法包装类 Collections

虽没有 C++ 标准库中实现的几十种算法丰富，但 Java 集合框架也提供了基本的排序、二分查找等实用算法，这些算法实现主要封装于集合框架的工具类 [Collections](https://docs.oracle.com/javase/9/docs/api/java/util/Collections.html) 中。

### 8.1. 排序

Collections.sort 方法可以对实现了 List 接口的集合进行排序：

- `static <T extends Comparable<? super T>> void	sort​(List<T> list)`

  该方法假定集合的元素都实现了 Comparable 接口。

- `static <T> void	sort​(List<T> list, Comparator<? super T> c)`

  通过传入 Comparator 对象，可自定义排序的规则。

列表包含了数组结构和链表结构，其中对数组的排序算法基本上都是基于随机访问的，然而对链表排序时若采用基于随机访问的排序算法，效率会极低。因此，Java 集合框架中的排序方法的实现是直接将所有元素转入一个数组，然后对数组进行排序，最后再将排序后的序列复制回列表。

### 8.2. 混排

Collections.shuffle 方法的功能与 sort 正好相反，该方法会随机地混排列表中元素的顺序。

- `static void	shuffle​(List<?> list)`

- `static void	shuffle​(List<?> list, Random rnd)`

类似 sort 方法，若所给列表没有实现 RandomAccess 接口，该方法会将元素复制到数组中，然后打乱数组中元素的顺序，最后再将打乱顺序后的元素复制回列表。

### 8.3. 搜索

Collections.binarySearch​ 方法实现了二分查找算法。

- `static <T> int	binarySearch​(List<? extends Comparable<? super T>> list, T key)`

- `static <T> int	binarySearch​(List<? extends T> list, T key, Comparator<? super T> c)`

NOTE：
- 调用该方法时，集合必须是已排好序的，否则该方法会返回错误的答案。
- 返回值
  - 若调用返回数值非负数，表示匹配到的对象的索引；
  - 若返回负数，表示没有匹配到的元素，但可以利用返回值计算应该将 element 插入到集合的那个位置，以保持集合的有序性。若返回值为 i，则应插入的位置是 (-i - 1)。
- 只有采用随机访问时，二分查找才有意义。因此，若对链表结构调用 binarySearch​方法，它将自动变成线性查找。

### 8.4. 其它

Collections 中还包含了许多简单算法的封装实现，使用这些方法，**加快开发者开发效率的同时，更可提高代码可读性**。

- 返回集合中最大或最小的元素
  - `static <T extends Object & Comparable<? super T>> T	max​(Collection<? extends T> coll)`
  - `static <T> T	max​(Collection<? extends T> coll, Comparator<? super T> comp)`
  - `static <T extends Object & Comparable<? super T>> T	min​(Collection<? extends T> coll)`
  - `static <T> T	min​(Collection<? extends T> coll, Comparator<? super T> comp)`
- 将原列表的所有元素赋值到目标列表的相应位置上。新列表的长度至少应与原列表长度一样。
  - `static <T> void	copy​(List<? super T> dest, List<? extends T> src)`
- 将列表中所有位置设置所给值
  - `static <T> void	fill​(List<? super T> list, T obj)`
- 将所给所有值添加到集合中
  - `static <T> boolean	addAll​(Collection<? super T> c, T... elements)`
- 将集合中的所有匹配的元素的值替换为 newValue
  - `static <T> boolean	replaceAll​(List<T> list, T oldVal, T newVal)`
- 交互集合中指定的两个元素
  - `static void	swap​(List<?> list, int i, int j)`
- 逆置列表中元素的顺序，复杂度为 O(n)
  - `static void	reverse​(List<?> list)`
- 返回集合中与对象 o 相同的元素的个数
  - `static int	frequency​(Collection<?> c, Object o)`
- 若两个集合没有任何相同的元素，返回 true
  - `static boolean	disjoint​(Collection<?> c1, Collection<?> c2)`

## 9. Refer Links

[java 集合系列](http://blog.csdn.net/u010648555/article/details/56049460)