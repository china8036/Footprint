- [C++ 编程规范](#c-编程规范)
  - [1. 程序版式](#1-程序版式)
  - [2. 注释](#2-注释)
  - [3. 标识符命名](#3-标识符命名)
  - [4. 可读性](#4-可读性)
  - [5. 变量与结构](#5-变量与结构)
  - [6. 函数过程](#6-函数过程)
  - [7. 程序效率](#7-程序效率)
  - [8. 质量保证](#8-质量保证)
  - [9. Refer Links](#9-refer-links)

# C++ 编程规范

原则：
> 代码的清晰易懂远比其它方面重要。因为程序的时间、空间效率再高，算法再好，如果可读性差，无法重用、维护和扩展，那么最终是一堆无用的代码。

## 1. 程序版式

- 统一缩进为 4 个空格，不能使用 TAB 键
- 相对独立的程序块之间、变量说明之后必须加空行
- 长表达式（长度大于 80 字符）要在低优先级操作符处划分新行，操作符放在新行之首
- 一行代码只做一件事情
- 关键字后加空格，函数名后不加空格
- if、for、do、while 等语句的执行语句部分无论多少都要换行加大括号{}
- 缩进示例
  ```cpp
  do
  {    …
      break;
      …
  } while (…);

  for ( … ; … ; …)
  {    …
      continue;
      …
  }

  switch (…)
  {
    case Value1:
        …
        break;
    case Value2:
        …
        break;
    default:
        …
        break;
  }
  ```
- 在复杂的表达式中要多采用括号来降低运算符优先级带来的潜在错误
- 在变量和常量进行比较时，常量应该放在左边，这样在你不小心把 == 写成 = 的时候就会报错了
  ```cpp
  if (CONST_MAX_NUM == iInput)
  {
      …
  }
  ```
- 不要在 if 的条件表达式中进行赋值
- 分离结构体类型定义和结构体变量声明
- 声明块内多行之间要对齐
  ```cpp
  int    dwRetValue     = 0;;
  int*   pdwRetValue    = NULL;
  char*    pRetValue    = NULL;
  char     cRetValue    = 0;
  ```
- & 和*标志符应该和数据类型放在一起而不是变量
  ```cpp
  int* p; // ok
  int *p; // wrong
  int* const kp = &x; // ok
  int *const kp = &x; // wrong
  ```
- 变量仅当其第一次使用时才进行定义，如果一个变量对于函数是全局的，应该将变量的定义放在函数头的位置

## 2. 注释

- 源文件头部应进行注释，列出：生成日期、作者、模块目的 / 功能等
  ```cpp
  // ------------------------------------------------------------
  // Name        : Account
  // Description :
  //     This is the implementation of account as a business service
  //     entity class.
  // Author:
  //     Kevin Cheng(Kevin@tom.com)
  // Date:
  //     2001-4-15
  // History     :
  // 1.0     20010415 Kevin Cheng
  //                  Initial version
  // 1.1     20010427 Kevin Cheng
  //                  Fixed bug in Property Get Balance that caused
  //                  an incorrect value to be returned when the actual
  //                  value of the balance exceeded 32767. (SES0000054)
  //                  The correct value is now returned.
  // 1.2     20010429 Kevin Cheng
  //                  Added Delete method to allow deletion of the
  //                  account. (SES0000071)
  // ------------------------------------------------------------
  ```
- 函数头部应进行注释，列出：函数的目的 / 功能、输入参数、输出参数、返回值等
  ```cpp
  // ------------------------------------------------------------
  // Description :
  //     This function transfers an amount from one account to another.
  // Parameters :
  //     FromAccountId [IN]      – Id of the account to be debited.
  //     ToAccountId    [IN]      – Id of the account to be credited.
  //     Amount          [IN/OUT] – Amount to be transferred.
  // Return Value :
  //     None.
  // Errors :
  //     Throws AccountException if there are insufficient funds in the
  //     account to be debited or if either account does not exist.
  //     If AccountException is thrown, no transaction has been performed
  //     and the account remains in its original state.
  // ------------------------------------------------------------
  ```
- 注释应该和代码同时更新，不再有用的注释要删除
- 常用关键字
  ```
  :TODO: topic
      表示这里还有后续的工作要做，不要忘了
  :BUG: [bugid] topic
      表示这里有一个已知的 Bug，描述一下 Bug, 也可以用一个 BugID 来标示这个 Bug
  :KLUDGE:
      表示你在这段写得很烂，如果有时间可以进行优化或改写
  :TRICKY:
      表示这里用到了一个技巧，注明这个技巧
  :WARNING:
      提示一个注意事项
  :COMPILER:
      为了解决一个编译器问题编写的代码
  ```
- 注释应与其描述的代码相近，对代码的注释应放在其上方或右方（对单条语句的注释）相邻位置，不可放在下面。变量、常量、宏的注释应放在其上方相邻位置或右方
  ```cpp
  //if pass then call fun to handle the status
  if(bPass)
  {
  //{comments for this call}
  fun();
  }
  else //{comments for the else condition handling}
  {
    …
  }
  ```
- 除了声明不要使用行注释
- 对于所有有物理含义的变量、常量，如果其命名不是充分自注释的，在声明时都必须加以注释，说明其物理含义
- 数据结构声明（包括数组、结构、类、枚举等)，如果其命名不是充分自注释的，必须加以注释。
- 注释不宜过多，也不能太少，源程序中有效注释量控制在 20％~30% 之间。
- 注释中主要说明"Why"而不仅仅是"What"。

## 3. 标识符命名

- 命名尽量使用英文单词，较短的单词可通过去掉“元音”形成缩写；较长的单词可取单词的头几个字母形成缩写；一些单词有大家公认的缩写。
  - temp 可缩写为  tmp
  - flag 可缩写为  flg
  - statistic 可缩写为  stat
  - increment 可缩写为  inc
  - message 可缩写为  msg
- 命名规范必须与所使用的系统风格保持一致，并在同一项目中统一
  - 变量的命名可参考“匈牙利”标记法（Hungarian Notation）：TypePrefix+ Name
- 常量、宏和模板名采用全大写的方式，每个单词间用下划线分隔
- 对于变量命名，禁止取单个字符（如 i、j、k...），建议除了要有具体含义外，还能表明其变量类型、数据类型等，但 i、j、k 作局部循环变量是允许的
- 函数名以大写字母开头，采用谓 - 宾结构（动 - 名），且应反映函数执行什么操作以及返回什么内容
  - 不好的命名：`if (CheckSize(x))` 没有帮助作用，因为它没有告诉我们 CheckSize 是在出错时返回 true 还是在不出错时返回 true。
  - 好的命名：`if (ValidSize(x))` 则使函数的意图很明确。

## 4. 可读性

- 避免使用不易理解的数字，用有意义的标识来替代。涉及物理状态或者含有物理意义的常量，不应直接使用数字，必须用有意义的枚举或宏来代替。
  ```cpp
  #define TRUNK_IDLE 0
  #define TRUNK_BUSY 1

  if (Trunk[index].trunk_state == TRUNK_IDLE)
  {
      Trunk[index].trunk_state = TRUNK_BUSY;
      ...  // program code
  }
  ```
- 要使用难懂的技巧性很高的语句，除非很有必要时。高技巧语句不等于高效率的程序，实际上程序的效率关键在于算法。

## 5. 变量与结构

- 尽量少使用全局变量，尽量去掉没必要的公共变量。
- 变量，特别是指针变量，被创建之后应当及时把它们初始化，以防止把未被初始化的变量当成右值使用。
- **留心具体语言及编译器处理不同数据类型的原则及有关细节**。如在 C 语言中，static 局部变量将在内存“数据区”中生成，而非 static 局部变量将在“堆栈”中生成。这些细节对程序质量的保证非常重要。
- 尽量减少没有必要的数据类型默认转换与强制转换。
- **当声明用于分布式环境或不同 CPU 间通信环境的数据结构时，必须考虑机器的字节顺序、使用的位域及字节对齐等问题**。
  - 在 Intel CPU 与 SPARC CPU，在处理位域及整数时，其在内存存放的“顺序”正好相反。

    假如有如下短整数及结构：
    ```cpp
    unsigned short int exam;
    typedef struct EXAM_BIT_STRU
    {                       /* Intel 68360 */
        unsigned int A1: 1; /* bit  0      7   */
        unsigned int A2: 1; /* bit  1      6   */
        unsigned int A3: 1; /* bit  2      5   */
    } EXAM_BIT;
    ```
    如下是 Intel CPU 生成短整数及位域的方式：
    ```
    内存： 0          1         2    ...  （从低到高，以字节为单位）
    exam  exam 低字节  exam 高字节

    内存：        0 bit     1 bit      2 bit    ...  （字节的各“位”）
    EXAM_BIT     A1        A2         A3
    ```
    如下是 SPARC CPU 生成短整数及位域的方式：
    ```
    内存： 0          1         2    ...  （从低到高，以字节为单位）
    exam  exam 高字节  exam 低字节

    内存：        7 bit     6 bit      5 bit    ...  （字节的各“位”）
    EXAM_BIT     A1        A2         A3
    ```

  - 在对齐方式下，CPU 的运行效率要快一些。
    ```
    1       8       16      24      32
    ------- ------- ------- -------
    | long1 | long1 | long1 | long1|
    ------- ------- ------- -------
    |        |         |        |        |
    ------- ------- ------- --------
    | long2 | long2 | long2 |  long2|
    ------- ------- ------- --------
    | ....
    ```
    当一个 long 型数（如图中 long1）在内存中的位置正好与内存的字边界对齐时，CPU 存取这个数只需访问一次内存，而当一个 long 型数（如图中的 long2）在内存中的位置跨越了字边界时，CPU 存取这个数就需要多次访问内存，如 i960cx 访问这样的数需读内存三次（一个 BYTE、一个 SHORT、一个 BYTE，由 CPU 的微代码执行，对软件透明），所有对齐方式下 CPU 的运行效率明显快多了。

## 6. 函数过程

- 调用函数要检查所有可能的返回情况，不应该的返回情况要用 ASSERT 来确认。
- 在同一项目组应明确规定，对接口函数参数的合法性检查应由函数的调用者负责还是由接口函数本身负责，缺省是由函数调用者负责。
- **编写可重入函数时，应注意局部变量的使用，应使用 auto 即缺省态局部变量或寄存器变量**。不应使用 static 局部变量，否则必须经过特殊处理，才能使函数具有可重入性。
- 函数的规模尽量限制在 200 行以内（不包括注释和空格行）。
- 一个函数仅完成一件功能。
- **尽量写类的构造、拷贝构造、析构和赋值函数，而不使用系统缺省的**。编译器以“位拷贝”的方式自动生成缺省的拷贝构造函数和赋值函数函数，倘若类中含有指针变量，那么这两个缺省的函数就隐含了错误。
  ```cpp
  class String
  {
      public:
      String(const char *str = NULL);	// 普通构造函数
      String(const String &other);	// 拷贝构造函数
      ~ String(void);					// 析构函数
      String & operate =(const String &other);	// 赋值函数
      private:
      char  	*m_data;				// 用于保存字符串
  };
  String  a, b;
  ```
  如果将 a 赋给 b，缺省赋值函数的“位拷贝”意味着执行 b.m_data = a.m_data。这将造成以下的错误：
  - b.m_data 原有的内存没被释放，造成内存泄露；
  - b.m_data 和 a.m_data 指向同一块内存，a 或 b 任何一方变动都会影响另一方；
  - 在对象被析构时，m_data 被释放了两次。
- **尽量不使用与程序运行的环境相关的系统函数**。如 strxfrm() 和 strcoll()，这两个函数依赖于 LC_COLLATE 的设置。
- **检查函数所有参数与非参数的有效性**。
  - 函数的输入主要有两种：一种是参数输入；另一种是全局变量、数据文件的输入，即非参数输入。函数在使用输入之前，应进行必要的检查。
  - 不应该的入口情况要用 ASSERT 来确认。
  - 有时候不处理也是一种处理，但要明确哪些情况不处理。`try...catch` 是一种常用的不处理的处理手段。
- 函数实现中不改变内容的参数要定义成 const。
- **设计高扇入、合理扇出（小于 7）的函数**。
  - 扇出是指一个函数直接调用（控制）其它函数的数目，而扇入是指有多少上级函数调用它。
  - 扇入越大，表明使用此函数的上级函数越多，这样的函数使用效率高，但不能违背函数间的独立性而单纯地追求高扇入。公共模块中的函数及底层函数应该有较高的扇入。
  - 扇出过大，表明函数过分复杂，需要控制和协调过多的下级函数；而扇出过小，如总是 1，表明函数的调用层次可能过多，这样不利程序阅读和函数结构的分析，并且程序运行时会对系统资源如堆栈空间等造成压力。**函数较合理的扇出（调度函数除外）通常是 3-5**。扇出太大，一般是由于缺乏中间层次，可适当增加中间层次的函数。扇出太小，可把下级函数进一步分解多个函数，或合并到上级函数中。当然分解或合并函数时，不能改变要实现的功能，也不能违背函数间的独立性。
  - **较良好的软件结构通常是顶层函数的扇出较高，中层函数的扇出较少，而底层函数则扇入到公共模块中**。
- 除非为某些算法或功能的实现方便，应减少没必要的递归调用。

## 7. 程序效率

- 空间效率
  - **通过对系统数据结构的划分与组织的改进，以及对程序算法的优化来提高空间效率**。这是解决软件空间效率的根本办法。
- 时间效率
  - 循环体内工作量最小化。
    - **应仔细考虑循环体内的语句是否可以放在循环体之外，使循环体内工作量最小**，从而提高程序的时间效率。
    - **在多重循环中，应将最忙的循环放在最内层，这样可以减少 CPU 切入循环层的次数**，从而提高效率。
  - **避免循环体内含判断语句，应将循环语句置于判断语句的代码块之中**。
    ```cpp
    for (ind = 0; ind < MAX_RECT_NUMBER; ind++)
    {
        if (data_type == RECT_AREA)
        {
            area_sum += rect_area[ind];
        }
        else
        {
            rect_length_sum += rect[ind].length;
            rect_width_sum += rect[ind].width;
        }
    }
    ```
    因为判断语句与循环变量无关，故可如下改进，以减少判断次数：
    ```cpp
    if (data_type == RECT_AREA)
    {
        for (ind = 0; ind < MAX_RECT_NUMBER; ind++)
        {
            area_sum += rect_area[ind];
        }
    }
    else
    {
        for (ind = 0; ind < MAX_RECT_NUMBER; ind++)
        {
            rect_length_sum += rect[ind].length;
            rect_width_sum  += rect[ind].width;
        }
    }
    ```
- 在逻辑清楚且不影响可读性的情况下，代码越少越好，写程序不是以行数的多少来判断一个人的工作效率。
- **尽量使用标准库函数，不要“发明”已经存在的库函数**。
- 要尽量重用已有的代码，直接调用已有的 API。**如果原有的代码质量比较好，尽量复用它。但是不要修补很差劲的代码，应当重新编写**。

## 8. 质量保证

- 过程 / 函数中动态分配的资源（包括内存、文件等），在过程 / 函数退出之前要释放。
  ```cpp
  // 以下例子将造成内存泄露
  void Func(void)
  {
      char *p = (char *) malloc(128);
      …… //do something
      return ;
  }
  ```
- **充分理解 new/delete，malloc/free 等指针相关的函数的意义，对指针操作时需小心翼翼**。
  - 内存被释放了，并不表示指针会消亡或者成了 NULL 指针。
    ```cpp
    char *p = (char *) malloc(100);
    strcpy(p, “hello”);
    free(p);
    …… //do something

    if (p != NULL)
    {
        …… //These code will be executed ?
    }
    // p 没有成为 NULL 指针，if 中间的代码被错误执行了
    ```
  - `new` 与 `delete` 要“步调一致”。
    ```cpp
    void Func()
    {
        Object *pObjects=new Object[10] ;
        // do something
        delete pObjects ;
    }

    // 申请了 10 个 Object 空间，但只释放了 1 个，造成内存泄露。正确的写法是：
    delete  []pObjects ;
    ```
- 防止内存操作越界。内存操作主要是指对数组、指针、内存地址等的操作。**内存操作越界是软件系统主要错误之一，后果往往非常严重**，所以当我们进行这些操作时一定要仔细小心。
- if 语句尽量加上 else 分支，对没有 else 分支的语句要小心对待；switch 语句必须有 default 分支。
- 尽量少用 goto 语句。
- **不使用与硬件或操作系统关系很大的语句，而使用建议的标准语句**，以提高软件的可移植性和可重用性。
  ```cpp
  for (idx=0; idx<40; dest[idx]=src[idx++])
  {
    // ...
  }
  ```
  这段代码中，在不同的操作系统，有不同的执行顺序。是先执行 idx++ 呢？还是先执行 dest[idx]=src[idx++] ？

  正确的写法是：
  ```cpp
  for (idx=0 ; idx<40; idx ++)
  {
    dest[idx]=src[idx] ;
  }
  ```
- **时刻注意表达式是否会上溢、下溢**。
  ```cpp
  unsigned char size;
  while (size-- >= 0) // 将出现下溢
  {
      ... // program code
  }
  ```
  当 size 等于 0 时，再减 1 不会小于 0，而是 0xFF（造成变量下溢），故程序是一个死循环。应如下修改：
  ```cpp
  char size; // 从 unsigned char 改为 char
  while (size-- >= 0)
  {
      ... // program code
  }
  ```
- **打开编译器的所有告警开关对程序进行编译，并且要确认、处理所有的编译告警**。
- 如果可能，单元测试要覆盖 98% 以上的代码，尽可能早地发现和解决问题。**开发人员要树立“不要依赖测试人员”的观念，且不要抱侥幸心理**，会出问题的地方总是会出问题的。
- 如果可能，**尽量使用 pc-lint，purify，LogiScope 等测试工具**，以提高效率。

## 9. Refer Links