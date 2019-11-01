- [IPython & Jupyter](#ipython--jupyter)
  - [1. IPython](#1-ipython)
    - [1.1. 基本概念](#11-基本概念)
    - [1.2. 安装配置](#12-安装配置)
    - [1.3. 使用技巧](#13-使用技巧)
      - [1.3.1. 特殊命令](#131-特殊命令)
      - [1.3.2. Magic 函数](#132-magic-函数)
      - [1.3.3. 扩展系统](#133-扩展系统)
        - [1.3.3.1. storemagic](#1331-storemagic)
        - [1.3.3.2. autoreload](#1332-autoreload)
        - [1.3.3.3. 自定义扩展](#1333-自定义扩展)
  - [2. IPython Notebook / Jupyter Notebook](#2-ipython-notebook--jupyter-notebook)
    - [2.1. 基本概念](#21-基本概念)
    - [2.2. 安装配置](#22-安装配置)
    - [2.3. 使用方法](#23-使用方法)
      - [2.3.1. 基本操作](#231-基本操作)
      - [2.3.2. kernel 操作](#232-kernel-操作)
  - [3. Refer Links](#3-refer-links)

# IPython & Jupyter

## 1. IPython

### 1.1. 基本概念

[IPython](https://ipython.org/) 由 Fernando Pérez 于 2001 年开始开发，他的目标是让 IPython 成为像 Mathematica 一样的交互式计算环境，这是科研工作者熟悉的工作环境。

知乎 @wjhbb：
> 事实上，IPython Notebook （现在叫 Jupyter Notebook) 跟 Mathematica 的 Notebook 很像：不止是一个执行和调试代码的环境，同时也是一个可以写文档 （支持 markdown 语法) 和进行交互式绘图的环境；如果说有些地方还不够像 Mathematica Notebook，那是因为后者申请了专利，所以不敢做得太像。

在 IPython 4.0 版本之前，IPython 包含了 notebook server、qtconsole 等多种丰富的 language-agnostic components；从 4.0 版本开始，所有 language-agnostic components 移到了 Jupyter 项目中，而 [IPython](https://ipython.readthedocs.io/en/stable/) 只包含 2 个主要的组件：
- A powerful interactive Python shell：交互式解释器环境。
- A Jupyter kernel to work with Python code in Jupyter notebooks and other interactive frontends.：作为 Jupyter Kernel 为 Jupyter notebooks 提供支持。

> IPython is a growing project, with increasingly language-agnostic components. IPython 3.x was the last monolithic release of IPython, containing the notebook server, qtconsole, etc. As of IPython 4.0, the language-agnostic parts of the project: the notebook format, message protocol, qtconsole, notebook web application, etc. have moved to new projects under the name Jupyter. IPython itself is focused on interactive Python, part of which is providing a Python kernel for Jupyter.

### 1.2. 安装配置

使用 pip 进行安装：
```
pip install ipython
pip3 install ipython
```

安装完成后：
- 执行 `ipython2` 进入 python2 环境。
- 执行 `ipython3` 进入 python3 环境。
- 执行 `ipython` 进入 python3 环境，可以通过 `vim ipython` 修改第一行指定的 python 解释器类型来改变 `ipython` 使用的默认 python 版本。

如果我们也想在 notebook 中或者在 Qt console 中使用 IPython，我们还需要安装 Jupyter，如下命令：
```
pip install jupyter
```

### 1.3. 使用技巧

IPython 支持变量自动补全和自动缩进，支持 bash shell 命令，内置了许多很有用的功能和函数。IPython 目前的主要功能如下：

#### 1.3.1. 特殊命令

- tab 自动补全

- `?` & `??`
  - 在变量的前面或者后面加上一个问号 `?`，就可以将有关该对象的一些通用信息显示出来（如 Type、Length、Docstring 等）。
  - 如果使用两个问号 `??`，那么还可以显示出该方法的源代码。

- `!`

  调用系统 Shell 命令。只需要在命令前加 `!` 即可。

#### 1.3.2. Magic 函数

- `%lsmagic`: 可以使用获得全部可用的 Magic 函数。

- `%history`: IPython 把输入的历史记录存放在个人配置目录下的 history.sqlite 文件中，可以使用 history 命令 (`hisstory -n`) 来查看所有的历史输入，并且可以结合 `%rerun`、`%recall`、`%macro`、`%save` 等 Magic 函数使用。除此之外，ipython 还把最近的三次执行记录绑定在 `_`、`__` 和 `___` 这三个变量上，`_n` 和 `_in` 变量则保存了第 n 个输出和输入的历史命令。

- `%timeit`: 使用 %timeit 命令快速测量时间。

- `%run`: 使用 %run 命令运行脚本。

- `%pdb`: 使用 %pdb 命令快速 debug。

- `%debug`: 如果控制台抛出了一个异常，可以使用 `%debug` 魔法命令在异常点启动调试器。接着你就能调试模式下访问所有的本地变量和整个栈回溯。使用 u 和 d 向上和向下访问栈，使用 q 退出调试器。

- `%pylab`: 魔法命令 `%pylab` 可以使 Numpy 和 matplotlib 中的科学计算功能生效，这些功能被称为基于向量和矩阵的高效操作，交互可视化特性。它能够让我们在控制台进行交互式计算和动态绘图。

#### 1.3.3. 扩展系统

##### 1.3.3.1. storemagic

storemagic 可以持久化宏、变量和别名。

启用 storemagic 需要添加如下配置到 ipython_config.py 实现自动保存：
```
c.StoreMagics.autorestore = True
```

e.g.

第一次在 IPython 中执行如下命令：
```python
In : l = ['hello', 10, 'world']
In : %store l
Stored 'l' (list)
In : exit()
```
现在退出 IPython 后重新进入：
```python
In : l
Out: ['hello', 10, 'world']
```
可以看到，l 能直接使用。

我们还可以用这个功能保存一些重要的资源，这样即使退出 IPython 也能找回来。

##### 1.3.3.2. autoreload

autoreload 可以让我们不退出 IPython 就动态修改代码，在执行代码前 IPython 会帮我们自动重载改动的模块。

e.g.
```python
❯ cat py_autoreload.py
def a():
    return 1
```
在 IPython 里面执行它：
```python
In : %load_ext autoreload
In : %autoreload 2
In : from py_autoreload import a
In : a()
Out: 1
```
然后打开另外一个终端修改函数 a:
```python
def a():
    return 2
```
在之前的 IPython 中重新调用 a 函数：
```python
In : a()
Out: 2
```
可以看到返回值动态地改变了。

##### 1.3.3.3. 自定义扩展

## 2. IPython Notebook / Jupyter Notebook

### 2.1. 基本概念

> The notebook extends the console-based approach to interactive computing in a qualitatively new direction, providing a web-based application suitable for capturing the whole computation process: developing, documenting, and executing code, as well as communicating the results.

[Jupyter Notebook](https://jupyter-notebook.readthedocs.io/en/stable/) 使用浏览器作为界面，可以在网页页面中直接编写代码，通过向后台的 Jupyter 服务器发送请求来运行代码并在网页上显示运行结果。同时，Jupyter 也支持 MarkDown 语法，如果在编程过程中需要编写说明文档，可在同一个页面中直接编写，便于作及时的说明和解释。

> The Jupyter notebook combines two components:
>
> A web application: a browser-based tool for interactive authoring of documents which combine explanatory text, mathematics, computations and their rich media output.
>
> Notebook documents: a representation of all content visible in the web application, including inputs and outputs of the computations, explanatory text, mathematics, images, and rich media representations of objects.

Jupyter Notebook 主要由 2 部分组成：
- 网页应用

  网页应用即基于网页形式的、结合了编写说明文档、数学公式、交互计算和其他富媒体形式的工具。简言之，网页应用是可以实现各种功能的工具。

- 文档

  Jupyter Notebook 中所有交互计算、编写说明文档、数学公式、图片以及其他富媒体形式的输入和输出，都是以文档的形式体现的。这些文档是保存为后缀名为。ipynb 的 JSON 格式文件，不仅便于版本控制，也方便与他人共享。

  此外，文档还可以导出为：HTML、LaTeX、PDF 等格式。

Jupyter Notebook 的主要特点：
- 编程时具有语法高亮、缩进、tab 补全的功能。
- 可直接通过浏览器运行代码，同时在代码块下方展示运行结果。
- 以富媒体格式展示计算结果。富媒体格式包括：HTML，LaTeX，PNG，SVG 等。
- 对代码编写说明文档或语句时，支持 Markdown 语法。
- 支持使用 LaTeX 编写数学性说明。

### 2.2. 安装配置

jupyter 安装
```
pip install jupyter
pip3 install jupyter
```

### 2.3. 使用方法

#### 2.3.1. 基本操作

- 帮助信息
  ```
  jupyter notebook -h
  ```

- 启动（默认监听 8888 端口）
  ```
  jupyter notebook --port <port_number>
  ```

  若默认端口“8888”被占用，因此地址栏中的数字将从“8888”起，每多启动一个 Jupyter Notebook 数字就加 1，如“8889”、“8890”。

  启动服务器但不打开浏览器：
  ```
  jupyter notebook --no-browser
  ```

#### 2.3.2. kernel 操作

- 添加 kernel（其它 python 版本）
  ```
  pip install ipykernel
  python2 -m ipykernel uninstall --name python27
  ```
- 列出所有 kernel
  ```
  jupyter kernelspec list
  ```
- 删除指定 kernel
  ```
  jupyter kernelspec remove python27
  ```
- 列出当前运行的 kernel
  ```
  jupyter notebook list
  ```

## 3. Refer Links

[IPython 介绍](https://blog.csdn.net/gavin_john/article/details/53086766)

[知乎：使用 IPython 有哪些好处？](https://www.zhihu.com/question/51467397)

TODO:

[Python 数据科学手册：第 1 章　IPython：超越 Python](http://www.ituring.com.cn/book/tupubarticle/19702)

[Jupyter Notebook 介绍、安装及使用教程](https://zhuanlan.zhihu.com/p/33105153)