- [Java 集合：Collection 族 - List](#java-%E9%9B%86%E5%90%88%EF%BC%9Acollection-%E6%97%8F---list)
  - [1. List 接口](#1-list-%E6%8E%A5%E5%8F%A3)
  - [2. ListIterator](#2-listiterator)
  - [3. LinkedList](#3-linkedlist)
    - [3.1. 构造器](#31-%E6%9E%84%E9%80%A0%E5%99%A8)
    - [3.2. 添加元素](#32-%E6%B7%BB%E5%8A%A0%E5%85%83%E7%B4%A0)
    - [3.3. 访问元素](#33-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
  - [4. ArrayList](#4-arraylist)
    - [4.1. 构造器](#41-%E6%9E%84%E9%80%A0%E5%99%A8)
    - [4.2. 添加 & 删除元素](#42-%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
    - [4.3. 访问元素](#43-%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
    - [4.4. 转换为数组](#44-%E8%BD%AC%E6%8D%A2%E4%B8%BA%E6%95%B0%E7%BB%84)
    - [4.5. 其它](#45-%E5%85%B6%E5%AE%83)
  - [5. Refer Links](#5-refer-links)
  
# Java 集合：Collection 族 - List

## 1. List 接口

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

## 2. ListIterator

[ListIterator](https://docs.oracle.com/javase/9/docs/api/java/util/ListIterator.html) 接口是 Iterator 的子接口，是用于顺序访问的迭代器，主要用在链表结构中。

- `void	add​(E e)`：在迭代器前添加元素。

- `boolean	hasPrevious​()`

- `E	previous​()`：返回越过的对象。

  NOTE：调用 next 后再调用 remove 方法，会删除迭代器左侧的元素，但调用 previous 后再调用 remove 方法，会删除迭代器右侧的元素，并且同样不能连续调用两次 remove。

- `int	previousIndex​()`：返回上一个元素的索引。

- `int	nextIndex​()`：返回下一个元素的索引。

- `void	set​(E e)`

## 3. LinkedList

在 Java 中，[LinkedList](https://docs.oracle.com/javase/9/docs/api/java/util/LinkedList.html) 是链表的数据结构封装，而且在 Java 中所有的链表都是双向链表。

- 优点
  - 便于在集合中间位置插入元素
- 缺点
  - 访问每个元素只能从头搜索，随机访问效率极低

### 3.1. 构造器

- `LinkedList​()`

  构造一个空链表。

- `LinkedList​(Collection<? extends E> c)`

  构造一个链表，并将所给集合中的所有元素添加到这个链表中。

### 3.2. 添加元素

LinkedList 提供了	`add​(E e)` 方法将对象添加到链表的尾部，但对于链表中间位置的元素添加，需要使用迭代器 ListIterator 中的 add 方法。

- `ListIterator<E>	listIterator​(int index)`：返回一个 ListIterator 迭代器。

P.S. 并非所有的集合都需要使用迭代器来插入中间位置的元素，比如无序的 Set 结构，其中元素完全无序。因此，Java 集合框架的设计者不在 Iterator 接口中定义 add 方法，而是在 Iterator 的子接口 ListIterator 中才定义了 add 方法，更具实际意义。

### 3.3. 访问元素

LinkedList 提供了一个用于访问元素的 get​(int index) 方法，但该方法的实现效率很低。

虽然 get 方法做了优化（索引大于 size/2 时，从链表尾部开始搜索元素），但是，若要访问第 n 个元素，就需要从链表头部开始，越过 n-1 个元素，没有任何捷径可走，且 LinkedList 对象访问元素时不做任何缓存位置信息的操作。

因此，若需要使用该方法访问元素，应考虑是否更换所用的数据结构。

core java 10th
> 我们建议避免使用以整数索引表示链表中位置的所有方法。若需要对集合进行随机访问，就使用数组或 ArrayList，不要使用链表。

## 4. ArrayList

Java 中允许创建数组的长度为变量，即允许在运行时动态确定数组的大小，但这并没有解决运行时动态改变数组大小的问题。一旦确定了数组的大小，再改变它就不太容易了（比如通过 Arrays.copyOf 进行扩容）。因此，Java 提供了 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html) 类。

ArrayList 是一个采用类型参数的泛型类，在添加或删除数组元素时，它具有自动调节数组容量的功能。

- 优点
  - 随机访问元素效率高
- 缺点
  - 在集合中间位置插入元素时需要移动该元素后的所有元素，效率低

### 4.1. 构造器

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

### 4.2. 添加 & 删除元素

- `void	add​(E element)`：可为数组列表创建一块新的存储空间并添加元素。若调用 add 时内部数组已满，数组列表将自动创建一个更大的数组，并将所有的对象从较小的数组中拷贝到一个较大的数组中。

- `void	add​(int index, E element)`：在数组指定位置插入元素。为插入该元素，位于 index 之后的所有元素都要向后移动一个位置，若插入的元素导致数组列表大小超过了容量，数组列表会被重新分配存储空间，在原来基础上增大一半。

- `E	remove​(int index)`：删除数组列表中指定位置的元素。位于 index 之后的元素都向前移动一个位置，并且数组大小减一。

- `boolean	remove​(Object o)`：删除数组列表中值为 o 的元素。

NOTE：

对数组进行插入或删除元素的操作效率都比较低。对小型数组而言，这部分效率的损失还不必担心；但对于大型数组，频繁的在数组的中间位置插入、删除元素，将耗费大量时间成本在元素的移动上，事实上这种情况下应该使用链表作为数据结构存储。

### 4.3. 访问元素

数组列表的自动扩展容量带来了便利，但也增加了访问元素时语法的复杂性。因为 ArrayList 不是 Java 程序设计语言的一部分，只是由其他人编写并包含在标准库中的实用工具类，而 Java 中不支持重载运算符的特性，因此在 ArrayList 中访问元素无法使用普通数组的`[]`格式，而需要使用 get 和 set 方法进行访问。

`E	get​(int index)`：获取数组列表指定位置的元素。

`E	set​(int index, E element)`: 设置数组列表指定位置的元素的值。该方法不会创建新的存储空间，因此不可用于添加新元素。

### 4.4. 转换为数组

`Object[]	toArray​()`：使用该方法将数组列表中的数组元素拷贝到一个数组中。

### 4.5. 其它

`int	size​()`：返回数组列表的元素个数。

`void	ensureCapacity​(int minCapacity)`：若能够知道数组可能存储的元素数量，可在填充数组之前调用该方法。该方法会分配一个包含 minCapacity 个对象的内部数组，再调用 minCapacity 次 add 方法，而不用重新分配空间。

`void	trimToSize​()`：当确认数组列表的大小将不再发生变化，可以调用该方法，将存储区域的大小调整为当前元素数量所需要的存储空间数目，多余的空间由垃圾回收器回收。需要注意的是，一旦调用该方法整理数组列表的大小后，添加新元素就需要耗费时间再次移动存储快，因此应该在确认不会再添加任何元素时，才调用该方法。

## 5. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)