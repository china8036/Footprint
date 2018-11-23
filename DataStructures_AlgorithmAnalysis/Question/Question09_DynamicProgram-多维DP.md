- [Dynamic programming 多维 DP](#dynamic-programming-多维-dp)
  - [1. 着色问题](#1-着色问题)
    - [1.1. 不同颜色球排列 Ⅰ -- 组合 DP](#11-不同颜色球排列-Ⅰ----组合-dp)
    - [1.2. 不同颜色球排列 Ⅱ -- 组合 DP](#12-不同颜色球排列-Ⅱ----组合-dp)
    - [1.3. 环形涂色 -- 环形 DP](#13-环形涂色----环形-dp)
  - [2. Refer Links](#2-refer-links)

# Dynamic programming 多维 DP

## 1. 着色问题

### 1.1. 不同颜色球排列 Ⅰ -- 组合 DP

- Question

  ByteDance 2018.9.20 笔试第 4 题
  > 有 3 种颜色的球各 n、m、k 个，现将这些球排成一行，要求相邻的球颜色不能相同，问有几种排列方式。其中，`3 <= n,m,k <= 20`。

- Solution
  - 暴力回溯
    ```java
    public class Main {
      public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int m = in.nextInt();
        int k = in.nextInt();
        System.out.println(solution(n, m, k));
      }

      private static long res = 0L;

      private static long solution(int n, int m, int k) {
        __solution(n, m, k, -1);
        return res;
      }

      private static void  __solution(int n, int m, int k, int last) {
        if (n + m + k == 0) {
          res++;
          return;
        }
        for (int i = 0; i < 3; i++) {
          if (i == last)
            continue;
          if (i == 0 && n > 0)
            __solution(n-1, m, k, 0);
          else if (i == 1 && m > 0)
            __solution(n, m-1, k, 1);
          else if (i == 2 && k > 0)
            __solution(n, m, k-1, 2);
        }
      }
    }
    ```
  - 动态规划

    设 f(n,m,k,i) 表示剩余 n 个白色、m 个黑色、k 个红色，且最后一个球的颜色为 i（用 0、1、2 分别表示 3 种颜色）时方案的总数，则状态转移方程为：
    ```
    f(n,m,k,i) =
      f(n+1,m,k,1) + f(n+1,m,k,2) (i=0);
      f(n,m+1,k,0) + f(n,m+1,k,2) (i=1);
      f(n,m,k+1,0) + f(n,m,k+1,1) (i=2);
    ```
    由于数组索引表示的是剩余的数量，因此**在动态规划过程中应该使用递减顺序来赋值**。

    ```java
    public class Main {
      public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int n = in.nextInt();
        int m = in.nextInt();
        int k = in.nextInt();
        System.out.println(solution(n, m, k));
      }

      private static long solution(int n, int m, int o) {
        long [][][][] dp = new long[n+2][m+2][o+2][3];
        for (int i = n; i <= n + 1; i++)
          for (int j = m; j <= m + 1; j++)
            for (int k = o; k <= o + 1; k++)
              for (int h = 0; h < 3; h++)
                dp[i][j][k][h] = 0;

        dp[n - 1][m][o][0] = 1;
        dp[n][m - 1][o][1] = 1;
        dp[n][m][o - 1][2] = 1;

        for (int i = n; i >= 0; i--) {
          for (int j = m; j >= 0; j--) {
            for (int k = o; k >= 0; k--) {
              dp[i][j][k][0] += dp[i + 1][j][k][1] + dp[i + 1][j][k][2];
              dp[i][j][k][1] += dp[i][j + 1][k][0] + dp[i][j + 1][k][2];
              dp[i][j][k][2] += dp[i][j][k + 1][0] + dp[i][j][k + 1][1];
            }
          }
        }
        return dp[0][0][0][0] + dp[0][0][0][1] + dp[0][0][0][2];
      }
    }
    ```

### 1.2. 不同颜色球排列 Ⅱ -- 组合 DP

https://blog.csdn.net/Vectorxj/article/details/51491754

- Question
  > 有 k 种颜色的球各 C1、C2、C3...Ck 个，现将这些球排成一行，要求相邻的球颜色不能相同，问有几种排列方式。其中，`1 <= k <= 15`, `1 <= Ci <= 5`。

- Solution

### 1.3. 环形涂色 -- 环形 DP

https://blog.csdn.net/update7/article/details/79770832

https://blog.csdn.net/Tc_To_Top/article/details/48934857

https://blog.csdn.net/fyy2603/article/details/54767836

## 2. Refer Links

[动态规划的状态确定 -“不能连续跳两级的青蛙跳”和“栅栏涂色”](https://www.jianshu.com/p/e5dd3cdf158a?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)