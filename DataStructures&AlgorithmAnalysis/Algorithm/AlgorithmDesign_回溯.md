- [算法设计：回溯](#)
    - [1. 基本概念](#1)
    - [2. 算法思想](#2)
    - [3. 算法框架](#3)
    - [4. 时间效率](#4)
    - [5. 应用](#5)
        - [5.1. 排列问题 Permutation](#51--permutation)
            - [5.1.1. Permutations](#511-permutations)
            - [5.1.2. Permutations II](#512-permutations-ii)
        - [5.2. 组合问题 Combination](#52--combination)
            - [5.2.1. Combinations](#521-combinations)
            - [5.2.2. Combination Sum](#522-combination-sum)
            - [5.2.3. Combination Sum II](#523-combination-sum-ii)
            - [5.2.4. Combination Sum III](#524-combination-sum-iii)
            - [5.2.5. Subsets](#525-subsets)
            - [5.2.6. Subsets II](#526-subsets-ii)
            - [5.2.7. Binary Watch](#527-binary-watch)
        - [5.3. 二维平面问题](#53)
            - [5.3.1. Word Search](#531-word-search)
            - [5.3.2. Flood Fill 问题](#532-flood-fill)
                - [5.3.2.1. Number of Islands](#5321-number-of-islands)
                - [5.3.2.2. Surrounded Regions](#5322-surrounded-regions)
                - [5.3.2.3. Pacific Atlantic Water Flow](#5323-pacific-atlantic-water-flow)
            - [5.3.3. N=Queue 问题](#533-nqueue)
                - [5.3.3.1. N-Queens](#5331-n-queens)
                - [5.3.3.2. N-Queens II](#5332-n-queens-ii)
            - [5.3.4. Sudoku Solver](#534-sudoku-solver)
        - [5.4. 货船装箱](#54)
        - [5.5. 背包问题](#55)
        - [5.6. 最大完备子图](#56)
        - [5.7. 其它问题](#57)
            - [5.7.1. Letter Combinations of a Phone Number](#571-letter-combinations-of-a-phone-number)
            - [5.7.2. Restore IP Addresses](#572-restore-ip-addresses)
            - [5.7.3. Palindrome Partitioning](#573-palindrome-partitioning)
    - [6. Refer Links](#6-refer-links)

# 算法设计：回溯

## 1. 基本概念

回溯法（backtracking）是暴力搜索法中的一种。许多复杂的、规模较大的问题都可以使用回溯法，因此回溯法也有“通用解题方法”的美称。

回溯法采用试错的思路，它尝试分步的去解决一个问题。在分步解决问题的过程中，当它通过尝试发现现有的分步答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其它的可能的分步解答再次尝试寻找问题的答案。满足回溯条件的某个状态的点称为“回溯点”。

## 2. 算法思想

回溯法实际上是对隐式图的**深度优先搜索**算法。

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

  回溯法是对解空间的深度优先搜索，在一般情况下使用递归函数来实现回溯法比较简单：
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

在最坏的情况下，回溯法会导致一次复杂度为指数时间的计算。

## 5. 应用

### 5.1. 排列问题 Permutation

#### 5.1.1. Permutations

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
  Perms(nums[0...n-1]) = {取出一个数字} + Perms(nums[{0...n-1} - 这个数字』)
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
          if(index == nums.size()){
              res.push_back(p);
              return;
          }
          for(int i = 0 ; i < nums.size() ; i ++)
              if(!used[i]){
                  used[i] = true;
                  p.push_back(nums[i]);
                  generatePermutation(nums, index + 1, p );
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

#### 5.1.2. Permutations II

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

### 5.2. 组合问题 Combination

#### 5.2.1. Combinations

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
          if(c.size() == k){
              res.push_back(c);
              return;
          }
          // 还有 k - c.size() 个空位，所以，[i...n] 中至少要有 k - c.size() 个元素
          // i 最多为 n - (k - c.size()) + 1
          for(int i = start; i <= n - (k - c.size()) + 1 ; i ++){
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

#### 5.2.2. Combination Sum

[39. Combination Sum](https://leetcode.com/problems/combination-sum/description/)

#### 5.2.3. Combination Sum II

[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

#### 5.2.4. Combination Sum III

[216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

#### 5.2.5. Subsets

[78. Subsets](https://leetcode.com/problems/subsets/description/)

#### 5.2.6. Subsets II

[90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)

#### 5.2.7. Binary Watch

[401. Binary Watch](https://leetcode.com/problems/binary-watch/description/)

### 5.3. 二维平面问题

#### 5.3.1. Word Search

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

#### 5.3.2. Flood Fill 问题

##### 5.3.2.1. Number of Islands

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

##### 5.3.2.2. Surrounded Regions

[130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/)

##### 5.3.2.3. Pacific Atlantic Water Flow

[417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)

#### 5.3.3. N=Queue 问题

##### 5.3.3.1. N-Queens

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
      void putQueen(int n, int index, vector<int> &row){
          if(index == n){
              res.push_back(generateBoard(n, row));
              return;
          }

          for(int i = 0 ; i < n ; i ++)
              // 尝试将第 index 行的皇后摆放在第 i 列
              if(!col[i] && !dia1[index + i] && !dia2[index - i + n - 1]){
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

##### 5.3.3.2. N-Queens II

[52. N-Queens II](https://leetcode.com/problems/n-queens-ii/description/)

#### 5.3.4. Sudoku Solver

[37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/description/)

### 5.4. 货船装箱

### 5.5. 背包问题

### 5.6. 最大完备子图

### 5.7. 其它问题

#### 5.7.1. Letter Combinations of a Phone Number

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

#### 5.7.2. Restore IP Addresses

[93. Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/description/)

- Question
  > Given a string containing only digits, restore it by returning all possible valid IP address combinations.
  Example:
  ```
  Input: "25525511135"
  Output: ["255.255.11.135", "255.255.111.35"]
  ```
- Solution
  
#### 5.7.3. Palindrome Partitioning

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

## 6. Refer Links

[维基百科：回溯法](https://zh.wikipedia.org/wiki/%E5%9B%9E%E6%BA%AF%E6%B3%95)

[五大常用算法之四：回溯法](https://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741376.html)

[[Leetcode] Backtracking 回溯法（又称 DFS, 递归) 全解』(https://segmentfault.com/a/1190000006121957)