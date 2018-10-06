- [Shell 变量](#shell-变量)
  - [1. 用户自定义变量](#1-用户自定义变量)
  - [2. 位置变量](#2-位置变量)
  - [3. 预定义变量](#3-预定义变量)
    - [3.1. 环境变量](#31-环境变量)
    - [3.2. 特殊变量](#32-特殊变量)
  - [4. Refer Links](#4-refer-links)

# Shell 变量

## 1. 用户自定义变量

## 2. 位置变量

## 3. 预定义变量

### 3.1. 环境变量

### 3.2. 特殊变量

普通变量的变量名只能包含数字、字母和下划线，如果一个变量的变量名中包含其他特殊含义的字符，这样的变量被称为特殊变量。

- 命令行参数
  - `$n`: 表示传递给脚本或函数的指定参数。其中，n 是一个数字，表示第几个参数。例如，第一个参数是 $1，第二个参数是 $2。特别的，`$0` 表示当前脚本的文件名。
  - `$#`: 表示传递给脚本或函数的参数个数。
  - `$*`: 表示传递给脚本或函数的所有参数，即以"$1" "$2" ... "$n" 的形式输出所有参数。当被双引号 (" ") 包含时，"$*" 会将所有的参数作为一个整体，以"$1 $2 ... $n"的形式输出所有参数。
  - `$@`: 表示传递给脚本或函数的所有参数，即以"$1" "$2" ... "$n" 的形式输出所有参数。当被双引号 (" ") 包含时，"$@" 会将各个参数分开，以"$1" "$2" ... "$n" 的形式输出所有参数。
- 返回值
  - `$?`: 表示上个命令的退出状态或函数的返回值。大部分命令执行成功会返回 0，失败返回 1。
- PID
  - `$$`: 表示当前 Shell 进程的 PID。对于 Shell 脚本，就是这个脚本所在的进程的 PID。

eg:
```shell
#!/bin/bash

echo "File Name: $0"
echo "First Parameter : $1"
echo "First Parameter : $2"
echo "Quoted Values: $@"
echo "Quoted Values: $*"
echo "Total Number of Parameters : $#"
```
执行脚本：
```
$ ./test.sh AAA BBB

  File Name : ./test.sh
  First Parameter : AAA
  Second Parameter : BBB
  Quoted Values: AAA BBB
  Total Number of Parameters : 2
```

eg:
```shell
#!/bin/bash
echo "\$*=" $*
echo "\"\$*\"=" "$*"
echo "\$@=" $@
echo "\"\$@\"=" "$@"
echo "print each param from \$*"
for var in $*
do
    echo "$var"
done
echo "print each param from \$@"
for var in $@
do
    echo "$var"
done
echo "print each param from \"\$*\""
for var in "$*"
do
    echo "$var"
done
echo "print each param from \"\$@\""
for var in "$@"
do
    echo "$var"
done
```
执行脚本：
```
$ ./test.sh "a" "b" "c" "d"

  $*=  a b c d
  "$*"= a b c d
  $@=  a b c d
  "$@"= a b c d
  print each param from $*
  a
  b
  c
  d
  print each param from $@
  a
  b
  c
  d
  print each param from "$*"
  a b c d
  print each param from "$@"
  a
  b
  c
  d
```

## 4. Refer Links

[Shell 特殊变量：Shell $0, $#, $*, $@, $?, $$ 和命令行参数](http://c.biancheng.net/cpp/view/2739.html)
