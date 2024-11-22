# 在 Ruby 语言中获取或提取 URL 的查询参数

> 原文：[`techbyexample.com/extract-query-param-ruby/`](https://techbyexample.com/extract-query-param-ruby/)

# 概述

**URI** 模块可以用来解析 URL 并提取所有部分

[`ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html`](https://ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html)

一旦给定的 URL 被正确解析，它将返回 URI 对象。然后我们可以从该 URL 对象中访问查询参数

让我们来看一个实际的程序示例：

我们将解析下面的 URL

```go
https://test:abcd123@techbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

查询参数是 **type=advance** 和 **compact=false**，它们在上面的 URL 中通过 & 符号分隔

# **程序**

```go
require 'uri'

uri = "https://test:abcd123@techbyexample.com:8000/tutorials/intro?type=advance&compact=false#history"
pasrse_uri = URI(uri)

puts(pasrse_uri.query)
```

**输出**

```go
type=advance&compact=false
```

它正确地显示了所有查询参数，如输出所示
