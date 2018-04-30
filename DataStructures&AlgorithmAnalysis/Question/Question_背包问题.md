- [背包问题](#%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98)
  - [1. 定义](#1-%E5%AE%9A%E4%B9%89)
  - [2. 分类](#2-%E5%88%86%E7%B1%BB)
  - [3. 解法](#3-%E8%A7%A3%E6%B3%95)
    - [3.1. -背包问题](#31--%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98)
      - [3.1.1. 第一类问题](#311-%E7%AC%AC%E4%B8%80%E7%B1%BB%E9%97%AE%E9%A2%98)
      - [3.1.2. 第二类问题](#312-%E7%AC%AC%E4%BA%8C%E7%B1%BB%E9%97%AE%E9%A2%98)
    - [3.2. 背包内物品的权值都一样，只有重量不一样，要让数量尽量多的背包问题](#32-%E8%83%8C%E5%8C%85%E5%86%85%E7%89%A9%E5%93%81%E7%9A%84%E6%9D%83%E5%80%BC%E9%83%BD%E4%B8%80%E6%A0%B7%EF%BC%8C%E5%8F%AA%E6%9C%89%E9%87%8D%E9%87%8F%E4%B8%8D%E4%B8%80%E6%A0%B7%EF%BC%8C%E8%A6%81%E8%AE%A9%E6%95%B0%E9%87%8F%E5%B0%BD%E9%87%8F%E5%A4%9A%E7%9A%84%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%98)
    - [3.3. TODO:](#33-todo)

# 背包问题

## 1. 定义

背包问题（Knapsack problem）是一种组合优化的 NP 完全问题。问题可以描述为：给定一组物品，每种物品都有自己的重量和价格，在限定的总重量内，我们如何选择，才能使得物品的总价格最高。问题的名称来源于如何选择最合适的物品放置于给定背包中。

在计算机科学领域，人们对背包问题感兴趣的原因在于：
- 利用动态规划，背包问题存在一个伪多项式时间算法
- 把上面算法作为子程序，背包问题存在完全逼近多项式时间方案
- 作为 NP 完全问题，背包问题没有一种既准确又快速（多项式时间）的算法

## 2. 分类

- 0-1 背包问题
  
  有 n 种物品，如果限定每种物品只能选择 0 个或 1 个，则问题称为 0-1 背包问题。0-1 背包问题是最基本的背包问题。各类复杂的背包问题总可以变换为简单的 0-1 背包问题进行求解。

	- 不考虑重量只考虑权值（可理解为：背包可背无限重的东西），要求选出若干个项，使他们的权值之和刚好等于 target。
	- 考虑重量和权值，物品 j 的重量为 wj，权值为 pj（假定所有物品的重量和价格都是非负的）背包所能承受的最大重量为 W，要求选出若干个项，再总重量不超过 W 的前提下，使他们的权值之和尽量大。

- 无界背包问题（完全背包问题）
  
  如果不限定每种物品的数量，则问题称为无界背包问题。

- 有界背包问题（多重背包问题）

  如果限定物品 j 最多只能选择 b 个，则问题称为有界背包问题。

- 部分背包问题

  每个项可以按比例只取走一部分（比如盐、糖之类的），则称为部分背包问题。

- 最优装载问题

  背包内物品的权值都一样（直接忽略该因素），重量不一样，要求选择数量尽量多的物体，但总重量不超过 W。

- 乘船问题

  有 n 个人，第 i 个人重量为 wi。每艘船最大载重量为 W，且最多只能乘坐两个人。求方案使得用最少的船装载所有的人。

- 推广的背包问题还有二次背包问题，多维背包问题，多目标背包问题等。

## 3. 解法

http://love-oriented.com/pack/

### 3.1. -背包问题

#### 3.1.1. 第一类问题

http://www.cnblogs.com/Myhsg/archive/2009/08/29/1556460.html

http://mrknight.info/algorithm/2015/04/12/Simple-Knapsack/

http://www.programgo.com/article/9139994208/;jsessionid=255510294E5DEB1A8463B76763F43111

不考虑重量只考虑权值（可理解为：背包可背无限重的东西），要求选出若干个项，使他们的权值之和刚好等于 target。

- 解法 1：递归（回溯）实现
  ```cpp
  //weight[] 存放各个项的权值
  //knapsack（t, i）将回答是否能从权值 weight[i]~weight[n] 之间选出一些数，使得总和为 t，并在可能的情况下将所选定的权值打印出来
  bool knapsack(int t, int i) {
  if (t == 0) return true;
  else {
  if (t < 0 || i > n) return false;
      else {
        if (knapsack(t - weight[i], i + 1)) {
          //.... 打印 weight[i] 的代码
          return true;
        } else {
          return knapsack(t, i + 1);// 不可能有 i 的解
        }
      }
    }
  }
  ```

- 解法 2：非递归实现（用栈实现）
  ```cpp
  #include <iostream>
  #include <vector>

  using namespace std;

  vector<int> temp;
  vector<vector<int> > result;
  void SimpleKnapsack(vector<int> items, int capacity, int i)
  {
    if (capacity == 0)
    {
      result.push_back(temp);
      return;
    }
    else if (i >= items.size() || items[i] > capacity)
      return;

    temp.push_back(items[i]);
    SimpleKnapsack(items, capacity - items[i], i + 1);
    
    temp.pop_back();
    SimpleKnapsack(items, capacity, i + 1);
  }
  void SimpleKnapsack_stack(vector<int> items, int capacity)
  {
    int n = items.size(), sum = 0, i = 0, start = 0;
    vector<int> stack;
    vector<int> visited(n, 0);
    
    while (1)
    {
      for (i = start; i < n; ++i)
      {
        if (!visited[i] && items[i] + sum <= capacity)
        {
          sum += items[i];
          stack.push_back(i);
          temp.push_back(items[i]);
          visited[i] = 1;
          if (sum == capacity)
          {
            result.push_back(temp);
            break;
          }
        }
      }
      
      if (i == n)
      {
        if (stack.empty())
          return;
        int top = stack.back();
        stack.pop_back();
        temp.pop_back();
        visited[top] = 0;
        sum -= items[top];
        start = top + 1;
      }
    }
  }
  int main()
  {
    int w[] = { 2, 3, 5, 7, 8};
    vector<int> items(w, w + 5);

    //SimpleKnapsack(items, 15, 0);
    SimpleKnapsack_stack(items, 15);
  }
  ```

#### 3.1.2. 第二类问题

考虑重量和权值，物品 j 的重量为 wj，权值为 pj（假定所有物品的重量和价格都是非负的）背包所能承受的最大重量为 W，要求选出若干个项，再总重量不超过 W 的前提下，使他们的权值之和尽量大。

- 解法 1：递归实现

  http://blog.csdn.net/buyingfei8888/article/details/8972103
  ```
  递归式：
    当 wn>C 时，  f(n,C)=f(n-1,C);
    当 wn<=C 时，f(n，C) = max(f(n-1,C), vn+f(n-1, C-wn) );
  初始条件为：f(i, 0) = 0; f(0,i) = 0; f(0,0) = 0;
  ```

  ```cpp
  #include <stdio.h>  
  #define MAX 100  

  int weight[MAX];  
  int price[MAX];  
  int y[MAX]={0};  
    
  // 进行递归主要方法  
  int f(int t,int c){  
      if((t==0)||c<=0){  // 当物品个数为 0 或背包容积为 0 事退出  
        return 0;  
      }else{  
          for(int i=t-1;i>=0;i--){    
              if(weight[i]>c){   // 当物品重量大于背包容积  
                  y[i]=0;        // 此时物品不被选中  
                  return f(t-1,c);  // 在剩余物品中选取  
              }else{  
              int temp1=f(t-1,c); // 当第 t 个物品没被选中时  
              int temp2=price[i]+f(t-1,c-weight[i]);// 被选中时  
                  if(temp1>temp2){  
                      y[i]=0;  
                      return f(t-1,c);  
                  }else{  
                      y[i]=1;  
                      return price[i]+f(t-1,c-weight[i]);  
                  }  
              }  
          }  
      }  
  }  
  int main(){  
      int c,t,maxval,i;  
      printf("请输入物品的的个数：");  
      scanf("%d",&t);  
      for(i=0;i<t;i++){  
          y[i]=0;  
          printf("\n 请输入第 %d 个物品的重量和价值",i+1);  
          scanf("%d%d",&weight[i],&price[i]);  
      }  
      printf("\n 请输入背包的容积");  
      scanf("%d",&c);  
      maxval=f(t,c);  
      printf("j 结果为：1 代表选中");  
      for(i=0;i<t;i++){  
          //if(y[i]==1)  
              printf("\n%d %d %d\n",y[i],weight[i],price[i]);  
      }  
      printf("总价值为：%d",maxval);  
      return 0;  
  }
  ```

- 解法 2：枚举 / 蛮力法 O（2^n）：列出所有可能的情况再选最优

- 解法 3：动态规划

### 3.2. 背包内物品的权值都一样，只有重量不一样，要让数量尽量多的背包问题

贪心法：

### 3.3. TODO: