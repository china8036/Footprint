- [Dynamic programming 二维 DP](#dynamic-programming-二维-dp)
  - [1. 最小修改次数问题](#1-最小修改次数问题)
    - [1.1. Edit Distance](#11-edit-distance)
    - [1.2. Delete Operation for Two Strings](#12-delete-operation-for-two-strings)
    - [1.3. Minimum ASCII Delete Sum for Two Strings](#13-minimum-ascii-delete-sum-for-two-strings)
  - [2. 背包问题](#2-背包问题)
    - [2.1. Partition Equal Subset Sum](#21-partition-equal-subset-sum)
    - [2.2. Coin Change](#22-coin-change)
    - [2.3. Combination Sum IV](#23-combination-sum-iv)
    - [2.4. Ones and Zeroes](#24-ones-and-zeroes)
    - [2.5. Word Break](#25-word-break)
    - [2.6. Target Sum](#26-target-sum)
  - [3. LCSs 问题](#3-lcss-问题)
    - [3.1. 最长公共子序列问题](#31-最长公共子序列问题)
    - [3.2. 最长公共子串问题](#32-最长公共子串问题)
  - [4. 矩阵系列问题](#4-矩阵系列问题)
    - [4.1. 矩阵内的不同路径](#41-矩阵内的不同路径)
      - [4.1.1. Unique Paths](#411-unique-paths)
      - [4.1.2. Unique Paths II](#412-unique-paths-ii)
    - [4.2. 直方图内的最大覆盖矩阵](#42-直方图内的最大覆盖矩阵)
    - [4.3. 矩阵内的最大全一长方形](#43-矩阵内的最大全一长方形)
    - [4.4. 矩阵内的最大全一正方形](#44-矩阵内的最大全一正方形)
    - [4.5. 矩阵内的最大加号](#45-矩阵内的最大加号)
    - [4.6. 矩阵内的最大和](#46-矩阵内的最大和)
  - [5. 字母交换](#5-字母交换)
  - [6. Refer Links](#6-refer-links)

# Dynamic programming 二维 DP

## 1. 最小修改次数问题

### 1.1. Edit Distance

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

### 1.2. Delete Operation for Two Strings

[583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/description/)

### 1.3. Minimum ASCII Delete Sum for Two Strings

[712. Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/description/)

## 2. 背包问题

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
        memo[index][c] = res;
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
                memo[i][j] = memo[i-1][j];
                if(j >= w[i])
                    // 策略 2：放第 i 个物品
                    memo[i][j] = max(memo[i] [j], v[i] + memo[i - 1][j - w[i]]);
            }
        return memo[n - 1][C];
    }
};
```

### 2.1. Partition Equal Subset Sum

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

        if(memo[index][sum] != -1)
            return memo[index][sum] == 1;

        memo[index][sum] = (tryPartition(nums, index - 1, sum) ||
               tryPartition(nums, index - 1, sum - nums[index])) ? 1 : 0;

        return memo[index][sum] == 1;
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

### 2.2. Coin Change

[322. Coin Change](https://leetcode.com/problems/coin-change/description/)

### 2.3. Combination Sum IV

[377. Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/description/)

### 2.4. Ones and Zeroes

[474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/description/)

### 2.5. Word Break

[139. Word Break](https://leetcode.com/problems/word-break/description/)

### 2.6. Target Sum

[494. Target Sum](https://leetcode.com/problems/target-sum/description/)

## 3. LCSs 问题

**LCSs 问题实际上是指最长公共子序列问题（Longest-Common-Subsequence）和最长公共子串（Longest-Common-Substring）问题**，这两个问题非常的相似，区别在于：
- **子序列是有序的，但不一定是连续，作用对象是序列**。例如：序列 X = <B, C, D, B> 是序列 Y = <A, B, C, B, D, A, B> 的子序列，对应的下标序列为 <2, 3, 5, 7>。
- **子串是有序且连续的，左右对象是字符串**。例如：a = abcd 是 c = aaabcdddd 的一个子串；但是 b = acdddd 就不是 c 的子串。

两者问题很相似，均可以使用动态规划的思想进行求解。

### 3.1. 最长公共子序列问题

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
                memo[0][j] = 1;
        for(int i = 0 ; i < m ; i ++)
            if(s1[i] == s2[0])
                memo[i][0] = 1;

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

### 3.2. 最长公共子串问题

## 4. 矩阵系列问题

### 4.1. 矩阵内的不同路径

#### 4.1.1. Unique Paths

[62. Unique Paths](https://leetcode.com/problems/unique-paths/description/)

[《剑指 offer》 面试题 67](https://www.nowcoder.com/practice/6e5207314b5241fb83f2329e89fdecc8?tpId=13&tqId=11219&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

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

#### 4.1.2. Unique Paths II

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

### 4.2. 直方图内的最大覆盖矩阵

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

### 4.3. 矩阵内的最大全一长方形

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
      // number of consecutive 1s to the left of matrix[i](j), including itself
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

### 4.4. 矩阵内的最大全一正方形

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

### 4.5. 矩阵内的最大加号

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

### 4.6. 矩阵内的最大和

TODO:

《CC 150》 18.12

- Question
  > 给定一个正数和负数组成的 N * N 矩阵，编写程序找出元素总和最大的子矩阵。

- Solution
  - 暴力，时间复杂度为 O(N^6)
  - 动态规划，时间复杂度为 O(N^4)
  - 进一步优化，时间复杂度为 O(N^3)

## 5. 字母交换

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

          // dp[i](j) 表示把 pos[i] 和 pos[j] 之间的目标字母移动到一起，形成 j-i+1 长度的连续子序列所需要的操作次数
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

## 6. Refer Links

[LCSs——最长公共子序列和最长公共子串](https://www.cnblogs.com/maybe2030/p/5469877.html)

[如何计算排列方法数，其中相同元素不能相邻？](https://www.zhihu.com/question/29890888)