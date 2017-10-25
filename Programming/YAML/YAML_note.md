- [YAML Note](#yaml-note)
  - [基本规则](#%E5%9F%BA%E6%9C%AC%E8%A7%84%E5%88%99)
- [5)	# 表示注释，从这个字符一直到行尾，都会被解析器忽略；](#5-%E8%A1%A8%E7%A4%BA%E6%B3%A8%E9%87%8A%EF%BC%8C%E4%BB%8E%E8%BF%99%E4%B8%AA%E5%AD%97%E7%AC%A6%E4%B8%80%E7%9B%B4%E5%88%B0%E8%A1%8C%E5%B0%BE%EF%BC%8C%E9%83%BD%E4%BC%9A%E8%A2%AB%E8%A7%A3%E6%9E%90%E5%99%A8%E5%BF%BD%E7%95%A5%EF%BC%9B)
  - [数据结构](#%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
    - [对象](#%E5%AF%B9%E8%B1%A1)
    - [数组](#%E6%95%B0%E7%BB%84)
    - [纯量](#%E7%BA%AF%E9%87%8F)
  - [字符串](#%E5%AD%97%E7%AC%A6%E4%B8%B2)
  - [引用](#%E5%BC%95%E7%94%A8)
  - [Refer Links](#refer-links)

# YAML Note

YAML 是专门用来写配置文件的语言，非常简洁和强大，远比 JSON 格式方便。

## 基本规则

1)	大小写敏感；

2)	使用缩进表示层级关系；

3)	缩进时不允许使用 Tab 键，只允许使用空格；

4)	缩进的空格数目不重要，只要相同层级的元素左侧对齐即可；

5)	# 表示注释，从这个字符一直到行尾，都会被解析器忽略；

## 数据结构

YAML 支持的数据结构有三种：
- 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
- 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
- 纯量（scalars）：单个的、不可再分的值

### 对象

对象的一组键值对，使用冒号结构表示，**注意冒号后一定要跟一个空格**：
```yaml
animal: pets
```
Yaml 也允许另一种写法，将所有键值对写成一个行内对象：
```yaml
hash: { name: Steve, foo: bar }
```

### 数组

一组连词线开头的行，构成一个数组：
```yaml
- Cat
- Dog
- Goldfish
```
数据结构的子成员是一个数组，则可以在该项下面缩进一个空格：
```yaml
-
 - Cat
 - Dog
 - Goldfish
```
数组也可以采用行内表示法：
```yaml
animal: [Cat, Dog]
```
对象和数组可以结合使用，形成复合结构：
```yaml
languages:
 - Ruby
 - Perl
 - Python 
websites:
 YAML: yaml.org 
 Ruby: ruby-lang.org 
 Python: python.org 
 Perl: use.perl.org
```

### 纯量

纯量是最基本的、不可再分的值

数值直接以字面量的形式表示：
```yaml
number: 12.30
```
null 用~表示：
```yaml
parent: ~
```
时间采用 ISO8601 格式：
```yaml
iso8601: 2001-12-14t21:59:43.10-05:00
```
日期采用复合 iso8601 格式的年、月、日表示：
```yaml
date: 1976-07-31
```
YAML 允许使用两个感叹号，强制转换数据类型：
```yaml
e: !!str 123
f: !!str true
```

## 字符串
字符串是最常见，也是最复杂的一种数据类型。

字符串默认不使用引号表示：
```yaml
str: 这是一行字符串
```
如果字符串之中包含空格或特殊字符，需要放在引号之中：
```yaml
str: '内容： 字符串'
```
单引号和双引号都可以使用，双引号不会对特殊字符转义：
```yaml
s1: '内容、n 字符串'
s2: "内容、n 字符串"
```
单引号之中如果还有单引号，必须连续使用两个单引号转义：
```yaml
str: 'labor''s day'
# 相当于 js 代码：{ str: 'labor\'s day' }
```
字符串可以写成多行，从第二行开始，必须有一个单空格缩进。换行符会被转为空格：
```yaml
str: 这是一段
  多行
  字符串
```
多行字符串可以使用|保留换行符，也可以使用》折叠换行：
```yaml
this: |
  Foo
  Bar
that: >
  Foo
  Bar
```
+ 表示保留文字块末尾的换行，- 表示删除字符串末尾的换行：
```yaml
s1: |
  Foo

s2: |+
  Foo

s3: |-
  Foo
```

## 	引用
锚点 & 和别名*，可以用来引用：
```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
```
等效于
```yaml
defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhost

test:
  database: myapp_test
  adapter:  postgres
  host:     localhost
```

## Refer Links

http://www.ruanyifeng.com/blog/2016/07/yaml.html 
