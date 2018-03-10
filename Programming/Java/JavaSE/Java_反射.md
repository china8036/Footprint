- [Java 反射](#java-%E5%8F%8D%E5%B0%84)
  - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
  - [2. Class](#2-class)
    - [2.1. 定义](#21-%E5%AE%9A%E4%B9%89)
    - [2.2. Class 对象的获取](#22-class-%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%8E%B7%E5%8F%96)
      - [2.2.1. 通过 .class](#221-%E9%80%9A%E8%BF%87-class)
      - [2.2.2. 通过 Object.getClass()](#222-%E9%80%9A%E8%BF%87-objectgetclass)
      - [2.2.3. 通过 Class.forName()](#223-%E9%80%9A%E8%BF%87-classforname)
      - [2.2.4. NOTE](#224-note)
    - [2.3. Class 对象的操作](#23-class-%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%93%8D%E4%BD%9C)
      - [2.3.1. 类名](#231-%E7%B1%BB%E5%90%8D)
      - [2.3.2. 类的修饰符](#232-%E7%B1%BB%E7%9A%84%E4%BF%AE%E9%A5%B0%E7%AC%A6)
      - [2.3.3. 类的成员变量 Field](#233-%E7%B1%BB%E7%9A%84%E6%88%90%E5%91%98%E5%8F%98%E9%87%8F-field)
        - [2.3.3.1. 获取 Field 对象（使用 Class 类中的方法）](#2331-%E8%8E%B7%E5%8F%96-field-%E5%AF%B9%E8%B1%A1%EF%BC%88%E4%BD%BF%E7%94%A8-class-%E7%B1%BB%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
        - [2.3.3.2. 获取 Field 类型（使用 Field 类中的方法）](#2332-%E8%8E%B7%E5%8F%96-field-%E7%B1%BB%E5%9E%8B%EF%BC%88%E4%BD%BF%E7%94%A8-field-%E7%B1%BB%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
          - [2.3.3.2.1. 数组类型的反射操作](#23321-%E6%95%B0%E7%BB%84%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%8F%8D%E5%B0%84%E6%93%8D%E4%BD%9C)
          - [2.3.3.2.2. 枚举类型的反射操作](#23322-%E6%9E%9A%E4%B8%BE%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%8F%8D%E5%B0%84%E6%93%8D%E4%BD%9C)
        - [2.3.3.3. 读取 / 修改 Field 值（使用 Field 类中的方法）](#2333-%E8%AF%BB%E5%8F%96-%E4%BF%AE%E6%94%B9-field-%E5%80%BC%EF%BC%88%E4%BD%BF%E7%94%A8-field-%E7%B1%BB%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
      - [2.3.4. 类的方法 Method](#234-%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95-method)
        - [2.3.4.1. 获取 Method 对象（使用 Class 类中的方法）](#2341-%E8%8E%B7%E5%8F%96-method-%E5%AF%B9%E8%B1%A1%EF%BC%88%E4%BD%BF%E7%94%A8-class-%E7%B1%BB%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
        - [2.3.4.2. 获取方法要素（使用 Method 类中的方法）](#2342-%E8%8E%B7%E5%8F%96%E6%96%B9%E6%B3%95%E8%A6%81%E7%B4%A0%EF%BC%88%E4%BD%BF%E7%94%A8-method-%E7%B1%BB%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
        - [2.3.4.3. 执行方法（使用 Method 类的方法）](#2343-%E6%89%A7%E8%A1%8C%E6%96%B9%E6%B3%95%EF%BC%88%E4%BD%BF%E7%94%A8-method-%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
      - [2.3.5. 类的构造方法 Constructor](#235-%E7%B1%BB%E7%9A%84%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95-constructor)
        - [2.3.5.1. 获取 Constructor 对象（使用 Class 类的方法）](#2351-%E8%8E%B7%E5%8F%96-constructor-%E5%AF%B9%E8%B1%A1%EF%BC%88%E4%BD%BF%E7%94%A8-class-%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
        - [2.3.5.2. 创建类的实例对象（使用 Constructor 类的方法）](#2352-%E5%88%9B%E5%BB%BA%E7%B1%BB%E7%9A%84%E5%AE%9E%E4%BE%8B%E5%AF%B9%E8%B1%A1%EF%BC%88%E4%BD%BF%E7%94%A8-constructor-%E7%B1%BB%E7%9A%84%E6%96%B9%E6%B3%95%EF%BC%89)
  - [3. 反射异常](#3-%E5%8F%8D%E5%B0%84%E5%BC%82%E5%B8%B8)
  - [4. 应用](#4-%E5%BA%94%E7%94%A8)
    - [4.1. 绕过安全机制](#41-%E7%BB%95%E8%BF%87%E5%AE%89%E5%85%A8%E6%9C%BA%E5%88%B6)
    - [4.2. 实现 JDK 动态代理](#42-%E5%AE%9E%E7%8E%B0-jdk-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86)
      - [4.2.1. 基本概念](#421-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [4.2.1.1. 代理模式](#4211-%E4%BB%A3%E7%90%86%E6%A8%A1%E5%BC%8F)
        - [4.2.1.2. 静态代理](#4212-%E9%9D%99%E6%80%81%E4%BB%A3%E7%90%86)
        - [4.2.1.3. 动态代理](#4213-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86)
      - [4.2.2. JDK 动态代理实现方法](#422-jdk-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E5%AE%9E%E7%8E%B0%E6%96%B9%E6%B3%95)
      - [4.2.3. 动态代理与 AOP](#423-%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86%E4%B8%8E-aop)
    - [4.3. 使用泛型 Class 避免强制类型转换](#43-%E4%BD%BF%E7%94%A8%E6%B3%9B%E5%9E%8B-class-%E9%81%BF%E5%85%8D%E5%BC%BA%E5%88%B6%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [5. Refer Links](#5-refer-links)

# Java 反射

## 1. 基本概念

[官网定义](https://docs.oracle.com/javase/tutorial/reflect/)：
> Reflection is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine. This is a relatively advanced feature and should be used only by developers who have a strong grasp of the fundamentals of the language. With that caveat in mind, reflection is a powerful technique and can enable applications to perform operations which would otherwise be impossible.

反射技术通常被用来检测和改变应用程序在 Java 虚拟机中的运行阶段的行为表现。它是一个相对而言比较高级的技术，通常它应用的前提是开发者本身对于 Java 语言特性有很强的理解的基础上。值得说明的是，反射是一种强有力的技术特性，因此可以使得应用程序突破一些藩篱，执行一些常规手段无法企及的目的。

反射操作都是编译之后的操作，即运行时操作。

反射属于非常规的开发手段，它会抛弃 Java 虚拟机的很多优化，因此往往同样功能的代码，反射要比正常方式执行要慢，所以采用反射时，要考虑它的时间成本。

## 2. Class

### 2.1. 定义

> Instances of the class Class represent classes and interfaces in a running Java application. An enum is a kind of class and an annotation is a kind of interface. Every array also belongs to a class that is reflected as a Class object that is shared by all arrays with the same element type and number of dimensions. The primitive Java types (boolean, byte, char, short, int, long, float, and double), and the keyword void are also represented as Class objects.

java.lang.[Class](https://docs.oracle.com/javase/9/docs/api/java/lang/Class.html) 类是 Java 反射机制的基础和入口，在 Java 中的所有类都是 Class 类的实例对象（也称为类的“类类型”），通过 Class 类我们可以获得关于一个类的相关信息。

在 Java 中每个类都有一个 Class 对象，每当我们编写并且编译一个新创建的类就会产生一个对应 Class 对象并且这个 Class 对象会被保存在同名。class 文件里（编译后的字节码文件保存的就是 Class 对象)，那为什么需要这样一个 Class 对象呢？是这样的，当我们 new 一个新对象或者引用静态成员变量时，Java 虚拟机 (JVM) 中的类加载器子系统会将对应 Class 对象加载到 JVM 中，然后 JVM 再根据这个类型信息相关的 Class 对象创建我们需要实例对象或者提供静态变量的引用值。需要特别注意的是，手动编写的每个 class 类，无论创建多少个实例对象，在 JVM 中都只有一个 Class 对象，即在内存中每个类有且只有一个相对应的 Class 对象。

```java
public final class Class<T> extends Object
                            implements java.io.Serializable,
                                GenericDeclaration,
                                Type,
                                AnnotatedElement {}
```

### 2.2. Class 对象的获取

> Class has no public constructor. Instead Class objects are constructed automatically by the Java Virtual Machine as classes are loaded and by calls to the defineClass method in the class loader.

一个正在运行的 Java 程序中的类或接口，都是一个 Class 类的实例对象。Class 没有 public 的构造方法，也就是说没有办法像创建一个类一样通过 new 关键字来获取一个 Class 对象，只有在类加载时 JVM 才会自动的直接创建 Class 的实例对象。但可以通过以下 3 种方式间接获取 Class 类的实例对象：

#### 2.2.1. 通过 .class

任何一个类都有一个隐含的静态成员变量 class
```java
public class Car {}

public class Test {
    public static void main(String[] args) {
        Class clazz = Car.class;
        Class cls1 = int.class;
        Class cls2 = String.class;

    }
}
```

#### 2.2.2. 通过 Object.getClass()

任何一个类对象都有一个隐含的方法 Object.getClass()
```java
public class Car {}

public class Test {
    public static void main(String[] args) {
        Car car = new Car();
        Class clazz = car.getClass();
    }
}
```
NOTE: 这种方法不适合基本数据类型 （如 int、float) 的类类型的获取。

#### 2.2.3. 通过 Class.forName()

通过 `Class.forName()` 获取类的类类型

`Class.forName()` 的参数是指定类的全限定名称（包括包名 + 类名），该方法会到 Java 虚拟机中去寻找指定的类有没有被加载，如果查找的类没有在 JVM 中加载，会抛出 ClassNotFoundException 异常。

```java
try {
    Class clz = Class.forName("com.demo.test.Car");
} catch (ClassNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
}
```

#### 2.2.4. NOTE

- 可以通过不同的方式获取类的 Class 对象，但对于同一个类以及该类的不同实例对象，获取到的 Class 对象都指向同一个 Class 对象。
  ```java
  public class Test {
    public int a;

    public void static main(String [] args) {
      Test a = new Test();
      Test b = new Test();
      Class ca = a.getClass();
      Class cb = b.getClass();
      Class c = Test.class;

      System.out.println(ca == cb);
      System.out.println(ca == c);
      System.out.println(cb == c);
    }
  }
  ```
  运行结果
  ```
  true
  ```

- 事实上，Class 对象本身不对成员进行储存，它只提供检索功能。因此，若要对类的不同实例对象进行操作，需要用 Field、Method、Constructor 对象来承载这些成员，为了精确定位，还需要为成员指定类的实例对象的引用。
  ```java
  public class Test {
    public int field;

    public void static main(String [] args) {
      Test a = new Test();
      Test b = new Test();
      Class ca = a.getClass();
      Class cb = b.getClass();
      Class c = Test.class;

      a.field = 1;
      b.field = 2;
      
      Field af = ca.getField("field");
      System.out.println(af.getInt(a));
      System.out.println(af.getInt(b));
    }
  }
  ```
  运行结果
  ```
  1
  2
  ```

- 编译时加载类称为静态加载类，运行时加载类称为动态加载类。
  - 使用 new 创建类对象属于静态加载类，在编译时就需要加载所有可能使用到的类。
  - 使用 `Class.forName()` 获取类的类类型属于动态加载类，JVM 会在运行阶段才加载所需要的类。

### 2.3. Class 对象的操作

获取到类的 Class 对象后，这正是反射机制中开始的地方，我们运用反射的目的就是为了获取和操控 Class 对象中的各种成员。

#### 2.3.1. 类名

相关 API:

- `Class.getName();`: Returns the name of the entity (class, interface, array class, primitive type, or void) represented by this Class object, as a String.
  - 当 Class 对象是一个引用时，该方法返回的是一个二进制形式的字符串，比如“com.demo.test.Car”。
  - 当 Class 对象是一个基本数据类型时，该方法返回的是它们的关键字，比如 int.class 的名字是 int。
  - 当 Class 对象是一个基础数据类型的数组时，Java 本身对于这一块制定了相应规则，在元素的类型前面添加相应数量的 [ 符号，用 [ 的个数来提示数组的维度，并且值得注意的是，对于基本类型或者是类，都有相应的编码，所谓的编码大多数是用一个大写字母来指示某种类型，规则如下：
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/9/662c3ae9e2e927ff69821495729562db.jpg)

    （注意：类或者是接口的类型编码是 L 类名；的形式，后面有一个分号)

    例：
    - int[][][] 这样的 3 维数组，getName() 返回 `[[[I` 这样的字符串。
    - String[].getClass().getName() 结果是 `[Ljava.lang.String;`。

  例：
  ```java
  public class Test {
      public static void main(String[] args) {
          try {
              Class clz = Class.forName("com.demo.test.Car");
              Class clz1 = float.class;
              Class clz2 = Void.class;
              Class clz3 = new int[]{}.getClass();
              Class clz4 = new Car[]{}.getClass();

              System.out.println(clz.getName());
              System.out.println(clz1.getName());
              System.out.println(clz2.getName());
              System.out.println(clz3.getName());
              System.out.println(clz4.getName());
          } catch (ClassNotFoundException e) {
              // TODO Auto-generated catch block
              e.printStackTrace();
          }
      }
  }
  ```
  运行结果：
  ```
  com.demo.test.Car
  float
  java.lang.Void
  [I
  [Lcom.frank.test.Car;
  ```

- `Class.getSimpleName();`: Returns the simple name of the underlying class as given in the source code.
  - 当获取一个类的 simplename 时，会返回该类的类名，但不包类名的包名前缀和内部类前缀。
    ```java
    public class Outter {
        static class Inner {}
    }
    ```
    ```java
    Class clz = Outter.Inner.class;

    System.out.println(" Inner Class name:"+clz.getName());
    System.out.println(" Inner Class simple name:"+clz.getSimpleName());
    ```
    运行结果
    ```
    Inner Class name:com.frank.test.Outter$Inner
    Inner Class simple name:Inner
    ```
  
  - 对于匿名内部类，getSimpleName() 返回的是一个空的字符串。
    ```java
    Runnable run = new Runnable() {
        @Override
        public void run() {
            // TODO Auto-generated method stub
        }
    };

    System.out.println("Inner Class name:"+run.getClass().getName());
    System.out.println("Inner Class simple name:"+run.getClass().getSimpleName());
    ```
    运行结果
    ```
    anonymous Class name:com.frank.test.Test$1
    anonymous Class simple name:
    ```

  - 当获取一个数组的 Class 中的 simplename 时，不同于 getName() 方法，simplename 不是在前面加 [，而是在后面添加对应数量的 []。
    ```java
    Class clz = Cl new Outter.Inner[][][]{}.getClass();

    System.out.println(" Inner Class name:"+clz.getName());
    System.out.println(" Inner Class simple name:"+clz.getSimpleName());
    ```
    运行结果
    ```
    Inner Class name:[[[Lcom.frank.test.Outter$Inner;
    Inner Class simple name:Inner[][][]
    ```

- `Class.getCanonicalName();`: Returns the canonical name of the underlying class as defined by the Java Language Specification.

  getCanonicalName() 是 getName() 和 getSimpleName() 的结合，该方法返回一个 Class 对象的官方名字，这个官方名字 canonicalName 是 Java 语言规范制定的，如果 Class 对象没有 canonicalName 的话就返回 null（比如局部类和匿名内部类）。

  - `getCanonicalName()` 返回的也是全限定类名，但是对于内部类，不用 $ 开头，而用 .。
  - `getCanonicalName()` 对于数组类型的 Class，同 simplename 一样直接在后面添加 [] 。
  - `getCanonicalName()` 不同于 simplename 的地方是，不存在 canonicalName 的时候返回 null 而不是空字符串。

#### 2.3.2. 类的修饰符

通常，在 Java 中定义一个类，往往是要通过许多修饰符来配合使用的，它们大致分为 4 类。
- 用来限制作用域，如 public、protected、priviate。
- 用来提示子类复写，abstract。
- 用来标记为静态类 static。
- 注解 @interface。

Java 反射提供了 API 去获取这些修饰符：
> `int	getModifiers​()`: Returns the Java language modifiers for this class or interface, encoded in an integer.

该方法返回的是一个 int 数值，为什么会返回一个整型数值呢？这是因为一个类定义的时候可能会被多个修饰符修饰，为了一并获取，所以 Java 工程师考虑到了**位运算**，用一个 int 数值来记录所有的修饰符，然后不同的位对应不同的修饰符，这些修饰符对应的位都定义在 Modifier 这个类当中：
```java
public class Modifier {

    public static final int PUBLIC           = 0x00000001;

    public static final int PRIVATE          = 0x00000002;

    public static final int PROTECTED        = 0x00000004;

    public static final int STATIC           = 0x00000008;

    public static final int FINAL            = 0x00000010;

    public static final int SYNCHRONIZED     = 0x00000020;

    public static final int VOLATILE         = 0x00000040;

    public static final int TRANSIENT        = 0x00000080;

    public static final int NATIVE           = 0x00000100;

    public static final int INTERFACE        = 0x00000200;

    public static final int ABSTRACT         = 0x00000400;

    public static final int STRICT           = 0x00000800;

    public static String toString(int mod) {
        StringBuilder sb = new StringBuilder();
        int len;

        if ((mod & PUBLIC) != 0)        sb.append("public ");
        if ((mod & PROTECTED) != 0)     sb.append("protected ");
        if ((mod & PRIVATE) != 0)       sb.append("private ");

        /* Canonical order */
        if ((mod & ABSTRACT) != 0)      sb.append("abstract ");
        if ((mod & STATIC) != 0)        sb.append("static ");
        if ((mod & FINAL) != 0)         sb.append("final ");
        if ((mod & TRANSIENT) != 0)     sb.append("transient ");
        if ((mod & VOLATILE) != 0)      sb.append("volatile ");
        if ((mod & SYNCHRONIZED) != 0)  sb.append("synchronized ");
        if ((mod & NATIVE) != 0)        sb.append("native ");
        if ((mod & STRICT) != 0)        sb.append("strictfp ");
        if ((mod & INTERFACE) != 0)     sb.append("interface ");

        if ((len = sb.length()) > 0)    /* trim trailing space */
            return sb.toString().substring(0, len-1);
        return "";
    }

    public static boolean isPublic(int mod) {
        return (mod & PUBLIC) != 0;
    }

    public static boolean isPrivate(int mod) {
        return (mod & PRIVATE) != 0;
    }

    public static boolean isProtected(int mod) {
        return (mod & PROTECTED) != 0;
    }

    public static boolean isStatic(int mod) {
        return (mod & STATIC) != 0;
    }

    public static boolean isFinal(int mod) {
        return (mod & FINAL) != 0;
    }

    public static boolean isSynchronized(int mod) {
        return (mod & SYNCHRONIZED) != 0;
    }

    public static boolean isVolatile(int mod) {
        return (mod & VOLATILE) != 0;
    }

    public static boolean isTransient(int mod) {
        return (mod & TRANSIENT) != 0;
    }

    public static boolean isNative(int mod) {
        return (mod & NATIVE) != 0;
    }

    public static boolean isInterface(int mod) {
        return (mod & INTERFACE) != 0;
    }

    public static boolean isAbstract(int mod) {
        return (mod & ABSTRACT) != 0;
    }

    public static boolean isStrict(int mod) {
        return (mod & STRICT) != 0;
    }

}
```

因此，调用 `Modifier.toString()` 方法就可以打印出一个类的所有修饰符，调用 Modifier 类的一系列静态工具方法可判断是否包含某个修饰符（如 `Modifier.isPrivate`）。

#### 2.3.3. 类的成员变量 Field

https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/Field.html

```java
public final class Field
                    extends AccessibleObject
                    implements Member
```

##### 2.3.3.1. 获取 Field 对象（使用 Class 类中的方法）

- `Field getField(String name)`: 获取指定名字的属性，该方法获取的是非私有属性，并且在当前 Class 获取不到时会向祖先类获取。
- `Field getDeclaredField(String name)`: 获取指定名字的属性，该方法获取的是被 private 修饰的属性，若在当前 Class 获取不到时不会向祖先类获取。
- `Field[] getFields()`: 获取自身的所有的 public 属性，包括从父类继承下来的。
- `Field[] getDeclaredFields()`: 获取所有的属性，但不包括从父类继承下来的属性。

NOTE：
- getField() 方法和 getDeclaredField() 方法的能力范围：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/9/2a303b5e3a59d4fdc927e62983430242.jpg)

- 如何获取一个 Class 中继承下来的非 public 修饰的 Field？
  
  通过获取这个 Class 的 superClass，然后调用这个 superClass 的 getDeclaredField() 方法。

##### 2.3.3.2. 获取 Field 类型（使用 Field 类中的方法）

- `public Type getGenericType()`：能够获取泛型类型，Type 是 Class 所实现的接口。
- `public Class<?> getType()`：不能获取泛型类型。

###### 2.3.3.2.1. 数组类型的反射操作

- 若 Field 类型为数组类型，可使用 Class 类中的 API 获取数组类型的 Field 的具体信息：
  - `getName()`
  - `getComponentType()`

  例：
  ```java
  public class Shuzu {
      private int[] array;

      private Car[] cars;
  }
  ```
  ```java
  public class ArraysTest {

      public static void main(String[] args) {
          Class clz = Shuzu.class;

          Field[] fields = clz.getDeclaredFields();

          for ( Field f : fields ) {
              // 获取 Field 的类型
              Class c = f.getType();
              // 判断这个类型是不是数组类型
              if ( c.isArray()) {
                  System.out.println("Type is "+c.getName());
                  System.out.println("ComponentType type is :"+c.getComponentType());
              }
          }
      }
  }
  ```
  运行结果：
  ```
  Type is [I
  ComponentType type is :int
  Type is [Lcom.frank.test.Car;
  ComponentType type is :class com.frank.test.Car
  ```

- 在反射中创建数组可以通过 Array.newInstance() 方法。 
  ```java
  public static Object newInstance(Class<?> componentType, int... dimensions)
          throws IllegalArgumentException, NegativeArraySizeException {}
  ```
  第一个参数指定的是数组内的元素类型，后面的是可变参数，表示的是相应维度的数组长度限制。
  例：
  ```java
  Array.newInstance(int.class,2,3);
  ```

###### 2.3.3.2.2. 枚举类型的反射操作

在 Java 反射中，可以把枚举看成一般的 Class，但是反射机制也提供了 3 个特别的的 API 用于枚举类型的操作。
```java
// 用来判定 Class 对象是不是枚举类型
Class.isEnum()

// 获取所有的枚举常量
Class.getEnumConstants()

// 判断一个 Field 是不是枚举常量
java.lang.reflect.Field.isEnumConstant()
```

##### 2.3.3.3. 读取 / 修改 Field 值（使用 Field 类中的方法）

- Field 类中定义了一系列的 get 方法来读取不同类型的值
  - `Object get(Object obj);`
  - `int getInt(Object obj);`
  - `long getLong(Object obj) throws IllegalArgumentException, IllegalAccessException;`
  - `float getFloat(Object obj) throws IllegalArgumentException, IllegalAccessException;`
  - `short getShort(Object obj) throws IllegalArgumentException, IllegalAccessException;`
  - `double getDouble(Object obj) throws IllegalArgumentException, IllegalAccessException;`
  - `char getChar(Object obj) throws IllegalArgumentException, IllegalAccessException;`
  - `byte getByte(Object obj) throws IllegalArgumentException, IllegalAccessException;`
  - `boolean getBoolean(Object obj) throws IllegalArgumentException, IllegalAccessException`
- Field 类中定义了一系列的 set 方法来修改不同类型的值
  - `void set(Object obj, Object value);`
  - `void setInt(Object obj,int value);`
  - `void setLong(Object obj,long value) throws IllegalArgumentException, IllegalAccessException;`
  - `void setFloat(Object obj,float value) throws IllegalArgumentException, IllegalAccessException;`
  - `void setShort(Object obj,short value) throws IllegalArgumentException, IllegalAccessException;`
  - `void setDouble(Object obj,double value) throws IllegalArgumentException, IllegalAccessException;`
  - `void setChar(Object obj,char value) throws IllegalArgumentException, IllegalAccessException;`
  - `void setByte(Object obj,byte b) throws IllegalArgumentException, IllegalAccessException;`
  - `void setBoolean(Object obj,boolean b) throws IllegalArgumentException, IllegalAccessException;`

其中，Object 参数是类的实例引用。
- **Class 本身不对成员进行储存，它只提供检索，所以需要用 Field、Method、Constructor 对象来承载这些成员，所以，针对成员的操作时，为了精确定位，一般需要为成员指定类的实例引用。**

例：
```java
A testa = new A();
testa.a = 10;

System.out.println("testa.a = "+testa.a);

Class c = A.class;

try {
    Field fielda = c.getField("a");

    int ra = fielda.getInt(testa);

    System.out.println("reflection testa.a = "+ra);

    fielda.setInt(testa, 15);

    System.out.println("testa.a = "+testa.a);

} catch (NoSuchFieldException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
} catch (SecurityException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
} catch (IllegalArgumentException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
} catch (IllegalAccessException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
}
```

NOTE: 若在反射中访问了 private 修饰的成员，会抛出 IllegalAccessException 异常，可通过 `setAccessible(true)` 消除异常。

#### 2.3.4. 类的方法 Method

[Method](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/Method.html) 是反射机制最核心的内容，通常的反射都是为了调用某个 Method 的 invoke() 方法。

```java
public final class Method 
                      extends Executable
```

##### 2.3.4.1. 获取 Method 对象（使用 Class 类中的方法）

- `Method getMethod(String name, Class<?>... parameterTypes)`

- `Method getDeclaredMethod(String name, Class<?>... parameterTypes)`

- `Method[]	getMethods​() throws SecurityException`

- `Method[] getDeclaredMethods() throws SecurityException`

其中，parameterTypes 是方法对应的参数。

NOTE：
- getMethod() 和 getDeclaredMethod() 的能力范围差异：
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/9/345ed02fcaa2a51690388bdcee1dedbb.jpg)

##### 2.3.4.2. 获取方法要素（使用 Method 类中的方法）

一个方法签名由下面几个要素构成： 
- 方法名 
- 方法参数 
- 方法返回值 
- 方法的修饰符 
- 方法可能会抛出的异常

- 获取方法名：`String	getName​()`

  ```java
  public class MethodTest {

      public static void main(String[] args) {
          // TODO Auto-generated method stub
          Car car = new Car();

          Class clz = car.getClass();

          Method methods[] = clz.getDeclaredMethods();

          for ( Method m : methods ) {
              System.out.println("method name:"+m.getName());
          } 
      }

  }
  ```

- 获取方法参数
  - `Parameter[] getParameters()`: 返回的是一个 Parameter 数组，在反射中 Parameter 对象就是用来映射方法中的参数：
    ```java
    // Parameter.java
    // 获取参数名字
    public String getName() {}

    // 获取参数类型
    public Class<?> getType() {}

    // 获取参数的修饰符
    public int getModifiers() {}
    ```

  - `Class<?>[] getParameterTypes()`: 获取所有的参数类型
  - `Type[] getGenericParameterTypes()`: 获取所有的参数类型，包括泛型

- 获取返回值类型
  - `Class<?> getReturnType()`: 获取返回值类型
  - `Type getGenericReturnType()`: 获取返回值类型包括泛型

- 获取修饰符：`int getModifiers()`

- 获取异常类型
  - `Class<?>[] getExceptionTypes()`
  - `Type[] getGenericExceptionTypes()`

##### 2.3.4.3. 执行方法（使用 Method 类的方法）

这是整个反射机制的核心内容，很多时候运用反射的目的其实就是为了以非常规手段执行 Method。

```java
Object invoke(Object obj, Object... args) {}
```

NOTE
- 第一个参数 Object 实质上是 Method 所依附的 Class 对应的类的实例，如果这个方法是一个静态方法，那么 ojb 为 null。
- 第二个参数是可变参数，对应的是调用方法时要传递的参数。
- 返回的对象是 Object，所以实际上执行的时候要进行强制转换。
- 在对 Method 调用 invoke() 的时候，如果方法本身会抛出异常，那么这个异常就会经过包装，由 Method 统一抛出 InvocationTargetException，而通过 InvocationTargetException.getCause() 可以获取真正的异常。

#### 2.3.5. 类的构造方法 Constructor

Constructor 同 Method 差不多，但是它特别的地方在于，它能够创建一个对象。

##### 2.3.5.1. 获取 Constructor 对象（使用 Class 类的方法）

- `Constructor<T> getConstructor(Class<?>... parameterTypes)`

- `Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)`

- `Constructor<?>[] getConstructors() throws SecurityException`

- `Constructor<?>[] getDeclaredConstructors() throws SecurityException`

其中，parameterTypes 参数用于获取指定签名的构造器。

NOTE：
- 由于 Constructor 不能从父类继承，因此无法通过 getConstructor() 获取到父类的 Constructor。
 
- getConstructor() 和 getDeclaredConstructor() 的能力范围差异：
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/9/0ced8645a316f8be0cd243e43b727b7f.jpg)

##### 2.3.5.2. 创建类的实例对象（使用 Constructor 类的方法）

在 Java 反射机制中有两种方法可以用来创建类的对象实例：`Class.newInstance()` 和 `Constructor.newInstance()`。

官方文档建议开发者使用 `Constructor.newInstance()`，理由如下：
- `Class.newInstance()` 只能调用无参的构造方法，而 `Constructor.newInstance()` 则可以调用任意的构造方法。
- `Class.newInstance()` 通过构造方法直接抛出异常，而 `Constructor.newInstance()` 会把抛出来的异常包装到 InvocationTargetException 里面去，这个和 Method 行为一致。
- `Class.newInstance()` 要求构造方法能够被访问，而 `Constructor.newInstance()` 却能够访问 private 修饰的构造器。

## 3. 反射异常

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/9/26f0948b5f3758d4bcbeffd726ef3c18.jpg)

## 4. 应用

### 4.1. 绕过安全机制

- 成员权限检查绕过：获取 private 修饰的 Field、Method、Constructor。

- final 检查绕过：改变 final 属性的 Field。

- 集合泛型检查绕过
  
  集合的泛型只是用于防止错误输入，即只在编译阶段有效；若使用方法的反射操作绕过编译检查，可以使集合的泛型限制失效。
  
  ```java
  ArrayList<String> al = new ArrayList<>();
  Class c = al.getClass();
  Method m = c.getMethod("add", Object.class);
  m.invoke(al, 20);// 绕过集合泛型的编译时检查，添加非 String 元素
  System.out.println(al);
  ```

### 4.2. 实现 JDK 动态代理

#### 4.2.1. 基本概念

##### 4.2.1.1. 代理模式

代理模式是面向对象编程中比较常见的设计模式。

举例来说，顾客可以直接从厂家购买产品，但是现实生活中，很少有这样的销售模式。一般都是厂家委托给代理商进行销售，顾客跟代理商打交道，而不直接与产品实际生产者进行关联。在这个过程中，代理商就起着代理的作用。

代理模式可以在不修改被代理对象的基础上，通过扩展代理类，进行一些功能的附加与增强。值得注意的是，代理类和被代理类应该共同实现一个接口，或者是共同继承某个类。

代理可以分为静态代理和动态代理两种。

##### 4.2.1.2. 静态代理

举例说明：我们平常去电影院看电影的时候，在电影开始的阶段经常会放广告。电影是电影公司委托给影院进行播放的，但是影院可以在播放电影的时候，产生一些自己的经济收益，比如卖爆米花、可乐等，然后在影片开始结束时播放一些广告。这个过程中，电影公司是被代理者，电影院是代理。

```java
// 通用的接口是代理模式实现的基础
// Movie，代表电影播放的能力
public interface Movie {
    void play();
}

// 表示一部真正的影片。它实现了 Movie 接口，play() 方法调用时，影片就开始播放
public class RealMovie implements Movie {
    @Override
    public void play() {
        // TODO Auto-generated method stub
        System.out.println("您正在观看电影 《肖申克的救赎》");
    }
}

// Cinema 就是 Proxy 代理对象，它有一个 play() 方法。不过调用 play() 方法时，它进行了一些相关利益的处理，那就是广告
public class Cinema implements Movie {
    RealMovie movie;

    public Cinema(RealMovie movie) {
        super();
        this.movie = movie;
    }

    @Override
    public void play() {
        guanggao(true);
        movie.play();
        guanggao(false);
    }

    public void guanggao(boolean isStart){
        if ( isStart ) {
            System.out.println("电影马上开始了，爆米花、可乐、口香糖 9.8 折，快来买啊！");
        } else {
            System.out.println("电影马上结束了，爆米花、可乐、口香糖 9.8 折，买回家吃吧！");
        }
    }
}

// 编写测试代码
public class ProxyTest {
    public static void main(String[] args) {
        RealMovie realmovie = new RealMovie();
        Movie movie = new Cinema(realmovie);
        movie.play();
    }
}
```
运行结果：
```
电影马上开始了，爆米花、可乐、口香糖 9.8 折，快来买啊！
您正在观看电影 《肖申克的救赎》
电影马上结束了，爆米花、可乐、口香糖 9.8 折，买回家吃吧！
```

上面介绍的是静态代理的内容，为什么叫做静态代理呢？因为它的类型是事先预定好，硬编码到代码中去的，比如上面代码中的 Cinema 这个类。

##### 4.2.1.3. 动态代理

在静态代理的示例代码中，Cinema 类是代理，我们需要手动编写代码让 Cinema 实现 Movie 接口，而在动态代理中，我们可以让程序在运行的时候自动在内存中创建一个实现 Movie 接口的代理，而不需要去定义 Cinema 这个类。这就是它被称为动态的原因。

#### 4.2.2. JDK 动态代理实现方法

在 Java 的 java.lang.reflect 包下提供了一个 [Proxy](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/Proxy.html) 类和一个 [InvocationHandler](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/InvocationHandler.html) 接口，通过使用这个类和接口，就可以生成 JDK 动态代理类或动态代理对象。

Proxy 类是所有动态代理类的父类，它提供了用于创建动态代理类和动态代理对象的静态方法：
- `static Class<?>	getProxyClass​(ClassLoader loader, Class<?>... interfaces)`: 创建一个动态代理类，返回类所对应的 Class 对象，但该方法官方文档不推荐使用（Constructor.newInstance will throw IllegalAccessException when it is called on an inaccessible proxy class. Use newProxyInstance(ClassLoader, Class[], InvocationHandler) to create a proxy instance instead）。

- `static Object	newProxyInstance​(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)`: 直接创建一个动态代理对象。
  
  参数说明：
  - 第一个参数指定该代理对象的类加载器。
  - 第二个参数指定该代理对象要实现的多个接口，JDK 动态代理只能为接口创建动态代理。
  - 第三个参数指定该代理对象的 InvocationHandler 对象，执行代理对象的每个方法时都会被替换成执行 InvocationHandler 对象的 invoke 方法。
    - `Object invoke​(Object proxy, Method method, Object[] args) throws Throwable`: 调用代理对象的所有方法时都会被替换成调用该 invoke 方法并返回结果。
      
      参数说明：
      - proxy：动态代理的对象实例。
      - method：正在调用的方法。
      - args：正在调用的方法所传入的实参数组。

例：假设有一个大商场，商场有很多的柜台，有一个柜台卖茅台酒。
```java
// SellWine 是一个接口，你可以理解它为卖酒的许可证
public interface SellWine {
     void mainJiu();
}

// 茅台酒
public class MaotaiJiu implements SellWine {
    @Override
    public void mainJiu() {
        // TODO Auto-generated method stub
        System.out.println("我卖得是茅台酒。");
    }
}

// 五粮液
public class Wuliangye implements SellWine {
    @Override
    public void mainJiu() {
        // TODO Auto-generated method stub
        System.out.println("我卖得是五粮液。");
    }
}

// 我们还需要一个柜台来卖酒，这个柜台就是动态代理
public class GuitaiA implements InvocationHandler {

    private Object pingpai;
    
    public GuitaiA(Object pingpai) {
        this.pingpai = pingpai;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args)
            throws Throwable {
        // TODO Auto-generated method stub
        System.out.println("销售开始  柜台是： "+this.getClass().getSimpleName());
        method.invoke(pingpai, args);
        System.out.println("销售结束");
        return null;
    }
}

// 编写测试代码
public class Test {
    public static void main(String[] args) {
        // TODO Auto-generated method stub

        MaotaiJiu maotaijiu = new MaotaiJiu();
        Wuliangye wu = new Wuliangye();

        InvocationHandler jingxiao1 = new GuitaiA(maotaijiu);
        InvocationHandler jingxiao2 = new GuitaiA(wu);

        SellWine dynamicProxy = (SellWine) Proxy.newProxyInstance(MaotaiJiu.class.getClassLoader(),
                MaotaiJiu.class.getInterfaces(), jingxiao1);
        SellWine dynamicProxy1 = (SellWine) Proxy.newProxyInstance(MaotaiJiu.class.getClassLoader(),
                MaotaiJiu.class.getInterfaces(), jingxiao2);
        dynamicProxy.mainJiu();
        dynamicProxy1.mainJiu();
    }

}
```
运行结果：
```
销售开始  柜台是： GuitaiA
我卖得是茅台酒。
销售结束
销售开始  柜台是： GuitaiA
我卖得是五粮液。
销售结束
```

#### 4.2.3. 动态代理与 AOP

实际上，在普通编程中，确实很少会使用到动态代理，但在编写框架或底层基础代码时，动态代理的作用十分强大。

开发软件时，经常会遇到存在相同代码段重复出现的情况，在这种情况下，可通过将这些代码不断地复制粘贴，快速实现软件功能。但采用这样的方法，若在维护时需要修改这部分重复的代码，就需要在每一处粘贴的地方进行修改，工作量巨大。

因此，可以将这部分重复代码封装成一个方法，再在每处用到该代码段的地方调用该方法即可。通过这种方式，大大降低了后期维护的复杂度，但在每处用到该代码段的地方硬编码调用特定的方法，又产生了新的耦合。**最理想的效果应是：在每处需要用到该代码段的地方即可以执行该代码段，又无需在程序中以硬编码的方式直接调用特定的封装方法。**这时就可以使用动态代理来实现。

例：
```java
public interface Dog {
  void info();
  void run();
}

public class GunDog implements Dog {
  @Override
  public void info() {
    System.out.println("我是一只猎狗");
  }

  @Override
  public void run() {
    System.out.println("我奔跑迅速");
  }
}

// 通用工具类，包含了通用方法
public class DogUtil {
  public static void method1() {
    System.out.println("执行第一个通用方法");
  }

  public static void method2() {
    System.out.println("执行第二个通用方法");
  }
}
```
现在的要实现的功能是：当程序执行 info() 和 run() 方法时，能够调用 method1() 和 method2() 两个通用方法，即自动将 method1() 和 method2() 两个通用方法插入到 info() 和 run() 方法中执行，但又不能以硬编码的方式在 info() 和 run() 的实现代码中调用这两个方法。
```java
public class MyInvocationHandler implements InvocationHandler {
  // 需要被代理的对象
  private Object target;

  public void setTarget(Objecr target) {
    this.target = target;
  }

  @Override
  public object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    DogUtil.method1();
    // 通过反射以 target 作为主调来执行 target 对象的原有的方法 method
    Object result = method.invoke(this.target, args);
    DogUtil.method2();
    
    return result;
  }
}

// 代理工厂类，专为指定的 target 生成动态代理实例
public class MyProxyFactory {
  // 为指定的 target 生成动态代理实例
  public static Object getProxy(Object target) throws Exception {
    MyInvocationHandler handler = new MyInvocationHandler();
    handler.setTarget(target);
    // 创建并返回一个动态代理对象
    return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), handler);
  }
}

// 编写测试代码
public class Test {
  public static void main(String [] args) throws Exception {
    Dog gunDog = new GunDog();
    Dog proxyDog = (Dog)MyProxyFactory.getProxy(gunDog);
    proxyDog.info();
    proxyDog.run();
  }
}
```

通过动态代理，可以非常灵活的实现解耦。一般情况下，使用 Proxy 生成一个动态代理时，往往并不会凭空产生一个动态代理，这样没有太大的实际意义，通常都是为指定的目标对象生成动态代理。

这种动态代理在 AOP 中也称为 AOP 代理，AOP 代理里的方法可以在执行目标方法之前、之后插入一些通用的处理代码。

### 4.3. 使用泛型 Class 避免强制类型转换

JDK 5 之后，Java 允许使用泛型来限制 Class 类，例如`String.class`的类型实际上是`Class<String>`。通过在反射中使用泛型，可以避免使用反射生成的对象时需要进行不安全的强制类型转换。

```java
public class MyObjectFactory {
  public static Object getInstance(String clsName) {
    try {
      Class cls = Class.forName(clsName);
      return cls.newInstance();
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    }
  }

  public static void main(String [] args) {
    Date d = (Date)MyObjectFactory.getInstance("java.util.Date");//1
    JFrame j = (JFrame)MyObjectFactory.getInstance("java.util.Date");//2
  }
}
```
上述代码在编译时都不会产生任何问题，但在运行时第 2 行代码将会抛出 ClassCastException 异常。

如果将上边代码改写为使用泛型后的 Class，就可以在编译阶段进行严格的类型检查，从而无需使用强制类型转换，避免发生这种运行时异常：
```java
public class MyObjectFactory {
  public static <T> T getInstance(Class<T> cls) {
    try {
      return cls.newInstance();
    } catch (Exception e) {
      e.printStackTrace();
      return null;
    }
  }

  public static void main(String [] args) {
    Date d = (Date)MyObjectFactory.getInstance(Date.class);// 正常运行
    JFrame j1 = (JFrame)MyObjectFactory.getInstance(JFrame.class);// 正常运行
    JFrame j2 = (JFrame)MyObjectFactory.getInstance(Date.class);// 编译时就会报错
  }
}
```

## 5. Refer Links

[慕课网：反射——Java 高级开发必须懂的](https://www.imooc.com/learn/199)

[细说反射，Java 和 Android 开发者必须跨越的坎](http://blog.csdn.net/briblue/article/details/74616922)

[反射进阶，编写反射代码值得注意的诸多细节](http://blog.csdn.net/briblue/article/details/76223206)

[轻松学，Java 中的代理模式及动态代理](http://blog.csdn.net/briblue/article/details/73928350)

[深入理解 Java 类型信息 (Class 对象) 与反射机制](http://blog.csdn.net/javazejian/article/details/70768369) 