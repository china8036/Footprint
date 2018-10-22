- [Stack & Queue & Heap](#stack--queue--heap)
  - [1. Stack](#1-stack)
    - [1.1. 实现一个栈](#11-实现一个栈)
    - [1.2. Tower of Hanoi](#12-tower-of-hanoi)
    - [1.3. Valid Parentheses](#13-valid-parentheses)
    - [1.4. Evaluate Reverse Polish Notation](#14-evaluate-reverse-polish-notation)
    - [1.5. Simplify Path](#15-simplify-path)
    - [1.6. 二叉树的 DFS](#16-二叉树的-dfs)
      - [1.6.1. Binary Tree Preorder Traversal](#161-binary-tree-preorder-traversal)
      - [1.6.2. Binary Tree Inorder Traversal](#162-binary-tree-inorder-traversal)
      - [1.6.3. Binary Tree Postorder Traversal](#163-binary-tree-postorder-traversal)
    - [1.7. 双栈排序](#17-双栈排序)
  - [2. Queue](#2-queue)
    - [2.1. 实现一个队列](#21-实现一个队列)
    - [2.2. 二叉树的层序遍历](#22-二叉树的层序遍历)
      - [2.2.1. Binary Tree Level Order Traversal](#221-binary-tree-level-order-traversal)
      - [2.2.2. Binary Tree Level Order Traversal Ⅱ](#222-binary-tree-level-order-traversal-Ⅱ)
      - [2.2.3. Binary Tree Zigzag Level Order Traversal](#223-binary-tree-zigzag-level-order-traversal)
      - [2.2.4. Binary Tree Right Side View](#224-binary-tree-right-side-view)
    - [2.3. 无权图的最短路径](#23-无权图的最短路径)
      - [2.3.1. Perfect Squares](#231-perfect-squares)
      - [2.3.2. Word Ladder](#232-word-ladder)
      - [2.3.3. Word Ladder II](#233-word-ladder-ii)
  - [3. Heap](#3-heap)
    - [3.1. Top K Frequent Elements](#31-top-k-frequent-elements)
    - [3.2. Merge k Sorted Lists](#32-merge-k-sorted-lists)
  - [4. Refer Links](#4-refer-links)

# Stack & Queue & Heap

## 1. Stack

### 1.1. 实现一个栈

```java
// based on linked list
public class MyStack<T> {
  private Node<T> top; // 保存栈顶元素

  public T peek() {
    return top == null ? null : top.data;
  }

  public T pop() {
    if (top == null) 
      return null;
    T data = top.data;
    top = top.next;
    return data;
  }

  public void push(T data) {
    Node<T> node = new Node<>(data);
    node.next = top;
    top = node;
  }
}
```

### 1.2. Tower of Hanoi

- Question

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/17/3b7381aed22b5bbd915208c219d7e586.jpg)

  编写程序，将所有盘子从第 1 个塔移动到第 3 个塔。

- Solution

  采用基本案例构建法进行分析：
  1. 当 n=1 时，显然可以将盘子从 origin 移动到 destination。
  1. 当 n=2 时，显然可以将盘子从 origin 移动到 destination。
  1. 当 n=3 时，先将前 2 个盘子从 origin 移动到 buffer，再把第 3 个盘子从 origin 移动到 destination，最后把前 2 个盘子移动到 destination 即可。
  
  因此，可以采用递归的思想：
  ```java
  public void hanoi(int n, Tower origin, Tower destination, Tower buffer) {
    if (n <= 0)
      return;
    hanoi(n - 1, origin, buffer, destination); 	// 把前 n-1 个盘子从 origin 移到 buffer
    moveTop(origin, destination);								// 把最大的盘子从 origin 移到 destination
    hanoi(n - 1, buffer, destination, origin);	// 把前 n-1 个盘子从 buffer 移到 destination
  }
  ```

### 1.3. Valid Parentheses

[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

- Question
  > Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

  An input string is valid if:
  - Open brackets must be closed by the same type of brackets.
  - Open brackets must be closed in the correct order.

  Note that an empty string is also considered valid.

  Example 1:
  ```
  Input: "()"
  Output: true
  ```
  Example 2:
  ```
  Input: "()[]{}"
  Output: true
  ```
  Example 3:
  ```
  Input: "(]"
  Output: false
  ```
  Example 4:
  ```
  Input: "([)]"
  Output: false
  ```
  Example 5:
  ```
  Input: "{[]}"
  Output: true
  ```
- Solution
  
  该问题是典型的可以使用 stack 进行解决的问题，栈顶元素反映了在嵌套的层次关系中，最近的需要匹配的元素。
  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(n)
  public boolean isValid(String s) {
      if (s == null || s.length() == 0)
          return true;
      ArrayDeque<Character> stack = new ArrayDeque<>();
      for (char c : s.toCharArray()) {
          if (c == '(')
              stack.push(')');
          else if (c == '{')
              stack.push('}');
          else if (c == '[')
              stack.push(']');
          else if (stack.isEmpty() || stack.pop() != c)
              return false;
      }
      return stack.isEmpty();
  }
  ```

### 1.4. Evaluate Reverse Polish Notation

[150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

- Question: 
  > Evaluate the value of an arithmetic expression in Reverse Polish Notation.
  >
  > Valid operators are +, -, *, /. Each operand may be an integer or another expression.

  Example:
  ```
  Input: ["2", "1", "+", "3", "*"]
  Output: 9
  Explanation: ((2 + 1) * 3) = 9
  ```

  逆波兰表达式（后缀表达式）：

  > 在逆波兰记法中，所有操作符置于操作数的后面，因此也被称为后缀表示法。逆波兰记法不需要括号来标识操作符的优先级，是一种利用栈来进行运算的数学表达式。相比传统的计算机表达式求值，逆波兰表达式算法取得了时间复杂度和空间复杂度上的双重优势。

- Solution
  ```java
  public int evalRPN(String[] tokens) {
      int a, b;
      ArrayDeque<Integer> S = new ArrayDeque<>();
      for (String s : tokens) {
          if(s.equals("+"))
              S.push(S.pop()+S.pop());
          else if(s.equals("*")) 
              S.push(S.pop() * S.pop());
          else if(s.equals("/")) {
              b = S.pop(); // 栈顶为被除数
              a = S.pop();
              S.push(a / b);
          }
          else if(s.equals("-")) {
              b = S.pop(); // 栈顶为被减数
              a = S.pop();
              S.push(a - b);
          }
          else 
              S.push(Integer.parseInt(s));
      }
      return S.pop();
  }
  ```

### 1.5. Simplify Path

[71. Simplify Path](https://leetcode.com/problems/simplify-path/description/)

- Question
  > Given an absolute path for a file (Unix-style), simplify it.	
  For example,
  ```
  path = "/home/", => "/home"
  path = "/a/./b/../../c/", => "/c"
  ```

- Solution
  ```java
  public String simplifyPath(String path) {
      Deque<String> stack = new LinkedList<>();
      Set<String> skip = new HashSet<>(Arrays.asList("..", ".", ""));
      
      String [] paths = path.split("/");
      for (String dir : paths) {
          if (dir.equals("..") && !stack.isEmpty()) 
              stack.pop();
          else if (!skip.contains(dir)) 
              stack.push(dir);
      }
      String res = "";
      for (String dir : stack)
          res = "/" + dir + res;
      return res.isEmpty() ? "/" : res;
  }
  ```

### 1.6. 二叉树的 DFS

#### 1.6.1. Binary Tree Preorder Traversal

[144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/description/)

- Question
  > Given a binary tree, return the preorder traversal of its nodes' values.

  Example:
  ```
  Input: [1,null,2,3]
    1
      \
      2
      /
    3

  Output: [1,2,3]
  ```
  Follow up: Recursive solution is trivial, could you do it iteratively?
- Solution
  - 递归
    ```java
    // 时间复杂度：O(n), n 为树的节点个数
    // 空间复杂度：O(h), h 为树的高度
    public class Solution144 {
        public List<Integer> preorderTraversal(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<>();
            preorderTraversal(root, res);
            return res;
        }

        private void preorderTraversal(TreeNode node, List<Integer> list){
            if (node != null) {
                list.add(node.val);
                preorderTraversal(node.left, list);
                preorderTraversal(node.right, list);
            }
        }
    }
    ```
  - 非递归（利用 stack）
    ```java
    // 时间复杂度：O(n), n 为树的节点个数
    // 空间复杂度：O(h), h 为树的高度
    public class Solution144 {
        private class Command{
            String s;   // go, print
            TreeNode node;
            Command(String s, TreeNode node){
                this.s = s;
                this.node = node;
            }
        };

        public List<Integer> preorderTraversal(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<>();
            if(root == null)
                return res;
            Stack<Command> stack = new Stack<>();
            stack.push(new Command("go", root));
            while(!stack.empty()){
                Command command = stack.pop();
                if(command.s.equals("print"))
                    res.add(command.node.val);
                else{
                    assert command.s.equals("go");
                    if(command.node.right != null)
                        stack.push(new Command("go",command.node.right));
                    if(command.node.left != null)
                        stack.push(new Command("go",command.node.left));
                    stack.push(new Command("print", command.node));
                }
            }
            return res;
        }
    }
    ```
    ```java
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        ArrayDeque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root;
        while(!stack.isEmpty() || cur != null) {
            if(cur != null) {
                stack.push(cur);
                res.add(cur.val);  // Add before going to children
                cur = cur.left;
            } else {
                cur = stack.pop().right;   
            }
        }
        return res;
    }
    ```

#### 1.6.2. Binary Tree Inorder Traversal

[94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/description/)

- Question
  > Given a binary tree, return the inorder traversal of its nodes' values.

  Example:
  ```
  Input: [1,null,2,3]
    1
      \
      2
      /
    3

  Output: [1,3,2]
  ```
  Follow up: Recursive solution is trivial, could you do it iteratively?
- Solution

  - 递归
    ```java
    // 时间复杂度：O(n), n 为树的节点个数
    // 空间复杂度：O(h), h 为树的高度
    public class Solution094 {
        public List<Integer> inorderTraversal(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<Integer>();
            inorderTraversal(root, res);
            return res;
        }

        private void inorderTraversal(TreeNode node, List<Integer> list){
            if(node != null){
                inorderTraversal(node.left, list);
                list.add(node.val);
                inorderTraversal(node.right, list);
            }
        }
    }
    ```
  - 非递归（利用 stack）
    ```java
    // 时间复杂度：O(n), n 为树的节点个数
    // 空间复杂度：O(h), h 为树的高度
    public class Solution094 {
        private class Command{
            String s;   // go, print
            TreeNode node;
            Command(String s, TreeNode node){
                this.s = s;
                this.node = node;
            }
        };
        public List<Integer> inorderTraversal(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<Integer>();
            if(root == null)
                return res;
            Stack<Command> stack = new Stack<Command>();
            stack.push(new Command("go", root));
            while(!stack.empty()){
                Command command = stack.pop();

                if(command.s.equals("print"))
                    res.add(command.node.val);
                else{
                    assert command.s.equals("go");
                    if(command.node.right != null)
                        stack.push(new Command("go",command.node.right));
                    stack.push(new Command("print", command.node));
                    if(command.node.left != null)
                        stack.push(new Command("go",command.node.left));
                }
            }
            return res;
        }
    }
    ```
    ```java
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        ArrayDeque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root;
        while(!stack.isEmpty() || cur != null) {
            if(cur != null) {
                stack.push(cur);
                cur = cur.left;
            } else {
                cur = stack.pop();
                res.add(cur.val);  // Add after all left children
                cur = cur.right;   
            }
        }
        return res;
    }
    ```
    
#### 1.6.3. Binary Tree Postorder Traversal

[145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)

- Question
  > Given a binary tree, return the postorder traversal of its nodes' values.

  Example:
  ```
  Input: [1,null,2,3]
    1
      \
      2
      /
    3

  Output: [3,2,1]
  ```
  Follow up: Recursive solution is trivial, could you do it iteratively?
- Solution

  - 递归
    ```java
    // 时间复杂度：O(n), n 为树的节点个数
    // 空间复杂度：O(h), h 为树的高度
    public class Solution145 {
        public List<Integer> postorderTraversal(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<Integer>();
            postorderTraversal(root, res);
            return res;
        }

        private void postorderTraversal(TreeNode node, List<Integer> list){
            if(node != null){
                postorderTraversal(node.left, list);
                postorderTraversal(node.right, list);
                list.add(node.val);
            }
        }
    }
    ```
  - 非递归（利用 stack）
    ```java
    // 时间复杂度：O(n), n 为树的节点个数
    // 空间复杂度：O(h), h 为树的高度
    public class Solution145 {
        private class Command{
            String s;   // go, print
            TreeNode node;
            Command(String s, TreeNode node){
                this.s = s;
                this.node = node;
            }
        };

        public List<Integer> postorderTraversal(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<Integer>();
            if(root == null)
                return res;
            Stack<Command> stack = new Stack<Command>();
            stack.push(new Command("go", root));
            while(!stack.empty()){
                Command command = stack.pop();
                if(command.s.equals("print"))
                    res.add(command.node.val);
                else{
                    assert command.s.equals("go");
                    stack.push(new Command("print", command.node));
                    if(command.node.right != null)
                        stack.push(new Command("go",command.node.right));
                    if(command.node.left != null)
                        stack.push(new Command("go",command.node.left));
                }
            }
            return res;
        }
    }
    ```
    ```java
    public List<Integer> postorderTraversal(TreeNode root) {
        LinkedList<Integer> res = new LinkedList<>();
        ArrayDeque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root;
        while(!stack.isEmpty() || cur != null) {
            if(cur != null) {
                stack.push(cur);
                res.addFirst(cur.val);  // 利用链表结构倒序添加
                cur = cur.right;
            } else {
                cur = stack.pop().left;   
            }
        }
        return res;
    }
    ```

### 1.7. 双栈排序

[《CC 150 3.6》](https://www.nowcoder.com/questionTerminal/d0d0cddc1489476da6b782a6301e7dec)

- Question

  编写程序对一个栈进行排序（最大元素在栈顶），要求最多只能使用栈作为辅助空间的数据结构，且该栈只支持 isEmpty、push、pop 和 peek 操作。

- Solution
  - 解法 1：使用 1 个辅助栈，时间效率为 O(n^2)，空间效率为 O(n)。

    假设有 2 个栈 s1 和 s2，令 s1 是无序栈，s2 是有序栈（结果栈）：
    1. 对 s1 执行 pop 操作，并将该栈顶元素存储到一个临时变量 tmp 中。
    1. 对 s2 不断执行 pop 操作，并将 pop 的元素 push 到 s1 中，直到 s2 为空或者 pop 的元素小于 tmp，则把 tmp 再 push 到 s2 中。
    1. 重复以上步骤。

    例如：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/17/ff32d738146875ce65fb5ddf18c4c3d6.jpg)

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/10/17/53bc29f9c9a531b999e4ea95371c15b7.jpg)

    ```java
    public void Stack<Integer> sort(Stack<Integer> s) {
      Stack<Integer> res = new Stack<>();
      while (!s.isEmpty()) {
        int tmp = s.pop();
        while (!res.isEmpty() && res.peek() > tmp)
          s.push(res.pop());
        res.push(tmp);
      }
      return res;
    }
    ```

  - 解法 2：使用多个辅助栈，时间效率为 O(nlog n)，空间效率为 O(n^n)。
    - QuickSort：在每一层递归中，创建 2 个辅助栈，并根据 pivot element 将原栈的元素分到 2 个辅助栈中。进行递归排序后，将所有栈的元素归并放回原栈。
    - MergeSort：在每一层递归中，创建 2 个辅助栈，递归排序每个栈，然后再把它们归并到一起排好序，放回原来的栈。

## 2. Queue

### 2.1. 实现一个队列

```java
// based on linked list
public class MyQueue<T> {
  private Node<T> head, tail; // head -> ... -> ... -> tail

  public enqueue(T data) {
    if (head == null) {
      tail = new Node<>(data);
      head = tail;
    } else {
      tail.next = new Node<>(data);
      tail = tail.next;
    }
  }

  public dequeue() {
    if (head == null)
      return null;
    T data = head.data;
    head = head.next;
    return data;
  }

}
```

### 2.2. 二叉树的层序遍历

#### 2.2.1. Binary Tree Level Order Traversal

[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

- Question
  > Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

  For example:
  ```
  Given binary tree [3,9,20,null,null,15,7],
      3
    / \
    9  20
      /  \
    15   7
  return its level order traversal as:
  [
    [3],
    [9,20],
    [15,7]
  ]
  ```
- Solution
  ```java
  /// 时间复杂度：O(n), n 为树的节点个数
  /// 空间复杂度：O(n)
  public List<List<Integer>> levelOrder(TreeNode root) {
      Queue<TreeNode> queue = new LinkedList<>();
      List<List<Integer>> wrapList = new LinkedList<>();
      if(root == null) 
          return wrapList;
      queue.offer(root);
      while(!queue.isEmpty()) {
          int levelNum = queue.size();
          List<Integer> subList = new LinkedList<>();
          for(int i=0; i<levelNum; i++) {
              if(queue.peek().left != null) 
                  queue.offer(queue.peek().left);
              if(queue.peek().right != null) 
                  queue.offer(queue.peek().right);
              subList.add(queue.poll().val);
          }
          wrapList.add(subList);
      }
      return wrapList;
  }
  ```

#### 2.2.2. Binary Tree Level Order Traversal Ⅱ

[107. Binary Tree Level Order Traversal Ⅱ](https://leetcode.com/problems/binary-tree-level-order-traversal-ii)

#### 2.2.3. Binary Tree Zigzag Level Order Traversal

[103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal)

#### 2.2.4. Binary Tree Right Side View

[199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view)

### 2.3. 无权图的最短路径

#### 2.3.1. Perfect Squares

[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

- Question
  > Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.
  > 
  > For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.

- Solution

  对问题进行建模，可将整个问题转化为图论问题：**从 n 到 0 的每个数字表示一个节点，若任意两个数字 x 和 y 相差一个完全平方数，则由大的数向小的数连接一条边，从而可以得到一个有向无权图**。原问题便转化为：求这个图中从 n 到 0 的最短路径。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/26/3d1d1e5cc41f585032561c6a5612f417.jpg)

  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(n)
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

#### 2.3.2. Word Ladder

[127. Word Ladder](https://leetcode.com/problems/word-ladder/description/)

- Question
  > Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord.
  Example 1:
  ```
  Input:
  beginWord = "hit",
  endWord = "cog",
  wordList = ["hot","dot","dog","lot","log","cog"]

  Output: 5

  Explanation: As one shortest transformation is "hit" -> "hot" -> "dot" -> "dog" -> "cog",
  return its length 5.
  ```

- Solution

  同样构造一个无向图，通过 BFS 即可求解。
  ```java
  public int ladderLength(String beginWord, String endWord, Set<String> wordList) {
      Set<String> beginSet = new HashSet<String>(), endSet = new HashSet<String>();

      int len = 1;
      int strLen = beginWord.length();
      HashSet<String> visited = new HashSet<String>();

      beginSet.add(beginWord);
      endSet.add(endWord);
      while (!beginSet.isEmpty() && !endSet.isEmpty()) {
          if (beginSet.size() > endSet.size()) {
              Set<String> set = beginSet;
              beginSet = endSet;
              endSet = set;
          }

          Set<String> temp = new HashSet<String>();
          for (String word : beginSet) {
              char[] chs = word.toCharArray();

              for (int i = 0; i < chs.length; i++) {
                  for (char c = 'a'; c <= 'z'; c++) {
                      char old = chs[i];
                      chs[i] = c;
                      String target = String.valueOf(chs);

                      if (endSet.contains(target)) {
                          return len + 1;
                      }

                      if (!visited.contains(target) && wordList.contains(target)) {
                          temp.add(target);
                          visited.add(target);
                      }
                      chs[i] = old;
                  }
              }
          }

          beginSet = temp;
          len++;
      }

      return 0;
  }
  ```

    TODO: 双向 BFS 求解

#### 2.3.3. Word Ladder II

[126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/description/)

- Question
  
  在 Word Ladder 的基础上，返回整个变化的过程。

- Solution

## 3. Heap

### 3.1. Top K Frequent Elements

[347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/description/)

- Question
  > Given a non-empty array of integers, return the k most frequent elements.

  For example, Given [1,1,1,2,2,3] and k = 2, return [1,2].

  Note: 
  - You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
  - Your algorithm's time complexity must be better than O(n log n), where n is the array's size.
- Solution

  维护一个含有 k 个元素的最小堆。先遍历给定数组，使用 map 存储各个元素及其 frequency。然后再遍历 map，若 map 中遍历到的元素的 frequency 比最小堆中堆顶元素的 frequency 大，则移除最小堆中堆顶元素，并将新的元素加入最小堆。最终最小堆中的元素就是前 k 个出现频率最高的元素。

  ```java
  // 时间复杂度：O(nlogk)
  // 空间复杂度：O(n + k)
  import javafx.util.Pair;
  class Solution {
      public List<Integer> topKFrequent(int[] nums, int k) {
          assert k > 0;
          HashMap<Integer, Integer> freq = new HashMap<>();
          for (int n : nums) 
              freq.put(n, freq.getOrDefault(n, 0) + 1);
          assert k <= freq.size();
          PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<>(
              (Pair<Integer, Integer> p1, Pair<Integer, Integer> p2) -> {
                      if(p1.getKey() != p2.getKey())
                          return p1.getKey() - p2.getKey();
                      return p1.getValue() - p2.getValue();
              });
          for (Integer num : freq.keySet()) {
              int numFreq = freq.get(num);
              if (pq.size() == k) {
                  if (numFreq > pq.peek().getKey()) {
                      pq.poll();
                      pq.add(new Pair(numFreq, num));
                  }
              } else
                  pq.add(new Pair(numFreq, num));
          }
          ArrayList<Integer> res = new ArrayList<>();
          while(!pq.isEmpty())
              res.add(pq.poll().getValue());
          return res;
      }
  }
  ```

### 3.2. Merge k Sorted Lists

[23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/description/)

- Question
  > Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

  Example:
  ```
  Input:
  [
    1->4->5,
    1->3->4,
    2->6
  ]
  Output: 1->1->2->3->4->4->5->6
  ```
- Solution
  
  Compare every \text{k}k nodes (head of every linked list) and get the node with the smallest value. Extend the final sorted linked list with the selected nodes.
  ```java
  public ListNode mergeKLists(ListNode[] lists) {
      if (lists == null || lists.length == 0) 
          return null;
      PriorityQueue<ListNode> queue = new PriorityQueue<ListNode>(lists.length, (o1,o2) -> o1.val-o2.val);
      ListNode dummy = new ListNode(0);
      ListNode cur = dummy;
      for (ListNode node : lists)
          if (node != null)
              queue.add(node);
      while (!queue.isEmpty()){
          cur.next = queue.poll();
          cur = cur.next;
          if (cur.next!=null)
              queue.add(cur.next);
      }
      return dummy.next;
  }
  ```

## 4. Refer Links