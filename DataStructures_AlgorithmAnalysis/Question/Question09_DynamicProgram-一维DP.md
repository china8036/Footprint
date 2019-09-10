- [Dynamic programming 一维 DP](#dynamic-programming-一维-dp)
  - [1. 斐波那契数列 Fibonacci polynomial](#1-斐波那契数列-fibonacci-polynomial)
    - [1.1.1. 爬楼梯问题 Climbing Stairs](#111-爬楼梯问题-climbing-stairs)
    - [1.1.1. 爬楼梯问题 Ⅱ](#111-爬楼梯问题-Ⅱ)
    - [爬楼梯问题 Ⅲ](#爬楼梯问题-Ⅲ)
    - [矩阵覆盖问题](#矩阵覆盖问题)
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
    - [1.4.4. 堆箱子问题 (stack of boxes)](#144-堆箱子问题-stack-of-boxes)
  - [1.5. 跳房子](#15-跳房子)
  - [4. Refer Links](#4-refer-links)

# Dynamic programming 一维 DP

TODO: 动态规划题库：https://www.cnblogs.com/Roni-i/p/9010094.html

## 1. 斐波那契数列 Fibonacci polynomial

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

### 1.1.1. 爬楼梯问题 Climbing Stairs

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

  ![image](http://img.cdn.firejq.com/jpg/2018/4/23/326b837d0a0d9848714011b5f0a6f518.jpg)

  为了到第 n 阶台阶，存在以下可能的情况：
  - 从第 n-1 阶台阶爬 1 阶，到了第 n 阶
  - 从第 n-2 阶台阶爬 2 阶，到了第 n 阶
  因此，f(n) = f(n-1) + f(n-2).
  ```java
  // 时间复杂度：O(n), 空间复杂度：O(n)
  public int JumpFloor(int target) {
      if (target == 0)
          return 1;
      int [] res = new int[target+1];
      res[0] = 1; // 注意此处与斐波那契数列的区别
      res[1] = 1;
      for (int i = 2; i <= target; i++)
          res[i] = res[i - 1] + res[i - 2];
      return res[target];
  }
  ```

### 1.1.1. 爬楼梯问题 Ⅱ

爬楼梯时可以一次爬 2 级或者 1 级，但是**不能连续爬 2 级**。问 n 级台阶有多少种可能的情况？

用 f(n) 表示第 n 级时的方案数量，为了到第 n 级台阶，存在以下可能的情况：
- 从第 n-1 级跳 1 级，到了第 n 级台阶。因此可能的方案有 f(n-1) 种。
- 从第 n-2 级跳 2 级，到了第 n 级台阶。由于不能连续跳 2 次 2 级，因此可以确定是从第 n-3 级台阶先跳了一次 1 级，再跳了一次 2 级，才到达第 n 级的。因此可能的方案有 f(n-3) 种。

### 爬楼梯问题 Ⅲ

- Question
  > 爬楼梯时一次可以爬 1 级台阶，也可以爬 2 级 ... 也可以爬 n 级。求爬上一个 n 级的台阶总共有多少种爬法。

- Solution
  - 直接思路：`f(n) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2) + f(n-1)`
    ```java
    public int JumpFloor(int target) {
        if (target == 0)
            return 1;
        int [] res = new int[target+1];
        res[0] = 1;
        res[1] = 1;
        for (int i = 2; i <= target; i++)
            for (int j = 1; j <= i; j++)
                res[i] += res[i - j];
        return res[target];
    }
    ```

  - 进一步优化：
    ```
    f(n-1) = f(0) + f(1)+f(2)+f(3) + ... + f((n-1)-1) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2)

    f(n) = f(0) + f(1) + f(2) + f(3) + ... + f(n-2) + f(n-1) = f(n-1) + f(n-1)
    ```
    因此，可以得出：
    ```
    f(n) = 2*f(n-1)
    ```

    非递归版本：
    ```java
    public int JumpFloor(int target) {
        if (target == 0)
            return 1;
        int [] res = new int[target+1];
        res[0] = 1;
        res[1] = 1;
        for (int i = 2; i <= target; i++)
            res[i] = 2 * res[i - 1];
        return res[target];
    }
    ```

    递归版本：
    ```java
    public int JumpFloor(int target) {
        if (target <= 0) {
            return -1;
        } else if (target == 1) {
            return 1;
        } else {
            return 2 * JumpFloor(target - 1);
        }
    }
    ```

### 矩阵覆盖问题

- Question
  > 我们可以用 `2*1` 的小矩形横着或者竖着去覆盖更大的矩形。请问用 n 个 `2*1` 的小矩形无重叠地覆盖一个 `2*n` 的大矩形，总共有多少种方法？

- Solution

  可以发现实际上仍是一个斐波那契的题目，但初始值需要特别注意。递推公式：`f(n)=f(n-1)+f(n-2)`。

  ```java
  public int RectCover(int target) {
    if (target == 0)
        return 0;
    int [] res = new int[target+1];
    res[0] = 1;
    res[1] = 1;
    for (int i = 2; i <= target; i++)
        res[i] = res[i - 1] + res[i - 2];
    return res[target];
  }
  ```

### 1.1.2. 摆放球问题

有两种不同颜色的球，共 n 个，要求**不能有三个连续的相同颜色的排在一起**，问有几种排列方式。

用 f(n) 表示 n 个球时有几种方案：
- 如果第 n 个球与 n-1 个球的颜色一样，那么当前 n-1 的状态算已经确定下来了，对当前结果起作用的是 n-2 这个状态，需要根 n-2 颜色取的不一样。也就是 (2-1)f(n-2) 种（总共有两种颜色，还剩 1 种能取)。
- 如果第 n 个球与 n-1 个球颜色不一样，那么就需要看 n-1 有几种状态，然后和 n-1 取不一样的。也就是 (2-1)f(n-1) 种。

所以最后就是 **f(n)=f(n-1)+f(n-2)**，这么看很像斐波那契数列，上楼梯问题。只不过初始边界值不同。

### 1.1.3. Triangle

### 1.1.4. Minimum Path Sum

## 1.2. 整数分解

### 1.2.1. Integer Break

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

### 1.2.2. Perfect Squares

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

### 1.2.3. Decode Ways

[91. Decode Ways](https://leetcode.com/problems/decode-ways/description/)

## 1.3. 抢劫房子问题

### 1.3.1. House Robber

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

### 1.3.2. House Robber II

[213. House Robber II](https://leetcode.com/problems/house-robber-ii/description/)

### 1.3.3. House Robber III

[Ho337. House Robber III](https://leetcode.com/problems/house-robber-iii/description/)

### 1.3.4. Best Time to Buy and Sell Stock with Cooldown

[309. Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

## 1.4. 最长上升子序列 （LIS 问题）

### 1.4.1. Longest Increasing Subsequence

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

### 1.4.2. Wiggle Subsequence

[376. Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/description/)

### 1.4.4. 堆箱子问题 (stack of boxes)

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

## 1.5. 跳房子

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

## 4. Refer Links

[动态规划的状态确定 -“不能连续跳两级的青蛙跳”和“栅栏涂色”](https://www.jianshu.com/p/e5dd3cdf158a?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)