- [算法设计：回溯](#)
    - [1. 基本概念](#1)
    - [2. 算法思想](#2)
    - [3. 算法框架](#3)
    - [4. 时间效率](#4)
    - [5. 应用](#5)
        - [5.1. 货船装箱](#51)
        - [5.2. 背包问题](#52)
        - [5.3. 最大完备子图](#53)
    - [6. Refer Links](#6-refer-links)

# 算法设计：回溯

## 1. 基本概念

回溯法（backtracking）是暴力搜索法中的一种。许多复杂的、规模较大的问题都可以使用回溯法，因此回溯法也有“通用解题方法”的美称。

**回溯法采用试错的思路，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其它的可能的分步解答再次尝试寻找问题的答案。**满足回溯条件的某个状态的点称为“回溯点”。

## 2. 算法思想

**回溯法实际上是对隐式图的深度优先搜索算法。**

在包含问题的所有解的解空间树中，按照深度优先策略，从根结点出发搜索解空间树，当搜索到任一结点时，先判断该结点是否包含问题的解，如果不包含，则跳过以该结点为根的子树的搜索，逐层向根结点回溯；否则进入该子树，继续按深度优先策略进行搜索。

如果要找到问题的所有解，则必须回溯到根，并搜索完根结点的所有子树才结束，而如果只求解问题的一个解，则只要搜索到问题的一个解便可结束。

## 3. 算法框架

- 问题框架

  设问题的解是一个 n 维向量 (a1,a2,………,an), 约束条件是 ai(i=1,2,3,…..,n) 之间满足某种条件，记为 f(ai)。一般求解的问题有三种：
  - Find a path to success 有没有解
  - Find all paths to success 求所有解
    - 求所有解的个数
    - 求所有解的具体信息
  - Find the best path to success 求最优解

- 用回溯法解题的一般步骤：
  1. 针对所给问题，确定问题的解空间。首先应明确定义问题的解空间，问题的解空间应至少包含问题的一个（最优）解。
  1. 确定结点的扩展搜索规则
  1. 以深度优先方式搜索解空间，并在搜索过程中用剪枝函数避免无效搜索。

- 算法框架

  **回溯法是对解空间的深度优先搜索，在一般情况下使用递归函数来实现回溯法比较简单**：
  - 有没有解
    ```cpp
    boolean solve(Node n) {
        if n is a leaf node {
            if the leaf is a goal node, return true
            else return false
        } else {
            for each child c of n {
                if solve(c) succeeds, return true
            }
            return false
        }
    }
    ```
  - 求所有解
    ```cpp
    void solve(Node n) {
        if n is a leaf node {
            if the leaf is a goal node, count++, return;
            else return
        } else {
            for each child c of n {
                solve(c)
            }
        }
    }
    ```
  - 求最优解
    ```cpp
    void solve(Node n) {
        if n is a leaf node {
            if the leaf is a goal node, update best result, return;
            else return
        } else {
            for each child c of n {
                solve(c)
            }
        }
    }
    ```

## 4. 时间效率

**在最坏的情况下，回溯法会导致一次复杂度为指数时间的计算**。

## 5. 应用

### 5.1. 货船装箱

### 5.2. 背包问题

### 5.3. 最大完备子图

## 6. Refer Links

[维基百科：回溯法](https://zh.wikipedia.org/wiki/%E5%9B%9E%E6%BA%AF%E6%B3%95)

[五大常用算法之四：回溯法](https://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741376.html)

[Leetcode Backtracking 回溯法（又称 DFS, 递归) 全解 ](https://segmentfault.com/a/1190000006121957)