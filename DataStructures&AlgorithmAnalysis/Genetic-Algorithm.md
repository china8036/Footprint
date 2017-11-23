- [遗传算法](#%E9%81%97%E4%BC%A0%E7%AE%97%E6%B3%95)
  - [概念](#%E6%A6%82%E5%BF%B5)
    - [智能优化算法](#%E6%99%BA%E8%83%BD%E4%BC%98%E5%8C%96%E7%AE%97%E6%B3%95)
  - [遗传算法](#%E9%81%97%E4%BC%A0%E7%AE%97%E6%B3%95)
  - [算法思想](#%E7%AE%97%E6%B3%95%E6%80%9D%E6%83%B3)
    - [几个关键概念](#%E5%87%A0%E4%B8%AA%E5%85%B3%E9%94%AE%E6%A6%82%E5%BF%B5)
    - [传统的遗传算法流程](#%E4%BC%A0%E7%BB%9F%E7%9A%84%E9%81%97%E4%BC%A0%E7%AE%97%E6%B3%95%E6%B5%81%E7%A8%8B)
  - [应用](#%E5%BA%94%E7%94%A8)
    - [自动组卷](#%E8%87%AA%E5%8A%A8%E7%BB%84%E5%8D%B7)
  - [GA 的改进](#ga-%E7%9A%84%E6%94%B9%E8%BF%9B)
    - [对编码方式的改进](#%E5%AF%B9%E7%BC%96%E7%A0%81%E6%96%B9%E5%BC%8F%E7%9A%84%E6%94%B9%E8%BF%9B)
    - [对遗传算子的改进](#%E5%AF%B9%E9%81%97%E4%BC%A0%E7%AE%97%E5%AD%90%E7%9A%84%E6%94%B9%E8%BF%9B)
    - [对控制参数的改进](#%E5%AF%B9%E6%8E%A7%E5%88%B6%E5%8F%82%E6%95%B0%E7%9A%84%E6%94%B9%E8%BF%9B)
    - [对执行策略的改进](#%E5%AF%B9%E6%89%A7%E8%A1%8C%E7%AD%96%E7%95%A5%E7%9A%84%E6%94%B9%E8%BF%9B)
  - [Refer Links](#refer-links)

# 遗传算法

## 概念

### 智能优化算法

智能优化算法又称为现代启发式算法，是一种具有全局优化性能、通用性强、且适用于并行处理的算法。

这种算法一般具有严密的理论依据，而不是单纯凭借专家经验，理论上可以在一定的时间内找到最优解或近似最优解。

常见的智能优化算法：

- 遗传算法 (Genetic Algorithm, 简称 GA)；

- 模拟退火算法 (Simulated Annealing, 简称 SA)；

- 禁忌搜索算法 (Tabu Search, 简称 TS)；

## 遗传算法

遗传算法 (Genetic Algorithm) 是一种模拟自然界的进化规律 - 优胜劣汰演化来的启发式搜索算法，其在解决多种约束条件下的最优解这类问题上具有优秀的表现，在 1975 年第一次被提出。

## 算法思想

### 几个关键概念

1. 基因 ( Gene ) ：一个遗传因子。

2. 染色体 ( Chromosome ) ：一组的基因。

3. 个体 ( individual )：单个生物。在遗传算法中，个体一般只包含一条染色体。

4. 种群 ( Population )：由个体组成的群体。生物的进化以种群的形式进化。

5. 适者生存 ( The survival of the fittest )：对环境适应度高的个体参与繁殖的机会比较多，后代就会越来越多。适应度低的个体参与繁殖的机会比较少，后代就会越来越少。

### 传统的遗传算法流程

1.	初始化 (initialize)： 随机生成一个规模为 N 的种群，设置最大进化次数以及停止进化条件；

2.	计算适应度 (fitness)： 适应度被用来评价个体的质量，且适应度是唯一评判因子。计算种群中每个个体的适应度，得到最优秀的个体；

3.	适者生存 / 选择 (selection)：选择是用来得到一些优秀的个体来产生下一代。选择算法的好坏至关重要，因为在一定程度上选择会影响种群的进化方向。常用的选择算法有：随机抽取、竞标赛选择以及轮盘赌模拟法等等；

4.	交叉配对 (crossover)：交叉是两个个体繁衍下一代的过程，实际上是子代获取父亲和母亲的部分基因，即基因重组。常用的交叉方法有：单点交叉、多点交叉等；

5.	变异 (mutation)：变异即模拟突变过程。通过变异，种群中个体变得多样化，但是变异是有一个概率的；

其中最重要的过程就是选择和交叉配对：

- **选择**要能够合理的反映"适者生存"的自然法则，而**交叉配对**必须将有利的基因尽量遗传给下一代(这个算法很关键)；

- 编码的过程要能够使编码后的染色体能充分反映个体的特征并且能够方便计算。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/3f349bff40d651f38289f904a87ca72f.jpg)

根据上面的流程图，遗传算法包含三个基本操作：选择、交叉和变异：

1.	选择，也就是流程图中"根据适应度进行串赋值"。如下图所示，当一个种群三个个体进行自然选择时，适应度越大则被选择的概率就越大。

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/88223cfface972700ddd38101ae0c60a.jpg)

2.	交叉，两条染色体相互交换基因片段。遗传算法交叉比人体内染色体交叉要简单许多。遗传算法的染色体是单倍体，而人体内的真正的染色体是双倍体。下图是遗传算法中两条染色体在中间进行交叉的示意图。

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/bc5b4e4e5daa92b4c5a602e4fa204b1c.jpg)

3.	变异，某个基因位发生变化。下图是遗传算法中一条染色体在第二位发生基因变异的示意图。

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/72c1a82f615b13478b253bfe2a9c554c.jpg)

上述的选择、交叉和变异是最简单的类型，而且并不是所有问题的解决方案都可以编码成 0-1 向量的形式。实际上，应用遗传算法的主要工作是设计编码方案、交叉过程、变异过程和选择过程。

## 应用

[遗传算法的应用](http://www.algorithmdog.com/%E9%81%97%E4%BC%A0%E7%AE%97%E6%B3%95%E7%B3%BB%E5%88%97%E4%B9%8B%E4%BA%8C%E6%84%9A%E5%BC%84%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E7%9A%84%E9%81%97%E4%BC%A0%E7%AE%97%E6%B3%95 )

[遗传算法有哪些有趣的应用](https://www.zhihu.com/question/20085479 )

遗传算法的应用领域：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/7172664c5e693e1d28431cbdcc15b0bb.jpg)


[句子配对](https://morvanzhou.github.io/tutorials/machine-learning/evolutionary-algorithm/2-02-genetic-algorithm-match-phrase/)

[旅行商人](https://morvanzhou.github.io/tutorials/machine-learning/evolutionary-algorithm/2-03-genetic-algorithm-travel-sales-problem/)


### 自动组卷

https://www.cnblogs.com/artwl/archive/2011/05/19/2051556.html

https://www.cnblogs.com/artwl/archive/2011/05/20/2052262.html

http://www.jianshu.com/p/7fe9d3bb00ac

http://blog.csdn.net/localhost01/article/details/52141554

## GA 的改进

### 对编码方式的改进

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/a8a7064a37b205dc56b71f4e1fb24d45.jpg)

### 对遗传算子的改进

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/3d0881cda75d7c9ea28a2953fa7b9664.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/82695dba0a5c2d46e47e89310642af3b.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/d5ed992072d0e80a7124b3a70a8045fa.jpg)

### 对控制参数的改进

### 对执行策略的改进

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/20/a0ddfd0ba2e4907eff58f1e8c1863934.jpg)

## Refer Links

http://dataunion.org/10694.html

http://www.algorithmdog.com/ml/ga 

http://blog.sciencenet.cn/blog-234554-228907.html 

http://blog.csdn.net/b2b160/article/details/4680853/ （遗传算法手工模拟计算示例 -- 通俗易懂）

https://www.zhihu.com/question/23293449 

https://morvanzhou.github.io/tutorials/machine-learning/evolutionary-algorithm/2-01-genetic-algorithm/ （莫烦的遗传算法教程）