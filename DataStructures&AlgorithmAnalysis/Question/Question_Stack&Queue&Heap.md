- [Stack & Queue & Heap](#stack-queue-heap)
  - [1. Stack](#1-stack)
    - [1.1. Valid Parentheses](#11-valid-parentheses)
    - [1.2. Evaluate Reverse Polish Notation](#12-evaluate-reverse-polish-notation)
    - [1.3. Simplify Path](#13-simplify-path)
    - [1.4. 二叉树的 DFS](#14--dfs)
      - [1.4.1. Binary Tree Preorder Traversal](#141-binary-tree-preorder-traversal)
      - [1.4.2. Binary Tree Inorder Traversal](#142-binary-tree-inorder-traversal)
      - [1.4.3. Binary Tree Postorder Traversal](#143-binary-tree-postorder-traversal)
  - [2. Queue](#2-queue)
    - [2.1. 二叉树的层序遍历](#21)
      - [2.1.1. Binary Tree Level Order Traversal](#211-binary-tree-level-order-traversal)
      - [2.1.2. Binary Tree Level Order Traversal Ⅱ](#212-binary-tree-level-order-traversal)
      - [2.1.3. Binary Tree Zigzag Level Order Traversal](#213-binary-tree-zigzag-level-order-traversal)
      - [2.1.4. Binary Tree Right Side View](#214-binary-tree-right-side-view)
    - [2.2. 无权图的最短路径](#22)
      - [2.2.1. Perfect Squares](#221-perfect-squares)
      - [2.2.2. Word Ladder](#222-word-ladder)
      - [2.2.3. Word Ladder II](#223-word-ladder-ii)
  - [3. Heap](#3-heap)
    - [3.1. Top K Frequent Elements](#31-top-k-frequent-elements)
    - [3.2. Merge k Sorted Lists](#32-merge-k-sorted-lists)
  - [4. Refer Links](#4-refer-links)

# Stack & Queue & Heap

## 1. Stack

### 1.1. Valid Parentheses

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

### 1.2. Evaluate Reverse Polish Notation

[150. Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

逆波兰表达式求值问题

### 1.3. Simplify Path

[71. Simplify Path](https://leetcode.com/problems/simplify-path/description/)

### 1.4. 二叉树的 DFS

#### 1.4.1. Binary Tree Preorder Traversal

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
    /// 时间复杂度：O(n), n 为树的节点个数
    /// 空间复杂度：O(h), h 为树的高度
    public class Solution144 {
        public List<Integer> preorderTraversal(TreeNode root) {
            ArrayList<Integer> res = new ArrayList<>();
            preorderTraversal(root, res);
            return res;
        }

        private void preorderTraversal(TreeNode node, List<Integer> list){
            if(node != null) {
                list.add(node.val);
                preorderTraversal(node.left, list);
                preorderTraversal(node.right, list);
            }
        }
    }
    ```
  - 非递归（利用 stack）
    ```java
    /// 时间复杂度：O(n), n 为树的节点个数
    /// 空间复杂度：O(h), h 为树的高度
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

#### 1.4.2. Binary Tree Inorder Traversal

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
    /// 时间复杂度：O(n), n 为树的节点个数
    /// 空间复杂度：O(h), h 为树的高度
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
    /// 时间复杂度：O(n), n 为树的节点个数
    /// 空间复杂度：O(h), h 为树的高度
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
    
#### 1.4.3. Binary Tree Postorder Traversal

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
    /// 时间复杂度：O(n), n 为树的节点个数
    /// 空间复杂度：O(h), h 为树的高度
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
    /// 时间复杂度：O(n), n 为树的节点个数
    /// 空间复杂度：O(h), h 为树的高度
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
    
## 2. Queue

### 2.1. 二叉树的层序遍历

#### 2.1.1. Binary Tree Level Order Traversal

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
      while(!queue.isEmpty()){
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

#### 2.1.2. Binary Tree Level Order Traversal Ⅱ

[107. Binary Tree Level Order Traversal Ⅱ](https://leetcode.com/problems/binary-tree-level-order-traversal-ii)

#### 2.1.3. Binary Tree Zigzag Level Order Traversal

[103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal)

#### 2.1.4. Binary Tree Right Side View

[199.	Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view)

### 2.2. 无权图的最短路径

#### 2.2.1. Perfect Squares

[279. Perfect Squares](https://leetcode.com/problems/perfect-squares/description/)

- Question
  > Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.

  For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.

- Solution

  对问题进行建模，可将整个问题转化为图论问题：从 n 到 0 的每个数字表示一个节点，若任意两个数字 x 和 y 相差一个完全平方数，则由大的数向小的数连接一条边，从而可以得到一个有向无权图。原问题便转化为：求这个图中从 n 到 0 的最短路径。

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

#### 2.2.2. Word Ladder

[127. Word Ladder](https://leetcode.com/problems/word-ladder/description/)

#### 2.2.3. Word Ladder II

[126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/description/)

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