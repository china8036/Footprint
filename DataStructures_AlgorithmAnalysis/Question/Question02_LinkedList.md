- [LinkedList](#linkedlist)
  - [1. 链表反转](#1-链表反转)
    - [1.1. Reverse Linked List](#11-reverse-linked-list)
    - [1.2. Reverse Linked List II](#12-reverse-linked-list-ii)
    - [1.3. Swap Nodes in Pairs](#13-swap-nodes-in-pairs)
    - [1.4. Reverse Nodes in k-Group](#14-reverse-nodes-in-k-group)
  - [2. 删除节点](#2-删除节点)
    - [2.1. Remove Linked List Elements](#21-remove-linked-list-elements)
    - [2.2. Remove Duplicates from Sorted List II](#22-remove-duplicates-from-sorted-list-ii)
    - [2.3. Delete Node in a Linked List](#23-delete-node-in-a-linked-list)
  - [3. 链表排序](#3-链表排序)
    - [3.1. Insertion Sort List](#31-insertion-sort-list)
    - [3.2. Sort List](#32-sort-list)
  - [4. 快行指针](#4-快行指针)
    - [4.1. Remove Nth Node From End of List](#41-remove-nth-node-from-end-of-list)
    - [4.2. Rotate List](#42-rotate-list)
    - [4.3. Reorder List](#43-reorder-list)
    - [4.4. Palindrome Linked List](#44-palindrome-linked-list)
    - [4.5. 有环链表问题](#45-有环链表问题)
      - [4.5.1. 是否有环](#451-是否有环)
      - [4.5.2. 环路长度](#452-环路长度)
      - [4.5.3. 环路起点](#453-环路起点)
  - [5. 链表相交问题](#5-链表相交问题)
    - [5.1. 判断链表是否相交](#51-判断链表是否相交)
    - [5.2. 求相交链表的交点](#52-求相交链表的交点)
  - [6. 链表拆分与合并](#6-链表拆分与合并)
    - [6.1. 链表奇偶拆分](#61-链表奇偶拆分)
    - [6.2. 有序链表合并](#62-有序链表合并)
    - [6.3. 奇偶有序链表合并](#63-奇偶有序链表合并)
  - [7. 水池采样问题 (Reservoir Sampling Problem)](#7-水池采样问题-reservoir-sampling-problem)
    - [7.1. Linked List Random Node](#71-linked-list-random-node)
    - [7.2. Random Pick Index](#72-random-pick-index)
  - [8. Refer Links](#8-refer-links)

# LinkedList

链表操作类题目，非常依赖基础概念，从无到有实现链表也是一项基本要求。求解此类问题一般有以下技巧：
- 多指针操作

- 虚拟头节点

  在很多链表问题的操作中，往往需要对链表头节点做单独处理，增加了工作量。因此，可以设置一个虚拟的头节点，从而可以对链表中的所有节点进行统一处理。

- 快行指针

  快行指针指的是同时用两个指针来迭代访问链表，但其中一个比另一个超前一些，“快”可以是先行几步后同步前进，也可以是速度快。

  例：p1 每次向前移动 2 步，p2 每次向前移动 1 步。当 p1 到达链表末尾时，p2 刚好处于链表中间位置。

- 递归思路

  **许多链表问题都要用到递归。解决链表问题时，可以尝试使用递归思路进行求解**。

## 1. 链表反转

### 1.1. Reverse Linked List

[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

- Question
  > Reverse a singly linked list.

- Solution
  - 非原地解法：将链表复制到数组中，反转赋值即可。

  - 原地解法

    使用 pre、cur、next 三个指针：

    ![image](http://img.cdn.firejq.com/jpg/2018/4/25/abfdeffa6b2857f4e654591fb4625e70.jpg)

    ![image](http://img.cdn.firejq.com/jpg/2018/4/25/6e254b45c1db442f322ebf49630eb38d.jpg)

    ```java
    // 时间复杂度：O(n)
    // 空间复杂度：O(1)
    public class Solution1 {
        public class ListNode {
            int val;
            ListNode next;
            ListNode(int x) { val = x; }
        }
        public ListNode reverseList(ListNode head) {
            ListNode pre = null;
            ListNode cur = head;
            while (cur != null){
                ListNode next = cur.next;
                cur.next = pre;
                pre = cur;
                cur = next;
            }
            return pre;
        }
    }
    ```

### 1.2. Reverse Linked List II

[92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/description/)

- Question
  > Reverse a linked list from position m to n. Do it in one-pass.

  Note: 1 ≤ m ≤ n ≤ length of list.

  Example:
  ```
  Input: 1->2->3->4->5->NULL, m = 2, n = 4
  Output: 1->4->3->2->5->NULL
  ```
- Solution

  TODO:

### 1.3. Swap Nodes in Pairs

[24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

- Question
  > Given a linked list, swap every two adjacent nodes and return its head.

  Example:

  Given 1->2->3->4, you should return the list as 2->1->4->3.

- Solution

  假设有 P->A->B->C->... 链表，交换 A、B 节点即把 A.next 指向 C，B.next 指向 A，P.next 指向 B:

  ![image](http://img.cdn.firejq.com/jpg/2018/11/6/39d14d29e8da08285572976829c3d6ee.jpg)

  在实现时，可以**在头部添加一个空结点（虚拟头结点）**。
  ```java
  class Solution {
      public ListNode swapPairs(ListNode head) {
          ListNode dummyHead = new ListNode(0);
          dummyHead.next = head;
          ListNode p = dummyHead;
          while(p.next != null && p.next.next != null) {
              ListNode node1 = p.next;
              ListNode node2 = node1.next;
              ListNode next = node2.next;
              node2.next = node1;
              node1.next = next;
              p.next = node2;
              p = node1;
          }
          return dummyHead.next;
      }
  }
  ```

### 1.4. Reverse Nodes in k-Group

[25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

- Question
  > Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.
  >
  > k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

  Example:

  Given this linked list: 1->2->3->4->5

  For k = 2, you should return: 2->1->4->3->5

  For k = 3, you should return: 3->2->1->4->5

- Solution

  TODO:
  ```java
  public ListNode reverseKGroup(ListNode head, int k) {
      ListNode curr = head;
      int count = 0;
      while (curr != null && count != k) { // find the k+1 node
          curr = curr.next;
          count++;
      }
      if (count == k) { // if k+1 node is found
          curr = reverseKGroup(curr, k); // reverse list with k+1 node as head
          // head - head-pointer to direct part,
          // curr - head-pointer to reversed part;
          while (count-- > 0) { // reverse current k-group:
              ListNode tmp = head.next; // tmp - next head in direct part
              head.next = curr; // preappending "direct" head to the reversed list
              curr = head; // move head of reversed part to a new node
              head = tmp; // move "direct" head to the next node in direct part
          }
          head = curr;
      }
      return head;
  }
  ```

## 2. 删除节点

### 2.1. Remove Linked List Elements

[203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/description/)

- Question
  > Remove all elements from a linked list of integers that have value val.

  Example
  ```
  Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
  Return: 1 --> 2 --> 3 --> 4 --> 5
  ```
- Solution

  删除操作：

  ![image](http://img.cdn.firejq.com/jpg/2018/4/25/93b94c7e4e55cc3165f8c11ccbec77cf.jpg)

  由示意图可以看出，该操作逻辑不适用于要删除头节点的情况，需要对头节点进行单独处理：
  ```java
  public ListNode removeElements(ListNode head, int val) {
      // 需要对头结点进行特殊处理
      while(head != null && head.val == val){
          ListNode node = head;
          head = head.next;
      }
      if(head == null)
          return head;
      ListNode cur = head;
      while(cur.next != null){
          if(cur.next.val == val){
              ListNode delNode = cur.next;
              cur.next = delNode.next;
          }
          else
              cur = cur.next;
      }
      return head;
  }
  ```
  因此，可以设置虚拟头节点来统一处理：

  ![image](http://img.cdn.firejq.com/jpg/2018/4/25/fc9d52faf357c04fbb967a0fe4340cad.jpg)

  ```java
  public ListNode removeElements(ListNode head, int val) {
      ListNode dummyHead = new ListNode(0); // 创建虚拟头结点
      dummyHead.next = head;

      for (ListNode cur = dummyHead; cur.next != null; cur = cur.next)
          if (cur.next.val == val)
              cur.next = cur.next.next;
      return dummyHead.next;
  }
  ```

### 2.2. Remove Duplicates from Sorted List II

[82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/description/)

### 2.3. Delete Node in a Linked List

[237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/description/)

- Question
  > Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.
  >
  > Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.
- Solution

  **常规的删除操作中，需要至少获取 pre、cur 两个指针才能进行，而此题中只给出了待删除节点的 cur 指针，因此可以将 cur 后继节点向前移动一位，再删除多余的后继节点即可**：

  ![image](http://img.cdn.firejq.com/jpg/2018/4/25/06e71d5b135cb2100598b6818ea17d16.jpg)

  ![image](http://img.cdn.firejq.com/jpg/2018/4/25/11c73cde0d2a7c16b846e5671cf7c122.jpg)

  **需要注意的是，该方法不适用于尾节点。若待删除节点刚好是尾结点，则此问题无解，但可通过将尾结点作特殊标记来达到“删除”的效果**。

  ```java
  // 时间复杂度：O(1)
  // 空间复杂度：O(1)
  public void deleteNode(ListNode node) {
      // 如果为 null 则抛出异常，确保了 node 不是尾节点
      if(node == null || node.next == null)
          throw new IllegalArgumentException("node should be valid and can not be the tail node.");

      node.val = node.next.val;
      node.next = node.next.next;
  }
  ```

## 3. 链表排序

### 3.1. Insertion Sort List

[147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/description/)

### 3.2. Sort List

[148. Sort List](https://leetcode.com/problems/sort-list/description/)

可使用自底向上的归并排序思路进行排序。

## 4. 快行指针

### 4.1. Remove Nth Node From End of List

[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

- Question
  > Given a linked list, remove the n-th node from the end of list and return its head.

  Example:
  ```
  Given linked list: 1->2->3->4->5, and n = 2.

  After removing the second node from the end, the linked list becomes 1->2->3->5.
  ```
  Note:

  Given n will always be valid.

  Follow up:

  Could you do this in one pass?
- Solution

  使用 p1 和 p2 两个指针，且 p2 先行于 p1 n 步，则当 p2 到达链表末尾时，p1 刚好处于倒数第 n 个元素。
  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(1)
  public ListNode removeNthFromEnd(ListNode head, int n) {
      ListNode dummyHead = new ListNode(0);
      dummyHead.next = head;
      ListNode p = dummyHead;
      ListNode q = dummyHead;
      for( int i = 0 ; i < n + 1 ; i ++ ) {// 先行 n+1 步
          assert q != null;
          q = q.next;
      }
      while(q != null){
          p = p.next;
          q = q.next;
      }
      p.next = p.next.next;
      return dummyHead.next;
  }
  ```

### 4.2. Rotate List

[61. Rotate List](https://leetcode.com/problems/rotate-list/description/)

### 4.3. Reorder List

[143. Reorder List](https://leetcode.com/problems/reorder-list/description/)

### 4.4. Palindrome Linked List

[234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/description/)

### 4.5. 有环链表问题

#### 4.5.1. 是否有环

- Question
  > 给定一个单向链表，编写程序判断该链表是否有环。

- Solution

  定义 2 个指针 fast 和 slow，同时从链表的头节点出发，**slow 指针走 1 步 / 次，fast 指针走 2 步 / 次**：
  - 如果 fast 指针撞上了 slow 指针，那么说明该链表中有环。
  - 如果 fast 指针走到了链表的末尾（next 指向 NULL）都没有撞上 slow 指针，那么说明该链表中没有环。

  证明：
  1. 在一个环形赛道上，方向相同而速度不相同的 2 辆车迟早会相遇。
  1. 速度快多少才能保证一定刚好“追上”而不是刚好“越过”？相差 1 步。因为如果 fast 真的刚好“越过”了 slow，假设 fast 位于 `i` 位置，slow 位于 `i-1` 位置，那么可知在上一个单位时间，fast 就位于 `i-2` 位置，而 slow 位于 `i-1-1 = i-2`，也就是说，在上一个单位时间 2 者就已经“追上”了。

#### 4.5.2. 环路长度

- Question
  > 给定一个有环链表，遍写程序返回环路的长度。

- Solution

  定义 2 个指针 fast 和 slow，同时从链表的头节点出发，**slow 指针走 1 步 / 次，fast 指针走 2 步 / 次**，由于链表有环，fast 和 slow 会不断相遇，那么 fast 和 slow 第一次相遇和第二次相遇之间，slow 走过的节点处即为环的长度。

#### 4.5.3. 环路起点

- Question
  > 给定一个有环链表，编写程序返回环路的起点（开头节点）。

- Solution

  定义 2 个指针 fast 和 slow，同时从链表的头节点出发，**slow 指针走 1 步 / 次，fast 指针走 2 步 / 次**，由于链表有环，fast 和 slow 会不断相遇，当 fast 和 slow 第一次相遇时，将 slow 指向链表头部 head，那么**此时 fast 和 slow 离环路起点的距离是相同的**，因此让 fast 和 slow 以相同速度 1 步 / 次前进，第二次相遇的位置即为环路的起点。
  ```java
  public Node findBeginning(Node head) {
    Node fast = head;
    Node slow = head;

    while (fast != null && fast.next != null) {
      slow = slow.next;
      fast = fast.next.next;
      if (slow == fast)
        break;
    }

    if (fast == null || fast.next == null) // no loop
      return null;

    slow = head;
    while (slow != fast) {
      slow = slow.next;
      fast = fast.next;
    }

    return fast;
  }
  ```

## 5. 链表相交问题

假设两个单链表**只存在 Y 型交叉，不会存在 X 型交叉**，即：

![image](http://img.cdn.firejq.com/jpg/2018/10/19/ba560e8520787dcb3cbbc7227c1e478d.jpg)

### 5.1. 判断链表是否相交

[如何高效地判断两个单链表是否有交叉？](https://www.zhihu.com/question/20218641)

- 解法 1：先遍历链表 A，遍历链表 A 的时候将 node 加入到一个 hash 表或者二叉树中。之后遍历链表 B 的 node 时，检查该 node 是否存在在数据结构中对应的位置就可以了。时间复杂度为 O(n+m)，空间复杂度为 O(n+m)。

- 解法 2：遍历两个链表分别知道两个表的长度 n, m。然后让长表先走 `|n-m|` 步后，短表再开始走，如果有（内存地址）相同的节点并且不为空，则说明这两个链表是相交的。时间复杂度为 o(m+n)，空间复杂度为 O(1)。

### 5.2. 求相交链表的交点

[160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/description/)

- 解法 1：将其中一个链表的头尾相连，那么可以将问题转化为求另一个链表中环的开始节点，使用快行指针（fast 走 2 步 / 次，slow 走 1 步 / 次）即可。

- 解法 2：用两个栈分别记录两个链表的节点，再弹出，找到最后一个相等的节点，即为这两个链表的交点。

- 解法 3：将长的链表移动长度差的距离，然后同时移动两个链表，找到第一个相等的节点，即为这两个链表的交点。

## 6. 链表拆分与合并

### 6.1. 链表奇偶拆分

[328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)

- Question
  > Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.
  >
  > You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

  Example 1:
  ```
  Input: 1->2->3->4->5->NULL
  Output: 1->3->5->2->4->NULL
  ```
  Example 2:
  ```
  Input: 2->1->3->5->6->4->7->NULL
  Output: 2->3->6->7->1->5->4->NULL
  ```

- Solution

TODO:

### 6.2. 有序链表合并

[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists)

- Question
  > Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

  Example:
  ```
  Input: 1->2->4, 1->3->4
  Output: 1->1->2->3->4->4
  ```

- Solution

  递归实现：
  ```java
  public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
      if (l1 == null)
        return l2;
      if (l2 == null)
        return l1;
      if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
      } else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
      }
  }
  ```
  非递归实现：
  ```java
  public ListNode mergeTwoLists(ListNode head1, ListNode head2) {
      if (head1 == null || head2 == null)
          return head1 != null ? head1 : head2;
      Node head = new Node();
      Node cur = head;
      while (head1 != null && head2 != null) {
          if (head1.value >= head2.value) {
              cur.next = head2;
              head2 = head2.next;
          } else {
              cur.next = head1;
              head1 = head1.next;
          }
          cur = cur.next;
      }
      if (head1 != null)
          cur.next = head1;
      if (head2 != null)
          cur.next = head2;
      return head.next;
  }
  ```

### 6.3. 奇偶有序链表合并

- Question
  >  一个单链表，奇数位置升序，偶数位置降序，将这个链表调整为整体升序。如：1 8 3 6 5 4 7 2 9，最后输出 1 2 3 4 5 6 7 8 9。

- Solution

  分成三步：
  1. 首先根据奇数位和偶数位拆分成两个链表。
  1. 然后对偶数链表进行反转。
  1. 最后将两个有序链表进行合并。

  ```java
  private static Node[] divideList(Node head) {
      // 头结点不保存实际的元素
      Node head1 = new Node();
      Node head2 = new Node();
      Node cur1 = head1;
      Node cur2 = head2;
      int cnt = 1;// 用来计数
      while (head != null) {
          if (cnt % 2 != 0) {
              cur1.next = head;
              cur1 = cur1.next;
          } else {
              cur2.next = head;
              cur2 = cur2.next;
          }
          head = head.next;
          cnt++;
      }
      // 最后要让两个末尾元素的下一个都指向 null，否则奇数个的时候，后面可能带有剩下的结点
      cur1.next = null;
      cur2.next = null;
      return new Node[] { head1.next, head2.next };
  }

  private static Node reverseList(Node head) {
      if (head == null || head.next == null) {
          return head;
      }
      Node pre = null;
      Node behind = null;
      while (head.next != null) {
          behind = head.next;
          head.next = pre;
          pre = head;
          head = behind;
      }
      head.next = pre;
      return head;
  }

  private static Node combineList(Node l1, Node l2) {
      if (l1 == null)
        return l2;
      if (l2 == null)
        return l1;
      if (l1.val < l2.val) {
        l1.next = combineList(l1.next, l2);
        return l1;
      } else {
        l2.next = combineList(l1, l2.next);
        return l2;
      }
  }
  ```

## 7. 水池采样问题 (Reservoir Sampling Problem)

[Stackoverflow: Efficiently selecting a set of random elements from a linked list](https://stackoverflow.com/questions/54059/efficiently-selecting-a-set-of-random-elements-from-a-linked-list)

[Reservoir Sampling 原理动画演示](https://www.youtube.com/watch?v=A1iwzSew5QY)

[单链表，只让过一次，返回随机元素](https://leetcodenotes.wordpress.com/2013/08/20/phone-%E5%8D%95%E9%93%BE%E8%A1%A8%EF%BC%8C%E5%8F%AA%E8%AE%A9%E8%BF%87%E4%B8%80%E6%AC%A1%EF%BC%8C%E8%BF%94%E5%9B%9E%E9%9A%8F%E6%9C%BA%E5%85%83%E7%B4%A0/)

[链表随机节点](https://www.unclegem.cn/2018/09/07/Leetcode%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-382-%E9%93%BE%E8%A1%A8%E9%9A%8F%E6%9C%BA%E8%8A%82%E7%82%B9/)

[未知链表长度的情况下从中随机取 k 个数](https://www.cnblogs.com/longdouhzt/archive/2012/02/28/2371480.html)

问题原型：有一个很大的水池（不知道有多大，设为 n 桶水），设计一个算法从水池里随机取 k 桶水，概率为 k/n。

问题变形：有一个单向链表，只允许遍历一次（因此不能先把 size 找出来再 rand(size)），设计一个算法从链表中随机取一个元素。

思路解析：**先将前 k 个数取出来放入结果集中，然后从第 k+1 个数开始遍历。假设遍历到第 i 个数，以 k / i 的概率替换掉蓄水池中的某个元素即可**。

思路证明：
```
1/i * (1 - 1/(i+1)) * (1 - 1/(i+2)) * (1 - 1/(i+3)) * ... * (1 - 1/n)
= 1/i * i/(i+1) * (i+1)/(i+2) * (i+2)/(i+3) * ... * (n-1)/n
= 1/n
```

实际上，这个方法的思路属于[蒙特卡罗 (Monte Carlo) 算法](http://www.ruanyifeng.com/blog/2015/07/monte-carlo-method.html)。
> 举个例子，假如筐里有 100 个苹果，让我每次闭眼拿 1 个，挑出最大的。于是我随机拿 1 个，再随机拿 1 个跟它比，留下大的，再随机拿 1 个……我每拿一次，留下的苹果都至少不比上次的小。拿的次数越多，挑出的苹果就越大，但我除非拿 100 次，否则无法肯定挑出了最大的。这个挑苹果的算法，就属于蒙特卡罗算法，即**尽量找好的，但不保证是最好的**。

### 7.1. Linked List Random Node

[382. Linked List Random Node](https://leetcode.com/problems/linked-list-random-node)

- Question
  > Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

- Solution
  ```java
  class Solution {
      private ListNode head;

      private Random random;

      public Solution(ListNode head) {
          this.head = head;
          this.random = new Random();
      }

      public int getRandom() {
          ListNode cur = head;
          int res = cur.val;
          for (int i = 1; cur.next != null; i++) {
              cur = cur.next;
              if (random.nextInt(i + 1) == i)
                  res = cur.val;
          }
          return res;
      }
  }
  ```

### 7.2. Random Pick Index

[398. Random Pick Index](https://leetcode.com/problems/random-pick-index/)

## 8. Refer Links