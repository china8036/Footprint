- [C++ 类型转换](#c-%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [1. 隐式类型转换](#1-%E9%9A%90%E5%BC%8F%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
  - [2. 强制类型转换](#2-%E5%BC%BA%E5%88%B6%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2)
    - [2.1. C 风格](#21-c-%E9%A3%8E%E6%A0%BC)
    - [2.2. C++ API](#22-c-api)
  - [3. Refer Links](#3-refer-links)

# C++ 类型转换

## 1. 隐式类型转换

## 2. 强制类型转换

### 2.1. C 风格

```c
TypeName b = (TypeName)a;
```
C++ 支持 C 风格的强制转换，但 C 风格的强制转换可能带来一些隐患，让一些问题难以察觉。

### 2.2. C++ API

为解决 C 风格的类型强制转换存在的问题，C++ 提供了一组可以用在不同场合的强制转换的函数

- **const_cast**
  
  const_cast 一般用于修改指针，如 const char *p 形式：
  - 常量指针被转化成非常量的指针，并且仍然指向原来的对象。  
  - 常量引用被转换成非常量的引用，并且仍然指向原来的对象。  

  ```cpp
  int main() {
      // 原始数组
      int ary[4] = { 1,2,3,4 };

      // 常量化数组指针
      const int*c_ptr = ary;
      //c_ptr[1] = 233;   //error

      // 通过 const_cast<Ty> 去常量
      int *ptr = const_cast<int*>(c_ptr);

      // 修改数据
      for (int i = 0; i < 4; i++)
          ptr[i] += 1;    //pass

      return 0;
  }
  ```

- **static_cast**  

  > 事实上，C++ 的任何的隐式转换都是使用 static_cast 来实现的。
  - static_cast 作用和 C 语言风格强制转换的效果基本一样，由于没有运行时类型检查来保证转换的安全性，所以这类型的强制转换和 C 语言风格的强制转换都有安全隐患。
  - 用于类层次结构中基类（父类）和派生类（子类）之间指针或引用的转换。注意：进行上行转换（把派生类的指针或引用转换成基类表示）是安全的；进行下行转换（把基类指针或引用转换成派生类表示）时，由于没有动态类型检查，所以是不安全的。
  - 用于基本数据类型之间的转换，如把 int 转换成 char，把 int 转换成 enum。这种转换的安全性需要开发者来维护。
  - static_cast 不能转换掉原有类型的 const、volatile、或者 __unaligned 属性（前两种可以使用 const_cast 来去除）。
  
  ```cpp
  /* 常规的使用方法 */
  float f_pi=3.141592f
  int   i_pi=static_cast<int>(f_pi); /// i_pi 的值为 3

  /* class 的上下行转换 */
  class Base{
      // something
  };
  class Sub:public Base{
      // something
  }

  // 上行 Sub -> Base
  // 编译通过，安全
  Sub sub;
  Base *base_ptr = static_cast<Base*>(&sub);  

  // 下行 Base -> Sub
  // 编译通过，不安全
  Base base;
  Sub *sub_ptr = static_cast<Sub*>(&base);    
  ```

- **dynamic_cast**

  dynamic_cast 强制转换，应该是这四种中最特殊的一个，因为他涉及到面向对象的多态性和程序运行时的状态，也与编译器的属性设置有关。所以不能完全使用 C 语言的强制转换替代，它也是最常有用的，最不可缺少的一种强制转换。

- **reinterpret_cast**  

  reinterpret_cast 为操作数的位模式提供较低层次的重新解释，但是他仅仅是重新解释了给出的对象的比特模型，并没有进行二进制的转换。

  reinterpret_cast 通常用于：
  - 任意的指针之间的转换
  - 引用之间的转换
  - 指针和足够大的 int 型之间的转换
  - 整数到指针的转换

## 3. Refer Links

[c++ 四种强制类型转换介绍](https://blog.csdn.net/ydar95/article/details/69822540)