- [Binary Tree & Graphs](#binary-tree--graphs)
  - [1. Binary Tree](#1-binary-tree)
    - [1.1. Unique Binary Search Trees](#11-unique-binary-search-trees)
    - [1.2. 层序遍历](#12-层序遍历)
      - [1.2.1. Binary Tree Level Order Traversal](#121-binary-tree-level-order-traversal)
      - [1.2.2. Populating Next Right Pointers in Each Node](#122-populating-next-right-pointers-in-each-node)
      - [1.2.3. Populating Next Right Pointers in Each Node Ⅱ](#123-populating-next-right-pointers-in-each-node-Ⅱ)
    - [1.3. Maximum Depth of Binary Tree](#13-maximum-depth-of-binary-tree)
    - [1.4. Minimum Depth of Binary Tree](#14-minimum-depth-of-binary-tree)
    - [1.5. Invert Binary Tree](#15-invert-binary-tree)
    - [1.6. Same Tree](#16-same-tree)
    - [1.7. Symmetric Tree](#17-symmetric-tree)
    - [1.8. Children Tree](#18-children-tree)
    - [1.9. Count Complete Tree Nodes](#19-count-complete-tree-nodes)
    - [1.10. Sum of Left Leaves](#110-sum-of-left-leaves)
    - [1.11. Binary Tree Paths](#111-binary-tree-paths)
    - [1.12. Sum Root to Leaf Numbers](#112-sum-root-to-leaf-numbers)
    - [1.13. Path Sum](#113-path-sum)
    - [1.14. Path Sum II](#114-path-sum-ii)
    - [1.15. Path Sum III](#115-path-sum-iii)
    - [1.16. Lowest Common Ancestor of a Binary Tree (BT - LCA 问题)](#116-lowest-common-ancestor-of-a-binary-tree-bt---lca-问题)
  - [2. Binary Search Tree](#2-binary-search-tree)
    - [2.1. Lowest Common Ancestor of a Binary Search Tree (BST - LCA 问题)](#21-lowest-common-ancestor-of-a-binary-search-tree-bst---lca-问题)
    - [2.2. 判断一棵二叉树是否是 BST](#22-判断一棵二叉树是否是-bst)
    - [2.3. Delete Node in a BST](#23-delete-node-in-a-bst)
    - [2.4. Convert Sorted Array to Binary Search Tree](#24-convert-sorted-array-to-binary-search-tree)
    - [2.5. kth Smallest Element in a BST](#25-kth-smallest-element-in-a-bst)
  - [3. Balanced Binary Tree](#3-balanced-binary-tree)
    - [3.1. 判断一棵二叉树是否平衡](#31-判断一棵二叉树是否平衡)
  - [4. Graphs](#4-graphs)
    - [4.1. 判断一个有向图是否有环](#41-判断一个有向图是否有环)
      - [4.1.1. DFS](#411-dfs)
      - [4.1.2. 拓扑排序](#412-拓扑排序)
    - [4.2. 判断一个无向图是否是树](#42-判断一个无向图是否是树)
      - [4.2.1. 并查集法](#421-并查集法)
      - [4.2.2. BFS/DFS 法](#422-bfsdfs-法)
    - [4.3. 推箱子](#43-推箱子)
  - [5. Refer Links](#5-refer-links)

# Binary Tree & Graphs

## 1. Binary Tree

### 1.1. Unique Binary Search Trees

[96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)

- Question
  > Given n, how many structurally unique BST's (binary search trees) that store values 1 ... n?

  Example:
  ```
  Input: 3
  Output: 5
  Explanation:
  Given n = 3, there are a total of 5 unique BST's:

    1         3     3      2      1
      \       /     /      / \      \
      3     2     1      1   3      2
      /     /       \                 \
    2     1         2                 3
  ```

- Solution

  该问题属于卡特兰数数列，使用 DP 解决：
  ```java
  public int numTrees(int n) {
      int [] dp = new int[n+1];
      dp[0] = 1;
      for (int i = 1; i <= n; i++)
          for (int j = 0; j < i; j++)
              dp[i] += dp[j] * dp[i - 1 - j];
      return dp[n];
  }
  ```

### 1.2. 层序遍历

#### 1.2.1. Binary Tree Level Order Traversal

[102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

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
  class Solution {
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
  }
  ```

#### 1.2.2. Populating Next Right Pointers in Each Node

[116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

- Question

  Given a binary tree
  ```
  struct TreeLinkNode {
    TreeLinkNode *left;
    TreeLinkNode *right;
    TreeLinkNode *next;
  }
  ```
  Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

  Initially, all next pointers are set to NULL.

  Example:
  ```
  Given the following perfect binary tree,

      1
    /  \
    2    3
  / \  / \
  4  5  6  7
  After calling your function, the tree should look like:

      1 -> NULL
    /  \
    2 -> 3 -> NULL
  / \  / \
  4->5->6->7 -> NULL
  ```

- Solution
  ```java
  public void connect(TreeLinkNode root) {
      TreeLinkNode level_start = root;
      while (level_start != null) {
          TreeLinkNode cur = level_start;
          while (cur != null) {
              if (cur.left != null)
                  cur.left.next = cur.right;
              if (cur.right != null && cur.next != null)
                  cur.right.next = cur.next.left;
              cur = cur.next;
          }
          level_start = level_start.left;
      }
  }
  ```

#### 1.2.3. Populating Next Right Pointers in Each Node Ⅱ

[117. Populating Next Right Pointers in Each Node Ⅱ](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

- Question

  Given a binary tree
  ```
  struct TreeLinkNode {
    TreeLinkNode *left;
    TreeLinkNode *right;
    TreeLinkNode *next;
  }
  ```
  Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

  Initially, all next pointers are set to NULL.

  Example:
  ```
  Given the following binary tree,

      1
    /  \
    2    3
  / \    \
  4   5    7
  After calling your function, the tree should look like:

      1 -> NULL
    /  \
    2 -> 3 -> NULL
  / \    \
  4-> 5 -> 7 -> NULL
  ```

- Solution

  ```java
  // based on level order traversal
  // O(1) space and O(n) Time complexity
  public void connect(TreeLinkNode root) {

      TreeLinkNode head = null; //head of the next level
      TreeLinkNode prev = null; //the leading node on the next level
      TreeLinkNode cur = root;  //current node of current level

      while (cur != null) {

          while (cur != null) { //iterate on the current level
              //left child
              if (cur.left != null) {
                  if (prev != null) {
                      prev.next = cur.left;
                  } else {
                      head = cur.left;
                  }
                  prev = cur.left;
              }
              //right child
              if (cur.right != null) {
                  if (prev != null) {
                      prev.next = cur.right;
                  } else {
                      head = cur.right;
                  }
                  prev = cur.right;
              }
              //move to next node
              cur = cur.next;
          }

          //move to next level
          cur = head;
          head = null;
          prev = null;
      }

  }
  ```

### 1.3. Maximum Depth of Binary Tree

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

- Question
  > Given a binary tree, find its maximum depth.
  >
  > The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

  NOTE: A leaf is a node with no children.

  Example:
  ```
  Given binary tree [3,9,20,null,null,15,7],

      3
    / \
    9  20
      /  \
    15   7
  ```
  return its depth = 3.
- Solution
  ```java
  public int maxDepth(TreeNode root) {
      return root == null ? 0 : Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
  }
  ```

### 1.4. Minimum Depth of Binary Tree

[111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/description/)

- Question
  > Given a binary tree, find its minimum depth.
  >
  > The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

  Note: A leaf is a node with no children.

  Example:
  ```
  Given binary tree [3,9,20,null,null,15,7],

      3
    / \
    9  20
      /  \
    15   7
  ```
  return its minimum depth = 2.
- Solution

  **需要特别注意递归终止条件：当左子树 / 右子树为空时，最小高度取决于非空的另一边**。
  ```java
  public int minDepth(TreeNode root) {
      if (root == null)
          return 0;
      if (root.left == null)
          return minDepth(root.right) + 1;
      if (root.right == null)
          return minDepth(root.left) + 1;
      return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
  }
  ```

### 1.5. Invert Binary Tree

[226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/)

- Question
  > Invert a binary tree.
  ```
      4
    /   \
    2     7
  / \   / 	\
  1   3 6   9
  ```
  to
  ```
      4
    /   \
    7     2
  / \   / \
  9   6 3   1
  ```
- Solution
  ```java
  // 返回翻转后的根结点
  public TreeNode invertTree(TreeNode root) {
      if (root == null)
          return null;
      TreeNode left = invertTree(root.left);
      TreeNode right = invertTree(root.right);
      root.left = right;
      root.right = left;
      return root;
  }
  ```

### 1.6. Same Tree

[100. Same Tree](https://leetcode.com/problems/same-tree/description/)

- Question
  > Given two binary trees, write a function to check if they are the same or not.
  >
  > Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

  Example 1:
  ```
  Input:     1         1
            / \       / \
          2   3     2   3

          [1,2,3],   [1,2,3]

  Output: true
  ```
  Example 2:
  ```
  Input:     1         1
            /           \
          2             2

          [1,2],     [1,null,2]

  Output: false
  ```
  Example 3:
  ```
  Input:     1         1
            / \       / \
          2   1     1   2

          [1,2,1],   [1,1,2]

  Output: false
  ```
- Solution
  ```java
  public boolean isSameTree(TreeNode p, TreeNode q) {
      if(p == null || q == null)
          return p == q;
      if(p.val == q.val)
          return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
      return false;
  }
  ```

### 1.7. Symmetric Tree

[101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/description/)

- Question
  > Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

  For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
  ```
      1
    / \
    2   2
  / \ / \
  3  4 4  3
  But the following [1,2,2,null,3,null,3] is not:
      1
    / \
    2   2
    \   \
    3    3
  ```
  Note: Bonus points if you could solve it both recursively and iteratively.
- Solution
  ```java
  class Solution {
      public boolean isSymmetric(TreeNode root) {
          return root == null || _isSymmetric(root.left, root.right);
      }

      private boolean _isSymmetric(TreeNode left, TreeNode right) {
          if (left == null || right == null)
              return left == right;
          if (left.val == right.val)
              return _isSymmetric(left.left, right.right) && _isSymmetric(left.right, right.left);
          return false;
      }
  }
  ```

### 1.8. Children Tree

- Question

  有 2 棵二叉树 T1 和 T2，T1 的节点数远大于 T2，编写程序判断 T2 是否是 T1 的子树，即是否存在节点 n，使得从 n 把 T1 砍断后得到的树与 T2 完全相同。

- Solution
  - 解法 1

    可以为每棵树创建 2 个字符串，分别表示前序遍历和中序遍历，若 T2 前序遍历是 T1 前序遍历的子串，且 T2 中序遍历是 T1 中序遍历的子串，则 T2 是 T1 的子树。其中，利用后缀树可以在 O(n) 时间内检查是否为子串；同时需要在节点为 null 时用特殊字符来表示，否则无法区分元素相同而形态不同的情况。

    这种解法存在的问题是，算法空间复杂度为 O(n+m)，如果树的节点数量非常大，会造成大量内存空间的占用。

  - 解法 2

    **遍历 T1，每当 T1 的某个节点与 T2 的根结点相同时，则调用 `treeMatch()` 进行匹配，即比较两颗子树是否完全相同**。时间效率为 O(n+km)，n 为 T1 的节点数，m 为 T2 的节点数，k 为 T2 根结点在 T1 中出现的次数。而事实上，在进行匹配时，一旦发现有节点不同就可以提前结束 `treeMatch()`，因此实际的时间效率会更高，空间复杂度为 O(logn+logm)。

### 1.9. Count Complete Tree Nodes

[222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/description/)

- Question
  > Given a complete binary tree, count the number of nodes.
- Solution
  ```java
  public int countNodes(TreeNode root) {
      int lh = 0, rh = 0;
      TreeNode left = root, right = root;
      while (left != null) {
          lh++;
          left = left.left;
      }
      while (right != null) {
          rh++;
          right = right.right;
      }
      if(lh == rh) // full binary tree
          return (1 << lh) - 1;
      return 1 + countNodes(root.left) + countNodes(root.right);
  }
  ```

### 1.10. Sum of Left Leaves

[404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/description/)

- Question
  > Find the sum of all left leaves in a given binary tree.

  Example:
  ```
      3
    / \
    9  20
      /  \
    15   7

  There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
  ```
- Solution
  ```java
  public int sumOfLeftLeaves(TreeNode root) {
      if (root == null)
          return 0;
      int res = 0;
      if (root.left != null) {
          if(root.left.left == null && root.left.right == null)
              res += root.left.val;
          else
              res += sumOfLeftLeaves(root.left);
      }
      res += sumOfLeftLeaves(root.right);
      return res;
  }
  ```

### 1.11. Binary Tree Paths

[257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/description/)

- Question

  > Given a binary tree, return all root-to-leaf paths.

  For example, given the following binary tree:
  ```
    1
  /   \
  2     3
  \
    5
  ```
  All root-to-leaf paths are:
  ```
  ["1->2->5", "1->3"]
  ```

- Solution
  ```java
  // 时间复杂度：O(n), n 为树中的节点个数
  // 空间复杂度：O(h), h 为树的高度
  public List<String> binaryTreePaths(TreeNode root) {
      List<String> paths = new LinkedList<>();
      if (root == null)
          return paths;
      if (root.left == null && root.right == null){
          paths.add(root.val + "");
          return paths;
      }
      for (String path : binaryTreePaths(root.left))
          paths.add(root.val + "->" + path);
      for (String path : binaryTreePaths(root.right))
          paths.add(root.val + "->" + path);

      return paths;
  }
  ```

### 1.12. Sum Root to Leaf Numbers

[129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/description/)

### 1.13. Path Sum

[112. Path Sum](https://leetcode.com/problems/path-sum/description/)

- Question
  > Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

  Note: A leaf is a node with no children.

  Example:
  ```
  Given the below binary tree and sum = 22,

        5
      / \
      4   8
    /   / \
    11  13  4
  /  \      \
  7    2      1
  return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
  ```

- Solution
  ```java
  public boolean hasPathSum(TreeNode root, int sum) {
      if (root == null)
          return false;
      if (root.left == null && root.right == null) // 注意终止条件
          return root.val == sum;
      return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
  }
  ```

### 1.14. Path Sum II

[113. Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)

### 1.15. Path Sum III

[437. Path Sum III](https://leetcode.com/problems/path-sum-iii/description/)

- Question
  > You are given a binary tree in which each node contains an integer value.
  >
  > Find the number of paths that sum to a given value.
  >
  > The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).
  >
  > The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

  Example:
  ```
  root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

        10
      /  \
      5   -3
    / \    \
    3   2   11
  / \   \
  3  -2   1

  Return 3. The paths that sum to 8 are:

  1.  5 -> 3
  2.  5 -> 2 -> 1
  3. -3 -> 11
  ```
- Solution
  ```java
  // 时间复杂度：O(n), n 为树的节点个数
  // 空间复杂度：O(h), h 为树的高度
  class Solution {
      // 在以 root 为根节点的二叉树中，寻找和为 sum 的路径，返回这样的路径个数
      public int pathSum(TreeNode root, int sum) {
          if (root == null)
              return 0;
          return findPath(root, sum)
                  + pathSum(root.left, sum)
                  + pathSum(root.right, sum);
      }

      // 在以 node 为根节点的二叉树中，寻找包含 node 的路径，和为 sum, 返回这样的路径个数
      private int findPath(TreeNode node, int num) {
          if (node == null)
              return 0;
          int res = 0;
          if (node.val == num)
              res += 1;
          res += findPath(node.left , num - node.val);
          res += findPath(node.right , num - node.val);
          return res;
      }
  }
  ```

### 1.16. Lowest Common Ancestor of a Binary Tree (BT - LCA 问题)

[236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree)

- Question
  > Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.
  >
  > According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

  Given the following binary tree:  root = [3,5,1,6,2,0,8,null,null,7,4]
  ```
         _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
  ```
  Example 1:
  ```
  Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
  Output: 3
  Explanation: The LCA of of nodes 5 and 1 is 3.
  ```

- Solution
  ```java
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
      if (root == null || root == p || root == q)
          return root;

      TreeNode left = lowestCommonAncestor(root.left, p, q);
      TreeNode right = lowestCommonAncestor(root.right, p, q);
      return left == null ? right : (right == null ? left : root);
  }
  ```

## 2. Binary Search Tree

### 2.1. Lowest Common Ancestor of a Binary Search Tree (BST - LCA 问题)

[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

- Question
  > Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.
  >
  > According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”
  ```
          _______6______
          /             \
      ___2__          ___8__
    /      \        /      \
    0      _4       7       9
          /  \
          3   5
  ```
  For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.

- Solution

  利用好 BST 有序性的特点：

  ```java
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
      while ((root.val - p.val) * (root.val - q.val) > 0)
          root = p.val < root.val ? root.left : root.right;
      return root;
  }
  ```

### 2.2. 判断一棵二叉树是否是 BST

[98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

- Question
  > Given a binary tree, determine if it is a valid binary search tree (BST).

- Solution
  - 解法 1：中序遍历判断，比较每一个元素与下一个元素是否有序。

  - 解法 2：最大最小值递归判断。
    ```java
    class Solution {
        public boolean isValidBST(TreeNode root) {
            return _isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
        }

        public boolean _isValidBST(TreeNode root, long minVal, long maxVal) {
            if (root == null)
                return true;
            if (root.val >= maxVal || root.val <= minVal)
                return false;
            return _isValidBST(root.left, minVal, root.val) && _isValidBST(root.right, root.val, maxVal);
        }
    }
    ```

### 2.3. Delete Node in a BST

[450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst)

- Question
  > Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

- Solution
  ```java
  class Solution {
      public TreeNode deleteNode(TreeNode root, int key) {
          if (root == null)
              return null;
          if (key < root.val)
              root.left = deleteNode(root.left, key);
          else if (key > root.val)
              root.right = deleteNode(root.right, key);
          else {
              if (root.left == null)
                  return root.right;
              else if (root.right == null)
                  return root.left;
              else {
                  TreeNode minNode = findMin(root.right);
                  root.val = minNode.val;
                  root.right = deleteNode(root.right, root.val);
              }
          }
          return root;
      }

      private TreeNode findMin(TreeNode node) {
          while (node.left != null)
              node = node.left;
          return node;
      }
  }
  ```

### 2.4. Convert Sorted Array to Binary Search Tree

[108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree)

- Question
  > Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

- Solution

  使用二分的思想：
  ```java
  class Solution {
      public TreeNode sortedArrayToBST(int[] num) {
          return num.length == 0 ? null : helper(num, 0, num.length - 1);
      }

      public TreeNode helper(int[] num, int low, int high) {
          if (low > high) // Done
              return null;

          int mid = (low + high) / 2;
          TreeNode node = new TreeNode(num[mid]);
          node.left = helper(num, low, mid - 1);
          node.right = helper(num, mid + 1, high);
          return node;
      }
  }
  ```

### 2.5. kth Smallest Element in a BST

[230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

- Question
  > Given a binary search tree, write a function kth Smallest to find the kth smallest element in it.

- Solution

  使用一个辅助栈：
  ```java
  public int kthSmallest(TreeNode root, int k) {
    Stack<TreeNode> st = new Stack<>();

    while (root != null) {
        st.push(root);
        root = root.left;
    }

    while (k != 0) {
        TreeNode n = st.pop();
        k--;
        if (k == 0)
            return n.val;
        TreeNode right = n.right;
        while (right != null) {
            st.push(right);
            right = right.left;
        }
    }

    return -1; // never hit if k is valid
  }
  ```

  <!-- TODO: 可使用快排 partition 的思想 -->

## 3. Balanced Binary Tree

### 3.1. 判断一棵二叉树是否平衡

[110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/description/)

- Question
  > Given a binary tree, determine if it is height-balanced.

  For this problem, a height-balanced binary tree is defined as:

  > a binary tree in which the depth of the two subtrees of every node never differ by more than 1.
  Example 1:
  ```
  Given the following tree [3,9,20,null,null,15,7]:

      3
    / \
    9  20
      /  \
    15   7
  Return true.
  ```
  Example 2:
  ```
  Given the following tree [1,2,2,3,3,null,null,4,4]:

        1
        / \
      2   2
      / \
    3   3
    / \
  4   4
  Return false.
  ```

- Solution

  **计算高度的同时，检查是否平衡，若不平衡则直接返回**。时间效率为 O(N)，空间效率为 O(H)。
  ```java
  class Solution {
      public boolean isBalanced(TreeNode root) {
          return checkHeight(root) == -1 ? false : true;
      }
      private int checkHeight(TreeNode root) {
          if (root == null)
              return 0;
          int lh = checkHeight(root.left);
          if (lh == -1)
              return -1;
          int rh = checkHeight(root.right);
          if (rh == -1)
              return -1;
          if (Math.abs(lh - rh) > 1)
              return -1;
          return Math.max(lh, rh) + 1;
      }
  }
  ```

## 4. Graphs

### 4.1. 判断一个有向图是否有环

#### 4.1.1. DFS

#### 4.1.2. 拓扑排序

### 4.2. 判断一个无向图是否是树

一个无向图要成为树，需要满足两个性质：
- 连通
- 无环

#### 4.2.1. 并查集法

无向图边的两端是对称的，无向图讲究连通这个概念，没有方向，没有拓扑，很像集合，所以非常适合用并查集来解决。

解决的方法：
1. 想象一开始这个图只有顶点，没有边，我们来一条一条的添加边。
1. 每遇到一条边，判断这边的两个端点是否在同一个集合里：
    - 不在的话，表示不构成环，我们应该合并这两个集合。
    - 在的话，表示有环，因为两个点在一个集合里就表示这两个点已经有一条路径了，现在再加一条路径，必然构成环。此时说明该无向图不是树。
1. 添加完所有边后，没有遇到成环的情况，且所有顶点在同一个集合中，说明该无向图是一棵树。

#### 4.2.2. BFS/DFS 法

DFS 过程中如果碰到访问过的节点（当然这个节点不能是来时的节点），就是有环。

### 4.3. 推箱子

[HDU 1254 推箱子](http://acm.hdu.edu.cn/showproblem.php?pid=1254)

- Question
  > 现在给定房间的结构、箱子的位置、搬运工的位置和箱子要被推去的位置，请你计算出搬运工至少要推动箱子多少格。

  ![image](http://img.cdn.firejq.com/jpg/2018/11/1/7dbaab7ce341c14d6cf8b58cf80819ea.jpg)

  Input: 第一行输入两个数字 N，M 表示地图的大小 (2<=M, N<=7)。接下来有 N 行，每行包含 M 个字符表示该行地图。其中 `.` 表示空地、`X` 表示玩家、`*` 表示箱子、`#` 表示障碍、`@` 表示目的地。每个地图必定包含 1 个玩家、1 个箱子、1 个目的地。

  Output: 输出一个数字表示玩家最少需要移动多少步才能将游戏目标达成。当无论如何达成不了的时候，输出 -1。

  eg:
  ```
  Input
  4 4
  ....
  ..*@
  ....
  .X..

  6 6
  ...#..
  ......
  #*##..
  ..##.#
  ..X...
  .@#...

  Output
  3
  11
  ```

- Solution

  - 解法一：双重 BFS。首先要判断箱子是否能到达目标点，这是主 BFS，箱子每走一步都要调用辅 BFS，即判断人是否能到达箱子所前进方向的反方向。在箱子的移动中，判重的时候需要一个三维数组，因为箱子从不同方向过来，人的位置是不一样的，也就意味着状态不一样。实现参考[这里的代码](https://blog.csdn.net/a601025382s/article/details/11832031)。

  - 解法二：四维数组标记 + BFS。

    ```java
    class State {
      State(int px, int py, int bx, int by) {
        this.px = px;
        this.py = py;
        this.bx = bx;
        this.by = by;
      }
      // 当前状态的玩家坐标
      int px;
      int py;
      // 当前状态的箱子坐标
      int bx;
      int by;
    }

    public class Main {
      private static int bfs(char[][] data) {
        int sx = 0, sy = 0; // 起始玩家坐标 startX startY
        int bx = 0, by = 0; // 起始箱子坐标 boxX boxY
        for (int i = 0; i < data.length; i++) {
          for (int j = 0; j < data[0].length; j++) {
            if (data[i][j] == 'X') { // 玩家
              sx = i;
              sy = j;
            } else if (data[i][j] == '*') { // 箱子
              bx = i;
              by = j;
            }
          }
        }

        int [][] next = {{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
        ArrayDeque<State> que = new ArrayDeque<>();
        que.add(new State(sx, sy, bx, by));

        int [][][][] history = new int[8][8][8][8];
        history[sx][sy][bx][by] = 1;

        while (!que.isEmpty()) {
          State cur = que.poll();
          for (int i = 0; i < 4; i++) {
            int nx = cur.px + next[i][0], ny = cur.py + next[i][1];
            int nnx = nx + next[i][0], nny = ny + next[i][1]; // 同向移动

            if (islegal(nx, ny, data) && (nx != cur.bx || ny != cur.by) && history[nx][ny][cur.bx][cur.by] == 0) { // 玩家从开始位置走到箱子的位置
              history[nx][ny][cur.bx][cur.by] = history[cur.px][cur.py][cur.bx][cur.by] + 1;
              que.add(new State(nx, ny, cur.bx, cur.by));
            } else if (nx == cur.bx && ny == cur.by && islegal(nnx, nny, data) && history[nx][ny][nnx][nny] == 0) { // 玩家把箱子推到指定位置
              history[nx][ny][nnx][nny] = history[cur.px][cur.py][cur.bx][cur.by] + 1;
              if (data[nnx][nny] == '@') // 到达终点
                return history[nx][ny][nnx][nny] - 1;
              que.add(new State(nx, ny, nnx, nny));
            }
          }
        }
        return -1;
      }

      private static boolean islegal(int x, int y, char [][] data) {
        int N = data.length;
        int M = data[0].length;
        if (x < 0 || x >= N || y < 0 || y >= M || data[x][y] == '#')
          return false;
        return true;
      }

      public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int N = sc.nextInt();
        int M = sc.nextInt();
        sc.nextLine();
        char[][] data = new char[N][M];
        for (int i = 0; i < N; i++) {
          String str = sc.nextLine();
          data[i] = str.toCharArray();
        }

        System.out.println(bfs(data));
      }
    }
    ```

## 5. Refer Links
