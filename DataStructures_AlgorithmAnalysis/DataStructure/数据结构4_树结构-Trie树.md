- [树结构 - Trie 树](#trie)
  - [1. 基本概念](#1)
  - [2. 基本性质](#2)
  - [3. 优缺点](#3)
    - [3.1. 优点](#31)
    - [3.2. 缺点](#32)
  - [4. 基本操作](#4)
    - [4.1. 节点定义](#41)
    - [4.2. 插入节点](#42)
    - [4.3. 删除节点](#43)
  - [5. 应用](#5)
  - [6. Refer Links](#6-refer-links)

# 树结构 - Trie 树

## 1. 基本概念

> Trie(retrieval)，又称前缀树或字典树，是一种有序树，用于保存关联数组，其中的键通常是字符串。与二叉查找树不同，键不是直接保存在节点中，而是由节点在树中的位置决定。一个节点的所有子孙都有相同的前缀，也就是这个节点对应的字符串，而根节点对应空字符串。一般情况下，不是所有的节点都有对应的值，只有叶子节点和部分内部节点所对应的键才有相关的值。
> 
> Trie 中的键通常是字符串，但也可以是其它的结构。trie 的算法可以很容易地修改为处理其它结构的有序序列，比如一串数字或者形状的排列。比如，bitwise trie 中的键是一串位元，可以用于表示整数或者内存地址。

## 2. 基本性质

Tire 树的三个基本性质：
- 根节点不包含字符，除根节点外每一个节点都只包含一个字符。
- 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。
- 每个节点的所有子节点包含的字符都不相同。

通常在实现的时候，会在节点结构中设置一个标志，用来标记该结点处是否构成一个单词（关键字）。

例：以下 Trie 树表示了关键字集合{“a”, “to”, “tea”, “ted”, “ten”, “i”, “in”, “inn”}

![image](http://img.cdn.firejq.com/jpg/2018/5/5/df303a9210a1b5533a15794ceb1c523a.jpg)

## 3. 优缺点

**Trie 树的核心思想是空间换时间，利用字符串的公共前缀来减少无谓的字符串比较以达到提高查询效率的目的**。

### 3.1. 优点

- 利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较，因此插入和查询的效率很高，时间效率都为 O(m)，其中 m 是待插入 / 查询的字符串的长度。
  - 关于查询，会有人说 hash 表时间复杂度是 O(1) 不是更快？确实，但**哈希搜索的效率通常取决于 hash 函数的好坏，若一个坏的 hash 函数导致很多的冲突，效率并不一定比 Trie 树高**。
- Trie 树中不同的关键字不会产生冲突。

- Trie 树只有在允许一个关键字关联多个值的情况下才有类似 hash 碰撞发生。

- Trie 树不用求 hash 值，对短字符串有更快的速度。通常，求 hash 值也是需要遍历字符串的。

- Trie 树可以对关键字按字典序排序。

### 3.2. 缺点

- 当 hash 函数很好时，Trie 树的查找效率会低于哈希搜索。

- 空间消耗比较大。

## 4. 基本操作

### 4.1. 节点定义

为了方便，可以采用纯英文字母，由于字母有 26 个，那么构建的 trie 树就是一个 26 叉树，每个节点包含 26 个子节点。

```java
public class TrieNode {
    /// 26 个字符，也就是 26 叉树
    public TrieNode[] childNodes;

    /// 词频统计
    public int freq;

    /// 记录该节点的字符
    public char nodeChar;

    /// 插入记录时的编码 id
    public HashSet<int> hashSet = new HashSet<int>();

    /// 初始化
    public TrieNode() {
        childNodes = new TrieNode[26];
        freq = 0;
    }
}
```

### 4.2. 插入节点

由于是 26 叉树，插入节点时就需要选择当前节点的后续子节点是放在当前节点的哪一叉中，也就是放在 childNodes 中哪一个位置，可以采用 `int k = word[0] - 'a'` 来计算位置。
```java
public void AddTrieNode(ref TrieNode root, string word, int id) {
    if (word.Length == 0)
        return;

    // 求字符地址，方便将该字符放入到 26 叉树中的哪一叉中
    int k = word[0] - 'a';

    // 如果该叉树为空，则初始化
    if (root.childNodes[k] == null) {
        root.childNodes[k] = new TrieNode();
        // 记录下字符
        root.childNodes[k].nodeChar = word[0];
    }

    // 该 id 途径的节点
    root.childNodes[k].hashSet.Add(id);

    String nextWord = word.Substring(1);

    // 说明是最后一个字符，统计该词出现的次数
    if (nextWord.Length == 0)
        root.childNodes[k].freq++;

    AddTrieNode(ref root.childNodes[k], nextWord, id);
}
```

### 4.3. 删除节点

删除操作中，我们不仅要删除该节点的字符串编号，还要对词频减一操作。

```java
public void DeleteTrieNode(ref TrieNode root, string word, int id) {
    if (word.Length == 0)
        return;

    // 求字符地址，方便将该字符放入到 26 叉树种的哪一颗树中
    int k = word[0] - 'a';

    // 如果该叉树为空，则说明没有找到要删除的点
    if (root.childNodes[k] == null)
        return;

    String nextWord = word.Substring(1);

    // 如果是最后一个单词，则减去词频
    if (word.Length == 0 && root.childNodes[k].freq > 0)
        root.childNodes[k].freq--;

    // 删除途经节点
    root.childNodes[k].hashSet.Remove(id);

    DeleteTrieNode(ref root.childNodes[k], nextWord, id);
}
```

## 5. 应用

**Trie 树的典型应用是用于统计、排序和保存大量的字符串（但不仅限于字符串），经常被搜索引擎系统用于文本词频统计**。

- **字符串检索**

  检索 / 查询功能是 Trie 树最原始的功能。

  思路：从根节点开始一个一个字符进行比较：
  1. 如果沿路比较，发现不同的字符，则表示该字符串在集合中不存在。
  1. 如果所有的字符全部比较完并且全部相同，还需判断最后一个节点的标志位（标记该节点是否代表一个关键字）。

  例：**给出 N 个单词组成的熟词表，以及一篇全用小写英文书写的文章，请你按最早出现的顺序写出所有不在熟词表中的生词**。

  在这道题中，我们可以用数组枚举、哈希等方法，**若用字典树，只需先把熟词建一棵树，然后读入文章进行比较，这种方法效率是比较高的**。

- **词频统计**

  Trie 树常被搜索引擎系统用于文本词频统计。

  思路：为了实现词频统计，我们修改了节点结构，用一个整型变量 count 来计数。对每一个关键字执行插入操作，若已存在，计数加 1，若不存在，插入后 count 置 1。

- **字符串排序**

  Trie 树可以对大量字符串按字典序进行排序，思路也很简单：遍历一次所有关键字，将它们全部插入 trie 树，树的每个结点的所有儿子很显然地按照字母表排序，然后先序遍历输出 Trie 树中所有关键字即可。

- **前缀匹配**

  例如：找出一个字符串集合中所有以 ab 开头的字符串。我们只需要用所有字符串构造一个 trie 树，然后输出以 a->b->开头的路径上的关键字即可。

  Trie 树前缀匹配常用于搜索提示。如当输入一个网址，可以自动搜索出可能的选择。当没有完全匹配的搜索结果，可以返回前缀最相似的可能。

- **最长公共前缀**

  对所有串建立字典树，对于两个串的最长公共前缀的长度即他们所在的结点的公共祖先个数，于是，问题就转化为求公共祖先的问题。

## 6. Refer Links

[coursera: 张铭数据结构 -Trie 树](https://www.coursera.org/learn/gaoji-shuju-jiegou/lecture/1s2Nc/trie-shu)

[Trie 树（Prefix Tree）介绍](https://blog.csdn.net/lisonglisonglisong/article/details/45584721)

[6 天通吃树结构—— 第五天 Trie 树](http://www.cnblogs.com/huangxincheng/archive/2012/11/25/2788268.html)

[从 Trie 树（字典树）谈到后缀树](https://blog.csdn.net/v_july_v/article/details/6897097)

[为什么 360 面试官说 Trie 树没用？](https://www.zhihu.com/question/27168319)