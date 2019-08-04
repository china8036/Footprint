- [C++ 基础](#c-基础)
  - [1. 基础概念](#1-基础概念)
  - [2. 发展历史](#2-发展历史)
  - [3. 语言设计原则](#3-语言设计原则)
  - [4. 与 C 的关系](#4-与-c-的关系)
  - [5. 运行效率](#5-运行效率)
  - [6. 应用领域](#6-应用领域)
  - [7. 常见问题](#7-常见问题)
    - [7.1. C++ 源代码跨平台吗？](#71-c-源代码跨平台吗)
    - [7.2. C++ 程序容易崩溃？](#72-c-程序容易崩溃)
    - [7.3. C++ 要手动做内存管理？](#73-c-要手动做内存管理)
    - [7.4. 使用 C++ 常要重造轮子？](#74-使用-c-常要重造轮子)
    - [7.5. C++ 编译速度很慢？](#75-c-编译速度很慢)
    - [7.6. C++ 缺乏什么功能？](#76-c-缺乏什么功能)
  - [8. 使用建议](#8-使用建议)
    - [8.1. 为应用挑选特性集](#81-为应用挑选特性集)
    - [8.2. 为团队建立编程规范](#82-为团队建立编程规范)
    - [8.3. 尽量使用 C++ 风格而非 C 风格](#83-尽量使用-c-风格而非-c-风格)
    - [8.4. 结合其他语言](#84-结合其他语言)
  - [9. C++ 编译器](#9-c-编译器)
  - [10. Refer Links](#10-refer-links)

# C++ 基础

## 1. 基础概念

> C++ is a general-purpose programming language that was developed by Bjarne Stroustrup as an extension of the C language, or "C with Classes". It has imperative, object-oriented and generic programming features, while also providing facilities for low-level memory manipulation. It is almost always implemented as a compiled language, and many vendors provide C++ compilers, including the Free Software Foundation, Microsoft, Intel, and IBM, so it is available on many platforms.
>
> C++ was designed with a bias toward system programming and embedded, resource-constrained software and large systems, with performance, efficiency and flexibility of use as its design highlights.

C++ 是一门通用编程语言，支持多种编程范式，包括过程式、面向对象 (object-oriented programming, OP)、泛型 (generic programming, GP)，后来为泛型而设计的模版，被[发现及证明是图灵完备的](http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=AA022F3026015EF910CAEF5156901019?doi=10.1.1.14.3670&rep=rep1&type=pdf)，因此使 C++ 亦可支持[模板元编程范式 (template metaprogramming, TMP)](http://en.wikipedia.org/wiki/Template_metaprogramming)。C++ 继承了 C 的特色，既为高级语言，又含低级语言功能，可同时作为系统和应用编程语言。

## 2. 发展历史

- **诞生**

  > Bjarne Stroustrup is generally considered to be the father of C++. He started developing it in 1979 at Bell Labs as an enhancement to the C programming language and originally named it "C with Classes". It was renamed to C++ in 1983.

  1979 年，[Bjarne Stroustrup](http://www.stroustrup.com/) 在贝尔实验室工作期间发明并实现了 C++。当时，这种语言被称作 C with Classes，即“包含‘类’的 C 语言”，作为 C 语言的增强版出现。在 1983 年才更名为 C++(C Plus Plus)。

  之后，C++ 不断增加新特性。虚函数（virtual function）、运算符重载（operator overloading）、多继承（multiple inheritance）、标准模板库（standard template library, STL）、异常处理（exception）、运行时类型信息（Runtime type information）、命名空间（namespace）等概念逐渐纳入标准。

- **C++98**

  1998 年，国际标准组织（ISO）颁布了 C++ 程序设计语言的第一个国际标准 ISO/IEC 14882:1998，目前最新标准为 ISO/IEC 14882:2017。

  P.S.
  > The C++ standard consists of two parts: the core language and the standard library.

  C++ 标准包括了 C++ 核心语法和 C++ 标准库两部分，因此本笔记也将从这两个方面进行总结归纳。

- **C++03**

  2003 年，作为第二个 C++ 标准，ISO/IEC 14882:2003 发布。

- **C++11**

  2011 年，作为第三个 C++ 标准，ISO/IEC 14882:2011 发布。

- **C++14**

  2014 年，作为第四个 C++ 标准，ISO/IEC 14882:2014 发布。

- **C++17**

  2017 年，作为第五个 C++ 标准，ISO/IEC 14882:2017 发布。

## 3. 语言设计原则

在《C++ 语言的设计和演化》（1994）中，Bjarne Stroustrup 描述了他在设计 C++ 时，所使用的一些原则：
- 支持多种程序设计风格：过程化程序设计、数据抽象、面向对象程序设计、泛型程序设计。
- 给程序设计者更多的选择，但可能导致程序设计者选择错误。
- 尽可能与 C 相互兼容，借此提供一个从 C 到 C++ 的平滑过渡。
- 不使用平台限制或没有普遍用途的特性。
- 不使用会带来额外开销的特性。
- 不需要复杂的程序开发环境。

## 4. 与 C 的关系

C++ 和 C 的设计哲学并不一样，两者取舍不同，所以不同的程序员和软件项目会有不同选择，难以一概而论。

- C++ 时常被认为是 C 的超集（superset），但这并不严谨，各个版本的 ISO/IEC 14882 的附录 C 中都指出了 C++ 和 ISO C 的一些不兼容之处。大部分的 C 代码可以很轻易的在 C++ 中正确编译，但仍有少数差异，导致某些有效的 C 代码在 C++ 中失效，或者在 C++ 中有不同的行为。

  i.e.
  - C 允许从 void*隐式转换到其它的指标类型，但 C++ 不允许。

    对于有效的 C 代码：
    ```c
    // 从 void *隐式转换为 int *
    int *i = malloc(sizeof(int) * 5);
    ```
    但要使其在 C 和 C++ 两者皆能运作，就需要使用显式转换：
    ```c
    int *i = (int *)malloc(sizeof(int) * 5);
    ```

  - C++ 定义了很多的新关键字，如 new 和 class，它们在 C 程式中，是可以作为识别字（例：变量名）的。

  P.S.
  > To intermix C and C++ code, any function declaration or definition that is to be called from/used both in C and C++ must be declared with C linkage by placing it within an extern "C" {/*...*/} block. Such a function may not rely on features depending on name mangling (i.e., function overloading).

- 与 C++ 相比，C 具备编译速度快、容易学习、显式描述程序细节、较少更新标准（后两者也可同时视为缺点) 等优点。

- 在语言层面上，C++ 包含绝大部分 C 语言的功能（例外之一，C++ 没有 C99 的变长数组 VLA)，且提供 OOP 和 GP 的特性。但其实用 C 也可实现 OOP 思想，亦可利用宏去实现某程度的 GP，只不过 C++ 的语法能较简洁、自动地实现 OOP/GP。C++ 的 RAII(resource acquisition is initialization，资源获取就是初始化) 特性比较独特，C/C#/Java 没有相应功能。

- 回顾历史，Stroustrup 开发的早期 C++ 编译器 Cpre/Cfront 是把 C++ 源代码翻译为 C，再用 C 编译器编译的。由此可知，**C++ 编写的程序，基本都能用等效的 C 程序代替**，但 C++ 在语言层面上提供了 OOP/GP 语法、更严格的类型检查系统、大量额外的语言特性（如异常、RTTI 等)，并且 C++ 标准库也较丰富。有时候 C++ 的语法可使程序更简洁，如运算符重载、隐式转换。

- 但另一方面，C 语言的 API 通常比 C++ 简洁，能较容易供其他语言程序调用。因此，一些 C++ 库会提供 C 的 API 封装，同时也可供 C 程序调用。相反，有时候也会把 C 的 API 封装成 C++ 形式，以支持 RAII 和其他 C++ 库整合等。

## 5. 运行效率

根据《Thinking in C++》一书，C++ 与 C 的代码执行效率往往相差在 ±5% 之间。

相对运行于虚拟机语言（如 C#/Java)，C/C++ 直接以静态形式把源程序编译为目标平台的机器码。一般而言，C/C++ 程序在编译及链接时可进行的优化最丰富，启动时的速度最快，运行时的额外内存开销最少。而 C/C++ 相对动态语言（如 Python/Lua) 也减少了运行时的动态类型检测。此外，C/C++ 的运行行为是确定的，且不会有额外行为（例如 C#/Java 必然会初始化变量)，也不会有如垃圾收集 (GC) 而造成的不确定性延迟，而且 C/C++ 的数据结构在内存中的布局也是确定的。另一方面，C/C++ 能直接映射机器码，之间没有另一层中间语言，因此可以做底层优化，例如使用内部 (intrinsic) 函数和嵌入汇编语言。然而，许多 C++ 的性能优点并非免费午餐，代价包括较长的编译链接时间和较易出错，因而增加开发时间和成本。

有时 C++ 的一些功能会使程序性能优于 C，当中以内联和模版最为突出，这两项功能使 C++ 标准库的 `sort()` 通常比 C 标准库的 `qsort()` 快多倍 (C 可用宏或人手编码去解决此问题)。

## 6. 应用领域

C++ 广泛应用在不同领域，使用者以数百万计。根据近十年的调查，C++ 的流行程度约稳定排行第 3 位（于 C/Java 之后)。

C++ 并非万能丹，以下是一些 C++ 的适用时机：
- C++ 适合构造程序中需求较稳定的部分，需求变化较大的部分可使用脚本语言。
- 程序须尽量发挥硬件的最高性能，且性能瓶颈在于 CPU 和内存。
- 程序须频繁地与操作系统或硬件沟通。
- 程序必须使用 C++ 框架 / 库，如大部分游戏引擎（如 Unreal/Source) 及中间件（如 Havok/FMOD)，虽然有些 C++ 库提供其他语言的绑定，但通常原生的 API 性能最好、最新。
- 项目中某个目标平台只提供 C++ 编译器的支持。

按应用领域来说，C++ 适用于开发服务器软件、桌面应用、游戏、实时系统、高性能计算、嵌入式系统等。

## 7. 常见问题

### 7.1. C++ 源代码跨平台吗？

C++ 有不错的跨平台能力，但由于直接映射硬件，因性能优化的关系，跨平台能力不及 Java 及多数脚本语言。要使 C++ 软件源代码跨平台，需要注意以下问题：
- C++ 标准没有规定原始数据类型（如 int) 的大小，需要特定大小的类型时，可自订类型（如 int32_t)，同时对任何类型使用 sizeof() 而不假设其大小。
- 字节序 (byte order) 按 CPU 有所不同，特别要注意二进制输入输出、reinterpret_cast 法。
- 原始数据和结构类型的地址对齐有差异。
- 编译器提供的一些编译器或平台专用扩充指令。
- 避免作应用二进制接口 (application binary interface, ABI) 的假设，例如调用函数时参数的取值顺序在 C/C++ 中没定义，在 C++ 中也不可随便假设 RTTI/ 虚表等实现方式。

总括而言，跨平台 C++ 软件可在头文件中用宏检测编译器和平台，再用宏、typedef、指定平台相关实现等方法去实践跨平台，但 C++ 标准不会提供这类帮助。

### 7.2. C++ 程序容易崩溃？

和许多语言相比，C/C++ 提供不安全的功能以最优化性能，有可能造成崩溃。但要注意，很多运行时错误，如向空指针 / 引用解引用、数组越界、堆栈溢出等，其他语言也会报错或抛出异常，这些都是程序问题，而不是语言本身的问题。

有些意见认为，出现这类运行时错误，应该尽量写入日志并立即崩溃，不该让程序继续运行，以免造成更大的影响（例如程序继续把内存中错误的数据覆写文件)。若要容错，可按业务把程序分割为多进程，像 Chrome 或使用 fork() 的形式。事实上，C++ 有许多机制可以减少错误，例如以 string 代替 C 字符串；以 vector 或 array(TR1) 代替原始数组（有些实现可在调试模式检测越界)；使用智能指针也能减少一些原始指针的问题。

### 7.3. C++ 要手动做内存管理？

C++ 同时提供在堆栈上的自动局部变量，以及从自由存储 (free store) 分配的对象。对于后者，程序员需手动释放，或使用不同的容器和智能指针。 C++ 程序员经常进一步优化内存，自定义内存分配策略以提升效能，例如使用对象池、自定义的单向 / 双向堆栈区等。虽然 C++0x 还没加入 GC 功能，但也可以自行编写或使用现成库。此外，C/C++ 也可以直接使用操作系统提供的内存相关功能，例如内存映射文件、共享内存等。

### 7.4. 使用 C++ 常要重造轮子？

很多 C++ 项目，都会重造不少标准库已提供的功能，此情况在其他语言中较少出现，原因主要有以下几点：
- 首先，C++ 标准库相对很多语言来说是贫乏的，各开发者便会重复地制造自订库。从另一个角度看，C++ 标准库是用 C++ 编写的（很多其他语言不用自身而是用 C/C++ 去编写库)，在能力和性能上，自订库和标准库并无本质差别。
- 另外，标准库为通用而设，对不同平台及多种使用需求作取舍，性能上有所影响，例如 EA 公司就曾发表自制的 EASTL 规格，描述游戏开发方面对 STL 的性能及功能需求的特点。
- 此外，多个 C++ 库一起使用，经常会因规范不同而引起冲突，又或功能重叠，所以项目可能须自行开发，或引入其他库的概念或实现（如 Boost/TR1/Loki)，改写以符合项目规范。

### 7.5. C++ 编译速度很慢？

是的，而且是非常慢，实际上 C++ 可能是实用程序语言中编译速度最慢的。此问题涉及 C++ 沿用 C 的编译链接方式，又加入了复杂的类 / 泛型声明和内联机制，使编译时间倍增。

在 C++ 对编译方法改革之前（如 module 提案)，可使用以下技巧改善：
- 使用 pimpl 手法，因性能损耗应用于调用次数不多的类。
- 仅包含必要头文件，并尽量使用及提供前置声明版本的头文件（如 iosfwd)。
- 采用基于接口的设计，但须注意虚函数调用成本。
- 采用 unity build，即把多个 cpp 文件结合在一个编译单元进行编译。
- 采用分布式生成系统如 IncrediBuild。

### 7.6. C++ 缺乏什么功能？

虽然 C++ 已经非常复杂，但仍缺少很多常见功能。

C++0x 作出了不少改善，例如语言方面加入 Lambda 函数、闭包、类型推导声明等，而库方面则加入正则表达式、采用哈希表的 unordered_set/unordered_map、引用计数智能指针 shared_ptr/weak_ptr 等。但最值得留意的是 C++0x 引入多线程的语法和库功能，这是 C++ 演进的一大步。

然而，模组、GC、反射机制等功能虽有提案，却未加进 C++0x。

## 8. 使用建议

### 8.1. 为应用挑选特性集

> Just because you can do it, doesn't mean that you have to.

C++ 充满丰富的特性，但同时带来不同问题，例如过分复杂、编译及运行性能的损耗。一般可考虑是否使用多重继承、异常、RTTI，并调节使用模版及模版元编程的程度。使用过分复杂的设计和功能，可能会令部分团队成员更难理解和维护。

### 8.2. 为团队建立编程规范

C++ 的编码自由度很高，容易编写风格迥异的代码，C++ 本身也没有定义一些标准规范。而且，C++ 的源文件物理构成，较许多语言复杂。因此，除了决定特性集，每个团队应建立一套编程规范，包括源文件格式（可使用文件模版)、花括号风格。

### 8.3. 尽量使用 C++ 风格而非 C 风格

由于 C++ 有对 C 兼容的包袱，一些功能可以使用 C 风格实现，但最好使用 C++ 提供的新功能。最基本的是尽量以具名常量、内联函数和泛型取代宏，只把宏用在条件式编译及特殊情况。旧式的 C 要求局部变量声明在作用域开端，C++ 则无此限制，应把变量声明尽量置于邻近其使用的地方，for() 的循环变量声明可置于 for 的括号内。 C++ 中能加强类型安全的功能应尽量使用，例如避免“万能”指针 void *，而使用个别或泛型类型；用 bool 而非 int 表示布尔值；选用 4 种 C++ cast 关键字代替简单的强制转换。

### 8.4. 结合其他语言

C++ 并非适合所有应用情境，有时可以混合其他语言使用，包括用 C++ 扩展其他语言，或在 C++ 程序中嵌入脚本语言引擎。对于后者，除了使用各种脚本语言的专门 API，还可使用 Boost 或 SWIG 作整合。

## 9. C++ 编译器

## 10. Refer Links

[Wikipedia: C++](https://en.wikipedia.org/wiki/C%2B%2B)

[Milo 的游戏开发：C++ 强大背后](https://www.cnblogs.com/miloyip/archive/2010/09/17/behind_cplusplus.html)
