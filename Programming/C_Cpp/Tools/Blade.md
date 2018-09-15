- [构建工具 Blade](#构建工具-blade)
  - [1. Blade 安装](#1-blade-安装)
  - [2. 根目录描述文件 BLADE_ROOT](#2-根目录描述文件-blade_root)
  - [3. 构建描述文件 BUILD](#3-构建描述文件-build)
  - [4. Blade 命令行](#4-blade-命令行)
  - [5. Blade 集成测试](#5-blade-集成测试)
  - [6. Refer Links](#6-refer-links)

# 构建工具 Blade

## 1. Blade 安装

1. 从 GitHub 上下载 Blade 项目文件
1. 在项目目录下，执行 `sudo ./install `
1. `source ~/.profile`
1. 校验安装是否成功：`blade -h`

## 2. 根目录描述文件 BLADE_ROOT

Blade 获取源代码根的方法是，无论当前从哪一级子目录运行，都从当前目录开始向上查找 BLADE_ROOT 文件，有这个文件的目录即为源代码树的根。该文件是空的，只是用于 blade 判断根目录"//"的位置所在，所有的编译配置文件都不使用".."这种相对路径。

## 3. 构建描述文件 BUILD

Blade 通过一系列名字为 "BUILD" 的构建描述文件（文件名全大写）进行项目构建，这些文件需要开发者去编写。每个 BUILD 文件通过一组目标描述函数描述一个目标的源文件，所依赖的其他目标，以及其它一些属性等。

eg:
```
proto_library(
  name = 'card_interface_proto', # 目标名
  srcs = [
    'card_interface.proto' # 源代码集合
  ],
)

cc_library(
  name = 'protobuf_util',
  srcs = [
    'protobuf_util.cc',
  ],
  deps = [
    '//thirdparty/glog:glog', # 直接依赖集合，没有可以不写
    '//thirdparty/protobuf:protobuf',
    ':card_interface_proto',
  ]
)

cc_library(
  name = 'test_helper',
  srcs = [
    'test_helper.cc',
  ],
)

cc_test(
  name = 'protobuf_util_test',
  srcs = [
    'protobuf_util_test.cc',
  ],
  deps = [
    '//thirdparty/gmock:gmock',
    '//thirdparty/gflags:gflags',
    ':protobuf_util',
    ':test_helper',
  ],
  testdata = [
    ('//card/testdata/card.conf', 'card.conf'),
    ('//log', 'log'),
  ],
)
```
- 构建目标
  
  Blade 支持的构建目标有 11 种：
  - cc_library: 编译库文件的标记
  - cc_binary: 编译二进制文件的标记
  - cc_test
  - proto_library
  - lex_yacc_library
  - gen_rule
  - swig_library
  - cc_plugin
  - resource_library
  - java_jar
  - py_binary

- Blade 用一组 target 函数来定义目标。
  
  target 的通用属性有：
  - name: 编译的目标名，和路径一起成为 target 的唯一标识，也决定了构建的输出命名
  - srcs: 列表或字符串，构建该对象需要的源文件，一般在当前目录，或相对于当前目录的子目录中
  - deps: 列表或字符串，该对象所依赖的其它 targets。deps 的允许的格式：
    - `//path/to/dir/:name` 某目录下的 target，path 为从 BLADE_ROOT 出发的路径，name 为被依赖的目标名。
    - `:name` 当前目录下的 target， path 可以省略
    - `#pthread` 系统库。直接用#拼接名字即可
  - incs: 头文件路径列表，仅用于 thirdparty
  - defs: 宏定义列表，仅用于 thirdparty
  - warning: 警告设置，仅用于 thirdparty
  - optimize: 优化设置

## 4. Blade 命令行

语法格式：
```bash
blade [action] [options] [targets]
```
- action 是一个动作，包括：
  - build  表示构建项目
  - test   表示构建并且跑单元测试
  - clean  表示清除目标的构建结果
  - query  查询目标的依赖项与被依赖项
  - run    构建并 run 一个单一目标
  每个动作支持的 options 都不一样，具体的 options 可以参考 Blade 用户手册或者通过 blade [action]  --help 获取每个 option 的具体含义。
- targets 是一个列表，支持的格式：
  - path:name 表示 path 中的某个 target
  - path      表示 path 中所有 targets
  - path/...  表示 path 中所有 targets，并递归包括所有子目录
  - 默认表示当前目录

eg: 对于目标 common/base/string:string

构建这个目标的动态库：
```
blade build common/base/string:string –-generate-dynamic
```
构建 32 位的静态库 debug 版本：
```
blade build common/base/string:string –m32 –pdebug
```
构建 common/base/string 目录下的所有目标并且跑单元测试，产生覆盖率信息
```
blade test common/base/string… --gcov
```

## 5. Blade 集成测试

## 6. Refer Links

[GitHub typhoon-blade](https://github.com/chen3feng/typhoon-blade)