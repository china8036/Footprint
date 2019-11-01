- [超参数优化 (Hyper parameter optimization, HPO)](#超参数优化-hyper-parameter-optimization-hpo)
  - [1. 基础概念](#1-基础概念)
    - [1.1. 超参数优化](#11-超参数优化)
    - [1.2. 超参优化算法](#12-超参优化算法)
  - [2. 随机类算法](#2-随机类算法)
    - [2.1. 穷举式优化算法：网络搜索 (Grid Search)](#21-穷举式优化算法网络搜索-grid-search)
    - [2.2. 采样式优化算法：随机搜索 (Random Search)](#22-采样式优化算法随机搜索-random-search)
  - [3. 拟合类算法：贝叶斯优化 (Bayesian Optimization)](#3-拟合类算法贝叶斯优化-bayesian-optimization)
    - [3.1. 基本概念](#31-基本概念)
    - [3.2. 算法流程](#32-算法流程)
    - [3.3. 算法特点](#33-算法特点)
    - [3.4. 代码实现](#34-代码实现)
  - [4. Hyperband 算法和 BOHB 算法](#4-hyperband-算法和-bohb-算法)
    - [4.1. Successive Halving 算法](#41-successive-halving-算法)
    - [4.2. Hyperband 算法](#42-hyperband-算法)
    - [4.3. BOHB 算法](#43-bohb-算法)
    - [4.4. 代码实现](#44-代码实现)
  - [5. 种群优化类算法](#5-种群优化类算法)
    - [5.1. 遗传算法 (Genetic algorithm)](#51-遗传算法-genetic-algorithm)
      - [5.1.1. 基本概念](#511-基本概念)
      - [5.1.2. 算法流程](#512-算法流程)
    - [5.2. 基于种群的训练 (Population Based Training，PBT)](#52-基于种群的训练-population-based-trainingpbt)
  - [6. 粒子群优化 (Paticle Swarm Optimization)](#6-粒子群优化-paticle-swarm-optimization)
  - [7. TPE (tree of parzen estimators)](#7-tpe-tree-of-parzen-estimators)
  - [8. SMAC](#8-smac)
  - [9. 现有框架](#9-现有框架)
  - [10. Refer Links](#10-refer-links)

# 超参数优化 (Hyper parameter optimization, HPO)

## 1. 基础概念

### 1.1. 超参数优化

超参数是指机器学习模型里面的框架参数，比如聚类方法里面类的个数或者话题模型里面话题的个数等，都称为超参数。它们跟训练过程中学习的参数（权重）是不一样的，通常是手工设定，不断试错调整。深度学习和神经网络模型，有很多这样的参数需要学习，而模型的训练过程是相对 expensive 的，不能通过快速计算获取大量样本。这就是为什么过去这么多年从业者弃之不顾的原因。以前给人的印象，深度学习就是“黑魔法”。

时至今日，非参数学习研究正在帮助深度学习更加自动的优化模型参数选择，当然有经验的专家仍然是必须的。每一个机器学习系统都有超参数，自动机器学习 (AutoML) 最基本的任务就是自动进行超参数优化 (Hyper parameter optimization, HPO)。深度神经网络的性能更是尤其依赖于网络的结构、正则项、最优化目标等相关超参数的选取。

机器学习模型超参数调优一般认为是一个黑盒优化问题。所谓黑盒问题，就是在超参数优化这个问题中，输入的是模型超参数，输出的是使得损失函数最小的一组超参数，而我们只能看到模型的输入和输出，不能获取模型训练过程的梯度信息，也不能假设模型超参数和最终指标符合凸优化条件，否则的话我们通过求导或者凸优化方法就可以求导最优解，不需要使用这些黑盒优化算法。我们输入超参数与最后需要优化的损失函数存在某种函数关系，但我们并不知道具体是什么关系，因此称为黑盒函数。

### 1.2. 超参优化算法

自动调参算法主要有 Grid search（网格搜索）、Random search（随机搜索）、Genetic algorithm（遗传算法）、Paticle Swarm Optimization（粒子群优化）、Bayesian Optimization（贝叶斯优化）、TPE、SMAC 等。

像遗传算法和 PSO 这些经典黑盒优化算法，可以归类为群体优化算法，也不是特别适合模型超参数调优场景，因为需要有足够多的初始样本点，并且优化效率不是特别高。

目前业界用得比较多的分别是 Grid search、Random search 和 Bayesian Optimization。

## 2. 随机类算法

### 2.1. 穷举式优化算法：网络搜索 (Grid Search)

在机器学习领域（**超参数比较少**的情况）中，可以利用设置网格点的方式来调试超参数，这也是应用最广泛的超参数搜索算法。网格搜索 (Grid Search) 是一种穷举式搜索，即将各种参数排列成矩阵的形式，然后使用笛卡尔积将所有的组合可能遍历一遍，从而得到最优的超参数组合。

![image](http://img.cdn.firejq.com/jpg/2019/7/19/1245d95b45f392ee63ebca17e8fbcbe7.jpg)

优点：
- 网格搜索以穷举的方式遍历所有可能的参数组合，该方法是一定可以找到全局最大值或最小值的。

缺点：
- 随着超参数规模越来越大，GridSearch 时间复杂度会呈指数型增长，十分消耗计算资源。
- 有一些参数比较重要一些参数不太重要，GridSearch 就会浪费很多的资源在非重要的参数搜索上。

### 2.2. 采样式优化算法：随机搜索 (Random Search)

针对网格搜索的不足，Bengio 等人在 2012 年发表的论文 [Random Search for Hyper-Parameter Optimization](http://jmlr.csail.mit.edu/papers/volume13/bergstra12a/bergstra12a.pdf) 中提出了随机搜索方法。随机搜索首先为每类超参数定义一个边缘分布（通常取均匀分布），然后依据这种分布在参数组合上采样，随机的得到一些候选参数组合方案。如果在某一区域找到一个效果好的点，将关注点放到点附近的小区域内继续寻找。

随机搜索虽然有随机因素导致搜索结果可能特别差，但是也可能效果特别好。总体来说，相比网格搜索，随机搜索并未尝试所有参数值，而是从指定的分布中采样固定数量的参数组合，找到近似最优解的效率更高，但是不保证一定能找到比较好的超参数，这种方法建立在概率论的基础上，所取随机点越多，则得到最优解的概率也就越大。适用于深度学习领域（**超参数较多**的情况）。

![image](http://img.cdn.firejq.com/jpg/2019/7/19/f882ed5f971b60793166aee8f9c63408.jpg)

优点：
- 通过对搜索空间的随机取样，随机搜索一般会比网格搜索要快一些，不会受限于超参数规模，不会有指数爆炸、组合爆炸的问题。
- 不存在重要参数和非重要参数浪费资源的问题：在处理问题的时候，是无法知道哪个超参数是更重要的，所以随机的方式去测试超参数点的性能，更为合理，这样可以探究更超参数的潜在价值。
- 在较少的迭代次数内可以比网格搜索找到更好的结果。

缺点：
- 不能保证找到最优参数。
- 多组超参数之间是相互独立的，不能利用先验知识选择下一组超参。

## 3. 拟合类算法：贝叶斯优化 (Bayesian Optimization)

### 3.1. 基本概念

贝叶斯优化用于机器学习调参由 J. Snoek(2012) 在 [Practical Bayesian Optimization of Machine Learning Algorithms](https://arxiv.org/pdf/1206.2944.pdf) 中提出，主要思想是，给定优化的目标函数（广义的函数，只需指定输入和输出即可，无需知道内部结构以及数学性质)，通过不断地添加样本点来更新目标函数的后验分布（高斯过程），直到后验分布基本贴合于真实分布。

简单的说，就是利用先验知识来选择下一组超参，即根据上一次参数组合的优劣评价信息来更好地生成新的参数组合，并不断迭代从而得到使结果向全局最大提升，最终满足需求阈值的最优参数组合。

### 3.2. 算法流程

![image](http://img.cdn.firejq.com/jpg/2019/7/19/c5b493f68ba1e1cd35a2ea1220d4a480.jpg)

算法流程主要分为 4 步：
1. 随机一些超参数 x 并训练得到模型，然后刻画这些模型的能力 y，得到先验数据集合 D = (x1, y1)、(x2, y2)、...、(xk, yk)。
1. 通过先验数据 D 来拟合出高斯模型 GM。
1. 通过采集函数找到在 GM 下的极大值超参数 x'，并通过 x' 训练得到模型和刻画模型能力 y'，将 (x'，y') 加入数据集 D。
1. 重复 2-3 步，直至得到的模型的模型能力 y 达到终止条件。

NOTE:

- 第 2 步拟合的模型 GM，对于任何一组超参数 x 输入，输出的是一个分布（即**均值和标准差**）（因而需要效用函数量化成一个值，具备可比性)，而**不是一个准确的值**，具体如下图所示，通过蓝色先验点拟合出的 GM，绿色区域即为对应输入 x，输出 y 的可能范围：

  ![image](http://img.cdn.firejq.com/jpg/2019/7/19/a0d9eea0e0d8f31cbf6543cb943eaf18.jpg)

- [针对第 1 步和第 2 步：根据先验数据拟合高斯模型的详细分析](https://cloud.tencent.com/developer/article/1378669)：

  假设关于模型最优超参数组合的函数是一维曲线，由于它是一个黑盒无法直到具体的函数形式，但是可以输入某些值并得到输出。现在随机尝试了 4 个超参数，并得到了对应的性能指标，如下图：

  ![image](http://img.cdn.firejq.com/jpg/2019/8/28/dd1f040ba4adb2eac0bc0e68df8de00d.jpg)

  那么问题来了，最优超参数可能在哪里？下一个待探索的超参数是哪个？如果猜测最优值可能在 0.4 这里，函数的真实形式可能长这样子的：

  ![image](http://img.cdn.firejq.com/jpg/2019/8/28/c6ca1b856a0e130d9b277b513f9f003c.jpg)

  而每个人猜测的是不一样的，因此每次生成的函数也不同：

  ![image](http://img.cdn.firejq.com/jpg/2019/8/28/51398cadca20efa505379358714f96f1.jpg)

  可以看到大部分都认为最优超参数是在第 3 个点附近，由于开始时在右侧采点的离线指标是最差的，所以先验认为最优超参数在这里的可能性不大。接着把这个过程取极限，就会得到一个关于最优超参数的概率分布。假设每个分布都是高斯分布，那么得到的是一个高斯过程，其中高斯分布的均值为 0，方差大概为 5：

  ![image](http://img.cdn.firejq.com/jpg/2019/8/28/aae6b8d294d8fb8a65d0119d7000bc7a.jpg)

  这样无论我们猜测最优超参数是取哪个值，总能得到一个关于超参数好坏的描述，即是均值和方差。

### 3.3. 算法特点

贝叶斯优化与常规的网格搜索或者随机搜索的区别是：
- 贝叶斯调参采用高斯过程，考虑之前的参数信息，不断地更新先验；而传统的网格搜索和随机搜索都未考虑之前的参数信息。
- 贝叶斯调参迭代次数少，速度快；而网格搜索速度慢，参数多时易导致维度爆炸。
- 贝叶斯调参针对非凸问题依然稳健；而网格搜索针对非凸问题易得到局部最优。

贝叶斯优化需要面对的几个难题：
- 训练的不稳定性，同一组超参数多次训练无法完全一致。
- 模型能力的刻画，衡量方法很难准确的刻画出模型能力。
- 受资源和时间的原因，无法进行大量的尝试。
- 随机因素太多，对于那些具有未知平滑度、有噪声的高维非凸函数，往往很难对其进行拟合和优化。
- 搜索结果会受初始种子节点选择的影响。
- 整个搜索过程只能串行进行。
- 需要注意代理函数和真实的指标是有差距的，这个差距会决定实际调参效果的优劣。

### 3.4. 代码实现

在实际应用中，通常使用 [BayesOpt](https://github.com/fmfn/BayesianOptimization/blob/master/bayes_opt/bayesian_optimization.py) 包来进行贝叶斯优化调参：
```python
pip install bayesian-optimization
```

BayesOpt 包主要使用 BayesianOptimization 函数来创建一个优化对象，该函数接受一个模型评估函数 function，这个 function 的输入应该是 xgboost（或者其他 ML 模型) 的超参数，输出是模型在测试集上的效果（可以是 Accuracy，也可以是 RMSE，取决于具体的任务，一般返回 K-Fold 的均值)。

e.g. 基于 5-Fold 的 LightGBM 贝叶斯优化：
```python
import lightgbm as lgb
from bayes_opt import BayesianOptimization

train_X, train_y = None, None

def BayesianSearch(clf, params):
    """贝叶斯优化器"""
    # 迭代次数
    num_iter = 25
    init_points = 5
    # 创建一个贝叶斯优化对象，输入为自定义的模型评估函数与超参数的范围
    bayes = BayesianOptimization(clf, params)
    # 开始优化
    bayes.maximize(init_points=init_points, n_iter=num_iter)
    # 输出结果
    params = bayes.res['max']
    print(params['max_params'])

    return params

def GBM_evaluate(min_child_samples, min_child_weight, colsample_bytree, max_depth, subsample, reg_alpha, reg_lambda):
    """自定义的模型评估函数"""

    # 模型固定的超参数
    param = {
        'objective': 'regression',
        'n_estimators': 275,
        'metric': 'rmse',
        'random_state': 2018}

    # 贝叶斯优化器生成的超参数
    param['min_child_weight'] = int(min_child_weight)
    param['colsample_bytree'] = float(colsample_bytree),
    param['max_depth'] = int(max_depth),
    param['subsample'] = float(subsample),
    param['reg_lambda'] = float(reg_lambda),
    param['reg_alpha'] = float(reg_alpha),
    param['min_child_samples'] = int(min_child_samples)

    # 5-flod 交叉检验，注意 BayesianOptimization 会向最大评估值的方向优化，因此对于回归任务需要取负数。
    # 这里的评估函数为 neg_mean_squared_error，即负的 MSE。
    val = cross_val_score(lgb.LGBMRegressor(**param),
        train_X, train_y ,scoring='neg_mean_squared_error', cv=5).mean()

    return val

if __name__ == '__main__':
    # 获取数据，这里使用的是 Kaggle 比赛的数据
    train_X, train_y = get_data()
    # 调参范围
    adj_params = {'min_child_weight': (3, 20),
                 'colsample_bytree': (0.4, 1),
                 'max_depth': (5, 15),
                 'subsample': (0.5, 1),
                 'reg_lambda': (0.1, 1),
                 'reg_alpha': (0.1, 1),
                 'min_child_samples': (10, 30)}
    # 调用贝叶斯优化
    BayesianSearch(GBM_evaluate, adj_params)
```

## 4. Hyperband 算法和 BOHB 算法

### 4.1. Successive Halving 算法

虽然通过网格搜索尝试所有的超参组合，肯定是可以找到最优的，但通常在实际的应用场景下，我们需要考虑计算时间成本、计算资源成本等因素。假设初始候选的超参数组合数量是 n，那么分配到每个超参数组的成本预算 (Budget) 就是 B/n，这里便存在一个矛盾：
- 候选的超参数越多，能够包含最优超参数的可能性也就越大。
- 候选的超参数越多，此时分配到每个超参数组的预算也就越少，那么找到最优超参数的可能性就越低。

因此，针对这个矛盾点，[Jamieson & Talwlkar 在 2015 年提出了 Successive Halving 算法](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1502.07943)，其核心思想是：假设有 n 组超参数组合，然后对这 n 组超参数均匀地分配预算并进行验证评估，根据验证结果淘汰一半表现差的超参数组，然后重复迭代上述过程直到找到最终的一个最优超参数组合。

随着每次 loop，用于评估的超参数组合数量越来越少，与此同时单个超参数组合能分配的预算也逐渐增加，因此相比网格搜索，这个过程能更快地找到合适的超参数。

### 4.2. Hyperband 算法

Hyperband 算法 ([Hyperband: Bandit-Based Configuration Evaluation for Hyperparameter Optimization](https://openrevie.net/pdf?id=ry18Ww5ee)) 基于 Successive Halving 算法的思想进行了扩展：

![image](http://img.cdn.firejq.com/jpg/2019/8/29/93311a28c70efb1ac91d68a35cae37e2.jpg)

其算法流程如下：

![image](http://img.cdn.firejq.com/jpg/2019/8/28/a4986383e2fd4abcb641ca33f045d881.jpg)

其中：
- r: 单个超参数组合实际所能分配的预算。
- R: 单个超参数组合所能分配的最大预算。
- s_max: 用来控制总预算的大小。上面算法中 `smax = logη(R)`。
- B: 总共的预算，`B=(s_max+1)R`。
- η: 用于控制每次迭代后淘汰参数设置的比例。
- `get_hyperparameter_configuration(n)`: 采样得到 n 组不同的超参数设置。
- `run_then_return_val_loss(t,ri)`: 根据指定的参数设置和预算计算 valid loss。LL 表示在预算为 riri 的情况下各个超参数设置的验证误差。
- `topk(T,L,⌊n_i/η⌋)`: 第三个参数表示需要选择 top k(k=⌊n_i/η⌋) 参数设置。

NOTE
- 从图中可以看出，实际上 Hyperband 包含了两个 loop，其中 inner loop 即为 SuccessiveHalving 算法。随着每次 inner loop，用于评估的超参数组合数量越来越少，与此同时单个超参数组合能分配的预算也逐渐增加，所以这个过程能更快地找到合适的超参数。

### 4.3. BOHB 算法

在 Hyperband 算法中，对超参数设置采样使用的是均匀随机采样。因此，Stefan Falkner 结合贝叶斯进行采样提出了 [BOHB: Practical Hyperparameter Optimization for Deep Learning](https://openreview.net/pdf?id=HJMudFkDf)。

[BOHB: ROBUST AND EFFICIENT HYPERPARAMETER OPTIMIZATION AT SCALE](https://www.automl.org/blog_bohb/)：

> BOHB, a new and versatile tool for hyperparameter optimization which comes with substantial speedups through the combination of Hyperband with Bayesian optimization. The following figure shows an exemplary result: BOHB starts to make progress much faster than vanilla Bayesian optimization and random search and finds far better solutions than Hyperband and random search.

![image](http://img.cdn.firejq.com/jpg/2019/8/29/9957febfeac393686f7be95731335a5e.jpg)

### 4.4. 代码实现

[GitHub HpBandSter: a distributed Hyperband implementation on Steroids](https://github.com/automl/HpBandSter)：

> HpBandSter started out as a simple implementation of Hyperband (Li et al. 2017), and contains an implementation of BOHB (Falkner et al. 2018)

## 5. 种群优化类算法

### 5.1. 遗传算法 (Genetic algorithm)

#### 5.1.1. 基本概念

遗传算法（Genetic Algorithm）是一种通过模拟自然进化过程搜索最优解的方法，它的思想来自于进化论，生物种群具有自我进化的能力，能够不断适应环境，优势劣汰之后得到最优的种群个体。进化的行为主要有选择，遗传，变异。遗传算法希望能够通过将初始解空间进化到一个较好的解空间。

![image](http://img.cdn.firejq.com/jpg/2019/8/28/e466502c49f62f14aaab62b7833d8286.jpg)

#### 5.1.2. 算法流程

1. 初始化种群（随机产生 N 组超参数）

1. 计算各个超参组合的适应度（Auc，Loss）

1. 执行进化过程：
    1. 选择：把优秀的个体（topAuc 个体）选择出来
    1. 交叉：模拟繁殖后代的基因重组，如根据多个模型参数向量组合得到新的模型参数向量
    1. 变异：模拟基因突变，如对超参数增加噪音产生新的超参数

1. 经过选择、交叉和变异的进化过程后，产生了下一代种群（新的一组超参数）。将产生的新种群继续执行第 2、3 步，重复此过程直到超参数组合的适应度满足要求为止。

### 5.2. 基于种群的训练 (Population Based Training，PBT)

2017 年，DeepMind 在 [Population Based Training of Neural Networks](https://arxiv.org/pdf/1711.09846.pdf) 中提出了基于遗传算法改进的 PBT 算法。

从结果上讲他是找到了一个训练过程（一个阶段一组超参数)，而不是一组最优超参数：

![image](http://img.cdn.firejq.com/jpg/2019/8/27/f3a73e08bc0835d9201ff643d8f1ab91.jpg)

种群优化算法框架：
1. 初始化种群（随机产生 N 组超参数）
1. 计算各个超参组合的适应度（Auc，Loss）
1. 执行进化过程：
    1. 选择是把优秀的个体（topAuc 个体）选择出来
    1. 交叉是模拟繁殖后代的基因重组，如根据多个模型参数向量组合得到新的模型参数向量
    1. 变异是模拟基因突变，如对超参数增加噪音产生新的超参数
1. 经过选择、交叉、变异，产生下一代种群。产生的新种群继续执行（2）（3），重复此过程直到停止。

## 6. 粒子群优化 (Paticle Swarm Optimization)

## 7. TPE (tree of parzen estimators)

Hyperopt & HPOLib 都实现了该算法。

## 8. SMAC

基于序列模型的算法配置 (SMAC)

[Spearmint](https://github.com/JasperSnoek/spearmint

RoBO- 鲁棒的贝叶斯优化框架 (Robust Bayesian Optimization framework)

SMAC3 - SMAC 算法的 python 实现

## 9. 现有框架

HyperOpt, Spearmint, and HPOLib (Snoek et al. (2012); Eggensperger et al. (2013)) are distributed model selection tools that manage both the search and evaluation of the model, implementing search techniques such as random search and tree of parzen estimators (TPE). However, both frameworks are tightly coupled to the search algorithm structure and requires manual management of computational resources across a cluster. Further, systems such as Spearmint, HyperOpt, and TuPAQ (MLBase) (Sparks et al. (2015)) treat a full trial execution as an atomic unit, which does not allow for intermediate control of trial execution. This inhibits eﬃcient usage of cluster resources and also does not provide the expressivity needed to support algorithms such as HyperBand.

Google Vizier (Golovin et al. (2017)) is a Google-internal service that provides model selection capabilities. Similar to Tune, Vizier provides parallel evaluation of trials, hosts many state-of-the-art optimization algorithms, provides functionality for performance analysis. However, it is ﬁrst and foremost a service and is tied to closed-source infrastructure.

Mistique (Vartak et al. (2018)) is also a system that addresses model selection. However, rather than focusing on the execution of the selection process, Mistique focuses on model debugging, emphasizing iterative procedures and memory footprint minimization.

Finally, Auto-SKLearn (Feurer et al. (2015)) and Auto-WEKA (Thornton et al. (2013)) are systems for automating model selection, integrating meta learning and ensembling into a single system. The focus of these systems is at the execution layer rather than the algorithmic level. This implies that in principle, it would be possible to implement AutoWEKA and Auto-SKLearn on top of Tune, providing distributed execution of these AutoML components. Further, both Auto-SKLearn and Auto-WEKA are tied to Scikit-Learn and WEKA respectively as the only machine learning frameworks supported.

Ray.Tune

## 10. Refer Links

[超参数优化](https://www.cnblogs.com/yifdu25/p/8202811.html)

[ML 模型超参数调节：网格搜索、随机搜索与贝叶斯优化](https://www.jianshu.com/p/5378ef009cae)

[AutoML 之超参数优化](https://fuhailin.github.io/AutoML%E4%B9%8B%E8%B6%85%E5%8F%82%E6%95%B0%E4%BC%98%E5%8C%96/)

[Javier Gonz´alez @Lancaster University: Introduction to Bayesian Optimization](http://gpss.cc/gpmc17/slides/LancasterMasterclass_1.pdf)

[拒绝日夜调参：超参数搜索算法一览](https://cloud.tencent.com/developer/article/1378669)

[机器学习超参数优化算法 -Hyperband](https://zhuanlan.zhihu.com/p/53088201)

TODO:

[【干货】手把手教你 Python 实现自动贝叶斯调整超参数](https://cloud.tencent.com/developer/article/1168318)

[贝叶斯优化：一种更好的超参数调优方式](https://zhuanlan.zhihu.com/p/29779000)
