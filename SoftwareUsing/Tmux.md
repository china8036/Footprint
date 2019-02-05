- [Tmux](#tmux)
  - [1. Shell Command](#1-shell-command)
  - [2. Prefix Command](#2-prefix-command)
  - [3. Refer Links](#3-refer-links)

# Tmux

## 1. Shell Command

- 启动新会话：
  ```shell
  tmux [ new -s 会话名 -n 窗口名 ]
  ```
- 恢复会话：
  ```shell
  tmux at [-t 会话名』
  ```
- 列出所有会话：
  ```shell
  tmux ls
  ```
- 关闭会话：
  ```shell
  tmux kill-session -t 会话名
  ```
- 关闭所有会话：
  ```shell
  tmux ls | grep : | cut -d. -f1 | awk '{print substr($1, 0, length($1)-1)}' | xargs kill
  ```

## 2. Prefix Command

在 Tmux 中，按下 Tmux 前缀 ctrl+b，然后即可执行以下命令：

- 基本
  - `d`  退出 tmux（tmux 仍在后台运行）
  - `t`  窗口中央显示一个数字时钟
  - `?`  列出所有快捷键
  - `:`  命令提示符

- 会话
  - `:new`  启动新会话
  - `s`           列出所有会话
  - `$`           重命名当前会话
- 窗口 （标签页)
  - `c`  创建新窗口
  - `w`  列出所有窗口
  - `n`  后一个窗口
  - `p`  前一个窗口
  - `f`  查找窗口
  - `,`  重命名当前窗口
  - `&`  关闭当前窗口
  - swap-window -s 3 -t 1  交换 3 号和 1 号窗口
  - swap-window -t 1       交换当前和 1 号窗口
  - move-window -t 1       移动当前窗口到 1 号
- 窗格（分割窗口）
  - `%`  垂直分割
  - `"`  水平分割
  - `o`  交换窗格
  - `x`  关闭窗格
  - `space `  切换布局
  - `q` **显示每个窗格是第几个，当数字出现的时候按数字几就选中第几个窗格**
  - `{` 与上一个窗格交换位置
  - `}` 与下一个窗格交换位置
  - `z` 切换窗格最大化 / 最小化

## 3. Refer Links

[Tmux - Linux 从业者必备利器](http://cenalulu.github.io/linux/tmux/)

[Tmux 快捷键 & 速查表](https://gist.github.com/ryerh/14b7c24dfd623ef8edc7)

[十分钟学会 tmux](https://www.cnblogs.com/kaiye/p/6275207.html)