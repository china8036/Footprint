- [常见问题](#常见问题)
  - [1. 素数判断](#1-素数判断)
  - [2. 进制转换](#2-进制转换)
    - [2.1. 二进制转化为十进制](#21-二进制转化为十进制)
    - [2.2. 十进制转二进制](#22-十进制转二进制)
    - [2.3. 十进制转八进制](#23-十进制转八进制)
    - [2.4. 十进制转换为任意进制](#24-十进制转换为任意进制)
    - [2.5. 任意进制转换成任意进制](#25-任意进制转换成任意进制)
  - [3. 回文判断](#3-回文判断)
    - [3.1. 回文数](#31-回文数)
    - [3.2. 回文串](#32-回文串)
  - [4. 求中位数](#4-求中位数)
    - [4.1. 静态求中位数](#41-静态求中位数)
    - [4.2. 动态求中位数](#42-动态求中位数)
  - [5. 实现一个 LRU](#5-实现一个-lru)
    - [5.1. 只使用 HashMap](#51-只使用-hashmap)
    - [5.2. 可使用 LinkedHashMap](#52-可使用-linkedhashmap)
  - [6. 背包问题](#6-背包问题)
    - [6.1. 定义](#61-定义)
    - [6.2. 分类](#62-分类)
    - [6.3. 解法](#63-解法)
      - [6.3.1. - 背包问题](#631---背包问题)
        - [6.3.1.1. 第一类问题](#6311-第一类问题)
        - [6.3.1.2. 第二类问题](#6312-第二类问题)
      - [6.3.2. 背包内物品的权值都一样，只有重量不一样，要让数量尽量多的背包问题](#632-背包内物品的权值都一样只有重量不一样要让数量尽量多的背包问题)
  - [7. 数据结构设计题](#7-数据结构设计题)
    - [7.1. 用两个队列实现一个栈](#71-用两个队列实现一个栈)
    - [7.2. 用两个栈实现一个队列](#72-用两个栈实现一个队列)
    - [7.3. 设计一个支持常数时间增加、删除和随机选择操作的数据结构](#73-设计一个支持常数时间增加删除和随机选择操作的数据结构)
    - [7.4. 设计一个带有 getMin 功能的栈](#74-设计一个带有-getmin-功能的栈)
  - [8. 最大公因数 && 最小公倍数](#8-最大公因数--最小公倍数)
  - [9. 众数 / 主元素](#9-众数--主元素)
  - [10. 生成随机数](#10-生成随机数)
    - [10.1. Java](#101-java)
    - [10.2. C++](#102-c)
  - [11. 实现随机数](#11-实现随机数)
  - [12. 洗牌算法实现](#12-洗牌算法实现)
  - [13. Refer Links](#13-refer-links)

# 常见问题

## 1. 素数判断

基于以下理论：
- 判断一个数是否为素数只需判断从 2 到 sqrt(n) 是否存在其约数。
- 除了 2 和 3 以外，所有素数都一定与 6 的倍数相邻。因此，在 5 到 sqrt(n) 中每 6 个数只判断 2 个约数即可，时间复杂度 O(sqrt(n)/3)。

```java
public boolean isPrime(int n) {
    if (n == 1)
        return false;
    if (n == 2 || n == 3)
        return true;
    if (n % 6 != 1 && n % 6 != 5)
        return false;
    for (int i = 5; i * i <= n; i += 6) 
        if (n % i == 0 || n % (i + 2) == 0)
            return false;
    return true;
}
```

## 2. 进制转换

### 2.1. 二进制转化为十进制

```cpp
// 二进制转化为十进制
int f(int n)
{
    int a=0;
    for(int i=0;n;i++)// 要从右到左用二进制的每个数去乘以 2 的相应次方求和；
    {
         int t=n%10;
         a+=t<<i;   // 左移 i 位，即：t 乘以 2 的 i 次方
         n/=10;
    }
    return a;
}
```

### 2.2. 十进制转二进制

```cpp
// 十进制转二进制   
void printbinary(const unsigned int val)  
{   
    for(int i = 16; i >= 0; i--)  
    {  
        if(val & (1 << i))  
            cout << "1";  
        else  
            cout << "0";  
    }  
}  
```

### 2.3. 十进制转八进制

```cpp
// 十进制转八进制   
int main()  
{  
    cout<<"input a number:"<<endl;  
    int d;  
    vector<int> vec;  
  
    cin>>d;  
    while (d)  
    {  
        vec.push_back(d%8);  
        d=d/8;  
    }  
  
    cout<<"the result is:"<<endl;  
    for(vector<int>::iterator ip=vec.end()-1;ip>=vec.begin();)  
    {  
        cout<<*ip--;  
    }  
    cout<<endl;  
      
    return 0;  
}  
```

```cpp
// 通过库函数实现八进制、十六进制输出
int main()  
{  
    int test=64;  
    cout<<"DEC:"<<test<<endl;  
    cout<<"OCT:"<<oct<<test<<endl;// 八进制   
    cout<<"HEX:"<<hex<<test<<endl;// 十六进制   
  
    return 0;  
}  
```

### 2.4. 十进制转换为任意进制

```cpp
// 十进制转换为任意进制 
int main()  
{  
    long n;  
    int p,c,m=0,s[100];  
    cout<<"输入要转换的数字："<<endl;  
    cin>>n;  
    cout<<"输入要转换的进制："<<endl;  
    cin>>p;  
  
    cout<<"("<<n<<")10="<<"(";  
  
    while (n!=0)// 数制转换，结果存入数组 s[m]   
    {  
        c=n%p;  
        n=n/p;  
        m++;s[m]=c;   // 将余数按顺序存入数组 s[m] 中   
    }  
  
    for(int k=m;k>=1;k--)// 输出转换后的序列   
    {  
        if(s[k]>=10) // 若为十六进制等则输出相对应的字母   
            cout<<(char)(s[k]+55);  
        else         // 否则直接输出数字   
            cout<<s[k];  
    }  
  
    cout<<")"<<p<<endl;  
  
    return 0;  
}  
```

### 2.5. 任意进制转换成任意进制

Java
```java
public class NumericConvertUtil {  
    /** 
     * 在进制表示中的字符集合 
     */  
  private final static char[] digits = {'0', '1', '2', '3', '4', '5', '6',
                      '7', '8', '9', 'A', 'B', 'C', 'D',
                      'E', 'F', 'G', 'H', 'I', 'J', 'K',
                      'L', 'M', 'N', 'O', 'P', 'Q', 'R',
                      'S', 'T', 'U', 'V', 'W', 'X', 'Y',
                      'Z'};
  
    /** 
     * 将十进制的数字转换为指定进制的字符串 
     *  
     * @param n 十进制的数字 
     * @param base 指定的进制 
     * @return 
     */  
    public static String toOtherBaseString(long n, int base) {  
        long num = 0;  
        if (n < 0) {  
            num = ((long) 2 * 0x7fffffff) + n + 2;  
        } else {  
            num = n;  
        }  
        char[] buf = new char[32];  
        int charPos = 32;  
        while ((num / base) > 0) {  
            buf[--charPos] = digits[(int) (num % base)];  
            num /= base;  
        }  
        buf[--charPos] = digits[(int) (num % base)];  
        return new String(buf, charPos, (32 - charPos));  
    }  
  
    /** 
     * 将其它进制的数字（字符串形式）转换为十进制的数字 
     *  
     * @param str 其它进制的数字（字符串形式） 
     * @param base 指定的进制 
     * @return 
     */  
    public static long toDecimalism(String str, int base) {  
        char[] buf = new char[str.length()];  
        str.getChars(0, str.length(), buf, 0);  
        long num = 0;  
        for (int i = 0; i < buf.length; i++) {  
            for (int j = 0; j < digits.length; j++) {  
                if (digits[j] == buf[i]) {  
                    num += j * Math.pow(base, buf.length - i - 1);  
                    break;  
                }  
            }  
        }  
        return num;  
    }  
  
    public static void main(String[] args) {  
        System.out.println(toOtherBaseString(16857120L, 36));  
        System.out.println(toDecimalism("A1B1AJASWDE", 36));  
    }  
}  
```

C++
```cpp
// 递归实现任意进制转换成任意进制 
int conservation[100];  // 保存结果的数组 
int number;             // 数组中保存结果实际的位数
int change(int base,int jinzhi)  
{  
    //base 为基数，jinzhi 为想要转化的进制   
    int a;  
    int b;  
    a=base%jinzhi;  
    b=base/jinzhi;  
    conservation[number++]=a;  
    if(b!=0)   return change(b,jinzhi);  
    else return 0;  
}  

int main()  
{  
    int base,jinzhi;  
    cout<<"input base and jinzhi:";  
    cin>>base>>jinzhi;  
    change(base,jinzhi);  
    for(int i=number-1;i>=0;i--)  
      cout<<conservation[i];  
    cout<<endl;  
    return 0;  
}
```

## 3. 回文判断

### 3.1. 回文数

```java
public boolean isPalindrome(int a) {
    int s = 0;
    for (int m = a, i = m % 10; m != 0; m /= 10, i = m % 10)
        s = s * 10 + i;
    return s == a ? true : false;
}
```

### 3.2. 回文串

```java
public boolean isPalindrome(String s) {
    for (int i = 0; i < s.length() / 2; i++)
        if (s.charAt(i) != s.charAt(s.length() - 1 - i))
            return false;
    return true;
}
```

## 4. 求中位数

求出序列中第 n/2 大的元素。要求时间复杂度为 O(n)。

### 4.1. 静态求中位数

所谓“静态”求中位数，指的是给定一个未排序的整数数组，找到其中位数。

找中位数是要找到序列有序时位置在中间的那个数，即 mid=(low+high)/2 时的那个数。可以联想到使用快速排序中 partition 的思想，找到最终位置在 n/2 的那个数。这也就是 BFPRT 算法。

但找中位数并不需要像快速排序那样找到每个数的最终位置，所以并不需要划分序列。**如果进行一次 partition(L,low,high) 排序后找到那个数的最终位置偏右，即他的最终位置大于 mid，则这个数比中位数大，又因为这个数右边的数都比这个数大，则可以 high=pos-1，将这个数右边的数都舍去再进行快排 partition。同理，如果最终位置偏左，则继续进行 low=pos+1，将小于中位数的数舍去再进行快排 partition。最后当 mid=(low+high)/2 时，函数结束，输出结果。**

当数组的长度是偶数时，要输出中间两个数的平均值。可以将 mid 的值返回到主函数中，输出 mid 位置和 mid+1 位置的两个数的平均值即可。

利用快排划分的思想，递归处理：
```cpp
int partition(int L[], int low, int high)
{
  int i, num=low;
  for(i=low+1;i<=high;i++)
  {
    if(L[i]<L[low])
    {
      swap(&L[i], &L[num+1]);
      num++;
    }
  }
  swap(&L[low],&L[num]);
  return num;
}

int getmid(int L[], int low, int high)
{
  int mid = (low+high)/2;
  while(1)
  {
    int pos = partition(L, low, high);
    if(pos == mid)
      break;
    else if(pos > mid)
      high = pos-1;
    else 
            low = pos+1;
  }
    return L[mid];
}
```

这种方法实际上是对数组进行了局部的排序，每次只排序 partition 后的一半，因为将快速排序的平均时间 O(nlogn) 降到了 O(n)。严谨的数学证明请参见《算法导论》。

<!-- TODO: 复杂度为什么是 O(n) ？尾递归优化？-->

### 4.2. 动态求中位数

《CC 150》 18.9

所谓“动态”求中位数，指的是对一个元素动态改变的数据流，在每次元素更改时求出其中位数。

由于数据是从一个数据流中读出来的，数据的数目随着时间的变化而增加。因此可以用一个数据容器来保存从流中读出来的数据，当有新的数据流中读出来时，这些数据就插入到数据容器中。那么这个数据容器用什么数据结构定义更合适呢？

如果能够保证数据容器左边的数据都小于右边的数据，这样即使左、右两边内部的数据没有排序，也可以根据左边最大的数及右边最小的数得到中位数。因此可以用如下思路来解决这个问题：**用一个最大堆实现左边的数据容器，用一个最小堆实现右边的数据容器**。

在实现中：
- 首先要**保证数据平均分配到两个堆中**，即两个堆中数据的数目之差不能超过 1。

- 还要**保证最大堆中里的所有数据都要小于最小堆中的数据**。

因此，可以**先把新的数据插入到最大堆中，接着把最大堆中的最大的数字拿出来插入到最小堆中**。由于最终插入到最小堆的数字都是原最大堆中最大的数字，这样就保证了最小堆中的所有数字都大于最大堆中的数字。

往堆中插入一个数据的时间效率是 O(logn)。由于只需 O(1) 时间就可以得到位于堆顶的数据，因此得到中位数的时间效率是 O(1)。

## 5. 实现一个 LRU

LRU (Least Recently Used) 即最近最少使用，通常用于缓存的淘汰策略实现。

> Q: 保证基本的 get 和 set 的功能的同时，还要保证最近访问 (get 或 put) 的节点保持在限定容量的 Cache 中，如果超过容量则应该把 LRU（近期最少使用）的节点删除掉。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/25/e27256840117242e5b0b9dc2c02f325f.jpg)

NOTE: **在面试中解决该问题时，由于代码量相对较多，应该尽量将代码分离开来，写成独立的方法，从而使得代码更加清晰并在最短时间内铺设好代码骨架**。

### 5.1. 只使用 HashMap

同时维护 1 个 Double Linked List 和 1 个 `HashMap<keyType, Node>`，其中 Node 为链表节点类型（包含 prev，next，key 和 value），以及元素数量 size。

- `add(key, value)`, O(1)
  1. 在 HashMap 中 get(key) 检查该 key 是否已存在。如果是，则通过 get(key) 得到链表节点后，更新节点中的 value 即可；如果不是，进入下一步。
  1. 检查 size+1 是否达到 limit。如果是，删除链表头部节点，同时在 HashMap 中删除头部节点的 key 对应的节点；如果不是，进入下一步。
  1. 在链表尾部添加一个新节点 Node(key, value)，同时在 HashMap 中 put(key, Node)。

- `get(key)`, O(1)
  1. 在 HashMap 中 get(key) 得到该 key 对应的链表节点，读取该节点得到该 key 对应的 value。
  1. 将该链表节点移动到链表尾部。

- `remove(key)`, O(1)
  1. 在 HashMap 中 get(key) 得到该 key 对应的链表节点，然后指向 remove(key)。
  1. 在链表中将该节点删除。

```java

```

### 5.2. 可使用 LinkedHashMap

LinkedHashMap（基于 HashMap 和双向链表的实现）提供了键值对的储存功能，且可根据其支持访问排序的特性来模拟 LRU 算法。简单来说，LinkedHashMap 在访问已存在元素或插入新元素时，会将该元素放置在链表的尾部，所以在链表头部的元素是最近最少未使用的元素，而这正是 LRU 算法的描述。由于其底层基于链表实现，所以对于元素的移动和插入操作性能表现优异。

如果在 LocalCache 中只使用一个 LRU Map，将产生性能问题：
- 单个 LinkedHashMap 中元素数量太多
- 高并发下读写锁限制
因此，可以在 LocalCache 中使用多个 LRU Map，并使用 key 来 hash 到某个 LRU Map 上，以此来提高在单个 LinkedHashMap 中检索的速度以及提高整体并发度。

## 6. 背包问题

### 6.1. 定义

背包问题（Knapsack problem）是一种组合优化的 NP 完全问题。问题可以描述为：给定一组物品，每种物品都有自己的重量和价格，在限定的总重量内，我们如何选择，才能使得物品的总价格最高。问题的名称来源于如何选择最合适的物品放置于给定背包中。

在计算机科学领域，人们对背包问题感兴趣的原因在于：
- 利用动态规划，背包问题存在一个伪多项式时间算法
- 把上面算法作为子程序，背包问题存在完全逼近多项式时间方案
- 作为 NP 完全问题，背包问题没有一种既准确又快速（多项式时间）的算法

### 6.2. 分类

- 0-1 背包问题
  
  有 n 种物品，如果限定每种物品只能选择 0 个或 1 个，则问题称为 0-1 背包问题。0-1 背包问题是最基本的背包问题。各类复杂的背包问题总可以变换为简单的 0-1 背包问题进行求解。

  - 不考虑重量只考虑权值（可理解为：背包可背无限重的东西），要求选出若干个项，使他们的权值之和刚好等于 target。
  - 考虑重量和权值，物品 j 的重量为 wj，权值为 pj（假定所有物品的重量和价格都是非负的）背包所能承受的最大重量为 W，要求选出若干个项，再总重量不超过 W 的前提下，使他们的权值之和尽量大。

- 无界背包问题（完全背包问题）
  
  如果不限定每种物品的数量，则问题称为无界背包问题。

- 有界背包问题（多重背包问题）

  如果限定物品 j 最多只能选择 b 个，则问题称为有界背包问题。

- 部分背包问题

  每个项可以按比例只取走一部分（比如盐、糖之类的），则称为部分背包问题。

- 最优装载问题

  背包内物品的权值都一样（直接忽略该因素），重量不一样，要求选择数量尽量多的物体，但总重量不超过 W。

- 乘船问题

  有 n 个人，第 i 个人重量为 wi。每艘船最大载重量为 W，且最多只能乘坐两个人。求方案使得用最少的船装载所有的人。

- 推广的背包问题还有二次背包问题，多维背包问题，多目标背包问题等。

### 6.3. 解法

http://love-oriented.com/pack/

#### 6.3.1. - 背包问题

##### 6.3.1.1. 第一类问题

http://www.cnblogs.com/Myhsg/archive/2009/08/29/1556460.html

http://mrknight.info/algorithm/2015/04/12/Simple-Knapsack/

http://www.programgo.com/article/9139994208/;jsessionid=255510294E5DEB1A8463B76763F43111

不考虑重量只考虑权值（可理解为：背包可背无限重的东西），要求选出若干个项，使他们的权值之和刚好等于 target。

- 解法 1：递归（回溯）实现
  ```cpp
  //weight[] 存放各个项的权值
  //knapsack（t, i）将回答是否能从权值 weight[i]~weight[n] 之间选出一些数，使得总和为 t，并在可能的情况下将所选定的权值打印出来
  bool knapsack(int t, int i) {
    if (t == 0) 
      return true;
    else {
      if (t < 0 || i > n) 
        return false;
      else {
        if (knapsack(t - weight[i], i + 1)) {
          //.... 打印 weight[i] 的代码
          return true;
        } else {
          return knapsack(t, i + 1);// 不可能有 i 的解
        }
      }
    }
  }
  ```

- 解法 2：非递归实现（用栈实现）
  ```cpp
  #include <iostream>
  #include <vector>

  using namespace std;

  vector<int> temp;
  vector<vector<int> > result;
  void SimpleKnapsack(vector<int> items, int capacity, int i)
  {
    if (capacity == 0)
    {
      result.push_back(temp);
      return;
    }
    else if (i >= items.size() || items[i] > capacity)
      return;

    temp.push_back(items[i]);
    SimpleKnapsack(items, capacity - items[i], i + 1);
    
    temp.pop_back();
    SimpleKnapsack(items, capacity, i + 1);
  }
  void SimpleKnapsack_stack(vector<int> items, int capacity)
  {
    int n = items.size(), sum = 0, i = 0, start = 0;
    vector<int> stack;
    vector<int> visited(n, 0);
    
    while (1)
    {
      for (i = start; i < n; ++i)
      {
        if (!visited[i] && items[i] + sum <= capacity)
        {
          sum += items[i];
          stack.push_back(i);
          temp.push_back(items[i]);
          visited[i] = 1;
          if (sum == capacity)
          {
            result.push_back(temp);
            break;
          }
        }
      }
      
      if (i == n)
      {
        if (stack.empty())
          return;
        int top = stack.back();
        stack.pop_back();
        temp.pop_back();
        visited[top] = 0;
        sum -= items[top];
        start = top + 1;
      }
    }
  }
  int main()
  {
    int w[] = { 2, 3, 5, 7, 8};
    vector<int> items(w, w + 5);

    //SimpleKnapsack(items, 15, 0);
    SimpleKnapsack_stack(items, 15);
  }
  ```

##### 6.3.1.2. 第二类问题

考虑重量和权值，物品 j 的重量为 wj，权值为 pj（假定所有物品的重量和价格都是非负的）背包所能承受的最大重量为 W，要求选出若干个项，再总重量不超过 W 的前提下，使他们的权值之和尽量大。

- 解法 1：递归实现

  http://blog.csdn.net/buyingfei8888/article/details/8972103
  ```
  递归式：
    当 wn>C 时，  f(n,C)=f(n-1,C);
    当 wn<=C 时，f(n，C) = max(f(n-1,C), vn+f(n-1, C-wn) );
  初始条件为：f(i, 0) = 0; f(0,i) = 0; f(0,0) = 0;
  ```

  ```cpp
  #include <stdio.h>  
  #define MAX 100  

  int weight[MAX];  
  int price[MAX];  
  int y[MAX]={0};  
    
  // 进行递归主要方法  
  int f(int t,int c){  
      if((t==0)||c<=0){  // 当物品个数为 0 或背包容积为 0 事退出  
        return 0;  
      }else{  
          for(int i=t-1;i>=0;i--){    
              if(weight[i]>c){   // 当物品重量大于背包容积  
                  y[i]=0;        // 此时物品不被选中  
                  return f(t-1,c);  // 在剩余物品中选取  
              }else{  
              int temp1=f(t-1,c); // 当第 t 个物品没被选中时  
              int temp2=price[i]+f(t-1,c-weight[i]);// 被选中时  
                  if(temp1>temp2){  
                      y[i]=0;  
                      return f(t-1,c);  
                  }else{  
                      y[i]=1;  
                      return price[i]+f(t-1,c-weight[i]);  
                  }  
              }  
          }  
      }  
  }  
  int main(){  
      int c,t,maxval,i;  
      printf("请输入物品的的个数：");  
      scanf("%d",&t);  
      for(i=0;i<t;i++){  
          y[i]=0;  
          printf("\n 请输入第 %d 个物品的重量和价值",i+1);  
          scanf("%d%d",&weight[i],&price[i]);  
      }  
      printf("\n 请输入背包的容积");  
      scanf("%d",&c);  
      maxval=f(t,c);  
      printf("j 结果为：1 代表选中");  
      for(i=0;i<t;i++){  
          //if(y[i]==1)  
              printf("\n%d %d %d\n",y[i],weight[i],price[i]);  
      }  
      printf("总价值为：%d",maxval);  
      return 0;  
  }
  ```

- 解法 2：枚举 / 蛮力法 O（2^n）：列出所有可能的情况再选最优

- 解法 3：动态规划

#### 6.3.2. 背包内物品的权值都一样，只有重量不一样，要让数量尽量多的背包问题

贪心法：

## 7. 数据结构设计题

### 7.1. 用两个队列实现一个栈

> 用两个队列，实现栈的从队尾插入元素 push() 和从队尾抛出元素 pop()。

假设有两个队列 Q1 和 Q2，当二者都为空时，入栈操作可以用入队操作来模拟，可以随便选一个空队列，假设选 Q1 进行入栈操作，现在假设 a,b,c 依次入栈了（即依次进入队列 Q1），这时如果想模拟出栈操作，则需要将 c 出栈，因为在栈顶，这时候可以考虑用空队列 Q2，将 a，b 依次从 Q1 中出队，而后进入队列 Q2，将 Q1 的最后一个元素 c 出队即可，此时 Q1 变为了空队列，Q2 中有两个元素，队头元素为 a，队尾元素为 b，接下来如果再执行入栈操作，则需要将元素进入到 Q1 和 Q2 中的非空队列，即进入 Q2 队列，出栈的话，就跟前面的一样，将 Q2 除最后一个元素外全部出队，并依次进入队列 Q1，再将 Q2 的最后一个元素出队即可。

在这个过程中，始终保证每次执行 push 或 pop 时，有一个 Queue 为空。

### 7.2. 用两个栈实现一个队列

> 用两个栈，实现队列的从栈底插入元素 push() 和从栈顶抛出元素 pop()。

入队时，将元素压入 s1。

出队时，将 s1 的元素逐个“倒入”（弹出并压入）s2，在 s1 栈底的元素，不用“倒入”s2（即只“倒”s1.Count()-1 个），直接弹出作为出队元素返回。之后再将 s2 中的元素逐个“倒回”s1。

优化：不用每次弹出后都将元素“倒回”，下次执行 pop 的时候再倒即可。

### 7.3. 设计一个支持常数时间增加、删除和随机选择操作的数据结构

[支持 O(1) 时间增加，删除和随机选择操作的数据结构](https://blog.csdn.net/whuwangyi/article/details/18897387)

[实现一个插入，删除，随机都是 O(1) 复杂度的 Set](https://blog.csdn.net/u010157717/article/details/42339539)

[O(1) 时间插入、删除和获取随机元素 - 允许重复](https://www.jianshu.com/p/ab1091eba727)

- Question:

  设计一个支持在平均时间复杂度为 O(1) 下，执行以下操作的数据结构：
  - insert(val)：向集合中插入元素 val。
  - remove(val)：当 val 存在时，从集合中移除一个 val。
  - getRandom()：从现有集合中随机获取一个元素，要求每个元素被返回的概率为 1/n。

  进一步设计：在以上要求的基础上，允许数据结构中出现重复元素，且调用 getRandom() 时，每个元素被返回的概率与其在集合中的数量呈线性相关，即 m/n。

- Solution:

  使用一个变长数组和一个 HashMap 组合即可，其中，变长数组用于保存所有元素，HashMap 的 key 为元素值，value 为元素在 ArrayList 中的索引：
  - Insert: 将元素添加到数组尾部，并以元素为 key、元素在 ArrayList 中的下标为 value 添加到 HashMap 中。
  - Get Randomly: 生成一个范围是 0~n-1 的随机索引，返回数组中该索引位置的元素。
  - Delete: 
    1. 在 HashMap 中找到该元素 x 的 value，即在数组中的索引值 xi。
    1. 在 HashMap 中删除该元素 x。
    1. 将数组的最后一个元素 y 复制到索引 xi 的位置，覆盖被删除元素。
    1. 删除数组最后一个元素 y。
    1. 在 HashMap 中将 key 为元素 y 的 map 的 value 更新为 xi。

  ```java
  public class RandomSet<E> {
      List<E> dta = new ArrayList<E>();
      Map<E, Integer> idx = new HashMap<E, Integer>();
      public boolean add(E item) {
          if (idx.containsKey(item))
              return false;
          idx.put(item, dta.size());
          dta.add(item);
          return true;
      }
      public E removeAt(int id) {
          if (id >= dta.size())
              return null;
          
          E res = dta.get(id);
          idx.remove(res);
          E last = dta.remove(dta.size() - 1);
          // skip filling the hole if last is removed
          if (id < dta.size()) {
              idx.put(last, id);
              dta.set(id, last);
          }
          return res;
      }
      public boolean remove(E item) {
          Integer id = idx.get(item);
          if (id == null) {
              return false;
          }
          removeAt(id);
          return true;
      }
      public E random(Random rnd) {
          if (dta.isEmpty()) {
              return null;
          }
          int id = rnd.nextInt(dta.size());
          return removeAt(id);
      }
  }
  ```

  对于要求允许重复元素的设计，只需将上述结构中，HashMap 的 value 更改为一个 HashSet，并在 HashSet 中存储所有相同元素值的数组索引即可。

### 7.4. 设计一个带有 getMin 功能的栈

使用一个辅助栈，用于存储当前所有元素中的最小值。

```java
public class MyStack {
    private Stack<Integer> stackData;
    private Stack<Integer> stackMin;

    public MyStack() {
        stackData = new Stack<Integer>();
        stackMin = new Stack<Integer>();
    }

    public void push(int num) {
        if (stackMin.isEmpty()) {
            stackMin.push(num);
        } else if (getMin() > num) { // 入栈时维护 min 栈
            stackMin.push(num);
        }
        stackData.push(num);
    }

    public int pop() {
        if (this.stackData.isEmpty()) {
            throw new RuntimeException("stack is empty");
        }
        Integer value = stackData.pop();
        if (value == getMin()) { // 出栈时维护 min 栈
            stackMin.pop();
        }

        return value;

    }

    public int getMin() {
        if (stackMin.isEmpty()) {
            throw new RuntimeException("stack is empty");
        }
        return stackMin.peek();
    }

}
```
## 8. 最大公因数 && 最小公倍数

```java
// 使用辗转相除法求最大公因数 (Greatest common divisor)(a < b) 
public int gcd(int a, int b) {
    if (b == 0)
        return a;
    int c = a % b;
    return c == 0 ? b : gcd(b, c);
}

// 求最小公倍数 (Least common multiple)
public int lcm(int a, int b) { 
    return a * b / gcd(a, b);
}
```

## 9. 众数 / 主元素

基本概念：
- 众数 / 主元素是指一个序列中出现次数最多的那个数。

- 绝对众数：若众数出现次数大于 n/2，则称该众数为绝对众数。

- 1/k 众数：若众数的出现次数大于 N/k，则称该众数为 1/k 众数。

解决思路：
- 计数数组（时间效率和空间效率都是 O(n)）
  
  首先求出序列中的最大值 max，然后定义一个计数数组 int count[max+1]，用来对数组中数字出现的次数进行计数，最后遍历 count 数组中取出最大值元素，其对应的下标即为出现次数最多的那个数。
  ```java
  public static void candidate (int[] array) {
      int[] count = new int[101];                // 计数数组，每个元素的默认值为 0
      for(int i = 0; i < array.length; i++)
          count[array[i]]++;                    // 对应的计数值加 1

      int maxCount = count[0];
      int maxNumber = 0;
      for(int i = 1; i < 100; i++)            // 找出最多出现的次数
          if(count[i] > maxCount)
              maxCount = count[i];
      for(int i = 0; i < 100; i++)            // 找出出现最多次的那个数字
          if(count[i] == maxCount)
              maxNumber = i;

      System.out.println("出现次数最多的数字为：" + maxNumber);
      System.out.println("该数字一共出现" + maxCount + "次");
  }
  ```

- 使用 HashMap（时间效率和空间效率都是 O(n)）

  首先遍历数组元素构造 HashMap，每个 Entry 的 key 存放数组中的数字，value 存放该数字出现的次数。然后遍历每个 Entry，找出最大 value 对应的 key，即是出现次数最多的那个数。
  ```java
  public static void candidate (int[] array) {
      HashMap<Integer, Integer> map = new HashMap<>();
      for (int i = 0; i < array.length; i++) 
          if (map.containsKey(array[i])) 
              map.put(array[i], map.getOrDefault(array[i], 0) + 1);    
      
      // 找出 map 的 value 中最大值，也就是数组中出现最多的数字所出现的次数
      int maxCount = Collections.max(map.values());
      int maxNumber = 0;
      for (Map.Entry<Integer, Integer> entry : map.entrySet())
          if (entry.getValue() == maxCount)
              maxNumber = entry.getKey();
      System.out.println("出现次数最多的数字为：" + maxNumber);
      System.out.println("该数字一共出现" + maxCount + "次");
  }
  ```

- 若已知序列的众数是绝对众数：

  - 快排 partition
    
    可推得，该众数必定同时是该序列的中位数，因此，可通过快排 partition 的思路来求（时间效率是 O(n)，空间效率是 O(1)）。
    ```java
    while(pivot_index != middle) { 
        if (pivot_index < middle)
            left = pivot_index + 1;
        else 
            right = pivot_index - 1;
        pivot_index = partition(A, left, right);
    }
    ```

  - 摩尔投票法

    对于绝对众数，拥有这样的性质：**删除数组 A 中两个不同的数，绝对众数不变**。
    - 若两个数中有 1 个是绝对众数，则剩余的 N-2 个数中，绝对众数仍然大于 (N-2)/2。
    - 若两个数中没有绝对众数，显然不影响绝对众数。

    因此，可采用摩尔投票的方法，即在每一轮投票过程中，从数组中找出一对不同的元素，将其从数组中删除：
    1. 记 m 为候选绝对众数，初始化为数组第一个元素；记众数出现次数为 cnt，并初始化为 1。 
    1. 遍历数组 A：
      - 若 cnt==0，则 m=A[i]。
      - 若 cnt≠0 且 m≠A[i]，则 cnt--（即同时删掉 m 和 A[i]）。
      - 若 cnt≠0 且 m==A[i]，则 cnt++。

    eg: [Leetcode 169. Majority Element](https://leetcode.com/problems/majority-element/description/)
    ```java
    public int majorityElement(int[] nums) {
        int major = nums[0];
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            if (count == 0) {
                count++; 
                major = nums[i];
            } else if (major == nums[i])
                count++;
            else 
                count--;
        }
        return major;
    }
    ```

- 若已知序列是有序的：

  直接遍历序列，使用临时变量存储出现次数的最大值以及对于的元素即可（时间效率是 O(n)，空间效率是 O(1)）。

## 10. 生成随机数

### 10.1. Java

- 通过 `System.currentTimeMillis()` 来获取一个当前时间毫秒数的 long 型数字。

  eg: 
  ```java
  // 获取 [0, 100) 之间的 int 整数
  int rand = (int)(System.currentTimeMillis() % 100);
  ```

- 通过 `Math.random()` 返回一个 0（包含）到 1（不包含）之间的 double 值。

  eg: 
  ```java
  // 获取 [0, 100) 之间的 int 整数
  int rand = (int)(Math.random() * 100);
  ```

- 通过专业的 `Random` 工具类来产生一个随机数。

  API:
  ```java
  Random() 															// 默认构造方法。
  Random(long seed) 										// 指定种子数字构造一个新随机数生成器。

  boolean nextBoolean()         				// 返回下一个“boolean 类型”伪随机数。 
  void    nextBytes(byte[] buf) 				// 生成随机字节并将其置于字节数组 buf 中。 
  double  nextDouble()          				// 返回一个“[0.0, 1.0) 之间的 double 类型”的随机数。 
  float   nextFloat()           				// 返回一个“[0.0, 1.0) 之间的 float 类型”的随机数。 
  int     nextInt()             				// 返回下一个“int 类型”随机数。 
  int     nextInt(int n)        				// 返回一个“[0, n) 之间的 int 类型”的随机数。 
  long    nextLong()            				// 返回下一个“long 类型”随机数。 
  synchronized double nextGaussian()   	// 返回下一个“double 类型”的随机数，它是呈高斯分布的 double 值，其平均值是 0.0，标准偏差是 1.0。 
  synchronized void setSeed(long seed) 	// 使用单个 long 种子设置此随机数生成器的种子。
  ```

  eg: 
  ```java
  // 创建 Random 对象
  Random random = new Random(System.currentTimeMillis());	

  // 获取 [0, 100) 之间的 int 整数
  int rand = random.nextInt(100);
  ```
  
### 10.2. C++

```cpp
int generateRandom(int i, int j) 
{
  srand(time(null));
  int r = rand() % (j - i + 1) + i;
  return r;
}
```

## 11. 实现随机数

《CC 150 17.11》

- Question
  > 给定一个可以产生 0~4 随机数的函数 rand5()，编写 rand7() 方法，来产生 0~6 的随机数。

- Solution
  
  我们只需要产生出一个范围的数值，且每个数值出现的概率相同（这个范围至少有 7 个元素），然后舍弃后边大于 7 的倍数的部分，将余下元素除以 7 取余数即可。
  ```java
  public int rand7() {
    while (true) {
      int num = 5 * rand5() + rand5(); // 产生 0~24 的一个数 TODO: 直接 6 * rand5() ?
      if (num < 21) // 舍弃 21~24 之间的数值，否则 rand7() 返回 0~3 的值就会偏多
        return num % 7;
    }
  }
  ```

## 12. 洗牌算法实现

```java
public void shuffle(int [] a) {
  for (int i = 0; i < a.length; i++)
    Array.swap(a, i, rand(0, i));
}
```

## 13. Refer Links

[O(n) 时间快速选择](http://www.shadowxh.com/?p=598)

[找中位数 O(n) 算法](https://www.cnblogs.com/claireyuancy/p/7140840.html)

[快速排序详解及不排序求中位数 o（n）算法](https://blog.csdn.net/Q_M_X_D_D_/article/details/80108541)

[求无序数组的中位数](https://blog.csdn.net/zdl1016/article/details/4676882)

[数据流中的中位数](http://wiki.jikexueyuan.com/project/for-offer/question-sixty-four.html)

[最大最小堆计算动态中位数](http://gaocegege.com/Blog/algorithm/dynamicmedian)

[动手实现一个 LRU cache](http://blog.didispace.com/write-LRU-cache/)

[手写一个自己的 LocalCache - 基于 LinkedHashMap 实现 LRU](https://blog.csdn.net/Troy__/article/details/41014861)

[Leetcode: Graph Valid Tree 判断一个图是否为树](https://segmentfault.com/a/1190000005687618)

[Java 十进制转换 N 进制并反转换的工具类](http://jsczxy2.iteye.com/blog/1876499)

[设计一个具有 getMin 功能的栈](https://www.jianshu.com/p/36d0c209d29e)

[求最大公约数和最大公倍数 (Java 算法)](https://www.cnblogs.com/JumperMan/p/6529748.html)

[【LeetCode】寻找众数（绝对众数、1/k 众数）](https://blog.csdn.net/ljyljyok/article/details/78155333)

[找出数组中出现次数最多的那个数——主元素问题](https://www.cnblogs.com/eniac12/p/5296139.html)

[Java 随机数](https://www.cnblogs.com/skywang12345/p/3341423.html)

[剑指 offer 第二版 - 用队列实现一个栈](https://www.jianshu.com/p/99d919146bf5)