- [Java 集合：Collection 族 - Collection 接口](#java-集合collection-族---collection-接口)
  - [1. Iterable 接口](#1-iterable-接口)
    - [1.1. forEach() 方法](#11-foreach-方法)
    - [1.2. Iterator 接口](#12-iterator-接口)
  - [2. Collection 接口](#2-collection-接口)
  - [6. Refer Links](#6-refer-links)
  
# Java 集合：Collection 族 - Collection 接口

## 1. Iterable 接口

[Iterable](https://docs.oracle.com/javase/9/docs/api/java/lang/Iterable.html) 接口用于表示可被迭代的对象，该接口只包含了 1 个成员变量和 2 个成员方法：
- `Iterator<T> iterator()`
- `default void forEach(Consumer<? super T> action) {}`
- `default Spliterator<T> spliterator() {}`

### 1.1. forEach() 方法

`forEach(Consumer<? super T> action)`方法是 Java 8 为 Iterable 接口新增的默认方法，由于 Collection 接口扩展了 Iterable 接口，因此任何 Collection 集合都可以直接调用该方法。

该方法所需参数的类型是一个函数式接口 Consumer，可以使用 Lambda 表达式。当调用该方法时，程序会依次将集合元素传给 Consumer 的 accept() 方法。

### 1.2. Iterator 接口

[Iterator](https://docs.oracle.com/javase/9/docs/api/java/util/Iterator.html) 接口也是 Java 集合框架的成员，Iterator 对象也称为迭代器，用于对象的迭代。

Iterator 接口实际上是 Java 早期 Enumeration 接口的“升级版”。

- 基本 API

  - `boolean hasNext()`

    判断集合的“光标”当前所在位置后是否还有元素。

  - `E next()`

    Java 迭代器可以认为位于两个元素之间，每次调用 next 方法时，会将集合的“光标”向前移动一个位置，并返回移动时越过的元素的引用。**若到达集合的末尾，将抛出一个异常。**因此，一般在调用 next 方法之前需要使用 hasNext 进行判断。

  - `default void remove​()`

    删除上次调用 next 方法时返回的元素。

    **next 方法与 remove 方法的调用有相互依赖性。若调用 remove 之前没有调用 next 将会抛出一个异常。**

  - `default void forEachRemaining(Consumer<? super E> action)`

    该方法从 Java SE 8 开始加入，可通过为该方法提供一个 Lambda 表达式，对迭代器的每一个元素调用该 Lambda 表达式进行处理。

## 2. Collection 接口

Java 类库中，集合类最基本的接口是 [Collection](https://docs.oracle.com/javase/9/docs/api/java/util/Collection.html) 接口，用于存储相同类型的元素。

```java
public interface Collection<E> extends Iterable<E> {/* ... */}
```

基本 API
- `int size​()`

  返回当前存储在集合中的元素个数。

- `boolean isEmpty​()`

  判断集合是否为空。

- `boolean contains​(Object o)`

  判断集合是否包含一个与对象 o 相等的元素。

- `boolean add​(E e)`
  
  向集合中添加元素。若操作改变了集合则返回 true，若集合没有发生变化则返回 false。

- `Iterator<E> iterator()`

  返回一个实现了 Iterator 接口的对象，通过此迭代器对象可以依次访问集合中的元素。

- `boolean remove(Object o)`

  删除集合中等于对象 o 的对象。若该方法使集合发生了改变则返回 true。

- `default boolean removeIf(Predicate<? super E> filter)`

  批量删除 filter 返回 true 的所有元素。若该方法使集合发生了改变则返回 true。

  该方法是 Java 8 新增的函数式方法，参数类型是函数式接口 [Predicate](https://docs.oracle.com/javase/9/docs/api/java/util/function/Predicate.html)，因此可以使用 Lambda 表达式作为参数。

  例：
  ```java
  books.removeIf(element -> element.length() < 10);
  ```

- `boolean retainAll(Collection<?> c)`

  删除集合中所有与 other 集合不同的元素。若该方法使集合发生了改变则返回 true。

- `Object[] toArray()`

  返回集合的对象数组，并填入 arrayToFill 中。若 arrayToFill 足够大，则剩余空间补 null；否则分配一个新的数组，其长度等于集合的大小，并重新填充集合元素。

- `default Stream<E> stream()`

  该方法是 Java 8 新增的方法，返回 Collection 集合对应的流对象 [Stream](https://docs.oracle.com/javase/9/docs/api/java/util/stream/Stream.html)，从而可通过该流对象使用流式 API 来操作集合元素。

  例：
  ```java
  // 统计书籍集合中书名包含“Java”子串的书的数量
  System.out.println(books.stream().filter(element -> element.contains("Java")).count());
  ```

## 6. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)
