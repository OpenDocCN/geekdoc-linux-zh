# 从 Ruby 语言中的字符串中提取一个 URL

> 原文：[`techbyexample.com/extract-url-ruby/`](https://techbyexample.com/extract-url-ruby/)

# **概述**

**URI** 模块可以用来从给定的字符串中提取 URL

[`ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html`](https://ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html)

我们可以使用 URI 模块的 Extract 方法

[`ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html#method-c-extract`](https://ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html#method-c-extract)

以下是该方法的签名

```go
URI::extract(str[, schemes][,&blk])
```

参数

+   str – 这是用于从中提取多个 URL 的输入字符串

+   schemes – 用于限制提取的 URL 范围

# **程序**

首先让我们看看一个提取单个 URL 的程序

```go
require 'uri'

input = "The webiste is https://techbyexample.com:8000/tutorials/intro"
urls = URI.extract(input)

puts(urls)
```

**输出**

```go
https://techbyexample.com:8000/tutorials/intro
```

让我们来看另一个提取多个 URL 的程序

```go
require 'uri'

input = "The webiste is https://techbyexample.com:8000/tutorials/intro amd mail to mailto:contactus@techbyexample.com"
urls = URI.extract(input)

puts(urls)
```

**输出**

```go
https://techbyexample.com:8000/tutorials/intro
mailto:contactus@techbyexample.com
```

如果我们想要限制方案，也可以做到。

```go
require 'uri'

input = "The webiste is https://techbyexample.com:8000/tutorials/intro amd mail to mailto:contactus@techbyexample.com"
urls = URI.extract(input, "https")

puts(urls)
```

**输出**

```go
https://techbyexample.com:8000/tutorials/intro
```

在上面的程序中，我们提供的方案是 https，这就是为什么我们只得到一个输出的原因

+   [提取](https://techbyexample.com/tag/extract/)*   [ruby](https://techbyexample.com/tag/ruby/)*   [url](https://techbyexample.com/tag/url/)
