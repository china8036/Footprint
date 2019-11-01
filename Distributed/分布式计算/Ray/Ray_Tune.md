- [Ray: Tune](#ray-tune)
  - [1. 背景介绍](#1-背景介绍)
  - [2. 主要 API](#2-主要-api)
    - [2.1. tune.Trainable](#21-tunetrainable)
    - [2.2. tune.run()](#22-tunerun)
    - [2.3. tune.schedulers.TrialScheduler](#23-tuneschedulerstrialscheduler)
    - [2.4. tune.suggest.SuggestionAlgorithm](#24-tunesuggestsuggestionalgorithm)
  - [3. 内部设计](#3-内部设计)
    - [3.1. 核心组件](#31-核心组件)
      - [3.1.1. TrialRunner](#311-trialrunner)
      - [3.1.2. Trial objects](#312-trial-objects)
      - [3.1.3. Trainable](#313-trainable)
      - [3.1.4. TrialExecutor](#314-trialexecutor)
      - [3.1.5. TrialScheduler](#315-trialscheduler)
      - [3.1.6. SearchAlg](#316-searchalg)
    - [3.2. 工作流程](#32-工作流程)
  - [4. 使用实例](#4-使用实例)
    - [4.1. 结合 PyTorch 进行 GridSearch](#41-结合-pytorch-进行-gridsearch)
    - [4.2. 使用 BayesOptSearch + AsyncHyperBand](#42-使用-bayesoptsearch--asynchyperband)
  - [5. Refer Links](#5-refer-links)

# Ray: Tune

## 1. 背景介绍

超参数调优作为 AutoML 的一个重要组成部分，现今已有不少超参数优化框架被实现和应用，如：
- HyperOpt & HPOLib: 实现了 random search 和 tree of parzen estimators (TPE) 超参数搜索算法，但其框架的实现与算法本身耦合紧密，不利于扩展；同时这些框架将整个实验过程视为一个原子过程，因此无法进行更细粒度的实验控制，无法实现 HyperBand 一类的算法。
- Auto-SKLearn & Auto-WEKA & AutoKeras: 提供了超参数搜索、meta-learning 的封装实现，但这类框架都与具体的机器学习框架绑定（如 Scikit-Learn、WEKA 以及 Keras），无法扩展到其它框架进行使用。
- Google Vizier: Google 内部的调优服务，提供了多种超参数优化算法的封装实现，并行实验调度以及训练结果分析，但由于 Google Vizier 属于闭源的商业框架，无法直接学习和使用。

在这样的背景下，UC Berkeley AMP 实验室在其提出的分布式计算框架 Ray 的基础上，又提出了一个可扩展的超参数优化框架 [Ray.Tune](https://ray.readthedocs.io/en/latest/tune.html)。该框架具有以下特点：
- 面向开发者提供了 User API 和 Scheduler API，既支持对已封装实现的搜索算法（如 PBT、HyperBand 等）直接调用，也支持自定义搜索和调度算法进行实验。
- 与可视化工具集成，友好展示搜索结果，如 TensorBoard 和 VisKit 等。
- 底层默认使用 Ray 框架执行训练，可进行资源感知的动态调度，支持进行从单机到多机的超参数搜索实验。
- 支持多种机器学习框架，如 PyTorch、TensorFlow、Keras 等。

## 2. 主要 API

Tune 主要提供了两种 API：
- User API：适用于要直接进行超参数调优的用户，通过此类 API，用户可以便捷地使用多种超参数搜索算法对模型进行优化。
- Scheduler API：适用于对模型搜索过程本身感兴趣的用户。通过此类 API，用户能够针对不同范围的训练需求，实现并使用自定义的调度和搜索算法。

### 2.1. tune.Trainable

在 Tune 中，每个 Trainable 对象在 Ray 中将作为一个 actor 对象，运行在一个独立的进程中。Trainable 对象可以以类或函数两种方式来实现：

![Trainable API](http://img.cdn.firejq.com/jpg/2019/10/21/d62ab4607e8c2d468d8feb03b2313a59.jpg)

- 基于 Class

  基于 Class 实现时，用户需要继承 `ray.tune.Trainable` 接口并实现 Trainable 接口的  `_setup`, `_train`, `_save`, `_restore` 方法：
  ```python
  class MyClass(Trainable):
    def _setup(self):
        self.saver = tf.train.Saver()
        self.sess = ...
        self.iteration = 0

    def _train(self):
        # run training code
        self.sess.run(...)
        self.iteration += 1

    def _save(self, checkpoint_dir):
        return self.saver.save(
            self.sess, checkpoint_dir + "/save",
            global_step=self.iteration)

    def _restore(self, path):
        return self.saver.restore(self.sess, path)
  ```
  - _setup(): 初始化函数，一般用作数据初始化等。
  - _train(): 对一个训练对象调用 _train() 将执行一个训练的逻辑迭代。
  - _save(): 调用 _save() 可将可训练对象的训练状态保存到磁盘。
  - _restore(): 调用 _restore(path) 可将训练对象恢复到给定的训练状态。

  Trainable 类在对外提供的操作接口 train()、save()、restore() 中调用了以上用户自定义的方法，从而实现通过用户自定义的训练逻辑来完成训练过程。

- 基于 Function

  基于 Function 实现时，用户需要按照以下函数签名定义一个 trainable() 方法，并通过在方法中调用 tune.track.log() 方法进行训练指标的上报以实现 scheduling, search, or early stopping：
  ```python
  def trainable(config):
      """
      Args:
          config (dict): Parameters provided from the search algorithm
              or variant generation.
      """
      while True:
          # ...
          tune.track.log(**kwargs)
  ```
  e.g.
  ```python
  def easy_objective(config, reporter):
      import time
      time.sleep(0.2)
      for i in range(config["iterations"]):
          reporter(
              timesteps_total=i,
              mean_loss=(config["height"] - 14)**2 - abs(config["width"] - 3))
          time.sleep(0.02)

  if __name__ == "__main__":
      # ...
      ray.init()
      config = {
          "num_samples": 10 if args.smoke_test else 1000,
          "config": {
              "iterations": 100,
          },
          "stop": {
              "timesteps_total": 100
          }
      }
      tune.run(easy_objective,
              name="my_exp",
              search_alg=BayesOptSearch(
                  {"width": (0, 20), "height": (-100, 100)},
                  max_concurrent=4,
                  metric="mean_loss",
                  mode="min",
                  utility_kwargs={
                      "kind": "ucb",
                      "kappa": 2.5,
                      "xi": 0.0
              }),
              scheduler=AsyncHyperBandScheduler(metric="mean_loss", mode="min"),
              **config)
  ```

### 2.2. tune.run()

`tune.run(trainable)`: 通过该方法启动超参数搜索，并阻塞至训练结束然后返回一个 ExperimentAnalysis 对象。通过训练结束后返回的 ExperimentAnalysis 对象，可以获取超参数搜索结果：
```
analysis = tune.run(
    trainable,
    name="example-experiment",
    num_samples=10,
)
print("Best config is", analysis.get_best_config(metric="mean_accuracy"))
```
运行结果：
```
Best config is: {'lr': 0.011537575723482687, 'momentum': 0.8921971713692662}
```

Options:
- `resources_per_trial`: 默认情况下，Tune 会根据 CPU 核数来确定实验的并行数量，但可通过指定 resources_per_trial 数来人工指定每个实验所使用的资源数量。e.g.
  ```
  # If you have 4 CPUs on your machine, this will run 4 concurrent trials at a time.
  tune.run(trainable, num_samples=10)

  # If you have 4 CPUs on your machine, this will run 2 concurrent trials at a time.
  tune.run(trainable, num_samples=10, resources_per_trial={"cpu": 2})

  # If you have 4 CPUs on your machine, this will run 1 trial at a time.
  tune.run(trainable, num_samples=10, resources_per_trial={"cpu": 4})

  # If you have 4 CPUs on your machine and 1 GPU, this will run 1 trial at a time.
  tune.run(trainable, num_samples=10, resources_per_trial={"cpu": 2, "gpu": 1})
  ```

- `config`: Search Space 配置，传入指定超参数的搜索范围，可通过内置或用户自定义的搜索算法来提供。也可传入环境变量，以供算法使用。
  - `tune.grid_search`
    ```python
    tune.run(
        ...,
        config={
            "env": "CartPole-v0",
            "alpha": tune.grid_search([0.001, 0.01, 0.1])),
            "beta": tune.grid_search([0.001, 0.01, 0.1])
        }
    )
    ```
  - `tune.sample_from(<func>)`
    ```python
    tune.run(
        ...,
        config={
            "alpha": tune.sample_from(lambda spec: np.random.rand()),
            "beta": tune.sample_from(lambda spec: np.random.rand())
        }
    )
    ```
- `num_samples`: 指定采样次数。默认情况下，无论是随机采样还是网格搜索，都只会采样一次，但可通过 num_samples 参数来进行多次的采样。e.g.
  ```python
  tune.run(
      my_trainable,
      name="my_trainable",
      config={
          "alpha": tune.sample_from(lambda spec: np.random.uniform(100)),
          "beta": tune.sample_from(lambda spec: spec.config.alpha * np.random.normal()),
          "nn_layers": [
              tune.grid_search([16, 64, 256]),
          ],
      },
      num_samples=10
  )
  ```
  以上代码将会执行 10 次 3x3 grid search，共生成 90 组实验任务，每一组都包含了随机采样的 alpha 和 beta。

- `stop`: 指定训练结束的停止条件。

- `search_alg` (SearchAlgorithm): 指定实验使用的 SearchAlgorithm 对象，默认使用 BasicVariantGenerator.

- `scheduler` (TrialScheduler): 指定实验使用的 TrialScheduler 对象，默认使用 FIFOScheduler.

- etc..

### 2.3. tune.schedulers.TrialScheduler

```
class ray.tune.schedulers.TrialScheduler
```
通过继承 TrialScheduler 基类，可以实现用户自定义的任务调度器。该接口提供了以下方法供用户自定义实现：
- `on_trial_add(trial_runner, trial)`
- `on_trial_error(trial_runner, trial)`
- `on_trial_result(trial_runner, trial, result)`
- `on_trial_complete(trial_runner, trial, result)`
- `on_trial_remove(trial_runner, trial)`
- `choose_trial_to_run(trial_runner)`
- `debug_string()`

### 2.4. tune.suggest.SuggestionAlgorithm

```
class ray.tune.suggest.SearchAlgorithm
```
通过继承 SearchAlgorithm 基类，可以实现用户自定义的 SearchAlgorithm 组件。该接口提供了以下方法供用户自定义实现：
- `add_configurations(experiments)`
- `next_trials()`
- `on_trial_result(trial_id, result)`
- `on_trial_complete(trial_id, result=None, error=False, early_terminated=False)`
- `is_finished()`

## 3. 内部设计

### 3.1. 核心组件

![Architecture](http://img.cdn.firejq.com/jpg/2019/10/21/55986bb0f0e28603a555a9a3ea73f927.jpg)

#### 3.1.1. TrialRunner

TrialRunner 是完成训练过程的主要控制组件，该组件通过 SearchAlgorithm 组件生成超参数配置组合，并通过 TrialScheduler 组件对多组实验进行调度执行，同时还提供了一定的容错处理逻辑（如：当设置了 run 方法的 checkpoint_freq 参数时，会在适当的时候执行 checkpointing 方法保存训练的中间结果，从而实现在训练出错失败时自动重启实验任务）。

#### 3.1.2. Trial objects

Trial 对象是一个 Tune 内部定义的数据结构，包含了每次训练的元信息（如状态信息："PENDING", "RUNNING", "PAUSED", "ERRORED", "TERMINATED"）。每个 Trial 对象与 Trainable 对象一一对应。

#### 3.1.3. Trainable

Trainable 组件可以基于类也可以基于函数，是由用户指定的一个在训练进程执行的程序。若 Trainable 组件基于类对象实现，则需要继承 Trainable 接口；若 Trainable 组件基于函数实现，则需要实现指定的函数签名并在训练过程中使用 `tune.track.log()` 上报训练指标。

#### 3.1.4. TrialExecutor

> The trials are scheduled and managed by a trial scheduler.

TrialExecutor 是与底层训练执行框架进行交互的组件，同时负责对可用资源的管理以确保训练任务不会过载。

用户可以通过继承 TrialExecutor 接口，来使用自己的训练执行程序或平台。在 Tune 中，TrialExecutor 组件默认使用 Ray 分布式训练框架 (`class RayTrialExecutor(TrialExecutor)`) 来完成实验任务。

#### 3.1.5. TrialScheduler

TrialScheduler 组件对多组实验任务进行调度，包括实验的终止、暂停或优先调度等操作。

用户可通过继承 TrialScheduler 接口，并在 tune.run() 中指定 scheduler 参数来使用自定义的任务调度逻辑。
```python
tune.run( ... , scheduler=AsyncHyperBandScheduler())
```
Tune 已提供了以下 TrialScheduler 组件的实现，默认使用 FIFOScheduler：
- FIFOScheduler (`class FIFOScheduler(TrialScheduler)`)
- PopulationBasedTraining (`class PopulationBasedTraining(FIFOScheduler)`)
- HyperBandScheduler (`class HyperBandScheduler(FIFOScheduler)`)
- AsyncHyperBandScheduler (`class AsyncHyperBandScheduler(FIFOScheduler)`)
- HyperBandForBOHB (`class HyperBandForBOHB(HyperBandScheduler)`)
- MedianStoppingRule (`class MedianStoppingRule(FIFOScheduler)`)

e.g.
```python
pbt_scheduler = PopulationBasedTraining(
        time_attr='time_total_s',
        metric='mean_accuracy',
        mode='max',
        perturbation_interval=600.0,
        hyperparam_mutations={
            "lr": [1e-3, 5e-4, 1e-4, 5e-5, 1e-5],
            "alpha": lambda: random.uniform(0.0, 1.0),
            ...
        })
tune.run( ... , scheduler=pbt_scheduler)
```

#### 3.1.6. SearchAlg

> Configurations are either generated by Tune or drawn from a user-specified search algorithm.

SearchAlgorithm 组件用于提供要进行实验评估的超参数配置，当训练的每一个 step 结束后、训练发生错误或者训练完成时，该组件都会被 TrialRunner 调用以生成下一轮搜索任务的超参数配置。**不同于 TrialScheduler 组件，SearchAlgorithms 无法对实验过程进行控制（如：终止或暂停实验）**。

用户可通过继承 SearchAlgorithm 或 SuggestionAlgorithm 接口 (`class SuggestionAlgorithm(SearchAlgorithm)`)，并在 tune.run() 中指定 search_alg 参数来使用自定义的搜索算法。
```python
tune.run(my_function, search_alg=SearchAlgorithm(...))
```
Tune 默认已提供了 BayesOpt Search、HyperOpt Search(Tree-structured Parzen Estimators)、SigOpt Search、SkOpt Search (Scikit-Optimize)、Nevergrad Search、Ax Search、BOHB (HpBandSter) 等实现。

### 3.2. 工作流程

以下从 Tune 框架的入口方法 `tune.run()` 出发，根据[实现代码](https://github.com/ray-project/ray/blob/master/python/ray/tune/tune.py#L58) 进行分析：

![tune.run() 实现时序图](http://img.cdn.firejq.com/2.png)

从图中可以看出：
- Experiment 对象封装了实验任务的详细配置，而 Trial 对象根据 Experiment 对象生成，在其基础上封装了训练的状态机，是实验任务最终执行的主体。
- TrialRunner 对象是整个过程的驱动组件，协调 Tune 框架中的各个组件，保证实验的完整进行。
- SearchAlgorithm 对象根据 Experiment 配置生成具体的 Trial 对象列表，包含了要进行调优的超参数配置。
- TrialScheduler 对象对 Trial 对象列表进行调度，决定每一轮实验要执行的 Trial 对象。
- TrialExecutor 对象负责最终启动和执行 Trial 对象所封装的实验过程，默认使用 Ray 作为后端实现。

**Tune 框架通过这样的组件分工与协作，实现了低耦合的超参数调优训练框架，训练的执行逻辑与超参数搜索算法相互分离，超参数搜索算法又与实验调度算法相互分离，从而既保证了框架的可扩展性，同时又可进行细粒度的实验过程控制。同时训练底层直接基于 Ray，提供了较为完整的分布式训练支持，但又支持对接其它的训练框架或实验平台**。

<!-- plantuml

@startuml

tune.run -> tune.trial_executor.TrialExecutor : 根据 trial_executor 参数、n 构造 TrialExecutor 对象

tune.run -> tune.experiment.Experiment : 调用_register_if_needed()\n 注册 trainable 参数对象

tune.experiment.Experiment -> tune.experiment.Experiment : 若传入的 trainable 对象是函数形式，\n 使用 wrap_function 将其包装成类

tune.experiment.Experiment -> tune.registry: 调用 register_trainable() 将 trainable\n 传入全局注册器_global_registry

tune.registry -> tune.registry : 调用 pickle.dumps() 将 trainable 序列化

tune.registry -> tune.registry : 调用 flush_values() 将序列化的 trainable 对象、n 存入 global_worker 的 redis

tune.experiment.Experiment -> tune.run : 返回序列化完成的 run_identifier

tune.run -> tune.experiment.Experiment : 根据 run_identifier 等参数构造 Experiment 对象

tune.run -> tune.trial_runner.TrialRunner : 根据 search_alg、scheduler、trial_executor\n 等参数构造 TrialRunner 对象

tune.run -> tune.trial_runner.TrialRunner : 调用 add_experiment()\n 将 Experiment 对象添加到 TrialRunner 对象中

tune.trial_runner.TrialRunner -> tune.suggest.search.SearchAlgorithm : 调用_search_alg.add_configurations([experiment])，\n 使用传入的 SearchAlg 组件，\n 根据 Experiment 对象生成 Trial 对象

tune.suggest.search.SearchAlgorithm -> tune.trial_runner.TrialRunner: 将生成的 Trial 对象存储到 TrialRunner 中

tune.run ->  tune.trial_runner.TrialRunner: 调用 runner.step() 进入 event loop，\n 驱动训练过程步进，\n 直到 runner.is_finished() 返回为 True

tune.trial_runner.TrialRunner -> tune.trial_scheduler.TrialScheduler : 调用_scheduler_alg.choose_trial_to_run()，\n 使用传入的 TrialScheduler 组件、n 决定 TrialRunner 中下一个要执行的 Trial 对象

tune.trial_scheduler.TrialScheduler -> tune.trial_runner.TrialRunner : 调用 choose_trial_to_run()\n 将选择的下一个 Trial 对象返回给 TrialRunner

tune.trial_runner.TrialRunner -> tune.trial_executor.TrialExecutor : 调用 trial_executor.start_trial()，\n 使用传入的 TrialExecutor 组件执行 Trial 对象

tune.trial_runner.TrialRunner -> tune.trial_runner.TrialRunner : 维护训练过程，执行 Trial 对象的状态机

tune.run -> tune.trial_runner.TrialRunner : 调用 runner.checkpoint()\n 尝试保存 checkpoint

tune.run -> tune.run : 记录失败的 Trial 对象

tune.run -> tune.run : 根据所有训练完成的 Trial 对象，\n 构造 ExperimentAnalysis 对象，\n 返回训练结果

@enduml

-->

## 4. 使用实例

### 4.1. 结合 PyTorch 进行 GridSearch

基于 PyTorch，使用 Ray.Tune 框架训练一个 CNN 模型并针对超参数 lr 进行超参数调优：
```python
import torch.optim as optim

from ray import tune
from ray.tune.examples.mnist_pytorch import get_data_loaders, ConvNet, train, test

def train_mnist(config):
    train_loader, test_loader = get_data_loaders()
    model = ConvNet()
    optimizer = optim.SGD(model.parameters(), lr=config["lr"])
    for i in range(10):
        train(model, optimizer, train_loader)
        acc = test(model, test_loader)
        tune.track.log(mean_accuracy=acc)  # 向 Tune 上报训练指标

if __name__ == '__main__':
    analysis = tune.run(train_mnist, config={"lr": tune.grid_search([0.001, 0.01, 0.1])})

    print("Best config: ", analysis.get_best_config(metric="mean_accuracy"))
```

上述程序中，通过 `tune.run()` 接口向 Tune 传入了训练执行函数 train_mnist，并指定要优化的超参数 lr 的搜索空间，run 方法将阻塞至训练完毕并返回存储了训练结果的 ExperimentAnalysis 对象，最后调用 get_best_config() 方法，根据 mean_accuracy 表现找到最优的 lr 超参数配置。

### 4.2. 使用 BayesOptSearch + AsyncHyperBand

```python
def easy_objective(config, reporter):
    import time
    time.sleep(0.2)
    for i in range(config["iterations"]):
        reporter(
            timesteps_total=i,
            mean_loss=(config["height"] - 14)**2 - abs(config["width"] - 3))
        time.sleep(0.02)

if __name__ == "__main__":
    import argparse

    parser = argparse.ArgumentParser()
    parser.add_argument(
        "--smoke-test", action="store_true", help="Finish quickly for testing")
    args, _ = parser.parse_known_args()
    ray.init()

    space = {"width": (0, 20), "height": (-100, 100)}

    config = {
        "num_samples": 10 if args.smoke_test else 1000,
        "config": {
            "iterations": 100,
        },
        "stop": {
            "timesteps_total": 100
        }
    }
    algo = BayesOptSearch(
        space,
        max_concurrent=4,
        metric="mean_loss",
        mode="min",
        utility_kwargs={
            "kind": "ucb",
            "kappa": 2.5,
            "xi": 0.0
        })
    scheduler = AsyncHyperBandScheduler(metric="mean_loss", mode="min")
    run(easy_objective,
        name="my_exp",
        search_alg=algo,
        scheduler=scheduler,
        **config)
```

## 5. Refer Links

[Ray Github](https://github.com/ray-project/ray)

[Ray Tune Docs](https://ray.readthedocs.io/en/latest/tune.html)

[Tune: A Research Platform for Distributed Model Selection and Training](https://arxiv.org/pdf/1807.05118.pdf)

[Cutting edge hyperparameter tuning with Ray Tune](https://medium.com/riselab/cutting-edge-hyperparameter-tuning-with-ray-tune-be6c0447afdf)
- 译文：[用 Tune 快速进行超参数优化（Hyperparameter Tuning）](https://www.pytorchtutorial.com/use-tune-for-fast-hyperparameter-tuning/)

[Simple hyperparameter and architecture search in tensorflow with ray tune](http://louiskirsch.com/ai/ray-tune)
- 译文：[Tune：一个分布式模型选择与训练研究平台](https://blog.csdn.net/weixin_43255962/article/details/89322767)

[Ray----Tune(4):Tune 的搜索（Search）算法](https://blog.csdn.net/weixin_43255962/article/details/89307928)

[Ray----Tune(3):Tune 试验（trial）调度](https://blog.csdn.net/weixin_43255962/article/details/89290174)

[Ray----Tune(6):Tune 的实例（一)](https://blog.csdn.net/weixin_43255962/article/details/89416847)

[Tune Package Reference](https://ray.readthedocs.io/en/latest/tune-package-ref.html)
- 译文：[Ray----Tune(5):Tune 包中的类和函数参考](https://blog.csdn.net/weixin_43255962/article/details/89342609)
- 译文：[Ray Tune 相关 API 介绍](https://blog.csdn.net/u011254180/article/details/81175151)

[Tune User Guide](https://ray.readthedocs.io/en/latest/tune-usage.html#tune-user-guide)
- 译文：[Ray----Tune(2):Tune 的用户指南](https://blog.csdn.net/weixin_43255962/article/details/89012548)

[Tune design](https://ray.readthedocs.io/en/latest/tune-design.html)

[Tune 实例：Github - ray/python/ray/tune/examples/](https://github.com/ray-project/ray/tree/master/python/ray/tune/examples)
