- [Ray 概述](#ray-概述)
  - [1. 基本组成](#1-基本组成)
  - [Distributed Training](#distributed-training)
  - [2. Tune](#2-tune)
  - [3. RLib](#3-rlib)
  - [5. Refer Links](#5-refer-links)

# Ray 概述

> Ray is a fast and simple framework for building and running distributed applications.

Ray 是由加州大学伯克利分校 RISELab 开源的新兴人工智能应用的分布式框架。它实现了一个统一的接口、分布式调度器、分布式容错存储，以满足高级人工智能技术对系统最新的、苛刻的要求。Ray 允许用户轻松高效地运行许多新兴的人工智能应用，例如，使用 RLlib 的深度强化学习、使用 Ray Tune 的可扩展超参数搜索、使用 AutoPandas 的自动程序合成等等。

## 1. 基本组成

Ray is packaged with the following libraries for accelerating machine learning workloads:
- Tune: Scalable Hyperparameter Tuning

- RLlib: Scalable Reinforcement Learning

- Distributed Training

## Distributed Training

## 2. Tune

With Tune, you can launch a multi-node distributed hyperparameter sweep in less than 10 lines of code. Tune supports any deep learning framework, including PyTorch, TensorFlow, and Keras.

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
        tune.track.log(mean_accuracy=acc)

analysis = tune.run(
    train_mnist, config={"lr": tune.grid_search([0.001, 0.01, 0.1])})

print("Best config: ", analysis.get_best_config(metric="mean_accuracy"))

df = analysis.dataframe()  # Get a dataframe for analyzing trial results.
```

## 3. RLib

RLlib is an open-source library for reinforcement learning built on top of Ray that offers both high scalability and a unified API for a variety of applications.

```python
import gym
from gym.spaces import Discrete, Box
from ray import tune

class SimpleCorridor(gym.Env):
    def __init__(self, config):
        self.end_pos = config["corridor_length"]
        self.cur_pos = 0
        self.action_space = Discrete(2)
        self.observation_space = Box(0.0, self.end_pos, shape=(1, ))

    def reset(self):
        self.cur_pos = 0
        return [self.cur_pos]

    def step(self, action):
        if action == 0 and self.cur_pos > 0:
            self.cur_pos -= 1
        elif action == 1:
            self.cur_pos += 1
        done = self.cur_pos >= self.end_pos
        return [self.cur_pos], 1 if done else 0, done, {}

tune.run(
    "PPO",
    config={
        "env": SimpleCorridor,
        "num_workers": 4,
        "env_config": {"corridor_length": 5}})
```

Distributed Training

```python
import ray
ray.init()

@ray.remote
def f(x):
    return x * x

futures = [f.remote(i) for i in range(4)]
print(ray.get(futures))

To use Ray’s actor model:
import ray
ray.init()

@ray.remote
class Counter():
    def __init__(self):
        self.n = 0

    def increment(self):
        self.n += 1

    def read(self):
        return self.n

counters = [Counter.remote() for i in range(4)]
[c.increment.remote() for c in counters]
futures = [c.read.remote() for c in counters]
print(ray.get(futures))
```


## 5. Refer Links

[Ray Github](https://github.com/ray-project/ray)

Docs:
- [Ray Docs](https://ray.readthedocs.io/en/latest/index.html)
- [Ray Tune Docs](https://ray.readthedocs.io/en/latest/tune.html)
- [RLlib Docs](https://ray.readthedocs.io/en/latest/rllib.html)

Paper:
- [Ray: A Distributed Framework for Emerging AI Applications](https://arxiv.org/pdf/1712.05889.pdf)
- [Real-Time Machine Learning: The Missing Pieces](https://arxiv.org/pdf/1703.03924.pdf)
- [RLlib: Abstractions for Distributed Reinforcement Learning](https://arxiv.org/pdf/1712.09381.pdf)
- [Tune: A Research Platform for Distributed Model Selection and Training](https://arxiv.org/pdf/1807.05118.pdf)

[Ray - 面向增强学习场景的分布式计算框架](https://blog.csdn.net/colorant/article/details/80417412)

[知乎：如何看 UCBerkeley RISELab 即将问世的 Ray，replacement of Spark？](https://www.zhihu.com/question/265485941)

[Intel Courses: Distributed AI with the Ray Framework](https://software.intel.com/en-us/ai/courses/distributed-AI-ray)

[Ray：面向 AI 应用的分布式执行框架](https://www.infoq.cn/article/ray-ai-framework)

[取代 Python 多进程！伯克利开源分布式框架 Ray](https://www.infoq.cn/article/6_7CfthGiXg0aytptoai)
