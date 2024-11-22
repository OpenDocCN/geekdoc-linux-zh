# 在 Ruby 语言中加载 .env 或环境文件

> 原文：[`techbyexample.com/env-file-load-ruby/`](https://techbyexample.com/env-file-load-ruby/)

# **概述**

**dotenv** gem 可以用来在 Ruby 中加载 **.env** 或 **环境** 文件

[`github.com/bkeepers/dotenv`](https://github.com/bkeepers/dotenv)

它接受可变数量的参数，每个参数可以是它需要加载的 **.env** 文件名

# **程序**

创建一个 **local.env** 文件，内容如下：

```go
STACK=DEV
DATABASE=SQL
```

这是程序示例

```go
require 'dotenv'

Dotenv.load("local.env")
puts ENV["STACK"]
puts ENV["DATABASE"]
```

**输出**

```go
DEV
SQL
```

它加载 **local.env** 文件并给出正确的输出

它还可以用来加载多个 .env 文件。创建一个新的 **test.env** 文件，内容如下：

```go
TEST=UNIT
```

```go
require 'dotenv'

Dotenv.load("local.env", "test.env")
puts ENV["STACK"]
puts ENV["DATABASE"]
puts ENV["TEST"]
```

**输出**

```go
DEV
SQL
UNIT
```

这段代码是关于在 Ruby 中加载 **.env** 文件的。
