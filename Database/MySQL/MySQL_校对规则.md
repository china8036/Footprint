- [MySQL 校对 / 排序规则](#mysql)
  - [1. 常见规则](#1)
  - [2. bin、ci](#2-binci)
  - [3. general、unicode](#3-generalunicode)
    - [3.1. utf_general_ci 和 utf_unicode_ci 的主要差别](#31-utf-general-ci--utf-unicode-ci)

# MySQL 校对 / 排序规则

## 1. 常见规则

MySQL 中常用的排序 / 校对规则有：

![image](http://img.cdn.firejq.com/jpg/2018/4/30/91d6c737d0da0fab467f08ac9ec30e9a.jpg)

即：
- utf8_general_ci
- utf8_unicode_ci
- utf8_bin

## 2. bin、ci

- ci 是 case insensitive, 即 "大小写不敏感", a 和 A 会在字符判断中会被当做一样的。
- bin 是二进制，a 和 A 会别区别对待。

例：
```sql
SELECT * FROM table WHERE txt = 'a'
```
那么在 utf8_bin 中你就找不到 txt = 'A' 的那一行，而 utf8_general_ci 则可以。

## 3. general、unicode

### 3.1. utf_general_ci 和 utf_unicode_ci 的主要差别

- utf8_unicode_ci 的最主要的特色是支持扩展，即当把一个字母看作与其它字母组合相等。

  - utf8_general_ci 是一个遗留的 校对规则，不支持扩展。它仅能够在字符之间进行逐个比较。
  - 具体主要表现在德语和法语的字符上，unicode 能完整的表示德语、法语，而 general 不能，因此 Unicode 准确性更高。
  - 但对于中文、英文而言，这两者在字符表示上几乎没有差别。

- general 由于支持的字符比 Unicode 少，因此 general 查询匹配速度要优于 unicode。

  例：对于德语字符ß，utf8_general_ci 中 ß = s ；而 unicode 中 ß = ss 。

- 总结

  对于中文而言，字符的表现几乎没有差别，而因为 general 更快，所以一般使用 utf8_general_ci；如果对德语和法语的对比有更高的要求，才使用 unicode，它比 general 更准确一些。
