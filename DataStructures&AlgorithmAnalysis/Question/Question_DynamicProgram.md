- [Dynamic programming](#dynamic-programming)
    - [1. 斐波那契数列 Fibonacci polynomial](#1--fibonacci-polynomial)
        - [1.1. 爬楼梯问题 Climbing Stairs](#11--climbing-stairs)
        - [1.2. Triangle](#12-triangle)
        - [1.3. Minimum Path Sum](#13-minimum-path-sum)
    - [2. 整数分解](#2)
        - [2.1. Integer Break](#21-integer-break)
        - [2.2. Perfect Squares](#22-perfect-squares)
        - [2.3. Decode Ways](#23-decode-ways)
        - [2.4. Unique Paths](#24-unique-paths)
        - [2.5. Unique Paths II](#25-unique-paths-ii)
    - [3. 抢劫房子问题](#3)
        - [3.1. House Robber](#31-house-robber)
        - [3.2. House Robber II](#32-house-robber-ii)
        - [3.3. House Robber III](#33-house-robber-iii)
        - [3.4. Best Time to Buy and Sell Stock with Cooldown](#34-best-time-to-buy-and-sell-stock-with-cooldown)
    - [4. 背包问题](#4)
        - [4.1. Partition Equal Subset Sum](#41-partition-equal-subset-sum)
        - [4.2. Coin Change](#42-coin-change)
        - [4.3. Combination Sum IV](#43-combination-sum-iv)
        - [4.4. Ones and Zeroes](#44-ones-and-zeroes)
        - [4.5. Word Break](#45-word-break)
        - [4.6. Target Sum](#46-target-sum)
    - [5. 最长上升子序列 （LIS 问题）](#5--lis)
        - [5.1. Longest Increasing Subsequence](#51-longest-increasing-subsequence)
        - [5.2. Wiggle Subsequence](#52-wiggle-subsequence)
    - [6. 最长公共子序列（LCS 问题）](#6-lcs)
    - [7. Refer Links](#7-refer-links)

# Dynamic programming

## 1. 斐波那契数列 Fibonacci polynomial

斐波那契数列的递推表达式为：
```
F(0)=1
F(1)=1
F(n)=F(n-1)+F(n-2)
```

其递归树如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/c5e3885ef8809db940b6625156894db4.jpg)

可见，其中包含了大量的重叠子问题，因此可以通过记忆化搜索（自顶向下，假设基本问题已经解决）和动态规划（自低向上，从基本问题递推到复杂问题）来解决。

- 记忆化搜索（自顶向下）
  ```cpp
  vector<int> memo = vector<int>(n + 1, -1);
  int fib(int n){
      if(n == 0)
          return 0;

      if(n == 1)
          return 1;

      if(memo[n] == -1)
          memo[n] = fib(n - 1) + fib(n - 2);

      return memo[n];
  }
  ```

- 动态规划（自底向上）
  ```cpp
  // 动态规划
  int fib( int n ){
      vector<int> memo(n + 1, -1);

      memo[0] = 0;
      memo[1] = 1;
      for(int i = 2 ; i <= n ; i ++)
          memo[i] = memo[i - 1] + memo[i - 2];

      return memo[n];
  }
  ```

### 1.1. 爬楼梯问题 Climbing Stairs

[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)

- Question
  > You are climbing a stair case. It takes n steps to reach to the top.
  >
  > Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

  Note: Given n will be a positive integer.

  Example 1:
  ```
  Input: 2
  Output: 2
  Explanation: There are two ways to climb to the top.
  1. 1 step + 1 step
  2. 2 steps
  ```
  Example 2:
  ```
  Input: 3
  Output: 3
  Explanation: There are three ways to climb to the top.
  1. 1 step + 1 step + 1 step
  2. 1 step + 2 steps
  3. 2 steps + 1 step
  ```
- Solution
  - 自顶向下

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/326b837d0a0d9848714011b5f0a6f518.jpg)

    ```cpp
    // 时间复杂度：O(n), 空间复杂度：O(n)
    class Solution {
    private:
        vector<int> memo;
        int calcWays(int n){ 
            if(n == 0 || n == 1)
                return 1;
            if(memo[n] == -1)
                memo[n] = calcWays(n - 1) + calcWays(n - 2);
            return memo[n];
        }
    public:
        int climbStairs(int n) {
            memo = vector<int>(n + 1, -1);
            return calcWays(n);
        }
    };
    ```

  - 自底向上
    ```cpp
    // 时间复杂度：O(n), 空间复杂度：O(n)
    class Solution {
    public:
        int climbStairs(int n) {
            vector<int> memo(n + 1, -1);
            memo[0] = 1;
            memo[1] = 1;
            for(int i = 2 ; i <= n ; i ++)
                memo[i] = memo[i - 1] + memo[i - 2];
            return memo[n];
        }
    };
    ```

### 1.2. Triangle

[120. Triangle](https://leetcode.com/problems/triangle/description/)

### 1.3. Minimum Path Sum

[64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/description/)

## 2. 整数分解

### 2.1. Integer Break

[343. Integer Break](https://leetcode.com/problems/integer-break/description/)

- Question
  > Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.
  > 
  > For example, given n = 2, return 1 (2 = 1 + 1); given n = 10, return 36 (10 = 3 + 3 + 4).

  Note: You may assume that n is not less than 2 and not larger than 58.
- Solution
  - 自顶向下

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/928da10fa0f8b2ca0cd4a012c8703e83.jpg)

    ```cpp
    // 时间复杂度：O(n^2), 空间复杂度：O(n)
    class Solution {
    private:
        vector<int> memo;

        int max3(int a, int b, int c){
            return max(a, max(b, c));
        }

        // 将 n 进行分割（至少分割两部分), 可以获得的最大乘积
        int breakInteger(int n){
            if(n == 1)
                return 1;

            if(memo[n] != -1)
                return memo[n];

            int res = -1;
            for(int i = 1 ; i <= n - 1 ; i ++)
                res = max3(res, i * (n - i) , i * breakInteger(n - i));
            memo[n] = res;

            return res;
        }

    public:
        int integerBreak(int n) {
            assert(n >= 1);
            memo.clear();
            for(int i = 0 ; i < n + 1 ; i ++)
                memo.push_back(-1);
            return breakInteger(n);
        }
    };
    ```

  - 自底向上

    ```cpp
    // 时间复杂度：O(n^2), 空间复杂度：O(n)
    class Solution {
    private:
        int max3(int a, int b, int c ){
            return max(max(a, b), c);
        }

    public:
        int integerBreak(int n) {
            assert(n >= 1);
            // memo[i] 表示将数字 i 分割（至少分割成两部分) 后得到的最大乘积
            vector<int> memo(n + 1, -1);
            memo[1] = 1;
            for(int i = 2 ; i <= n ; i ++)
                for(int j = 1 ; j <= i - 1 ; j ++) // 求解 memo[i]
                    memo[i] = max3(memo[i], j * (i - j), j * memo[i - j]);

            return memo[n];
        }
    };
    ```

### 2.2. Perfect Squares

[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

### 2.3. Decode Ways

[91. Decode Ways](https://leetcode.com/problems/decode-ways/description/)

### 2.4. Unique Paths

[62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)

### 2.5. Unique Paths II

[63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/)

## 3. 抢劫房子问题

### 3.1. House Robber

[198. House Robber](https://leetcode.com/problems/house-robber/description/)

- Question
	> You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
	> 
	> Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.
- Solution

- 暴力解法

	检查所有房子的组合，对每一个组合，检查是否有相邻的房子，如果没有，记录其价值。最后找最大值即可。

	时间复杂度为 O((2^n)*n)。

- 动态规划
	
	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/24c86a15cf1e053e9658340ff06ae592.jpg)

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/071ec2923b07b87330f52a8b2611baad.jpg)

	```cpp
	// 时间复杂度：O(n^2), 空间复杂度：O(n)
	class Solution {
	private:
			// memo[i] 表示考虑抢劫 nums[i...n) 所能获得的最大收益
			vector<int> memo;

			// 考虑抢劫 nums[index...nums.size()) 这个范围的所有房子
			int tryRob(const vector<int> &nums, int index){

					if(index >= nums.size())
							return 0;

					if(memo[index] != -1)
							return memo[index];

					int res = 0;
					for(int i = index ; i < nums.size() ; i ++)
							res = max(res, nums[i] + tryRob(nums, i + 2));
					memo[index] = res;
					return res;
			}

	public:
			int rob(vector<int>& nums) {
					memo.clear();
					for(int i = 0 ; i < nums.size() ; i ++)
							memo.push_back(-1);
					return tryRob(nums, 0);
			}
	};
	```

	```cpp
	// 时间复杂度：O(n^2), 空间复杂度：O(n)
	class Solution {
	public:
			int rob(vector<int>& nums) {
					int n = nums.size();
					if(n == 0)
							return 0;
					// memo[i] 表示考虑抢劫 nums[i...n) 所能获得的最大收益
					vector<int> memo(n, 0);
					memo[n - 1] = nums[n - 1];
					for(int i = n - 2 ; i >= 0 ; i --)
							for (int j = i; j < n; j++)
									memo[i] = max(memo[i],
																nums[j] + (j + 2 < n ? memo[j + 2] : 0));

					return memo[0];
			}
	};
	```

### 3.2. House Robber II

[213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

### 3.3. House Robber III

[Ho337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

### 3.4. Best Time to Buy and Sell Stock with Cooldown

[309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

## 4. 背包问题

对于 0-1 背包问题，需要用 2 个维度的参数进行状态定义：
```
F(n, C) 表示将 n 个物品放进容量为 C 的背包，使其总价值最大。
```
则状态转移方程为：
```
F(i, c) = max(F(i-1, c), v(i) + F(i-1, c-w(i)))
```

代码实现：
```cpp
// 时间复杂度：O(n * C) 其中 n 为物品个数；C 为背包容积，空间复杂度：O(n * C)
class Knapsack01{
private:
    vector<vector<int>> memo;
    // 用 [0...index] 的物品，填充容积为 c 的背包的最大价值
    int bestValue(const vector<int> &w, const vector<int> &v, int index, int c){
        if(c <= 0 || index < 0)
            return 0;

        if(memo[index] [c] != -1)
            return memo[index](c);
				// 策略 1：不放第 i 个物品
        int res = bestValue(w, v, index-1, c);
        if(c >= w[index])
						// 策略 2：放第 i 个物品
            res = max(res, v[index] + bestValue(w, v, index - 1, c - w[index]));
        memo[index] [c] = res;
        return res;
    }

public:
    int knapsack01(const vector<int> &w, const vector<int> &v, int C){
        assert(w.size() == v.size() && C >= 0);
        int n = w.size();
        if(n == 0 || C == 0)
            return 0;

        memo.clear();
        for(int i = 0 ; i < n ; i ++)
            memo.push_back(vector<int>(C + 1, -1));
        return bestValue(w, v, n - 1, C);
    }
};
```

```cpp
/// 时间复杂度：O(n * C) 其中 n 为物品个数；C 为背包容积，空间复杂度：O(n * C)
class Knapsack01{
public:
    int knapsack01(const vector<int> &w, const vector<int> &v, int C){
        assert(w.size() == v.size() && C >= 0);
        int n = w.size();
        if(n == 0 || C == 0)
            return 0;
				// 二维数组 memo 表示用 [0...n) 的物品，填充容积为 C 的背包的最大价值
        vector<vector<int>> memo(n, vector<int>(C + 1,0));

        for(int j = 0 ; j <= C ; j ++)
            memo[0] [j] = (j >= w[0] ? v[0] : 0 );

        for(int i = 1 ; i < n ; i ++)
            for(int j = 0 ; j <= C ; j ++){
								// 策略 1：不放第 i 个物品
                memo[i] [j] = memo[i-1] [j];
                if(j >= w[i])
										// 策略 2：放第 i 个物品
                    memo[i] [j] = max(memo[i] [j], v[i] + memo[i - 1] [j - w[i]]);
            }
        return memo[n - 1] [C];
    }
};
```

### 4.1. Partition Equal Subset Sum

[416. Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)

问题可转化为：在 n 个物品中选出一定物品，填满 sum/2 的背包。

状态定义：F(n, C) 表示将 n 个物品填满容量为 C 的背包。

状态转移方程：F(i, c) = F(i - 1, c) || F(i - 1, c - w(i))。

代码实现：
```cpp
/// 时间复杂度：O(len(nums) * O(sum(nums)))
/// 空间复杂度：O(len(nums) * O(sum(nums)))
class Solution {
private:
    // memo[i](c) 表示使用索引为 [0...i] 的这些元素，是否可以完全填充一个容量为 c 的背包
    // -1 表示为未计算；0 表示不可以填充；1 表示可以填充
    vector<vector<int>> memo;

    // 使用 nums[0...index], 是否可以完全填充一个容量为 sum 的背包
    bool tryPartition(const vector<int> &nums, int index, int sum){

        if(sum == 0)
            return true;

        if(sum < 0 || index < 0)
            return false;

        if(memo[index](sum) != -1)
            return memo[index](sum) == 1;

        memo[index](sum) = (tryPartition(nums, index - 1, sum) ||
               tryPartition(nums, index - 1, sum - nums[index])) ? 1 : 0;

        return memo[index](sum) == 1;
    }

public:
    bool canPartition(vector<int>& nums) {

        int sum = 0;
        for(int i = 0 ; i < nums.size() ; i ++){
            assert(nums[i] > 0);
            sum += nums[i];
        }

        if(sum % 2)
            return false;

        memo.clear();
        for(int i = 0 ; i < nums.size() ; i ++)
            memo.push_back(vector<int>(sum / 2 + 1, -1));
        return tryPartition(nums, nums.size() - 1, sum / 2);
    }
};
```

```cpp
/// 时间复杂度：O(len(nums) * O(sum(nums)))
/// 空间复杂度：O(len(nums) * O(sum(nums)))
class Solution {
public:
    bool canPartition(vector<int>& nums) {

        int sum = 0;
        for(int i = 0 ; i < nums.size() ; i ++){
            assert(nums[i] > 0);
            sum += nums[i];
        }

        if(sum % 2)
            return false;

        int n = nums.size();
        int C = sum / 2;
        vector<bool> memo(C + 1, false);
        for(int i = 0 ; i <= C ; i ++)
            memo[i] = (nums[0] == i);

        for(int i = 1 ; i < n ; i ++)
            for(int j = C; j >= nums[i] ; j --)
                memo[j] = memo[j] || memo[j - nums[i]];

        return memo[C];
    }
};
```

### 4.2. Coin Change

[322. Coin Change](https://leetcode.com/problems/coin-change/description/)

### 4.3. Combination Sum IV

[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)

### 4.4. Ones and Zeroes

[474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)

### 4.5. Word Break

[139. Word Break](https://leetcode.com/problems/word-break/description/)

### 4.6. Target Sum

[494. Target Sum](https://leetcode.com/problems/target-sum/description/)

## 5. 最长上升子序列 （LIS 问题）

### 5.1. Longest Increasing Subsequence

[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

- Question
	> Given an unsorted array of integers, find the length of longest increasing subsequence.

	For example,
	> Given [10, 9, 2, 5, 3, 7, 101, 18], the longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

	Your algorithm should run in O(n^2) complexity.

	Follow up: Could you improve it to O(n log n) time complexity?
- Solution

	- 暴力解法

		序列的每个元素都有 2 种选择：放入 LIS/ 不放入 LIS，因此，若对序列的所有子序列进行判断，时间复杂度为 O((2^n)*n)。
	
	- 动态规划

		由于在该问题的解空间中存在重叠子问题和最优子结构，因此可以采用记忆化搜索 / 动态规划的方法进行求解。

		状态定义：LIS(i) 表示以第 i 个数字为结尾的最长上升子序列的长度。

		状态转移方程：LIS(i) = max(1 + LIS(j) if nums[i] > nums[j]) (i > j).

		例：
		
		![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/67326c8c913d5ec4fe36c12a85e6028d.jpg)

		代码实现：
		```cpp
		// 时间复杂度：O(n^2), 空间复杂度：O(n)
		class Solution {
		private:
				vector<int> memo;

				// 以 nums[index] 为结尾的最长上升子序列的长度
				int getMaxLength(const vector<int> &nums, int index){
						if(memo[index] != -1)
								return memo[index];

						int res = 1;
						for(int i = 0 ; i <= index-1 ; i ++)
								if(nums[index] > nums[i])
										res = max(res, 1 + getMaxLength(nums, i));

						memo[index] = res;
						return res;
				}
		public:
				int lengthOfLIS(vector<int>& nums) {
						if(nums.size() == 0)
								return 0;

						memo = vector<int>(nums.size(), -1);
						int res = 1;
						for(int i = 0 ; i < nums.size() ; i ++)
								res = max(res, getMaxLength(nums, i));

						return res;
				}
		};
		```
		```cpp
		// 时间复杂度：O(n^2), 空间复杂度：O(n)
		class Solution {
		public:
				int lengthOfLIS(vector<int>& nums) {
						if(nums.size() == 0)
								return 0;

						// memo[i] 表示以 nums[i] 为结尾的最长上升子序列的长度
						vector<int> memo(nums.size(), 1);
						for(int i = 1 ; i < nums.size() ; i ++)
								for(int j = 0 ; j < i ; j ++)
										if(nums[i] > nums[j])
												memo[i] = max(memo[i], 1 + memo[j]);

						// 求出最长的长度
						int res = memo[0];
						for(int i = 1 ; i < nums.size() ; i ++)
								res = max(res, memo[i]);

						return res;
				}
		};
		```

### 5.2. Wiggle Subsequence

[376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)

## 6. 最长公共子序列（LCS 问题）

> 给出 s1 和 s2，求出它们的最长公共子序列的长度。

该问题被广泛的应用在各个领域，如在基因工程中，可通过计算两个基因间的 LCS 长度，求出它们的相似程度。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/a3eab068b81da598f8f918123dcca890.jpg)

LCS 问题需要使用 2 个维度的参数进行状态定义：
```
LCS(m, n) 表示 s1[0...m] 和 s2[0...n] 的最长公共子序列的长度。
```
状态转移方程：
```
LCS(m, n) = 1 + LCS(m-1, n-1). (s1[m] == s2[n])
LCS(m, n) = max(LCS(m-1, n), LCS(m, n-1)). (s1[m] != s2[n])
```

例：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/d48bdb2271acfb408147414de2332187.jpg)

代码实现：
```cpp
// 时间复杂度：O(len(s1)*len(s2)), 空间复杂度：O(len(s1)*len(s2))
class LCS{
private:
    vector<vector<int> > memo;

    // 求 s1[0...m] 和 s2[0...n] 的最长公共子序列的长度值
    int __LCS(const string &s1, const string &s2, int m, int n){
        if(m < 0 || n < 0)
            return 0;

        if(memo[m](n) != -1)
            return memo[m](n);

        int res = 0;
        if(s1[m] == s2[n])
            res = 1 + __LCS(s1, s2, m - 1, n - 1);
        else
            res = max(__LCS(s1, s2, m - 1, n),
                      __LCS(s1, s2, m, n - 1));
        memo[m](n) = res;
        return res;
    }

    // 通过 memo 反向求解 s1 和 s2 的最长公共子序列
    string __getLCS(const string &s1, const string &s2){

        int m = s1.size() - 1;
        int n = s2.size() - 1;

        string res = "";
        while(m >= 0 && n >= 0)
            if(s1[m] == s2[n]){
                res = s1[m] + res;
                m --;
                n --;
            }
            else if(m == 0)
                n --;
            else if(n == 0)
                m --;
            else{
                if(memo[m-1](n) > memo[m](n-1))
                    m --;
                else
                    n --;
            }

        return res;
    }
public:
    string getLCS(const string &s1, const string &s2){
        memo.clear();
        for(int i = 0 ; i < s1.size() ; i ++)
            memo.push_back(vector<int>(s2.size(), -1));

        __LCS(s1, s2, s1.size() - 1, s2.size() - 1);
        return __getLCS(s1, s2);
    }
};
```

```cpp
// 时间复杂度：O(len(s1)*len(s2)), 空间复杂度：O(len(s1)*len(s2))
class LCS{
public:
    string getLCS(const string &s1, const string &s2){
        int m = s1.size();
        int n = s2.size();

        // memo[i] [j] 表示 s1[0...i] 和 s2[0...j] 的最长公共子序列的长度
        vector<vector<int> > memo(m, vector<int>(n, 0));
				// 对 memo 的第 0 行和第 0 列进行初始化
        for(int j = 0 ; j < n ; j ++)
            if(s1[0] == s2[j])
                memo[0] [j] = 1;
        for(int i = 0 ; i < m ; i ++)
            if(s1[i] == s2[0])
                memo[i] [0] = 1;

        // 动态规划的过程
        for(int i = 1 ; i < m ; i ++)
            for(int j = 1 ; j < n ; j ++)
                if(s1[i] == s2[j])
                    memo[i] [j] = 1 + memo[i-1] [j-1];
                else
                    memo[i] [j] = max(memo[i-1] [j], memo[i] [j-1]);

        // 通过 memo 反向求解 s1 和 s2 的最长公共子序列的具体解
        m = s1.size() - 1;
        n = s2.size() - 1;
        string res = "";
        while(m >= 0 && n >= 0)
            if( s1[m] == s2[n] ){
                res = s1[m] + res;
                m --;
                n --;
            }
            else if(m == 0)
                n --;
            else if(n == 0)
                m --;
            else{
                if(memo[m-1](n) > memo[m] [n-1])
                    m --;
                else
                    n --;
            }
        return res;
    }
};
```

## 7. Refer Links
