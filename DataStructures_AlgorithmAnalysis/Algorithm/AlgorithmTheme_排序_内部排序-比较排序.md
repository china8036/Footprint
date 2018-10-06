- [内部排序 - 比较排序](#%E5%86%85%E9%83%A8%E6%8E%92%E5%BA%8F---%E6%AF%94%E8%BE%83%E6%8E%92%E5%BA%8F)
	- [1. 插入排序](#1-%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
		- [1.1. 直接插入排序 (Straight insertion Sort)](#11-%E7%9B%B4%E6%8E%A5%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F-straight-insertion-sort)
		- [1.2. 希尔排序 (Shell Sort)](#12-%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F-shell-sort)
	- [2. 选择排序](#2-%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F)
		- [2.1. 直接选择排序 （Straight Selection Sort）](#21-%E7%9B%B4%E6%8E%A5%E9%80%89%E6%8B%A9%E6%8E%92%E5%BA%8F-%EF%BC%88straight-selection-sort%EF%BC%89)
		- [2.2. 堆排序 (Heap Sort)](#22-%E5%A0%86%E6%8E%92%E5%BA%8F-heap-sort)
			- [2.2.1. 建堆优化](#221-%E5%BB%BA%E5%A0%86%E4%BC%98%E5%8C%96)
			- [2.2.2. 空间优化：原地堆排序](#222-%E7%A9%BA%E9%97%B4%E4%BC%98%E5%8C%96%EF%BC%9A%E5%8E%9F%E5%9C%B0%E5%A0%86%E6%8E%92%E5%BA%8F)
	- [3. 交换排序](#3-%E4%BA%A4%E6%8D%A2%E6%8E%92%E5%BA%8F)
		- [3.1. 冒泡排序 (Bubble Sort)](#31-%E5%86%92%E6%B3%A1%E6%8E%92%E5%BA%8F-bubble-sort)
		- [3.2. 快速排序 (Quick Sort)](#32-%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F-quick-sort)
			- [3.2.1. 针对短序列的优化：直接使用插入排序](#321-%E9%92%88%E5%AF%B9%E7%9F%AD%E5%BA%8F%E5%88%97%E7%9A%84%E4%BC%98%E5%8C%96%EF%BC%9A%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
			- [3.2.2. 针对基本有序序列的优化：随机化基准](#322-%E9%92%88%E5%AF%B9%E5%9F%BA%E6%9C%AC%E6%9C%89%E5%BA%8F%E5%BA%8F%E5%88%97%E7%9A%84%E4%BC%98%E5%8C%96%EF%BC%9A%E9%9A%8F%E6%9C%BA%E5%8C%96%E5%9F%BA%E5%87%86)
			- [3.2.3. 针对重复元素的优化：二路快排和三路快排](#323-%E9%92%88%E5%AF%B9%E9%87%8D%E5%A4%8D%E5%85%83%E7%B4%A0%E7%9A%84%E4%BC%98%E5%8C%96%EF%BC%9A%E4%BA%8C%E8%B7%AF%E5%BF%AB%E6%8E%92%E5%92%8C%E4%B8%89%E8%B7%AF%E5%BF%AB%E6%8E%92)
				- [3.2.3.1. 二路快排](#3231-%E4%BA%8C%E8%B7%AF%E5%BF%AB%E6%8E%92)
				- [3.2.3.2. 三路快排](#3232-%E4%B8%89%E8%B7%AF%E5%BF%AB%E6%8E%92)
			- [3.2.4. 算法思想扩展](#324-%E7%AE%97%E6%B3%95%E6%80%9D%E6%83%B3%E6%89%A9%E5%B1%95)
				- [3.2.4.1. 求无序数组的中位数](#3241-%E6%B1%82%E6%97%A0%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E4%B8%AD%E4%BD%8D%E6%95%B0)
				- [3.2.4.2. 求第 n 大](#3242-%E6%B1%82%E7%AC%AC-n-%E5%A4%A7)
	- [4. （两路）归并排序 (Merge Sort)](#4-%EF%BC%88%E4%B8%A4%E8%B7%AF%EF%BC%89%E5%BD%92%E5%B9%B6%E6%8E%92%E5%BA%8F-merge-sort)
		- [4.1. 针对短序列优化：直接使用插入排序](#41-%E9%92%88%E5%AF%B9%E7%9F%AD%E5%BA%8F%E5%88%97%E4%BC%98%E5%8C%96%EF%BC%9A%E7%9B%B4%E6%8E%A5%E4%BD%BF%E7%94%A8%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)
		- [4.2. 针对基本有序序列优化：提前终止](#42-%E9%92%88%E5%AF%B9%E5%9F%BA%E6%9C%AC%E6%9C%89%E5%BA%8F%E5%BA%8F%E5%88%97%E4%BC%98%E5%8C%96%EF%BC%9A%E6%8F%90%E5%89%8D%E7%BB%88%E6%AD%A2)
		- [4.3. 自顶向下与自底向上的比较](#43-%E8%87%AA%E9%A1%B6%E5%90%91%E4%B8%8B%E4%B8%8E%E8%87%AA%E5%BA%95%E5%90%91%E4%B8%8A%E7%9A%84%E6%AF%94%E8%BE%83)
		- [4.4. 算法思想扩展](#44-%E7%AE%97%E6%B3%95%E6%80%9D%E6%83%B3%E6%89%A9%E5%B1%95)
	- [5. 综合比较](#5-%E7%BB%BC%E5%90%88%E6%AF%94%E8%BE%83)
		- [5.1. 性能测试](#51-%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95)
		- [5.2. 空间复杂度](#52-%E7%A9%BA%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6)
		- [5.3. 时间复杂度](#53-%E6%97%B6%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6)
		- [5.4. 稳定性](#54-%E7%A8%B3%E5%AE%9A%E6%80%A7)
		- [5.5. 算法选择](#55-%E7%AE%97%E6%B3%95%E9%80%89%E6%8B%A9)
		- [5.6. 其它](#56-%E5%85%B6%E5%AE%83)
	- [6. Refer Links](#6-refer-links)

# 内部排序 - 比较排序

## 1. 插入排序

插入排序（insertion sort）的基本思想是，将待排序的记录，按其关键码的大小插入到已经排好序的有序子表中，直到全部记录插入完成为止。

通常需要一个辅助空间来存储当前待插入的记录，在插入时，需要移动已排序记录，为新插入的元素提供空间。

### 1.1. 直接插入排序 (Straight insertion Sort)

直接插入排序将待排序的无序数列看成是**一个仅含有一个元素的有序数列和一个无序数列**，将无序数列中的元素逐个在有序数列中查找正确位置并插入，从而获得最终的有序数列。

如果碰见一个和插入元素相等的，那么插入元素把想插入的元素放在相等元素的后面。所以，相等元素的前后顺序没有改变，从原无序序列出去的顺序就是排好序后的顺序，所以**直接插入排序是稳定的**。

- 算法流程
  1. 初始时，a[0] 自成一个有序区， 无序区为 a[1, ... , n-1], 令 i=1。
  1. 将 a[i] 插入到当前的有序区 a[0, ... , i-1]。
  1. i++ 并重复第二步直到无序区为空，排序完成。
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/4763204802d27b45b1561c6ed2454e38.jpg)

- 时间复杂度：O(n^2)。

	> Although it is one of the elementary sorting algorithms with O(n2) worst-case time, insertion sort is the algorithm of choice either when the data is nearly sorted (because it is adaptive) or when the problem size is small (because it has low overhead).
	> 
	> For these reasons, and because it is also stable, insertion sort is often used as the recursive base case (when the problem size is small) for higher overhead divide-and-conquer sorting algorithms, such as merge sort or quick sort.

	在以下情况时 StraightInsertionSort 能有极好的时间效率：
	- 有大量重复元素
	- 待排序序列基本有序 (nearly sorted)
	- 元素个数较少 (small problem size)
	在这些情况下，StraightInsertionSort 甚至比 O(nlogn) 的排序算法还要快。
	
	原因是：
	- StraightInsertionSort 虽然渐近时间复杂度为 O(n^2)，但当我们使用大 O 来描述算法的复杂度的时候，是忽略常数项的，而事实上 StraightInsertionSort 就是一个常数项非常小的排序算法（小于大多数排序）。因此，当 n 较小时，StraightInsertionSort 的耗时是可能会更少的。
	- 小数据量的数组，拥有更高的概率是近乎有序的。对于有序（或者近乎有序）的序列，StraightInsertionSort 的时间复杂度进化到 O(n) 级别（因为第二层循环可以提前终止）。
	- 相比快排，n 较小时，O(nlogn) 的时间效率优势不足以弥补其额外内存的申请和释放开销以及递归栈的开销。因此，当 n 较小时，快排是比 StraightInsertionSort 要慢的。

- 空间复杂度：O(1)。

- 代码实现（按升序排序）
	
	精简版本（三行代码实现）：
  ```cpp
  // 用元素交换代替元素后移（比较对象只考虑两个元素)
  void StraightInsertionSort(int a[], int n)
  {
      for (int i=1; i<n; i++) // 从第二个元素开始
          for (int j=i-1; j>=0 && a[j]>a[j+1]; j--)
              Swap(a[j], a[j+1]);
  }
  ```
	此版本的 StraightInsertionSort 每次比较都需要进行元素交换，而每次交换实际上都包含了 3 次赋值操作，因此效率稍低。
  
	优化版本（**每次循环都在找到合适的位置后再进行赋值，将多次赋值操作操作简化为 1 次，优化了效率**）：
  ```cpp
  // 搜索和后移同时进行
  void StraightInsertionSort(int a[], int n)
  {
      for(int i=1; i<n; i++) // 从第二个元素开始
      {
          if(a[i] < a[i-1])
          {
              int temp = a[i];
	            int j;
              for(j=i; j>=0 && a[j-1]>temp; j--)
                  a[j] = a[j-1]; // 边移动元素边搜索合适的插入位置
              a[j] = temp;
          }
      }
  }
  ```

### 1.2. 希尔排序 (Shell Sort)

希尔排序又叫缩小增量排序，是 1959 年由 D.L.Shell 提出来的，相比直接插入排序有较大的改进，被称为“最优的插入排序”。

希尔排序先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，从而减少参与直接插入排序的数据量。当经过几次的分组排序后，整个序列中的记录已“**基本有序**”，此时再对全体记录进行直接插入排序。

由于希尔排序是**通过不断缩小增量（步长）而将原始序列分成若干个子序列的**，这使得序列变得越来越有序，从而提高插入排序的效率。

**希尔排序是一种不稳定的排序算法**。

- 算法流程
  1. 选择一个增量序列 t1，t2，…，tk，其中 ti>tj，tk=1。
  1. 按增量序列个数 k，对序列进行 k 趟排序。
  1. 每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/fea7c8df26d30bfe7f67c5c705bf215a.jpg)

- 时间复杂度：O(n^(1+e))（其中 0<e<1)，在元素基本有序的情况下，效率极高。

- 空间复杂度：O(1)。

- [步长序列](https://en.wikipedia.org/wiki/Shellsort#Gap_sequences)
	
	Shell Sort 中 gap sequence 有多种不同的取法，其选择影响着排序的最终时间效率。**一般认为，步长都取成奇数且步长之间互素比较好**。但如何选取步长最好，至今在理论上仍没有得到论证：
	- 1959 年 Shell 提出的 n/(2^k) 序列时间复杂度为 O(n^2)。
	- 1971 年 Pratt 提出的 3n+1 序列时间复杂度为 O(n^(3/2))，权衡实现难度与时间效率，这种序列使用最为广泛。
	- 1986 年 Sedgewick 提出了 O(n^(4/3)) 的序列，但实现较为复杂。
	需要注意，步长因子中除 1 外没有公因子，且**最后一个步长因子必须是 1**。当步长为 1 时，算法变为插入排序，这也保证了数据一定会被排序。

- 代码实现
	
	- 使用 Shell 步长序列：
		```cpp
		// 希尔排序
		void ShellSort(int a[], int n)
		{
		    // 分组，初始步长为 n/2，且每次循环缩小一半
		    for(int gap=n/2; gap>0; gap/=2)
		        // 直接插入排序
		        for(int i=gap; i<n; i++) {
		            int temp = a[i];
		            int j;
		            for(j=i; j>=0 && a[j-gap]>temp; j-=gap)
		                    a[j] = a[j-1];
		            a[j] = temp;
		        }
		} 
		```
	- 使用 Pratt 步长序列：
		```cpp
		// 希尔排序
		void ShellSort(int a[], int n)
		{
		    // 计算 increment sequence: 1, 4, 13, 40, 121, 364, 1093...
		    int gap = 1;
		    while( gap < n/3 )
		        gap = 3 * gap + 1;

		    for(; gap>=1; gap/=3) {
		        // 直接插入排序
		        for(int i=gap; i<n; i++) {
		            if (a[i] < a[i-gap]) {
		                int temp = a[i];
		                int j;
		                for(j=i; j>=0 && a[j-1]>temp; j-=gap)
		                        a[j] = a[j-1];
		                a[j] = temp;
		            }
		        }
		    }
		} 
		```

## 2. 选择排序

选择排序（selection sort）的基本思想是，将排序序列分为有序区和无序区，每一趟从待排序无序区的记录中选出关键码最小 / 大的记录，顺序放在已排好序的有序区序列后面，直到无序区为空，则全部记录排序完毕。

### 2.1. 直接选择排序 （Straight Selection Sort）

直接选择排序在要排序的一组数中，选出最小（或者最大）的一个数与第 1 个位置的数交换；然后在剩下的数当中再找最小（或者最大）的与第 2 个位置的数交换，依次类推，直到第 n-1 个元素（倒数第二个数）和第 n 个元素（最后一个数）比较为止。

**直接选择排序是一种不稳定的排序算法**。

- 算法流程
  1. 初始时，数组全为无序区 a[0, ... , n-1], 令 i=0;
  1. 在无序区 a[i, ... , n-1] 中选取一个最小的元素与 a[i] 交换，交换之后 a[0, ... , i] 即为有序区；
  1. 重复 2），直到 i=n-1，排序完成。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/ee46da9fd5cb7e18eba74317a5c9530c.jpg)

- 时间复杂度：O(n^2)。

	直接选择排序每次循环都必须遍历相应的所有元素，而直接插入排序有提前终止的机制，因此**理论上直接插入排序应稍快于直接选择排序**。

- 空间复杂度：O(1)。

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
  

### 2.2. 堆排序 (Heap Sort)

堆排序是一种树形选择排序，是对直接选择排序的有效改进。

堆排序（Heap Sort）的基本思想是，在首先要将给定的待排序序列构造成一个最大 / 小堆，然后通过反复拿出最大 / 小堆的堆顶元素（每次拿出堆顶元素都需要用最后一个元素代替堆顶元素并进行向下调整），从而得到一个有序序列。

**堆排序是一种不稳定的排序算法**。

- 时间复杂度

	由于每次取出堆顶元素的时间效率为 O(logn)，因此对一个序列进行堆排序的时间复杂度为 O(nlogn)。

  当记录数比较少时，堆排序的效率并不理想，但**当记录数很大时，堆排序是非常有效的。同时堆排序的时间效率与待排序记录的初始次序无关，这是堆排序优于简单选择排序的地方**。

	由于堆排序相比其它排序算法，没有明显的优势，但拥有独特的特性，**更多地被应用在系统的动态数据结构中**。

- 空间复杂度：O(1)。

- 代码实现
	```cpp
	template<typename Item>
	class MaxHeap {
	private:
			Item *data; // 节点元素值
			int count; // 堆的节点数量
			int capacity; // 堆的最大容量
			
			// 对索引为 k 的节点进行向上调整
			void shiftUp(int k) { 
					for( ; k > 1 && data[k/2] < data[k]; k /= 2 ) {
							swap( data[k/2], data[k] );
					}
			}

			// 对索引为 k 的元素进行向下调整
			void shiftDown(int k){
					// 完全二叉树中若有孩子，则必定先有左孩子。因此这里通过判断是否有左孩子来判断是否有孩子
					while( 2*k <= count ) { 
							// j 表示将与 k 节点交换的节点，预设为左孩子
							int j = 2*k;
							// 若有右孩子，且右孩子大于左孩子，则交换右孩子
							if( j+1 <= count && data[j+1] > data[j] ) j ++;
							// 若较大的孩子节点小于 k 节点，说明已满足最大堆性质，结束调整
							if( data[k] >= data[j] ) break;
							// 否则，交接元素值
							swap( data[k] , data[j] );
							// 继续向下调整
							k = j;
					}
			}

			// 堆化
			void heapify() {
					for (int i = count / 2; i >= 1; i --) {
							shiftDown(i);
					}
			}
	public:
			// 构造函数，构造一个空堆，可容纳 capacity 个元素
			MaxHeap(int capacity) {
					data = new Item[capacity+1];
					count = 0;
					this->capacity = capacity;
			}

			// 构造函数，通过一个给定数组创建一个最大堆
			MaxHeap(Item arr[], int n){
					data = new Item[n+1];
					capacity = n;

					for( int i = 0 ; i < n ; i ++ )
							data[i+1] = arr[i];
					count = n;

					heapify();
			}

			// 析构函数
			~MaxHeap() {
					delete[] data;
			}

			// 向最大堆中插入一个新的元素 item
			void insert(Item item) {
					assert( count + 1 <= capacity );

					data[++count] = item;
					shiftUp(count);
			}

			// 从最大堆中删除堆顶元素并返回该元素的值
			Item extractMax() {
					assert( count > 0 ); // 判空

					Item ret = data[1];
					swap( data[1] , data[count--] );
					shiftDown(1);
					return ret;
			}
	}

	// 堆排序
	template<typename T>
	void heapSort(T arr[], int n){
			MaxHeap<T> maxHeap = new MaxHeap<T>(n);
			// 构造最大堆，时间复杂度为 O(nlogn)
			for (int i = 0; i < n; i ++) { 
					maxHeap.insert(arr[i]);
			}

			// 倒序赋值，实现使用最大堆进行从小到大的排序
			for (int i = n - 1; i >= 0; i --) {
					arr[i] = maxHeap.extractMax();
			}
	}
	```

#### 2.2.1. 建堆优化

在堆排序中，通过不断的向最大堆中插入待排序序列的元素，来构造一个最大堆，构造操作的时间复杂度为 O(nlogn)。

但**若直接将待排序序列构造成无序的完全二叉树，再对每一个非叶子节点进行向下调整，即 heapify 操作，构造操作的时间复杂度可优化为 O(n)**。虽然堆排序的总体时间复杂度依然是 O(nlogn)，但整个堆排序的性能还是得到了提升。

```cpp
template<typename Item>
class MaxHeap {
private:
		Item *data; // 节点元素值
		int count; // 堆的节点数量
		int capacity; // 堆的最大容量
		
		// 对索引为 k 的元素进行向下调整
		void shiftDown(int k){
				// 完全二叉树中若有孩子，则必定先有左孩子。因此这里通过判断是否有左孩子来判断是否有孩子
				while( 2*k <= count ) { 
						// j 表示将与 k 节点交换的节点，预设为左孩子
						int j = 2*k;
						// 若有右孩子，且右孩子大于左孩子，则交换右孩子
						if( j+1 <= count && data[j+1] > data[j] ) j ++;
						// 若较大的孩子节点小于 k 节点，说明已满足最大堆性质，结束调整
						if( data[k] >= data[j] ) break;
						// 否则，交接元素值
						swap( data[k] , data[j] );
						// 继续向下调整
						k = j;
				}
		}

		// 堆化
		void heapify() {
				for (int i = count / 2; i >= 1; i --) {
						shiftDown(i);
				}
		}
public:
		// 构造函数，通过一个给定数组创建一个最大堆
		MaxHeap(Item arr[], int n){
				data = new Item[n+1];
				capacity = n;
				// 直接构造一个无序的完全二叉树
				for( int i = 0 ; i < n ; i ++ )
						data[i+1] = arr[i];
				count = n;
				// 将该完全二叉树转换为最大堆
				heapify();
		}

		// 析构函数
		~MaxHeap() {
				delete[] data;
		}

		// 从最大堆中删除堆顶元素并返回该元素的值
		Item extractMax() {
				assert( count > 0 ); // 判空

				Item ret = data[1];
				swap( data[1] , data[count--] );
				shiftDown(1);
				return ret;
		}
}

template<typename T>
void heapSort2(T arr[], int n){
    MaxHeap<T> maxheap = MaxHeap<T>(arr,n);
    for( int i = n-1 ; i >= 0 ; i-- )
        arr[i] = maxheap.extractMax();

}
```

#### 2.2.2. 空间优化：原地堆排序

堆排序的过程中开辟了新的数组空间，造成了空间资源的消耗。因此，可以通过原地堆排序，来进行空间的优化。而且由于不需要开辟新空间、对新空间进行处理，这种空间优化同样起到了时间优化的作用。**这种版本的堆排序的也比使用建堆优化的堆排序更加高效。**

排序过程中将数组分成了二叉堆区域（主要操作区，待排序）和有序区域（已排好序）：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/d944e8d4a4d3816c5fb67aa0211fb58f.jpg)

```cpp
template<typename T>
void __shiftDown(T arr[], int n, int k){
    T e = arr[k];
    while( 2*k+1 < n ){
        int j = 2*k+1;
        if( j+1 < n && arr[j+1] > arr[j] ) j ++;
        if( e >= arr[j] ) break;
        arr[k] = arr[j];
        k = j;
    }
    arr[k] = e;
}

// 不使用一个额外的最大堆，直接在原数组上进行原地的堆排序
// 注意，此时的堆是从 0 开始索引的，非叶子节点从（最后一个元素的索引 - 1)/2 开始，最后一个元素的索引 = n-1
template<typename T>
void heapSort3(T arr[], int n){
    // 在原数组上进行堆化操作，初始化一个最大堆
    for( int i = (n-1-1)/2 ; i >= 0 ; i -- )
        __shiftDown(arr, n, i);
		// 每次将数组最后一个元素与最大堆的堆顶元素交换，并对新的堆顶元素进行向下调整
		// 相当于不断的把“最大”的元素排到队尾，从而实现从小到大排序
    for( int i = n-1; i > 0 ; i-- ){
        swap( arr[0] , arr[i] );
        __shiftDown(arr, i, 0);
    }
}
```

## 3. 交换排序

交换排序的基本思想是，将待排序记录的关键码进行两两比较，发现两个记录的次序相反时即进行交换，直到没有反序的记录为止。

### 3.1. 冒泡排序 (Bubble Sort)

冒泡排序（bubble sort）是交换排序中一种简单的排序方法。

冒泡排序的基本思想是，将含有 n 个元素的序列分为无序区和有序区，对无序区进行 n 次遍历，每次遍历都对相邻的两个数依次进行比较和调整，即每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换，从而**使得每次遍历都让无序区的最大值“下沉”到了最右端，相对较小的元素左移了一个位置 ( 冒泡 )**。直到 n 次遍历结束或在某次遍历时没有任何元素被交换位置，则排序完成。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/30c07c2319cbf6b96dde9f1dac09007d.jpg)

**冒泡排序是一种稳定的排序算法**。

- 时间复杂度：O(n^2)。

	由于冒泡排序每次循环都必须遍历所有相应的元素，且有大量的交换操作，因此**在时间效率上，冒泡排序 < 直接选择排序 < 直接插入排序**。

- 空间复杂度：O(1)。

- 代码实现
  ```cpp
  // 冒泡排序
  void BubbleSort(int a[], int n)
  {
      // 最多遍历 n 次
      for (int i=0; i<n; i++)
      {
          int flag =  0;
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

### 3.2. 快速排序 (Quick Sort)

快速排序（quick sort）是 Hoare 于 1962 年提出的一种**基于分治思想**的分区交换排序，它采用了**分而治之**的策略，是对冒泡排序的一种改进，在所有排序算法中使用得最广泛，速度也较快。

**在对随机分布的无序序列进行排序时，快速排序是速度最快的排序算法**。

- 算法流程
  1. 在数据集之中，选择一个元素作为"基准"（pivot）。
  1. 所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
  1. 对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

  例：对{85, 24, 63, 45, 17, 31, 96, 50}进行排序。

  - 第一步，选择中间的元素 45 作为"基准"。（基准值可以任意选择，但是选择中间的值比较容易理解）

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/6c1fc388fbba0324e28428a73ed6430b.jpg)

  - 第二步，按照顺序，将每个元素与"基准"进行比较，形成两个子集，一个"小于 45"，另一个"大于等于 45"。

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/71ecc929ab1eb240e994893a0fb3d3e1.jpg)

  - 第三步，对两个子集不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/663e1107cd90a67042d6f5ca2a308e33.jpg)
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/3b3c7249a0304388b3c0d556179f5d27.jpg)
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/a4b047505b4b09452f45597554404648.jpg)
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/8c7a0779fee100060f37fbef0e782723.jpg)

- 时间复杂度
  - **平均时间复杂度：O(nlogn)。**

    **对于乱序的序列，没有任何优化的快速排序的速度要比优化后的归并排序还要快**。

    其中，**每次 partition 过程的时间效率为 O(n)，递归深度为 O(log n)，因此总的时间效率为 O(n log n)**。

  - **最坏时间复杂度：O(n^2)。**

    与其它排序算法不同的是，**快速排序极其不适用于基本有序序列的排序**。当所有元素都相同的极端情况下，partition 将一直返回 R, 递归的深度高达 N，每一次递归中 partition 又循环 N 次，时间复杂度退化为了 O(N^2)。

- **空间复杂度：O(log n)。**

  快速排序算法的递归实现需要栈的辅助，由于递归深度为 O(log n)，因此需要 O(log n) 层的栈空间来保存每次递归过程中的临时变量以供递归返回时继续使用。

  **当整个数列完全有序的极端情况时，栈的深度会达到 O(n)。**

- 代码实现

  C++ 实现（递归）（CLRS 版本）
  ```cpp
  //Quick sort, L 为起始索引，R 为结束索引
  void Quicksort(int a[], int L, int R) {
      if (L < R) { // 当被排序的元素大于 1 个时才执行快排
          // partition: 求 pivot
          int pivot = L; // 预留基准位置，将序列第一个元素作为哨兵
          for (int i = L; i < R; i++)  // 遍历整个序列
              if (a[i] <= a[R])  // 以末尾元素为基准元素
                  Swap(a[i], a[pivot++]);// 将比基准元素小的元素移动到基准位置左边，并将基准位置右移一位
          Swap(a[R], a[pivot]); // 最后将基准元素放到基准位置
  
          Quicksort(a, L, pivot - 1); // 递归调用，对基准位置左边的子序列进行快排
          Quicksort(a, pivot + 1, R); // 递归调用，对基准位置右边的子序列进行快排
      }
  }
  ```
  NOTE: 每次 partition 求得的 pivot 位置都不会再发生改变，即若 pivot 索引位置为 k，则 pivot 就是整个序列中第 k 大的元素。

  JavaScript 实现（递归）（阮一峰版本）
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
  这个版本存在的问题：TODO:

#### 3.2.1. 针对短序列的优化：直接使用插入排序

对于任何高级排序算法，在对元素个数很少的元素进行排序时，都可以直接使用插入排序进行操作，其速度会优于绝大部分高级排序算法。

```cpp
if (R - L <= 47)
{
  StraightInsertionSort(a, L, R);
  return;
}
```

#### 3.2.2. 针对基本有序序列的优化：随机化基准

由于快速排序算法中无法保证 partition 后的两个子序列长度相同，导致当序列中的基本有序时，快速排序递归调用时的递归树平衡性较差。**在序列完全有序的极端情况下，快速排序退化到了 O(N^2)**。

因此，可以使用了一个很简单的**随机选取 pivot** 的方式来处理这个问题。这步**随机化让快速排序的时间期望成为了 O(nlogn)，并且只有极低的概率退化为 O(n^2)**。

> 关于这一点，背后的数学证明比较复杂，对背后的数学不感兴趣的同学，只要相信这个结论就好了。事实上，n 不需要太大，在 100 这个量级，其退化成 O(n^2) 算法的概率就已经低于大家彩票中大奖的概率了。

```cpp
swap(arr[R], arr[rand()%(r-l+1)+l]);// 在 l~r 范围内随机选择一个元素作为基准，并将基准元素放在序列末尾
```

#### 3.2.3. 针对重复元素的优化：二路快排和三路快排

对于快速排序算法，当待排序序列中的元素随机分布时，算法可以取得非常可观的效率。但随着待排序序列中的重复（相等）元素增多，快排的效率显著下降。**在所有元素都相同的极端情况下，partition 将一直返回 R, 递归的深度高达 N，每一次递归中 partition 又循环 N 次，时间复杂度到了 O(N^2)**。

在传统的快速排序算法中，每次选取基准后，会将序列划分为小于基准和大于基准的两个部分。而**针对有大量重复元素的序列，我们可以使用二路快排和三路排序**。

##### 3.2.3.1. 二路快排

二路快排使用 2 个索引，从序列的两端同时向中间进行排序，**将等于基准的元素均分到了 pivot 的两边，从而避免了算法退化成 O(n^2)**。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/4e75b9f4a1f7b0a438d0f25bec65ba47.jpg)

```java
// 返回 p, 使得 arr[l...p-1] < arr[p] ; arr[p+1...r] > arr[p]
template <typename T>
int _partition2(T arr[], int l, int r) {
    // 随机在 arr[l...r] 的范围中，选择一个数值作为标定点 pivot
    swap( arr[l] , arr[rand()%(r-l+1)+l] );
    T v = arr[l];

    // arr[l+1...i) <= v; arr(j...r] >= v
    int i = l+1, j = r;
    while( true ){
        while( i <= r && arr[i] < v ) i ++;
        while( j >= l+1 && arr[j] > v ) j --;
        if( i > j ) break;
        swap( arr[i++] , arr[j--] );
    }
    swap( arr[l] , arr[j]);
    return j;
}

// 对 arr[l...r] 部分进行快速排序
template <typename T>
void _quickSort(T arr[], int l, int r) {

    // 对于小规模数组，使用插入排序进行优化
    if( r - l <= 15 ){
        insertionSort(arr,l,r);
        return;
    }

    // 调用双路快速排序的 partition
    int p = _partition2(arr, l, r);
    _quickSort(arr, l, p-1 );
    _quickSort(arr, p+1, r);
}

template <typename T>
void quickSort(T arr[], int n){
    srand(time(NULL));
    _quickSort(arr, 0, n-1);
}
```

##### 3.2.3.2. 三路快排

三路快排指的是在每次选取基准后，将序列划分为小于基准、等于**基准**和大于基准的三个部分。在递归的过程中，中间部分不参与递归，分治的是两边。**三路快排比二路快排拥有更高的效率**。

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
              } else {  // a[i] == v
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

三路快排很好的解决了近乎有序的数组和有大量重复数组的元素排序问题，对于随机序列也有很好的性能，因此**在很多语言的标准库中，排序接口使用的就是三路快排的思路，比如 Java 语言中的 Array.sort()。**

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

    如果我们对这个数组进行三路快排的话，并且选择 1 是 pivot，那么第一次 partition 的过程，就会把数组分成小于 1 的部分和大于 1 的部分，也就是所有的 0 在 1 的左边；所有的 2 在 1 的右边，就已经完成了排序。换句话说，我们**对整个数组进行一次以 1 为 pivot 的三路快排的 partition 就完成了排序**，整个过程只有一次遍历。

    ```cpp
    void sortColors(vector<int>& nums) {
        int i = 0;  // nums[0..<i) == 0
        int j = 0;  // nums[i..<j) == 1
        int k = nums.size(); // nums[k..<n) == 2

        while( j < k ) {
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

#### 3.2.4. 算法思想扩展

##### 3.2.4.1. 求无序数组的中位数

> Q: 实现一个 O(n) 的算法来求一个序列的中位数，即第 n/2 大的元素。

找中位数是要找到序列有序时位置在中间的那个数，即 mid=(low+high)/2 时的那个数。可以联想到使用快速排序中 partition 的思想，找到最终位置在 n/2 的那个数。这也就是 **BFPRT 算法**。

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
	int mid = (low + high) / 2;
	while (1)
	{
		int pos = partition(L, low, high);
		if(pos == mid)
			break;
		else if(pos > mid)
			high = pos - 1;
		else 
            low = pos + 1;
	}
    return L[mid];
}
```

这种方法实际上是对数组进行了局部的排序，每次只排序 partition 后的一半，因为将快速排序的平均时间 O(nlogn) 降到了 O(n)。严谨的数学证明请参见《算法导论》。

<!-- TODO: 复杂度为什么是 O(n) ？尾递归优化？-->

##### 3.2.4.2. 求第 n 大

求一个序列中第 n 大的元素值。 

仿照求中位数的 partition 思想实现即可。

## 4. （两路）归并排序 (Merge Sort)

归并排序（merge sort）也是一种**基于分治思想**的排序算法。它将两个（或两个以上）有序表合并成一个新的有序表。即**把待排序序列分为若干个子序列，每个子序列是有序的，然后再把有序子序列合并为整体有序序列**。归并排序算法依赖归并操作。

归并排序有多路归并排序、两路归并排序，可用于内排序，也可以用于外排序。这里仅对内排序的两路归并方法进行讨论。

根据具体的实现，归并排序包括"从上往下"和"从下往上"2 种方式：
- **从上往下**

	从上往下的归并排序指的是先从上往下进行分解，再从下往上进行合并。基本包括 3 步：
	1. 分解 -- 将当前区间一分为二，即求分裂点 mid = (low + high)/2; 
	1. 求解 -- 递归地对两个子区间 a[low...mid] 和 a[mid+1...high] 进行归并排序。递归的终结条件是子区间长度为 1。
	1. 合并 -- 在每次递归中，将已排序的两个子区间 a[low...mid] 和 a[mid+1...high] 归并为一个有序的区间 a[low...high]。

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/be0fdc4760a7ee4f4eca12176ba0917f.jpg)

	由于序列分解后，共有 logn 层，因此要实现 O(nlogn) 的归并排序，就变成：**如何使用 O(n) 的算法将各自有序的两个子序列，合并成一个完全有序的序列**？

	这个问题的实现，无法通过原地交换位置来完全，因此需要借助一个 O(n) 的辅助空间。通过与辅助数组进行比较，在原数组上进行赋值。
	
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/d9f9ba28306f3187f553b2dbfa889685.jpg)

	使用 3 个索引来追踪操作的位置，其中蓝色指针表示在目标序列（原序列）中的索引，红色指针表示在两个各自有序的序列（辅助空间）中的索引：

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/d26ec3cb7c936054b678c7e7d9a3fdc8.jpg)

	比较当前两个红色指针指向的值，选择较小 / 大值赋值给蓝色指针指向的位置，并将被选择的红色指针和蓝色指针都后移一位：

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/8c46fe155161f012291a65391f780ce3.jpg)

	以此类推：

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/c4ae39aef9eafa3547dee34ba77d41f1.jpg)

	在实际实现中，需要定义清楚各个指针和位置的变量，从而完成完整的归并过程：

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/43318a9ef0aad21918b6a6b72007ec0d.jpg)

- **从下往上**

	从下往上的归并排序与"从下往上"在排序上是反方向的。它将待排序的数列分成若干个长度为 1 的子数列，然后将这些数列两两合并；得到若干个长度为 2 的有序数列，再将这些数列两两合并；得到若干个长度为 4 的有序数列，再将它们两两合并；直接合并成一个数列为止。这样就得到了我们想要的排序结果。

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/13/120033d0bf3205135cc40dce145e1075.jpg)
	
	这种方式的实现不需要递归，只须迭代就可以完成归并排序。

**归并排序是一种稳定的排序算法**。

- 时间复杂度
	
	归并排序将待排序序列分为了 logn 个级别，每个级别包含了多个待排序子序列，其排序耗时为 O(N)，因此，归并排序的**时间复杂度为 O(nlogn)**。

- 空间复杂度

	在归并排序的归并实现中，需要将各自有序的子序列整合为完全有序的序列，实现这一点**需要 O(n) 的辅助空间**；若不使用辅助空间，则实现起来非常困难。

	**归并排序是唯一一种无法实现原地排序的排序算法，这也是归并排序的一个缺点**。

- 代码实现
	- 从上往下（递归实现）
		```cpp
		// 将 arr[l...mid] 和 arr[mid+1...r] 两部分进行归并
		template<typename T>
		void __merge(T arr[], int l, int mid, int r){
				// 申请辅助空间
				T *aux = new T[r-l+1];
				// 将 arr[l...r] 复制到辅助空间
				for( int i = l ; i <= r; i ++ )
						aux[i-l] = arr[i];// 注意 aux 与 arr 有位移差 l

				// 初始化，i 指向辅助数组左半部分的起始索引位置 l；j 指向辅助数组右半部分起始索引位置 mid+1
				for( int i = l, j = mid+1, k = l; k <= r; k ++ ) { // k 指向原数组（目标数组）的当前索引位置
						if( i > mid ){  // 如果左半部分元素已经全部处理完毕
								arr[k] = aux[j-l];
								j ++;
						}
						else if( j > r ){  // 如果右半部分元素已经全部处理完毕
								arr[k] = aux[i-l]; 
								i ++;
						}
						else if( aux[i-l] < aux[j-l] ) {  // 左半部分所指元素 < 右半部分所指元素
								arr[k] = aux[i-l]; 
								i ++;
						}
						else{  // 左半部分所指元素 >= 右半部分所指元素
								arr[k] = aux[j-l]; 
								j ++;
						}
				}
				delete[] aux;
		}

		// 递归使用归并排序，对 arr[l...r] 的范围进行排序
		template<typename T>
		void mergeSort(T arr[], int l, int r){
				if( l >= r )
		            // 递归到达终点，子序列元素为 1
		            return;
				int mid = (l+r)/2;
				mergeSort(arr, l, mid); // 对左边的子序列进行归并排序
				mergeSort(arr, mid+1, r); // 对右边的子序列进行归并排序
				__merge(arr, l, mid, r); // 将 arr[l...mid] 和 arr[mid+1...r] 两部分进行合并
		}
		```

	- 从下往上（迭代实现）
		```cpp
		// 将 arr[l...mid] 和 arr[mid+1...r] 两部分进行归并
		template<typename  T>
		void __merge(T arr[], int l, int mid, int r){
				// 申请辅助空间
				T *aux = new T[r-l+1];
				// 将 arr[l...r] 复制到辅助空间
				for( int i = l ; i <= r; i ++ )
						aux[i-l] = arr[i];// 注意 aux 与 arr 有位移差 l

				// 初始化，i 指向辅助数组左半部分的起始索引位置 l；j 指向辅助数组右半部分起始索引位置 mid+1
				int i = l, j = mid+1;
				for( int k = l ; k <= r; k ++ ){ k 指向原数组（目标数组）的当前索引位置

						if( i > mid ){  // 如果左半部分元素已经全部处理完毕
								arr[k] = aux[j-l];
								j ++;
						}
						else if( j > r ){  // 如果右半部分元素已经全部处理完毕
								arr[k] = aux[i-l]; 
								i ++;
						}
						else if( aux[i-l] < aux[j-l] ) {  // 左半部分所指元素 < 右半部分所指元素
								arr[k] = aux[i-l]; 
								i ++;
						}
						else{  // 左半部分所指元素 >= 右半部分所指元素
								arr[k] = aux[j-l]; 
								j ++;
						}
				}
				delete[] aux;
		}

		// 使用自底向上的归并排序算法
		template <typename T>
		void mergeSortBU(T arr[], int n){
		    // sz 表示每次循环的各个子序列的长度
		    for( int sz = 1; sz < n ; sz *= 2 )
		        // 两两进行合并
		        for( int i = 0 ; i < n - sz ; i += sz*2 ) 
		            // 对 arr[i...i+sz-1] 和 arr[i+sz...i+2*sz-1] 进行归并
		            __merge(arr, i, i+sz-1, min(i+sz*2-1, n-1) ); // min(i+sz*2-1, n-1) 防止越界
		}
		```

### 4.1. 针对短序列优化：直接使用插入排序

在归并排序中，当序列被拆分成为一定程序的短序列时，此时直接使用 Straight insertion Sort，要比对子序列继续进行归并排序效率更高。

- 自上向下
	```cpp
	// 使用优化的归并排序算法，对 arr[l...r] 的范围进行排序
	template<typename T>
	void __mergeSort2(T arr[], int l, int r){

			// 优化：对于小规模数组，使用插入排序
			if( r - l <= 15 ){
	            insertionSort(arr, l, r);
	            return;
			}

			int mid = (l+r)/2;
			__mergeSort2(arr, l, mid);
			__mergeSort2(arr, mid+1, r);
			__merge(arr, l, mid, r);
	}
	```
- 自下向上
	```cpp
	// 使用自底向上的归并排序算法
	template <typename T>
	void mergeSortBU(T arr[], int n){
		// 对于小数组，使用插入排序优化
    for( int i = 0 ; i < n ; i += 16 )
        insertionSort(arr,i,min(i+15, n-1));

		// sz 表示每次循环的各个子序列的长度
		for( int sz = 16; sz < n ; sz *= 2 )
	        for( int i = 0 ; i < n - sz ; i += sz+sz )
	            // 对 arr[i...i+sz-1] 和 arr[i+sz...i+2*sz-1] 进行归并
	            __merge(arr, i, i+sz-1, min(i+sz+sz-1,n-1) );
	}
	```

### 4.2. 针对基本有序序列优化：提前终止

若在合并子序列的时候，若两个子序列各自有序且已经完全有序，则可直接返回结果不再进行合并操作。在对基本有序的序列进行归并排序时，这种情况经常发生，因此此时这种优化特别有效。

- 自上向下
	```cpp
	// 使用优化的归并排序算法，对 arr[l...r] 的范围进行排序
	template<typename T>
	void __mergeSort2(T arr[], int l, int r){

			// 优化：对于小规模数组，使用插入排序
			if( r - l <= 15 ){
					insertionSort(arr, l, r);
					return;
			}

			int mid = (l+r)/2;
			__mergeSort2(arr, l, mid);
			__mergeSort2(arr, mid+1, r);

			// 优化：对于 arr[mid] <= arr[mid+1] 的情况，不进行 merge
			// 对于近乎有序的数组非常有效，但是对于一般情况，有一定的性能损失
			if( arr[mid] > arr[mid+1] )
					__merge(arr, l, mid, r);
	}
	```
- 自下向上

	```cpp
	// 使用自底向上的归并排序算法
	template <typename T>
	void mergeSortBU(T arr[], int n){
			// 对于小数组，使用插入排序优化
			for( int i = 0 ; i < n ; i += 16 )
					insertionSort(arr,i,min(i+15,n-1));

			for( int sz = 16; sz < n ; sz += sz )
					for( int i = 0 ; i < n - sz ; i += sz+sz )
							// 对于 arr[mid] <= arr[mid+1] 的情况，不进行 merge
							if( arr[i+sz-1] > arr[i+sz] )
									__merge(arr, i, i+sz-1, min(i+sz+sz-1,n-1) );
	}
	```

NOTE:
- 在自顶向下的归并排序中。这个优化可以发生在非常高的层次，也就是面对两个很大的数组，也可以通过这步优化，使得我们不需要进一步处理两个大数组。
- 但是在自底向上的归并排序中，我们却不能跨过具有这种性质的大数组，依然要一步一步从小数组向上构造。由于这个原因，这个优化在自底向上的归并排序中并不能起到多大的效果。

### 4.3. 自顶向下与自底向上的比较

http://coding.imooc.com/learn/questiondetail/3208.html

在不采用任何优化的实现中，自顶向下的版本通过递归实现，递归调用需要额外的开销，因此，使用迭代实现的自底向上版本要更快一些。

在采用了两种优化的实现中，由于自底向上的归并排序无法起到良好的优化效果，因此两种版本的归并排序速度差别变得极小，但总体上自底向上的归并排序还是会非常微弱的稍胜一筹。

### 4.4. 算法思想扩展

计算一个序列中逆序对的数量。TODO:

## 5. 综合比较

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/1/c82ba1ca51b4da05d788119de27b2cdc.jpg)

（改错：上述快速排序算法的空间复杂度应改为 O(logn)）

### 5.1. 性能测试

Test for random array, size = 100000, random range [0, 100000]
- Selection Sort : 13.718 s
- Insertion Sort : 8.313 s
- Bubble Sort : 39.937 s
- Shell Sort : 0.016 s
- Merge Sort : 0.015 s
- Bottom Up Merge Sort : 0.032 s

Test for nearly ordered array, size = 100000, swap time = 100
- Selection Sort : 13.781 s
- Insertion Sort : 0.016 s
- Bubble Sort : 14.297 s
- Shell Sort : 0.015 s
- Merge Sort : 0.012 s
- Bottom Up Merge Sort : 0.016 s

### 5.2. 空间复杂度

归并排序（但根据 TAOCP，合并排序也有原地排序的版本）、计数排序等不是原地排序。

希尔排序、冒泡排序、插入排序、选择排序、堆排序、快速排序都可以实现原地排序。

### 5.3. 时间复杂度

- O(n^2)：各类简单排序，即直接插入排序、直接选择排序和冒泡排序。
- O(n^(1+§))(0<§<1)：希尔排序。
- O(nlogn)：快速排序、堆排序和归并排序。
- O(n)：基数排序、桶排序、箱排序。

说明：
- 借助于“比较”进行排序的算法（比较排序算法）在最坏情况下能达到的最好的时间复杂度为 O(n log n)。
- 当原表有序或基本有序时，直接插入排序和冒泡排序将大大减少比较次数和移动记录的次数，时间复杂度可降至 O（n）。而快速排序则相反，当原表基本有序时，将退化为冒泡排序，时间复杂度提高为 O（n^2）。
- 原表是否有序，对简单选择排序、堆排序、归并排序和基数排序的时间复杂度影响不大。

### 5.4. 稳定性

四个不稳定排序：“快些选堆”。（其中“快”指快速排序，“些”指希尔排序，“选”指直接选择排序，“堆”指堆排序，即这四种排序方法是不稳定的，其他排序方法都是稳定的）

其它排序算法都是稳定排序。

若在一个系统级别的类库中，要实现稳定的排序，通常会选择归并排序算法。

### 5.5. 算法选择

影响排序的因素有很多，平均时间复杂度低的算法并不一定就是最优的。相反，有时平均时间复杂度高的算法可能更适合某些特殊情况。同时，选择算法时还得考虑它的可读性，以利于软件的维护。一般而言，需要考虑的因素有以下四点：
- 待排序的记录数目 n 的大小。
- 记录本身数据量的大小，也就是记录中除关键字外的其他信息量的大小。
- 关键字的结构及其分布情况。
- 对排序稳定性的要求。

设待排序元素的个数为 n，则：
- 当 n 较大，则应采用时间复杂度为 O(nlogn) 的排序方法：快速排序、堆排序或归并排序。
- 当 n 较小，可采用直接插入或直接选择排序。

### 5.6. 其它

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

## 6. Refer Links

[十种排序算法总结（冒泡、插入、选择、希尔、归并、堆、快速，计数，桶，基数）](http://blog.csdn.net/jnu_simba/article/details/9705111)

[八大排序算法](http://blog.csdn.net/hguisu/article/details/7776068)

[面试中的 10 大排序算法总结](http://www.codeceo.com/article/10-sort-algorithm-interview.html)

[排序算法动画表示 1](https://www.toptal.com/developers/sorting-algorithms/)

[排序算法动画表示 2](http://jsdo.it/norahiko/oxIy/fullscreen)

[阮一峰：快速排序（Quicksort）的 Javascript 实现](http://www.ruanyifeng.com/blog/2011/04/quicksort_in_javascript.html)

[『快速排序算法 C++ 实现『评注版』](https://www.cnblogs.com/pugang/archive/2012/06/27/2565093.html)

[面试官，您要的快排](https://segmentfault.com/a/1190000002651247) 

[【算法杂谈 1】 从一道面试题再看三路快排 partition](http://www.imooc.com/article/16141)

[浅谈算法和数据结构：四 快速排序](https://www.cnblogs.com/yangecnu/p/Introduce-Quick-Sort.html)

[如果天空不死：归并排序](http://www.cnblogs.com/skywang12345/p/3602369.html)

[Is there ever a good reason to use Insertion Sort?](https://stackoverflow.com/questions/736920/is-there-ever-a-good-reason-to-use-insertion-sort)

[Why is Insertion sort better than Quick sort for small list of elements?](https://stackoverflow.com/questions/8101546/why-is-insertion-sort-better-than-quick-sort-for-small-list-of-elements)

[O(n) 时间快速选择](http://www.shadowxh.com/?p=598)

[找中位数 O(n) 算法](https://www.cnblogs.com/claireyuancy/p/7140840.html)

[快速排序详解及不排序求中位数 o（n）算法](https://blog.csdn.net/Q_M_X_D_D_/article/details/80108541)

[求无序数组的中位数](https://blog.csdn.net/zdl1016/article/details/4676882)
