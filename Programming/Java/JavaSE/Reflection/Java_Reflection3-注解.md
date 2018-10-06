- [Java 注解](#java-%E6%B3%A8%E8%A7%A3)
    - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [1.1. 定义](#11-%E5%AE%9A%E4%B9%89)
        - [1.2. 分类](#12-%E5%88%86%E7%B1%BB)
    - [2. JDK 注解](#2-jdk-%E6%B3%A8%E8%A7%A3)
        - [2.1. 基本注解](#21-%E5%9F%BA%E6%9C%AC%E6%B3%A8%E8%A7%A3)
            - [2.1.1. 限定重写父类方法 @Override](#211-%E9%99%90%E5%AE%9A%E9%87%8D%E5%86%99%E7%88%B6%E7%B1%BB%E6%96%B9%E6%B3%95-override)
            - [2.1.2. 标示已过时 @Deprecated](#212-%E6%A0%87%E7%A4%BA%E5%B7%B2%E8%BF%87%E6%97%B6-deprecated)
            - [2.1.3. 抑制编译器警告 @SuppressWarnings](#213-%E6%8A%91%E5%88%B6%E7%BC%96%E8%AF%91%E5%99%A8%E8%AD%A6%E5%91%8A-suppresswarnings)
            - [2.1.4. 堆污染”警告 @SafeVarargs](#214-%E5%A0%86%E6%B1%A1%E6%9F%93%E2%80%9D%E8%AD%A6%E5%91%8A-safevarargs)
            - [2.1.5. 函数式接口 @FunctionalInterface](#215-%E5%87%BD%E6%95%B0%E5%BC%8F%E6%8E%A5%E5%8F%A3-functionalinterface)
        - [2.2. 元注解](#22-%E5%85%83%E6%B3%A8%E8%A7%A3)
            - [2.2.1. @Retention](#221-retention)
            - [2.2.2. @Target](#222-target)
            - [2.2.3. @Documented](#223-documented)
            - [2.2.4. @Inherited](#224-inherited)
            - [2.2.5. @Repeatable](#225-repeatable)
    - [3. 自定义注解](#3-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%B3%A8%E8%A7%A3)
        - [3.1. 注解的定义](#31-%E6%B3%A8%E8%A7%A3%E7%9A%84%E5%AE%9A%E4%B9%89)
            - [3.1.1. @interface](#311-interface)
            - [3.1.2. 成员属性](#312-%E6%88%90%E5%91%98%E5%B1%9E%E6%80%A7)
        - [3.2. 注解的解析](#32-%E6%B3%A8%E8%A7%A3%E7%9A%84%E8%A7%A3%E6%9E%90)
            - [3.2.1. 相关接口](#321-%E7%9B%B8%E5%85%B3%E6%8E%A5%E5%8F%A3)
            - [3.2.2. 一般步骤](#322-%E4%B8%80%E8%88%AC%E6%AD%A5%E9%AA%A4)
    - [4. 应用实例](#4-%E5%BA%94%E7%94%A8%E5%AE%9E%E4%BE%8B)
        - [4.1. 自定义简单测试注解](#41-%E8%87%AA%E5%AE%9A%E4%B9%89%E7%AE%80%E5%8D%95%E6%B5%8B%E8%AF%95%E6%B3%A8%E8%A7%A3)
        - [4.2. JUnit](#42-junit)
        - [4.3. Retrofit](#43-retrofit)
    - [5. Refer Links](#5-refer-links)

# Java 注解

## 1. 基本概念

### 1.1. 定义

从 Java SE 5.0 版本开始，Java 增加了对元数据的支持，也就是注解 (Annotation)。

**注解属于 Java 反射技术中的一部分**，是代码中的特殊标记，这些标记可以在编译、类加载、运行时被读取，并执行相应的处理。通过使用注解，开发人员可以在不改变原有逻辑的情况下，在源文件中嵌入一些补充的信息。

注解是一系列元数据，它提供数据用来解释程序代码，但是**注解并非是所解释的代码本身的一部分，对于代码的运行效果没有直接影响，主要针对的是编译器和其它工具软件 (SoftWare tool)**。

### 1.2. 分类

- 按运行机制分:
  - 源码注解：注解只在源码中存在，编译成 `.class` 文件后就不再存在。
  - 编译时注解：注解在源码和 `.class` 文件中都存在，如 @Override、@Deprecated、@SuppressWarnings。
  - 运行时注解：在运行阶段起作用，会影响运行逻辑，如 Spring 中的 `@Autowired`。

- 按来源分:
  - JDK 注解
  - 第三方注解
  - 自定义注解

## 2. JDK 注解

JDK 在 `java.lang` 包中提供了 5 个基本的注解，在 `java.lang.annotation` 包中提供了 6 个元注解。

### 2.1. 基本注解

#### 2.1.1. 限定重写父类方法 @Override

@Override 用于限定重写父类方法，它会告诉编译器要检查应该了该注解的方法，保证父类中要包含一个被该方法重写的方法，否则就产生编译错误。@Override 主要用于帮助程序员避免一些低级的错误。

#### 2.1.2. 标示已过时 @Deprecated

@Deprecated 用来标记过时的元素。编译器在编译阶段遇到这个注解时会发出提醒警告，告诉开发者正在调用一个过时的元素比如过时的方法、过时的类、过时的成员变量。

```java
public class Hero {

    @Deprecated
    public void say() {
        System.out.println("Noting has to say!");
    }

    public void speak() {
        System.out.println("I have a dream!");
    }

}
```

#### 2.1.3. 抑制编译器警告 @SuppressWarnings

@SuppressWarnings 用于抑制编译器警告。调用被 @Deprecated 注解的方法后，编译器会警告提醒，而有时候开发者会忽略这种警告，他们可以在调用的地方通过 @SuppressWarnings 达到目的。

```java
@SuppressWarnings("deprecation")
public void test1(){
    Hero hero = new Hero();
    hero.say();
    hero.speak();
}
```

NOTE: 使用 @SuppressWarnings 时，必须要在括号中为该注解的成员变量赋值。

#### 2.1.4. 堆污染”警告 @SafeVarargs

TODO:

当把一个不带泛型的对象赋值给一个带泛型的变量时，就会发生“堆污染”。

@SafeVarargs 是参数安全类型注解，从 Java7 开始加入 JDK。它的目的是提醒开发者不要用参数做一些不安全的操作，它的存在会阻止编译器产生 unchecked 这样的警告。

```java
@SafeVarargs // Not actually safe!
static void m(List<String>... stringLists) {
    Object[] array = stringLists;
    List<Integer> tmpList = Arrays.asList(42);
    array[0] = tmpList; // Semantically invalid, but compiles without warnings
    String s = stringLists[0].get(0); // Oh no, ClassCastException at runtime!
}
```
上面的代码中，编译阶段不会报错，但是运行时会抛出 ClassCastException 这个异常。

#### 2.1.5. 函数式接口 @FunctionalInterface

Java8 中规定：如果一个接口中只有一个抽象方法（可以包含多个默认方法或多个 static 方法），该接口就是函数式接口。

@FunctionalInterface 是函数式接口注解，从 Java8 开始加入 JDK。@FunctionalInterface 让编译器检查某个接口，保证该接口必须是一个函数式接口，否则就产生编译错误，从而避免程序员出现一些低级错误。

eg: 我们进行线程开发中常用的 Runnable 就是一个典型的函数式接口，上面源码可以看到它就被 @FunctionalInterface 注解。
```java
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```
可能有人会疑惑，函数式接口标记有什么用？函数式接口可以很容易转换为 Lambda 表达式。

### 2.2. 元注解

元注解是可以注解到注解上的注解，能够应用到其它的注解上面。

#### 2.2.1. @Retention

Retention 意为保留期，当 @Retention 应用到一个注解上的时候，它解释说明了这个注解的的存活时间。

@Retention 取值如下： 
- `RetentionPolicy.SOURCE`：注解只在源码阶段保留，在编译器进行编译时它将被丢弃忽视。 
- `RetentionPolicy.CLASS`：注解只被保留到编译进行的时候，它并不会被加载到 JVM 中。 
- `RetentionPolicy.RUNTIME`：注解可以保留到程序运行的时候，它会被加载进入到 JVM 中，所以在程序运行时可以获取到它们。

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {
}
```

#### 2.2.2. @Target

@Target 指定了注解运用的地方，当一个注解被 @Target 注解时，这个注解就被限定了运用的场景。

@Target 取值如下：
- `ElementType.ANNOTATION_TYPE`：可以给一个注解进行注解。
- `ElementType.CONSTRUCTOR`：可以给构造方法进行注解。
- `ElementType.FIELD`：可以给属性进行注解。
- `ElementType.LOCAL_VARIABLE`：可以给局部变量进行注解。
- `ElementType.METHOD`：可以给方法进行注解。
- `ElementType.PACKAGE`：可以给一个包进行注解。
- `ElementType.PARAMETER`：可以给一个方法内的参数进行注解。
- `ElementType.TYPE`：可以给一个类型进行注解，比如类、接口、枚举。

NOTE: 可以使用 `{}` 包含多个 @Target 的取值。

#### 2.2.3. @Documented

@Documented 用于指定被该元注解修饰的注解类是否将被 javadoc 工具提取成文档，如果定义注解时使用了 @Documented 修饰，则所有使用该注解修饰的程序元素的 API 文档中将会包含该注解的说明。

#### 2.2.4. @Inherited

如果一个超类被 @Inherited 注解过的注解进行注解的话，那么如果它的子类没有被任何注解应用的话，这个子类就会继承超类的所有注解。

**若 @Inherited 被应用在 interface 上，将不会生效，即实现该 interface 的类无法继承到 interface 的注解**。

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@interface Test {}

@Test
public class A {}

public class B extends A {}
```

#### 2.2.5. @Repeatable

@Repeatable 是 Java 8 新增的元注解，用于定义可重复注解，即可以被多次应用在同一个程序元素上的注解。

```java
@interface Persons {
    Person [] value();
}

@Repeatable(Persons.class) // @Repeatable 的成员变量是一个容器注解
@interface Person{
    String role default "";
}

@Person(role="artist")
@Person(role="coder")
@Person(role="PM")
public class SuperMan{

}
```
按照规定，它里面必须要有一个 value 的属性，属性类型是一个被 @Repeatable 注解过的注解数组，注意它是数组。

如果不好理解的话，可以这样理解。Persons 是一张总的标签，上面贴满了 Person 这种同类型但内容不一样的标签。把 Persons 给一个 SuperMan 贴上，相当于同时给他贴了程序员、产品经理、画家的标签。

NOTE: 重复注解实际上只是一种简化写法，多个重复注解事实上会被作为“容器”注解的 value 成员变量的数组元素。

## 3. 自定义注解

### 3.1. 注解的定义

#### 3.1.1. @interface

注解通过 @interface 关键字进行定义。
```java
public @interface TestAnnotation {
}
```

#### 3.1.2. 成员属性

注解的属性 / 成员变量，注解的成员变量在注解的定义中以**无形参无异常的方法**形式来声明，其方法名定义了该成员变量的名字，其返回值定义了该成员变量的类型。

注解只有成员变量，没有方法。

例：
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {

    int id();

    String msg();

}

// 代码定义了 TestAnnotation 这个注解中拥有 id 和 msg 两个属性。在使用的时候，我们应该给它们进行赋值
@TestAnnotation(id=3, msg="hello annotation")
public class Test {

}
```

NOTE:
- 在注解中定义属性时，它的类型必须是 8 种基本数据类型之一、类、接口、注解及它们的数组。
- 注解中属性可以有默认值，默认值需要用 default 关键值指定。
  ```java
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  public @interface TestAnnotation {

      public int id() default -1;

      public String msg() default "Hi";

  }
  ```
- 如果一个注解内仅仅只有一个名字为 value 的属性时，应用这个注解时可以直接接属性值填写到括号内。
  ```java
  public @interface Check {
      String value();
  }

  @Check("hi")
  int a;
  // 等同于
  @Check(value="hi")
  int a;
  ```
- 若一个注解没有任何属性，则在应用这个注解的时候，括号都可以省略。
  ```java
  public @interface Perform {}

  @Perform
  public void testMethod(){}
  ```

### 3.2. 注解的解析

当开发者使用了 Annotation 修饰了类、方法、Field 等成员之后，这些 Annotation 不会自己生效（对程序运行不会产生任何影响），必须由开发者提供相应的代码来提取并处理 Annotation 信息。

这些处理提取和处理 Annotation 的代码统称为 APT（Annotation Processing Tool)，使用 APT 的目的是简化开发者的工作量，代替传统的对代码信息和附属文件的维护工作。

#### 3.2.1. 相关接口

**注解通过反射机制获取和解析，由于反射比较慢，因此注解使用时也需要谨慎计较时间成本。**

- [Annotation](https://docs.oracle.com/javase/9/docs/api/java/lang/annotation/package-summary.html) 接口
  
  Java 中使用 `Annotation` 接口来代表程序元素前面的注解，是所有注解的父接口。

- [AnnotatedElement](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/AnnotatedElement.html) 接口
  
  `java.lang.reflect` 包下的 `AnnotatedElement` 接口代表程序中可以接受注解的程序元素，是所有程序元素的父接口。
  - 该接口主要有以下几个实现类：
    - Class：类定义
    - Constructor：构造器定义
    - Field：类的成员变量定义
    - Method：类的方法定义
    - Package：类的包定义
  - 该接口主要有以下几个方法：
    - `<T extends Annotation> T	getAnnotation​(Class<T> annotationClass)`: 返回改程序元素上存在的、指定类型的注解，如果该类型注解不存在，则返回 null。
    - `default <T extends Annotation> getDeclaredAnnotation​(Class<T> annotationClass)`: 返回直接存在于此元素上存在的、指定类型的注解。与此接口中的其他方法不同，该方法将忽略继承的注解（如果没有注解直接存在于此元素上，则返回长度为零的一个数组）。该方法的调用者可以随意修改返回的数组，这不会对其他调用者返回的数组产生任何影响。
    - `Annotation[]	getAnnotations​()`: 返回该程序元素上存在的所有注解。
    - `Annotation[]	getDeclaredAnnotations​()`: Returns annotations that are directly present on this element.
    - `default boolean	isAnnotationPresent​(Class<? extends Annotation> annotationClass)`: 判断该程序元素上是否包含指定类型的注解，存在则返回 true，否则返回 false。
    
#### 3.2.2. 一般步骤

1. 首先可以通过 Class 对象的 isAnnotationPresent() 方法判断它是否应用了某个注解：
		```java
		public boolean isAnnotationPresent(Class<? extends Annotation> annotationClass) {}
		```
1. 然后通过 getAnnotation() 方法或 getAnnotations() 方法来获取 Annotation 对象：
		```java
		public <A extends Annotation> A getAnnotation(Class<A> annotationClass) {} // 返回指定类型的注解
		public Annotation[] getAnnotations() {} // 返回注解到这个元素上的所有注解
		```
1. 如果获取到的 Annotation 如果不为 null，则就可以调用它们的属性方法了：

		eg:
		```java
		@TestAnnotation(msg="hello")
		public class Test {
				@Check(value="hi")
				int a;

				@Perform
				public void testMethod() {}

				@SuppressWarnings("deprecation")
				public void test1() {
						Hero hero = new Hero();
						hero.say();
						hero.speak();
				}

				public static void main(String[] args) {
						boolean hasAnnotation = Test.class.isAnnotationPresent(TestAnnotation.class);
						if ( hasAnnotation ) {
								// 获取类的注解
								TestAnnotation testAnnotation = Test.class.getAnnotation(TestAnnotation.class);
								System.out.println("id:"+testAnnotation.id());
								System.out.println("msg:"+testAnnotation.msg());
						}

						try {
								// 获取一个成员变量上的注解
								Field a = Test.class.getDeclaredField("a");
								a.setAccessible(true);
								Check check = a.getAnnotation(Check.class);

								if ( check != null ) {
										System.out.println("check value:"+check.value());
								}

								// 获取方法中的注解
								Method testMethod = Test.class.getDeclaredMethod("testMethod");
								if ( testMethod != null ) {
										Annotation[] ans = testMethod.getAnnotations();
										for( int i = 0; i < ans.length; i++) {
												System.out.println("method testMethod annotation:"+ans[i].annotationType().getSimpleName());
										}
								}
						} catch (NoSuchFieldException e) {
								// TODO Auto-generated catch block
								e.printStackTrace();
								System.out.println(e.getMessage());
						} catch (SecurityException e) {
								// TODO Auto-generated catch block
								e.printStackTrace();
								System.out.println(e.getMessage());
						} catch (NoSuchMethodException e) {
								// TODO Auto-generated catch block
								e.printStackTrace();
								System.out.println(e.getMessage());
						}
				}
		}
		```
		运行结果：
		```
		id:-1
		msg:hello
		check value:hi
		method testMethod annotation:Perform
		```

NOTE: 如果一个注解要在运行时被成功提取，那么 `@Retention(RetentionPolicy.RUNTIME)` 是必须的。

## 4. 应用实例

注解有许多用处，主要如下： 
- 提供信息给编译器：编译器可以利用注解来探测错误和警告信息。
- 编译阶段时的处理：软件工具可以用来利用注解信息来生成代码、HTML 文档或者做其它相应处理。
- 运行时的处理：某些注解可以在程序运行的时候接受代码的提取。

### 4.1. 自定义简单测试注解

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface Jiecha {

}
```
```java
public class NoBug {

    @Jiecha
    public void suanShu(){
        System.out.println("1234567890");
    }
    @Jiecha
    public void jiafa(){
        System.out.println("1+1="+1+1);
    }
    @Jiecha
    public void jiefa(){
        System.out.println("1-1="+(1-1));
    }
    @Jiecha
    public void chengfa(){
        System.out.println("3 x 5="+ 3*5);
    }
    @Jiecha
    public void chufa(){
        System.out.println("6 / 0="+ 6 / 0);
    }

    public void ziwojieshao(){
        System.out.println("我写的程序没有 bug!");
    }

}
```
```java
public class TestTool {

    public static void main(String[] args) {
        // TODO Auto-generated method stub

        NoBug testobj = new NoBug();

        Class clazz = testobj.getClass();

        Method[] method = clazz.getDeclaredMethods();
        // 用来记录测试产生的 log 信息
        StringBuilder log = new StringBuilder();
        // 记录异常的次数
        int errornum = 0;

        for ( Method m: method ) {
            // 只有被 @Jiecha 标注过的方法才进行测试
            if ( m.isAnnotationPresent( Jiecha.class )) {
                try {
                    m.setAccessible(true);
                    m.invoke(testobj, null);

                } catch (Exception e) {
                    // TODO Auto-generated catch block
                    //e.printStackTrace();
                    errornum++;
                    log.append(m.getName());
                    log.append(" ");
                    log.append("has error:");
                    log.append("\n\r  caused by ");
                    // 记录测试过程中，发生的异常的名称
                    log.append(e.getCause().getClass().getSimpleName());
                    log.append("\n\r");
                    // 记录测试过程中，发生的异常的具体信息
                    log.append(e.getCause().getMessage());
                    log.append("\n\r");
                } 
            }
        }

        log.append(clazz.getSimpleName());
        log.append(" has  ");
        log.append(errornum);
        log.append(" error.");

        // 生成测试报告
        System.out.println(log.toString());

    }

}
```

### 4.2. JUnit

@Test 标记了要进行测试的方法 `addition_isCorrect()`:
```java
public class ExampleUnitTest {
    @Test
    public void addition_isCorrect() throws Exception {
        assertEquals(4, 2 + 2);
    }
}
```

### 4.3. Retrofit

```java
public interface GitHubService {
  @GET("users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}

public class Main {
	public static void main(String [] args) {
		Retrofit retrofit = new Retrofit.Builder()
				.baseUrl("https://api.github.com/")
				.build();
	
		GitHubService service = retrofit.create(GitHubService.class);
	}
}
```

## 5. Refer Links

[慕课网：全面解析 Java 注解](https://www.imooc.com/learn/456)

[秒懂，Java 注解 （Annotation）你可以这样学](http://blog.csdn.net/briblue/article/details/73824058)

[深入理解 Java：注解（Annotation）基本概念](https://www.cnblogs.com/peida/archive/2013/04/23/3036035.html)

[深入理解 Java：注解（Annotation）自定义注解入门](https://www.cnblogs.com/peida/archive/2013/04/24/3036689.html)

[深入理解 Java：注解（Annotation）-- 注解处理器](https://www.cnblogs.com/peida/archive/2013/04/26/3038503.html)

[Java 注解的知识导图](https://images0.cnblogs.com/blog/34483/201304/25200814-475cf2f3a8d24e0bb3b4c442a4b44734.jpg)

[深入理解 Java 注解类型 (@Annotation)](http://blog.csdn.net/javazejian/article/details/71860633)