- [Java 基础：泛型](#java-基础泛型)
  - [1. 基础概念](#1-基础概念)
  - [2. 基本使用](#2-基本使用)
    - [2.1. 泛型类](#21-泛型类)
    - [2.2. 泛型方法](#22-泛型方法)
    - [2.3. 泛型构造器](#23-泛型构造器)
    - [2.4. 形参限制](#24-形参限制)
    - [2.5. 类型通配符](#25-类型通配符)
    - [2.6. 泛型数组](#26-泛型数组)
  - [3. 泛型擦除](#3-泛型擦除)
  - [4. 实现原理](#4-实现原理)
    - [4.1. Java 伪泛型机制](#41-java-伪泛型机制)
    - [4.2. C++ 模板机制](#42-c-模板机制)
  - [5. Refer Links](#5-refer-links)

# Java 基础：泛型

## 1. 基础概念

在 JDK 1.5 之前，一旦将某个对象放到 Java 集合中，集合就会忘记对象的类型，将所有的对象当成 Object 类型处理。这样的处理方式存在以下问题：
- 集合对元素类型没有任何限制，可能引发一些未知的问题。
- 当从集合中取出元素后必须使用强制类型转换才能正常使用对象，既增加了编程的复杂度，也有可能会引发 ClassCastException 异常。

从 JDK 1.5 开始，Java 引入了参数化类型（泛型），允许程序在创建集合时指定集合元素的类型。泛型集合可以在编译时检查集合中元素的类型，如果试图向集合中添加不满足类型要求的对象，编译器就会提示错误。泛型使得代码更加简洁、健壮。

除此之外，Java 泛型还增强了枚举类、反射等方面的功能。

## 2. 基本使用

### 2.1. 泛型类

```java
class Demo<T, S> {
  public void demo1(T [] a) {/* ... */}
  public void demo2(S [] a) {/* ... */}
}
```

在实例化泛型类时，需要传入类型参数：
```java
Demo<String, Integer> demo = new Demo<String, Integer>();
```
JDK 1.7 引入了菱形语法，Java 可以自动推断出尖括号内是什么类型参数，使得泛型类的实例化更加简洁高效：
```java
Demo<String, Integer> demo = new Demo<>();
```

NOTE:
- **泛型类 Demo<T> 的构造器名称是 Demo 而不是 Demo<T>**。
- 当创建了带泛型声明的接口、父类后，它们的实现类、子类不能再包含类型形参。
  ```java
  class A<T> {}
  class B extends A<T> {}       // wrong
  class B extends A<String> {}  // right
  class B extends A {}          // right, but has warnning: `Unchecked`
  ```
- 若 A 是 B 的父类，G 是一个泛型类，需要注意 G<A> 不是 G<B> 的父类。

### 2.2. 泛型方法
  
```java
class Demo {
  public <T, S> void demo(T [] a, Collection<S> c) {/* ... */}
}
```
与接口、类声明中定义的类型形参不同的是：
- 方法声明中定义的形参只能在该方法中使用，而接口、类声明中定义的形参可以在整个接口、类中使用。
- 调用泛型方法时，无须显式传入实际的类型参数，编译器会根据实参推断出形参的值。如：`demo(a, b)`。

NOTE: 若在同一个类中多个同签名的方法仅有泛型类型不同（方法重载），**在定义时不会有任何错误，但在调用时会引起编译错误**：
```java
class Demo {
  // 在调用时由于两个方法都能匹配到，会引起编译错误
  public static <T> void demo(Collection<T> a, Collection<? extends T> b) {/* ... */}
  public static <T> void demo(Collection<? super T> a, Collection<T> b) {/* ... */}
}
```

### 2.3. 泛型构造器

```java
class Demo {
  public <T, S> Demo(T a, S c) {/* ... */}
}
```  
一旦使用了泛型构造器，接下来在调用构造器时，就可以通过显式或隐式的方法给构造器传递类型参数：
```java
// 隐式传递类型参数 String, Integer
new Demo("ABCD", 200);
// 显式传递类型参数 String, Integer
new <String, Integer> Demo("ABCD", 200);
// 显式传递类型参数 String, Integer，但第二个实参为 Double，报错
new <String, Integer> Demo("ABCD", 200.0);
```

NOTE: 若类声明与构造器声明中使用了不同的类型形参：
```java
class Demo<E, F> {
  public <T, S> Demo(T a, S c) {/* ... */}
}
```
则在调用时：
```java
// 传递给类声明的类型实参为 Long,Double
// 传递给构造器声明的类型实参为 String,Integer，为隐式传递
// 同时在调用构造器时使用了 JDK 7 的菱形语法
Demo<Long, Double> demo = new Demo<>("ABCD", 200);

// 传递给类声明的类型实参为 Long,Double
// 传递给构造器声明的类型实参为 String,Integer，为显式传递
Demo<Long, Double> demo = new <String, Integer> Demo<Long, Double>("ABCD", 200);

// 传递给类声明的类型实参为 Long,Double
// 传递给构造器声明的类型实参为 String,Integer，为显式传递
// 同时在调用构造器时使用了 JDK 7 的菱形语法，但由于显式指定构造器的类型实参时不能使用菱形语法，因此以下代码报错
Demo<Long, Double> demo = new <String, Integer> Demo<>("ABCD", 200);
```

### 2.4. 形参限制

```java
class Demo<T extend Number & java.io.Serializeable, S> {
  public void demo1(T [] a) {/* ... */}
  public void demo2(S [] a) {/* ... */}
}
```

```java
class Demo {
  public <T extend Number & java.io.Serializeable> void demo1(T [] a) {/* ... */}
  public <S> void demo2(S [] a) {/* ... */}
}
```

### 2.5. 类型通配符

TODO: 只能用在泛型方法中？泛型类？
```java
class Demo {
  public void demo(List<?> list) {/* ... */}
}
```

需要注意的是：使用通配符创建泛型类实例时，不能向集合中添加任何元素（除了 null，因为 null 是所有类的实例），但可调用 get() 方法来返回集合中的元素。
```java
List<?> list = new ArrayList<String>();
list.add(new Object());   // 编译错误
list.add(null);           // 正常运行
list.get(0);              // 正常运行
```

- 通配符上限

  要求未知类型必须是 Number 类的子类，并且实现了 Serializeable 接口：
  ```java
  void demo<? extends Number & java.io.Serializeable> {/* ... */}
  ```

- 通配符下限

  eg: 

  java.util.TreeSet 的一个构造器中也使用了通配符下限的语法：
  ```java
  TreeSet(Comparator<? super E> c);
  ```
  通过这种通配符下限的方式来定义构造器的参数，就可以将所有可用的 Comparator 作为参数传入，从而增加了程序的灵活性。

### 2.6. 泛型数组

```java
// 编译错误：Generic array creation
List<Integer> [] p = new ArrayList<Integer>[10];
List<Integer> [] p = new ArrayList<>[10];

// 编译警告：Unchecked assignment: 'java.util.ArrayList[]' to 'java.util.List<java.lang.Integer>[]'，但不会报错，正常运行。
List<Integer> [] p = new ArrayList[10];

// 允许创建无上限的通配符泛型数组，正常编译和运行。但在这种情况下，使用时必须进行类型强制转换。
List<?> [] p = new ArrayList[10];
List<?> [] p = new ArrayList<?>[10];
```

P.S. 创建元素类型是类型变量的对象或数组时也会导致编译错误：`Type parameter 'T' cannot be instantiated directly`。
```java
// 报错
static <T> void test1() {T [] a = new T[10];}
// 报错
static <T> void test2() {T a = new T();}
// 正常 TODO: why?
static <T> void test3() {T a;}
```
**由于类型变量在运行时不存在，导致 JIT 无法确定实际类型是什么，因此会报错**。

## 3. 泛型擦除

当把一个具有泛型信息的对象赋值给另一个没有泛型信息的变量时，所有在尖括号之间的类型信息都会被擦除。

eg:
```java
class A<T extends Number> {
	T size;

	public A(T size) {
		size = size;
	}
}

public class Main {
	public static void main(String[] args) {
		A<Integer> a = new A<>(6);  // 隐式传递类型参数
		Integer t1 = a.size;        // 正常
		
    A b = a;                    // 将具有类型信息的对象 a 赋值给没有泛型信息的变量，发生了泛型擦除
		Number t2 = b.size;         // 正常
		Integer t3 = b.size;        // 报错：Incompatible types
  }
}
```

## 4. 实现原理

### 4.1. Java 伪泛型机制

Java 的泛型是伪泛型。

Java 中的泛型基本上都是在编译器这个层次来实现的，在编译期间，所有的泛型信息都会被擦除，在生成的 Java 字节码中是不包含泛型中的类型信息的。例如，在代码中定义的 `List<object>` 和 `List<String>` 等类型，在编译后都会变成 List。Java 编译器会在编译时尽可能的发现可能出错的地方，但是仍然无法避免在运行时刻出现类型转换异常的情况。JVM 看到的只是 List 类，而由泛型附加的类型信息对 JVM 来说是不可见的。

不同类型实参的泛型类只是在逻辑上互相独立，但在物理上它们都属于同一个类，在内存中也只占用一块内存空间，系统不会进行源代码复制，也不会生成新的 class 文件。

```java
List<Integer> l1 = new ArrayList<>();
List<String> l2 = new ArrayList<>();
System.out.println(l1.getClass() == l2.getClass()); // true
```

因此，**在静态方法、静态初始化块或静态变量的声明和初始化中不允许使用类型形参**。

另外，由于系统不会真正生成泛型类，所以 **instanceof 运算符后不能使用泛型类**。
```java
if (a instanceof ArrayList<String>) {/* ... */} // wrong
```

### 4.2. C++ 模板机制

类型擦除也是 Java 的泛型实现方法与 C++ 模版机制实现方式之间的重要区别。

C++ 的模板会对针对不同的模板参数静态实例化，目标代码体积会稍大一些，运行速度会快很多。

对比：
- C++ template 是 reified generic，Java generic 用的是 type erasure。
- C++ 是在 call site 做 instantiate type，Java 是在 call site 插入 cast。
- C++ template 在 call site 可以做 inline，Java generic 因为并没有在 call site 生成代码所以不行。
- C++ 在 runtime 没有额外的开销，Java 在 runtime 有 cast 的开销。
- C++ 的每个 reified generic type 都有一份独立的代码，Java 只有一份 type erased 之后的代码。
- C++ 的 type check 是在编译时做的，Java 的 type check 在编译期和运行期都要做一些工作。

## 5. Refer Links

[java 泛型（二）、泛型的内部原理：类型擦除以及类型擦除带来的问题](https://blog.csdn.net/lonelyroamer/article/details/7868820)

TODO:

[java 泛型擦除发生在哪个阶段，如何用反编译工具查看泛型擦除后的代码？](https://www.zhihu.com/question/38940308)

[Java 不能实现真正泛型的原因？](https://www.zhihu.com/question/28665443)

[C++ 模板和 Java 泛型有什么异同？](https://www.zhihu.com/question/33304378)