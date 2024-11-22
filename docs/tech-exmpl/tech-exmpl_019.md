# 在终端中一起搜索两个词

> 原文：[`techbyexample.com/grep-two-words-together-terminal/`](https://techbyexample.com/grep-two-words-together-terminal/)

目录

+   概览

+   方法 1

+   方法 2

+   方法 3

# **概览**

这里有一些示例来实现这一操作

首先，让我们创建一个 **sample.txt** 文件

**sample.txt**

```go
This file
contains word1 and
also word2
```

这里有一些使用 grep 搜索两个词的方法

# **方法 1**

+   使用 **\|** 作为需要搜索的两个词之间的分隔符

**示例**

```go
some_user@macOS grep 'word1\|word2' ./sample.txt
contains word1 and
also word2
```

# **方法 2**

+   使用 **-e** 为每个需要搜索的词

**示例**

```go
some_user@macOS grep -e word1 -e word2 ./sample.txt
contains word1 and
also word2
```

# **方法 3**

+   使用 **-E** 和 **|**

**示例**

```go
some_user@macOS grep -E 'word1|word2' ./sample.txt
contains word1 and
also word2
```

**注意：** 请查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
