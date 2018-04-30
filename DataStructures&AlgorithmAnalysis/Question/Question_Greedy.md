- [Greedy](#greedy)
  - [1. Assign Cookies](#1-assign-cookies)
  - [2. Is Subsequence](#2-is-subsequence)
  - [3. Non-overlapping Intervals](#3-non-overlapping-intervals)
  - [4. Refer Links](#4-refer-links)

# Greedy

## 1. Assign Cookies

[455. Assign Cookies](https://leetcode.com/problems/assign-cookies/description/)

- Question
  > Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.
  Note:
  - You may assume the greed factor is always positive. 
  - You cannot assign more than one cookie to one child.
  Example 1:
  ```
  Input: [1,2,3], [1,1]

  Output: 1

  Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
  And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
  You need to output 1.
  ```
  Example 2:
  ```
  Input: [1,2], [1,2,3]

  Output: 2

  Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
  You have 3 cookies and their sizes are big enough to gratify all of the children, 
  You need to output 2.
  ```
- Solution
  
  使用贪心算法，每次尝试将最大价值的 cookie 分给最贪心的 children，若无法满足，说明所有 cookie 都无法满足该 children，则放弃此 children。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/24/99e4a0f1b1deccaaa2c27e5d0278367f.jpg)
  
  代码实现：
  ```cpp
  /// 时间复杂度：O(nlogn)
  /// 空间复杂度：O(1)
  class Solution {
  public:
      int findContentChildren(vector<int>& g, vector<int>& s) {
          // 降序排序
          sort(g.begin(), g.end(), greater<int>());
          sort(s.begin(), s.end(), greater<int>());

          int gi = 0, si = 0; // 指向初始最大价值的饼干和最贪心的小朋友
          int res = 0;
          while(gi < g.size() && si < s.size()){
              if(s[si] >= g[gi]){
                  res ++;
                  si ++;
                  gi ++;
              }
              else
                  gi ++;
          }

          return res;
      }
  };
  ```

## 2. Is Subsequence

[392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)

```java
class Solution {
    public boolean isSubsequence(String s, String t) {  
        int sLen = s.length();  
        int tLen = t.length();  
        int sIndex = 0;  
        for (int i = 0; i < tLen && sIndex < sLen; i++)
            if (s.charAt(sIndex) == t.charAt(i))
                sIndex++;  
        return sIndex == sLen;  
    }  
}
```

## 3. Non-overlapping Intervals

[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)

贪心算法：按照区间的结尾进行排序，每次都选择结尾最早且和前一个区间不重叠的区间加入最长不重叠区间，最后返回区间总数与最长不重叠区间长度的差即可。

```cpp
// Definition for an interval.
struct Interval {
    int start;
    int end;
    Interval() : start(0), end(0) {}
    Interval(int s, int e) : start(s), end(e) {}
};

bool compare(const Interval &a, const Interval &b){
    if(a.end != b.end)
        return a.end < b.end;
    return a.start < b.start;
}

// 时间复杂度：O(n), 空间复杂度：O(n)
class Solution {
public:
    int eraseOverlapIntervals(vector<Interval>& intervals) {

        if(intervals.size() == 0)
            return 0;

        sort(intervals.begin(), intervals.end(), compare);

        int res = 1;
        int pre = 0;
        for(int i = 1 ; i < intervals.size() ; i ++)
            if(intervals[i].start >= intervals[pre].end){
                res ++;
                pre = i;
            }

        return intervals.size() - res;
    }
};
```

## 4. Refer Links