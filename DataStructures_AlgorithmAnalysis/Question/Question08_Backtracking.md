- [Backtracking](#backtracking)
	- [1. 排列问题 Permutation](#1-排列问题-permutation)
		- [1.1. Permutations](#11-permutations)
		- [1.2. Permutations II](#12-permutations-ii)
		- [1.3. 括号排列](#13-括号排列)
	- [2. 组合问题 Combination](#2-组合问题-combination)
		- [2.1. Combinations](#21-combinations)
		- [2.2. Combination Sum](#22-combination-sum)
		- [2.3. Combination Sum II](#23-combination-sum-ii)
		- [2.4. Combination Sum III](#24-combination-sum-iii)
		- [2.5. 组卷问题](#25-组卷问题)
		- [2.6. Subsets](#26-subsets)
		- [2.7. Subsets II](#27-subsets-ii)
		- [2.8. Binary Watch](#28-binary-watch)
		- [2.9. 硬币表示问题](#29-硬币表示问题)
	- [3. 二维平面问题](#3-二维平面问题)
		- [3.1. 派送员问题](#31-派送员问题)
		- [3.2. Word Search](#32-word-search)
		- [3.3. Word Search II](#33-word-search-ii)
		- [3.4. Flood Fill 问题](#34-flood-fill-问题)
			- [3.4.1. Number of Islands](#341-number-of-islands)
			- [3.4.2. Surrounded Regions](#342-surrounded-regions)
			- [3.4.3. Pacific Atlantic Water Flow](#343-pacific-atlantic-water-flow)
		- [3.5. N-Queues 问题](#35-n-queues-问题)
			- [3.5.1. N-Queens](#351-n-queens)
			- [3.5.2. N-Queens II](#352-n-queens-ii)
		- [3.6. Sudoku Solver](#36-sudoku-solver)
	- [4. 其它问题](#4-其它问题)
		- [4.1. Letter Combinations of a Phone Number](#41-letter-combinations-of-a-phone-number)
		- [4.2. Restore IP Addresses](#42-restore-ip-addresses)
		- [4.3. Palindrome Partitioning](#43-palindrome-partitioning)
	- [5. Refer Links](#5-refer-links)

# Backtracking

## 1. 排列问题 Permutation

排列公式：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/9/9/4e8ca7748548f4ec8404f6ba4bd8ac27.jpg)

指从给定 n 个数的元素中取出指定 r 个数的元素，进行排序。如 `A(9, 3) = 9 * 8 * 7 = 504`。

典型的排列问题：给定一个长度为 N 的序列，求该序列的所有排列方案。

```java
/**
 * input case:
 * 5
 * 1 2 3 4 5
 */
public class Main {
	private static int N;

	private static int [] nums;

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		N = sc.nextInt();
		nums = new int[N];
		for (int j = 0; j < N; j++) {
			nums[j] = sc.nextInt();
		}

		List<List<Integer>> res = permute(nums);

		for (List<Integer> l : res) {
			l.forEach(ele -> System.out.print(ele + " "));
			System.out.println(" ");
		}

	}

	public static List<List<Integer>> permute(int[] nums) {
		List<List<Integer>> list = new ArrayList<>();
		// Arrays.sort(nums); // not necessary
		__permute(list, new ArrayList<>(), nums);
		return list;
	}

	private static void __permute(List<List<Integer>> res,
									List<Integer> tempList,
									int [] nums) {
		if (tempList.size() == nums.length)
			res.add(new ArrayList<>(tempList));
		else {
			for(int i = 0; i < nums.length; i++) {
				if(tempList.contains(nums[i]))
					continue; // element already exists, skip
				tempList.add(nums[i]);
				__permute(res, tempList, nums);
				tempList.remove(tempList.size() - 1);
			}
		}
	}
}
```

### 1.1. Permutations

[46. Permutations](https://leetcode.com/problems/permutations/description/)

- Question
  > Given a collection of distinct integers, return all possible permutations.

  Example:
  ```
  Input: [1,2,3]
  Output:
  [
    [1,2,3],
    [1,3,2],
    [2,1,3],
    [2,3,1],
    [3,1,2],
    [3,2,1]
  ]
  ```
- Solution

  根据题意可得递归树：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/21/df28e92dc98f948eb2dda9c4498dcf71.jpg)

  递归表达式：
  ```
  Perms(nums[0...n-1]) = {取出一个数字} + Perms(nums[ {0...n-1} - 这个数字 ])
  ```

  代码实现：
  ```cpp
  // 时间复杂度：O(n^n), 空间复杂度：O(n)
  class Solution {
  private:
      vector<vector<int>> res;
      vector<bool> used;

      // p 中保存了一个有 index-1 个元素的排列
      // 向这个排列的末尾添加第 index 个元素，获得一个有 index 个元素的排列
      void generatePermutation(const vector<int>& nums, int index, vector<int>& p){
          if(index == nums.size()) { 
              res.push_back(p);
              return;
          }
          for(int i = 0 ; i < nums.size() ; i ++)
              if(!used[i]) {
                  used[i] = true;
                  p.push_back(nums[i]);
                  generatePermutation(nums, index + 1, p);
                  p.pop_back();
                  used[i] = false;
              }
          return;
      }
  public:
      vector<vector<int>> permute(vector<int>& nums) {
          res.clear();
          if(nums.size() == 0)
              return res;
          used = vector<bool>(nums.size(), false);
          vector<int> p;
          generatePermutation(nums, 0, p);
          return res;
      }
  };
  ```

### 1.2. Permutations II

[Permutations II](https://leetcode.com/problems/permutations-ii/description/)

- Question
  > Given a collection of numbers that might contain duplicates, return all possible unique permutations.

  Example:
  ```
  Input: [1,1,2]
  Output:
  [
    [1,1,2],
    [1,2,1],
    [2,1,1]
  ]
  ```
- Solution
	```java
	public List<List<Integer>> permuteUnique(int[] nums) {
			List<List<Integer>> list = new ArrayList<>();
			Arrays.sort(nums);
			backtrack(list, new ArrayList<>(), nums, new boolean[nums.length]);
			return list;
	}

	private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, boolean [] used){
			if(tempList.size() == nums.length)
					list.add(new ArrayList<>(tempList));
			else {
					for(int i = 0; i < nums.length; i++){
							if (used[i] || i > 0 && nums[i] == nums[i-1] && !used[i - 1]) 
									continue;
							used[i] = true; 
							tempList.add(nums[i]);
							backtrack(list, tempList, nums, used);
							used[i] = false; 
							tempList.remove(tempList.size() - 1);
					}
			}
	}
	```

### 1.3. 括号排列

- Question
	
	> 编写程序，输出 n 对括号的全部有序排列（即左右括号可以正确配对）。

- Solution

	从头构造字符串，逐一加入左括号或右括号：
	- 只要左括号还没用完，就可以加入左括号。
	- 只要右括号比左括号少，就可以加入右括号。

	也就是说，我们只需要记录允许插入的左右括号的数目。如果还有左括号可用，就插入一个左括号然后递归；如果剩下的右括号比左括号还多，就插入一个右括号然后递归。
	```java
	public ArrayList<String> generateParens(int n) {
		char [] str = new char[n * 2];
		ArrayList<String> list = new ArrayList<>();
		addParens(list, n, n, str, 0);
		return list;
	}

	/**
		* left 表示剩下的左括号数目
		* right 表示剩下的右括号数目
		*/
	public void addParens(ArrayList<String> list, int left, int right, char [] str, int n) {
		if (left < 0 || right < left)
			return;
		if (left == 0 && right == 0) 
			list.add(String.copyValueOf(str));
		else {
			if (left > 0) {
				str[n] = '(';
				addParens(list, left - 1, right, str, n + 1);
			}

			if (right > left) {
				str[n] = ')';
				addParens(list, left, right - 1, str, n + 1);
			}
		}
	}
	```

## 2. 组合问题 Combination

组合公式：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/9/9/597845501ac1a24c4eeef7d4961aea87.jpg)

指从给定 n 个数的元素中仅仅取出指定 r 个数的元素，不考虑排序。如：`C(9, 3) = (9 * 8 * 7) / (3 * 2 * 1) = 84`。

典型的组合问题：在一个给定的长度为 N 的序列中选取 M 个元素形成一个组合，求所有组合的方案。

```java
TODO:
```

### 2.1. Combinations

[77. Combinations](https://leetcode.com/problems/combinations/description/)

- Question
  > Given two integers n and k, return all possible combinations of k numbers out of 1 ... n. 
  
  Example:
  ```
  Input: n = 4, k = 2
  Output:
  [
    [2,4],
    [3,4],
    [2,3],
    [1,2],
    [1,3],
    [1,4],
  ]
  ```
- Solution

  根据题意可得递归树：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/21/ddf9fac10dc98c3bd7dd18617eca44de.jpg)

  ```cpp
  // 时间复杂度：O(n^k)，空间复杂度：O(k)
  class Solution {
  private:
      vector<vector<int>> res;
    
      // 求解 C(n,k), 当前已经找到的组合存储在 c 中，需要从 start 开始搜索新的元素
      void generateCombinations(int n, int k, int start, vector<int> &c){
          if (c.size() == k) {
              res.push_back(c);
              return;
          }
          // 还有 k - c.size() 个空位，所以，[i...n] 中至少要有 k - c.size() 个元素
          // i 最多为 n - (k - c.size()) + 1
          for (int i = start; i <= n - (k - c.size()) + 1; i++) {
              c.push_back(i);
              generateCombinations(n, k, i + 1 ,c);
              c.pop_back();
          }
          return;
      }
  public:
      vector<vector<int>> combine(int n, int k) {
          res.clear();
          if(n <= 0 || k <= 0 || k > n)
              return res;
          vector<int> c;
          generateCombinations(n, k, 1, c);
          return res;
      }
  };
  ```

### 2.2. Combination Sum

[39. Combination Sum](https://leetcode.com/problems/combination-sum/description/)

- Question
	> Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

- Solution
	```java
	public List<List<Integer>> combinationSum(int[] nums, int target) {
			List<List<Integer>> list = new ArrayList<>();
			Arrays.sort(nums);
			backtrack(list, new ArrayList<>(), nums, target, 0);
			return list;
	}

	private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start) { 
			if (remain < 0) 
					return;
			else if (remain == 0) 
					list.add(new ArrayList<>(tempList));
			else { 
					for (int i = start; i < nums.length; i++) {
							tempList.add(nums[i]);
							backtrack(list, tempList, nums, remain - nums[i], i); // not i + 1 because we can reuse same elements
							tempList.remove(tempList.size() - 1);
					}
			}
	}
	```

### 2.3. Combination Sum II

[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

- Question
	> Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.

	相比 Combination Sum 问题，Combination Sum II 的 candidate 中可能存在重复元素，因此需要在回溯时排除这些重复元素。

- Solution
	```java
	public List<List<Integer>> combinationSum2(int[] nums, int target) {
			List<List<Integer>> list = new ArrayList<>();
			Arrays.sort(nums);
			backtrack(list, new ArrayList<>(), nums, target, 0);
			return list;
	}

	private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start) {
			if (remain < 0) 
					return;	
			else if (remain == 0) 
					list.add(new ArrayList<>(tempList));
			else {
					for (int i = start; i < nums.length; i++) {
							if (i > start && nums[i] == nums[i-1]) // 因此需要先排序
									continue; // skip duplicates
							tempList.add(nums[i]);
							backtrack(list, tempList, nums, remain - nums[i], i + 1);
							tempList.remove(tempList.size() - 1); 
					}
			}
	}
	```

### 2.4. Combination Sum III

[216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

### 2.5. 组卷问题

网易互娱 2019.9.8 笔试题：

- Question
	> 每个题目包含以下属性：难度 X(1 <= X <= 5)、tag 数量 P 和 tag（每个题目可包含多个 tag）。在给定的 N 个题目中，选择 M 个组成试卷，要求试卷满足以下条件：
	> 1. M 个题目中最大难度和最小难度的差值不小于 S。
	> 2. M 个题目的难度之和不超过 L。
	> 3. M 个题目中任意 2 个题目的 tag 不重复。
	> 问：有多少种组卷方案？

	Example:
	```
	Input: 
	2								// 测试数据数目
	5 2 5 1					// N M L S
	1 2 string dfs	// X P tags
	1 1 heap
	2 1 dfs
	3 2 math search
	4 1 dp
	5 2 5 0					// X P tags
	1 2 string dfs
	1 1 heap
	2 1 dfs
	3 2 math search
	4 1 dp

	Output:
	6
	7
	```

- Solution

	使用回溯法：
	```java
	class Subject {
		int X;
		int P;
		ArrayList<String> tags = new ArrayList<>();
	}

	public class Main {
		private static int M;
		private static int N;
		private static ArrayList<Subject> subjects;

		public static void main(String[] args) {
			Scanner sc = new Scanner(System.in);
			int t = sc.nextInt();
			for (int i = 0; i < t; i++) {
				N = sc.nextInt();
				M = sc.nextInt();
				int L = sc.nextInt();
				int S = sc.nextInt();
				subjects = new ArrayList<>(N);
				for (int j = 0; j < N; j++) {
					Subject s = new Subject();
					s.X = sc.nextInt();
					s.P = sc.nextInt();
					for (int k = 0; k < s.P; k++) {
						s.tags.add(sc.next());
					}
					subjects.add(s);
				}
				// subjects.forEach(ele -> System.out.println(ele));

				List<List<Subject>> res = new ArrayList<>();
				for (int start = 0; start < N - M; start++)
					__solution(res, new ArrayList<>(), new HashSet<>(), start, L); // 以各个节点为起点分别调用回溯

				// System.out.println(res.size());
				// for (List<Subject> l : res) {
				// 	System.out.println("========");
				// 	l.forEach(ele -> System.out.println(ele));
				// }

				int cnt = 0;
				for (int j = 0; j < res.size(); j++) {
					List<Subject> subs = res.get(j);
					int maxX = Integer.MIN_VALUE, minX = Integer.MAX_VALUE;
					for (int k = 0; k < subs.size(); k++) {
						maxX = Math.max(maxX, subs.get(k).X);
						minX = Math.min(minX, subs.get(k).X);
					}
					if (maxX - minX >= S) {
						cnt++;
					}
				}
				System.out.println(cnt);
			}
		}

		private static void __solution(List<List<Subject>> res,
										List<Subject> pickedSubs,
										Set<String> pickedTags, 								   
										int start,
										int L) {
			if (pickedSubs.size() == M) {
				res.add(new ArrayList<>(pickedSubs));
				return;
			}
			if (pickedSubs.size() < M) {

				for (int i = start; i < subjects.size(); i++) {
					boolean isLegal = true;

					ArrayList<String> tags = subjects.get(i).tags;
					for (int j = 0; j < tags.size(); j++)
						if (!pickedTags.add(tags.get(j)))
							isLegal = false;

					if (subjects.get(i).X > L)
						isLegal = false;

					if (isLegal) {
						pickedSubs.add(subjects.get(i));
						__solution(res, pickedSubs, pickedTags, i  + 1,
									L - subjects.get(i).X);
						pickedSubs.remove(pickedSubs.size() - 1);
					}
				}
			}
		}
	}
	```

### 2.6. Subsets

[78. Subsets](https://leetcode.com/problems/subsets/description/)

- Question
	> Given a set of distinct integers, nums, return all possible subsets (the power set).

	Example:
	```
	Input: nums = [1,2,3]
	Output:
	[
		[3],
		[1],
		[2],
		[1,2,3],
		[1,3],
		[2,3],
		[1,2],
		[]
	]
	```

- Solution
	```java
	public List<List<Integer>> subsets(int[] nums) {
			List<List<Integer>> list = new ArrayList<>();
			Arrays.sort(nums);
			backtrack(list, new ArrayList<>(), nums, 0);
			return list;
	}

	private void backtrack(List<List<Integer>> list , List<Integer> tempList, int [] nums, int start) {
			list.add(new ArrayList<>(tempList));
			for(int i = start; i < nums.length; i++) {
					tempList.add(nums[i]);
					backtrack(list, tempList, nums, i + 1);
					tempList.remove(tempList.size() - 1);
			}
	}
	```

### 2.7. Subsets II

[90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)

TODO:

- Question
	> Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).

	Example:
	```
	Input: [1,2,2]
	Output:
	[
		[2],
		[1],
		[1,2,2],
		[2,2],
		[1,2],
		[]
	]
	```

- Solution
	```java
	public List<List<Integer>> subsetsWithDup(int[] nums) {
			List<List<Integer>> list = new ArrayList<>();
			Arrays.sort(nums);
			backtrack(list, new ArrayList<>(), nums, 0);
			return list;
	}

	private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int start) {
			list.add(new ArrayList<>(tempList));
			for (int i = start; i < nums.length; i++) {
					if (i > start && nums[i] == nums[i-1]) // 因此需要先排序
							continue; // skip duplicates 
					tempList.add(nums[i]);
					backtrack(list, tempList, nums, i + 1);
					tempList.remove(tempList.size() - 1);
			}
	} 
	```

### 2.8. Binary Watch

[401. Binary Watch](https://leetcode.com/problems/binary-watch/description/)

### 2.9. 硬币表示问题

《CC150》 9.8

- Question
  > 给定数量不限的硬币，币值分别为 25 分、10 分、5 分和 1 分，编写程序求得 n 分有几种表示方法。

- Solution

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/21/91ca283f09b814ece6acab4f76818c87.jpg)

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/21/1882cec5bda5a5af1ebea1353c01cd88.jpg)

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/21/70d74cde74bea5893ce6816bb03dbbe6.jpg)

  ```java
	public int solve(int n) {
		return makeChange(n, 25);
	}

	public int makeChange(int n, int denom) {
		int nextDenom = 0;
		switch (denom) {
		case 25:
			nextDenom = 10;
			break;
		case 10:
			nextDenom = 5;
			break;
		case 5:
			nextDenom = 1;
			break;
		case 1:
			return 1;
		}

		int ways = 0;
		for (int i = 0; i * denom <= n; i++)
			ways += makeChange(n - i * denom, nextDenom);
		
		return ways;
	}
	```

## 3. 二维平面问题

### 3.1. 派送员问题

- Question
  
  某物流派送员 p，需要给 a、b、c、d 共 4 个快递点派送包裹，请问派送员需要选择什么的路线，才能完成最短路程的派送。假设如图派送员的起点坐标 (0,0)，派送路线只能沿着图中的方格边行驶，每个小格都是正方形，且边长为 1，如 p 到 d 的距离就是 4。随机输入 n 个派送点坐标，求输出最短派送路线值（从起点开始完成 n 个点派送并回到起始点的距离）。

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/8/22/d63ab524c1a5e6d852e4f250a6b71fae.jpg)

- Solution

	采用回溯法求解：
	```java
	import java.util.Scanner;

	public class Main {
		/**
		* 坐标类
		*/
		private static class Point {
			private int x;
			private int y;
			private boolean visted;
			Point(int x, int y) {
				this.x = x;
				this.y = y;
				this.visted = false;
			}

			/**
			* 获取两点之间的最短距离
			*/
			int getDistance(Point point) {
				return Math.abs(x - point.x) + Math.abs(y - point.y);
			}
		}

		// 最小派送距离，初始值为无限大
		private static int minPath = Integer.MAX_VALUE;

		// 需要派送的坐标数组
		private static Point [] targetPoints;

		/**
		* 回溯法求出最小的派送距离
		* @param curPosition 当前起始点
		* @param sumOfPath 当前已走过的距离
		* @param finishedNum 当前已送达的坐标数量
		* @return 最小派送距离
		*/
		private static int caculate(Point curPosition, int sumOfPath, int finishedNum) {
			if (finishedNum == targetPoints.length) {
				minPath = Math.min(minPath, sumOfPath + curPosition.getDistance(new Point(0, 0)));
				return minPath;
			}

			for (Point point : targetPoints) {
				int distance = point.getDistance(curPosition);
				if (!point.visted && sumOfPath + distance < minPath) {
					point.visted = true;
					caculate(point, sumOfPath + distance, finishedNum + 1); // TODO: 为什么这里不设置回 false？
				}
			}
			return minPath;
		}

		public static void main(String[] args) {
			Scanner input = new Scanner(System.in);
			int num = Integer.parseInt(input.nextLine().trim());
			Point [] points = new Point[num];
			for (int i = 0; i < num; i++) {
				String pointsStr [] = input.nextLine().trim().split(",");
				points[i] = new Point(Integer.parseInt(pointsStr[0]), Integer.parseInt(pointsStr[1]));
			}
			Main.targetPoints = points;
			System.out.println(Main.caculate(new Point(0, 0), 0, 0));
		}
	}
	```

### 3.2. Word Search

[79. Word Search](https://leetcode.com/problems/word-search/description/)

- Question
  > Given a 2D board and a word, find if the word exists in the grid.
  > 
  > The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

  Example:
  ```
  board =
  [
    ['A','B','C','E'],
    ['S','F','C','S'],
    ['A','D','E','E']
  ]

  Given word = "ABCCED", return true.
  Given word = "SEE", return true.
  Given word = "ABCB", return false.
  ```
- Solution
  
  根据题意可得递归树：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/21/77a27a2cf80975d0bd98cbd9d4c953f4.jpg)

  代码实现：
  ```cpp
  // 时间复杂度：O(m*n*m*n)，空间复杂度：O(m*n)
  class Solution {
  private:
      int d[4] [2] = {{-1, 0}, {0,1}, {1, 0}, {0, -1}};
      int m, n;
      vector<vector<bool>> visited;

      bool inArea(int x, int y){
          return x >= 0 && x < m && y >= 0 && y < n;
      }

      // 从 board[startx] [starty] 开始，寻找 word[index...word.size())
      bool searchWord(const vector<vector<char>> &board, const string& word, int index,
                      int startx, int starty ){
          //assert(inArea(startx,starty));
          if(index == word.size() - 1)
              return board[startx] [starty] == word[index];

          if(board[startx] [starty] == word[index]){
              visited[startx] [starty] = true;
              // 从 startx, starty 出发，向四个方向寻
              for(int i = 0 ; i < 4 ; i ++){
                  int newx = startx + d[i] [0];
                  int newy = starty + d[i] [1];
                  if(inArea(newx, newy) && !visited[newx] [newy] &&
                    searchWord(board, word, index + 1, newx, newy))
                      return true;
              }
              visited[startx] [starty] = false;
          }
          return false;
      }

  public:
      bool exist(vector<vector<char>>& board, string word) {
          m = board.size();
          assert(m > 0);
          n = board[0].size();
          assert(n > 0);

          visited.clear();
          for(int i = 0 ; i < m ; i ++)
              visited.push_back(vector<bool>(n, false));

          for(int i = 0 ; i < board.size() ; i ++)
              for(int j = 0 ; j < board[i].size() ; j ++)
                  if(searchWord(board, word, 0, i, j))
                      return true;

          return false;
      }
  };
  ```

	```java
	public class Solution {
			public boolean exist(char[][] board, String word) {
					boolean [][] visited = new boolean[board.length](board[0).length];
					
					for (int i = 0; i < board.length; i++)
							for (int j = 0; j < board[i].length; j++)
									if ((word.charAt(0) == board[i](j)) && search(board, word, i, j, 0, visited))
											return true;
					return false;
			}

			private boolean search(char [][] board, String word, int i, int j, int index, boolean [][] visited) {
					if (index == word.length())
							return true;
					
					if (i >= board.length || i < 0 || j >= board[i].length || j < 0 || board[i](j) != word.charAt(index) || visited[i](j))
							return false;
					
					visited[i](j) = true;
					if (search(board, word, i-1, j, index+1, visited) || 
							search(board, word, i+1, j, index+1, visited) ||
							search(board, word, i, j-1, index+1, visited) || 
							search(board, word, i, j+1, index+1, visited))
							return true;
					visited[i](j) = false;
					return false;
			}
	}
	```

	优化：利用异或操作在原二维数组上标记 visited，而不创建额外的辅助数组。
	```java
	class Solution {
			public boolean exist(char[][] board, String word) {
					for (int i = 0; i < board.length; i++)
							for (int j = 0; j < board[i].length; j++)
									if ((word.charAt(0) == board[i](j)) && search(board, word, i, j, 0))
											return true;
					return false;
			}

			private boolean search(char [][] board, String word, int i, int j, int index) {
					if (index == word.length())
							return true;

					if (i >= board.length || i < 0 || j >= board[i].length || j < 0 || board[i](j) != word.charAt(index))
							return false;

					board[i](j) ^= 256;
					if (search(board, word, i-1, j, index+1) || 
									search(board, word, i+1, j, index+1) ||
									search(board, word, i, j-1, index+1) || 
									search(board, word, i, j+1, index+1))
							return true;
					board[i](j) ^= 256;

					return false;
			}
	}
	```

	进一步优化：使用 Trie 树

### 3.3. Word Search II

[212. Word Search II](https://leetcode.com/problems/word-search-ii/description/)

### 3.4. Flood Fill 问题

#### 3.4.1. Number of Islands

[200. Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

- Question
  > Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

  Example 1:
  ```
  11110
  11010
  11000
  00000
  Answer: 1
  ```
  Example 2:
  ```
  11000
  00100
  00011
  Answer: 3
  ```
- Solution
  
	使用递归实现的 DFS：
	```cpp
  // 时间复杂度：O(n*m), 空间复杂度：O(n*m)
  class Solution {
  private:
      int d[4] [2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
      int m, n;
      vector<vector<bool>> visited;

      bool inArea(int x, int y){
          return x >= 0 && x < m && y >= 0 && y < n;
      }

      // 从 grid[x] [y] 的位置开始，进行 floodfill
      // 保证 (x,y) 合法，且 grid[x](y) 是没有被访问过的陆地
      void dfs(vector<vector<char>>& grid, int x, int y){
          //assert(inArea(x,y));
          visited[x] [y] = true;
          for(int i = 0; i < 4; i ++){
              int newx = x + d[i] [0];
              int newy = y + d[i] [1];
              if(inArea(newx, newy) && !visited[newx] [newy] && grid[newx] [newy] == '1')
                  dfs(grid, newx, newy);
          }
          return;
      }

  public:
      int numIslands(vector<vector<char>>& grid) {
          m = grid.size();
          if(m == 0)
              return 0;
          n = grid[0].size();
          if(n == 0)
              return 0;

          for(int i = 0 ; i < m ; i ++)
              visited.push_back(vector<bool>(n, false));

          int res = 0;
          for(int i = 0 ; i < m ; i ++)
              for(int j = 0 ; j < n ; j ++)
                  if(grid[i] [j] == '1' && !visited[i] [j]){
                      dfs(grid, i, j);
                      res ++;
                  }
          return res;
      }
  };
  ```

#### 3.4.2. Surrounded Regions

[130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/)

#### 3.4.3. Pacific Atlantic Water Flow

[417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)

### 3.5. N-Queues 问题

#### 3.5.1. N-Queens

[51. N-Queens](https://leetcode.com/problems/n-queens/description/)

- Question
  > The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.
  > 
  > Given an integer n, return all distinct solutions to the n-queens puzzle.
  > 
  > Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.
  Example:
  ```
  Input: 4
  Output: [
  [".Q..",  // Solution 1
    "...Q",
    "Q...",
    "..Q."],

  ["..Q.",  // Solution 2
    "Q...",
    "...Q",
    ".Q.."]
  ]
  Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.
  ```
- Solution

  根据题意可得到如下形式的递归树：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/22/612b03b64145c0f2bec580fe7753f82c.jpg)

  使用 dia1 数组表示右上 - 左下的 2n-1 条对角线，每条对角线用 i+j 表示：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/22/d3fa474be5deb7db5ffad136b25c3591.jpg)
  
  使用 dia2 数组表示左上 - 右下的 2n-1 条对角线，每条对角线用 i-j 表示：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/22/8790cda6b0c5d7ca965588e0b1722051.jpg)

  ```cpp
  /// 时间复杂度：O(n^n), 空间复杂度：O(n)
  class Solution {
  private:
      // 使用 col[i] 表示第 i 列被占用
      // dia1[i] 表示第 i 对角线 1 被占用
      // dia2[i] 表示第 i 对角线 2 被占用
      vector<bool> col, dia1, dia2;
      vector<vector<string>> res;

      // 尝试在一个 n 皇后问题中，摆放第 index 行的皇后位置
      void putQueen(int n, int index, vector<int> &row) {
          if(index == n) {
              res.push_back(generateBoard(n, row));
              return;
          }
          for(int i = 0 ; i < n ; i ++)
              // 尝试将第 index 行的皇后摆放在第 i 列
              if(!col[i] && !dia1[index + i] && !dia2[index - i + n - 1]) {
                  row.push_back(i);
                  col[i] = true;
                  dia1[index + i] = true;
                  dia2[index - i + n - 1] = true;
                  putQueen(n, index + 1, row);
                  col[i] = false;
                  dia1[index + i] = false;
                  dia2[index - i + n - 1] = false;
                  row.pop_back();
              }
          return;
      }

      vector<string> generateBoard(int n, vector<int> &row){
          assert(row.size() == n);
          vector<string> board(n, string(n, '.'));
          for(int i = 0 ; i < n ; i ++)
              board[i] [row[i]] = 'Q';
          return board;
      }

  public:
      vector<vector<string>> solveNQueens(int n) {
          res.clear();

          col.clear();
          for(int i = 0 ; i < n ; i ++)
              col.push_back(false);

          dia1.clear();
          dia2.clear();
          for(int i = 0 ; i < 2 * n - 1 ; i ++){
              dia1.push_back(false);
              dia2.push_back(false);
          }

          vector<int> row;
          putQueen(n, 0, row);

          return res;
      }
  };
  ```

#### 3.5.2. N-Queens II

[52. N-Queens II](https://leetcode.com/problems/n-queens-ii/description/)

### 3.6. Sudoku Solver

[37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)

## 4. 其它问题

### 4.1. Letter Combinations of a Phone Number

[17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

- Question
  > Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.
  > 
  > A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/21/5912841f0ce99eaad34e42abb2e39409.jpg)

  Example:
  ```
  Input: "23"
  Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
  ```

- Solution

  根据题意进行分析，当输入 23 时，可得到以下解空间树：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/21/b151f66aba167a367497d05078cbc429.jpg)

  进一步分析可得到递归表达式：
  ```
  s(digits[0...n-1])  = letter(digits[0]) + s(digits[1...n-1]) 
                      = letter(digits[0]) + letter(digits[1]) + s(digits[2...n-1])
                      = ...
                      = letter(digits[0]) + letter(digits[1]) + ... + letter(digits[n-1])
  ```

  代码实现：
  ```cpp
  // 时间复杂度：O(2^len(s)), 空间复杂度：O(len(s))
  class Solution {
  private:
      const string letterMap[10] = {
              " ",    //0
              "",     //1
              "abc",  //2
              "def",  //3
              "ghi",  //4
              "jkl",  //5
              "mno",  //6
              "pqrs", //7
              "tuv",  //8
              "wxyz"  //9
      };

      // 保存结果
      vector<string> res;

      // s 中保存了此时从 digits[0...index-1] 翻译得到的一个字母字符串
      // 寻找和 digits[index] 匹配的字母，获得 digits[0...index] 翻译得到的解
      void findCombination(const string &digits, int index, const string &s){
          // cout << index << " : " << s << endl;
          if(index == digits.size()){
              res.push_back(s);
              // cout << "get " << s << " , return" << endl;
              return;
          }

          char c = digits[index];
          assert(c >= '0' && c <= '9' && c != '1');
          string letters = letterMap[c - '0'];
          for(int i = 0 ; i < letters.size() ; i ++){
              // cout << "digits[" << index << "] = " << c << " , use " << letters[i] << endl;
              findCombination(digits, index+1, s + letters[i]);
          }
          // cout << "digits[" << index << "] = " << c << " complete, return" << endl;
          return;
      }

  public:
      vector<string> letterCombinations(string digits) {
          res.clear();
          if(digits == "")
              return res;
          findCombination(digits, 0, "");
          return res;
      }
  };
  ```

### 4.2. Restore IP Addresses

[93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)

- Question
  > Given a string containing only digits, restore it by returning all possible valid IP address combinations.
  Example:
  ```
  Input: "25525511135"
  Output: ["255.255.11.135", "255.255.111.35"]
  ```
- Solution
  
### 4.3. Palindrome Partitioning

[131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/description/)

- Question
  > Given a string s, partition s such that every substring of the partition is a palindrome.
  > 
  > Return all possible palindrome partitioning of s.

  Example:
  ```
  Input: "aab"
  Output:
  [
    ["aa","b"],
    ["a","a","b"]
  ]
  ```
- Solution
	```java
	public List<List<String>> partition(String s) {
		List<List<String>> list = new ArrayList<>();
		backtrack(list, new ArrayList<>(), s, 0);
		return list;
	}

	public void backtrack(List<List<String>> list, List<String> tempList, String s, int start){
		if (start == s.length())
				list.add(new ArrayList<>(tempList));
		else {
				for (int i = start; i < s.length(); i++){
						if (isPalindrome(s, start, i)){
								tempList.add(s.substring(start, i + 1));
								backtrack(list, tempList, s, i + 1);
								tempList.remove(tempList.size() - 1);
						}
				}
		}
	}

	public boolean isPalindrome(String s, int low, int high){
		while(low < high)
				if(s.charAt(low++) != s.charAt(high--)) return false;
		return true;
	} 
	```

## 5. Refer Links
