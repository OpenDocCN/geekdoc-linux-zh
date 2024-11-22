# 检查文件在 Ruby 语言中是否可写

> 原文：[`techbyexample.com/file-writable-ruby/`](https://techbyexample.com/file-writable-ruby/)

# **概述**

Ruby 的 File 类提供了以下方法，可用于检查当前进程的有效用户和组 ID 是否具有文件的写权限。

这是该函数的签名

```go
writable?(file_name)
```

如果文件可写，它返回 true，否则返回 false。

# **程序**

在当前目录创建一个名为**test.txt**的文件。使用相同的用户运行以下程序

```go
writable = File.writable?("test.txt")
puts writable
```

**输出**

```go
true
```

现在使用**chown**命令更改**test.txt**文件的用户和组 ID。确保当前用户不属于新的组 ID。

再次运行上述程序。这次它将输出 false。
