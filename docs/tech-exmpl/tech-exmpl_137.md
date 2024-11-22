# 在 Ruby 语言中从 URL 获取完整的主机名和端口

> 原文：[`techbyexample.com/hostname-port-uri-ruby/`](https://techbyexample.com/hostname-port-uri-ruby/)

# **概述**

Ruby 的 URI 模块可用于从 URL 获取所有部分。**这个**模块可以用于解析 URL 并提取所有部分。[`ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html`](https://ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html)

一旦给定的 URL 被正确解析，它将返回 URI 对象。我们随后可以从 URI 中访问以下信息：

+   协议

+   用户信息

+   主机名

+   端口

+   路径名

+   查询参数

+   片段

一旦我们拥有所有部分，就可以将它们连接起来，以获得完整的主机名和端口。

## **程序**

以下是相同程序的代码：

```go
require 'uri'

uri = "https://test:abcd123@techbyexample.com:8000/tutorials/intro?type=advance&compact=false#history"
pasrse_uri = URI(uri)

hostname = pasrse_uri.scheme + "://" + pasrse_uri.hostname + ":" + pasrse_uri.port.to_s

puts hostname
```

**输出**

```go
https://techbyexample.com:8000
```

+   [ruby](https://techbyexample.com/tag/ruby/)
