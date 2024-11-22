# 检查文件是否在 Ruby 语言中存在

> 原文：[`techbyexample.com/file-exists-ruby/`](https://techbyexample.com/file-exists-ruby/)

# **概述**

Ruby 中的 File 类提供了一种方法，如果给定文件存在，则返回 true；如果给定文件不存在，则返回 false。

[`ruby-doc.org/core-3.0.0/File.html#method-c-exist-3F`](https://ruby-doc.org/core-3.0.0/File.html#method-c-exist-3F)

这是该方法的签名

```go
exist?(file_name) → true or false
```

# **示例**

以下是相应的程序。

```go
File.open("temp.txt", "w") do |f|     
    f.write("Test")   
  end

puts File.exist?('temp.txt')
```

**输出**

```go
true
```

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
