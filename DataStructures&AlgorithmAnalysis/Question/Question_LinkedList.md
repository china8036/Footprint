- [LinkedList](#linkedlist)
  - [1. TIP](#1-tip)
  - [2. 链表反转](#2)
    - [2.1. Reverse Linked List](#21-reverse-linked-list)
    - [2.2. Reverse Linked List II](#22-reverse-linked-list-ii)
  - [3. 删除节点](#3)
    - [3.1. Remove Linked List Elements](#31-remove-linked-list-elements)
    - [3.2. Remove Duplicates from Sorted List II](#32-remove-duplicates-from-sorted-list-ii)
    - [3.3. Delete Node in a Linked List](#33-delete-node-in-a-linked-list)
  - [4. 虚拟头节点](#4)
    - [4.1. Merge Two Sorted Lists](#41-merge-two-sorted-lists)
    - [4.2. Swap Nodes in Pairs](#42-swap-nodes-in-pairs)
    - [4.3. Reverse Nodes in k-Group](#43-reverse-nodes-in-k-group)
  - [5. 链表排序](#5)
    - [5.1. Insertion Sort List](#51-insertion-sort-list)
    - [5.2. Sort List](#52-sort-list)
  - [6. 快行指针](#6)
    - [6.1. Remove Nth Node From End of List](#61-remove-nth-node-from-end-of-list)
    - [6.2. Rotate List](#62-rotate-list)
    - [6.3. Reorder List](#63-reorder-list)
    - [6.4. Palindrome Linked List](#64-palindrome-linked-list)
  - [7. Refer Links](#7-refer-links)

# LinkedList

## 1. TIP

- 多指针操作

- 虚拟头节点

  在很多链表问题的操作中，往往需要对链表头节点做单独处理，增加了工作量。因此，可以设置一个虚拟的头节点，从而可以对链表中的所有节点进行统一处理。

- 快行指针

  快行指针指的是同时用两个指针来迭代访问链表，但其中一个比另一个超前一些，“快”可以是先行几步后同步前进，也可以是速度快。

  例：p1 每次向前移动 2 步，p2 每次向前移动 1 步。当 p1 到达链表末尾时，p2 刚好处于链表中间位置。

- 递归思路

  **许多链表问题都要用到递归。解决链表问题时，可以尝试使用递归思路进行求解**。

## 2. 链表反转

### 2.1. Reverse Linked List

[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/description/)

- Question
  > Reverse a singly linked list.
- Solution
  - 非原地解法：将链表复制到数组中，反转赋值即可。
  
  - 原地解法
    
    使用 pre、cur、next 三个指针：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/abfdeffa6b2857f4e654591fb4625e70.jpg)

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/6e254b45c1db442f322ebf49630eb38d.jpg)

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
            while(cur != null){
                ListNode next = cur.next;
                cur.next = pre;
                pre = cur;
                cur = next;
            }
            return pre;
        }
    }
    ```

### 2.2. Reverse Linked List II

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

## 3. 删除节点

### 3.1. Remove Linked List Elements

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

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/93b94c7e4e55cc3165f8c11ccbec77cf.jpg)

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

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/fc9d52faf357c04fbb967a0fe4340cad.jpg)
  
  ```java
  public ListNode removeElements(ListNode head, int val) {
      // 创建虚拟头结点
      ListNode dummyHead = new ListNode(0);
      dummyHead.next = head;
      ListNode cur = dummyHead;
      while(cur.next != null) {
          if(cur.next.val == val) {
              ListNode delNode = cur.next;
              cur.next = delNode.next;
          } else
              cur = cur.next;
      }
      return dummyHead.next;
  }
  ```

### 3.2. Remove Duplicates from Sorted List II

[82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/description/)

### 3.3. Delete Node in a Linked List

[237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/description/)

- Question
  > Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.
  > 
  > Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.
- Solution

  常规的删除操作中，需要至少获取 pre、cur 两个指针才能操作，而此题中只给出了待删除节点的 cur 指针，因此可以将 cur 后继节点向前移动一位，再删除多余的后继节点即可：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/06e71d5b135cb2100598b6818ea17d16.jpg)

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/11c73cde0d2a7c16b846e5671cf7c122.jpg)

  需要注意的是，该方法不适用于尾节点。

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

## 4. 虚拟头节点

### 4.1. Merge Two Sorted Lists

[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/description/)

可通过设置虚拟头节点来进行统一处理。

### 4.2. Swap Nodes in Pairs

[24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/description/)

- Question
  > Given a linked list, swap every two adjacent nodes and return its head.

  Example:
  ```
  Given 1->2->3->4, you should return the list as 2->1->4->3.
  ```
  Note:
  - Your algorithm should use only constant extra space.
  - You may not modify the values in the list's nodes, only nodes itself may be changed.
- Solution

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/99c1776831188c81881ba2e8e5a1403f.jpg)

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/97f09ba2aa6b9c8429e02a5f7d30947e.jpg)

  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(1)
  public ListNode swapPairs(ListNode head) {
      ListNode dummyHead = new ListNode(0);
      dummyHead.next = head;
      ListNode p = dummyHead;
      while(p.next != null && p.next.next != null ){
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
  ```

### 4.3. Reverse Nodes in k-Group

[25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)

## 5. 链表排序

### 5.1. Insertion Sort List

[147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/description/)

### 5.2. Sort List

[148. Sort List](https://leetcode.com/problems/sort-list/description/)

可使用自底向上的归并排序思路进行排序。

## 6. 快行指针

### 6.1. Remove Nth Node From End of List

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

### 6.2. Rotate List

[61. Rotate List](https://leetcode.com/problems/rotate-list/description/)

### 6.3. Reorder List

[143. Reorder List](https://leetcode.com/problems/reorder-list/description/)

### 6.4. Palindrome Linked List

[234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/description/)

## 7. Refer Links