- [Java Lambda 基本概念](#java-lambda-基本概念)
  - [1. 函数式编程](#1-函数式编程)
  - [2. Lambda 表达式](#2-lambda-表达式)
    - [2.1. 基本概念](#21-基本概念)
    - [2.2. 基本语法](#22-基本语法)
    - [2.3. 方法引用](#23-方法引用)
    - [2.4. 构造方法引用](#24-构造方法引用)
  - [3. Refer Links](#3-refer-links)

# Java Lambda 基本概念

## 1. 函数式编程

在面向对象编程之前还出现了面向函数的编程（函数式编程) ，以前一直被忽略、不被重视，现在从学术界已经走向了商业界，对函数编程语言的支持目前有 Scala、Erlang、F#、Python、Php、Java、Javascript 等。

## 2. Lambda 表达式

### 2.1. 基本概念

为什么需要 Lambda 表达式？

- 使用 Lambda 表达式可以使代码变的更加紧凑。
  ```java
  public static void main(String[] args) throws Exception {
    new Thread(() -> System.out.println("Hello World!")).start();
    TimeUnit.SECONDS.sleep(1000);
  }
  ```
- 使得函数中可以接受以函数为单元的参数，在 C/C++ 中通过函数指针来实现，而在 Java 中就是 Lambda 表达式。
  ```java
  public static void main(String[] args) {
      String []datas = new String[] {"peng","zhao","li"};
      Arrays.sort(datas,(v1 , v2) -> Integer.compare(v1.length(), v2.length()));
      Stream.of(datas).forEach(param -> System.out.println(param));
  }
  ```

### 2.2. 基本语法

Lambda 表达式的形式化表示如下：
```
Parameters -> Expression 
```

- 语句块

  如果 Lambda 表达式中要执行多个语句块，需要将多个语句块以{}进行包装，如果有返回值，需要显示指定 return 语句：
  ```
  Parameters -> {expressions;};
  ```

- 参数

  如果 Lambda 表达式不需要参数，可以使用一个空括号表示：
  ```
  () -> {for (int i = 0; i < 1000; i++) doSomething();};
  ```

  如果 Lambda 表达式需要参数，则需要显式指定参数。由于 Java 是一个强类型的语言，因此参数必须要有类型：
  ```java
  String []datas = new String[] {"peng","zhao","li"};
  Arrays.sort(datas,(String v1, String v2) -> Integer.compare(v1.length(), v2.length()));
  ```

  但如果编译器能够推测出 Lambda 表达式的参数类型，则不需要我们显示的进行指定：
  ```java
  // 编译器会根据 Lambda 表达式对应的函数式接口 Comparator<String>进行自动推断
  String []datas = new String[] {"peng","zhao","li"};;
  Arrays.sort(datas,(v1, v2) -> Integer.compare(v1.length(), v2.length()));
  ```

  如果 Lambda 表达式只有一个参数，并且参数的类型是可以由编译器推断出来的，则可以省略参数的类型及括号：
  ```java
  Stream.of(datas).forEach(param -> {System.out.println(param.length());});
  ```

  除此之外，Lambda 表达式的参数可以使用修饰符及注解，如 final、@NonNull 等。

- 返回值

  Lambda 表达式的返回类型，无需指定，编译器会自行推断。

### 2.3. 方法引用

有时，我们需要执行的代码在某些类中已经存在，这时我们没必要再去写 Lambda 表达式，可以直接使用该方法，这种情况我们称之为方法引用。

eg:
```java
Stream.of(datas).forEach(param -> {System.out.println(param);});
// 使用方法引用
Stream.of(datas).forEach(System.out::println);
```

方法引用的具体分类：
- Object:instanceMethod
- Class:staticMethod
- Class:instanceMethod

上面分类中前两种在 Lambda 表达式的意义上等同，都是将参数传递给方法，如下示例：
```java
System.out::println == x -> System.out.println(x)
```
最后一种分类，第一个参数是方法执行的目标，如下示例：
```java
String::compareToIgnoreCase == (x,y) -> x.compareToIgnoreCase(y)
```
还有类似于 `super::instanceMethod` 这种方法引用本质上与 `Object::instanceMethod` 类似

### 2.4. 构造方法引用

构造方法引用与方法引用类似，除了一点，就是构造方法引用的方法是 new。构造方法引用有两种形式
- Class::new
- Class[]::new

eg1:
```java
String str = "test";
Stream.of(str).map(String::new).peek(System.out::println).findFirst();
```
eg2:
```java
String []copyDatas = Stream.of(datas).toArray(String[]::new);
Stream.of(copyDatas).forEach(x -> System.out.println(x));
```

## 3. Refer Links

[怒学 Java8 系列一：Lambda 表达式](http://www.cnblogs.com/WJ5888/p/4618465.html)

[阮一峰：函数式编程初探](http://www.ruanyifeng.com/blog/2012/04/functional_programming.html)

[Coolshell: 函数式编程](https://coolshell.cn/articles/10822.html)

[Lambda 表达式有何用处？如何使用？](https://www.zhihu.com/question/20125256)