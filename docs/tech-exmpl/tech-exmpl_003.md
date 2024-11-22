# Ruby 将字符串转换为布尔值

> 原文：[`techbyexample.com/ruby-string-bool/`](https://techbyexample.com/ruby-string-bool/)

# **概述**

在 Ruby 语言中，当字符串“true”和“false”在**if**条件中使用时，它们会被解释为 true。见下面的示例。

```go
if "false"
   puts "yes"
end
```

**输出**

```go
"yes"
```

“false” 被评估为 true。

因此，正确处理字符串“false”或字符串“true”变得非常重要。

我们可以创建一个自定义方法，根据字符串的内容返回布尔值 true 或 false。

```go
def true?(str)
  str.to_s.downcase == "true"
end
```

我们可以尝试一下上述函数。

```go
true?("false")
 => false
```

```go
true?("true")
 => true
```

另外，请注意，对于除“false”以外的字符串，返回的值是 true，但该函数可以轻松修改以处理这种情况。

**注意：** 查看我们的系统设计教程系列 [System Design Questions](https://techbyexample.com/system-design-questions/)
