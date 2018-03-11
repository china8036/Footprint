- [Java 集合：Collection 族](#java-%E9%9B%86%E5%90%88%EF%BC%9Acollection-%E6%97%8F)
  - [1. Collection 接口](#1-collection-%E6%8E%A5%E5%8F%A3)
    - [1.1. 基本 API](#11-%E5%9F%BA%E6%9C%AC-api)
    - [1.2. 遍历 Collection](#12-%E9%81%8D%E5%8E%86-collection)
  - [2. List 接口](#2-list-%E6%8E%A5%E5%8F%A3)
    - [2.1. ListIterator](#21-listiterator)
    - [2.2. LinkedList](#22-linkedlist)
      - [2.2.1. 构造器](#221-%E6%9E%84%E9%80%A0%E5%99%A8)
      - [2.2.2. 添加元素](#222-%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
      - [2.2.3. 访问元素](#223-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
    - [2.3. ArrayList](#23-arraylist)
      - [2.3.1. 构造器](#231-%E6%9E%84%E9%80%A0%E5%99%A8)
      - [2.3.2. 添加 & 删除元素](#232-%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
      - [2.3.3. 访问元素](#233-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
      - [2.3.4. 转换为数组](#234-%E8%BD%AC%E6%8D%A2%E4%B8%BA%E6%95%B0%E7%BB%84)
      - [2.3.5. 其它](#235-%E5%85%B6%E5%AE%83)
  - [3. Set 接口](#3-set-%E6%8E%A5%E5%8F%A3)
    - [3.1. HashSet](#31-hashset)
      - [3.1.1. 构造器](#311-%E6%9E%84%E9%80%A0%E5%99%A8)
      - [3.1.2. 添加 & 删除元素](#312-%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
      - [3.1.3. 访问元素](#313-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
      - [3.1.4. 其它](#314-%E5%85%B6%E5%AE%83)
    - [3.2. SortedSet 接口 & TreeSet 类](#32-sortedset-%E6%8E%A5%E5%8F%A3-treeset-%E7%B1%BB)
  - [4. Queue 接口](#4-queue-%E6%8E%A5%E5%8F%A3)
    - [4.1. PriorityQueue](#41-priorityqueue)
    - [4.2. Deque 接口](#42-deque-%E6%8E%A5%E5%8F%A3)
    - [4.3. ArrayDeque](#43-arraydeque)
    - [4.4. LinkedList](#44-linkedlist)
  - [5. Refer Links](#5-refer-links)
  
# Java 集合：Collection 族

## 1. Collection 接口

Java 类库中，集合类最基本的接口是 [Collection](https://docs.oracle.com/javase/9/docs/api/java/util/Collection.html) 接口，用于存储相同类型的元素。

### 1.1. 基本 API

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

### 1.2. 遍历 Collection

Collection 集合元素被访问的顺序取决于集合类型。若对 List 进行迭代，则迭代器将从索引 0 处开始，每迭代一次索引值加 1；若对 Set 进行迭代，虽能确定最终结果能遍历所有元素，但无法预知元素被访问的顺序。

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
    NOTE: TODO:
    
    Iterator 迭代器采用 **fail-fast 机制**，一旦在迭代过程中检测到该集合被修改，程序会立即抛出 java.util.ConcurrentModificationException 异常，而不是显示修改后的结果，从而避免共享资源而引发的线程安全问题。

    因此，当使用 Iterator 迭代器迭代访问集合元素时，除了可以通过 Iterator 接口的 remove() 方法删除上一次 next() 方法返回的集合元素，不能通过其它任何方式改变集合中的元素，否则将会引发 ConcurrentModificationException 异常。

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

  - 使用 Iterator 接口的 forEachRemaining​() 方法遍历集合
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

- 通过 Java 8 提供的 Predicate 接口遍历集合 TODO:

- 通过 Java 8 提供的 Stream 接口遍历集合 TODO:

## 2. List 接口

[List](https://docs.oracle.com/javase/9/docs/api/java/util/List.html) 接口是 Collection 接口的扩展，是一个有序集合，元素会添加到容器中的特定位置。

- 优点：
  - 元素按顺序排列

- 缺点：
  - 搜索时需要遍历元素，查找速度较慢

- 可以采用两种方式访问 List 元素：
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

- core Java 10th
  > 集合框架的这个方面设计得很不好。实际中有两种有序集合，其性能有着很大差异。数组适合随机访问，但链表适合迭代器顺序访问。若能提供两个接口就会容易一些了。

- 可通过工具类 RandomAccess 来判断一个集合是否支持高效的随机访问：
  ```java
  if (c instanceof RandomAccess) {
    // 支持 
  } else {
    // 不支持
  }
  ```

### 2.1. ListIterator

[ListIterator](https://docs.oracle.com/javase/9/docs/api/java/util/ListIterator.html) 接口是 Iterator 的子接口，是用于顺序访问的迭代器，主要用在链表结构中。

- `void	add​(E e)`：在迭代器前添加元素。

- `boolean	hasPrevious​()`

- `E	previous​()`：返回越过的对象。

  NOTE：调用 next 后再调用 remove 方法，会删除迭代器左侧的元素，但调用 previous 后再调用 remove 方法，会删除迭代器右侧的元素，并且同样不能连续调用两次 remove。

- `int	previousIndex​()`：返回上一个元素的索引。

- `int	nextIndex​()`：返回下一个元素的索引。

- `void	set​(E e)`

### 2.2. LinkedList

在 Java 中，[LinkedList](https://docs.oracle.com/javase/9/docs/api/java/util/LinkedList.html) 是链表的数据结构封装，而且在 Java 中所有的链表都是双向链表。

- 优点
  - 便于在集合中间位置插入元素
- 缺点
  - 访问每个元素只能从头搜索，随机访问效率极低

#### 2.2.1. 构造器

- `LinkedList​()`

  构造一个空链表。

- `LinkedList​(Collection<? extends E> c)`

  构造一个链表，并将所给集合中的所有元素添加到这个链表中。

#### 2.2.2. 添加元素

LinkedList 提供了	`add​(E e)` 方法将对象添加到链表的尾部，但对于链表中间位置的元素添加，需要使用迭代器 ListIterator 中的 add 方法。

- `ListIterator<E>	listIterator​(int index)`：返回一个 ListIterator 迭代器。

P.S. 并非所有的集合都需要使用迭代器来插入中间位置的元素，比如无序的 Set 结构，其中元素完全无序。因此，Java 集合框架的设计者不在 Iterator 接口中定义 add 方法，而是在 Iterator 的子接口 ListIterator 中才定义了 add 方法，更具实际意义。

#### 2.2.3. 访问元素

LinkedList 提供了一个用于访问元素的 get​(int index) 方法，但该方法的实现效率很低。

虽然 get 方法做了优化（索引大于 size/2 时，从链表尾部开始搜索元素），但是，若要访问第 n 个元素，就需要从链表头部开始，越过 n-1 个元素，没有任何捷径可走，且 LinkedList 对象访问元素时不做任何缓存位置信息的操作。

因此，若需要使用该方法访问元素，应考虑是否更换所用的数据结构。

core java 10th
> 我们建议避免使用以整数索引表示链表中位置的所有方法。若需要对集合进行随机访问，就使用数组或 ArrayList，不要使用链表。

### 2.3. ArrayList

Java 中允许创建数组的长度为变量，即允许在运行时动态确定数组的大小，但这并没有解决运行时动态改变数组大小的问题。一旦确定了数组的大小，再改变它就不太容易了（比如通过 Arrays.copyOf 进行扩容）。因此，Java 提供了 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html) 类。

ArrayList 是一个采用类型参数的泛型类，在添加或删除数组元素时，它具有自动调节数组容量的功能。

- 优点
  - 随机访问元素效率高
- 缺点
  - 在集合中间位置插入元素时需要移动该元素后的所有元素，效率低

#### 2.3.1. 构造器

ArrayList 由三种构造方式：

- `ArrayList​()`：构造一个空的数组列表。
  ```java
  ArrayList<Employee> staff = new ArrayList<>(); 
  ```

- `ArrayList​(int initialCapacity)`：构造一个具有初始容量的数组列表。
  ```java
  ArrayList<Employee> staff = new ArrayList<>(100); 
  ```
  NOTE：数组列表的容量与普通数组的大小是有区别的。
  - 普通数组的大小，指的是数组初始化时已分配了 100 个元素的存储空间可供使用；
  - 数组列表的容量，指的是该数组列表拥有保存 100 个元素的潜力（实际上还可以通过重新分配空间从而超过 100 个），但是在完成初始化时，数组列表根本不含有任何元素空间。

- `ArrayList​(Collection<? extends E> c)`：以集合元素作为初始值构造一个数组列表。

NOTE: 尖括号内不可使用基本数据类型。

#### 2.3.2. 添加 & 删除元素

- `void	add​(E element)`：可为数组列表创建一块新的存储空间并添加元素。若调用 add 时内部数组已满，数组列表将自动创建一个更大的数组，并将所有的对象从较小的数组中拷贝到一个较大的数组中。

- `void	add​(int index, E element)`：在数组指定位置插入元素。为插入该元素，位于 index 之后的所有元素都要向后移动一个位置，若插入的元素导致数组列表大小超过了容量，数组列表会被重新分配存储空间，在原来基础上增大一半。

- `E	remove​(int index)`：删除数组列表中指定位置的元素。位于 index 之后的元素都向前移动一个位置，并且数组大小减一。

- `boolean	remove​(Object o)`：删除数组列表中值为 o 的元素。

NOTE：

对数组进行插入或删除元素的操作效率都比较低。对小型数组而言，这部分效率的损失还不必担心；但对于大型数组，频繁的在数组的中间位置插入、删除元素，将耗费大量时间成本在元素的移动上，事实上这种情况下应该使用链表作为数据结构存储。

#### 2.3.3. 访问元素

数组列表的自动扩展容量带来了便利，但也增加了访问元素时语法的复杂性。因为 ArrayList 不是 Java 程序设计语言的一部分，只是由其他人编写并包含在标准库中的实用工具类，而 Java 中不支持重载运算符的特性，因此在 ArrayList 中访问元素无法使用普通数组的`[]`格式，而需要使用 get 和 set 方法进行访问。

`E	get​(int index)`：获取数组列表指定位置的元素。

`E	set​(int index, E element)`: 设置数组列表指定位置的元素的值。该方法不会创建新的存储空间，因此不可用于添加新元素。

#### 2.3.4. 转换为数组

`Object[]	toArray​()`：使用该方法将数组列表中的数组元素拷贝到一个数组中。

#### 2.3.5. 其它

`int	size​()`：返回数组列表的元素个数。

`void	ensureCapacity​(int minCapacity)`：若能够知道数组可能存储的元素数量，可在填充数组之前调用该方法。该方法会分配一个包含 minCapacity 个对象的内部数组，再调用 minCapacity 次 add 方法，而不用重新分配空间。

`void	trimToSize​()`：当确认数组列表的大小将不再发生变化，可以调用该方法，将存储区域的大小调整为当前元素数量所需要的存储空间数目，多余的空间由垃圾回收器回收。需要注意的是，一旦调用该方法整理数组列表的大小后，添加新元素就需要耗费时间再次移动存储快，因此应该在确认不会再添加任何元素时，才调用该方法。

## 3. Set 接口

[Set](https://docs.oracle.com/javase/9/docs/api/java/util/Set.html) 接口是 Collection 接口的扩展，是一种无序、元素不可以重复的集合。

- 优点
  - 快速查找元素，利于搜索
- 缺点
  - 无法控制元素出现的次序

- Set 结构提供的方法的行为相比 Collection 接口有更严谨的定义：
  - `boolean	add​(E e)`

    添加元素时不允许添加重复的元素。

  - `boolean	equals​(Object o)`
    
    只要两个集合包含的元素相同就返回 true，不要求有相同的顺序。

  - `int	hashCode​()`

    要保证包含相同元素的两个集合会得到相同的散列码。

### 3.1. HashSet

Java 集合类库中提供了 [HashSet](https://docs.oracle.com/javase/9/docs/api/java/util/HashSet.html) 类，它实现了基于散列表实现的集合。

在 Java 中，散列表使用链表数组实现，每个列表称为桶。要想查找表中元素的位置，需要先计算其散列码，然后与桶的总数取余，所得到的结果就是保存这个元素的桶的索引。

例，某个对象散列码为 76268，且有 128 个桶，则对象应保存在第 108 号桶中（76268%128=108）。当桶被占满（散列冲突）时，需要用新对象与桶中的所有对象进行比较，查看这个对象是否已存在。

P.S. 在 Java 8 中，桶满时会从链表变成平衡二叉树。

桶数是指用于手机具有相同散列值的桶的数目，若大致知道最终有多少个元素要插入散列表中，就可以设置桶数。通常会将桶数设置为预计元素个数的 75%~150%。为防止键的聚集，最好将桶数设置素数。HashSet 中使用的桶数是 2 的幂，默认值为 16（为表大小提供的任何值都将被自动转换为 2 的下一个幂）。

当表太满时，就需要对散列表进行再散列。装填因子决定何时对表进行再散列，例如装填因子为 0.75 时，若表中超过 75% 的位置已填入元素，这个表就会以双倍的桶数自动进行再散列。对于大多数应用，装填因子设置为 0.75 是比较合理的。

散列将元素分散在散列表的各个位置上，若元素的散列码发生了变化，则元素在数据结构中的位置也会发生变化。

#### 3.1.1. 构造器

- `HashSet​()`

  构造一个空的散列表。

- `HashSet​(Collection<? extends E> c)`

  构造一个散列集，并将所给集合中的所有集合添加到该散列集中。

- `HashSet​(int initialCapacity)`

  构造一个空的具有指定容量 / 桶数的散列表。

- `HashSet​(int initialCapacity, float loadFactor)`

  构造一个空的具有指定容量 / 桶数和装填因子的散列表。

#### 3.1.2. 添加 & 删除元素

- `boolean	add​(E e)`

- `boolean	remove​(Object o)`

#### 3.1.3. 访问元素

散列表迭代器将依次访问所有的桶，但由于散列将元素分散在表中的各个位置上，因此在散列表中访问元素的顺序是完全随机的。

```java
Set<String> words = new HashSet<>();
//...
Iterator iter = words.iterator();
for (int i =1; i <= 20 && iter.hasNext(); i ++) {
  System.out.println(iter.next());
}
```

#### 3.1.4. 其它

- `boolean	contains​(Object o)`：用于快速查看某个元素是否已经出现在散列集中。该方法不会查看集合中的所有元素，而只会在某个桶中查找元素。

### 3.2. SortedSet 接口 & TreeSet 类

树集 [TreeSet](https://docs.oracle.com/javase/9/docs/api/java/util/TreeSet.html) 与散列表相似，但有所改进。它是一个有序的集合，可以以任意顺序将元素插入到集合中，但在对集合进行遍历时，每个值将自动按照排序后的顺序呈现。

TreeSet 采用红黑树实现，每次将一个元素添加到树中，都将被放置在正确排序的位置上。因此，迭代器总是以排好序的顺序访问各个元素。若树中包含 n 个元素，则查找新元素位置的操作需要 log2(n) 次比较。因此，将一个元素添加到树中要比添加到散列集中慢，但相比检查数组或链表中的重复元素还是快很多。

树集中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

**若不需要对集合的元素进行排序，那么应该选择散列集作为存储结构，可避免用于排序的开销。**

## 4. Queue 接口

队列 [Queue](https://docs.oracle.com/javase/9/docs/api/java/util/Queue.html) 接口是 Collection 接口的扩展，它支持有效地在集合的尾部添加元素、在头部删除元素，但不支持在集合的中间位置添加元素。

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

### 4.1. PriorityQueue

优先级队列 [PriorityQueue](https://docs.oracle.com/javase/9/docs/api/java/util/PriorityQueue.html) 是 Queue 接口的实现类，队列中的元素可以按照任意的顺序插入，却总是按照排序的顺序进行检索。

优先级队列使用堆作为数据结构实现，因此不需要对所有的元素进行排序。每次调用 remove 方法，都会删除当前优先级队列中最小的元素；每次调用 add 方法，都会将最小的元素移动到根。

同 TreeSet 一样，优先级队列中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

优先级队列的典型应用场景是任务的调度。任务以随机顺序添加到任务队列中，但每一次进行任务调度时，都会将一个优先级最高的元素从队列中删除并开始执行（可设置优先级为 1 时最高）。

- 构造器
  - `PriorityQueue​()`
  - `PriorityQueue​(int initialCapacity)`	
  - `PriorityQueue​(int initialCapacity, Comparator<? super E> comparator)`

### 4.2. Deque 接口

双端队列（double ended queue） [Deque](https://docs.oracle.com/javase/9/docs/api/java/util/Deque.html) 接口是 Queue 接口的扩展，支持有效地在头部和尾部同时添加或删除元素，但同样不支持在集合的中间位置添加元素。**它既可以当作栈使用，也可以当作队列使用。**

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

### 4.3. ArrayDeque

[ArrayDeque](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayDeque.html) 依赖于可变数组实现，作为栈来使用时，效率要高于 Stack；作为队列使用时，效率相较于基于双向链表的 LinkedList 也要更好一些。

当需要使用栈时，Java 已不推荐使用 Stack，而是推荐使用更高效的 ArrayDeque。

ArrayDeque 不支持为 null 的元素。

- 构造器
  - `ArrayDeque​()`
  - `ArrayDeque​(int numElements)`：用给定的初始容量构造一个无限双端队列。

### 4.4. LinkedList
 
## 5. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)