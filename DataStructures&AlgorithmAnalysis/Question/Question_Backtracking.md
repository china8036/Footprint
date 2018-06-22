- [Backtracking](#backtracking)
    - [1. 排列问题 Permutation](#1--permutation)
        - [1.1. Permutations](#11-permutations)
        - [1.2. Permutations II](#12-permutations-ii)
    - [2. 组合问题 Combination](#2--combination)
        - [2.1. Combinations](#21-combinations)
        - [2.2. Combination Sum](#22-combination-sum)
        - [2.3. Combination Sum II](#23-combination-sum-ii)
        - [2.4. Combination Sum III](#24-combination-sum-iii)
        - [2.5. Subsets](#25-subsets)
        - [2.6. Subsets II](#26-subsets-ii)
        - [2.7. Binary Watch](#27-binary-watch)
    - [3. 二维平面问题](#3)
        - [3.1. Word Search](#31-word-search)
        - [3.2. Flood Fill 问题](#32-flood-fill)
            - [3.2.1. Number of Islands](#321-number-of-islands)
            - [3.2.2. Surrounded Regions](#322-surrounded-regions)
            - [3.2.3. Pacific Atlantic Water Flow](#323-pacific-atlantic-water-flow)
        - [3.3. N=Queue 问题](#33-nqueue)
            - [3.3.1. N-Queens](#331-n-queens)
            - [3.3.2. N-Queens II](#332-n-queens-ii)
        - [3.4. Sudoku Solver](#34-sudoku-solver)
    - [4. 其它问题](#4)
        - [4.1. Letter Combinations of a Phone Number](#41-letter-combinations-of-a-phone-number)
        - [4.2. Restore IP Addresses](#42-restore-ip-addresses)
        - [4.3. Palindrome Partitioning](#43-palindrome-partitioning)
    - [5. Refer Links](#5-refer-links)

# Backtracking

## 1. 排列问题 Permutation

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

## 2. 组合问题 Combination

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

### 2.2. Combination Sum

[39. Combination Sum](https://leetcode.com/problems/combination-sum/description/)

### 2.3. Combination Sum II

[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/description/)

### 2.4. Combination Sum III

[216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/description/)

### 2.5. Subsets

[78. Subsets](https://leetcode.com/problems/subsets/description/)

### 2.6. Subsets II

[90. Subsets II](https://leetcode.com/problems/subsets-ii/description/)

### 2.7. Binary Watch

[401. Binary Watch](https://leetcode.com/problems/binary-watch/description/)

## 3. 二维平面问题

### 3.1. Word Search

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

### 3.2. Flood Fill 问题

#### 3.2.1. Number of Islands

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

#### 3.2.2. Surrounded Regions

[130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/description/)

#### 3.2.3. Pacific Atlantic Water Flow

[417. Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/description/)

### 3.3. N=Queue 问题

#### 3.3.1. N-Queens

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

#### 3.3.2. N-Queens II

[52. N-Queens II](https://leetcode.com/problems/n-queens-ii/description/)

### 3.4. Sudoku Solver

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

## 5. Refer Links
