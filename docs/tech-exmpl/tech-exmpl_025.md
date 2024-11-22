# Ruby 正则匹配在 if 检查中的应用

> 原文：[`techbyexample.com/ruby-regex-match-if/`](https://techbyexample.com/ruby-regex-match-if/)

# **概述**

我们可以使用以下符号在 if 检查中进行 ruby 正则匹配

```go
=~
```

# **程序**

让我们来看一个相同的示例

```go
if /\d/ =~ "3" 
    puts "Match"
else 
    puts "No match"
end
```

**输出**

```go
Match
```

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
