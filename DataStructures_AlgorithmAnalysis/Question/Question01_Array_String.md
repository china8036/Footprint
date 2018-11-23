- [Array & String](#array--string)
  - [1. 元素移动](#1-元素移动)
    - [1.1. Move Zeros](#11-move-zeros)
    - [1.2. 奇偶分离](#12-奇偶分离)
  - [2. 元素删除](#2-元素删除)
    - [2.1. Remove Element](#21-remove-element)
    - [2.2. Remove Duplicates from Sorted Array](#22-remove-duplicates-from-sorted-array)
    - [2.3. Remove Duplicates from Sorted Array II](#23-remove-duplicates-from-sorted-array-ii)
  - [3. 元素替换](#3-元素替换)
    - [3.1. 不等长度的替换](#31-不等长度的替换)
  - [4. 数组排序](#4-数组排序)
    - [4.1. Sort Colors](#41-sort-colors)
    - [4.2. Merge Sorted Array](#42-merge-sorted-array)
    - [4.3. Kth Largest Element in an Array](#43-kth-largest-element-in-an-array)
  - [5. 双索引 Two Pointer](#5-双索引-two-pointer)
    - [5.1. Two Sum II - Input array is sorted](#51-two-sum-ii---input-array-is-sorted)
    - [5.2. Valid Palindrome](#52-valid-palindrome)
    - [5.3. Reverse String](#53-reverse-string)
    - [5.4. Reverse Vowels of a String](#54-reverse-vowels-of-a-string)
    - [5.5. 雨水收集问题](#55-雨水收集问题)
      - [5.5.1. Container With Most Water](#551-container-with-most-water)
      - [5.5.2. Trapping Rain Water](#552-trapping-rain-water)
      - [5.5.3. Trapping Rain Water Ⅱ](#553-trapping-rain-water-Ⅱ)
      - [5.5.4. Product of Array Except Self](#554-product-of-array-except-self)
  - [6. 滑动窗口](#6-滑动窗口)
    - [6.1. Minimum Size Subarray Sum](#61-minimum-size-subarray-sum)
    - [6.2. Longest Substring Without Repeating Characters](#62-longest-substring-without-repeating-characters)
    - [6.3. Find All Anagrams in a String](#63-find-all-anagrams-in-a-string)
    - [6.4. Minimum Window Substring](#64-minimum-window-substring)
    - [6.5. 和为 S 的连续序列](#65-和为-s-的连续序列)
  - [7. 其它问题](#7-其它问题)
    - [7.1. 变位词](#71-变位词)
    - [7.2. 字符串旋转](#72-字符串旋转)
    - [7.3. 矩阵旋转](#73-矩阵旋转)
    - [7.4. 矩阵顺时针打印](#74-矩阵顺时针打印)
    - [7.5. 矩阵清零](#75-矩阵清零)
    - [7.6. 连续序列最大和](#76-连续序列最大和)
    - [7.7. 连续序列中数字一出现的次数](#77-连续序列中数字一出现的次数)
    - [7.8. 用数组排出最小的数](#78-用数组排出最小的数)
    - [7.9. 第 n 个丑数](#79-第-n-个丑数)
    - [7.10. 正则表达式匹配](#710-正则表达式匹配)
  - [8. Refer Links](#8-refer-links)

# Array & String

## 1. 元素移动

### 1.1. Move Zeros

[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/description/)

> Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

- 非原地解法
  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(n)
  class Solution1 {
      public void moveZeroes(int[] nums) {
          ArrayList<Integer> nonZeroElements = new ArrayList<>();
          // 将 vec 中所有非 0 元素放入 nonZeroElements 中
          for (int i = 0; i < nums.length; i++)
              if (nums[i] != 0)
                  nonZeroElements.add(nums[i]);
          // 将 nonZeroElements 中的所有元素依次放入到 nums 开始的位置
          for (int i = 0; i < nonZeroElements.size(); i++)
              nums[i] = nonZeroElements.get(i);
          // 将 nums 剩余的位置放置为 0
          for (int i = nonZeroElements.size(); i < nums.length; i++)
              nums[i] = 0;
      }
  }
  ```
- 原地解法
  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(1)
  class Solution2 {
      public void moveZeroes(int[] nums) {
          // 使用 k 索引，使得在 nums 中，[0...k) 的元素均为非 0 元素
          int k = 0;
          // 遍历到第 i 个元素后，保证 [0...i] 中所有非 0 元素都按照顺序排列在 [0...k) 中
          // 同时，[k...i] 为 0
          for(int i = 0; i < nums.length; i++)
              if(nums[i] != 0) {
                  if(k != i)
                      Arrays.swap(nums, k, i);
                  k++;
              }
      }
  }
  ```

### 1.2. 奇偶分离

[《剑指 offer》 面试题 14](https://www.nowcoder.com/practice/beb5aa231adc45b2a5dcc5b62c93f593?tpId=13&tqId=11166&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

- Question
  > 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

- Solution
  - 解法 1：使用冒泡排序的思想，每次循环都将各个奇数整体往前移动了一个位置。时间复杂度为 O(n^2)。
    ```java
    public void reOrderArray(int [] array) {
        for (int i = 0; i < array.length; i++) {
            int flag = 0;
            for (int j = array.length - 1; j > i; j--) {
                if (array[j] % 2 == 1 && array[j - 1] % 2 == 0) { // 前偶后奇就交换
                    swap(array, j, j - 1);
                    flag = 1;
                }
            }
            if (flag == 0)
                break;
        }
    }
    private void swap(int [] a, int i, int j) {
        a[i] ^= a[j];
        a[j] ^= a[i];
        a[i] ^= a[j];
    }
    ```
  - 解法 2：复制一个额外辅助空间，扫描辅助数组，直接赋值到原数组即可。空间复杂度和时间复杂度都为 O(n)。

## 2. 元素删除

### 2.1. Remove Element

[27. Remove Element](https://leetcode.com/problems/remove-element/description/)

> Given an array nums and a value val, remove all instances of that value in-place and return the new length.

- Solution
  ```java
  // Time complexity: O(n)
  // Space complexity: O(1)
  public int removeElement(int[] nums, int val) {
      int i = 0;
      for (int j = 0; j < nums.length; j++)
          if (nums[j] != val)
              nums[i++] = nums[j];
      return i;
  }
  ```

### 2.2. Remove Duplicates from Sorted Array

[26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)

> Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length.

- Solution
  ```java
  // Time complexity: O(n)
  // Space complexity: O(1)
  public int removeDuplicates(int[] nums) {
      int i = 0;
      for (int j = 1; j < nums.length; j++)
          if (nums[j - 1] != nums[j])
              nums[++i] = nums[j];
      return i + 1;
  }
  ```

### 2.3. Remove Duplicates from Sorted Array II

[80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/)

> Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.

- Solution
  ```java
  public int removeDuplicates(int[] nums) {
      int index=0;
      int ret=0;
      for (int i = 0; i < nums.length; i++) {
          int count=1;
          int num = nums[i];
          while (i<nums.length-1 && nums[i] == nums[i+1]) {
              i++;
              count++;
          }
          count = count >= 2 ? 2 : count;
          ret +=count;
          for (int j = 0; j < count; j++)
              nums[index++] = num;
      }
      return ret;
  }
  ```

## 3. 元素替换

### 3.1. 不等长度的替换

[《剑指 offer》 面试题 4](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

- Question

  编写程序，将字符串中的空格全部替换成 `%20`（假定该字符串有足够的空间存放新字符），要求空间效率为 O(1)。例如，当字符串为 We Are Happy. 则经过替换之后的字符串为 We%20Are%20Happy。

- Solution

  扫描 2 次字符串，第一次统计空格个数，计算新字符串长度；第二次从后往前遍历（避免覆写原有数据），将字符复制成新的值。
  ```java
  public void replaceSpace(char [] str) {
    int spaceCnt;
    for (int i = 0; i < str.length; i++)
      if (str[i] == ' ')
        spaceCnt++;

    int newLength = str.length + spaceCnt * 2;
    str[newLength] = '\0';
    for (int i = str.length - 1, j = newLength - 1; i >= 0 && j >= 0; i--, j--) {
      if (str[i] == ' ') {
        str[j] = '0';
        str[--j] = '2';
        str[--j] = '%';
      } else
        str[j] == str[i];
    }
  }
  ```

## 4. 数组排序

### 4.1. Sort Colors

[75. Sort Colors](https://leetcode.com/problems/sort-colors/description/)

> Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

- 计数排序思想
  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(k), k 为元素的取值范围
  public class Solution1 {
      public void sortColors(int[] nums) {
          int[] count = {0, 0, 0};    // 存放 0, 1, 2 三个元素的频率
          for(int i = 0 ; i < nums.length ; i ++){
              assert nums[i] >= 0 && nums[i] <= 2;
              count[nums[i]] ++;
          }

          int index = 0;
          for(int i = 0 ; i < count[0] ; i ++)
              nums[index++] = 0;
          for(int i = 0 ; i < count[1] ; i ++)
              nums[index++] = 1;
          for(int i = 0 ; i < count[2] ; i ++)
              nums[index++] = 2;
      }
  }
  ```

- 三路快排思想

  ![image](http://img.cdn.firejq.com/jpg/2018/4/24/fb16f22b4c88d05adda76b156c934058.jpg)

  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(1)
  public class Solution2 {
      public void sortColors(int[] nums) {
          int zero = -1;          // [0...zero] == 0
          int two = nums.length;  // [two...n-1] == 2
          for (int i = 0; i < two; ) {
              if (nums[i] == 1)
                  i++;
              else if (nums[i] == 2)
                  Arrays.swap(nums, i, --two);
              else // nums[i] == 0
                  Arrays.swap(nums, ++zero, i++);
          }
      }
  }
  ```

### 4.2. Merge Sorted Array

[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

<!-- 利用归并排序 merge 的思路 -->

### 4.3. Kth Largest Element in an Array

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

- 直接进行排序后返回指定位置元素
  ```java
  class Solution {
      public int findKthLargest(int[] nums, int k) {
          Arrays.sort(nums);
          return nums[nums.length - k];
      }
  }
  ```

- 利用快排 partition 的思路
  ```java
  class Solution {
      public int findKthLargest(int[] nums, int k) {
          int pivot = 0, left = 0, right = nums.length - 1;
          while (pivot >= 0 && pivot < nums.length) {
              pivot = partition(nums, left, right);
              if (pivot > k - 1)
                  right = pivot - 1;
              else if (pivot < k - 1)
                  left = pivot + 1;
              else // pivot == k - 1
                  return nums[pivot];
          }
          return -1;
      }

      private int partition(int[] nums, int L, int R) {
          if (L <= R) {
              int pivot = L;
              Arrays.swap(nums, new Random().nextInt(R-L+1)+L, R);
              for (int i = L; i < R; i++)
                  if (nums[i] > nums[R])
                      Arrays.swap(nums, i, pivot++);
              Arrays.swap(nums, pivot, R);
              return pivot;
          }
          return -1;
      }
  }
  ```

## 5. 双索引 Two Pointer

### 5.1. Two Sum II - Input array is sorted

[167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)

- Question
  > Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.
  >
  > The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

  You may assume that each input would have exactly one solution and you may not use the same element twice.
  ```
  Input: numbers = [2,7,11,15], target = 9
  Output: [1,2]
  Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
  ```

- Solution
  - 暴力解法：双层遍历，O(n^2)。
  - 二分查找：O(nlogn)

    ![image](http://img.cdn.firejq.com/jpg/2018/4/24/1236ec524e9180c8a1a0947ee94f7826.jpg)

    ```java
    // 时间复杂度：O(nlogn)
    // 空间复杂度：O(1)
    public class Solution2 {
        public int[] twoSum(int[] numbers, int target) {
            if(numbers.length < 2 /*|| !isSorted(numbers)*/)
                throw new IllegalArgumentException("Illegal argument numbers");
            for(int i = 0 ; i < numbers.length - 1 ; i ++) {
                int j = Arrays.binarySearch(numbers, i+1, numbers.length-1, target - numbers[i]);
                if(j != -1){
                    int[] res = {i+1, j+1};
                    return res;
                }
            }
            throw new IllegalStateException("The input has no solution");
        }
        private boolean isSorted(int[] numbers){
            for(int i = 1 ; i < numbers.length ; i ++)
                if(numbers[i] < numbers[i-1])
                    return false;
            return true;
        }
    }
    ```

  - 对撞指针：O(n)

    ![image](http://img.cdn.firejq.com/jpg/2018/4/24/72b5e6b4f06f34d5ab1a407433fac3fc.jpg)

    ```java
    // 时间复杂度：O(n)
    // 空间复杂度：O(1)
    public class Solution3 {
        public int[] twoSum(int[] numbers, int target) {
            if (numbers.length < 2 /*|| !isSorted(numbers)*/)
                throw new IllegalArgumentException("Illegal argument numbers");

            int l = 0, r = numbers.length - 1;
            while (l < r) {
                if (numbers[l] + numbers[r] == target) {
                    int[] res = {l+1, r+1};
                    return res;
                }
                else if (numbers[l] + numbers[r] < target)
                    l ++;
                else // numbers[l] + numbers[r] > target
                    r --;
            }
            throw new IllegalStateException("The input has no solution");
        }
        private boolean isSorted(int[] numbers) {
            for(int i = 1 ; i < numbers.length ; i ++)
                if(numbers[i] < numbers[i-1])
                    return false;
            return true;
        }
    }
    ```

### 5.2. Valid Palindrome

[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/description/)

### 5.3. Reverse String

[344. Reverse String](https://leetcode.com/problems/reverse-string/)

### 5.4. Reverse Vowels of a String

[345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/description/)

### 5.5. 雨水收集问题

#### 5.5.1. Container With Most Water

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

- Question

  > Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

  ![image](http://img.cdn.firejq.com/jpg/2018/4/24/ad145121d2296bc0e4d0fe0b51c7ad24.jpg)

  Example:
  ```
  Input: [1,8,6,2,5,4,8,3,7]
  Output: 49
  ```

- Solution

  根据题意，要求从 height 数组中任选两项 i 和 j（i <= j），使得 `Math.min(height[i], height[j]) * (j - i)` 最大化，求解这个最大值。
  - 暴力解法，枚举所有情况，时间效率为 O(n^2)

  - 双指针对撞（夹逼），时间效率为 O(n)
    ```java
    public int maxArea(int[] height) {
      int left = 0, right = height.length - 1;
      int maxArea = 0;

      while (left < right) {
        maxArea = Math.max(maxArea, Math.min(height[left], height[right]) * (right - left));
        if (height[left] < height[right])
          left++;
        else
          right--;
      }

      return maxArea;
    }
    ```

#### 5.5.2. Trapping Rain Water

[42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

- Question
  > Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

  ![image](http://img.cdn.firejq.com/jpg/2018/11/6/7be0393243c7d03e96f83bb1c1d6ed24.jpg)

  Example:
  ```
  Input: [0,1,0,2,1,0,1,3,2,1,2,1]
  Output: 6
  ```

- Solution
  - DP

    维护一个一维的 dp 数组，这个 DP 算法需要遍历两遍数组，第一遍遍历 dp[i] 中存入 i 位置左边的最大值，然后开始第二遍遍历数组，第二次遍历时找右边最大值，然后和左边最大值比较取其中的较小值，然后跟当前值 A[i] 相比，如果大于当前值，则将差值存入结果。
    ```java
    public int trap(int[] height) {
        int res = 0, mx = 0, n = height.length;
        int[] dp = new int[n];
        for (int i = 0; i < n; ++i) {
            dp[i] = mx;
            mx = Math.max(mx, height[i]);
        }
        mx = 0;
        for (int i = n - 1; i >= 0; --i) {
            dp[i] = Math.min(dp[i], mx);
            mx = Math.max(mx, height[i]);
            if (dp[i] - height[i] > 0) res += dp[i] - height[i];
        }
        return res;
    }
    ```

  - 双指针

    使用 left 和 right 两个指针分别指向数组的首尾位置，从两边向中间扫描，在当前两指针确定的范围内，先比较两头找出较小值，如果较小值是 left 指向的值，则从左向右扫描，如果较小值是 right 指向的值，则从右向左扫描，若遇到的值比当较小值小，则将差值存入结果，如遇到的值大，则重新确定新的窗口范围，以此类推直至 left 和 right 指针重合。
    ```java
    public int trap(int[] height) {
        int res = 0, l = 0, r = height.length - 1;

        while (l < r) {
            int cur = Math.min(height[l], height[r]);
            if (height[l] == cur) {
                ++l;
                while (l < r && height[l] < cur)
                    res += cur - height[l++];
            } else {
                --r;
                while (l < r && height[r] < cur)
                    res += cur - height[r--];
            }
        }
        return res;
    }
    ```

  - 辅助栈

    遍历高度，如果此时栈为空，或者当前高度小于等于栈顶高度，则把当前高度的坐标压入栈，注意我们不直接把高度压入栈，而是把坐标压入栈，这样方便我们在后来算水平距离。当我们遇到比栈顶高度大的时候，就说明有可能会有坑存在，可以装雨水。此时我们栈里至少有一个高度，如果只有一个的话，那么不能形成坑，我们直接跳过，如果多余一个的话，那么此时把栈顶元素取出来当作坑，新的栈顶元素就是左边界，当前高度是右边界，只要取二者较小的，减去坑的高度，长度就是右边界坐标减去左边界坐标再减 1，二者相乘就是盛水量。
    ```java
    public int trap(int[] height) {
        ArrayDeque<Integer> s = new ArrayDeque<>();
        int i = 0, n = height.length, res = 0;
        while (i < n) {
            if (s.isEmpty() || height[i] <= height[s.peek()]) {
                s.push(i++);
            } else {
                int t = s.pop();
                if (s.isEmpty()) continue;
                res += (Math.min(height[i], height[s.peek()]) - height[t]) * (i - s.peek() - 1);
            }
        }
        return res;
    }
    ```

#### 5.5.3. Trapping Rain Water Ⅱ

[407. Trapping Rain Water Ⅱ](https://leetcode.com/problems/trapping-rain-water-ii/)

#### 5.5.4. Product of Array Except Self

[238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

[《剑指 offer》 面试题 52](https://www.nowcoder.com/practice/94a4d381a68b47b7a8bed86f2975db46?tpId=13&tqId=11204&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

- Question
  > Given an array nums of n integers where n > 1, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

  Example:
  ```
  Input:  [1,2,3,4]
  Output: [24,12,8,6]
  ```
  Note: Please solve it without division and in O(n).

- Solution

  由于题目规定不能使用除法，且时间复杂度为 O(n)，因此可以申请两个大小为 n 的数组，第一个数组的第 i 个元素存储第 0 个到第 i-1 个元素的乘积，第二个数组的第 i 个元素存储第 i+1 到最后一个元素的乘积，然后将两个数组的第 i 个元素和第 i 个元素相乘就能得到结果。

  更进一步，可以使用一个辅助变量来代替一个辅助数组，也就是说只申请一个结果数组就可以达到目的，不使用任何额外的辅助空间。
  ```java
  public int[] productExceptSelf(int[] nums) {
      int n = nums.length;
      int [] res = new int[n];

      // 从左往右
      res[0] = 1;
      for (int i = 1; i < n; i++)
          res[i] = res[i - 1] * nums[i - 1];

      // 从右往左
      int right = 1; // 辅助变量
      for (int i = n - 1; i >= 0; i--) {
          res[i] *= right;
          right *= nums[i];
      }
      return res;
  }
  ```

## 6. 滑动窗口

在 ACM 中一般将滑动窗口技巧称为“**尺取法**”。

### 6.1. Minimum Size Subarray Sum

[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

- Question
  > Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.
  >
  > For example, given the array [2,3,1,2,4,3] and s = 7, the subarray [4,3] has the minimal length under the problem constraint.

- Solution
  - 暴力解法：遍历所有连续子数组再逐一验证，O(n^3)。
  - 滑动窗口

    ![image](http://img.cdn.firejq.com/jpg/2018/4/24/4de6893405d014a7533ad5a47dfc54f9.jpg)

    ```java
    // 时间复杂度：O(n)
    // 空间复杂度：O(1)
    class Solution {
        public int minSubArrayLen(int s, int[] nums) {
            if(s <= 0 || nums == null)
                throw new IllegalArgumentException("Illigal Arguments");

            int l = 0 , r = -1; // nums[l...r] 为我们的滑动窗口
            int sum = 0;
            int res = nums.length + 1;
            while(l < nums.length){   // 窗口的左边界在数组范围内，则循环继续
                if(r + 1 < nums.length && sum < s)
                    sum += nums[++r];
                else // r 已经到头 或者 sum >= s
                    sum -= nums[l++];

                if(sum >= s)
                    res = Math.min(res, r - l + 1);
            }
            if(res == nums.length + 1)
                return 0;
            return res;
        }
    }
    ```

### 6.2. Longest Substring Without Repeating Characters

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

- Question
  > Given a string, find the length of the longest substring without repeating characters.

  Examples:
  ```
  Given "abcabcbb", the answer is "abc", which the length is 3.

  Given "bbbbb", the answer is "b", with the length of 1.

  Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
  ```
- Solution

  ![image](http://img.cdn.firejq.com/jpg/2018/4/24/5838a5f1d656c82e3d898d50559d1e2b.jpg)

  ```java
  // 时间复杂度：O(len(s))
  // 空间复杂度：O(len(charset))
  public class Solution2 {
      public int lengthOfLongestSubstring(String s) {
          int [] freq = new int[256];
          int l = 0, r = -1; // 滑动窗口为 s[l...r]
          int res = 0;
          while(r + 1 < s.length()) {
              if(freq[s.charAt(r+1)] == 0)
                  freq[s.charAt(++r)]++;
              else    //freq[s[r+1]] == 1
                  freq[s.charAt(l++)]--;
              res = Math.max(res, r-l+1);
          }
          return res;
      }
  }
  ```

### 6.3. Find All Anagrams in a String

[438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)

### 6.4. Minimum Window Substring

[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)

### 6.5. 和为 S 的连续序列

- Question
  > 小明很喜欢数学，有一天他在做数学作业时，要求计算出 9~16 的和，他马上就写出了正确答案是 100。但是他并不满足于此，他在想究竟有多少种连续的正数序列的和为 100（至少包括两个数)。没多久，他就得到另一组连续正数和为 100 的序列：`18,19,20,21,22`。现在把问题交给你，你能不能也很快的找出所有和为 S 的连续正数序列？

- Solution

  使用滑动窗口。当总和小于 sum，大指针继续加，否则小指针加。
  ```java
  public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
      ArrayList<ArrayList<Integer>> allRes = new ArrayList<>();
      int phigh = 2, plow = 1;
      while (phigh > plow) {
          int cur = (phigh + plow) * (phigh - plow + 1) / 2; // a~b 的连续序列求和公式
          if (cur < sum)
              phigh++;
          if (cur == sum) {
              ArrayList<Integer> res = new ArrayList<>();
              for (int i = plow; i <= phigh; i++)
                  res.add(i);
              allRes.add(res);
              plow++;
          }
          if (cur > sum)
              plow++;
      }
      return allRes;
  }
  ```

## 7. 其它问题

### 7.1. 变位词

- Question

  给定 2 个字符串，判断其中一个字符串的字符重新排序后，能否变成另一个字符串。

- Solution
  - 解法 1：先排序后比较。若 2 个字符串拥有同一组字符，那么排序后肯定会变成完全相同的 2 个字符串。

  - 解法 2：统计字符频度。遍历字符串的每个字符，统计出现次数，再进行比较即可。

### 7.2. 字符串旋转

- Question

  给定 2 个字符串，判断其中一个字符串旋转后，能否变成另一个字符串。如：`waterbottle` 以 `wat` 旋转后，就会得到 `erbottlewat`。

- Solution

  假定 s2 可以由 s1 旋转而来，此时令 `x = wat`, `y = erbottle`，则 `s1 = xy = waterbottle`，`s2 = yx = erbottlewat`。由于 `yx` 是 `xyxy` 的子串，因此 `s2` 是 `s1s1` 的子串。也就是说，解决这个问题只需要判断 `s2` 是不是 `s1s1` 的子串即可。
  ```java
  public boolean isRotation(String s1, String s2) {
    if (s1.length() > 0 && s1.length() == s2.length())
      return isSubstring(s1+s1, s2);
    return false;
  }
  ```

### 7.3. 矩阵旋转

- Question

  给定一个 n*n 的二维矩阵，编写程序将矩阵旋转 90°，要求空间效率为 O(1)。

  ![image](http://img.cdn.firejq.com/jpg/2018/10/17/726c2bdd26fcae49f9efa8407fdea6c0.jpg)

- Solution

  从最外面一层开始，逐渐向里（也可以从内层向外），在每一层上进行旋转，即将上边移到右边，右边移到下边，下边移到左边，左边移到上边。同时为实现空间效率为 O(1)，每次移动时都按索引一个个进行交换。
  ```java
  public void rotate(int [][] matrix, int n) {
    for (int layer = 0; layer < n / 2; layer++) {
      int first = layer;
      int last = n - layer - 1;
      for (int i = first; i < largest; i++) {
        int offset = i - first;
        int top  = matrix[first][i];
        matrix[first][i] = matrix[last - offset][first]; // left to top
        matrix[last - offset][first] = matrix[last][last - offset]; // bottom to left
        matrix[last][last - offset] = matrix[i][last]; // right to bottom
        matrix[i][last] = top; // top to right
      }
    }
  }
  ```
  时间效率为 O(n^2)，但已是最优解法，因为任何算法都至少需要访问 n^2 个元素。

### 7.4. 矩阵顺时针打印

[《剑指 offer》 面试题 20](https://www.nowcoder.com/practice/9b4c81a02cd34f76be2659fa0d54342a?tpId=13&tqId=11172&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

- Question
  > 输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下 4 X 4 矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字 1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

- Solution
  ```java
  public ArrayList<Integer> printMatrix(int [][] matrix) {
      ArrayList<Integer> result = new ArrayList<>();
      if (matrix.length == 0)
          return result;
      int n = matrix.length, m = matrix[0].length;
      if (m == 0)
          return result;
      int layers = (Math.min(n, m) - 1) / 2 + 1;// 层数
      for (int i = 0; i < layers; i++) {
          for (int k = i; k < m - i; k++)
              result.add(matrix[i][k]);// 左至右
          for (int j = i + 1; j < n - i; j++)
              result.add(matrix[j][m - i - 1]);// 右上至右下
          for (int k = m - i - 2; (k >= i) && (n - i - 1 != i); k--)
              result.add(matrix[n - i - 1][k]);// 右至左
          for (int j = n - i - 2; (j > i) && (m - i - 1 != i); j--)
              result.add(matrix[j][i]);// 左下至左上
      }
      return result;
  }
  ```

### 7.5. 矩阵清零

- Question

  给定一个 m*n 的二维矩阵，编写程序实现：如果某个元素为 0，则将该元素所在的行和列清零。

- Solution

  **注意陷阱：如果直接遍历矩阵进行操作，清零操作会导致下一步判断出错，在读取被清零的行或列时，读到的尽是零，很可能很快会就将整个矩阵清零**。

  因此，可以使用额外的矩阵来标记 0 元素的位置，然后在第二次遍历时将 0 元素所在的行和列清零，空间效率为 O(mn)。

  进一步优化，实际上题目并不需要记录 0 元素的确切位置，因此可以只用 2 个数组来记录包含 0 元素的所有行和列，然后再第二次遍历时将这些行和列清零，空间效率为 O(m+n)。

  ```java
  public void setZeros(int [][] m) {
    boolean [] row = new boolean[m.length];
    boolean [] col = new boolean[m[0].length];

    for (int i = 0; i < m.length; i++)
      for (int j = 0; j < m[0].length; j++)
        if (m[i][j] == 0) {
          row[i] = true;
          col[j] = true;
        }

    for (int i = 0; i < m.length; i++)
      for (int j = 0; j < m[0].length; j++)
        if (row[i] || col[j])
          m[i][j] = 0;
  }
  ```

  再进一步优化，可以使用位向量来代替 2 个布尔数组，空间效率为 O(1)。

### 7.6. 连续序列最大和

《CC 150》 17.8

[《剑指 offer》 面试题 31](https://www.nowcoder.com/practice/459bd355da1549fa8a49e350bf3df484?tpId=13&tqId=11183&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

- Question

  给定一个整数数组（有正数也有负数），找出总和最大的连续数列（长度可为 0），返回最大和。

- Solution
  - 解法 1：
    ```java
    public int getMaxSum(int [] a) {
      int maxSum = 0;
      int sum = 0;
      for (int i = 0; i < a.length; i++) {
        sum += a[i];
        maxSum = Math.max(maxSum, sum);
        sum = sum < 0 ? 0 : sum;
      }
      return maxSum;
    }
    ```
  - 解法 2：DP。用 f(i) 表示以第 i 个数字为结尾的子数列的最大和，那么
    ```
    f(i) = a[i]             (i == 0 或 f(i - 1) < 0)
         = a[i] + f(i - 1)  (i != 0 且 f(i - 1) > 0)
    ```

- Follow Up：连续数列的长度至少为 1
  ```java
  public int getMaxSum(int[] array) {
      if (array.length == 0 || array == null)
          return 0;
      int curSum = 0;
      int maxSum = 0x80000000;
      for (int i = 0; i < array.length; i++) {
          if (curSum <= 0)
              curSum = array[i]; // 记录当前最大值
          else
              curSum += array[i]; // 当 array[i] 为正数时，加上之前的最大值并更新最大值
          maxSum = Math.max(maxSum, curSum); // 更新结果最大值
      }
      return maxSum;
  }
  ```

### 7.7. 连续序列中数字一出现的次数

[《剑指 offer》 面试题 32](https://www.nowcoder.com/practice/bd7f978302044eee894445e244c7eee6?tpId=13&tqId=11184&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

- Question
  > 求出任意非负整数区间中 1 出现的次数（从 1 到 n 中 1 出现的次数）。如 1~13 中包含 1 的数字有 1、10、11、12、13，因此共出现 6 次。

- Solution

  设定整数点（如 1、10、100 等等）作为位置点 i（对应 n 的各位、十位、百位等等），分别对每个数位上有多少包含 1 的点进行分析。根据设定的整数位置，对 n 进行分割，分为两部分，高位 n/i，低位 n%i：
  - 当 i 表示百位，且百位对应的数 >=2, 如 n=31456,i=100，则 a=314,b=56，此时百位为 1 的次数有 a/10+1=32（最高两位 0~31），每一次都包含 100 个连续的点，即共有 (a%10+1)*100 个点的百位为 1
  - 当 i 表示百位，且百位对应的数为 1，如 n=31156,i=100，则 a=311,b=56，此时百位对应的就是 1，则共有 a%10（最高两位 0-30) 次是包含 100 个连续点，当最高两位为 31（即 a=311），本次只对应局部点 00~56，共 b+1 次，所有点加起来共有（a%10*100）+(b+1)，这些点百位对应为 1
  - 当 i 表示百位，且百位对应的数为 0, 如 n=31056,i=100，则 a=310,b=56，此时百位为 1 的次数有 a/10=31（最高两位 0~30）
  综合以上三种情况，当百位对应 0 或》=2 时，有 (a+8)/10 次包含所有 100 个点，还有当百位为 1(a%10==1)，需要增加局部点 b+1。之所以补 8，是因为当百位为 0，则 a/10==(a+8)/10，当百位》=2，补 8 会产生进位位，效果等同于 (a/10+1)。

  ```java
  public int NumberOf1Between1AndN_Solution(int n) {
      int count = 0;
      for (int i = 1; i <= n; i *= 10) {
          //i 表示当前分析的是哪一个数位
          int a = n / i, b = n % i;
          count = count + (a + 8) / 10 * i + (a % 10 == 1 ? 1 : 0) * (b + 1);
      }
      return count;
  }
  ```

### 7.8. 用数组排出最小的数

[《剑指 offer》 面试题 33](https://www.nowcoder.com/practice/8fecd3f8ba334add803bf2a06af1b993?tpId=13&tqId=11185&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

- Question
  > 输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为 321323。

- Solution
  ```java
  public String PrintMinNumber(int [] numbers) {
      ArrayList<Integer> list = new ArrayList<>();
      int n = numbers.length;
      for (int i = 0; i < n; i++)
          list.add(numbers[i]);
      Collections.sort(list, (Integer str1, Integer str2) -> {
              String s1 = str1 + "" + str2;
              String s2 = str2 + "" + str1;
              return s1.compareTo(s2);
      });
      String s = "";
      for (int j : list)
          s += j;
      return s;
  }
  ```

### 7.9. 第 n 个丑数

[《剑指 offer》 面试题 34](https://www.nowcoder.com/practice/6aa9e04fc3794f68acf8778237ba065b?tpId=13&tqId=11186&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

- Question
  > 把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。例如 6、8 都是丑数，但 14 不是，因为它包含质因子 7。习惯上我们把 1 当做是第一个丑数。求按从小到大的顺序的第 N 个丑数。

- Solution
  ```java
  public int GetUglyNumber_Solution(int index) {
      if (index < 7)
          return index;
      int [] res = new int[index];
      res[0] = 1;
      int t2 = 0, t3 = 0, t5 = 0;
      for (int i = 1; i < index; i++) {
          res[i] = Math.min(res[t2] * 2, Math.min(res[t3] * 3, res[t5] * 5));
          if (res[i] == res[t2] * 2)
              t2++;
          if (res[i] == res[t3] * 3)
              t3++;
          if (res[i] == res[t5] * 5)
              t5++;
      }
      return res[index - 1];
  }
  ```

### 7.10. 正则表达式匹配

[《剑指 offer》 面试题 53](https://www.nowcoder.com/practice/45327ae22b7b413ea21df13ee7d6429c?tpId=13&tqId=11205&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

- Question
  > 请实现一个函数用来匹配包括'`.`'和'`*`'的正则表达式。模式中的字符'`.`'表示任意一个字符，而'`*`'表示它前面的字符可以出现任意次（包含 0 次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"`aaa`"与模式"`a.a`"和"`ab*ac*a`"匹配，但是与"`aa.a`"和"`ab*a`"均不匹配

- Solution

  当模式中的第二个字符不是“*”时：
  1. 如果字符串第一个字符和模式中的第一个字符相匹配，那么字符串和模式都后移一个字符，然后匹配剩余的。
  1. 如果 字符串第一个字符和模式中的第一个字符相不匹配，直接返回 false。

  当模式中的第二个字符是“*”时：
  - 如果字符串第一个字符跟模式第一个字符不匹配，则模式后移 2 个字符，继续匹配。
  - 如果字符串第一个字符跟模式第一个字符匹配，可以有 3 种匹配方式：
    1. 模式后移 2 字符，相当于 `x*` 被忽略。
    1. 字符串后移 1 字符，模式后移 2 字符。
    1. 字符串后移 1 字符，模式不变，即继续匹配字符下一位，因为*可以匹配多位。
  ```java
  public class Solution {
      public boolean match(char[] str, char[] pattern) {
          if (str == null || pattern == null)
              return false;
          int strIndex = 0;
          int patternIndex = 0;
          return matchCore(str, strIndex, pattern, patternIndex);
      }
      private boolean matchCore(char[] str, int strIndex, char[] pattern, int patternIndex) {
          // 有效性检验：str 到尾，pattern 到尾，匹配成功
          if (strIndex == str.length && patternIndex == pattern.length)
              return true;
          //pattern 先到尾，匹配失败
          if (strIndex != str.length && patternIndex == pattern.length)
              return false;
          // 模式第 2 个是*，且字符串第 1 个跟模式第 1 个匹配，分 3 种匹配模式；如不匹配，模式后移 2 位
          if (patternIndex + 1 < pattern.length && pattern[patternIndex + 1] == '*') {
              if ((strIndex != str.length && pattern[patternIndex] == str[strIndex]) || (pattern[patternIndex] == '.' && strIndex != str.length)) {
                  return matchCore(str, strIndex, pattern, patternIndex + 2)// 模式后移 2，视为 x*匹配 0 个字符
                          || matchCore(str, strIndex + 1, pattern, patternIndex + 2)// 视为模式匹配 1 个字符
                          || matchCore(str, strIndex + 1, pattern, patternIndex);//*匹配 1 个，再匹配 str 中的下一个
              } else
                  return matchCore(str, strIndex, pattern, patternIndex + 2);
          }
          // 模式第 2 个不是*，且字符串第 1 个跟模式第 1 个匹配，则都后移 1 位，否则直接返回 false
          if ((strIndex != str.length && pattern[patternIndex] == str[strIndex]) || (pattern[patternIndex] == '.' && strIndex != str.length))
              return matchCore(str, strIndex + 1, pattern, patternIndex + 1);
          return false;
      }
  }
  ```

## 8. Refer Links
