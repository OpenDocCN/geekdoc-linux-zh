# 使用 Ruby 语言在 RubyMine 中将文件路径转换为终端中的超链接

> 原文：[`techbyexample.com/filepath-hyperlink-terminal-ruby/`](https://techbyexample.com/filepath-hyperlink-terminal-ruby/)

# **概述**

我们可以在文件路径前添加 **file://**，使其在 RubyMine 编辑器的终端中变为可点击链接

例如，下面是我们如何创建完整链接的示例

```go
puts "file://" + {filepath}
```

# **程序**

让我们来看一个相同功能的程序

在 **/files** 目录下创建一个名为 test.html 的文件，内容为 **以下** 所示

```go
<html></hmtl>
```

现在创建一个包含以下内容的 Ruby 文件

**main.rb**

```go
puts "file://" + "/files/test.html"
```

运行这个 Ruby 文件

```go
ruby main.rb
```

**输出**

```go
file:///files/test.html
```

这个路径在 RubyMine 中将是可点击的
