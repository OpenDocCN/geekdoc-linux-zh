# Ruby 正则表达式：匹配正则表达式中的任何字符

> 原文：[`techbyexample.com/ruby-regex-match-any-character/`](https://techbyexample.com/ruby-regex-match-any-character/)

# **概述**

点号‘.’是正则表达式中最常用的元字符之一。它用于匹配任何字符。默认情况下，它不匹配换行符。

现在让我们看看一个简单的程序，展示点号‘.’字符。

# **程序**

```go
match = "a".match(/./)
puts "For a: " + match.to_s

match = "b".match(/./)
puts "For b: " +  match.to_s

match = "ab".match(/./)
puts "For ab: " +  match.to_s

match = "".match(/./)
puts "For Empty String: " +  match.to_s
```

**输出**

```go
For a: a
For b: b
For ab: a
For Empty String:
```

在上面的程序中，我们有一个简单的正则表达式，只包含一个点号字符。

```go
/./
```

它匹配以下字符和字符串。

```go
a
b
ab
```

它匹配**ab**，因为默认情况下，正则表达式不会匹配整个字符串，除非使用锚字符（插入符号和美元符号）。这就是为什么它匹配字符串‘ab’中的第一个字符‘a’并报告匹配。它不会匹配空字符串。

让我们看一个例子，其中正则表达式中有两个点号。

```go
match = "ab".match(/../)
puts "For ab: " + match.to_s

match = "ba".match(/../)
puts "For ba: " + match.to_s

match = "abc".match(/../)
puts "For abc: " + match.to_s

match = "a".match(/../)
puts "For a: " + match.to_s
```

**输出**

```go
For ab: ab
For ba: ba
For abc: ab
For a:
```

在上面的程序中，我们有一个简单的正则表达式，包含两个点号。

```go
/../
```

它将匹配任何包含至少两个字符的子字符串。

这就是为什么它匹配

```go
ab
ba
abc
```

并且不会给出匹配（它会匹配一个空字符串）。

```go
a
```
