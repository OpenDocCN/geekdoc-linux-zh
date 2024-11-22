# 在 Ruby 语言中同时遍历三个数组

> 原文：[`techbyexample.com/traverse-three-arrays-ruby/`](https://techbyexample.com/traverse-three-arrays-ruby/)

# **概述**

在 Ruby 中，可以同时遍历三个数组。可以使用 `.zip` 函数来实现。以下是 `zip` 函数的文档链接

[`ruby-doc.org/core-2.0.0/Array.html#method-i-zip`](https://ruby-doc.org/core-2.0.0/Array.html#method-i-zip)

让我们来看一下相应的程序

# **程序**

下面是当三个数组的大小相同时，遍历三个数组的代码

```go
a = ["d", "e", "f"]
b = ["g", "h", "i"]

["a", "b", "c"].zip(a, b) do |x,y,z| 
   puts x+ " " + y + " " + z 
end
```

**输出**

```go
a d g
b e h
c f i
```

如果数组的大小不相同，则会为较小数组中的元素打印空白空间

```go
a = ["d", "e"]
b = ["g"]

["a", "b", "c"].zip(a, b) do |x,y,z| 
   puts x
   puts y
   puts z
end
```

**输出**

```go
a
d
g
b
e

c 
```

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
