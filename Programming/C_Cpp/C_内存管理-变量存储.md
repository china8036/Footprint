- [C 内存管理：变量存储](#c-%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86%EF%BC%9A%E5%8F%98%E9%87%8F%E5%AD%98%E5%82%A8)
	- [1. 局部变量](#1-%E5%B1%80%E9%83%A8%E5%8F%98%E9%87%8F)
	- [2. 全局变量](#2-%E5%85%A8%E5%B1%80%E5%8F%98%E9%87%8F)
	- [3. 数组](#3-%E6%95%B0%E7%BB%84)
	- [4. 结构体变量](#4-%E7%BB%93%E6%9E%84%E4%BD%93%E5%8F%98%E9%87%8F)
	- [5. 函数调用](#5-%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8)
		- [5.1. 普通函数调用](#51-%E6%99%AE%E9%80%9A%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8)
		- [5.2. 递归函数调用](#52-%E9%80%92%E5%BD%92%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8)
	- [6. Refer Links](#6-refer-links)

# C 内存管理：变量存储

变量在内存中以二进制形式存储，一个变量占用的存储空间，不仅和变量类型有关，还和编译环境有关，同一种类型的变量在不同编译环境下占用的存储空间不一样。比如开发中常用的基本数据类型 char、int 等在不同编译环境下就会占用不同大小的空间。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/3f940c68f3b0ab91173e18626d0bb2f5.jpg)

C 的存储类别包括 4 种：auto（自动的）、static（静态的）、register（寄存器的）、extern（外部的）。根据变量的存储类别可以得知其作用域和生命周期。

## 1. 局部变量

```cpp
int main(int argc, const char * argv[]) {
    int a = 1;
    int b = 2;
    printf("%p\n",&a);  //0x7fff5fbff79b
    printf("%p\n",&b);  //0x7fff5fbff797
    return 0;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/d2c8cbbde8b3ee30984395125f883c13.jpg)

由于是 a、b 是临时变量，因此他们的内存空间分配在栈上，栈中内存寻址由高到低，所以 a 变量的地址比 b 变量的地址要大，其次由于是在 64 位编译环境中，int 型变量占据 4 个字节的空间，每一个字节由低到高依次对应着 8 位二进制数，四个 8 位二进制数就是十进制中的 1 或 2，而变量 a、b 的地址就是四个字节中最小值的内存地址。

## 2. 全局变量

## 3. 数组

C 语言中数组的存储和普通的变量不太一样，**数值中存储的元素，是从所占用的低地址开始存储的**。

```cpp
int main(int argc, const char * argv[]) {
    char chars[4] = {'l','o','v','e'};
    printf("chars[0] = %p\n",&chars[0]); //0x7fff5fbff79b
    printf("chars[1] = %p\n",&chars[1]); //0x7fff5fbff79c
    printf("chars[2] = %p\n",&chars[2]); //0x7fff5fbff79d
    printf("chars[3] = %p\n",&chars[3]); //0x7fff5fbff79e
    return 0;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/9e1074382e6dbb6b24ab9925e7dca0da.jpg)

```cpp
int main(int argc, const char * argv[]) {
    int nums[2] = {5, 6};
    printf("nums[0] = %p\n",&nums[0]); // 0x7fff5fbff7a0
    printf("nums[1] = %p\n",&nums[1]); // 0x7fff5fbff7a4
    return 0;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/66c80fcdc5966bbb5e24bb2e6f983dd4.jpg)

**数组中的元素按照存放顺序依次从低地址到高地址存放，但是每个元素中的内容又是按高地址向低地址方向存储**。

## 4. 结构体变量

结构体在 C 语言中是一种常见的数据结构，作为由基本数据类型组合而成的结构体与普通变量存储不一样，结构体变量占用空间不是简单的结构体成员空间单纯相加，其在内存中分配空间和数组很类似，可以总结为以下几点：
- 结构体变量占用的内存空间是其成员占用最大内存空间的整数倍 
	```cpp
	struct Person {
    int age;
    int hegiht;
    int weight;
	};
	int main(int argc, const char * argv[]) {
			struct Person p1 = {1, 2, 3};
			int size = sizeof(p1);
			printf("%i\n",size);              //12 = 4 * 3
			printf("变量的地址：%p\n",&p1);     //0x7fff5fbff750
			printf("%p\n",&p1.age);           //0x7fff5fbff750
			printf("%p\n",&p1.hegiht);        //0x7fff5fbff754
			printf("%p\n",&p1.weight);        //0x7fff5fbff758
			return 0;
	}
	```
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/98d604e6f1c9e6385f07330845a06b58.jpg)

	在 p1 变量中，由于其成员都是 int 类型，在 64 位编译器中占用 4 个字节空间，所以系统为 p1 变量分配的内存空间大小应该是：4 X 3 = 12 个字节。另外：结构体成员内存地址分配是从低地址到高地址的，结构体变量的地址是其首成员的地址，这点和数组是一致的。

- 系统从结构体首个成员分配空间；如果空间不够则重新分配，如果空间剩余则会把下一个成员的数据存储到剩余的空间中
	```cpp
	struct Student { 
      char id;
      double score;
      int  age;    
	};
	int main(int argc, const char * argv[]) { 
				struct Student s1 = {'A', 10.0, 15};  
				int size = sizeof(s1);   
				printf("%i\n",size);       //24
				printf("%p\n",&s1.id);     //0x7fff5fbff788
				printf("%p\n",&s1.score);  //0x7fff5fbff790
				printf("%p\n",&s1.age);    //0x7fff5fbff798
				return 0;
	}
	```
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/fb1e518d45725cbba8956f89fad051e1.jpg)

	系统为 s1 分配内存时以 sizeof(double)8 个字节为单位，所以为 s1.id 分配了 8 个字节的空间，但是由于 id 定义为 char 类型，所以只占了 8 个字节中数值最小的一个内存空间；由于前面剩下的 8 - 1 = 7 个字节不足以存放 double 类型的值，所以接着为 s1.score 分配 8 个字节并占满 8 个字节空间；最后为 s1.age 分配 8 个字节并占用了前 4 个字节空间。故 s1 变量在内存中占用的内存大小为 8 X 3 = 24 个字节。

	如果调换结构体 Student 成员之间的顺序如下，情况又会发生变化。
	```cpp
	struct Student {
				double score;
				char id;
				int  age;
	};
	int main(int argc, const char * argv[]) {
				struct Student s1 = {'A', 10.0, 15};
				int size = sizeof(s1);
				printf("%i\n",size); //16
				printf("%p\n",&s1.score) //0x7fff5fbff790
				printf("%p\n",&s1.id); //0x7fff5fbff790
				printf("%p\n",&s1.age); //0x7fff5fbff79c
				return 0;
	}
	```
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/0aa5cd382d3bb037586e79acddd87eab.jpg)

	这是由于第二次为 s1.id 分配内存时没有完全占满 8 个字节的空间，而且第三次为 s1.age 分配时其需要的 4 个字节空间也没有超出剩余的 8-1 = 7 个字节空间，所以 s1.age 的值按照内存对齐的原则就存放在了第二次分配的 8 个字节的后 4 位空间中。

- 为结构体变量分配内存需要遵循内存对齐原则

	系统在为不同类型的变量分配空间时按照一定的规则在空间上排列，而不是顺序的一个接一个的排放，这是对内存对齐的模糊解释。简单来说每个特定平台上的编译器都有自己的默认“对齐系数”（也叫对齐模数 n=1,2,4,8,16)。不同的对齐系数会对结构体变量分配的内存空间产生影响。

	结构体变量所占存储空间受其不同类型的成员排列顺序及编译器内存对齐影响，因此在开发中尽量将相同类型的成员依次定义，有助于节省内存空间。

## 5. 函数调用

函数作为编程中实现功能的重要手段，深入理解函数的调用过程对提升开发能力有很大的帮助。

### 5.1. 普通函数调用

与汇编程序设计中主程序和子程序之间的链接及信息交换类似，在高级语言编写的程序中（比如 C 语言），调用函数与被调用函数之间的链接及信息交换需要通过栈来进行，即系统将整个程序运行所需的数据空间安排在一个栈中，每当调用一个函数就为它在栈顶分配一个存储区，每当从一个函数退出时，就释放它的存储区，当前正在运行的函数存储区必在栈顶。

函数运行期间调用另外一个函数，在运行被调用函数之前，系统需要先完成 3 件事情：
1. 将所有的实参、返回地址等信息传递给被调用函数保存
1. 为被调用函数局部变量在栈上分配内存
1. 将控制转移到被调用函数入口

被调用函数返回调用函数之前，系统也需要相应的完成 3 件事情：
1. 在栈中保存被调用函数的计算结果（返回值）
1. 释放在栈中为被调用函数分配的数据区
1. 依照被调用函数保存的返回地址将控制转移到调用函数

栈帧也称为过程活动记录，是编译器用来实现过程 / 函数调用的一种数据结构。**每个栈帧对应着一个未运行完的函数，栈帧中保存了该函数形参、返回地址、局部变量等信息，简而言之栈帧就是一个函数执行的环境**。有时候函数嵌套调用，栈中会有多个函数的信息，每个函数占用一个连续的区域。

被调用函数的形参，在未出现函数调用时不占用内存空间，发生函数调用时，形参按照确定的类型在该函数帧中被分配指定大小的空间。并且由调用函数的实参传递给被调用函数的形参保存。

函数具体调用过程：
1. 为被调用函数在栈顶分配空间，为被调用函数的形参分配空间
1. 将实参的值传递给形参
1. 被调用函数利用形参（如果存在）进行运算
1. 通过 return 语句将返回值带回调用函数
1. 调用结束，释放被调用函数空间，释放其形参分配空间

eg:
```cpp
int add(int num1, int num2) {
         int tempSum = num1 + num2;
         return tempSum;
}
int main(int argc, const char * argv[]) {
          int a = 10;
          int b = 20;
          int sum = add(a, b);
          printf("%i\n",sum); //   sum = 30
          return 0;
}
```
上面的 main()、add() 函数之间调用的过程及参数传递在内存的示意图如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/ee22807cc1f012cd0a236aa30e178918.jpg)

1. 首先执行 main() 函数，系统为 main() 函数在栈顶分配一定大小的空间，其次为 a、b 局部变量分配空间；
1. 调用 add() 函数，main() 函数压入栈底，栈顶指针上移，系统为 add() 函数在栈顶分配一定大小的空间，其次为 num1、num2 局部变量分配空间；
1. 执行两个整数的加法运算，在 add() 函数帧中新开辟一块空间存放计算后的结果 tempSum；
1. 最后 add() 函数返回，在 main() 函数帧中开辟一块新的空间存放 add() 函数的返回值 sum，
1. add() 函数帧调用结束出栈，系统释放其空间并且栈顶指针下移，main() 函数重新回到栈顶。

注意：当前正在运行的函数存储区必在栈顶。

### 5.2. 递归函数调用

一个递归函数的运行过程类似于多个函数之间的嵌套调用，只是调用函数和被调用函数是同一个函数，因此，和每次调用相关的一个重要概念是递归函数运行的“层数”。

为了保证递归函数正确执行，系统建立一个“递归工作栈”作为整个递归函数运行期间使用的数据存储区，每一层递归所需信息构成一个“工作记录”，其中包括所有的实参、局部变量以及上一层的返回地址。每进入一层递归，就产生一个新的工作记录压入栈顶。每退出一层递归，就从栈顶弹出一个工作记录。当前活动的工作记录成为“活动记录”，并称活动记录的栈顶指针为“当前环境指针”。

假设调用该递归函数的主函数为第 0 层，主函数调用递归函数进入第一层；从第 i 层调用本函数为进入第 i+1 层，反之，退出第 i 层递归则应返回至第 i - 1 层。

eg:
```cpp
int getAge(int n) {
      if (n == 1) {
          return 10;
      } else {      
          return getAge(n-1) + 2;
      }
}
int main(int argc, const char * argv[]) {
      printf("NO.5.age: %d\n",getAge(5));
      return 0;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/8/eb24c7028dbf2ae50cdfe62f1dab9361.jpg)

一个递归问题可以分为“递推”和“回溯”两个阶段，要经历若干步才能求出最后的结果。但是其原理和一般函数的调用没有本质区别。递归函数调用次数越多，在栈上为其分配的空间就越大，所以我们应该避免调用次数过多的递归函数，因为该操作很可能会使栈的容量“溢出”。

## 6. Refer Links

[C 语言 - 内存管理基础](https://www.jianshu.com/p/b2380e47d005)

[C 语言 - 内存管理深入](https://www.jianshu.com/p/ccb337572498)