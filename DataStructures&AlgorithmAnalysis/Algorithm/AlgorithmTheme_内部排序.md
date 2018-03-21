- [内部排序](#%E5%86%85%E9%83%A8%E6%8E%92%E5%BA%8F)
	- [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
	- [2. 比较排序](#2-%E6%AF%94%E8%BE%83%E6%8E%92%E5%BA%8F)
		- [2.1. 插入排序](#21-%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
			- [2.1.1. 直接插入排序 (Straight insertion Sort)](#211-%E7%9B%B4%E6%8E%A5%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F-straight-insertion-sort)
			- [2.1.2. 希尔排序 (Shell Sort)](#212-%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F-shell-sort)
		- [2.2. 选择排序](#22-%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F)
			- [2.2.1. 直接选择排序 （Straight Selection Sort）](#221-%E7%9B%B4%E6%8E%A5%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F-%EF%BC%88straight-selection-sort%EF%BC%89)
			- [2.2.2. 堆排序 (Heap Sort)](#222-%E5%A0%86%E6%8E%92%E5%BA%8F-heap-sort)
		- [2.3. 交换排序](#23-%E4%BA%A4%E6%8D%A2%E6%8E%92%E5%BA%8F)
			- [2.3.1. 冒泡排序 (Bubble Sort)](#231-%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F-bubble-sort)
			- [2.3.2. 快速排序 (Quick Sort)](#232-%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F-quick-sort)
				- [2.3.2.1. 针对短序列的优化：直接使用插入排序](#2321-%E9%92%88%E5%AF%B9%E7%9F%AD%E5%BA%8F%E5%88%97%E7%9A%84%E4%BC%98%E5%8C%96%EF%BC%9A%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
				- [2.3.2.2. 针对基本有序序列的优化：随机化基准](#2322-%E9%92%88%E5%AF%B9%E5%9F%BA%E6%9C%AC%E6%9C%89%E5%BA%8F%E5%BA%8F%E5%88%97%E7%9A%84%E4%BC%98%E5%8C%96%EF%BC%9A%E9%9A%8F%E6%9C%BA%E5%8C%96%E5%9F%BA%E5%87%86)
				- [2.3.2.3. 针对重复元素的优化：三路快排](#2323-%E9%92%88%E5%AF%B9%E9%87%8D%E5%A4%8D%E5%85%83%E7%B4%A0%E7%9A%84%E4%BC%98%E5%8C%96%EF%BC%9A%E4%B8%89%E8%B7%AF%E5%BF%AB%E6%8E%92)
		- [2.4. （二路）归并排序 (Merge Sort)](#24-%EF%BC%88%E4%BA%8C%E8%B7%AF%EF%BC%89%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F-merge-sort)
	- [3. 非比较排序 / 分布排序](#3-%E9%9D%9E%E6%AF%94%E8%BE%83%E6%8E%92%E5%BA%8F-%E5%88%86%E5%B8%83%E6%8E%92%E5%BA%8F)
		- [3.1. 桶排序](#31-%E6%A1%B6%E6%8E%92%E5%BA%8F)
		- [3.2. 计数排序](#32-%E8%AE%A1%E6%95%B0%E6%8E%92%E5%BA%8F)
		- [3.3. 基数排序](#33-%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F)
	- [4. 综合比较](#4-%E7%BB%BC%E5%90%88%E6%AF%94%E8%BE%83)
	- [5. Refer Links](#5-refer-links)
# 内部排序

排序是数据处理过程中常用的一种运算，如何进行高效率的排序是计算机软件理论研究的重要课题之一。

排序方法按照排序过程中所涉及的存储器有内部排序和外部排序之分：
- 内部排序：待排序的数据记录全部在内存中进行排序。
- 外部排序：因待排序的数据很大，一次不能容纳全部的排序记录，在排序过程中需要访问外存。

以下说的排序算法均为内部排序。

## 1. 基本概念

- 排序算法的稳定性

  在待排序文件中，若存在**多个关键码相同的记录**，经过排序后，这些**具有相同关键码的记录之间的相对次序保持不变**，则称该排序方法是稳定的；否则，称这种排序方法是不稳定的。

  稳定性的好处：排序算法如果是稳定的，那么从一个键上排序，然后再从另一个键上排序，第一个键排序的结果可以为第二个键排序所用。基数排序就是这样，先按低位排序，逐次按高位排序，低位相同的元素其顺序再高位也相同时是不会改变的。另外，如果排序算法稳定，可以避免多余的比较。

- 排序算法的基本操作
  - 比较两个关键码的大小。
  - 将记录从一个位置移动到另一个位置。

- 排序算法的效率

  评价排序算法的效率主要有两点：
  - 在数据规模一定的条件下，算法执行所消耗的平均时间。
    
    对于排序操作，时间主要消耗在关键码之间的**比较**和数据元素的**移动**上，因此可认为**高效率的排序算法应该有尽可能少的比较次数和尽可能少的数据元素移动次数**。

  - 在数据规模一定的条件下，执行算法所需要的辅助存储空间。
    
    辅助存储空间是除了存放待排序数据元素占用的存储空间之外，执行算法所需要的其他存储空间。**理想的空间效率是，算法执行期间所需要的辅助空间与待排序的数据量无关**。

## 2. 比较排序

### 2.1. 插入排序

插入排序（insertion sort）的基本思想是，将待排序的记录，按其关键码的大小插入到已经排好序的有序子表中，直到全部记录插入完成为止。

通常需要一个辅助空间来存储当前待插入的记录，在插入时，需要移动已排序记录，为新插入的元素提供空间。

#### 2.1.1. 直接插入排序 (Straight insertion Sort)

直接插入排序将待排序的无序数列看成是一个仅含有一个元素的有序数列和一个无序数列，将无序数列中的元素逐个在有序数列中查找正确位置并插入，从而获得最终的有序数列。

如果碰见一个和插入元素相等的，那么插入元素把想插入的元素放在相等元素的后面。所以，**相等元素的前后顺序没有改变，从原无序序列出去的顺序就是排好序后的顺序，所以插入排序是稳定的**。

- 算法流程
  1. 初始时， a[0] 自成一个有序区， 无序区为 a[1, ... , n-1], 令 i=1。
  1. 将 a[i] 插入到当前的有序区 a[0, ... , i-1]。
  1. i++ 并重复第二步直到无序区为空，排序完成。
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/4763204802d27b45b1561c6ed2454e38.jpg)

- 时间复杂度：O(n^2)。

- 代码实现（按升序排序）
  ```cpp
  // 搜索和后移同时进行
  void StraightInsertionSort(int a[], int n)
  {
      int i, j, k;
      for(i=1; i<n; i++) // 从第二个元素开始
      {
          if(a[i] < a[i-1])
          {
              int temp = a[i];
              for(j=i-1; j>=0 && a[j]>temp; j--)
                  a[j+1] = a[j]; // 边移动元素边搜索合适的插入位置
              a[j+1] = temp;
          }
      }
  }
  ```
  
  精简版本（三行代码实现）：
  ```cpp
  // 用元素交换代替元素后移（比较对象只考虑两个元素)
  void StraightInsertionSort(int a[], int n)
  {
      for(int i=1; i<n; i++) // 从第二个元素开始
          for(int j=i-1; j>=0 && a[j]>a[j+1]; j--)
              Swap(a[j], a[j+1]);
  }
  ```

#### 2.1.2. 希尔排序 (Shell Sort)

希尔排序又叫缩小增量排序，是 1959 年由 D.L.Shell 提出来的，相比直接插入排序有较大的改进，被称为“最优的插入排序”。

希尔排序先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，从而减少参与直接插入排序的数据量。当经过几次的分组排序后，整个序列中的记录已“**基本有序**”，此时再对全体记录进行直接插入排序。

希尔排序是一种不稳定的排序算法。

- 算法流程
  1. 选择一个增量序列 t1，t2，…，tk，其中 ti>tj，tk=1。
  1. 按增量序列个数 k，对序列进行 k 趟排序。
  1. 每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/fea7c8df26d30bfe7f67c5c705bf215a.jpg)

- 时间复杂度：O(n^(1+e))（其中 0<e<1)，在元素基本有序的情况下，效率很高。

- 代码实现（按升序排序）
  ```cpp
  // 希尔排序
  void ShellSort(int a[], int n)
  {
      // 分组，初始步长为 n/2，且每次循环缩小一半
      for(int gap=n/2; gap>0; gap/=2)
          // 直接插入排序
          for(int i=gap; i<n; i+=gap)
              for(int j=i-gap; j>=0 && a[j]>a[j+gap]; j-=gap)
                  Swap(a[j], a[j+gap]);
  }
  ```

- 由于希尔排序是**通过不断缩小增量（步长）而将原始序列分成若干个子序列的**，这使得序列变得越来越有序，从而提高插入排序的效率。**希尔排序中的步长有各种不同的取法，一般认为，步长都取成奇数且步长之间互素比较好**。但如何选取步长为最好，至今在理论上仍没有得到论证，只是需要注意，步长因子中除 1 外没有公因子，且**最后一个步长因子必须是 1**。

### 2.2. 选择排序

选择排序（selection sort）的基本思想是，将排序序列分为有序区和无序区，每一趟从待排序无序区的记录中选出关键码最小的记录，顺序放在已排好序的有序区序列后面，直到无序区为空，则全部记录排序完毕。

#### 2.2.1. 直接选择排序 （Straight Selection Sort）

直接选择排序在要排序的一组数中，选出最小（或者最大）的一个数与第 1 个位置的数交换；然后在剩下的数当中再找最小（或者最大）的与第 2 个位置的数交换，依次类推，直到第 n-1 个元素（倒数第二个数）和第 n 个元素（最后一个数）比较为止。

直接选择排序是一种不稳定的排序算法。

- 算法流程
  1. 初始时，数组全为无序区 a[0, ... , n-1], 令 i=0;
  1. 在无序区 a[i, ... , n-1] 中选取一个最小的元素与 a[i] 交换，交换之后 a[0, ... , i] 即为有序区；
  1. 重复 2），直到 i=n-1，排序完成。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/ee46da9fd5cb7e18eba74317a5c9530c.jpg)

- 时间复杂度：O(n^2)。

- 代码实现（按升序排序）
  ```cpp
  // 直接选择排序
  void StraightSelectionSort(int a[], int n)
  {
      int minIndex;
      for(int i=0; i<n; i++)
      {
          minIndex=i;
          for(int j=i+1; j<n; j++)
              if(a[j]<a[minIndex])
                  minIndex=j;
          Swap(a[i], a[minIndex]);
      }
  }
  ```

#### 2.2.2. 堆排序 (Heap Sort)

堆排序是一种树形选择排序，是对直接选择排序的有效改进。堆排序（Heap Sort）是指若在输出堆顶的最大（小）值之后，使得剩余 n–1 个元素的序列又重建成一个堆，则得到 n 个元素中的次大（小）值。如此反复执行，便能得到一个有序序列。

**堆排序的实质就是对无序序列不断进行建堆和调整堆的过程。**为使排序后的记录序列按关键码非递减排列，在堆排序的实现过程中可先建一个最大堆，将堆顶记录与序列中最后一个记录进行交换，然后对序列中前 n–1 记录进行筛选，重新将它调整为一个最大堆，如此反复，直至排序结束。

排序过程中将数组分成了二叉堆区域（主要操作区，待排序）和有序区域（已排好序）：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/d944e8d4a4d3816c5fb67aa0211fb58f.jpg)

- 时间复杂度
  
  当记录数比较少时，堆排序的效率并不理想，但当记录数很大时，堆排序是非常有效的。在最坏的情况下，堆排序的时间复杂度也为 O(n log n)，同时堆排序的时间效率与待排序记录的初始次序无关，这是堆排序优于简单选择排序的地方。
  
  但堆排序也是不稳定排序，同时它需要一个 O(1) 的辅助空间。

- 代码实现
  ```cpp
  // 调整堆
  void HeapAdjusting(int a[], int root, int n)
  {
      int temp = a[root];
      int child = 2*root+1; // 左孩子的位置
      while(child<n)
      {
          // 找到孩子节点中较小的那个
          if(child+1<n && a[child+1]<a[child])
              child++;
          // 如果较大的孩子节点小于父节点，用较小的子节点替换父节点，并重新设置下一个需要调整的父节点和子节点。
          if(a[root]>a[child])
          {
              a[root] = a[child];
              root = child;
              child = 2*root+1;
          }
          else
              break;
          // 将调整前父节点的值赋给调整后的位置。
          a[root] = temp;
      }
  }

  // 初始化建堆
  void HeapBuilding(int a[], int n)
  {
      // 从最后一个有孩子节点的位置开始调整，最后一个有孩子节点的位置为 (n-1)/2
      for(int i=(n-1)/2; i>=0; i--)
          HeapAdjusting(a, i, n);
  }

  // 堆排序
  void HeapSort(int a[], int n)
  {
      // 初始化堆
      HeapBuilding(a, n);
      // 从最后一个节点开始进行调整
      for(int i=n; i>0; i--)
      {
          // 交换堆顶元素和最后一个元素
          Swap(a[1], a[i]);
          // 每次交换后都要进行调整
          HeapAdjusting(a, 1, i);
      }
  }
  ```

### 2.3. 交换排序

交换排序的基本思想是，将待排序记录的关键码进行两两比较，发现两个记录的次序相反时即进行交换，直到没有反序的记录为止。

#### 2.3.1. 冒泡排序 (Bubble Sort)

冒泡排序（bubble sort）是交换排序中一种简单的排序方法。冒泡排序将含有 n 个元素的序列分为无序区和有序区，对无序区进行 n 次遍历，每次遍历都对相邻的两个数依次进行比较和调整，即每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换，从而使得每次遍历都让无序区的最大值“下沉”到了最右端，相对较小的元素左移了一个位置（冒泡）。直到 n 次遍历结束或在某次遍历时没有任何元素被交换位置，则排序完成。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/30c07c2319cbf6b96dde9f1dac09007d.jpg)

- 时间复杂度：O(n^2)，冒泡排序是一种不稳定排序算法。

- 代码实现
  ```cpp
  // 冒泡排序
  void BubbleSort(int a[], int n)
  {
      // 最多遍历 n 次
      for(int i=0; i<n; i++)
      {
          int flag ==  0;
          // 每次遍历的起始位置为 0，终止位置为 n-i
          for(int j=0; j<n-i; j++)
              if(a[j]<a[j-1])
              {
                  Swap(a[j-1], a[j]);
                  flag = 1;
              }
          if(flag==0) break;
      }
  }
  ```

#### 2.3.2. 快速排序 (Quick Sort)

快速排序（quick sort）是 Hoare 于 1962 年提出的一种分区交换排序，它采用了**分而治之**的策略，是对冒泡排序的一种改进，在所有排序算法中使用得最广泛，速度也较快。在对随机分布的无序序列时，快速排序是速度最快的排序算法。

- 算法流程
  1. 在数据集之中，选择一个元素作为"基准"（pivot）。
  1. 所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
  1. 对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

  例：
  
  对{85, 24, 63, 45, 17, 31, 96, 50}进行排序。
  - 第一步，选择中间的元素 45 作为"基准"。（基准值可以任意选择，但是选择中间的值比较容易理解。）
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/6c1fc388fbba0324e28428a73ed6430b.jpg)
  - 第二步，按照顺序，将每个元素与"基准"进行比较，形成两个子集，一个"小于 45"，另一个"大于等于 45"。
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/71ecc929ab1eb240e994893a0fb3d3e1.jpg)
  - 第三步，对两个子集不断重复第一步和第二步，直到所有子集只剩下一个元素为止。
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/663e1107cd90a67042d6f5ca2a308e33.jpg)
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/3b3c7249a0304388b3c0d556179f5d27.jpg)
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/a4b047505b4b09452f45597554404648.jpg)
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/8c7a0779fee100060f37fbef0e782723.jpg)

- 时间复杂度
  - 最坏时间复杂度：O(n^2)。
    - 当所有元素都相同的情况下，partition 将一直返回 R, 递归的深度高达 N，每一次递归中 partition 又循环 N 次，时间复杂度到了 O(N^2)。
  - 平均时间复杂度：O(n log n)。

- 空间复杂度：O(log n)~O(n)。

  快速排序算法的实现需要栈的辅助，栈的递归深度为 O(log n)；当整个数列均有序时，栈的深度会达到 O(n)。

- 代码实现

  CPP 实现（递归）（CLRS 版本）
  ```cpp
  //Quick sort, L 为起始索引，R 为结束索引
  void Quicksort(int a[], int L, int R) {
    if (L < R) { // 当被排序的元素大于 1 个时才执行快排
      // partition
      int pivot = L; // 预留基准位置
      for (int i = L; i < R; i++) { // 遍历整个序列
        if (a[i] <= a[R]) { // 以末尾元素为基准元素
          Swap(a[i], a[pivot++]);// 将比基准元素小的元素移动到基准位置左边，同时右移基准位置
        }
      }
      Swap(a[R], a[pivot]); // 最后将基准元素放到基准位置

      Quicksort(a, L, pivot - 1); // 递归调用，对基准位置左边的子序列进行快排
      Quicksort(a, pivot + 1, R); // 递归调用，对基准位置右边的子序列进行快排
    }
  }
  ```

  JavaScript 实现（递归）
  ```javascript
  let quickSort = function(arr) {
      // 检查数组的元素个数，如果小于等于 1，就返回
  　　if (arr.length <= 1) { return arr; }
      // 选择"基准"（pivot），并将其与原数组分离，再定义两个空数组，用来存放一左一右的两个子集
      const pivotIndex = Math.floor(arr.length / 2);
      const pivot = arr.splice(pivotIndex, 1)[0];
      let left = [];
      let right = [];
      // 开始遍历数组，小于"基准"的元素放入左边的子集，大于基准的元素放入右边的子集
  　　for (let i = 0; i < arr.length; i++){
  　　　　if (arr[i] < pivot) {
  　　　　　　left.push(arr[i]);
  　　　　} else {
  　　　　　　right.push(arr[i]);
  　　　　}
  　　}
      // 使用递归不断重复这个过程，就可以得到排序后的数组
  　　return quickSort(left).concat([pivot], quickSort(right));

  };
  ```

##### 2.3.2.1. 针对短序列的优化：直接使用插入排序

对于任何高级排序算法，在对元素个数很少的元素进行排序时，都可以直接使用插入排序进行操作，其速度会优于高级排序算法。

```cpp
if (R - L <= 15)
{
  StraightInsertionSort(a, L, R);
  return;
}
```

##### 2.3.2.2. 针对基本有序序列的优化：随机化基准

由于快速排序算法中无法保证 partition 后的两个子序列长度相同，导致当序列中的基本有序时，快速排序递归调用时的递归树平衡性较差。在序列完全有序的极端情况下，快速排序退化到了 O(N^2)。

因此，可以使用了一个很简单的随机选取 pivot 的方式来处理这个问题。这步随机化让快速排序的时间期望成为了 O(nlogn)，并且只有极低的概率退化为 O(n^2)。关于这一点，背后的数学证明比较复杂，对背后的数学不感兴趣的同学，只要相信这个结论就好了。事实上，n 不需要太大，在 100 这个量级，其退化成 O(n^2) 算法的概率就已经低于大家彩票中大奖的概率了。

```cpp
swap(arr[R], arr[rand()%(r-l+1)+l]);// 在 l~r 范围内随机选择一个元素作为基准，并将基准元素放在序列末尾
```

##### 2.3.2.3. 针对重复元素的优化：三路快排

对于快速排序算法，当待排序序列中的元素随机分布时，算法可以取得非常可观的效率。但随着待排序序列中的重复（相等）元素增多，快排的效率显著下降。在所有元素都相同的极端情况下，partition 将一直返回 R, 递归的深度高达 N，每一次递归中 partition 又循环 N 次，时间复杂度到了 O(N^2)。

在传统的快速排序算法中，每次选取基准后，会将序列划分为小于基准和大于基准的两个部分，这称为双路排序。因此，针对有大量重复元素的序列，我们可以使用三路排序，即在每次选取基准后，将序列划分为小于基准、等于**基准**和大于基准的三个部分。在递归的过程中，中间部分不参与递归，分治的是两边。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/f9ee21efd3f1fb90ee352bcc769a5c96.jpg)

- 将基准元素放在头部：
  ```cpp
  void Quicksort(int a[], int L, int R) 
  {
      if (R > L) {
          if (R - L <= 15) {
            StraightInsertionSort(a, L, R);
            return;
          }
          swap(a[L], a[rand() % (R - L + 1) + L]);

          int v = a[L];       // v 为 pivot，初始存储在 a[L] 的位置
          int lt = L;         // 循环过程中保持 a[L+1...lt] < v
          int gt = R + 1;     // 循环过程中保持 a[gt...R] > v
          int i = L + 1;      // 循环过程中保持 a[lt+1...i) == v
          while(i < gt) {
              if(a[i] < v) {
                  swap( a[i++], a[++lt]); 
              } else if(a[i] > v) {
                  swap(a[i], a[--gt]); 
              } else { 
                  // a[i] == v
                  i++;
              }
          }
          swap(a[L], a[lt]);
          Quicksort(a, L, lt - 1); // 对 a[L...lt-1] 进行三路快排
          Quicksort(a, gt, R); // 对 a[gt...R] 进行三路快排
      }
  }
  ```

- 将基准元素放在末尾：
  ```cpp
  void Quicksort(int a[], int L, int R) {
      if (R - L <= 15) {
        StraightInsertionSort(a, L, R);
        return;
      }
      swap(a[R], a[rand() % (R - L + 1) + L]);
      int lt = L - 1;       
      int gt = R;    
      int i = L;       
      while (i < gt) {
          if (a[i] < a[R]) {
              swap(a[i++], arr[++lt]); 
          } else if (a[i] > a[R]) {
              swap(a[i], arr[--gt]); 
          } else { // arr[i] == v
              i ++;
          }
      }
      swap(arr[R], arr[lt]);
      Quicksort(a, L, lt - 1); // 对 a[L...lt-1] 进行三路快排
      Quicksort(a, gt, R); // 对 a[gt...R] 进行三路快排
  }
  ```

三路快排很好的解决了近乎有序的数组和有大量重复数组的元素排序问题，对于随机序列也有很好的性能，因此在很多语言的标准库中，排序接口使用的就是三路快排的思路，比如 Java 语言中的 Array.sort()。

- 经典例题：[Sort Color](https://leetcode.com/problems/sort-colors/)
  > 有一个数组，其中的元素取值只有可能是 0，1，2。为这样一个数组排序。
  - 解法一：计数排序，时间效率为 O(n)。

    只需要两次遍历：第一次统计元素个数，第二次放回原数组，就解决了问题。
    ```cpp
    // 基于计数排序的解法，时间复杂度 O(n)，需要两遍遍历
    void sortColors(vector<int>& nums) {
        int count[3] = {0};  // count[i] 表示元素 i 的个数
        for( int i = 0 ; i < nums.size() ; i ++ ){
            assert( nums[i] >= 0 && nums[i] <= 2 ); // 确保传入的 nums 中的每个元素，取值确实都是 0，1，2 的
            count[ nums[i] ] += 1;
        }

        int index = 0;
        for( int i = 0 ; i < count[0] ; i ++ ) // 安置 count[0] 个 0
            nums[index++] = 0;
        for( int i = 0 ; i < count[1] ; i ++ ) // 安置 count[1] 个 1
            nums[index++] = 1;
        for( int i = 0 ; i < count[2] ; i ++ ) // 安置 count[2] 个 2
            nums[index++] = 2;

        return;
    }
    ```

    - 解法二：三路快排，时间效率为 O(n)。

      如果我们对这个数组进行三路快排的话，并且选择 1 是 pivot，那么第一次 partition 的过程，就会把数组分成小于 1 的部分和大于 1 的部分，也就是所有的 0 在 1 的左边；所有的 2 在 1 的右边，就已经完成了排序。换句话说，我们对整个数组进行一次以 1 为 pivot 的三路快排的 partition 就完成了排序，整个过程只有一次遍历。

      ```cpp
      void sortColors(vector<int>& nums) {
          int i = 0;  // nums[0..<i) == 0
          int j = 0;  // nums[i..<j) == 1
          int k = nums.size(); // nums[k..<n) == 2

          while( j < k ){
              if( nums[j] == 1 )
                  j++;
              else if( nums[j] == 0 )
                  swap( nums[i++] , nums[j++] );
              else{ // nums[j] == 2
                  assert( nums[j] == 2 );
                  swap( nums[j] , nums[k-1] );
                  k --;
              }
          }

          return;
      }
      ```

### 2.4. （二路）归并排序 (Merge Sort)

归并排序（merge sort）是利用“归并”技术来进行排序的，它将两个（或两个以上）有序表合并成一个新的有序表。即把待排序序列分为若干个子序列，每个子序列是有序的，然后再把有序子序列合并为整体有序序列。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/d44203f25dc5d17384db0f024db15d92.jpg)

- 时间复杂度：O(nlog(n))，归并算法是一种稳定排序算法。

- 代码实现
  ```cpp
  //merge 两个有序数列为一个有序数列
  void MergeArr(int a[], int first, int mid, int last, int temp[])
  {
      int i = first, j = mid+1;
      int m = mid, n = last;
      int k=0;
      // 通过比较，归并数列 a 和 b
      while(i<=m && j<=n)
      {
          if(a[i]<a[j])
              temp[k++] = a[i++];
          else
              temp[k++] = a[j++];
      }
      // 将数列 a 或者 b 剩余的元素直接插入到新数列后边
      while(i<=m)
          temp[k++] = a[i++];
      while(j<=n)
          temp[k++] = a[j++];

      for(i=0; i<k; i++)
          a[first+i] = temp[i];
  }

  // 归并排序
  void MergeSort(int a[], int first, int last, int temp[])
  {
      if(first<last)
      {
          int mid = (first+last)/2;
          MergeSort(a, first, mid, temp);
          MergeSort(a, mid+1, last, temp);
          MergeArr(a, first, mid, last, temp);
      }
  }
  ```

## 3. 非比较排序 / 分布排序

### 3.1. 桶排序

https://www.cnblogs.com/maybe2030/p/4715042.html

http://www.acmerblog.com/bucket-sort-4884.html

### 3.2. 计数排序

### 3.3. 基数排序

## 4. 综合比较

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/c82ba1ca51b4da05d788119de27b2cdc.jpg)

（改错：上述快速排序算法的空间复杂度应改为 O(log2n)）

- 空间复杂度

	合并排序（根据 TAOCP，合并排序也有原地排序的版本），计数排序等不是原地排序。

	希尔排序、冒泡排序、插入排序、选择排序、堆排序、快速排序都属于原地排序。

- 时间复杂度
  - O(n^2)：各类简单排序，即直接插入排序、直接选择排序和冒泡排序。
  - O(n^(1+§))(0<§<1)：希尔排序。
  - O(nlogn)：快速排序、堆排序和归并排序。
  - O(n)：基数排序、桶排序、箱排序。
  说明：
  - 借助于“比较”进行排序的算法（比较排序算法）在最坏情况下能达到的最好的时间复杂度为 O(n log n)。
  - 当原表有序或基本有序时，直接插入排序和冒泡排序将大大减少比较次数和移动记录的次数，时间复杂度可降至 O（n）。而快速排序则相反，当原表基本有序时，将退化为冒泡排序，时间复杂度提高为 O（n^2）。
  - 原表是否有序，对简单选择排序、堆排序、归并排序和基数排序的时间复杂度影响不大。

- 稳定性
  
  四个不稳定排序：“快些选堆”。（其中“快”指快速排序，“些”指希尔排序，“选”指直接选择排序，“堆”指堆排序，即这四种排序方法是不稳定的，其他排序方法都是稳定的）

- 算法选择

  影响排序的因素有很多，平均时间复杂度低的算法并不一定就是最优的。相反，有时平均时间复杂度高的算法可能更适合某些特殊情况。同时，选择算法时还得考虑它的可读性，以利于软件的维护。一般而言，需要考虑的因素有以下四点：
  - 待排序的记录数目 n 的大小。
  - 记录本身数据量的大小，也就是记录中除关键字外的其他信息量的大小。
  - 关键字的结构及其分布情况。
  - 对排序稳定性的要求。

  设待排序元素的个数为 n，则：
  - 当 n 较大，则应采用时间复杂度为 O(nlogn) 的排序方法：快速排序、堆排序或归并排序。
  - 当 n 较小，可采用直接插入或直接选择排序。

	P.S.

	在 Java 的 java.util.DualPivotQuicksort 类中，定义了以下排序算法转换的阈值：
	```java
	/**
		* If the length of an array to be sorted is less than this
		* constant, Quicksort is used in preference to merge sort.
		*/
	private static final int QUICKSORT_THRESHOLD = 286;

	/**
		* If the length of an array to be sorted is less than this
		* constant, insertion sort is used in preference to Quicksort.
		*/
	private static final int INSERTION_SORT_THRESHOLD = 47;

	/**
		* If the length of a byte array to be sorted is greater than this
		* constant, counting sort is used in preference to insertion sort.
		*/
	private static final int COUNTING_SORT_THRESHOLD_FOR_BYTE = 29;

	/**
		* If the length of a short or char array to be sorted is greater
		* than this constant, counting sort is used in preference to Quicksort.
		*/
	private static final int COUNTING_SORT_THRESHOLD_FOR_SHORT_OR_CHAR = 3200;
	```
	- 当待排序元素的个数小于 47 时，插入排序比快速排序更加高效。
	- 当待排序元素的个数小于 286 时，快速排序比归并排序更加高效。
	- 对 byte 类型的数组而言，当待排序元素的个数大于 29 时，计数排序比插入排序更加高效。
	- 对 short 类型的数组而言，当待排序元素的个数大于 3200 时，计数排序比快速排序更加高效。

## 5. Refer Links

http://blog.csdn.net/jnu_simba/article/details/9705111

http://blog.csdn.net/hguisu/article/details/7776068

http://www.codeceo.com/article/10-sort-algorithm-interview.html

https://www.toptal.com/developers/sorting-algorithms/

http://jsdo.it/norahiko/oxIy/fullscreen

http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html

https://www.cnblogs.com/pugang/archive/2012/06/27/2565093.html

https://segmentfault.com/a/1190000002651247 

http://www.imooc.com/article/16141

http://zqdevres.qiniucdn.com/data/20101111204439/index.html

https://www.cnblogs.com/yangecnu/p/Introduce-Quick-Sort.html