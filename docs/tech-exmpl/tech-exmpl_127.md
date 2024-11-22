# 理解正则表达式中的花括号

> 原文：[`techbyexample.com/curly-braces-quantifier-regex/`](https://techbyexample.com/curly-braces-quantifier-regex/)

# **概述**

花括号在正则表达式中充当重复量词。它们指定了在输入字符串或文本中，前面字符可以出现的次数。它们还可以用来指定一个范围，即指定一个字符出现的最小和最大次数。

它的语法是

```go
{min, max}
```

在哪里

+   **min** 表示字符可以出现的最小次数

+   **max** 表示字符可以出现的最大次数

例如

```go
a{n}
```

这指定了字符“a”可以出现恰好**n**次。类似地，对于下面的正则表达式

```go
\d{n}
```

这指定了任何数字可以出现恰好**n**次。花括号还可以用来定义一个范围。

例如

+   **{m,n}** – 至少**m**次，最多**n**次

+   **{m,}** – 至少**m**次

+   **{, n}** – 最多**n**次

让我们在 Ruby 语言中看一个相同的例子

**main.ruby**

```go
regex = "b{2}"

input = "bb"
match = input.match(/#{regex}/)
puts match

input = "bbb"
match = input.match(/#{regex}/)
puts match
```

**输出**

```go
bb
bb
```

默认情况下，花括号是贪婪的或非懒惰的。这意味着什么？它们会匹配所有可能的字符，并总是更倾向于匹配更多的字符。也可以通过在花括号操作符后加上问号来使花括号操作符变得非贪婪或懒惰。让我们看一个相同的例子。

正如你从输出中看到的那样，在花括号操作符后加上问号操作符后，它会尽可能匹配最少数量的字符，也就是说，它变成了非贪婪模式。

这就是为什么给定正则表达式

```go
ab{2,4}
```

它会为所有以下输入字符串给出匹配结果**abb**

```go
abb
abbb
abbbb
```

对应的 Ruby 语言程序

**main.ruby**

```go
regex = "ab{2,4}"

input = "abb"
match = input.match(/#{regex}/)
puts match

input = "abbb"
match = input.match(/#{regex}/)
puts match

input = "abbbb"
match = input.match(/#{regex}/)
puts match
```

**输出**

```go
abb
abbb
abbbb
```

而

**ab{2,4}?** 将始终为所有上述输入字符串匹配**abb**

对应的程序

```go
regex = "ab{2,4}?"

input = "abb"
match = input.match(/#{regex}/)
puts match

input = "abbb"
match = input.match(/#{regex}/)
puts match

input = "abbbb"
match = input.match(/#{regex}/)
puts match
```

**输出**

```go
abb
abb
abb
```

**花括号应用于分组**

正则表达式的一部分可以放在一个平衡的圆括号内。这部分现在是一个分组。我们还可以对这个分组应用花括号。花括号会添加到分组之后。

让我们来看一个相同的例子。

```go
regex = "(ab){2}"

input = "ababb"
match = input.match(/#{regex}/)
puts match

input = "ababbc"
match = input.match(/#{regex}/)
puts match
```

**输出**

```go
abab
abab
```

# **花括号应用于字符类**

花括号量词也可以应用于整个字符类。它的含义仍然是一样的。字符类在正则表达式中用方括号表示。让我们看一下相应的程序。

在上面的程序中，我们有以下正则表达式

```go
[ab]{4}
```

它意味着它将匹配一个长度恰好为 4 且由字符**‘a’**和**‘b’**组成的字符串，顺序不限。

这就是为什么正则表达式匹配了以下字符串

```go
abab
aaaa
bbbb
aabb
bbaa
```

并且它不匹配

```go
aba - String of length 3
abbaa - String of length 5
```

# **如何在正则表达式中将花括号作为字面字符使用。**

如果需要将花括号作为字面字符使用，可以在开括号或闭括号前放置转义字符。

如果一个右花括号前面没有左花括号，那么它将被视为字面上的右花括号。

这就是正则表达式中关于花括号的所有内容。希望你喜欢这篇文章。如果有任何反馈，请在评论中分享。
