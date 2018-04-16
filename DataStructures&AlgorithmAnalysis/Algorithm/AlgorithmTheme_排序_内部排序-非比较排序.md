- [内部排序 - 非比较排序 / 分布排序](#%E5%86%85%E9%83%A8%E6%8E%92%E5%BA%8F---%E9%9D%9E%E6%AF%94%E8%BE%83%E6%8E%92%E5%BA%8F-%E5%88%86%E5%B8%83%E6%8E%92%E5%BA%8F)
	- [1. 计数排序 (Counting Sort)](#1-%E8%AE%A1%E6%95%B0%E6%8E%92%E5%BA%8F-counting-sort)
	- [2. 桶排序 （Bucket Sort）](#2-%E6%A1%B6%E6%8E%92%E5%BA%8F-%EF%BC%88bucket-sort%EF%BC%89)
	- [3. 基数排序 (Radix Sort)](#3-%E5%9F%BA%E6%95%B0%E6%8E%92%E5%BA%8F-radix-sort)
	- [4. Refer Links](#4-refer-links)

# 内部排序 - 非比较排序 / 分布排序

## 1. 计数排序 (Counting Sort)

计数排序 (Counting sort) 是一种稳定的排序算法。计数排序使用一个额外的数组 C，其中第 i 个元素是待排序数组 A 中值等于 i 的元素的个数，然后根据数组 C 来将 A 中的元素排到正确的位置。

- 算法步骤：
	1. 找出待排序的数组中最大和最小的元素。
	1. 统计数组中每个值为 i 的元素出现的次数，存入数组 C 的第 i 项。
	1. 对所有的计数累加（从 C 中的位置为 1 的元素开始，每一项和前一项相加）。这样，C[i] 就表示 A 中值不大于 i 的元素的个数。
	1. 反向填充目标数组：将每个元素 i 放在新数组的第 C(i) 项，每放一个元素就将 C(i) 减去 1。

	例： 

	数组 A[] 保存原始数据｛1,5,4,6,2,3,2,1,4｝，数据范围从 0~6。C[] 数组是辅助数组，B[] 数组用来保存输出结果。

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/14/e65ee4b8b5d473e8a4cbbf0011b3eeb6.jpg)

	然后根据反向地 C[] 数组数据判断 B[] 对应位置应该保存哪个数字：

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/14/db9a03c5deea5302a30084e0a150475d.jpg)

- 时间效率

	时间复杂度为 O(n)。

- 空间效率

	由于用来计数的数组 C 的长度取决于待排序数组中数据的范围（等于待排序数组的最大值与最小值的差加上 1），这使得计数排序对于数据范围很大的数组，需要大量内存空间。

- 应用场景
	
	计数排序是用来排序 0 到 100 之间的数字的最好的算法，但是它不适合按字母顺序排序人名。但是，计数排序可以用在基数排序中的算法来排序数据范围很大的数组。

- 代码实现
	```cpp
	void  CountSort(int *arr, int num)
	{
			int mindata = arr[0];
			int maxdata = arr[0];
			for (int i = 1; i < num; i++)
			{
					if (arr[i] > maxdata)
							maxdata = arr[i];
					if (arr[i] < mindata)
							mindata = arr[i];
			}
			
			int size = maxdata - mindata + 1;
			// 申请空间并初始化为 0
			int *pCount = (int *)malloc(sizeof(int) * size);
			memset(pCount, 0, sizeof(int)*size);

			// 记录排序计数，每出现一次在对应位置加 1
			for (int i = 0; i < num; i++)
					++pCount[arr[i]-mindata];

			// 确定不比该位置大的数据个数
			for (int i = 1; i < size; i++)
					pCount[i] += pCount[i - 1]; // 加上前一个的计数

			int *pSort = (int *)malloc(sizeof(int) * num);
			memset((char*)pSort, 0, sizeof(int) * num);

			// 从末尾开始拷贝是为了重复数据首先出现的排在前面，即稳定排序
			for (int i = num - 1; i >= 0; i--)
			{
					// 包含自己需要减 1，重复数据循环回来也需要减 1
					--pCount[arr[i]-mindata];
					pSort[pCount[arr[i]-mindata]] = arr[i];
			}
			// 拷贝到原数组
			for (int i = 0; i < num; i++)
					arr[i] = pSort[i];

			free(pCount);
			free(pSort);
	}
	```

## 2. 桶排序 （Bucket Sort）

桶排序 / 箱排序也是一种稳定的排序算法。假设序列由一个随机过程产生，该过程将元素均匀而独立地分布在区间 [0,1) 上。我们把区间 [0,1) 划分成 n 个相同大小的子区间，称为桶。将 n 个记录分布到各个桶中去。如果有多于一个记录分到同一个桶中，需要进行桶内排序。最后依次把各个桶中的记录列出来记得到有序序列。

桶排序是计数排序的变种，把计数排序中相邻的 m 个"小桶"放到一个"大桶"中，在分完桶后，对每个桶进行排序（一般用快排），然后合并成最后的结果。

- 算法步骤：
	1. 扫描序列 A，根据每个元素的值所属的区间，放入指定的桶中（顺序放置)。
	1. 对每个桶中的元素进行排序，这时可采用比较排序算法进行排序，例如快速排序。
	1. 依次收集每个桶中的元素，顺序放置到输出序列中。

	例：对大小为 [1..1000] 范围内的 n 个整数 A[1..n] 排序。 

　首先，可以把桶设为大小为 10 的范围，设集合 B[1] 存储 [1..10] 的整数，集合 B[2] 存储 (10..20] 的整数，…… ，集合 B[i] 存储 ((i-1)*10, i*10] 的整数，i=1,2,..100，总共有 100 个桶。  

　然后，对 A[1, ... , n] 从头到尾扫描一遍，把每个 A[i] 放入对应的桶 B[j] 中。再对这 100 个桶中每个桶里的数字排序，这时可用冒泡，选择，乃至快排，一般来说任何排序法都可以。

　最后，依次输出每个桶里面的数字，且每个桶中的数字从小到大输出，这样就得到所有数字排好序的一个序列了。  

- 时间效率

	桶排序的平均时间复杂度为线性的 O(N+C)，其中 C 为桶内快排的时间复杂度。如果相对于同样的 N，桶数量 M 越大，其效率越高，最好的时间复杂度达到 O(N)。 

	当然，以上复杂度的计算是基于输入的 n 个数字是平均分布这个假设的。实际应用中效果并没有这么好，如果所有的数字都落在同一个桶中，那就退化成一般的排序了。 
	
- 空间效率

	桶排序的空间复杂度 为 O(N+M)，如果输入数据非常庞大，而桶的数量也非常多，则空间代价无疑是昂贵的。

- 代码实现
	```cpp
	struct linklist
	{
			linklist *next;
			int value;
			linklist(int v,linklist *n):value(v),next(n){}
			~linklist() {if (next) delete next;}
	};
	inline int cmp(const void *a,const void *b)
	{
			return *(int *)a-*(int *)b;
	}
	// 为了方便，将 A 中元素加入桶中时是倒序放入的，而收集取出时也是倒序放入序列的，所以不违背稳定排序
	void BucketSort(int *A,int *B,int N,int K)
	{
			linklist *Bucket[101],*p;// 建立桶
			int i,j,k,M;
			M=K/100;
			memset(Bucket,0,sizeof(Bucket));
			for (i=1;i<=N;i++)
			{
					k=A[i]/M; // 把 A 中的每个元素按照的范围值放入对应桶中
					Bucket[k]=new linklist(A[i],Bucket[k]);
			}
			for (k=j=0;k<=100;k++)
			{
					i=j;
					for (p=Bucket[k];p;p=p->next)
							B[++j]=p->value; // 把桶中每个元素取出，排序并加入 B
					delete Bucket[k];
					qsort(B+i+1,j-i,sizeof(B[0]),cmp);
			}
	}
	```

## 3. 基数排序 (Radix Sort)

基数排序也是一种稳定的排序算法，用于解决有多个关键字的排序问题。其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。由于整数也可以表达字符串（比如名字或日期）和特定格式的浮点数，所以基数排序也不是只能使用于整数。

- 算法思想

	假设我们有一些二元组 (a,b)，要对它们进行以 a 为首要关键字，b 的次要关键字的排序。我们可以先把它们先按照首要关键字排序，分成首要关键字相同的若干堆。然后，在按照次要关键值分别对每一堆进行单独排序。最后再把这些堆串连到一起，使首要关键字较小的一堆排在上面。按这种方式的基数排序称为 MSD(Most Significant Dight) 排序。

	第二种方式是从最低有效关键字开始排序，称为 LSD(Least Significant Dight) 排序。首先对所有的数据按照次要关键字排序，然后对所有的数据按照首要关键字排序。要注意的是，使用的排序算法必须是稳定的，否则就会取消前一次排序的结果。由于不需要分堆对每堆单独排序，LSD 方法往往比 MSD 简单而开销小。

	例：对 `278、109、063、930、589、184、505、269、008、083` 进行排序。

	我们将每个数值的个位，十位，百位分成三个关键字： `278 -> k1（个位)=8，k2（十位)=7，k3=（百位)=2`。

	然后从最低位个位开始（从最次关键字开始)，对所有数据的 k1 关键字进行桶分配（因为，每个数字都是 0-9 的，因此桶大小为 10)，再依次输出桶中的数据得到下面的序列。

	`930、063、083、184、505、278、008、109、589、269`

	再对上面的序列接着进行针对 k2 的桶分配，输出序列为：

	`505、008、109、930、063、269、278、083、184、589`

	最后针对 k3 的桶分配，输出序列为：

	`008、063、083、109、184、269、278、505、589、930`

- 时间效率

	基数排序的性能比桶排序要略差。每一次关键字的桶分配都需要 O(N) 的时间复杂度，而且分配之后得到新的关键字序列又需要 O(N) 的时间复杂度。假如待排数据可以分为 d 个关键字，则基数排序的时间复杂度将是 O(d*2N) ，当然 d 要远远小于 N，因此基本上还是线性级别的。

- 空间效率

	基数排序的空间复杂度为 O(N+M)，其中 M 为桶的数量。一般来说 N>>M，因此额外空间需要大概 N 个左右。

- 应用场景

	对比桶排序，基数排序每次需要的桶的数量并不多。而且基数排序几乎不需要任何“比较”操作，而桶排序在桶相对较少的情况下，桶内多个数据必须进行基于比较操作的排序。因此，在实际应用中，基数排序的应用范围更加广泛。

- 代码实现

	通常，基数排序要用到计数排序或者桶排序。使用计数排序时，需要的是 Order 数组。使用桶排序时，可以用链表的方法直接求出排序后的顺序。下面是一段用桶排序对二元组基数排序的程序：
	```cpp
	struct data
	{
			int key[2];
	};
	struct linklist
	{
			linklist *next;
			data value;
			linklist(data v,linklist *n):value(v),next(n){}
			~linklist() {if (next) delete next;}
	};
	void BucketSort(data *A,int N,int K,int y)
	{
			linklist *Bucket[101],*p;// 建立桶
			int i,j,k,M;
			M=K/100+1;
			memset(Bucket,0,sizeof(Bucket));
			for (i=1;i<=N;i++)
			{
					k=A[i].key[y]/M; // 把 A 中的每个元素按照的范围值放入对应桶中
					Bucket[k]=new linklist(A[i],Bucket[k]);
			}
			for (k=j=0;k<=100;k++)
			{
					for (p=Bucket[k];p;p=p->next) j++;
					for (p=Bucket[k],i=1;p;p=p->next,i++)
							A[j-i+1]=p->value; // 把桶中每个元素取出
					delete Bucket[k];
			}
	}
	void RadixSort(data *A,int N,int K)
	{
			for (int j=1;j>=0;j--) // 从低优先到高优先 LSD
					BucketSort(A,N,K,j);
	}
	```

## 4. Refer Links

[【算法导论】线性时间排序 - 计数排序](http://blog.51cto.com/hqalbert/1418667)

[三种线性排序算法 计数排序、桶排序与基数排序](https://www.byvoid.com/zhs/blog/sort-radix)

[桶排序（Bucket Sort）/ 基数排序（Radix Sort）](https://www.cnblogs.com/maybe2030/p/4715042.html#_label7)

[计数排序、桶排序和基数排序](https://segmentfault.com/a/1190000003054515)