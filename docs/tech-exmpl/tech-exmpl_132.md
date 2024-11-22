# 在 Ruby 语言中解析文件或字符串的 JSON

> 原文：[`techbyexample.com/json-parse-file-string-ruby/`](https://techbyexample.com/json-parse-file-string-ruby/)

# **概述**

ruby 的**json**模块可以用来解析 ruby 语言中的文件或字符串

[`ruby-doc.org/stdlib-2.6.3/libdoc/json/rdoc/JSON.html`](https://ruby-doc.org/stdlib-2.6.3/libdoc/json/rdoc/JSON.html)

它提供了一个解析函数，可以用来解析文件。

# **解析字符串**

以下是相应的程序。

```go
require 'json'
parsed = JSON.parse('{"a":"x","b":"y"}')
puts parsed
puts parsed["a"]
puts parsed["b"]
```

**输出**

```go
{"a"=>"x", "b"=>"y"}
x
y
```

# **解析文件**

创建一个名为**temp.json**的文件，内容如下：

```go
{"a":"x","b":"y"}
```

现在运行下面的程序

```go
require 'json'
file = File.read('temp.json')
parsed = JSON.parse(file)

puts parsed
puts parsed["a"]
puts parsed["b"]
```

**输出**

```go
{"a"=>"x", "b"=>"y"}
x
y
```

+   [ruby](https://techbyexample.com/tag/ruby/)
