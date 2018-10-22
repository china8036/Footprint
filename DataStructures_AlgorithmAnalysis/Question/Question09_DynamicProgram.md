- [Dynamic programming](#dynamic-programming)
  - [1. 斐波那契数列 Fibonacci polynomial](#1-斐波那契数列-fibonacci-polynomial)
    - [1.1. 爬楼梯问题 Climbing Stairs](#11-爬楼梯问题-climbing-stairs)
    - [1.2. 另一类爬楼梯问题](#12-另一类爬楼梯问题)
    - [1.3. 摆放球问题](#13-摆放球问题)
    - [1.4. Triangle](#14-triangle)
    - [1.5. Minimum Path Sum](#15-minimum-path-sum)
  - [2. 整数分解](#2-整数分解)
    - [2.1. Integer Break](#21-integer-break)
    - [2.2. Perfect Squares](#22-perfect-squares)
    - [2.3. Decode Ways](#23-decode-ways)
    - [2.4. 不同路径问题](#24-不同路径问题)
      - [2.4.1. Unique Paths](#241-unique-paths)
      - [2.4.2. Unique Paths II](#242-unique-paths-ii)
  - [3. 最小修改次数问题](#3-最小修改次数问题)
    - [3.1. Edit Distance](#31-edit-distance)
    - [3.2. Delete Operation for Two Strings](#32-delete-operation-for-two-strings)
    - [3.3. Minimum ASCII Delete Sum for Two Strings](#33-minimum-ascii-delete-sum-for-two-strings)
  - [4. 抢劫房子问题](#4-抢劫房子问题)
    - [4.1. House Robber](#41-house-robber)
    - [4.2. House Robber II](#42-house-robber-ii)
    - [4.3. House Robber III](#43-house-robber-iii)
    - [4.4. Best Time to Buy and Sell Stock with Cooldown](#44-best-time-to-buy-and-sell-stock-with-cooldown)
  - [5. 着色问题](#5-着色问题)
    - [5.1. 不同颜色球排列 Ⅰ -- 组合 DP](#51-不同颜色球排列-Ⅰ----组合-dp)
    - [5.2. 不同颜色球排列 Ⅱ -- 组合 DP](#52-不同颜色球排列-Ⅱ----组合-dp)
    - [5.3. 环形涂色 -- 环形 DP](#53-环形涂色----环形-dp)
  - [6. 背包问题](#6-背包问题)
    - [6.1. Partition Equal Subset Sum](#61-partition-equal-subset-sum)
    - [6.2. Coin Change](#62-coin-change)
    - [6.3. Combination Sum IV](#63-combination-sum-iv)
    - [6.4. Ones and Zeroes](#64-ones-and-zeroes)
    - [6.5. Word Break](#65-word-break)
    - [6.6. Target Sum](#66-target-sum)
  - [7. 最长上升子序列 （LIS 问题）](#7-最长上升子序列-lis-问题)
    - [7.1. Longest Increasing Subsequence](#71-longest-increasing-subsequence)
    - [7.2. Wiggle Subsequence](#72-wiggle-subsequence)
    - [7.3. 堆箱子问题 (stack of boxes)](#73-堆箱子问题-stack-of-boxes)
  - [8. LCSs 问题](#8-lcss-问题)
    - [8.1. 最长公共子序列问题](#81-最长公共子序列问题)
    - [8.2. 最长公共子串问题](#82-最长公共子串问题)
  - [9. 矩阵最大和](#9-矩阵最大和)
  - [10. Refer Links](#10-refer-links)

# Dynamic programming

## 1. 斐波那契数列 Fibonacci polynomial

斐波那契数列的递推

表达式为：

```
F(0)=1
F(1)=1
F(n)=F(n-1)+F(n-2)
```

其递归树如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/c5e3885ef8809db940b6625156894db4.jpg)

可见，其中包含了大量的重叠子问题，因此可以通过记忆化搜索（自顶向下，假设基本问题已经解决）和动态规划（自底向上，从基本问题递推到复杂问题）来解决。

- 记忆化搜索（自顶向下）
  ```cpp
  vector<int> memo = vector<int>(n + 1, -1);
  int fib(int n){
      if(n == 0)
          return 0;

      if(n == 1)
          return 1;

      return memo[n] == -1 ? fib(n - 1) + fib(n - 2) : memo[n];
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

    为了到第 n 阶台阶，存在以下可能的情况：
    - 从第 n-1 阶台阶爬 1 阶，到了第 n 阶
    - 从第 n-2 阶台阶爬 2 阶，到了第 n 阶
    因此，f(n) = f(n-1) + f(n-2).

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

### 1.2. 另一类爬楼梯问题

爬楼梯时可以一次爬 2 级或者 1 级，但是**不能连续爬 2 级**。问 n 级台阶有多少种可能的情况？

用 f(n) 表示第 n 级时的方案数量，为了到第 n 级台阶，存在以下可能的情况：
- 从第 n-1 级跳 1 级，到了第 n 级台阶。因此可能的方案有 f(n-1) 种。
- 从第 n-2 级跳 2 级，到了第 n 级台阶。由于不能连续跳 2 次 2 级，因此可以确定是从第 n-3 级台阶先跳了一次 1 级，再跳了一次 2 级，才到达第 n 级的。因此可能的方案有 f(n-3) 种。

因此，**f(n) = f(n-1) + f(n-3).**

### 1.3. 摆放球问题

有两种不同颜色的球，共 n 个，要求**不能有三个连续的相同颜色的排在一起**，问有几种排列方式。

用 f(n) 表示 n 个球时有几种方案：
- 如果第 n 个球与 n-1 个球的颜色一样，那么当前 n-1 的状态算已经确定下来了，对当前结果起作用的是 n-2 这个状态，需要根 n-2 颜色取的不一样。也就是 (2-1)f(n-2) 种（总共有两种颜色，还剩 1 种能取)。
- 如果第 n 个球与 n-1 个球颜色不一样，那么就需要看 n-1 有几种状态，然后和 n-1 取不一样的。也就是 (2-1)f(n-1) 种。

所以最后就是 f(n)=f(n-1)+f(n-2)，这么看很像斐波那契数列，上楼梯问题。只不过初始边界值不同。

当一共有 k 种颜色时，就是 **f(n)=(k-1)f(n-1)+(k-1)f(n-2)**。

### 1.4. Triangle

[120. Triangle](https://leetcode.com/problems/triangle/description/)

### 1.5. Minimum Path Sum

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

            if(memo[n] == -1) 
                for(int i = 1 ; i < n; i ++)
                    memo[n] = max3(memo[n], i * (n - i) , i * breakInteger(n - i));

            return memo[n];
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
            for (int i = 2 ; i <= n ; i ++)
                for (int j = 1; j <= i - 1; j ++) // 求解 memo[i]
                    // 分别表示：1. 已有最大值 2. 当前值不再分割 3. 当前值继续分割
                    memo[i] = max3(memo[i], j * (i - j), j * memo[i - j]);

            return memo[n];
        }
    };
    ```

### 2.2. Perfect Squares

[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

### 2.3. Decode Ways

[91. Decode Ways](https://leetcode.com/problems/decode-ways/description/)

### 2.4. 不同路径问题

#### 2.4.1. Unique Paths

[62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)

- Question
  > A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
  > 
  > The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
  > 
  > How many possible unique paths are there?

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/9/9/7d4028795f3c2aef02953bc1ae8a1f09.jpg)

- Solution

  按照题意，存在重叠子问题，因此可以使用 DP 解决。使用 way[i] [j] 表示走到 (i, j) 坐标时的不同路径数量，可得状态转移方程为：`ways[i] [j] = ways[i-1] [j] + ways[i] [j-1];`。
  ```java
  class Solution {
      public int uniquePaths(int m, int n) {
          if (m <= 0 || n <= 0) 
              return 0;
          int[][] ways = new int[m](n);
          for (int i = 0; i < m; i++) {			// 从第 1 列开始
              for (int j = 0; j < n; j++) {	// 从第 1 行开始
                  if (i == 0 || j == 0) 
                      ways[i](j) = 1;
                  else 
                      ways[i](j) = ways[i-1](j) + ways[i](j-1);
              }
          }
          return ways[m-1](n-1);
      }
  }
  ```
  进一步优化：**当走到 (i,j) 坐标时，由于 robot 只能往 2 个方向前进而不能后退，因此实际上要求得 way[i] [j]，只需要求得走到当前行有多少种路径**。因此，状态转移方程为：`res[j] = res[j] + res[j-1]`。
  ```java
  class Solution {
      public int uniquePaths(int m, int n) {
          if (m <= 0 || n <= 0)
              return 0;
          int[] res = new int[n];
          res[0] = 1;
          for (int i = 0; i < m; i++) 		// 从第 1 列开始
              for (int j = 1; j < n; j++) // 从第 2 行开始
                  res[j] += res[j - 1];

          return res[n - 1];
      }
  }
  ```	

#### 2.4.2. Unique Paths II

[63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/)

- Question
  > A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
  > 
  > The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
  > 
  > Now consider if some obstacles are added to the grids. How many unique paths would there be?

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/9/9/7d4028795f3c2aef02953bc1ae8a1f09.jpg)

- Solution

  这个题目是第 62 题的加强版本，在图中增加了障碍物，但同样可以通过 DP 求解。**如果遇到障碍物，这一列之前的所有路径都得归零**。TODO:
  ```java
  class Solution {
      public int uniquePathsWithObstacles(int[][] obstacleGrid) {
          int m = obstacleGrid.length;
          int n = obstacleGrid[0].length;
          int[] res = new int[n];
          res[0] = 1;
          for (int i = 0; i < m; i++) {
              for (int j = 0; j < n; j++) {
                  if (obstacleGrid[i](j) == 1) 
                      res[j] = 0;
                  else if (j > 0) 
                      res[j] += res[j - 1];
              }
          }
          return res[n - 1];
      }
  }
  ```

## 3. 最小修改次数问题

### 3.1. Edit Distance

[72. Edit Distance](https://leetcode.com/problems/edit-distance/description/)

- Question
  > Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.
  >
  > You have the following 3 operations permitted on a word:
  > 1. Insert a character
  > 2. Delete a character
  > 3. Replace a character

  Example 1:
  ```
  Input: word1 = "horse", word2 = "ros"
  Output: 3
  Explanation: 
  horse -> rorse (replace 'h' with 'r')
  rorse -> rose (remove 'r')
  rose -> ros (remove 'e')`
  ```
- Solution

  **用 f(i, j) 表示从以 word1[i] 结尾的子串变换到以 word2[j] 结尾的子串需要的操作数**，则状态转移方程：
  ```
  f(i, j) = f(i - 1, j - 1) (word1[i] != word2[j])
  f(i, j) = 1 + min {f(i, j - 1), f(i - 1, j), f(i - 1, j - 1)} (word1[i] == word2[j])
  ```
  Base Case:
  ```
  f(0, k) = f(k, 0) = k
  ```
  Code:
  ```java
  public int minDistance(String word1, String word2) {
      int m = word1.length();
      int n = word2.length();
      
      int [][] dp = new int[m + 1] [n + 1];
      dp[0] [0] = 0;											// word1 和 word2 都为空
      for (int i = 1; i <= n; i++) 
          dp[0] [i] = dp[0] [i - 1] + 1; 	// insert
      
      for(int i = 1; i <= m; i++) 
          dp[i] [0] = dp[i - 1] [0] + 1; 	// delete
      
      for (int i = 1; i <= m; i++) {
          for (int j = 1; j <= n; j++) {
              if (word1.charAt(i - 1) == word2.charAt(j - 1)) 
                  dp[i] [j] = dp[i - 1] [j - 1];
              else 
                  dp[i] [j] = Math.min(dp[i - 1] [j - 1], Math.min(dp[i - 1] [j], dp[i] [j - 1])) + 1;
          }
      }
      
      return dp[m] [n];
  }
  ```

### 3.2. Delete Operation for Two Strings

[583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/description/)

### 3.3. Minimum ASCII Delete Sum for Two Strings

[712. Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/description/)

## 4. 抢劫房子问题

### 4.1. House Robber

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
                  memo[i] = max(memo[i], nums[j] + (j + 2 < n ? memo[j + 2] : 0));

          return memo[0];
      }
  };
  ```

### 4.2. House Robber II

[213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

### 4.3. House Robber III

[Ho337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

### 4.4. Best Time to Buy and Sell Stock with Cooldown

[309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

## 5. 着色问题

### 5.1. 不同颜色球排列 Ⅰ -- 组合 DP

- Question
  
  ByteDance 2018.9.20 笔试第 4 题
  > 有 3 种颜色的球各 n、m、k 个，现将这些球排成一行，要求相邻的球颜色不能相同，问有几种排列方式。其中，`3 <= n,m,k <= 20`。

- Solution
  - 暴力回溯
    ```java
    public class Main {

      public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int m = in.nextInt();
        int k = in.nextInt();
        System.out.println(solution(n, m, k));
      }

      private static long res = 0L;

      private static long solution(int n, int m, int k) {
        __solution(n, m, k, -1);
        return res;
      }

      private static void  __solution(int n, int m, int k, int last) {
        if (n + m + k == 0) {
          res++;
          return;
        }
        for (int i = 0; i < 3; i++) {
          if (i == last)
            continue;
          if (i == 0 && n > 0)
            __solution(n-1, m, k, 0);
          else if (i == 1 && m > 0)
            __solution(n, m-1, k, 1);
          else if (i == 2 && k > 0)
            __solution(n, m, k-1, 2);
        }
      }

    }
    ```
  - 动态规划

    设 f(n,m,k,i) 表示剩余 n 个白色、m 个黑色、k 个红色，且最后一个球的颜色为 i（用 0、1、2 分别表示 3 种颜色）时方案的总数，则状态转移方程为：
    ```
    f(n,m,k,i) = 
      f(n+1,m,k,1) + f(n+1,m,k,2) (i=0);
      f(n,m+1,k,0) + f(n,m+1,k,2) (i=1);
      f(n,m,k+1,0) + f(n,m,k+1,1) (i=2);
    ```
    由于数组索引表示的是剩余的数量，因此在动态规划过程中应该使用递减顺序来赋值。

    Code:
    ```java
    public class Main {
      public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int m = in.nextInt();
        int k = in.nextInt();
        System.out.println(solution(n, m, k));
      }

      private static long solution(int n, int m, int o) {
        long [][][][] dp = new long[n+2](m+2)[o+2](3);
        for (int i = n; i <= n + 1; i++)
          for (int j = m; j <= m + 1; j++)
            for (int k = o; k <= o + 1; k++)
              for (int h = 0; h < 3; h++)
                dp[i](j)[k](h) = 0;
        dp[n - 1](m)[o](0) = 1;
        dp[n](m - 1)[o](1) = 1;
        dp[n](m)[o - 1](2) = 1;
        
        for (int i = n; i >= 0; i--) {
          for (int j = m; j >= 0; j--) {
            for (int k = o; k >= 0; k--) {
              dp[i](j)[k](0) += dp[i + 1](j)[k](1) + dp[i + 1](j)[k](2);
              dp[i](j)[k](1) += dp[i](j + 1)[k](0) + dp[i](j + 1)[k](2);
              dp[i](j)[k](2) += dp[i](j)[k + 1](0) + dp[i](j)[k + 1](1);
            }
          }
        }
        return dp[0](0)[0](0) + dp[0](0)[0](1) + dp[0](0)[0](2);
      }
    }
    ```

### 5.2. 不同颜色球排列 Ⅱ -- 组合 DP

https://blog.csdn.net/Vectorxj/article/details/51491754

- Question
  > 有 k 种颜色的球各 C1、C2、C3...Ck 个，现将这些球排成一行，要求相邻的球颜色不能相同，问有几种排列方式。其中，`1 <= k <= 15`, `1 <= Ci <= 5`。

- Solution

### 5.3. 环形涂色 -- 环形 DP

https://blog.csdn.net/update7/article/details/79770832

https://blog.csdn.net/Tc_To_Top/article/details/48934857

https://blog.csdn.net/fyy2603/article/details/54767836

## 6. 背包问题

对于 0-1 背包问题，需要用 2 个维度的参数进行状态定义：

> F(n, C) 表示将 n 个物品放进容量为 C 的背包的最大总价值。

则状态转移方程为：
```
F(i, c) = max(F(i-1, c), v(i) + F(i-1, c-w(i)))
```

代码实现：
```cpp
// 时间复杂度：O(n * C) 其中 n 为物品个数；C 为背包容积，空间复杂度：O(n * C)
class Knapsack01 {
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
            for(int j = 0 ; j <= C ; j ++) {
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

### 6.1. Partition Equal Subset Sum

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

### 6.2. Coin Change

[322. Coin Change](https://leetcode.com/problems/coin-change/description/)

### 6.3. Combination Sum IV

[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)

### 6.4. Ones and Zeroes

[474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)

### 6.5. Word Break

[139. Word Break](https://leetcode.com/problems/word-break/description/)

### 6.6. Target Sum

[494. Target Sum](https://leetcode.com/problems/target-sum/description/)

## 7. 最长上升子序列 （LIS 问题）

### 7.1. Longest Increasing Subsequence

[300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)

- Question
  > Given an unsorted array of integers, find the length of longest increasing subsequence.

  For example,
  > Given [10, 9, 2, 5, 3, 7, 101, 18], the longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

  Your algorithm should run in O(n^2) complexity.

  Follow up: Could you improve it to O(n log n) time complexity?
- Solution

  - 暴力解法

    **序列的每个元素都有 2 种选择：放入 LIS/ 不放入 LIS**，因此，若对序列的所有子序列进行判断，时间复杂度为 O((2^n)*n)。
  
  - 动态规划

    由于在该问题的解空间中存在重叠子问题和最优子结构，因此可以采用记忆化搜索 / 动态规划的方法进行求解。

    状态定义：**LIS(i) 表示以第 i 个数字为结尾的最长上升子序列的长度**。

    状态转移方程：**LIS(i) = max(1 + LIS(j) if nums[i] > nums[j]) (i > j).**

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

### 7.2. Wiggle Subsequence

[376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)

### 7.3. 堆箱子问题 (stack of boxes)

[《CC 150 9.10》](https://www.nowcoder.com/questionTerminal/daaec37090484f4587d0e8f5b612cda1) TODO:

- Question

  > 有一堆箱子，每个箱子宽为 wi，长为 di，高为 hi，现在需要将箱子都堆起来，而且为了使堆起来的箱子不倒，上面的箱子的宽度和长度必须小于下面的箱子。请实现一个方法，求出能堆出的最高的高度，这里的高度即堆起来的所有箱子的高度之和。
  > 
  > 给定三个 int 数组 w,l,h，分别表示每个箱子宽、长和高，同时给定箱子的数目 n。请返回能堆成的最高的高度。保证 n 小于等于 500。2

- Solution

  本题是一个利用动态规划求最大严格上升子序列的问题，由于要满足底盘长宽的限制，因此只需要找到满足长与宽闲置的一个最大上升子序列即可，然后求出最大高度。进行计算前，需要对初始数组进行排序（即三个传入参数），按单关键字（宽度或高度）排序即可。其中 f[n] 表示前 n 个箱子可以堆的最大高度。

  只要试着用每个箱子作为最底并搭出可能的最高高度，就能找出箱堆的最高高度。其次，怎么找出以某个箱子为底的最高高度呢？只要试着在第二层以不同的箱子为底继续堆箱子即可，如此反复。
  ```java
  public int getHeight(int[] w, int[] l, int[] h, int n) {
      // 从大到小排序
      Arrays.sort(w);
      Arrays.sort(l);
      Arrays.sort(h);

      int [] maxH = new int[n];
      // 从第 1 个开始为 h[0]
      maxH[0] = h[0];
      int res = maxH[0];
      for (int i = 1; i < n; i++) {
          // 初始 maxH[i] = h[i]
          maxH[i] = h[i];
          // tmax 记录放入第 i-1 个箱子时可达到的最大高度
          int tmax = 0;
          for (int j = i - 1; j >= 0; j--)
              if (w[j] > w[i] && l[j] > l[i])
                  tmax = Math.max(tmax, maxH[j]);
          maxH[i] += tmax;
          res = Math.max(res, maxH[i]);
      }
      return res;
  }
  ```

## 8. LCSs 问题

**LCSs 问题实际上是指最长公共子序列问题（Longest-Common-Subsequence）和最长公共子串（Longest-Common-Substring）问题**，这两个问题非常的相似，区别在于：
- **子序列是有序的，但不一定是连续，作用对象是序列**。例如：序列 X = <B, C, D, B> 是序列 Y = <A, B, C, B, D, A, B> 的子序列，对应的下标序列为 <2, 3, 5, 7>。
- **子串是有序且连续的，左右对象是字符串**。例如：a = abcd 是 c = aaabcdddd 的一个子串；但是 b = acdddd 就不是 c 的子串。

两者问题很相似，均可以使用动态规划的思想进行求解。

### 8.1. 最长公共子序列问题

> 给出 s1 和 s2，求出它们的最长公共子序列的长度。

该问题被广泛的应用在各个领域，如在基因工程中，可通过计算两个基因间的 LCS 长度，求出它们的相似程度。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/23/a3eab068b81da598f8f918123dcca890.jpg)

LCS 问题需要使用 2 个维度的参数进行状态定义：

> LCS(m, n) 表示 s1[0...m] 和 s2[0...n] 的最长公共子序列的长度。

状态转移方程：
```
LCS(m, n) = 1 + LCS(m-1, n-1). 				(s1[m] == s2[n])
       = max(LCS(m-1, n), LCS(m, n-1)). 	(s1[m] != s2[n])
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

### 8.2. 最长公共子串问题

## 9. 矩阵最大和

TODO:

《CC 150》 18.12

- Question
  > 给定一个正数和负数组成的 N * N 矩阵，编写程序找出元素总和最大的子矩阵。

- Solution
  - 暴力，时间复杂度为O(N^6)
  - 动态规划，时间复杂度为O(N^4)
  - 进一步优化，时间复杂度为O(N^3)

## 10. Refer Links

[LCSs——最长公共子序列和最长公共子串](https://www.cnblogs.com/maybe2030/p/5469877.html)

[动态规划的状态确定 -“不能连续跳两级的青蛙跳”和“栅栏涂色”](https://www.jianshu.com/p/e5dd3cdf158a?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

[如何计算排列方法数，其中相同元素不能相邻？](https://www.zhihu.com/question/29890888)