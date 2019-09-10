- [Python pyinstaller Note](#python-pyinstaller-note)
  - [1. 简介](#1-简介)
  - [2. 使用](#2-使用)
    - [2.1. 安装](#21-安装)
    - [2.2. 命令](#22-命令)
    - [2.3. 打包为一个文件夹](#23-打包为一个文件夹)
    - [2.4. 打包为一个文件](#24-打包为一个文件)
    - [2.5. spec 文件](#25-spec-文件)

# Python pyinstaller Note

## 1. 简介

PyInstaller 可以用来打包 python 应用程序，打包完的程序就可以在没有安装 Python 解释器的机器上运行了。PyInstaller 支持 Python 2.7 和 Python 3.3+。

Pyinstaller 可以在 Windows、Mac OS X 和 Linux 上使用，但在不同的平台上使用其效果是不相同的：

1)	在 Windows 上运行 PyInstaller 进行打包工作，会生成 exe 文件；

2)	在 Mac OS 上运行 Pyinstaller 进行打包工作，会打包成 mac app；

## 2. 使用

### 2.1. 安装

```
pip install pyinstaller
```

安装完成后会自动将文件加入环境变量，可在命令行中直接执行；

注意：
(2017/4/28) 此时的 python 最新版为 3.6.1，而 pyinstaller 最新发行版为 3.2.1，此版本不支持 py3.6！
运行会报错：
```
IndexError: tuple index out of range
```
解决方法：
官方在 GitHub 已经更新了 pyinstaller3.3 的源码，但还没有发布，而 pyinstaller3.3 版本是支持 oy3.6 的，因此，在 https://github.com/pyinstaller/pyinstaller 下载源码包，解压缩后复制其中的 Pyinstaller 文件夹，替换`C:\Users\firej\AppData\Local\Programs\Python\Python36\Lib\site-packages\PyInstaller`，即可。

### 2.2. 命令

在命令行中切换到要打包的程序所在目录，直接输入指令即可：
```
pyinstaller demo.py
# -F	指定打包后只生成一个可执行文件（如 exe）
# -D	–onedir 创建一个目录，包含可执行文件，但会依赖很多文件（默认）
# -c	–console, –nowindowed 使用控制台，无 GUI 界面（默认)
# -w	–windowed, –noconsole 使用 GUI 窗口，运行程序时不显示控制台，开发 GUI 应用时应加上该选项
# -p	添加搜索路径，自定义需要加载的类路径，让其找到对应的库。一般情况下用不到
# -i	改变生成程序的 icon 图标
# -h 	查看选项帮助
```

PyInstaller 会分析你的 python 程序，找到所有的依赖项。然后将依赖文件和 python 解释器放到一个文件夹下或者一个可执行文件中。

### 2.3. 打包为一个文件夹

使用 PyInstaller 打包的时候，默认生成一个文件夹，文件夹中包含所有依赖项，以及可执行文件。打包成文件夹的好处就是 debug 的时候可以清楚的看到依赖项有没有包含。另一个好处是更新的时候，只需要更新可执行文件就可以了。当然缺点也很明显，不方便，不易管理。
```
pyinstaller xxx.py
```

它是如何工作的呢？PyInstaller 的引导程序是一个二进制可执行程序。当用户启动你的程序的时候，PyInstaller 的引导程序开始运行，首先创建一个临时的 Python 环境，然后通过 Python 解释器导入程序的依赖，当然他们都在同一个文件夹下。

### 2.4. 打包为一个文件

```
pyinstaller -F xxx.py
```
打包成一个文件相对于文件夹更容易管理。坏处运行相对比较慢。这个文件中包含了压缩的依赖文件拷贝（.so 文件）。
当程序运行时，PyInstaller 的引导程序会新建一个临时文件夹。然后解压程序的第三方依赖文件到临时文件夹中。这也是为什么一个可执行文件比文件夹中执行的时间要长的原因。其余的和打包成一个文件夹是相同的。

注意：

- 运行`pyinstaller –F xx.py` 之后，会在当前目录下生成 dist、build 和一个 spec 文件，这些都是打包过程中生成的东西，因 -F 参数会将所有依赖打包到可执行文件中，这几个副产品都可以删除。
- 生成的可执行文件与 xxx.py 同名，如 xxx.exe，注意若 py 中使用了与路径相关的代码，exe 文件必须放在与 py 文件相同的路径下才能正确运行。

### 2.5. spec 文件

http://legendtkl.com/2015/11/06/pyinstaller/

```
pyinstaller options..script.py
```
PyInstaller 首先建一个 sepc(specification) 文件：script.spec。这个文件的存放地址可以使用参数–specpath= 来定义，默认放在当前文件夹下。

spec 文件的作用是什么呢？它会告诉 PyInstaller 如何处理你的 py 文件，它会将你的 py 文件名字和输入的大部分参数进行编码。PyInstaller 通过执行 spec 文件中的内容来生成 app，有点像 makefile。正常使用中我们是不需要管 spec 文件的，但是下面几种情况需要修改 spec 文件：
- 需要打包资源文件
- 需要 include 一些 PyInstaller 不知道的 run-time 库
- 为可执行文件添加 run-time 选项
- 多程序打包
