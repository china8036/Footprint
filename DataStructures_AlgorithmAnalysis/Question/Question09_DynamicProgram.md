- [Dynamic programming](#dynamic-programming)
  - [1. 一维 DP](#1-一维-dp)
    - [1.1. 斐波那契数列 Fibonacci polynomial](#11-斐波那契数列-fibonacci-polynomial)
      - [1.1.1. 爬楼梯问题 Climbing Stairs](#111-爬楼梯问题-climbing-stairs)
      - [1.1.1. 另一类爬楼梯问题](#111-另一类爬楼梯问题)
      - [1.1.2. 摆放球问题](#112-摆放球问题)
      - [1.1.3. Triangle](#113-triangle)
      - [1.1.4. Minimum Path Sum](#114-minimum-path-sum)
    - [1.2. 整数分解](#12-整数分解)
      - [1.2.1. Integer Break](#121-integer-break)
      - [1.2.2. Perfect Squares](#122-perfect-squares)
      - [1.2.3. Decode Ways](#123-decode-ways)
    - [1.3. 抢劫房子问题](#13-抢劫房子问题)
      - [1.3.1. House Robber](#131-house-robber)
      - [1.3.2. House Robber II](#132-house-robber-ii)
      - [1.3.3. House Robber III](#133-house-robber-iii)
      - [1.3.4. Best Time to Buy and Sell Stock with Cooldown](#134-best-time-to-buy-and-sell-stock-with-cooldown)
    - [1.4. 最长上升子序列 （LIS 问题）](#14-最长上升子序列-lis-问题)
      - [1.4.1. Longest Increasing Subsequence](#141-longest-increasing-subsequence)
      - [1.4.2. Wiggle Subsequence](#142-wiggle-subsequence)
      - [1.4.3. 区间最大和](#143-区间最大和)
      - [1.4.4. 堆箱子问题 (stack of boxes)](#144-堆箱子问题-stack-of-boxes)
    - [1.5. 跳房子](#15-跳房子)
  - [2. 二维 DP](#2-二维-dp)
    - [2.1. 最小修改次数问题](#21-最小修改次数问题)
      - [2.1.1. Edit Distance](#211-edit-distance)
      - [2.1.2. Delete Operation for Two Strings](#212-delete-operation-for-two-strings)
      - [2.1.3. Minimum ASCII Delete Sum for Two Strings](#213-minimum-ascii-delete-sum-for-two-strings)
    - [2.2. 背包问题](#22-背包问题)
      - [2.2.1. Partition Equal Subset Sum](#221-partition-equal-subset-sum)
      - [2.2.2. Coin Change](#222-coin-change)
      - [2.2.3. Combination Sum IV](#223-combination-sum-iv)
      - [2.2.4. Ones and Zeroes](#224-ones-and-zeroes)
      - [2.2.5. Word Break](#225-word-break)
      - [2.2.6. Target Sum](#226-target-sum)
    - [2.3. LCSs 问题](#23-lcss-问题)
      - [2.3.1. 最长公共子序列问题](#231-最长公共子序列问题)
      - [2.3.2. 最长公共子串问题](#232-最长公共子串问题)
    - [2.4. 矩阵系列问题](#24-矩阵系列问题)
      - [2.4.1. 矩阵内的不同路径](#241-矩阵内的不同路径)
        - [2.4.1.1. Unique Paths](#2411-unique-paths)
        - [2.4.1.2. Unique Paths II](#2412-unique-paths-ii)
      - [2.4.2. 直方图内的最大覆盖矩阵](#242-直方图内的最大覆盖矩阵)
      - [2.4.3. 矩阵内的最大全一长方形](#243-矩阵内的最大全一长方形)
      - [2.4.4. 矩阵内的最大全一正方形](#244-矩阵内的最大全一正方形)
      - [2.4.5. 矩阵内的最大加号](#245-矩阵内的最大加号)
      - [2.4.6. 矩阵内的最大和](#246-矩阵内的最大和)
    - [2.5. 字母交换](#25-字母交换)
  - [3. 多维 DP](#3-多维-dp)
    - [3.1. 着色问题](#31-着色问题)
      - [3.1.1. 不同颜色球排列 Ⅰ -- 组合 DP](#311-不同颜色球排列-Ⅰ----组合-dp)
      - [3.1.2. 不同颜色球排列 Ⅱ -- 组合 DP](#312-不同颜色球排列-Ⅱ----组合-dp)
      - [3.1.3. 环形涂色 -- 环形 DP](#313-环形涂色----环形-dp)
  - [4. Refer Links](#4-refer-links)

# Dynamic programming

## 1. 一维 DP

### 1.1. 斐波那契数列 Fibonacci polynomial

斐波那契数列的递推

表达式为：

```
F(0)=1
F(1)=1
F(n)=F(n-1)+F(n-2)
```

其递归树如下：

![image](http://img.cdn.firejq.com/jpg/2018/4/23/c5e3885ef8809db940b6625156894db4.jpg)

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

#### 1.1.1. 爬楼梯问题 Climbing Stairs

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

    ![image](http://img.cdn.firejq.com/jpg/2018/4/23/326b837d0a0d9848714011b5f0a6f518.jpg)

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

#### 1.1.1. 另一类爬楼梯问题

爬楼梯时可以一次爬 2 级或者 1 级，但是**不能连续爬 2 级**。问 n 级台阶有多少种可能的情况？

用 f(n) 表示第 n 级时的方案数量，为了到第 n 级台阶，存在以下可能的情况：
- 从第 n-1 级跳 1 级，到了第 n 级台阶。因此可能的方案有 f(n-1) 种。
- 从第 n-2 级跳 2 级，到了第 n 级台阶。由于不能连续跳 2 次 2 级，因此可以确定是从第 n-3 级台阶先跳了一次 1 级，再跳了一次 2 级，才到达第 n 级的。因此可能的方案有 f(n-3) 种。

#### 1.1.2. 摆放球问题

有两种不同颜色的球，共 n 个，要求**不能有三个连续的相同颜色的排在一起**，问有几种排列方式。

用 f(n) 表示 n 个球时有几种方案：
- 如果第 n 个球与 n-1 个球的颜色一样，那么当前 n-1 的状态算已经确定下来了，对当前结果起作用的是 n-2 这个状态，需要根 n-2 颜色取的不一样。也就是 (2-1)f(n-2) 种（总共有两种颜色，还剩 1 种能取)。
- 如果第 n 个球与 n-1 个球颜色不一样，那么就需要看 n-1 有几种状态，然后和 n-1 取不一样的。也就是 (2-1)f(n-1) 种。

所以最后就是 **f(n)=f(n-1)+f(n-2)**，这么看很像斐波那契数列，上楼梯问题。只不过初始边界值不同。

#### 1.1.3. Triangle

#### 1.1.4. Minimum Path Sum

### 1.2. 整数分解

#### 1.2.1. Integer Break

[343. Integer Break](https://leetcode.com/problems/integer-break/description/)

- Question
  > Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.
  >
  > For example, given n = 2, return 1 (2 = 1 + 1); given n = 10, return 36 (10 = 3 + 3 + 4).

  Note: You may assume that n is not less than 2 and not larger than 58.

- Solution
  - 自顶向下

    ![image](http://img.cdn.firejq.com/jpg/2018/4/23/928da10fa0f8b2ca0cd4a012c8703e83.jpg)

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
    ```java
    class Solution {
        public int integerBreak(int n) {
            if(n < 1)
                throw new IllegalArgumentException("n should be greater than zero");
            int [] memo = new int[n+1];
            memo[1] = 1;
            for(int i = 2; i <= n; i ++)
                for(int j = 1; j <= i - 1; j ++)
                    memo[i] = max3(memo[i], j * (i - j), j * memo[i - j]);

            return memo[n];
        }

        private int max3(int a, int b, int c){
            return Math.max(a, Math.max(b, c));
    }
    ```

#### 1.2.2. Perfect Squares

[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

- Question
  > Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

  Example 1:
  ```
  Input: n = 12
  Output: 3
  Explanation: 12 = 4 + 4 + 4.
  ```
  Example 2:
  ```
  Input: n = 13
  Output: 2
  Explanation: 13 = 4 + 9.
  ```

- Solution
  - 解法 1：转换为图的最短路径 BFS 问题
    ```java
    class Solution {
        private class Node {
            int num;
            int step;
            Node(int num, int step) {
                this.num = num;
                this.step = step;
            }
        }
        public int numSquares(int n) {
            if(n == 0)
                return 0;
            ArrayDeque<Node> queue = new ArrayDeque<>();
            queue.add(new Node(n, 0));

            boolean[] visited = new boolean[n+1];
            visited[n] = true;

            while(!queue.isEmpty()){
                Node front = queue.remove();
                for(int i = 1; ; i ++) {
                    int a = front.num - i*i;
                    if (a < 0)
                        break;
                    if(a == 0)
                        return front.step + 1;
                    if(!visited[a]) {
                        queue.add(new Node(front.num - i * i, front.step + 1));
                        visited[front.num - i * i] = true;
                    }
                }
            }
            throw new IllegalStateException("No Solution.");
        }
    }
    ```
  - 解法 2：动态规划 `dp[n] = Min{ dp[n - i*i] + 1 },  n - i*i >=0 && i >= 1`
    ```java
    public int numSquares(int n) {
      int[] dp = new int[n + 1];
      // Arrays.fill(dp, Integer.MAX_VALUE);
      dp[0] = 0;
      for (int i = 1; i <= n; i++) {
        int min = Integer.MAX_VALUE;
        for (int j = 1; i - j*j >= 0; j++)
          min = Math.min(min, dp[i - j*j] + 1);
        dp[i] = min;
      }
    }
    ```

#### 1.2.3. Decode Ways

[91. Decode Ways](https://leetcode.com/problems/decode-ways/description/)

### 1.3. 抢劫房子问题

#### 1.3.1. House Robber

[198. House Robber](https://leetcode.com/problems/house-robber/description/)

- Question
  > You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
  >
  > Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.
- Solution

- 暴力解法

  检查所有房子的组合，对每一个组合，检查是否有相邻的房子，如果没有，记录其价值。最后找最大值即可。时间复杂度为 O((2^n)*n)。

- 动态规划

  ![image](http://img.cdn.firejq.com/jpg/2018/4/23/24c86a15cf1e053e9658340ff06ae592.jpg)

  ![image](http://img.cdn.firejq.com/jpg/2018/4/23/071ec2923b07b87330f52a8b2611baad.jpg)

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

  ```java
  class Solution {
      public int rob(int[] nums) {
          int n = nums.length;
          if (n == 0)
              return 0;
          int [] memo = new int[n];
          memo[n-1] = nums[n-1];
          for (int i = n - 2; i >= 0; i--)
              for (int j = i; j < n; j++)
                  memo[i] = Math.max(memo[i], nums[j] + (j + 2 < n ? memo[j + 2] : 0));
          return memo[0];
      }
  }
  ```

#### 1.3.2. House Robber II

[213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

#### 1.3.3. House Robber III

[Ho337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

#### 1.3.4. Best Time to Buy and Sell Stock with Cooldown

[309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

### 1.4. 最长上升子序列 （LIS 问题）

#### 1.4.1. Longest Increasing Subsequence

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

    ![image](http://img.cdn.firejq.com/jpg/2018/4/23/67326c8c913d5ec4fe36c12a85e6028d.jpg)

    代码实现：
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

    ```java
    class Solution {
        public int lengthOfLIS(int[] nums) {
            int n = nums.length;
            if (n == 0)
                return 0;
            int [] memo = new int[n];
            Arrays.fill(memo, 1);
            for (int i = 1; i < n; i++)
                for (int j = 0; j < i; j++)
                    if (nums[j] < nums[i])
                        memo[i] = Math.max(memo[i], 1 + memo[j]);

            int res = memo[0];
            for (int i = 1; i < n; i++)
                res = Math.max(res, memo[i]);
            return res;
        }
    }
    ```

#### 1.4.2. Wiggle Subsequence

[376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)

#### 1.4.3. 区间最大和

TODO:

- Question

  定义最大连续区间和：给定一个长度为n的序列a[1],a[2]...a[n]，求一个连续的子序列a[i],a[i+1]...a[j-1],a[j]使得a[i]+a[i+1]...+a[j-1]+a[j]最大。

- Solution

  定义maxn[i]为以i为结尾的最大连续和，则很容易找到递推关系：maxn[i] = max{0,max[i-1] + a[i]} 

  所以只要扫描一遍即可，总时间复杂度为O(n)。
  ```java
  for (int i =1; i <= n; i++) {
    last = max(0,maxn[i-1]) + a[i];

    ans = max(ans,last);

  } 
  ```

#### 1.4.4. 堆箱子问题 (stack of boxes)

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

### 1.5. 跳房子

[跳房子](https://jxy370.com/index.php/2018/08/27/room/)

- Question

  存在 n+1 个房间，每个房间依次为房间 1 2 3…i，每个房间都存在一个传送门，i 房间的传送门可以把人传送到房间 pi(1<=pi<=i), 现在路人甲从房间 1 开始出发（当前房间 1 即第一次访问)，每次移动他有两种移动策略：
  - A. 如果访问过当前房间 i 偶数次，那么下一次移动到房间 i+1；
  - B. 如果访问过当前房间 i 奇数次，那么移动到房间 pi；
  现在路人甲想知道移动到房间 n+1 一共需要多少次移动？

  eg:
  ```
  输入例子 1:
  2
  1 2

  输出例子 1:
  4
  ```

- Solution

  令`dp[i]`代表第二次到达房间 i 经过的步骤。首先考虑第一次到达第`i`个房间，则一定是经过了两次房间`i – 1`，即步数为`dp[i – 1] + 1`；

  同时，由于这次是第一次到达房间 i，接下来会跳到 pi，此时的步数更新为`dp[i – 1] + 2`。要想再次到达房间 i，则一定要再两次经过房间`i – 1`。而此时前`i – 1`个房间的状态为：

  除了第 pi 个房间经过了奇数次，其他房间都是偶数次，与第一次到达第 pi 个房间时的状态一致。而第一次到达 pi 时经过的步数为`dp[pi – 1] + 1`;

  此时要再次到达第 i 个房间，必须再次两次经过 i – 1，从此时的状态到两次经过 i – 1 的状态所需的步数，等于从初始状态到第二次到达房间 i – 1 时的状态所需要的步数减去从初始状态到第一次到达房间 pi 的状态所需要的步数，即`dp[i – 1] – (dp[pi – 1] + 1)`，再走一步，第二次到达房间 i。

  综上，第二次到达房间 i 的步数的状态转移方程为为：`dp[i] = dp[i – 1] + 1 + 1 + dp[i – 1] – (dp[pi – 1] + 1) +1 = dp[i – 1] * 2 – dp[pi – 1] + 2`.

  ```java
  public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      long mod = 1000000007;
      int n = sc.nextInt();
      int [] next = new int[n + 1];
      for(int i = 1; i <= n; i++)
          next[i] = sc.nextInt();

      long [] dp = new long[n + 1];
      if (n == 1)
          System.out.println(1);
      else
          for(int i = 1; i <= n; i++)
              dp[i] = ((dp[i - 1] * 2) % mod - dp[next[i] - 1] + 2) % mod;

      System.out.println((dp[n]) % 1000000007);
  }
  ```

## 2. 二维 DP

### 2.1. 最小修改次数问题

#### 2.1.1. Edit Distance

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

  **用 f(i, j) 表示从以 `word1[i]` 结尾的子串变换到以 `word2[j]` 结尾的子串需要的操作数**，则状态转移方程：
  ```
  f(i, j) = f(i - 1, j - 1) (word1[i] == word2[j])
  f(i, j) = 1 + min {f(i, j - 1), f(i - 1, j), f(i - 1, j - 1)} (word1[i] != word2[j])
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

      int [][] dp = new int[m + 1][n + 1];
      dp[0][0] = 0;											// word1 和 word2 都为空
      for (int i = 1; i <= n; i++)
          dp[0][i] = dp[0][i - 1] + 1; 	// insert

      for(int i = 1; i <= m; i++)
          dp[i][0] = dp[i - 1][0] + 1; 	// delete

      for (int i = 1; i <= m; i++) {
          for (int j = 1; j <= n; j++) {
              if (word1.charAt(i - 1) == word2.charAt(j - 1))
                  dp[i][j] = dp[i - 1][j - 1];
              else
                  dp[i][j] = Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
          }
      }

      return dp[m][n];
  }
  ```

#### 2.1.2. Delete Operation for Two Strings

[583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/description/)

#### 2.1.3. Minimum ASCII Delete Sum for Two Strings

[712. Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/description/)

### 2.2. 背包问题

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

#### 2.2.1. Partition Equal Subset Sum

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

#### 2.2.2. Coin Change

[322. Coin Change](https://leetcode.com/problems/coin-change/description/)

#### 2.2.3. Combination Sum IV

[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)

#### 2.2.4. Ones and Zeroes

[474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)

#### 2.2.5. Word Break

[139. Word Break](https://leetcode.com/problems/word-break/description/)

#### 2.2.6. Target Sum

[494. Target Sum](https://leetcode.com/problems/target-sum/description/)

### 2.3. LCSs 问题

**LCSs 问题实际上是指最长公共子序列问题（Longest-Common-Subsequence）和最长公共子串（Longest-Common-Substring）问题**，这两个问题非常的相似，区别在于：
- **子序列是有序的，但不一定是连续，作用对象是序列**。例如：序列 X = <B, C, D, B> 是序列 Y = <A, B, C, B, D, A, B> 的子序列，对应的下标序列为 <2, 3, 5, 7>。
- **子串是有序且连续的，左右对象是字符串**。例如：a = abcd 是 c = aaabcdddd 的一个子串；但是 b = acdddd 就不是 c 的子串。

两者问题很相似，均可以使用动态规划的思想进行求解。

#### 2.3.1. 最长公共子序列问题

> 给出 s1 和 s2，求出它们的最长公共子序列的长度。

该问题被广泛的应用在各个领域，如在基因工程中，可通过计算两个基因间的 LCS 长度，求出它们的相似程度。

![image](http://img.cdn.firejq.com/jpg/2018/4/23/a3eab068b81da598f8f918123dcca890.jpg)

LCS 问题需要使用 2 个维度的参数进行状态定义：

> LCS(m, n) 表示 s1[0...m] 和 s2[0...n] 的最长公共子序列的长度。

状态转移方程：
```
LCS(m, n) = 1 + LCS(m-1, n-1). 				      (s1[m] == s2[n])
          = max(LCS(m-1, n), LCS(m, n-1)). 	(s1[m] != s2[n])
```

例：

![image](http://img.cdn.firejq.com/jpg/2018/4/23/d48bdb2271acfb408147414de2332187.jpg)

代码实现：
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
                    memo[i][j] = 1 + memo[i-1][j-1];
                else
                    memo[i][j] = max(memo[i-1][j], memo[i][j-1]);

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
                if(memo[m-1][n] > memo[m][n-1])
                    m --;
                else
                    n --;
            }
        return res;
    }
};
```

#### 2.3.2. 最长公共子串问题

### 2.4. 矩阵系列问题

#### 2.4.1. 矩阵内的不同路径

##### 2.4.1.1. Unique Paths

[62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)

- Question
  > A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
  >
  > The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
  >
  > How many possible unique paths are there?

  ![image](http://img.cdn.firejq.com/jpg/2018/9/9/7d4028795f3c2aef02953bc1ae8a1f09.jpg)

- Solution

  按照题意，存在重叠子问题，因此可以使用 DP 解决。使用 `way[i][j]` 表示走到 (i, j) 坐标时的不同路径数量，可得状态转移方程为：`ways[i][j] = ways[i-1][j] + ways[i][j-1]`。
  ```java
  class Solution {
      public int uniquePaths(int m, int n) {
          if (m <= 0 || n <= 0)
              return 0;
          int[][] ways = new int[m][n];
          for (int i = 0; i < m; i++) {			// 从第 1 列开始
              for (int j = 0; j < n; j++) {	// 从第 1 行开始
                  if (i == 0 || j == 0)
                      ways[i][j] = 1;
                  else
                      ways[i][j] = ways[i-1][j] + ways[i][j-1];
              }
          }
          return ways[m-1][n-1];
      }
  }
  ```
  进一步优化：**当走到 (i,j) 坐标时，由于 robot 只能往 2 个方向前进而不能后退，因此实际上要求得 `way[i][j]`，只需要求得走到当前行有多少种路径**。因此，状态转移方程为：`res[j] = res[j] + res[j-1]`。
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

##### 2.4.1.2. Unique Paths II

[63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/description/)

- Question
  > A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
  >
  > The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
  >
  > Now consider if some obstacles are added to the grids. How many unique paths would there be?

  ![image](http://img.cdn.firejq.com/jpg/2018/9/9/7d4028795f3c2aef02953bc1ae8a1f09.jpg)

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
                  if (obstacleGrid[i][j] == 1)
                      res[j] = 0;
                  else if (j > 0)
                      res[j] += res[j - 1];
              }
          }
          return res[n - 1];
      }
  }
  ```

#### 2.4.2. 直方图内的最大覆盖矩阵

[POJ 2559 Largest Rectangle in a Histogram](http://poj.org/problem?id=2559)

[Leetcode 84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

- Question
  > Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

  ![image](http://img.cdn.firejq.com/jpg/2018/10/30/70c17b9cb3bf90d4adf1485b48cc6af5.jpg)

- Solution

  如果我们能知道一个矩形条向左向右最远能延伸多远，我们就能计算出以该矩形条为高的矩形面积了。因此，以此为基本思路，有以下几种解法。

  - 解法 1，时间复杂度为 O(n^2)

    直接**暴力求解**：对于每一个柱形图，只要向左向右去遍历，然后找到左边第一个小于他的点和右边第一个小于他的点，就可以得到宽度，然后再乘上它的高，就可以得到当前的矩形面积。从左到右依次遍历并且更新结果，最后就可以求得最大的矩形面积。

  - 解法 2，时间复杂度为 O(n)

    使用**辅助栈**求解：依次遍历所有矩形条，尝试计算以该矩形条为高度的矩形面积。但是在遍历的时候我们不知道后面还有什么样的矩形条怎么办？没关系，对于没法确定面积的矩形，压栈，留着以后处理，而对于那些已经可以确定计算出面积的矩形条，留着也没用，弹栈。

    Stack 中总是保持递增的元素的索引，然后当遇到较小的元素后，依次出栈并计算栈中 bar 能围成的面积，直到栈中元素小于当前元素。
    ```java
    public int largestRectangleArea(int[] height) {
        int len = height.length;
        ArrayDeque<Integer> stack = new ArrayDeque<>();
        int maxArea = 0;
        for (int i = 0; i <= len; i++) {
            int h = (i == len ? 0 : height[i]);
            if (stack.isEmpty() || h >= height[stack.peek()])
                stack.push(i);
            else {
                int tp = stack.pop();
                maxArea = Math.max(maxArea, height[tp] * (stack.isEmpty() ? i : i - 1 - stack.peek()));
                i--;
            }
        }
        return maxArea;
    }
    ```

  - 解法 3，时间复杂度为 O(n)

    在解法 1 的基础上，使用**动态规划**的思想（空间换时间），对 height 数组进行 3 次遍历：
    1. 从左向右遍历，在 left[i] 中记录 height[i] 能向左延伸的最大距离，即找到从 i 向左一直递增的最后一个位置 j，使 left[i] = j。
    1. 从右向左遍历，在 right[i] 中记录 height[i] 能向右延伸的最大距离，即找到从 i 向右一直递增的最后一个位置 j，使 right[i] = j。
    1. 从左向右遍历，依次计算以 height[i] 为矩形高度时能达到的最大面积。
    ```java
    public int largestRectangleArea(int[] heights) {
        if (heights == null || heights.length == 0)
            return 0;

        int max = 0;
        int n = heights.length;
        int [] left = new int[heights.length];
        int [] right = new int[heights.length];

        left[0] = -1;
        for (int i = 1; i < n; i++) {
            int j = i - 1;
            while (j >= 0 && heights[j] >= heights[i])
                j = left[j]; // 直接使用已有结果
            left[i] = j;
        }

        right[n - 1] = n;
        for (int i = n - 2; i >= 0; i--) {
            int j = i + 1;
            while (j < n && heights[j] >= heights[i])
                j = right[j]; // 直接使用已有结果
            right[i] = j;
        }

        for (int i = 0; i < n; i++)
            max = Math.max(heights[i] * (right[i] - left[i] - 1), max);

        return max;
    }
    ```

#### 2.4.3. 矩阵内的最大全一长方形

[85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/)

- Question

  Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

  Example:
  ```
  Input:
  [
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
  ]
  Output: 6
  ```

- Solution
  - 暴力：从左上角第一个点进行遍历，对每一个点都枚举以该点为左上角的所有可能的（全为一的）子矩阵，并在此过程中不断更新最大面积即可。但时间复杂度达到了 O(n^2 * m^2 * min(n,m)) = O(n^5).
  - DP：在暴力解法中，我们每碰到一个高度不为 0 的情况就往左搜索找到当前最低的高度然后更新面积，所以会导致大量的重复计算。

    时间复杂度为 O(n^3).
    ```java
    public int maximalRectangle(char[][] matrix) {
      if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
        return 0;
      // number of consecutive 1s to the left of matrix[i][j], including itself
      int [][] dp = new int[matrix.length][matrix[0].length];

      int area = 0;
      for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
          if (matrix[i][j] == '1') {
            dp[i][j] = j == 0 ? 1 : dp[i][j-1] + 1;
            int x = matrix[0].length; // width
            int y = 1; // height
            while (i - y + 1 >= 0 && matrix[i-y+1][j] == '1') {
              x = Math.min(x, dp[i-y+1][j]);
              area = Math.max(area, x*y);
              y++;
            }
          }
        }
      }

      return area;
    }
    ```
  - 辅助栈：实际上可以把这道题看作是 [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) 的扩展，**对每一行，把它上面的所有行往下做投影**，即依次把每一行都当成一个柱形图的底，然后就可以直接采用 85 题的思路了。

#### 2.4.4. 矩阵内的最大全一正方形

[221. Maximal Square](https://leetcode.com/problems/maximal-square/)

[花花酱 LeetCode 221. Maximal Square - 刷题找工作 EP62](https://www.youtube.com/watch?v=vkFUB--OYy0)

- Question

  Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

  Example:
  ```
  Input:
  1 0 1 0 0
  1 0 1 1 1
  1 1 1 1 1
  1 0 0 1 0
  Output: 4
  ```

- Solution

  此题实际上是 [85. Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/) 的弱化版本，区别在于 85 题是长方形而 221 题是正方形。**由于是正方形，因此可在通用解法的基础上进行进一步优化**。

  使用 DP 求解，用 f(x,y) 表示以点 (x,y) 为右下角的最大正方形的**边长**，则可得到递推公式如下：
  ```
  f(x,y) = min(f(x-1, y), f(x, y-1), f(x-1, y-1)) + 1, if m[x][y] == 1
  f(x,y) = 0,                                          if m[x][y] == 0
  ```

  ![image](http://img.cdn.firejq.com/jpg/2018/10/31/e19472155c64e3c420e166b3c0aa62a7.jpg)

  ![image](http://img.cdn.firejq.com/jpg/2018/10/31/c2fa67e1199f646a74f5d59c27651bdc.jpg)

  **时间复杂度为 O(n^2)**.
  ```java
  public static int maximalSquare(char[][] matrix) {
      if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
          return 0;
      int maxSize = 0;
      int [][] dp = new int[matrix.length][matrix[0].length]; // default all zero

      for (int i = 0; i < matrix.length; i++) // init first row
          if (matrix[i][0] == '1') {
              dp[i][0] = 1;
              maxSize = 1;
          }
      for (int i = 0; i < matrix[0].length; i++) // init first column
          if (matrix[0][i] == '1') {
              dp[0][i] = 1;
              maxSize = 1;
          }

      for (int i = 1; i < matrix.length; i++) {
          for (int j = 1; j < matrix[0].length; j++) {
              if (matrix[i][j] == '1') {
                  dp[i][j] = Math.min(dp[i-1][j], Math.min(dp[i][j-1], dp[i-1][j-1])) + 1;
                  maxSize = Math.max(maxSize, dp[i][j]);
              }
          }
      }

      return maxSize * maxSize;
  }
  ```
  对初始化代码进行优化，更加优雅：
  ```java
  public static int maximalSquare(char[][] matrix) {
      if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
          return 0;
      int maxSize = 0;
      int [][] dp = new int[matrix.length][matrix[0].length]; // default all zero

      for (int i = 0; i < matrix.length; i++) {
          for (int j = 0; j < matrix[0].length; j++) {
              if (matrix[i][j] == '1') {
                  dp[i][j] = 1;
                  if (i > 0 && j > 0)
                      dp[i][j] += Math.min(dp[i-1][j], Math.min(dp[i][j-1], dp[i-1][j-1]));
                  maxSize = Math.max(maxSize, dp[i][j]);
              }
          }
      }

      return maxSize * maxSize;
  }
  ```
  从上面解法二可以看出，实际上 `dp[i][j]` 只与 `dp[i-1][j]`, `dp[i][j-1]`, `dp[i-1][j-1]` 有关，即只与当前一列和上一列有关，因此可以进一步优化空间，即 `int [][] dp = new int[2][matrix[0].length];` 只使用含有两个矩阵宽度长的向量来保存数据即可。这样时间复杂度是 O(M*N)，空间复杂度是 O(N)。
  ```java
  public static int maximalSquare(char[][] matrix) {
      if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
          return 0;
      int maxSize = 0;
      int [][] dp = new int[2][matrix[0].length]; // default all zero

      for (int i = 0; i < matrix.length; i++) {
          for (int j = 0; j < matrix[0].length; j++) {
              dp[i%2][j] = 0;
              if (matrix[i][j] == '1') {
                  dp[i%2][j] = 1;
                  if (i > 0 && j > 0)
                      dp[i%2][j] += Math.min(dp[(i-1)%2][j], Math.min(dp[i%2][j-1], dp[(i-1)%2][j-1]));
                  maxSize = Math.max(maxSize, dp[i%2][j]);
              }
          }
      }

      return maxSize * maxSize;
  }
  ```

#### 2.4.5. 矩阵内的最大加号

[764. Largest Plus Sign](https://leetcode.com/problems/largest-plus-sign/)

- Question

  In a 2D grid from (0, 0) to (N-1, N-1), every cell contains a 1, except those cells in the given list mines which are 0. What is the largest axis-aligned plus sign of 1s contained in the grid? Return the order of the plus sign. If there is none, return 0.

  An "axis-aligned plus sign of 1s of order k" has some center grid[x](y) = 1 along with 4 arms of length k-1 going up, down, left, and right, and made of 1s. This is demonstrated in the diagrams below. Note that there could be 0s or 1s beyond the arms of the plus sign, only the relevant area of the plus sign is checked for 1s.

  Examples of Axis-Aligned Plus Signs of Order k:
  ```
  Order 1:
  000
  010
  000

  Order 2:
  00000
  00100
  01110
  00100
  00000

  Order 3:
  0000000
  0001000
  0111110
  0001000
  0000000
  ```
  Example 1:
  ```
  Input: N = 5, mines = [[4, 2]]
  Output: 2
  Explanation:
  11111
  11011
  In the above grid, the largest plus sign can only be order 2.  One of them is marked in bold.
  ```
  Example 2:
  ```
  Input: N = 2, mines = []
  Output: 1
  Explanation:
  There is no plus sign of order 2, but there is of order 1.
  ```
  Example 3:
  ```
  Input: N = 1, mines = [[0, 0]]
  Output: 0
  Explanation:
  There is no plus sign, so return 0.
  ```

- Solution

#### 2.4.6. 矩阵内的最大和

TODO:

《CC 150》 18.12

- Question
  > 给定一个正数和负数组成的 N * N 矩阵，编写程序找出元素总和最大的子矩阵。

- Solution
  - 暴力，时间复杂度为 O(N^6)
  - 动态规划，时间复杂度为 O(N^4)
  - 进一步优化，时间复杂度为 O(N^3)

### 2.5. 字母交换

[字母交换](https://www.nowcoder.com/questionTerminal/43488319efef4edabada3ca481068762)

- Question
  > 字符串 S 由小写字母构成，长度为 n。定义一种操作，每次都可以挑选字符串中任意的两个相邻字母进行交换。询问在至多交换 m 次之后，字符串中最多有多少个连续的位置上的字母相同？

  eg:
  ```
  输入
  abcbaa 2
  输出
  2
  ```

- Solution

  用 `dp[i][j]` 表示把 `pos[i]` 和 `pos[j]` 之间的目标字母移动到一起，形成 j - i + 1 长度的连续子序列所需要的操作次数，从而可得到状态转移方程：`dp[i][i + len - 1] = dp[i + 1][i + len - 2] + pos[i + len - 1] - pos[i] - len + 1`. 用最小的移动次数把两个目标字母移动到一起的方法就是把两个目标字母都往中间靠，状态转移方程就是根据这个来的，先把 `pos[i + 1]` ~ `pos[i + len - 2]` 之间的目标字母移动到一起，这个移动次数就是 `dp[i + 1][i + len - 2]`，然后把两个端点 `pos[i]` 和 `pos[i + len -1]` 处的目标字母往中间靠，所需要的移动次数是 `pos[i + len - 1] - pos[i] - len + 1`。
  ```java
  public static void main(String[] args) {
      Scanner sc = new Scanner(System.in);
      String s = sc.next();
      int m = sc.nextInt();
      int res = 1;

      for (char c = 'a'; c <= 'z'; c++) {
          ArrayList<Integer> position = new ArrayList<>();
          for (int i = 0; i < s.length(); i++)
              if (c == s.charAt(i))
                  position.add(i);

          if (position.size() < 2)
              continue;
          int ret = 1;

          // dp[i][j] 表示把 pos[i] 和 pos[j] 之间的目标字母移动到一起，形成 j-i+1 长度的连续子序列所需要的操作次数
          int [][] dp = new int[position.size()][position.size()];
          for (int len = 2; len <= position.size(); len++) { // 枚举连续序列的长度
              for (int i = 0; i + len - 1 < position.size(); i++) {
                  dp[i][i+len-1] = dp[i+1][i+len-2] + position.get(i+len-1) - position.get(i) - len + 1;
                  if (dp[i][i+len-1] <= m) // 若满足要求，则更新答案
                      ret = len;
              }
          }
          res = Math.max(res, ret);
      }
      System.out.println(res);
  }
  ```

## 3. 多维 DP

### 3.1. 着色问题

#### 3.1.1. 不同颜色球排列 Ⅰ -- 组合 DP

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
    由于数组索引表示的是剩余的数量，因此**在动态规划过程中应该使用递减顺序来赋值**。

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
        long [][][][] dp = new long[n+2][m+2][o+2][3];
        for (int i = n; i <= n + 1; i++)
          for (int j = m; j <= m + 1; j++)
            for (int k = o; k <= o + 1; k++)
              for (int h = 0; h < 3; h++)
                dp[i][j][k][h] = 0;

        dp[n - 1][m][o][0] = 1;
        dp[n][m - 1][o][1] = 1;
        dp[n][m][o - 1][2] = 1;

        for (int i = n; i >= 0; i--) {
          for (int j = m; j >= 0; j--) {
            for (int k = o; k >= 0; k--) {
              dp[i][j][k][0] += dp[i + 1][j][k][1] + dp[i + 1][j][k][2];
              dp[i][j][k][1] += dp[i][j + 1][k][0] + dp[i][j + 1][k][2];
              dp[i][j][k][2] += dp[i][j][k + 1][0] + dp[i][j][k + 1][1];
            }
          }
        }
        return dp[0][0][0][0] + dp[0][0][0][1] + dp[0][0][0][2];
      }
    }
    ```

#### 3.1.2. 不同颜色球排列 Ⅱ -- 组合 DP

https://blog.csdn.net/Vectorxj/article/details/51491754

- Question
  > 有 k 种颜色的球各 C1、C2、C3...Ck 个，现将这些球排成一行，要求相邻的球颜色不能相同，问有几种排列方式。其中，`1 <= k <= 15`, `1 <= Ci <= 5`。

- Solution

#### 3.1.3. 环形涂色 -- 环形 DP

https://blog.csdn.net/update7/article/details/79770832

https://blog.csdn.net/Tc_To_Top/article/details/48934857

https://blog.csdn.net/fyy2603/article/details/54767836

## 4. Refer Links

[LCSs——最长公共子序列和最长公共子串](https://www.cnblogs.com/maybe2030/p/5469877.html)

[动态规划的状态确定 -“不能连续跳两级的青蛙跳”和“栅栏涂色”](https://www.jianshu.com/p/e5dd3cdf158a?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

[如何计算排列方法数，其中相同元素不能相邻？](https://www.zhihu.com/question/29890888)