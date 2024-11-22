# Ruby 正则表达式或变量中的正则表达式

> 原文：[`techbyexample.com/ruby-regex-variable/`](https://techbyexample.com/ruby-regex-variable/)

# **概述**

在 Ruby 中，正则表达式是通过两个斜杠定义的。这是一种区分正则表达式与普通字符串的方式。正则表达式将以这种方式表示

```go
/#{regex}/
```

因此，也可以将正则表达式赋值给一个变量。下面是相应的示例程序。

# **程序**

```go
regex = "b+"

input = "bb"
match = input.match(/#{regex}/)
puts match

input = "bbb"
match = input.match(/#{regex}/)
puts match

input = "a"
match = input.match(/#{regex}/)
puts match
```

**输出**

```go
bb
bbb 
```

注意我们是如何将正则表达式赋值给一个变量的，像这样

```go
regex = "b+"
```

然后像这样在匹配中使用它

```go
match = input.match(/#{regex}/)
```

除了正则表达式，你还可以使用其他变量或任何类型的字符串

例如

```go
regex = "b+"
input = "abb"
match = input.match(/a#{regex}/)
puts match
```

注意这里的正则表达式，我们在前面加上了字符**“a”**

```go
/a#{regex}/
```

现在运行上面的程序。它会给出一个匹配结果

```go
abb
```

+   [正则表达式](https://techbyexample.com/tag/regex/)*   [Ruby](https://techbyexample.com/tag/ruby/)
