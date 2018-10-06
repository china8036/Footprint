- [Java 异常：处理方法](#java-异常处理方法)
  - [1. 基本操作](#1-基本操作)
    - [1.1. try...catch 块](#11-trycatch-块)
    - [1.2. finally 块](#12-finally-块)
    - [1.3. throw 子句](#13-throw-子句)
    - [1.4. throws 子句](#14-throws-子句)
    - [1.5. 自定义异常](#15-自定义异常)
  - [2. 异常转译](#2-异常转译)
  - [3. 处理原则](#3-处理原则)
  - [4. Refer Links](#4-refer-links)

# Java 异常：处理方法

## 1. 基本操作

Java 异常处理机制主要依赖于 try、catch、finally、throw 和 throws 这 5 个关键字。在 JDK 1.7 中，对异常处理机制进行了进一步增强，包括带资源的 try 语句、捕获多异常的 catch 语句，它们极好地简化了异常处理的代码。

### 1.1. try...catch 块

eg:
```java
public static void main(String args[]){
  try{
      int a[] = new int[2];
      System.out.println("Access element three :" + a[3]);
  }catch(ArrayIndexOutOfBoundsException e){
      System.out.println("Exception thrown  :" + e);
  }
  System.out.println("Out of the block");
}
```

运行过程：
1. 如果执行 try 块中的代码时出现异常，系统会自动生成一个异常对象，并将该异常对象提交给 JRE，这个过程称为**抛出异常**。

1. 当 JRE 收到异常对象时，会寻找能处理该异常对象的 catch 块，即依次判断该异常对象是否是 catch 块后异常类或其子类的实例对象：
    - 如果是，就将该异常对象作为实参传递到该 catch 块中由它来处理，这个过程称为**捕获异常**；
    - 如果不是，就与下一个 catch 块的异常类进行匹配；
    - 如果匹配所有的 catch 块后，仍没有找到能捕获异常的 catch 块，就会把异常对象抛给 JVM，JVM 终止 JRE，Java 程序也随之退出。

API:

如果程序需要在 catch 块中访问异常对象的相关信息，可以通过异常类实例对象的 API 来获取。所有异常对象都包含了以下几个常用方法：
- `getMessage()`: 返回该异常的详细描述字符串。
- `printStackTrace()`: 将该异常的跟踪栈信息输出到 stderr。
- `printStackTrace(PrintStream s)`: 将该异常的跟踪栈信息输出到指定输出流。
- `getStackTrace()`: 返回该异常的跟踪栈信息。

JDK 7 改进：
- 多异常捕获

  在 JDK 7 之前，每个 catch 块只能捕获一个异常类实例，但从 JDK 7 开始，在一个 catch 块中可以捕获多种类型的异常，各个类型之间用 `|` 间隔。

- 自动关闭资源

  JDK 7 允许在 try 关键字后紧跟一堆圆括号，并在圆括号中声明、初始化一个或多个实现了 AutoCloseable 接口或 Closeable 接口的资源类，try 语句会在该语句结束时 Zion 个关闭这些物理资源。

  自动关闭资源的 try 语句相当于包含了隐式的 finally 块，因此这个 try 语句可以同时没有 catch 块和 finally 块。

NOTE: 
- try...catch 结构中，try 块是必须的，而**catch 块和 finally 块可任选其一，也可同时具备，但不能同时缺少**。

### 1.2. finally 块

如果在 try 块中打开了某些物理资源，由于 Java GC 不会回收任何物理资源，为保证这些资源一定能够被关闭，异常机制提供了 finally 块。不管 try 块中的代码是否出现异常，finally 块中的代码总会保证被执行。

NOTE:

如果 Java 程序在执行 try/catch 块时遇到了 return/throw 语句，系统会先去寻找该异常处理流程中是否包含了 finally 块。如果没有，则立即执行 return/throw 语句，方法终止。如果有 finally 块，则系统会立即执行 finally 块，执行完毕后才跳回 try/catch 块中执行 return/throw 语句。

因此，一旦在 finally 块中使用了 return 或 throw 语句，会导致 try 块、catch 块中的 return、throw 语句失效。

### 1.3. throw 子句

当程序出现错误时，系统会自动抛出异常，而除此之外，Java 程序也可以使用 throw 语句来自行抛出异常。

- 如果 throw 抛出的异常属于 Checked 异常，那么该 throw 语句应包含在 try...catch 结构中，或者在一个带 throws 声明的方法中。
- 如果 throw 抛出的异常属于 Runtime 异常，那么程序可以显式使用 try...catch 结构来捕获处理，也可以完全不理会。

### 1.4. throws 子句

如果当前方法不知道如何处理异常，应该在方法声明处使用 throws 语句将异常抛出，由上一级调用者进行处理。

throws 语句可以抛出多个异常类，多个异常类之间使用逗号隔开。

### 1.5. 自定义异常

在通常情况下，程序很少会自行抛出系统异常，因为异常的类名通常也包含了该异常的有用信息。因此，在使用 throw 语句抛出异常时，可以使用自定义的异常，从而可以明确地描述该异常情况。

用户自定义的异常类，如果是 Checked 异常，应继承 Exception 基类；如果是 Runtime 异常，应继承 RuntimeException 基类。通常在定义异常类时，需要至少提供 2 个构造器：
- 无参构造器
- 带一个 String 参数的构造器，String 参数作为该异常对象的描述信息（getMessage() 方法的返回值）

eg:
```java
class TestException extends Exception {
	public TestException() {
	}

	public TestException(String message) {
		super(message);
	}
}
```

## 2. 异常转译

当业务逻辑层出现异常时，程序不应该将底层的异常传递给用户接口层，这是一种不负责任的行为。更好的做法是：程序先捕获原始异常，然后抛出一个新的业务异常，新的业务异常中包含了对用户的提示信息，这种处理方式称为异常转译。

异常转移可以保证底层异常不会扩散到表现层，避免向上暴露过多的实现细节，也完全符合面向对象的封装原则。同时，这种把捕获一个异常后接着抛出另一个异常，并把原始异常的信息保留下来是一种典型的链式处理，符合设计模式中的“职责链模式”思想，称为“异常链”。

```java
class TestException extends Exception {
	public TestException() {
	}

	public TestException(String message) {
		super(message);
	}

	public TestException(Throwable cause) {
		super(cause);
	}
}
```

## 3. 处理原则

成功的异常处理应满足以下 4 个基本目标：
- 使代码混乱最小化
- 捕获并保留诊断信息
- 通知合适的人员
- 采用合适的方式结束异常活动

为达到这些目的，通常有以下基本准则：
- 先处理小异常，再处理大异常。进行异常捕获时，应将所有父类异常捕获的 catch 块排在子类异常捕获的 catch 块后边，否则会出现编译错误。

- 异常处理的嵌套深度虽然没有明确的限制，但通过没有必要使用超过 2 层的嵌套异常处理，层次太深的嵌套会导致程序可读性严重降低。

- 不过度使用异常机制。异常处理的初衷是将不可预期的异常处理代码和正常的业务逻辑处理代码分离，因此绝不要使用异常处理来代替正常的业务逻辑判断，这将严重影响执行效率。

- 不要使用过于庞大的 try 块。应将代码放在不同的 try 块中，分别捕获并处理其异常类型。

## 4. Refer Links

[如何优雅的处理异常（java）？](https://www.zhihu.com/question/28254987)