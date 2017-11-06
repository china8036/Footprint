- [Anaconda 使用总结](#anaconda-%E4%BD%BF%E7%94%A8%E6%80%BB%E7%BB%93)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [介绍](#%E4%BB%8B%E7%BB%8D)
    - [安装](#%E5%AE%89%E8%A3%85)
      - [Windows](#windows)
      - [Linux](#linux)
  - [conda](#conda)
    - [环境管理](#%E7%8E%AF%E5%A2%83%E7%AE%A1%E7%90%86)
      - [使用 powershell 操作的坑](#%E4%BD%BF%E7%94%A8-powershell-%E6%93%8D%E4%BD%9C%E7%9A%84%E5%9D%91)
        - [PowerShell Execution Policy](#powershell-execution-policy)
        - [在 powershell 中 active 和 deactive 失效](#%E5%9C%A8-powershell-%E4%B8%AD-active-%E5%92%8C-deactive-%E5%A4%B1%E6%95%88)
    - [包管理](#%E5%8C%85%E7%AE%A1%E7%90%86)
    - [更改镜像源](#%E6%9B%B4%E6%94%B9%E9%95%9C%E5%83%8F%E6%BA%90)

# Anaconda 踏坑总结

## 概述

### 介绍

Anaconda 是一个用于科学计算的 Python 发行版，支持 Linux、Mac、Windows 系统，内置了常用的科学计算包； 

Anaconda 利用工具 / 命令 conda 来进行 package 和 environment 的管理，解决了官方 Python 的两大痛点：

  第一：提供了包管理功能，Windows 平台安装第三方包经常失败的场景得以解决；

  第二：提供环境管理的功能，功能类似 Virtualenv，解决了多版本 Python 并存、切换的问题；

### 安装

#### Windows

从官网上下载 Anaconda 最新安装包，安装成功后，将 Anaconda/Script 的路径（如 C:\ProgramData\Anaconda3\Scripts）加入到环境变量中；

验证安装成功：
```shell
# 查看版本
conda -V
# 查看帮助
conda -h
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/6/dc196e4ebafe7e32be7ff1bd6d0549d2.jpg)

#### Linux

```shell
wget anaconda Linux 版本安装包

bash xxxx.sh

export PATH=/usr/anaconda3/bin:$PATH
```

验证：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/6/328eff4c7881355805bd0070c8f7a7ba.jpg)

## conda

conda 是 Anaconda 下用于包管理和环境管理的工具，功能上类似 pip 和 vitualenv 的组合；

### 环境管理

基于 python3.6 版本创建一个名字为 python36 的环境（不用管是 3.6.x，conda 会为我们自动寻找 3.6.x 中的最新版本）
```
$ conda create --name python36 python=3.6 
```
激活 / 进入此环境
```
$ activate python36  # windows
# surce activate python36 # linux/mac
```
检查 python 版本
```
$ python -V  
```
退出当前环境
```
$ deactivate python36 
```
删除该环境
```
$ conda remove -n python36 --all
# 或
# conda env remove  -n python36
```

用户安装的不同版本 Python 环境都会放在目录~/ananconda2/envs 下，查看所有已安装的环境（当前被激活的环境会显示有一个星号)
```
$ conda info -e
python36              *  D:\Programs\Anaconda3\envs\python36
root                     D:\Programs\Anaconda3
```

#### 使用 powershell 操作的坑

##### PowerShell Execution Policy

ps1 脚本没有数字签名，导致无法运行 activate.ps1 和 deactivate.ps1 脚本；

解决：

设置 ExcutionPolicy 为 CurrentUser Scope
```
Set-Executionpolicy -Scope CurrentUser -ExecutionPolicy UnRestricted
```
参考：http://www.freebuf.com/articles/system/93829.html 

##### 在 powershell 中 active 和 deactive 失效

conda 默认只包含了 activate.bat 和 deactivate.bat，在 powershell 中无法很好的执行，因此，需要这两个脚本的 ps1 实现形式：
https://github.com/firejq/PSCondaEnvs/tree/patch-1 

在原项目中的 activate.ps1 文件中加了一行，解决了无法使用 deactivate 的问题
```
Copy-Item -Path "$anacondaInstallPath\Scripts\deactivate.ps1" -Destination "$env:ANACONDA_ENVS\$condaEnvName\Scripts"
```

### 包管理

conda 的包管理功能与 pip 基本相同；

例：
```python
# 安装 package
conda install -n python36 matplotlib
# 如果不用 -n 指定环境名称，则被安装在当前活跃环境
conda install matplotlib
# 也可以通过 -c 指定通过某个 channel 安装

# 包更新
conda update matplotlib
# conda update -n python36 matplotlib

# 删除包
conda remove matplotlib
# conda remove -n python36 matplotlib

# 查看已安装的包
conda list 
# 查看某个指定环境的已安装包
conda list -n python35

# 查找 package 信息
conda search numpy
```

在 conda 中 anything is a package，也就是说，conda 本身、python 环境、anaconda 也都可以看作是一个包，因此除了普通的第三方包支持更新之外，这 3 个包也支持；

例：
```shell
# 更新 conda 本身
conda update conda
# 更新 anaconda 应用
conda update anaconda
# 更新 python，假设当前 python 环境是 3.6.1，而最新版本是 3.6.2，那么就会升级到 3.6.2
conda update python
```

创建新环境时，默认只会安装该 python 版本必需的包，若希望安装 Anaconda 自带的科学计算依赖包，则只需在创建环境的时候一并安装 anaconda 包集合：

例：
```shell
conda create -n python35 python=3.5 anaconda
```

NOTE：
安装 Anaconda 后，只是提供多了一个包管理工具 conda，python 自带的 pip 依旧可以使用；

### 更改镜像源

使用清华的镜像源：

修改 ~/.condarc 文件（若不存在则手动创建即可）如下：
```
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
  - conda-forge
  - defaults
ssl_verify: true
show_channel_urls: true
```
