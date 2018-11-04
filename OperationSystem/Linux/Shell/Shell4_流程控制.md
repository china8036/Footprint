- [Shell 流程控制](#shell-流程控制)
  - [1. 条件控制](#1-条件控制)
  - [2. 循环控制](#2-循环控制)
    - [2.1. for 循环](#21-for-循环)
    - [2.2. while 循环](#22-while-循环)
    - [2.3. until 循环](#23-until-循环)
    - [2.4. break 控制](#24-break-控制)
    - [continue 控制](#continue-控制)
  - [3. 选择控制](#3-选择控制)
    - [3.1. case 结构](#31-case-结构)
    - [3.2. select 结构](#32-select-结构)
  - [4. 函数](#4-函数)
  - [5. Refer Links](#5-refer-links)

# Shell 流程控制

NOTE: 和 Java、PHP 等语言不一样，shell 的流程控制不可为空。

## 1. 条件控制

```shell
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

## 2. 循环控制

### 2.1. for 循环

```shell
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

NOTE
- `in 列表` 是可选的，如果不用它，for 循环使用命令行的位置参数。
- 列表中可使用通配符，如 `for var in *.txt`.

### 2.2. while 循环

```shell
while condition
do
    command
done
```

eg: 循环输入
```shell
while read INPUT
do
  echo 'your input is $INPUT'
done
```
使用 CTRL + D 结束循环。

### 2.3. until 循环

until 循环与 while 循环在处理方式上刚好相反。until 循环执行一系列命令直至条件为 true 时停止。

```shell
until condition
do
    command
done
```

util 循环的每次循环都先执行再检测条件，因此循环至少会执行一次。

### 2.4. break 控制

`break + n` 跳出 n 层循环（不加 n 则默认跳出 1 层循环）。

### continue 控制

`continue + n` 跳出 n 层循环（不加 n 则默认跳出 1 层循环）。

## 3. 选择控制

### 3.1. case 结构

```shell
case vaule in
pattern 1)
    command1
    command2
    ...
    commandN
    ;;
pattern 2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

NOTE:
- 每个模式必须以右括号结束，取值可为常数或变量。
- 模式中可使用“或”，即 `|`。
- `;;` 相当于其它语言中的 `break`。
- `*)` 为默认匹配，相当于其它语言的 default。
- 涉及较多的条件判断时，case 结构比 if 结构效率更优。

### 3.2. select 结构

```shell
select var in iterm1 iterm2 ...
do
  ...
done
```

## 4. 函数

linux shell 可以用户定义函数，然后在 shell 脚本中可以随便调用。

```shell
[ function ] funname [()]
{

    action;

    [return int;]

}
```
- `[]` 中的都是可选项，即可以带 function fun() 定义，也可以直接 fun() 定义，不带任何参数。
- 若无 return 值，则以最后一条命令的运行结果作为返回值。
- 可将函数定义在其它文件中，在需要使用的地方使用 `source + file_name` / `. file_name` 导入即可（被包含的文件不需要有执行权限）。

eg:
```bash
#!/bin/bash

demoFun(){
  echo "这是我的第一个 shell 函数！"
}
echo "----- 函数开始执行 -----"
demoFun
echo "----- 函数执行完毕 ----
```

## 5. Refer Links
