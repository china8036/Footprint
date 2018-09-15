- [Array & String](#array--string)
    - [1. 元素移动](#1-元素移动)
        - [1.1. Move Zeros](#11-move-zeros)
    - [2. 元素删除](#2-元素删除)
        - [2.1. Remove Element](#21-remove-element)
        - [2.2. Remove Duplicates from Sorted Array](#22-remove-duplicates-from-sorted-array)
        - [2.3. Remove Duplicates from Sorted Array II](#23-remove-duplicates-from-sorted-array-ii)
    - [3. 数组排序](#3-数组排序)
        - [3.1. Sort Colors](#31-sort-colors)
        - [3.2. Merge Sorted Array](#32-merge-sorted-array)
        - [3.3. Kth Largest Element in an Array](#33-kth-largest-element-in-an-array)
    - [4. 双索引 Two Pointer](#4-双索引-two-pointer)
        - [4.1. Two Sum II - Input array is sorted](#41-two-sum-ii---input-array-is-sorted)
        - [4.2. Valid Palindrome](#42-valid-palindrome)
        - [4.3. Reverse String](#43-reverse-string)
        - [4.4. Reverse Vowels of a String](#44-reverse-vowels-of-a-string)
        - [4.5. Container With Most Water](#45-container-with-most-water)
    - [5. 滑动窗口](#5-滑动窗口)
        - [5.1. Minimum Size Subarray Sum](#51-minimum-size-subarray-sum)
        - [5.2. Longest Substring Without Repeating Characters](#52-longest-substring-without-repeating-characters)
        - [5.3. Find All Anagrams in a String](#53-find-all-anagrams-in-a-string)
        - [5.4. Minimum Window Substring](#54-minimum-window-substring)
    - [6. Refer Links](#6-refer-links)

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
          for(int i = 0; i < nums.length; i ++)
              if(nums[i] != 0) {
                  if(k != i)
                      swap(nums, k, i);
                  k ++;
              }
      }
      private void swap(int[] nums, int i, int j){
          int t = nums[i];
          nums[i] = nums[j];
          nums[j] = t;
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
			for (int i=0;i<nums.length;i++) {
					int count=1;
					int num = nums[i];            
					while (i<nums.length-1 && nums[i] == nums[i+1]) {
							i++;
							count++;                
					}
					count = count >= 2 ? 2 : count;
					ret +=count;
					for (int j=0;j<count;j++)
							nums[index++] = num; 
			}
			return ret;
	}
	```

## 3. 数组排序

### 3.1. Sort Colors

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

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/24/fb16f22b4c88d05adda76b156c934058.jpg)

  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(1)
  public class Solution2 {
      public void sortColors(int[] nums) {
          int zero = -1;          // [0...zero] == 0
          int two = nums.length;  // [two...n-1] == 2
          for(int i = 0 ; i < two ; ){
              if(nums[i] == 1)
                  i ++;
              else if (nums[i] == 2)
                  swap(nums, i, --two);
              else{ // nums[i] == 0
                  assert nums[i] == 0;
                  swap(nums, ++zero, i++);
              }
          }
      }

      private void swap(int[] nums, int i, int j){
          int t = nums[i];
          nums[i]= nums[j];
          nums[j] = t;
      }
  }
  ```

### 3.2. Merge Sorted Array

[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

<!-- 利用归并排序 merge 的思路 -->

### 3.3. Kth Largest Element in an Array

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
              swap(nums, new Random().nextInt(R-L+1)+L, R);
              for (int i = L; i < R; i++) 
                  if (nums[i] > nums[R]) 
                      swap(nums, i, pivot++);
              swap(nums, pivot, R);
              return pivot;   
          }
          return -1;
      }
      
      private void swap(int[] a, int i, int j) {
          int tmp = a[i];
          a[i] = a[j];
          a[j] = tmp;
      }
  }
  ```

## 4. 双索引 Two Pointer

### 4.1. Two Sum II - Input array is sorted

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

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/24/1236ec524e9180c8a1a0947ee94f7826.jpg)

    ```java
    // 时间复杂度：O(nlogn)
    // 空间复杂度：O(1)
    public class Solution2 {
        public int[] twoSum(int[] numbers, int target) {
            if(numbers.length < 2 /*|| !isSorted(numbers)*/)
                throw new IllegalArgumentException("Illegal argument numbers");
            for(int i = 0 ; i < numbers.length - 1 ; i ++) {
                int j = binarySearch(numbers, i+1, numbers.length-1, target - numbers[i]);
                if(j != -1){
                    int[] res = {i+1, j+1};
                    return res;
                }
            }
            throw new IllegalStateException("The input has no solution");
        }

        private int binarySearch(int[] nums, int l, int r, int target) {
            if(l < 0 || l > nums.length)
                throw new IllegalArgumentException("l is out of bound");

            if(r < 0 || r > nums.length)
                throw new IllegalArgumentException("r is out of bound");

            while(l <= r){
                int mid = l + (r - l)/2;
                if(nums[mid] == target)
                    return mid;
                if(target > nums[mid])
                    l = mid + 1;
                else
                    r = mid - 1;
            }
            return -1;
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

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/24/72b5e6b4f06f34d5ab1a407433fac3fc.jpg)

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

### 4.2. Valid Palindrome

[125. Valid Palindrome](https://leetcode.com/problems/valid-palindrome/description/)

### 4.3. Reverse String

[344. Reverse String](https://leetcode.com/problems/reverse-string/)

### 4.4. Reverse Vowels of a String

[345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/description/)

### 4.5. Container With Most Water

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/description/)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/24/ad145121d2296bc0e4d0fe0b51c7ad24.jpg)

## 5. 滑动窗口

在 ACM 中一般将滑动窗口技巧称为“尺取法”。

### 5.1. Minimum Size Subarray Sum

[209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/description/)

- Question
  > Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.
  > 
  > For example, given the array [2,3,1,2,4,3] and s = 7, the subarray [4,3] has the minimal length under the problem constraint.

- Solution
  - 暴力解法：遍历所有连续子数组再逐一验证，O(n^3)。
  - 滑动窗口

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/24/4de6893405d014a7533ad5a47dfc54f9.jpg)

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

### 5.2. Longest Substring Without Repeating Characters

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

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/24/5838a5f1d656c82e3d898d50559d1e2b.jpg)

  ```java
  // 时间复杂度：O(len(s))
  // 空间复杂度：O(len(charset))
  public class Solution2 {
      public int lengthOfLongestSubstring(String s) {
          int[] freq = new int[256];
          int l = 0, r = -1; // 滑动窗口为 s[l...r]
          int res = 0;
          while(r + 1 < s.length()) {
              if(freq[s.charAt(r+1)] == 0)
                  freq[s.charAt(++r)] ++;
              else    //freq[s[r+1]] == 1
                  freq[s.charAt(l++)] --;
              res = Math.max(res, r-l+1);
          }
          return res;
      }
  }
  ```

### 5.3. Find All Anagrams in a String

[438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/description/)

### 5.4. Minimum Window Substring

[76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)

## 6. Refer Links
