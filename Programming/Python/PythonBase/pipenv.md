- [pipenv](#pipenv)
  - [1. 基础概念](#1-基础概念)
  - [2. 安装和配置](#2-安装和配置)
  - [3. 使用](#3-使用)
  - [4. Refer Links](#4-refer-links)

# pipenv

## 1. 基础概念

[pipenv](https://pipenv.readthedocs.io/en/latest/) 是 python 官方推荐的包管理工具，集成了 virtualenv、pyenv 和 pip 三者的功能于一身，类似于 php 中的 composer、nodejs 中的 npm 等现代包管理工具。

## 2. 安装和配置

安装：
```
brew install pipenv
```

配置：
- 自动补齐

  Linux or Mac 环境下，bash 下如果能自动命令补全岂不是更好？把如下语句追加到 `.bashrc` 或者 `.zshrc` 即可：
  ```
  eval "$(pipenv --completion)"
  ```

- 自定义虚拟环境文件夹路径

  默认情况下，Pipenv 会自动为你选择虚拟环境的存储位置，在 Windows 下通常为 `C:\Users\Administrator\.virtualenvs\`，而 Linux 或 macOS 则为 `~/.local/share/virtualenvs/`。如果你想将虚拟环境文件夹在项目目录内创建，可以设置环境变量 `export PIPENV_VENV_IN_PROJECT=1`，这时名为 `.venv` 的虚拟环境文件夹将在项目根目录被创建。

  也可以通过 WORKON_HOME 环境变量来自定义存储路径。

## 3. 使用

[Python 实践 32-PyCharm 里使用 pipenv 创建的环境](https://zhuanlan.zhihu.com/p/33407501)

## 4. Refer Links

[Pipenv – 超好用的 Python 包管理工具](https://segmentfault.com/a/1190000015389565)

[Pipenv：新一代 Python 项目环境与依赖管理工具](https://zhuanlan.zhihu.com/p/37581807)
