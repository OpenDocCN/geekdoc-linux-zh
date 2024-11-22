# 在 Ruby 语言中解析 URL 并提取所有部分

> 原文：[`techbyexample.com/parse-url-ruby/`](https://techbyexample.com/parse-url-ruby/)

# **概述**

**URI**模块可以用来解析 URL 并提取所有部分 [`ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html`](https://ruby-doc.org/stdlib-2.5.1/libdoc/uri/rdoc/URI.html)

一旦给定的 URL 正确解析，它将返回 URI 对象。然后我们可以从 URI 中访问以下信息

+   协议

+   用户信息

+   主机名

+   端口

+   路径名

+   查询参数

+   片段

让我们看看一个相同功能的程序：

我们将解析以下 URL

```go
https://test:abcd123@techbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

然后

+   协议是 HTTPS

+   用户信息 – 用户名是 test，密码是 abcd123\。用户名和密码由冒号:分隔

+   主机名是 [www.techbyexample.com](http://www.golangbyexample.com/)

+   端口是 8000

+   路径是 tutorials/intro

+   查询参数是**type=advance**和**compact=false**。它们由&符号分隔

+   片段是**history**。它会直接跳转到页面中的历史部分。**history**是指向该页面内特定部分的标识符

# **程序**

```go
require 'uri'

uri = "https://test:abcd123@techbyexample.com:8000/tutorials/intro?type=advance&compact=false#history"

pasrse_uri = URI(uri)

puts(pasrse_uri.scheme)  
puts(pasrse_uri.userinfo)  
puts(pasrse_uri.host)
puts(pasrse_uri.port)
puts(pasrse_uri.path)
puts(pasrse_uri.query)
puts(pasrse_uri.fragment)
puts(pasrse_uri.to_s)
```

**输出**

```go
https
test:abcd123
techbyexample.com
8000
/tutorials/intro
type=advance&compact=false
history
https://test:abcd123@techbyexample.com:8000/tutorials/intro?type=advance&compact=false#history
```

它正确地输出了所有信息，正如从输出中看到的那样

+   [ruby](https://techbyexample.com/tag/ruby/)*   [url](https://techbyexample.com/tag/url/)
