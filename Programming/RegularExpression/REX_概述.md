- [正则表达式：基本概念](#正则表达式基本概念)
  - [1. 基本概念](#1-基本概念)
  - [2. 标准分类](#2-标准分类)
    - [2.1. POSIX 正则表达式](#21-posix-正则表达式)
    - [2.2. PCRE 正则表达式](#22-pcre-正则表达式)
  - [3. Refer Links](#3-refer-links)

# 正则表达式：基本概念

## 1. 基本概念

正则表达式 (Regular expression) 使用单个字符串来描述、匹配一系列符合某个句法规则的字符串。

许多程序设计语言都支持利用正则表达式进行字符串操作。例如，在 Perl 中就内建了一个功能强大的正则表达式引擎。正则表达式这个概念最初是由 Unix 中的工具软件（例如 sed 和 grep）普及开的。

## 2. 标准分类

**正则表达式大体上可以分为 POSIX 和 PCRE 两类**。

### 2.1. POSIX 正则表达式

POSIX（Portable Operating System Interface）标准：由电气和电子工程师 协会（IEEE）制定的 POSIX Extended 1003.2 兼容正则，它由一系列规范构成，定义了 UNIX 操作系统应当支持的功能。

**在兼容 POSIX 的 Unix/Linux 系统中的工具（如 grep，awk，sed，vi 等）大多继承了 POSIX 规范，一些数据库系统中的正则表达式也符合 POSIX 规范**。

POSIX 标准的正则表达式定义了两个派别（flavor）：
- BRE（Basic Regular Expression，基本型正则表达式）
- ERE（Extended Regular Express，扩展型正则表达式）

ERE 在 BRE 的基础上加入了更多的元字符，支持更多的操作，从兼容上考虑，在仅支持 BRE 的工具中也可使用 ERE，但需要在 ERE 特有的元字符前加、进行转义。因此，BRE 和 ERE 的区别主要体现在部分元字符是否需要添加转义符的问题上。

### 2.2. PCRE 正则表达式

PCRE（Perl Compatible Regular Expression）标准：随着 Perl 语言的发展，Perl 语言中内建的正则表达式功能越来越强悍，为了把 Perl 语言中正则的功能移植到其他语言中，PCRE 就诞生了。

**现在的编程语言中的正则表达式，大部分都属于 PCRE 这个分支。**

## 3. Refer Links

[Wikipedia 正则表达式](https://zh.wikipedia.org/wiki/%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)

[正则表达式“派别”简述](https://liujiacai.net/blog/2014/12/07/regexp-favors/)

[解析 posix 与 perl 标准的正则表达式区别](http://blog.sae.sina.com.cn/archives/617)

[Linux/Unix 工具与正则表达式的 POSIX 规范](http://www.infoq.com/cn/news/2011/07/regular-expressions-6-POSIX)