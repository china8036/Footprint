- [Java OOP 多态](#java-oop- 多态)
  - [1. TODO:](#1-todo)
  - [2. Refer Links](#2-refer-links)

# Java OOP 多态

## 1. TODO:

[一个 java static class 奇怪行为背后的原因？](https://www.zhihu.com/question/41924954)

https://www.zhihu.com/question/40937546/answer/88919650

eg:
```java
public class Main {
	public static void main(String[] args) {
		P b = new B();
		System.out.println(b.a);
    b.diplay();
	}
}

class P {
	public int a = 11;
	public P() {
		a = 22;
		diplay();
	}

	public void diplay() {
		System.out.println("I am in P, value is " + a);
	}
}

class B extends P {
	public int a = 33;
	public B() {
		a = 44;
		diplay();
	}
	public void diplay() {
		System.out.println("I am in B, value is " + a);
	}
}
```
运行结果：
```
I am in B, value is 0
I am in B, value is 44
22
I am in B, value is 44
```

解释：
- 背景知识
  
  在 Java 中，只有方法可以是虚方法（可以参与多态），字段永远都不是虚的（不参与多态）。
  - 由于在编译的时候，生成的机器码（或字节码）里面引用的是编译器算好的字段的地址，而不是字段的名称。因此，字段是不参与多态的。而一些动态语言（如 Javascript 和 Python）在查找变量是在运行时根据名称查找的，所以字段也可以参与多态。
  
  也就是说，子类的方法可以重写 (override) 父类的同签名方法，但子类的字段和父类的同名字段在子类对象实例中是同时存在的，只不过子类的字段会遮蔽父类的同名字段。哪个类的方法访问某个名字的字段时，该名字指的就是该类能看到的那个字段。

- 实例分析
  
  在上例中，P.display() 与 B.display() 构成覆写（override）关系：后者覆写前者，这是虚方法的多态性的表现。所以在创建 B 类实例时，虽然会（显式或隐式）调用到 P 的构造器，但在 P 的构造器里调用的 display() 方法永远是按照虚方法的查找规则，找到该实例最准确的实现，即 P() 里调用 display() 找到的是 B.display() 方法。而由于此时 B.a 尚未初始化，默认值是 0，因此先输出了 `I am in B, value is 0`。

  父类 P 构造完毕后，开始调用子类 B 的构造器，因此输出 `I am in B, value is 44`。

  由于字段不参与多态，而对象实例 b 在声明时被声明为 P 类型，因此调用 b.a 时，读取的是父类 P 中的字段 a 的值，即 22。若在声明时将对象实例 b 声明为 B 类型，则调用 b.a 时读取的就是子类 B 中的字段 a 的值，即 44。

  最后，由于方法会参与多态，虽然对象实例 b 在声明时被声明为 P 类型，但在调用 b.display 方法时会调用子类 B 中的 display 方法，因此输出的是 `I am in B, value is 44`。

## 2. Refer Links
