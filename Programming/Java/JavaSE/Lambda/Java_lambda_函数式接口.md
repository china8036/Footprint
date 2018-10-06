- [Java Lambda 函数式接口](#java-lambda-函数式接口)
    - [1. 基本概念](#1-基本概念)
    - [2. 接口定义](#2-接口定义)
        - [2.1. 唯一抽象方法](#21-唯一抽象方法)
        - [2.2. 默认方法](#22-默认方法)
        - [2.3. 静态方法](#23-静态方法)
        - [2.4. Object 类 public 方法](#24-object-类-public-方法)
        - [2.5. 异常声明](#25-异常声明)
    - [3. 泛型及继承关系](#3-泛型及继承关系)
    - [4. @FunctionalInterface 注解](#4-functionalinterface-注解)
    - [5. 实现原理](#5-实现原理)
    - [6. Refer Links](#6-refer-links)

# Java Lambda 函数式接口

## 1. 基本概念

> This is a functional interface and can therefore be used as the assignment target for a lambda expression or method reference.

函数式接口 (Functional Interface) 是 Java 8 对一类特殊类型的接口的称呼，**这类接口要求定义一个且仅一个抽象方法**， 因此也称为 SAM 接口 (Single Abstract Method)。

函数式接口代表的一种契约，一种对某个特定函数类型的契约。在它出现的地方，实际期望一个符合契约要求的函数。Lambda 表达式不能脱离上下文而存在，它必须要有一个明确的目标类型，而这个目标类型就是某个函数式接口。
> 为什么会单单从接口中定义出此类接口呢？ 原因是在 Java Lambda 的实现中， 开发组不想再为 Lambda 表达式单独定义一种特殊的 Structural 函数类型，称之为箭头类型（arrow type），依然想采用 Java 既有的类型系统 (class, interface, method 等)， 原因是增加一个结构化的函数类型会增加函数类型的复杂性，破坏既有的 Java 类型，并对成千上万的 Java 类库造成严重的影响。权衡利弊，最终就利用了 SAM 接口来作为 Lambda 表达式的目标类型。

JDK 8 之前，已有一些本身就是函数式接口的接口：
```
java.lang.Runnable
java.util.concurrent.Callable
java.security.PrivilegedAction
java.util.Comparator
java.io.FileFilter
java.nio.file.PathMatcher
java.lang.reflect.InvocationHandler
java.beans.PropertyChangeListener
java.awt.event.ActionListener
javax.swing.event.ChangeListener
```
JDK 8 中在 java.util.function 包中增加了常用的函数式接口：
```
Predicate -- 传入一个参数，返回一个 bool 结果， 方法为 boolean test(T t)
Consumer -- 传入一个参数，无返回值，纯消费。 方法为 void accept(T t)
Function -- 传入一个参数，返回一个结果，方法为 R apply(T t)
Supplier -- 无参数传入，返回一个结果，方法为 T get()
UnaryOperator -- 一元操作符， 继承 Function, 传入参数的类型和返回类型相同。
BinaryOperator -- 二元操作符， 传入的两个参数的类型和返回类型相同， 继承 BiFunction
```

## 2. 接口定义

### 2.1. 唯一抽象方法

所谓的函数式接口，当然首先是一个接口，然后就是在这个接口里面只能有一个抽象方法。

eg:
```java
interface GreetingService {
    void sayMessage(String message);
}
```

### 2.2. 默认方法

由于默认方法不是抽象方法，且都有一个默认实现，因此在函数式接口里是可以包含默认方法的。

eg:
```java
interface GreetingService {
    void sayMessage(String message);

    default void doSomeMoreWork1() {
        // Method body
    }

    default void doSomeMoreWork2() {
        // Method body
    }
}
```

### 2.3. 静态方法

由于静态方法不能是抽象方法，是一个已经实现了的方法，即符合函数式接口的定义，因此在函数式接口里可以包含静态方法。

eg:
```java
interface GreetingService {
    void sayMessage(String message);
    
    static void printHello() {
        System.out.println("Hello");
    }
}
```

### 2.4. Object 类 public 方法

由于任何一个函数式接口的实现默认都继承了 Object 类，包含了来自 java.lang.Object 里对 Object 类中 public 抽象方法的实现，也就是说这些方法对于函数式接口来说，不被当成是抽象方法，因此函数式接口可以定义 Object 类的 public 方法。

eg:
```java
interface GreetingService {
    void sayMessage(String message);
    
    @Override
    boolean equals(Object obj);
}
```

NOTE: 为什么限定 public 类型的方法呢？因为接口中定义的方法都是 public 类型的。举个例子，下面的接口就不是函数式接口：
```java
interface WrongObjectMethodFunctionalInterface {
	void count(int i);
	
  @Override
	Object clone(); //Object.clone is protected
}
```

### 2.5. 异常声明

函数式接口的抽象方法可以声明抛出 Checked Exception，在调用目标对象的这个方法时必须 catch 这个异常。

eg:
```java
@FunctionalInterface
interface InterfaceWithException {
	void apply(int i) throws Exception;
}

public class FunctionalInterfaceWithException {
	public static void main(String[] args) {
		InterfaceWithException target = i -> {};
		try {
			target.apply(10);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

NOTE: 如果函数式接口中没有声明抛出 Checked Exception，而 Lambda 表达式中抛出了 Exception，则此接口不能作为此 Lambda 表达式的目标类型。

eg:
```java
@FunctionalInterface
interface InterfaceWithException {
	void apply(int i);
}

public class FunctionalInterfaceWithException {
	public static void main(String[] args) {
		InterfaceWithException target = i -> {
      throw new Exception();
    };
    target.apply(10);
	}
}
```
以上代码无法通过编译，因为 Lambda 表达式要求的目标类型和 InterfaceWithException 不同，即：在 Lambda 表达式中抛出了异常，但 InterfaceWithException 的函数却没有声明异常。

## 3. 泛型及继承关系

<!-- TODO: -->

接口可以继承接口，如果父接口是一个函数式接口，那么子接口也可能是一个函数式接口。判断标准依据下面的条件：

对于接口 I, 假定 M 是接口成员里的所有抽象方法的继承（包括继承于父接口的方法)， 除去具有和 Object 的 public 的实例方法签名的方法，那么如果存在一个一个方法 m， 满足：
- m 的签名（subsignature）是 M 中每一个方法签名的子签名（signature）
- m 的返回值类型是 M 中的每一个方法的返回值类型的替代类型（return-type-substitutable）

那么 I 就是一个函数式接口。

## 4. @FunctionalInterface 注解

Java 8 为函数式接口引入了一个新注解 @FunctionalInterface，主要用于编译级错误检查，加上该注解，当你写的接口不符合函数式接口定义的时候，编译器会报错。

NOTE: 加不加 @FunctionalInterface 对于接口是不是函数式接口没有直接影响，该注解只是提醒编译器去检查该接口是否符合函数式接口的要求。

## 5. 实现原理

**在 Java 8 中采用的是内部类来实现 Lambda 表达式**，即创建一个实现了函数式接口的内部实现类。

eg:
```java
@FunctionalInterface
interface Print<T> {
    public void print(T x);
}
public class Lambda {   
    public static void PrintString(String s, Print<String> print) {
        print.print(s);
    }
    public static void main(String[] args) {
        PrintString("test", (x) -> System.out.println(x));
    }
}
```
经过编译器处理后，生成类似以下的代码：
```java
@FunctionalInterface
interface Print<T> {
    public void print(T x);
}

public class Lambda {
    final class Lambda$$0 implements Print<String> {
        @Override
        public void print(String x) {
            System.out.println(x);
        }
    }  
    public static void PrintString(String s, Print<String> print) {
        print.print(s);
    } 
    public static void main(String[] args) {
        PrintString("test", new Lambda().new Lambda$$0());
    }
}
```

## 6. Refer Links

[Java 8 Lambda 实现原理分析](https://www.cnblogs.com/WJ5888/p/4667086.html)

[Java Lambda 表达式 实现原理分析](https://blog.csdn.net/jiankunking/article/details/79825928)

[Java 8 函数式接口 functional interface 的秘密](https://colobu.com/2014/10/28/secrets-of-java-8-functional-interface/)

[JAVA 8 函数式接口 - Functional Interface](https://www.cnblogs.com/chenpi/p/5890144.html)

[Java 8 动态类型语言 Lambda 表达式实现原理分析](https://blog.csdn.net/raintungli/article/details/54910152)