- [超参数优化 (Hyper parameter optimization)](#超参数优化-hyper-parameter-optimization)
  - [1. 基础概念](#1-基础概念)
  - [2. 超参优化算法](#2-超参优化算法)
    - [2.1. 随机类算法](#21-随机类算法)
      - [2.1.1. 网络搜索穷举式优化 (Grid Search)](#211-网络搜索穷举式优化-grid-search)
      - [2.1.2. 随机搜索采样式优化 (Random Search)](#212-随机搜索采样式优化-random-search)
    - [2.2. 拟合类算法](#22-拟合类算法)
      - [2.2.1. 贝叶斯优化 (Bayesian Optimization)](#221-贝叶斯优化-bayesian-optimization)
        - [2.2.1.1. 基本概念](#2211-基本概念)
        - [2.2.1.2. 代码实现](#2212-代码实现)
    - [2.3. 其它算法](#23-其它算法)
      - [2.3.1. 基于种群的训练 (Population Based Training，PBT)](#231-基于种群的训练-population-based-trainingpbt)
      - [Hyperband](#hyperband)
  - [3. Refer Links](#3-refer-links)

# 超参数优化 (Hyper parameter optimization)

## 1. 基础概念

超参数是指机器学习模型里面的框架参数，比如聚类方法里面类的个数或者话题模型里面话题的个数等，都称为超参数。它们跟训练过程中学习的参数（权重）是不一样的，通常是手工设定，不断试错调整。深度学习和神经网络模型，有很多这样的参数需要学习，而模型的训练过程是相对 expensive 的，不能通过快速计算获取大量样本。这就是为什么过去这么多年从业者弃之不顾的原因。以前给人的印象，深度学习就是“黑魔法”。

时至今日，非参数学习研究正在帮助深度学习更加自动的优化模型参数选择，当然有经验的专家仍然是必须的。每一个机器学习系统都有超参数，自动机器学习 (AutoML) 最基本的任务就是自动进行超参数优化 (Hyper parameter optimization, HPO)。深度神经网络的性能更是尤其依赖于网络的结构、正则项、最优化目标等相关超参数的选取。

机器学习模型超参数调优一般认为是一个黑盒优化问题。所谓黑盒问题就是我们在调优的过程中只看到模型的输入和输出，不能获取模型训练过程的梯度信息，也不能假设模型超参数和最终指标符合凸优化条件，否则的话我们通过求导或者凸优化方法就可以求导最优解，不需要使用这些黑盒优化算法，而实际上大部分的模型超参数也符合这个场景。

## 2. 超参优化算法

自动调参算法主要有 Grid search（网格搜索）、Random search（随机搜索）、Genetic algorithm（遗传算法）、Paticle Swarm Optimization（粒子群优化）、Bayesian Optimization（贝叶斯优化）、TPE、SMAC 等。

像遗传算法和 PSO 这些经典黑盒优化算法，可以归类为群体优化算法，也不是特别适合模型超参数调优场景，因为需要有足够多的初始样本点，并且优化效率不是特别高。

目前业界用得比较多的分别是 Grid search、Random search 和 Bayesian Optimization。

### 2.1. 随机类算法

#### 2.1.1. 网络搜索穷举式优化 (Grid Search)

在机器学习领域，超参数比较少的情况下，通常利用设置网格点的方式来调试超参数，这也是应用最广泛的超参数搜索算法。

![image](http://img.cdn.firejq.com/jpg/2019/7/19/1245d95b45f392ee63ebca17e8fbcbe7.jpg)

优点：
- 网格搜索以穷举的方式遍历所有可能的参数组合，该方法是一定可以找到全局最大值或最小值的。

缺点：
- 随着超参数规模越来越大，GridSearch 时间复杂度会呈指数型增长，十分消耗计算资源。
- 有一些参数比较重要一些参数不太重要，GridSearch 就会浪费很多的资源在非重要的参数上。
- 可以通过固定多数参数，分步对 1~2 个超参数进行调解，从而减少计算时间，但却难以自动化进行，而且由于目标参数一般是非凸的，因此容易陷入局部最小值。

#### 2.1.2. 随机搜索采样式优化 (Random Search)

在深度学习领域，超参数较多的情况下，不是设置规则的网格点，而是随机选择点进行调试。该方法依据某种分布对参数空间采样，随机的得到一些候选参数组合方案，如果在某一区域找到一个效果好的点，将关注点放到点附近的小区域内继续寻找。

![image](http://img.cdn.firejq.com/jpg/2019/7/19/f882ed5f971b60793166aee8f9c63408.jpg)

2012 年 Bengio 发表的论文《[Random Search for Hyper-Parameter Optimization](http://jmlr.csail.mit.edu/papers/volume13/bergstra12a/bergstra12a.pdf)》指出，随机搜索相比网格搜索找到近似最优解的效率更高。与网格搜索相比，随机搜索并未尝试所有参数值，而是从指定的分布中采样固定数量的参数设置。它的理论依据是，如果随即样本点集足够大，那么也可以找到全局的最大或最小值，或它们的近似值。通过对搜索范围的随机取样，随机搜索一般会比网格搜索要快一些。

优点：
- 不会受限于超参数规模，不会有指数爆炸、组合爆炸的问题
- 在处理问题的时候，是无法知道哪个超参数是更重要的，所以随机的方式去测试超参数点的性能，更为合理，这样可以探究更超参数的潜在价值，不会存在重要参数和非重要参数浪费资源的问题。
- 在较少的迭代次数内比网格搜索找到更好的结果。

缺点：
- 不能保证找到最优参数。

### 2.2. 拟合类算法

#### 2.2.1. 贝叶斯优化 (Bayesian Optimization)

##### 2.2.1.1. 基本概念

贝叶斯优化用于机器学习调参由 J. Snoek(2012) 提出，主要思想是，给定优化的目标函数（广义的函数，只需指定输入和输出即可，无需知道内部结构以及数学性质)，通过不断地添加样本点来更新目标函数的后验分布（高斯过程），直到后验分布基本贴合于真实分布。简单的说，就是根据上一次参数组合的优劣评价信息来更好地生成新的参数组合，并不断迭代从而得到满足需求的最优参数组合。

![image](http://img.cdn.firejq.com/jpg/2019/7/19/c5b493f68ba1e1cd35a2ea1220d4a480.jpg)

算法流程主要分为 4 步和第二节提到的流程几乎一致：
1. 随机一些超参数 x 并训练得到模型，然后刻画这些模型的能力 y，得到先验数据集合 D = (x1, y1)、(x2, y2)、...、(xk,yk)。
1. 通过先验数据 D 来拟合出高斯模型 GM。
1. 通过采集函数找到在 GM 下的极大值 超参数 x'，并通过 x'训练得到模型和刻画模型能力 y'，将 (x'，y') 加入数据集 D。
1. 重复 2-3 步，直至终止条件。

NOTE: 第 2 步拟合的模型 GM，对于任何一组超参数 x 输入，输出的是一个分布即均值和标准差（因而需要效用函数量化成一个值，具备可比性)，而不是一个准确的值，具体如下图所示，通过蓝色先验点拟合出的 GM，绿色区域即为对应输入 x，输出 y 的可能范围：

![image](http://img.cdn.firejq.com/jpg/2019/7/19/a0d9eea0e0d8f31cbf6543cb943eaf18.jpg)

贝叶斯优化与常规的网格搜索或者随机搜索的区别是：
- 贝叶斯调参采用高斯过程，考虑之前的参数信息，不断地更新先验；网格搜索未考虑之前的参数信息。
- 贝叶斯调参迭代次数少，速度快；网格搜索速度慢，参数多时易导致维度爆炸。
- 贝叶斯调参针对非凸问题依然稳健；网格搜索针对非凸问题易得到局部最优。

该方法需要面对的几个难题：
- 训练的不稳定性，同一组超参数多次训练无法完全一致。
- 模型能力的刻画，衡量方法很难准确的刻画出模型能力。
- 受资源和时间的原因，无法进行大量的尝试。
- 随机因素太多，很难用简单固定的模型来拟合。
- 搜索结果会受初始种子节点选择的影响。
- 整个搜索过程只能串行进行。
- 需要注意代理函数和真实的指标是有差距的，这个差距会决定实际调参效果的优劣。

##### 2.2.1.2. 代码实现

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

### 2.3. 其它算法

#### 2.3.1. 基于种群的训练 (Population Based Training，PBT)

DeepMind 提出新型超参数最优化方法：性能超越手动调参和贝叶斯优化《Population Based Training of Neural Networks》

pbt 是基于遗传算法的灵感改进出来的算法，从结果上讲他是找到了一个训练过程（一个阶段一组超参数)，而不是一组最优超参数

<!-- TODO: leap's shared PPT -->

遗传算法（Genetic Algorithm）是一种通过模拟自然进化过程搜索最优解的方法，它的思想来自于进化论，生物种群具有自我进化的能力，能够不断适应环境，优势劣汰之后得到最优的种群个体。进化的行为主要有选择，遗传，变异。遗传算法希望能够通过将初始解空间进化到一个较好的解空间。

种群优化算法框架：
1. 初始化种群（随机产生 N 组超参数）
1. 计算各个超参组合的适应度（Auc，Loss）
1. 执行进化过程：
    1. 选择是把优秀的个体（topAuc 个体）选择出来
    1. 交叉是模拟繁殖后代的基因重组，如根据多个模型参数向量组合得到新的模型参数向量
    1. 变异是模拟基因突变，如对超参数增加噪音产生新的超参数
1. 经过选择、交叉、变异，产生下一代种群。产生的新种群继续执行（2）（3），重复此过程直到停止。

#### Hyperband

如果为了找到机器学习算法最优的超参组合而不考虑其他因素，那我们可以用网格搜索尝试所有的超参组合，这样肯定可以找到最优的。但是通常在我们的应用场景下，需要考虑计算时间，计算资源等因素。假设候选的超参数组合数量是 n，那么分配到每个超参数组的预算就是 s/n，所以 Hyperband 做的事情就是在 n 与 s/n 做权衡 (tradeoff)。Hyperband 算法的核心思想是 SuccessiveHalving。假设有 n 组超参数组合，然后对这 n 组超参数均匀地分配预算并进行验证评估，根据验证结果淘汰一半表现差的超参数组，然后重复迭代上述过程直到找到最终的一个最优超参数组合。

## 3. Refer Links

[超参数优化](https://www.cnblogs.com/yifdu25/p/8202811.html)

[ML 模型超参数调节：网格搜索、随机搜索与贝叶斯优化](https://www.jianshu.com/p/5378ef009cae)

[AutoML 之超参数优化](https://fuhailin.github.io/AutoML%E4%B9%8B%E8%B6%85%E5%8F%82%E6%95%B0%E4%BC%98%E5%8C%96/)

TODO:

[【干货】手把手教你 Python 实现自动贝叶斯调整超参数](https://cloud.tencent.com/developer/article/1168318)

[贝叶斯优化：一种更好的超参数调优方式](https://zhuanlan.zhihu.com/p/29779000)
