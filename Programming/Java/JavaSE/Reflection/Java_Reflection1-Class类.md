- [Java 反射 - Class 类](#java-反射---class-类)
	- [1. 反射的定义](#1-反射的定义)
	- [2. Class 类的定义](#2-class-类的定义)
	- [3. Class 对象的获取](#3-class-对象的获取)
		- [3.1. 通过 .class](#31-通过-class)
		- [3.2. 通过 Object.getClass()](#32-通过-objectgetclass)
		- [3.3. 通过 Class.forName()](#33-通过-classforname)
		- [3.4. NOTE](#34-note)
	- [4. Class 对象的操作](#4-class-对象的操作)
		- [4.1. 类名](#41-类名)
		- [4.2. 类的修饰符](#42-类的修饰符)
		- [4.3. 类的成员变量 Field 类](#43-类的成员变量-field-类)
			- [4.3.1. 获取 Field 对象（使用 Class 类中的方法）](#431-获取-field-对象使用-class-类中的方法)
			- [4.3.2. 获取 Field 类型（使用 Field 类中的方法）](#432-获取-field-类型使用-field-类中的方法)
				- [4.3.2.1. 数组类型的反射操作](#4321-数组类型的反射操作)
				- [4.3.2.2. 枚举类型的反射操作](#4322-枚举类型的反射操作)
			- [4.3.3. 读取 / 修改 Field 值（使用 Field 类中的方法）](#433-读取--修改-field-值使用-field-类中的方法)
		- [4.4. 类的方法 Method 类](#44-类的方法-method-类)
			- [4.4.1. 获取 Method 对象（使用 Class 类中的方法）](#441-获取-method-对象使用-class-类中的方法)
			- [4.4.2. 获取方法要素（使用 Method 类中的方法）](#442-获取方法要素使用-method-类中的方法)
			- [4.4.3. 执行方法（使用 Method 类的方法）](#443-执行方法使用-method-类的方法)
		- [4.5. 类的构造方法 Constructor 类](#45-类的构造方法-constructor-类)
			- [4.5.1. 获取 Constructor 对象（使用 Class 类的方法）](#451-获取-constructor-对象使用-class-类的方法)
			- [4.5.2. 创建类的实例对象（使用 Constructor 类的方法）](#452-创建类的实例对象使用-constructor-类的方法)
	- [5. 反射操作常见的异常](#5-反射操作常见的异常)
	- [6. Refer Links](#6-refer-links)

# Java 反射 - Class 类

## 1. 反射的定义

[官网定义](https://docs.oracle.com/javase/tutorial/reflect/)：

> Reflection is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine. This is a relatively advanced feature and should be used only by developers who have a strong grasp of the fundamentals of the language. With that caveat in mind, reflection is a powerful technique and can enable applications to perform operations which would otherwise be impossible.

反射技术通常被用来检测和改变应用程序在 JVM 中的运行阶段的行为表现。它是一个相对而言比较高级的技术，通常它应用的前提是开发者本身对于 Java 语言特性有很强的理解的基础上。值得说明的是，反射是一种强有力的技术特性，因此可以使得应用程序突破一些藩篱，执行一些常规手段无法企及的目的。

Java 反射机制可以让我们在编译期 (Compile Time) 之外的**运行期**(Runtime) 检查类、接口、变量以及方法的信息，还可以让我们在**运行期**实例化对象，调用方法，通过调用 get/set 方法获取变量的值。

**反射属于非常规的开发手段，由于是在运行期进行本应在编译期完成的操作，它抛弃了 JVM 的很多优化，因此往往同样功能的代码，反射要比正常方式执行要慢，所以采用反射时，要考虑它的时间成本**。

## 2. Class 类的定义

> Instances of the class Class represent classes and interfaces in a running Java application. An enum is a kind of class and an annotation is a kind of interface. Every array also belongs to a class that is reflected as a Class object that is shared by all arrays with the same element type and number of dimensions. The primitive Java types (boolean, byte, char, short, int, long, float, and double), and the keyword void are also represented as Class objects.

java.lang.[Class](https://docs.oracle.com/javase/9/docs/api/java/lang/Class.html) 类是 Java 反射机制的基础和入口，在 Java 中的所有类都是 Class 类的实例对象（也称为类的“类类型”），通过 Class 类我们可以获得关于一个类的相关信息。

在 Java 中每个类都有一个 Class 对象，**每当我们编写并且编译一个新创建的类就会产生一个对应 Class 对象并且这个 Class 对象会被保存在同名的 `.class` 文件里（编译后的字节码文件保存的就是 Class 对象)**，那为什么需要这样一个 Class 对象呢？是这样的，当我们 new 一个新对象或者引用静态成员变量时，JVM 中的类加载器子系统会将对应 Class 对象加载到 JVM 中，然后 JVM 再根据这个类型信息相关的 Class 对象创建我们需要实例对象或者提供静态变量的引用值。需要特别注意的是，手动编写的**每个 class 类，无论创建多少个实例对象，在 JVM 中都只有一个 Class 对象，即在内存中每个类有且只有一个相对应的 Class 对象**。

```java
public final class Class<T> extends Object
                            implements java.io.Serializable,
                                GenericDeclaration,
                                Type,
                                AnnotatedElement {}
```

## 3. Class 对象的获取

> Class has no public constructor. Instead Class objects are constructed automatically by the Java Virtual Machine as classes are loaded and by calls to the defineClass method in the class loader.

一个正在运行的 Java 程序中的类或接口，都是一个 Class 类的实例对象。Class 没有 public 的构造方法，也就是说**没有办法像创建一个类一样通过 new 关键字来获取一个 Class 对象，只有在类加载时 JVM 才会自动的直接创建 Class 的实例对象**，但可以通过以下 3 种方式间接获取 Class 类的实例对象：

### 3.1. 通过 .class

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

### 3.2. 通过 Object.getClass()

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

### 3.3. 通过 Class.forName()

通过 `Class.forName()` 获取类的类类型。

`Class.forName()` 的参数是指定类的全限定名称（包括包名 + 类名），**该方法会到 JVM 中去寻找指定的类有没有被加载，如果没有找到指定的类，会抛出 ClassNotFoundException 异常；如果查找的类没有在 JVM 中加载，则会加载该类（动态加载）**。

```java
try {
    Class clz = Class.forName("com.demo.test.Car");
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}
```

### 3.4. NOTE

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

- **编译时加载类称为静态加载类，运行时加载类称为动态加载类**。
  - 使用 new 创建类对象属于静态加载类，在编译时就需要加载所有可能使用到的类。
  - 使用 `Class.forName()` 获取类的类类型属于动态加载类，JVM 会在运行阶段才加载所需要的类。

## 4. Class 对象的操作

获取到类的 Class 对象后，这正是反射机制中开始的地方，我们运用反射的目的就是为了获取和操控 Class 对象中的各种成员。

### 4.1. 类名

相关 API:

- `Class.getName();`: Returns the name of the entity (class, interface, array class, primitive type, or void) represented by this Class object, as a String.
  - 当 Class 对象是一个引用时，该方法返回的是一个二进制形式的字符串，比如“com.demo.test.Car”。
  - 当 Class 对象是一个基本数据类型时，该方法返回的是它们的关键字，比如 int.class 的名字是 int。
  - 当 Class 对象是一个基础数据类型的数组时，Java 本身对于这一块制定了相应规则，在元素的类型前面添加相应数量的 [ 符号，用 [ 的个数来提示数组的维度，并且值得注意的是，对于基本类型或者是类，都有相应的编码，所谓的编码大多数是用一个大写字母来指示某种类型，规则如下：

    ![image](http://img.cdn.firejq.com/jpg/2018/3/9/662c3ae9e2e927ff69821495729562db.jpg)

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

### 4.2. 类的修饰符

通常，在 Java 中定义一个类，往往是要通过许多修饰符来配合使用的，它们大致分为 4 类。
- 用来限制作用域，如 `public`、`protected`、`priviate`。
- 用来提示子类复写，`abstract`。
- 用来标记为静态类 `static`。
- 注解 `@interface`。

Java 反射提供了 API 去获取这些修饰符：
> `int getModifiers​()`: Returns the Java language modifiers for this class or interface, encoded in an integer.

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

### 4.3. 类的成员变量 Field 类

https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/Field.html

```java
public final class Field
                    extends AccessibleObject
                    implements Member
```

#### 4.3.1. 获取 Field 对象（使用 Class 类中的方法）

- `Field getField(String name)`: 获取指定名字的属性，该方法获取的是非私有属性，并且在当前 Class 获取不到时会向祖先类获取。
- `Field getDeclaredField(String name)`: 获取指定名字的属性，该方法获取的是被 private 修饰的属性，若在当前 Class 获取不到时不会向祖先类获取。
- `Field[] getFields()`: 获取自身的所有的 public 属性，包括从父类继承下来的。
- `Field[] getDeclaredFields()`: 获取所有的属性，但不包括从父类继承下来的属性。

NOTE：
- `getField()` 方法和 `getDeclaredField()` 方法的能力范围：

  ![image](http://img.cdn.firejq.com/jpg/2018/3/9/2a303b5e3a59d4fdc927e62983430242.jpg)

- 如何获取一个 Class 中继承下来的非 public 修饰的 Field？

  通过获取这个 Class 的 superClass，然后调用这个 superClass 的 `getDeclaredField()` 方法。

#### 4.3.2. 获取 Field 类型（使用 Field 类中的方法）

- `public Type getGenericType()`：能够获取泛型类型，Type 是 Class 所实现的接口。
- `public Class<?> getType()`：不能获取泛型类型。

##### 4.3.2.1. 数组类型的反射操作

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

          for (Field f : fields) {
              // 获取 Field 的类型
              Class c = f.getType();
              // 判断这个类型是不是数组类型
              if (c.isArray()) {
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

- 在反射中创建数组可以通过 `Array.newInstance()` 方法。
  ```java
  public static Object newInstance(Class<?> componentType, int... dimensions)
          throws IllegalArgumentException, NegativeArraySizeException {}
  ```
  第一个参数指定的是数组内的元素类型，后面的是可变参数，表示的是相应维度的数组长度限制。

	eg:
  ```java
  Array.newInstance(int.class,2,3);
  ```

##### 4.3.2.2. 枚举类型的反射操作

在 Java 反射中，可以把枚举看成一般的 Class，但是反射机制也提供了 3 个特别的的 API 用于枚举类型的操作。
```java
// 用来判定 Class 对象是不是枚举类型
Class.isEnum()

// 获取所有的枚举常量
Class.getEnumConstants()

// 判断一个 Field 是不是枚举常量
java.lang.reflect.Field.isEnumConstant()
```

#### 4.3.3. 读取 / 修改 Field 值（使用 Field 类中的方法）

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
- **Class 本身不对成员进行储存，它只提供检索，所以需要用 Field 类、Method 类、Constructor 类的实例对象来承载这些成员，所以，针对成员的操作时，为了精确定位，一般需要为成员指定类的实例引用。**

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

### 4.4. 类的方法 Method 类

[Method](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/Method.html) 是反射机制最核心的内容，通常的反射都是为了调用某个 Method 的 invoke() 方法。

```java
public final class Method
                      extends Executable
```

#### 4.4.1. 获取 Method 对象（使用 Class 类中的方法）

- `Method getMethod(String name, Class<?>... parameterTypes)`

- `Method getDeclaredMethod(String name, Class<?>... parameterTypes)`

- `Method[]	getMethods​() throws SecurityException`

- `Method[] getDeclaredMethods() throws SecurityException`

其中，parameterTypes 是方法对应的参数。

NOTE：
- getMethod() 和 getDeclaredMethod() 的能力范围差异：

  ![image](http://img.cdn.firejq.com/jpg/2018/3/9/345ed02fcaa2a51690388bdcee1dedbb.jpg)

#### 4.4.2. 获取方法要素（使用 Method 类中的方法）

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

#### 4.4.3. 执行方法（使用 Method 类的方法）

这是整个反射机制的核心内容，很多时候运用反射的目的其实就是为了以非常规手段执行 Method。

```java
Object invoke(Object obj, Object... args) {}
```

NOTE
- 第一个参数 Object 实质上是 Method 所依附的 Class 对应的类的实例，如果这个方法是一个静态方法，那么 obj 为 null。
- 第二个参数是可变参数，对应的是调用方法时要传递的参数。
- 返回的对象是 Object，所以实际上执行的时候要进行强制转换。
- 在对 Method 调用 invoke() 的时候，如果方法本身会抛出异常，那么这个异常就会经过包装，由 Method 统一抛出 InvocationTargetException，而通过 InvocationTargetException.getCause() 可以获取真正的异常。

### 4.5. 类的构造方法 Constructor 类

Constructor 同 Method 差不多，但是它特别的地方在于，它能够创建一个对象。

#### 4.5.1. 获取 Constructor 对象（使用 Class 类的方法）

- `Constructor<T> getConstructor(Class<?>... parameterTypes)`

- `Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)`

- `Constructor<?>[] getConstructors() throws SecurityException`

- `Constructor<?>[] getDeclaredConstructors() throws SecurityException`

其中，parameterTypes 参数用于获取指定签名的构造器。

NOTE：
- 由于 Constructor 不能从父类继承，因此无法通过 getConstructor() 获取到父类的 Constructor。

- getConstructor() 和 getDeclaredConstructor() 的能力范围差异：

  ![image](http://img.cdn.firejq.com/jpg/2018/3/9/0ced8645a316f8be0cd243e43b727b7f.jpg)

#### 4.5.2. 创建类的实例对象（使用 Constructor 类的方法）

在 Java 反射机制中有两种方法可以用来创建类的对象实例：`Class.newInstance()` 和 `Constructor.newInstance()`。

但在官方文档中建议开发者使用 `Constructor.newInstance()` 来创建一个类的实例对象，理由如下：
- `Class.newInstance()` 只能调用无参的构造方法，而 `Constructor.newInstance()` 则可以调用任意的构造方法。
- `Class.newInstance()` 通过构造方法直接抛出异常，而 `Constructor.newInstance()` 会把抛出来的异常包装到 InvocationTargetException 里面去，这个和 Method 行为一致。
- `Class.newInstance()` 要求构造方法能够被访问，而 `Constructor.newInstance()` 却能够访问 private 修饰的构造器。

## 5. 反射操作常见的异常

![image](http://img.cdn.firejq.com/jpg/2018/3/9/26f0948b5f3758d4bcbeffd726ef3c18.jpg)

## 6. Refer Links

[慕课网：反射——Java 高级开发必须懂的](https://www.imooc.com/learn/199)

[细说反射，Java 和 Android 开发者必须跨越的坎](http://blog.csdn.net/briblue/article/details/74616922)

[反射进阶，编写反射代码值得注意的诸多细节](http://blog.csdn.net/briblue/article/details/76223206)

[轻松学，Java 中的代理模式及动态代理](http://blog.csdn.net/briblue/article/details/73928350)

[深入理解 Java 类型信息 (Class 对象) 与反射机制](http://blog.csdn.net/javazejian/article/details/70768369)