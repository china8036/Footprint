- [Map 和 Set](#map--set)
  - [1. 使用 set 结构](#1--set)
    - [1.1. Intersection of Two Arrays](#11-intersection-of-two-arrays)
    - [1.2. Happy Number](#12-happy-number)
    - [1.3. Contains Duplicate](#13-contains-duplicate)
    - [1.4. Contains Duplicate II](#14-contains-duplicate-ii)
    - [1.5. Contains Duplicate III](#15-contains-duplicate-iii)
  - [2. 使用 map 结构](#2--map)
    - [2.1. Intersection of Two Arrays II](#21-intersection-of-two-arrays-ii)
    - [2.2. Valid Anagram](#22-valid-anagram)
    - [2.3. Word Pattern](#23-word-pattern)
    - [2.4. Isomorphic Strings](#24-isomorphic-strings)
    - [2.5. Sort Characters By Frequency](#25-sort-characters-by-frequency)
    - [2.6. Two Sum](#26-two-sum)
    - [2.7. Tree-Sum](#27-tree-sum)
    - [2.8. Four-Sum](#28-four-sum)
    - [2.9. Three-Sum Closest](#29-three-sum-closest)
    - [2.10. Four-Sum II](#210-four-sum-ii)
    - [2.11. Group Anagrams](#211-group-anagrams)
    - [2.12. Number of Boomerangs](#212-number-of-boomerangs)
    - [2.13. Max Points on a Line](#213-max-points-on-a-line)
  - [3. Refer Links](#3-refer-links)
  
# Map 和 Set

Map 和 Set 数据结构可以使用 HashTable 或平衡 BST 实现。使用平衡 BST 实现的版本具有数据的顺序性，而使用 HashTable 实现的版本操作时间效率更高。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/85fed795cf64a2532bc134d85b5abbde.jpg)

- 在 Java 中，使用 HashTable 实现的为 HashMap 和 HashSet，使用平衡 BST 实现的为 TreeMap 和 TreeSet。
- 在 C++ 中，使用 HashTable 实现的为 unordered_map 和 unordered_set，使用平衡 BST 实现的为 map 和 set。

Map 和 Set 数据结构广泛应用于查找问题中，查找问题一般分为以下 2 种：
- 查找有无 - 使用 set 结构
- 查找 K-V 关系 - 使用 map 结构

## 1. 使用 set 结构

### 1.1. Intersection of Two Arrays

[349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/description/)

- Question
  > Given two arrays, write a function to compute their intersection.

  Example:
  ```
  Given nums1 = [1, 2, 2, 1], nums2 = [2, 2], return [2].
  ```
  Note:
  - Each element in the result must be unique.
  - The result can be in any order.
- Solution
  
  使用 set 数据结构进行求解：
  ```java
  // 时间复杂度：O(len(nums1)+len(nums2))
  // 空间复杂度：O(len(nums1))
  public class Solution {
      public int[] intersection(int[] nums1, int[] nums2) {
          HashSet<Integer> record = new HashSet<>();
          for(int num: nums1)
              record.add(num);

          HashSet<Integer> resultSet = new HashSet<>();
          for(int num: nums2)
              if(record.contains(num))
                  resultSet.add(num);

          int[] res = new int[resultSet.size()];
          int index = 0;
          for(Integer num: resultSet)
              res[index++] = num;

          return res;
      }
  }
  ```

### 1.2. Happy Number

[202. Happy Number](https://leetcode.com/problems/happy-number/description/)

  
### 1.3. Contains Duplicate

[217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/description/)

- Question
  > Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.
- Solution
  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(n)
  class Solution {
      public boolean containsDuplicate(int[] nums) {
          if (nums == null || nums.length <= 1)
              return false;
          HashSet<Integer> record = new HashSet<>();
          for (int i = 0; i < nums.length; i++)
              if (!record.add(nums[i]))
                  return true;
          return false;
      }
  }
  ```

### 1.4. Contains Duplicate II

[219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/description/)

- Question
  > Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the absolute difference between i and j is at most k.
- Solution

  可使用滑动窗口结合查找表进行求解：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/923e6fd70e9599480321848923152fc7.jpg)

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/0ef81a1edd7694491e4360706f84d711.jpg)

  ```java
  // 时间复杂度：O(n)
  // 空间复杂度：O(k)
  public class Solution {
      public boolean containsNearbyDuplicate(int[] nums, int k) {
          if(nums == null || nums.length <= 1 || k <= 0)
              return false;
          HashSet<Integer> record = new HashSet<>();
          for(int i = 0 ; i < nums.length; i ++){
              if(!record.add(nums[i]))
                  return true;
              if(record.size() == k + 1)
                  record.remove(nums[i-k]);
          }
          return false;
      }
  }
  ```

### 1.5. Contains Duplicate III

[220. Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/description/)

- Question
  > Given an array of integers, find out whether there are two distinct indices i and j in the array such that the absolute difference between nums[i] and nums[j] is at most t and the absolute difference between i and j is at most k.
- Solution

  同样可以使用滑动窗口来求解此问题，则需要在窗口中寻找以下元素：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/d2863163adb9f45ad2b129a67a7dfdd8.jpg)

  若将这些元素放入**具有顺序性的查找表**中，使用 ceil 和 floor 操作将非常方便：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/9fffd463423b84122e750046043c3d8d.jpg)

  因此，这里使用基于红黑树实现的 set 结构 TreeSet 作为查找表：
  ```java
  // 时间复杂度：O(nlogk)
  // 空间复杂度：O(k)
  public class Solution {
      public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
          // 使用 Long 防止数据溢出
          TreeSet<Long> record = new TreeSet<>();
          for(int i = 0 ; i < nums.length ; i ++) {
              if(record.ceiling((long)nums[i] - (long)t) != null &&
                      record.ceiling((long)nums[i] - (long)t) <= (long)nums[i] + (long)t)
                  return true;
              record.add((long)nums[i]);
              if(record.size() == k + 1)
                  record.remove((long)nums[i-k]);
          }
          return false;
      }
  }
  ```

## 2. 使用 map 结构

### 2.1. Intersection of Two Arrays II

[350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/description/)

- Question
  > Given two arrays, write a function to compute their intersection.

  Example:
  ```
  Given nums1 = [1, 2, 2, 1], nums2 = [2, 2], return [2, 2].
  ```
  Note:
  - Each element in the result should appear as many times as it shows in both arrays.
  - The result can be in any order.
  Follow up:
  - What if the given array is already sorted? How would you optimize your algorithm?
  - What if nums1's size is small compared to nums2's size? Which algorithm is better?
  - What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?
- Solution

  由题意可知这是一个典型的键值对应的问题，每一个元素都对应其出现的频次，因此应该使用 map 结构进行求解。使用 map 记录 nums1 中所有元素及其频次，并将 nums2 中的元素与之比较，进而决定该元素是否是两个数组的交集。
  ```java
  // 时间复杂度：O(len(nums1)+len(nums2))
  // 空间复杂度：O(len(nums1))
  public class Solution {
      public int[] intersect(int[] nums1, int[] nums2) {
          HashMap<Integer, Integer> record = new HashMap<Integer, Integer>();
          for(int num: nums1)
              if(!record.containsKey(num))
                  record.put(num, 1);
              else
                  record.put(num, record.get(num) + 1);
          ArrayList<Integer> result = new ArrayList<Integer>();
          for(int num: nums2)
              if(record.containsKey(num) && record.get(num) > 0){
                  result.add(num);
                  record.put(num, record.get(num) - 1);
              }
          int[] ret = new int[result.size()];
          int index = 0;
          for(Integer num: result)
              ret[index++] = num;

          return ret;
      }
  }
  ```

### 2.2. Valid Anagram

[242. Valid Anagram](https://leetcode.com/problems/valid-anagram/description/)

### 2.3. Word Pattern

[290. Word Pattern](https://leetcode.com/problems/word-pattern/description/)

### 2.4. Isomorphic Strings

[205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/description/)

### 2.5. Sort Characters By Frequency

[451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/description/)

### 2.6. Two Sum

[1. Two Sum](https://leetcode.com/problems/two-sum/description/)

- Question
  > Given an array of integers, return indices of the two numbers such that they add up to a specific target.
  > 
  > You may assume that each input would have exactly one solution, and you may not use the same element twice.

  Example:
  ```
  Given nums = [2, 7, 11, 15], target = 9,

  Because nums[0] + nums[1] = 2 + 7 = 9,
  return [0, 1].
  ```
- Solution
  - 暴力解法：O(n^2)
  - 先排序，再使用双索引对撞：O(nlogn)
  - 使用 map 查找表：O(n)
    ```java
    // 时间复杂度：O(n)
    // 空间复杂度：O(n)
    public class Solution {
        public int[] twoSum(int[] nums, int target) {
            HashMap<Integer, Integer> record = new HashMap<>();
            for(int i = 0; i < nums.length; i ++){
                int complement = target - nums[i];
                if(record.containsKey(complement)){
                    int[] res = {i, record.get(complement)};
                    return res;
                }
                record.put(nums[i], i);
            }
            throw new IllegalStateException("the input has no solution");
        }
    }
    ```

### 2.7. Tree-Sum

[15. 3Sum](https://leetcode.com/problems/3sum/description/)

### 2.8. Four-Sum

[18. 4Sum](https://leetcode.com/problems/4sum/description/)

### 2.9. Three-Sum Closest

[16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/description/)

### 2.10. Four-Sum II

[454. 4Sum II](https://leetcode.com/problems/4sum-ii/description/)

- Question
  > Given four lists A, B, C, D of integer values, compute how many tuples (i, j, k, l) there are such that A[i] + B[j] + C[k] + D[l] is zero.
  >
  > To make problem a bit easier, all A, B, C, D have same length of N where 0 ≤ N ≤ 500. All integers are in the range of -2^28 to 2^28 - 1 and the result is guaranteed to be at most 2^31 - 1.

  Example:
  ```
  Input:
  A = [ 1, 2]
  B = [-2,-1]
  C = [-1, 2]
  D = [ 0, 2]

  Output:
  2

  Explanation:
  The two tuples are:
  1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
  2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
  ```
- Solution

  若直接遍历四个数组，时间效率为 O(n^4)；若将 D 放入查找表中，时间效率 O(n^3)；若将 C+D 的每一种可能放入查找表中，时间效率为 O(n^2)。由于需要记录每一种可能出现的次数，因此需要使用 map 结构。
  ```java
  // 时间复杂度：O(n^2)
  // 空间复杂度：O(n^2)
  public class Solution {
      public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
          if(A == null || B == null || C == null || D == null)
              throw new IllegalArgumentException("Illegal argument");
          HashMap<Integer, Integer> map = new HashMap<>();
          for(int i = 0; i < C.length; i ++)
              for(int j = 0; j < D.length; j ++) {
                  int sum = C[i] + D[j];
                  map.put(sum, map.getOrDefault(sum, 0) + 1);
              }
          int res = 0;
          for(int i = 0; i < A.length; i ++)
              for(int j = 0; j < B.length; j ++)
                  res += map.getOrDefault(-1 * (A[i]+B[j]), 0);
          return res;
      }
  }
  ```

### 2.11. Group Anagrams

[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/description/)

### 2.12. Number of Boomerangs

[447. Number of Boomerangs](https://leetcode.com/problems/number-of-boomerangs/description/)

- Question
  > Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points (i, j, k) such that the distance between i and j equals the distance between i and k (the order of the tuple matters).
  > 
  > Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).

  Example:
  ```
  Input:
  [[0,0],[1,0],[2,0]]

  Output:
  2

  Explanation:
  The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]
  ```
- Solution

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/25/48cfbe0fea117af91a26303e7f4c063e.jpg)

  ```java
  // 时间复杂度：O(n^2)
  // 空间复杂度：O(n)
  public class Solution {
      public int numberOfBoomerangs(int[][] points) {
          int res = 0;
          for( int i = 0 ; i < points.length ; i ++ ) {
              // record 中存储点 i 到所有其他点的距离出现的频次
              HashMap<Integer, Integer> record = new HashMap<>();
              for(int j = 0 ; j < points.length ; j ++)
                  if(j != i) { // 不包括自身
                      // 计算距离时不进行开根运算，以保证精度
                      int dis = dis(points[i], points[j]);
                      record.put(dis, record.getOrDefault(dis, 0) + 1);
                  }
              for(Integer dis: record.keySet())
                  res += record.get(dis) * (record.get(dis) - 1);
          }
          return res;
      }

      private int dis(int[] pa, int pb[]){
          // 需要考虑数据溢出问题
          return (pa[0] - pb[0]) * (pa[0] - pb[0]) +
                (pa[1] - pb[1]) * (pa[1] - pb[1]);
      }
  }
  ```

### 2.13. Max Points on a Line

[149. Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/description/)

## 3. Refer Links