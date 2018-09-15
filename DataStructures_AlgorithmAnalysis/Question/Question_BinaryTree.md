- [Binary Tree](#binary-tree)
	- [1. Binary Tree](#1-binary-tree)
		- [1.1. Maximum Depth of Binary Tree](#11-maximum-depth-of-binary-tree)
		- [1.2. Minimum Depth of Binary Tree](#12-minimum-depth-of-binary-tree)
		- [1.3. Invert Binary Tree](#13-invert-binary-tree)
		- [1.4. Same Tree](#14-same-tree)
		- [1.5. Symmetric Tree](#15-symmetric-tree)
		- [1.6. Count Complete Tree Nodes](#16-count-complete-tree-nodes)
		- [1.7. Path Sum](#17-path-sum)
		- [1.8. Sum of Left Leaves](#18-sum-of-left-leaves)
		- [1.9. Binary Tree Paths](#19-binary-tree-paths)
		- [1.10. Path Sum II](#110-path-sum-ii)
		- [1.11. Sum Root to Leaf Numbers](#111-sum-root-to-leaf-numbers)
		- [1.12. Path Sum III](#112-path-sum-iii)
		- [1.13. Lowest Common Ancestor of a Binary Tree (BT - LCA 问题)](#113-lowest-common-ancestor-of-a-binary-tree-bt---lca-%E9%97%AE%E9%A2%98)
	- [2. Binary Search Tree](#2-binary-search-tree)
		- [2.1. Lowest Common Ancestor of a Binary Search Tree (BST - LCA 问题)](#21-lowest-common-ancestor-of-a-binary-search-tree-bst---lca-%E9%97%AE%E9%A2%98)
		- [2.2. Validate Binary Search Tree](#22-validate-binary-search-tree)
		- [2.3. Delete Node in a BST](#23-delete-node-in-a-bst)
		- [2.4. Convert Sorted Array to Binary Search Tree](#24-convert-sorted-array-to-binary-search-tree)
		- [2.5. kth Smallest Element in a BST](#25-kth-smallest-element-in-a-bst)
	- [3. Balanced Binary Tree](#3-balanced-binary-tree)
		- [3.1. Balanced Binary Tree](#31-balanced-binary-tree)
	- [4. Refer Links](#4-refer-links)

# Binary Tree

## 1. Binary Tree

### 1.1. Maximum Depth of Binary Tree

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

### 1.2. Minimum Depth of Binary Tree

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

### 1.3. Invert Binary Tree

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
  / \   / 	\
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

### 1.4. Same Tree

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

### 1.5. Symmetric Tree

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
      
      private boolean _isSymmetric(TreeNode left, TreeNode right){
          if (left == null || right == null)
              return left == right;
          if (left.val == right.val)
              return _isSymmetric(left.left, right.right) 
            				&& _isSymmetric(left.right, right.left);
          return false;
      }
  }
  ```

### 1.6. Count Complete Tree Nodes

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

### 1.7. Path Sum

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
  ```
  return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
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

### 1.8. Sum of Left Leaves

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

### 1.9. Binary Tree Paths

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

### 1.10. Path Sum II

[113. Path Sum II](https://leetcode.com/problems/path-sum-ii/description/)

### 1.11. Sum Root to Leaf Numbers

[129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/description/)

### 1.12. Path Sum III

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

### 1.13. Lowest Common Ancestor of a Binary Tree (BT - LCA 问题)

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

### 2.2. Validate Binary Search Tree

[98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/description/)

- Question
	> Given a binary tree, determine if it is a valid binary search tree (BST).

- Solution
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
  > Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

- Solution
	
	<!-- TODO: 可使用快排 partition 的思想 -->
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
				if (k == 0) return n.val;
				TreeNode right = n.right;
				while (right != null) {
						st.push(right);
						right = right.left;
				}
		}

		return -1; // never hit if k is valid
	}
	```

## 3. Balanced Binary Tree

### 3.1. Balanced Binary Tree

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

## 4. Refer Links
