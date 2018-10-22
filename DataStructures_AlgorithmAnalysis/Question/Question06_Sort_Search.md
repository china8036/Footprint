- [Sort & Search](#sort--search)
	- [1. Sort](#1-sort)
		- [1.1. 合并有序序列](#11-合并有序序列)
		- [1.2. 变位词排序](#12-变位词排序)
	- [2. Search](#2-search)
		- [2.1. 魔术索引](#21-魔术索引)
			- [2.1.1. 无序序列](#211-无序序列)
			- [2.1.2. 无重复元素的有序序列](#212-无重复元素的有序序列)
			- [2.1.3. 有重复元素的有序序列](#213-有重复元素的有序序列)
		- [2.2. 旋转过的数组中查找元素](#22-旋转过的数组中查找元素)
		- [2.3. 含空串的数组中查找元素](#23-含空串的数组中查找元素)
		- [2.4. 有序矩阵中查找元素](#24-有序矩阵中查找元素)
	- [3. Refer Links](#3-refer-links)

# Sort & Search

## 1. Sort

### 1.1. 合并有序序列

- Question

	给定 2 个有序数组 A 和 B，元素个数分别为 n 和 m，其中 A 的末端有足够的空间容纳 B。编写程序将 B 合并到 A 中，使得合并后 A 依旧有序。

- Solution

	如果逐一比较 A 和 B 中的元素，不断将元素插到 A 的前端，就必须不断将原有元素向后移动。因此，更好的做法是将元素插入数组 A 的末端，那里都是空闲的可用空间。
	```java
	public void merge(int [] a, int [] b, int n, int m) {
		int i = n - 1;
		int j = m - 1;
		int k = n + m - 1;

		while (i >= 0 && j >= 0) {
			if (a[i] > b[j]) 
				a[k--] = a[i--];
			else
				a[k--] = a[j--];
		}

		// 将 B 剩余元素复制到合适的位置
		while (j >= 0) 
			a[k--] = b[j--];
	}
	```

### 1.2. 变位词排序

《CC 150 11.2》

- Question
	> 编写一个方法，对一个字符串数组进行排序，将所有变位词排在相邻的位置。这里的变位词指变换其字母顺序所构成的新的词或短语。例如"triangle"和"integral"就是变位词。

	Example:
	```
	input:
	["ab","abc","ba","cba"]
	output:
	["ab","ba","abc","cba"]
	```

- Solution
	- 解法 1：套用一种排序算法，传入 Comparator 即可。
		```java
		public void sort(String [] arr) {
			Arrays.sort(arr, (String s1, String s2) -> {
				return sortChars(s1).compareTo(sortChars(s2));
			});
		}

		public String sortChars(String s) {
			char [] content = s.toCharArray();
			Arrays.sort(content);
			return new String(content);
		}
		```
	- 解法 2：使用桶排序的思想。使用 HashMap 存储，key 为 sortChars(s)，value 为字符串链表。

## 2. Search

### 2.1. 魔术索引

在一个数组中，如果 `arr[i] = i`，则称 i 为魔术索引。

#### 2.1.1. 无序序列

在一个无序序列中，寻找第一个魔术索引。

直接暴力遍历：
```java
public int magicIndex(int [] arr) {
	for (int i = 0; i < arr.length; i++)
		if (arr[i] == i)
			return i;
	return -1;
}
```

#### 2.1.2. 无重复元素的有序序列

由于序列有序且无重复元素，因此可以采用二分查找的思想进行优化。首先检查序列中间的元素 `arr[mid]` 和 `mid` 的大小：
- 若 `arr[mid] = mid`，则 mid 就是 magic index。
- 若 `arr[mid] > mid`，则 magic index 只可能在左侧。因为**往右侧移动时，索引每增加 k，值也至少增加 k**，因此左侧的元素永远不可能满足 `arr[i]==i`。
- 若 `arr[mid] < mid`，则 magic index 只可能在右侧。因为**往左侧移动时，索引每减少 k，值也至少减少 k**，因此左侧的元素永远不可能满足 `arr[i]==i`。

通过递归实现：
```java
public int magicIndex(int [] arr, int start, int end) {
	if (end < start || start <0 || end >= arr.length)
		return -1;
	int mid = start + (start - end) / 2
	if (arr[mid] == mid)
		return mid;
	else if (arr[mid] > mid)
		return magicIndex(arr, start, mid - 1);
	else
		return magicIndex(arr, mid + 1, end;)
}
```

#### 2.1.3. 有重复元素的有序序列

由于序列中可能有重复元素，因此无法根据 `arr[mid]` 和 `mid` 的大小来判断 magic index 的位置区间，**必须对左右两边都进行查找，但可以在查找时跳过一些元素**。

通过递归实现：
```java
public int magicIndex(int [] arr, int start, int end) {
	if (end < start || start <0 || end >= arr.length)
		return -1;
	int mid = start + (start - end) / 2
	if (arr[mid] == mid)
		return mid;
	// 搜索左侧
	int left = magicIndex(arr, start, Math.min(mid - 1, arr[mid]));
	if (left >= 0)
		return left;

	// 搜索右侧
	int right = magicIndex(arr, Math.min(mid + 1, arr[mid]), end);
	return right;
}
```

### 2.2. 旋转过的数组中查找元素

[《CC 150 11.3》](https://www.nowcoder.com/questionTerminal/72ff6503455c4a008675e79247ef2a3a)

- Question
	> 给定一个长度为 n 的升序数组，然后对该数组进行多次旋转，具体次数不详。编写程序在旋转过的数组中查找指定元素的位置。

- Solution

	这是一个 Binary Search 的变形题。由于数组被旋转了多次，因此可能存在拐点，如 `10 15 20 0 5` 和 `50 2 20 30 40`。
	
	观察可发现，**这样的数组中有一半是按正常顺序排列的**，因此，我们可以看按顺序排列的那一半数组，从而确定应该搜索左半边还是右半边。
	```java
	public int findElement(int[] A, int n, int x) {
			int left = 0;
			int right = n-1;
			int mid = 0;
			while (left <= right) {
					mid = (left + right) / 2;
					if (A[mid] == x)
							return mid;
					else if (A[mid] < x) {
							// A[mid] < A[left] 说明右边是有序的，且 x > A[right] 说明 x 在 mid 左边
							if (A[mid] < A[left] && x > A[right]) 
								right = mid - 1;
							else 
								left = mid + 1;
					} else {
							// A[mid] > A[left] 说明左边是有序的，且 x < A[left]，说明 x 在 mid 右边
							if (A[mid] > A[left] && x < A[left]) 
								left = mid + 1;
							else 
								right = mid - 1;
					}
			}
			return -1;
	}
	```
	算法的时间复杂度为 O(logn)，但若有很多重复元素，数组（子数组）的左右两边都得查找，因此时间复杂度退化为 O(n)。

### 2.3. 含空串的数组中查找元素

[《CC 150 11.5》](https://www.nowcoder.com/questionTerminal/06f532d3230042769b4d156e963a4f4a)

- Question
	> 有一个排过序的字符串数组，但是其中有插入了一些空字符串，请设计一个算法，找出给定字符串的位置。

- Solution

	这是一个 Binary Search 的变形题。唯一的关注点就是当 `str[mid] == ""` 时的处理，此时仅通过 `str[mid]=""` 是无法判断目标是在 mid 的左边还是右边。所以，我们遍历 mid 左边的元素找到第一个不是空字符串的元素。如果 mid 左边的所有元素都是空字符串，则直接令 `start = mid + 1`；否则找到第一个不是空字符串的元素下标为 index：
  - 如果 str[index] 等于目标正好返回。
  - 如果 str[index] 大于目标，则说明目标在 str[index] 左边，令 `end = index - 1`。
  - 如果 str[index] 小于目标，则说明目标在 str[mid] 右边，令 `start = mid + 1`。
	
	```java
	public class Finder {
			public int findString(String[] str, int n, String x) {
					if (str == null || str.length == 0) 
						return -1;
					int start = 0;
					int end = n - 1;
					while (str[start] == "") {
							start++;
							if (start == n) 
								return -1;
					}// 先找到左右两边都不是空格的端点

					while (str[end] == "")
							end--;
					
					while (start <= end) {
							int mid = (start + end) / 2;
							while (str[mid] == "") 
								mid--;
							if (str[mid].compareTo(x) == 0) 
								return mid;
							else if(str[mid].compareTo(x) > 0) {
									end = mid - 1;
									while (str[end] == "") 
										end--;
							} else {
									start = mid + 1;
									while(str[start] == "") 
										start++;
							}
					}
					return -1;
			}
	}
	```

### 2.4. 有序矩阵中查找元素

[《CC 150 11.6》](https://www.nowcoder.com/questionTerminal/3afe6fabdb2c46ed98f06cfd9a20f2ce)

- Question
	> 有一个 NxM 的整数矩阵，矩阵的行和列都是从小到大有序的。请设计一个高效的查找算法，查找矩阵中元素 x 的位置。

- Solution

	由于矩阵从左到右升序，从上到下升序。因此，**从右上角开始，每次将搜索值与右上角的值比较**，如果 x 大于 `mat[][]`，则下移一行；如果 x 小于 `mat[][]`，则左移动一列。

	如下图显示了查找 13 的轨迹。首先与右上角 15 比较，13<15，所以去掉最右 1 列，然后与 11 比较，这是 13>11，去掉最上面 1 行…以此类推，最后找到 13。算法复杂度 O(n)，最坏情况需要 2n 步，即从右上角开始查找，而要查找的目标值在左下角的时候。

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/22/ff60d65ce6729b24a681305a4d5e01d6.jpg)

	```java
	public int[] findElement(int[][] mat, int n, int m, int x) {
			if (mat == null || mat.length != n || mat[0].length != m)
					return null;
			int row = 0, col = m - 1;
			while (row < n && col >= 0) {
					if (mat[row](col) == x)
							return new int[]{row, col};
					else if(mat[row](col) < x)
							row++;
					else
							col--;
			}
			return null;
	}
	```

## 3. Refer Links