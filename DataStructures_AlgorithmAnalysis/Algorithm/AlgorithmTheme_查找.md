- [查找](#%E6%9F%A5%E6%89%BE)
  - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
  - [2. 顺序查找](#2-%E9%A1%BA%E5%BA%8F%E6%9F%A5%E6%89%BE)
  - [3. 分块查找](#3-%E5%88%86%E5%9D%97%E6%9F%A5%E6%89%BE)
  - [4. 二分查找](#4-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE)
  - [5. 插值查找](#5-%E6%8F%92%E5%80%BC%E6%9F%A5%E6%89%BE)
  - [6. 斐波那契查找](#6-%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%9F%A5%E6%89%BE)
  - [7. 树表查找](#7-%E6%A0%91%E8%A1%A8%E6%9F%A5%E6%89%BE)
    - [7.1. 二叉查找树](#71-%E4%BA%8C%E5%8F%89%E6%9F%A5%E6%89%BE%E6%A0%91)
    - [7.2. 二三树](#72-%E4%BA%8C%E4%B8%89%E6%A0%91)
    - [7.3. 红黑树](#73-%E7%BA%A2%E9%BB%91%E6%A0%91)
    - [7.4. B 树（B- 树）和 B+ 树](#74-b-%E6%A0%91%EF%BC%88b--%E6%A0%91%EF%BC%89%E5%92%8C-b-%E6%A0%91)
  - [8. 哈希查找](#8-%E5%93%88%E5%B8%8C%E6%9F%A5%E6%89%BE)
  - [9. 效率比较](#9-%E6%95%88%E7%8E%87%E6%AF%94%E8%BE%83)
  - [10. Refer Links](#10-refer-links)

# 查找

## 1. 基本概念

查找指的是在一个查找表中找出关键字等于给定值 K 的记录。若查找成功，则返回该记录的信息或该记录在表中的位置；否则，返回相关指示信息。

- 静态查找表（static search table）
  
  只对查找表进行“查找”操作的一类查找表。

- 动态查找表（dynamic search table）

  对查找表不仅进行“查找”操作，在查找过程中还同时插入新的数据元素或删除已有数据元素的一类查找表。

- 平均查找长度 ASL（average search length）

  查找运算的主要操作是关键字的比较，所以通常把查找过程中对关键字需要执行的平均比较次数（也称为平均查找长度）作为衡量一个查找算法效率优劣的标准。

  平均查找长度 ASL（average search length）定义为：

  ![image](http://img.cdn.firejq.com/jpg/2018/2/26/17738d12af641d438536b3dc3b9ae1fd.jpg)

  其中：
  - n 是记录的个数。
  - Pi 是查找到第 i 条记录的概率。若不特别声明，则认为查找每条记录的概率相等，即 `P1 = P2 = ... = Pn = 1/n`。
  - Ci 是找到第 i 条记录所需进行的比较次数。

  NOTE: **平均查找长度去掉系数之后就是时间复杂度**。

## 2. 顺序查找

顺序查找适用于存储结构为顺序存储或链接存储的线性表。

- 算法流程
  
  顺序查找也称为线形查找，属于无序查找算法。从数据结构线形表的一端开始，顺序扫描，依次将扫描到的结点关键字与给定值 k 相比较，若相等则表示查找成功；若扫描结束仍没有找到关键字等于 k 的结点，表示查找失败。

- 代码实现

  ```cpp
  // 顺序查找
  int SequenceSearch(int a[], int value, int n)
  {
      int i;
      for(i=0; i<n; i++)
          if(a[i]==value)
              return i;
      return -1;
  }
  ```

- 时间复杂度分析

  - 查找成功时的平均查找长度为：（假设每个数据元素的概率相等） ASL = 1/n(1+2+3+…+n) = (n+1)/2 ;
  - 当查找不成功时，需要 n+1 次比较，时间复杂度为 O(n);
　所以，顺序查找的时间复杂度为 O(n)。

- 优点
  - 算法简单，容易实现。
  - 对表的结构无任何要求，无论用向量还是用链表来存放结点，也无论结点之间是否按关键字有序，它都同样适用。

- 缺点
  - 查找效率低，平均查找长度较大，特别是当 n 很大时，查找效率较低。

## 3. 分块查找

分块查找又称索引顺序查找，它是顺序查找的一种改进方法，**性能介于顺序查找和二分查找之间**。

- 算法流程
  
  1. 先选取各块中的最大关键字构成一个索引表：
     
      将 n 个数据元素"按块有序"划分为 m 块（m ≤ n）。每一块中的结点不必有序，但块与块之间必须"按块有序"（第 1 块中任一元素的关键字都必须小于第 2 块中任一元素的关键字；而第 2 块中任一元素又都必须小于第 3 块中的任一元素，以此类推）。

      例如，下图所示为一个表及其索引表，表中含有 18 条记录，可分成 3 块，每块包括 6 条记录，对每一个块建立一个索引项。索引表中的元素是一个结构体，包含关键字和块的起始位置，其中关键字按升序排列，即索引表为有序，各个块中的元素为有序或者分块有序。分块有序是指后一个块中的所有记录的关键字均大于前一个块中的最大关键字。

      ![image](http://img.cdn.firejq.com/jpg/2018/2/27/38243821f067005b278ea5e2562ecaf5.jpg)

  1. 查找分两个部分：
      - 先查找索引表。索引表是有序表，可进行折半查找或顺序查找，以确定待查的结点在哪一块。
      - 再在已确定的块中使用顺序查找进行查找。

- 优点
  - 在表中插入或删除一个记录时，只要找到该记录所属的块，就在该块内进行插入和删除运算。
  - 因为块内记录的存放是任意的，所以插入或删除比较容易，无须移动大量记录。
- 缺点
  - 增加了一个辅助数组的存储空间。
  - 增加了将初始表分块排序的运算。

## 4. 二分查找

二分查找的思想在 1946 年就被提出，但第一个没有 bug 的二分查找实现到 1962 年才出现。**二分查找适用于顺序存储的有序表**。

- 算法流程
  
  二分查找也称为折半查找，属于有序查找算法。用给定值 k 先与中间结点的关键字比较，中间结点把线形表分成两个子表，若相等则查找成功；若不相等，再根据 k 与该中间结点关键字的比较结果确定下一步查找哪个子表，这样递归进行，直到查找到或查找结束发现表中没有这样的结点。

- 代码实现

  ```cpp
  // 二分查找（折半查找），非递归实现
  // 如果找到 value, 返回相应的索引 index；如果没有找到 value, 返回 -1
  int BinarySearch(int a[], int value, int n)
  {
      int low, high, mid;
      low = 0;
      high = n-1;
      while(low<=high)
      {
          //int mid = (l + r)/2; // 这种写法若 l 和 r 刚好是 int 的最大值，会导致程序溢出
          int mid = l + (r-l)/2; // 使用减法代替加法，从而防止极端情况下的整形溢出
          if(a[mid]==value)
              return mid;
          if(a[mid]>value)
              high = mid-1;
          if(a[mid]<value)
              low = mid+1;
      }
      return -1;
  }
  ```

  ```cpp
  // 二分查找，递归实现
  // 如果找到 value, 返回相应的索引 index；如果没有找到 value, 返回 -1
  int BinarySearch(int a[], int value, int low, int high)
  {
      if (low > high)
          return -1;
      //int mid = (l + r)/2; // 这种写法若 l 和 r 刚好是 int 的最大值，会导致程序溢出
      int mid = l + (r-l)/2; // 使用减法代替加法，从而防止极端情况下的整形溢出
      if(a[mid]==value)
          return mid;
      if(a[mid]>value)
          return BinarySearch(a, value, low, mid-1);
      if(a[mid]<value)
          return BinarySearch(a, value, mid+1, high);
  }
  ```

- 时间复杂度分析

  最坏情况下，关键词比较次数为 log(n+1)，时间复杂度的期望值为 O(logn)。

- 特点

  对于静态查找表，即一次排序后不再变化的查找表，使用折半查找能得到不错的效率。但对于需要频繁执行插入或删除操作的数据集来说，维护有序的排序会带来不小的工作量，那就不建议使用。

- [相关题目](https://www.nowcoder.com/questionTerminal/468dafd5578047fb993d9b5db35f3ea4)
  - 二分查找只能用于数组。（×）
  - 二分查找只能用于顺序结构。（√）（链式结构需借助跳转表 (skiplist) 才能实现二分查找）

- 利用二分查找的思想，实现 floor() 和 ceil() 函数
  ```cpp
  // 二分查找法，在有序数组 arr 中，查找 target
  // 如果找到 target, 返回第一个 target 相应的索引 index
  // 如果没有找到 target, 返回比 target 小的最大值相应的索引，如果这个最大值有多个，返回最大索引
  // 如果这个 target 比整个数组的最小元素值还要小，则不存在这个 target 的 floor 值，返回 -1
  template<typename T>
  int floor(T arr[], int n, T target){
      assert( n >= 0  );

      // 寻找比 target 小的最大索引
      int l = -1, r = n-1;
      while( l < r ){
          // 使用向上取整避免死循环
          int mid = l + (r-l+1)/2;
          if( arr[mid] >= target )
              r = mid - 1;
          else
              l = mid;
      }
      assert( l == r );
      // 如果该索引 +1 就是 target 本身，该索引 +1 即为返回值
      if( l + 1 < n && arr[l+1] == target )
          return l + 1;

      // 否则，该索引即为返回值
      return l;
  }

  // 二分查找法，在有序数组 arr 中，查找 target
  // 如果找到 target, 返回最后一个 target 相应的索引 index
  // 如果没有找到 target, 返回比 target 大的最小值相应的索引，如果这个最小值有多个，返回最小的索引
  // 如果这个 target 比整个数组的最大元素值还要大，则不存在这个 target 的 ceil 值，返回整个数组元素个数 n
  template<typename T>
  int ceil(T arr[], int n, T target){
      assert( n >= 0 );

      // 寻找比 target 大的最小索引值
      int l = 0, r = n;
      while( l < r ){
          // 使用普通的向下取整即可避免死循环
          int mid = l + (r-l)/2;
          if( arr[mid] <= target )
              l = mid + 1;
          else // arr[mid] > target
              r = mid;
      }
      assert( l == r );
      // 如果该索引 -1 就是 target 本身，该索引 +1 即为返回值
      if( r - 1 >= 0 && arr[r-1] == target )
          return r-1;

      // 否则，该索引即为返回值
      return r;
  }
  ```

eg: [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/description/)
- Question
	> Implement int sqrt(int x).
	> 
	> Compute and return the square root of x, where x is guaranteed to be a non-negative integer.
	> 
	> Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

- Solution
	```java
	public int mySqrt(int x) {
			if (x == 0)
					return 0;
			int left = 1, right = Integer.MAX_VALUE;
			while (true) {
					int mid = left + (right - left)/2;
					if (mid * mid > x) 
							right = mid - 1;
					else if ((mid + 1) * (mid + 1) > x)
							return mid;
					else
							left = mid + 1;
			}
	}
	```

## 5. 插值查找

插值查找是对二分查找算法的改进，将查找点的选择由 1/2 改进为自适应选择，可以提高查找效率。因此，插值查找也属于有序查找，只适用于顺序存储的有序表。

- 首先考虑一个新问题，为什么上述算法一定要是折半，而不是折四分之一或者折更多呢？
  　　
  打个比方，在英文字典里面查“apple”，你下意识翻开字典是翻前面的书页还是后面的书页呢？如果再让你查“zoo”，你又怎么查？显然，这里你绝对不会是从中间开始查起，而是有一定目的的往前或往后翻。同样的，比如要在取值范围 1 ~ 10000 之间 100 个元素从小到大均匀分布的数组中查找 5， 我们自然会考虑从数组下标较小的开始查找。
  　　
  经过以上分析，折半查找这种查找方式，不是自适应的（也就是说是傻瓜式的）。二分查找中查找点计算如下：

  ```
  mid=(low+high)/2, 即 mid=low+1/2*(high-low);
  ```

  通过类比，我们可以将查找的点改进为如下：
  ```
  mid=low+(key-a[low])/(a[high]-a[low])*(high-low)，
  ```
  也就是**将上述的比例参数 1/2 改进为自适应的，根据关键字在整个有序表中所处的位置，让 mid 值的变化更靠近关键字 key，这样也就间接地减少了比较次数**。

- 代码实现
  ```cpp
  // 插值查找，递归实现
  int InsertionSearch(int a[], int value, int low, int high)
  {
      if (low > high)
          return -1;
      int mid = low+(value-a[low])/(a[high]-a[low])*(high-low);
      if(a[mid]==value)
          return mid;
      if(a[mid]>value)
          return InsertionSearch(a, value, low, mid-1);
      if(a[mid]<value)
          return InsertionSearch(a, value, mid+1, high);
  }
  ```

- 时间复杂度

  **查找成功或者失败的时间复杂度均为 O(log(log n))**。

- 对于表长较大，而关键字分布又比较均匀的查找表来说，插值查找算法的平均性能比折半查找要好的多。反之，数组中如果分布非常不均匀，那么插值查找未必是很合适的选择。

## 6. 斐波那契查找

斐波那契查找也是对二分查找的改进，通过运用黄金比例的概念在数列中选择查找点进行查找，提高查找效率。因此，斐波那契查找仅适用于顺序存储的有序表。

- 黄金比例又称黄金分割，是指事物各部分间一定的数学比例关系，即将整体一分为二，较大部分与较小部分之比等于整体与较大部分之比，其比值约为 1:0.618 或 1.618:1。

  斐波那契数列：1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89…….（从第三个数开始，后边每一个数都是前两个数的和）。然后我们会发现，随着斐波那契数列的递增，前后两个数的比值会越来越接近 0.618，利用这个特性，我们就可以将黄金比例运用到查找技术中。

- 算法流程

  ![image](http://img.cdn.firejq.com/jpg/2018/2/27/1d28688a887aca076e44b5d01cddfbd5.jpg)

  斐波那契查找与折半查找很相似，不同点在于它是根据斐波那契序列的特点对有序表进行分割的。

- 代码实现
  ```cpp
  // 斐波那契查找
  #include "stdafx.h"
  #include <memory>
  #include  <iostream>
  using namespace std;

  const int max_size=20;// 斐波那契数组的长度

  /*构造一个斐波那契数组*/ 
  void Fibonacci(int * F)
  {
      F[0]=0;
      F[1]=1;
      for(int i=2;i<max_size;++i)
          F[i]=F[i-1]+F[i-2];
  }

  // 定义斐波那契查找法
  // a 为要查找的数组，n 为要查找的数组长度，key 为要查找的关键字
  int FibonacciSearch(int *a, int n, int key)  
  {
    int low=0;
    int high=n-1;
    
    int F[max_size];
    Fibonacci(F);// 构造一个斐波那契数组 F 

    int k=0;
    while(n>F[k]-1)// 计算 n 位于斐波那契数列的位置
        ++k;

    int  * temp;// 将数组 a 扩展到 F[k]-1 的长度
    temp=new int [F[k]-1];
    memcpy(temp,a,n*sizeof(int));

    for(int i=n;i<F[k]-1;++i)
        temp[i]=a[n-1];
    
    while(low<=high)
    {
        int mid=low+F[k-1]-1;
        if(key<temp[mid])
        {
            high=mid-1;
            k-=1;
        } 
        else if(key>temp[mid]) 
        {
            low=mid+1;
            k-=2;
        } 
        else 
        {
            if(mid<n)
                return mid; // 若相等则说明 mid 即为查找到的位置
            else
                return n-1; // 若 mid>=n 则说明是扩展的数值，返回 n-1
        }
    }  
    delete [] temp;
    return -1;
  }

  int main()
  {
      int a[] = {0,16,24,35,47,59,62,73,88,99};
      int key=100;
      int index=FibonacciSearch(a,sizeof(a)/sizeof(int),key);
      cout<<key<<" is located at:"<<index;
      return 0;
  }
  ```

- 时间复杂度

  最坏情况下，时间复杂度为 O(logn)，且其期望复杂度也为 O(logn)。

## 7. 树表查找

<!-- TODO: 动态查找指的是查找表的结构本身是在查找过程中动态生成的，即对于给定值 key，若表中存在其关键字等于 key 的记录，则查找成功返回，否则插入关键字等于 key 的记录。 -->

### 7.1. 二叉查找树

二叉查找树是先对待查找的数据进行生成树，确保树的左分支的值小于右分支的值，然后在就行和每个节点的父节点比较大小，查找最适合的范围。 这个算法的查找效率很高，但是如果使用这种查找方法要首先创建树。 

它和二分查找一样，插入和查找的时间复杂度均为 O(logn)，但是在最坏的情况下仍然会有 O(n) 的时间复杂度。原因在于插入和删除元素的时候，树没有保持平衡，这也是设计自平衡查找树的初衷。

### 7.2. 二三树

2-3 树属于自平衡查找树，在一个完全平衡的 2-3 查找树中，根节点到每一个为空节点的距离都相同。而节点到叶节点的最长距离对应于查找算法的最坏情况，因此对于最坏的情况也具有对数复杂度。

- 在最坏的情况下，也就是所有的节点都是 2-node 节点，查找效率为 lgN。
- 在最好的情况下，所有的节点都是 3-node 节点，查找效率为 log3N 约等于 0.631lgN。

### 7.3. 红黑树

2-3 查找树能保证在插入元素之后能保持树的平衡状态，最坏情况下即所有的子节点都是 2-node，树的高度为 lgn，从而保证了最坏情况下的时间复杂度。但是 2-3 树实现起来比较复杂，于是就有了一种简单实现 2-3 树的数据结构，即红黑树（Red-Black Tree）。

整个树完全黑色平衡，即从根节点到所以叶子结点的路径上，黑色链接的个数都相同性质，从根节点到叶子节点的距离都相等。

红黑树的平均高度大约为 logn。

### 7.4. B 树（B- 树）和 B+ 树

维基百科对 B 树的定义为“在计算机科学中，B 树（B-tree）是一种树状数据结构，它能够存储数据、对其进行排序并允许以 O(log n) 的时间复杂度运行进行查找、顺序读取、插入和删除的数据结构。B 树，概括来说是一个节点可以拥有多于 2 个子节点的二叉查找树。与自平衡二叉查找树不同，B 树为系统最优化大块数据的读和写操作。B-tree 算法减少定位记录时所经历的中间过程，从而加快存取速度。普遍运用在数据库和文件系统。

B 和 B+ 树的区别在于，B+ 树的非叶子结点只包含导航信息，不包含实际的值，所有的叶子结点和相连的节点使用链表相连，便于区间查找和遍历。

B/B+ 树常用于文件系统和数据库系统中，它通过对每个节点存储个数的扩展，使得对连续的数据能够进行较快的定位和访问，能够有效减少查找时间，提高存储的空间局部性从而减少 IO 操作。它广泛用于文件系统及数据库中，如：
- Windows：HPFS 文件系统；
- Mac：HFS，HFS+ 文件系统；
- Linux：ResiserFS，XFS，Ext3FS，JFS 文件系统；
- 数据库：ORACLE，MYSQL，SQLSERVER 等中。

## 8. 哈希查找

前面介绍的各种查找方法都是将结点的关键字与待查找的值按照某种方式进行一一比较而实现的，查找的效率依赖于比较的次数。哈希查找将关键字的值与其存储位置之间建立某种映射关系而直接获得地址，避免了频繁的比较，将时间效率提高到了常量级别。

- 算法流程
  1. 用给定的哈希函数构造哈希表。
  1. 根据选择的冲突处理方法解决地址冲突。
  1. 在哈希表的基础上执行哈希查找。

- 时间复杂度

  哈希表是一个在时间和空间上做出权衡的经典例子。
  - 如果没有内存限制，那么可以直接将键作为数组的索引。那么所有的查找时间复杂度为 O(1)。
  - 如果没有时间限制，那么我们可以使用无序数组并进行顺序查找，这样只需要很少的内存。
  哈希表使用了适度的时间和空间来在这两个极端之间找到了平衡。只需要调整哈希函数算法即可在时间和空间上做出取舍。在查找之前我们需要构建相应的 Hash 表，若单纯论查找操作的时间复杂度，对于无冲突的 Hash 表而言，查找时间复杂度为 O(1)。

## 9. 效率比较

![image](http://img.cdn.firejq.com/jpg/2018/2/27/e67ef6a38ecc29d9143ad182e9dd67f9.jpg)

## 10. Refer Links

https://www.cnblogs.com/maybe2030/p/4715035.html

http://c.biancheng.net/cpp/u/shuju9/

https://www.cnblogs.com/yangecnu/p/Introduce-Hashtable.html
