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

- 更换国内源

  在项目目录下的 Pipfile 中将 url 设置为国内地址即可：
  - 阿里云：http://mirrors.aliyun.com/pypi/simple/
  - 豆瓣：http://pypi.douban.com/simple/
  - 清华大学：https://pypi.tuna.tsinghua.edu.cn/simple/
  - 中国科学技术大学：https://pypi.mirrors.ustc.edu.cn/simple/

## 3. 使用

[Python 实践 32-PyCharm 里使用 pipenv 创建的环境](https://zhuanlan.zhihu.com/p/33407501)

- 创建项目目录
  ```
  mkdir newproject
  cd newproject
  pipenv install （耐心等待)
  pipenv shell # 进入新环境
  pipenv install requests # 安装第三方包
  exit # 退出
  ```

- 配置 PyCharm 使用 pipenv 环境

  1. 启动 PyCharm，打开名称为 new 的项目
  1. 进入项目设置，搜索 Project Interpreter
  1. 在 Project Interpreter 的右上角配置按钮上选择 Add Local
  1. 选择 VirtualEnv Environment
  1. 复制刚才的环境路径"/Users/zyt/.local/share/virtualenvs/new-cRH-55u9/bin/python"到粘贴板，粘贴到 existing environment 的 interpreter 下面，点击确定。
  1. 这样，你就可以在 PyCharm 里用为 new 专门创建的 python 环境了

## 4. Refer Links

[Pipenv – 超好用的 Python 包管理工具](https://segmentfault.com/a/1190000015389565)

[Pipenv：新一代 Python 项目环境与依赖管理工具](https://zhuanlan.zhihu.com/p/37581807)
