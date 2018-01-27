- [Java 集合](#java-%E9%9B%86%E5%90%88)
  - [ArrayList](#arraylist)
    - [构造器](#%E6%9E%84%E9%80%A0%E5%99%A8)
    - [添加 & 删除元素](#%E6%B7%BB%E5%8A%A0-%E5%88%A0%E9%99%A4%E5%85%83%E7%B4%A0)
    - [访问元素](#%E8%AE%BF%E9%97%AE%E5%85%83%E7%B4%A0)
    - [转换为数组](#%E8%BD%AC%E6%8D%A2%E4%B8%BA%E6%95%B0%E7%BB%84)
    - [其它](#%E5%85%B6%E5%AE%83)
  - [Refer Links](#refer-links)

# Java 集合

![image](/resources/images/java-collections.png)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/26/f9992918707e6ff6ea21ed0fd212b2ce.jpg)

## ArrayList

Java 中允许创建数组的长度为变量，即允许在运行时动态确定数组的大小，但这并没有解决运行时动态改变数组大小的问题。一旦确定了数组的大小，再改变它就不太容易了（比如通过 Arrays.copyOf 进行扩容）。因此，Java 提供了 [ArrayList](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayList.html) 类。

ArrayList 是一个采用类型参数的泛型类，在添加或删除数组元素时，它具有自动调节数组容量的功能。

### 构造器

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

### 添加 & 删除元素

`void	add​(E element)`：可为数组列表创建一块新的存储空间并添加元素。若调用 add 时内部数组已满，数组列表将自动创建一个更大的数组，并将所有的对象从较小的数组中拷贝到一个较大的数组中。

`void	add​(int index, E element)`：在数组指定位置插入元素。为插入该元素，位于 index 之后的所有元素都要向后移动一个位置，若插入的元素导致数组列表大小超过了容量，数组列表会被重新分配存储空间，在原来基础上增大一半。

`E	remove​(int index)`：删除数组列表中指定位置的元素。位于 index 之后的元素都向前移动一个位置，并且数组大小减一。

`boolean	remove​(Object o)`：删除数组列表中值为 o 的元素。

NOTE：

对数组进行插入或删除元素的操作效率都比较低。对小型数组而言，这部分效率的损失还不必担心；但对于大型数组，频繁的在数组的中间位置插入、删除元素，将耗费大量时间成本在元素的移动上，事实上这种情况下应该使用链表作为数据结构存储。

### 访问元素

数组列表的自动扩展容量带来了便利，但也增加了访问元素时语法的复杂性。因为 ArrayList 不是 Java 程序设计语言的一部分，只是由其他人编写并包含在标准库中的实用工具类，而 Java 中不支持重载运算符的特性，因此在 ArrayList 中访问元素无法使用普通数组的`[]`格式，而需要使用 get 和 set 方法进行访问。

`E	get​(int index)`：获取数组列表指定位置的元素。

`E	set​(int index, E element)`: 设置数组列表指定位置的元素的值。该方法不会创建新的存储空间，因此不可用于添加新元素。

### 转换为数组

`Object[]	toArray​()`：使用该方法将数组列表中的数组元素拷贝到一个数组中。

### 其它

`int	size​()`：返回数组列表的元素个数。

`void	ensureCapacity​(int minCapacity)`：若能够知道数组可能存储的元素数量，可在填充数组之前调用该方法。该方法会分配一个包含 minCapacity 个对象的内部数组，再调用 minCapacity 次 add 方法，而不用重新分配空间。

`void	trimToSize​()`：当确认数组列表的大小将不再发生变化，可以调用该方法，将存储区域的大小调整为当前元素数量所需要的存储空间数目，多余的空间由垃圾回收器回收。需要注意的是，一旦调用该方法整理数组列表的大小后，添加新元素就需要耗费时间再次移动存储快，因此应该在确认不会再添加任何元素时，才调用该方法。

## Refer Links

[java 集合系列](http://blog.csdn.net/u010648555/article/details/56049460)