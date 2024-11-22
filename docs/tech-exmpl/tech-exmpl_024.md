# 在 Ruby 语言中删除哈希中的嵌套键

> 原文：[`techbyexample.com/ruby-hash-nested-delete/`](https://techbyexample.com/ruby-hash-nested-delete/)

# **概述**

在 Ruby 语言中，可以删除哈希中的嵌套键。

让我们来看一些例子

# **一级嵌套键**

这是删除哈希中第一层键的代码

```go
sample = {
    "a" => "b",
    "c" => "d"
}
sample.delete("a")
puts sample
```

**输出**

```go
{"c"=>"d"}
```

# **二级嵌套键**

这是删除哈希中第一层键的代码

```go
sample = {
    "a" =>  {
        "b" => "c"
    }

}
sample["a"].delete("b")
puts sample
```

**输出**

```go
{"a"=>{}}
```

# **三级嵌套键**

这是删除哈希中第一层键的代码

```go
sample = {
    "a" =>  {
        "b" => {
            "c" => "d"
        }
    }

}
sample["a"]["b"].delete("c")
puts sample
```

**输出**

```go
{"a"=>{"b"=>{}}}
```

**注意：** 请查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
