- [Array & String](#array--string)
  - [1. 元素移动](#1-元素移动)
    - [1.1. Move Zeros](#11-move-zeros)
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
  - [7. 其它问题](#7-其它问题)
    - [7.1. 变位词](#71-变位词)
    - [7.2. 字符串旋转](#72-字符串旋转)
    - [7.3. 矩阵旋转](#73-矩阵旋转)
    - [7.4. 矩阵清零](#74-矩阵清零)
    - [7.5. 连续序列最大和](#75-连续序列最大和)
    - [7.6. 用数组排出最小的数](#76-用数组排出最小的数)
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

- Question

  编写程序，将字符串中的空格全部替换成 `%20`（假定该字符串有足够的空间存放新字符），要求空间效率为 O(1)。

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

### 7.4. 矩阵清零

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

### 7.5. 连续序列最大和

《CC 150》 17.8

《剑指 offer》 31 题

- Question

  给定一个整数数组（有正数也有负数），找出总和最大的连续数列，返回最大和。

- Solution

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

### 7.6. 用数组排出最小的数

《剑指 offer》 33 题

- Question

  TODO:

- Solution
  ```cpp
  bool cmp(int a, int b)
  {
      string ab = to_string(a) + to_string(b);
      string ba = to_string(b) + to_string(a);
      return ab < ba;
  }
  class Solution
  {
  public:
      string PrintMinNumber(vector<int> n)
      {
          sort(n.begin(), n.end(), cmp);
          string res = "";
          for (int i : n)
              res += to_string(i);
          return res;
      }
  };
  ```

## 8. Refer Links
