- [Greedy](#greedy)
	- [1. Assign Cookies](#1-assign-cookies)
	- [2. Is Subsequence](#2-is-subsequence)
	- [3. Non-overlapping Intervals](#3-non-overlapping-intervals)
	- [4. Packets](#4-packets)
	- [5. Refer Links](#5-refer-links)

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
  
  使用贪心算法，**每次尝试将最大价值的 cookie 分给最贪心的 children，若无法满足，说明所有 cookie 都无法满足该 children，则放弃此 children**。

  ![image](http://img.cdn.firejq.com/jpg/2018/4/24/99e4a0f1b1deccaaa2c27e5d0278367f.jpg)
  
  代码实现：
  ```cpp
  // 时间复杂度：O(nlogn)
  // 空间复杂度：O(1)
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

- Question
	> Given a string s and a string t, check if s is subsequence of t.

- Solution
	```java
	public boolean isSubsequence(String s, String t) {  
			int sLen = s.length();  
			int tLen = t.length();  
			int i = 0, j = 0;  
			for (; i < tLen && j < sLen; i++)
					if (s.charAt(j) == t.charAt(i))
							j++;  
			return j == sLen;  
	}  
	```

## 3. Non-overlapping Intervals

[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/description/)

- Question
	> Given a collection of intervals, find the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

- Solution
	
	贪心算法：**按照区间的结尾进行排序，每次都选择结尾最早且和前一个区间不重叠的区间加入最长不重叠区间，最后返回区间总数与最长不重叠区间长度的差即可**。

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

## 4. Packets

[POJ 1017 Packets](http://poj.org/problem?id=1017)

- Question

	> 工厂计划出售边长分别为 1,2,3,4,5,6 的正方形板子，但工厂现在只有 `6*6` 的板子，其他的板子都只能从这种板子上裁剪而来的。现在给出分别这些不同规格的板子的需求量，问最少需要多少块 `6*6` 的板子。

	```
	Sample Input

	0 0 4 0 0 1 
	7 5 1 0 0 0 
	0 0 0 0 0 0 
	Sample Output
	2 
	1 
	```

- Solution

	生活常识是，桶里先放碎石，再放沙，最后还可以装不少水。因此，此题可**使用贪心策略，从面积大的开始**。一块 `6*6` 的板子可以裁剪成：
	- 一块 `6*6` 的板子
	- 一块 `5*5` 的板子和 11 块 `1*1` 的板子
	- 一块 `4*4` 的板子和 5 块 `2*2` 的板子
	- 四块 `3*3` 的板子。
		- 在一块 `6*6` 板子上取 `3*3` 的板子数目为 1/2/3/4 的时候，剩下可以取 `2*2` 的板子数量分别为 5/3/1/0。
		- 剩余部分可以取 `1*1` 的板子；若`2*2`的板子有剩余，也可以取 `1*1`的板子。
	
	```java
	public class Main {
		public static void main(String[] args) {
			int [] a = new int[7];
			Scanner in = new Scanner(System.in);
			// eg: 6 4 1 0 0 0
			for (int i = 1; i <= 6; i++)
				a[i] = in.nextInt();
			System.out.println(solve(a));
		}
		
		public static int solve(int [] a) {
			// 1. 解决边长为 6、5、4、3 的
			// 计算边长为 3 4 5 6 的板子消耗量
			int res = a[6] + a[5] + a[4] + (a[3] + 3) / 4; // 注意调整取整

			// 2. 解决边长为 2 的
			// 记录当 `3*3` 的数量分别为 4/1/2/3 时，空隙可以放 `2*2` 的数量
			int [] dir = {0, 5, 3, 1};
			// 计算已消耗的板子的空隙能容纳的边长为 2 的板子数量
			int cnt_2 = a[4] * 5 + dir[a[3] % 4];
			// 当上面剩余的 2*2 板子量不足时，还需要消耗新的板子
			if (a[2] > cnt_2)
				res += (a[2] - cnt_2 + 8) / 9; // 注意调整取整

			// 3. 解决边长为 1 的
			// 计算已消耗的板子的空隙能容纳的边长为 1 的板子数量
			int cnt_1 = res * 36 - a[6] * 36 - a[5] * 25 - a[4] * 16 - a[3] * 9 - a[2] * 4;
			// 当上面剩余的 1*1 板子量不足时，还需要消耗新的板子
			if (a[1] > cnt_1)
				res += (a[1] - cnt_1 + 35) / 36; // 注意调整取整

			return res;
		}
	}
	```

## 5. Refer Links