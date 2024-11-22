# 修复 Ruby 2.7.0 中的 LoadError 无法加载文件 e2mmap

> 原文：[`techbyexample.com/e2mmap-load-error-ruby/`](https://techbyexample.com/e2mmap-load-error-ruby/)

# **概述**

e2mmap gem 不再是随 Ruby 安装捆绑的 gem，它需要单独安装。只需将下面的代码添加到你的 Gemfile 中，然后运行‘bundle’。

```go
gem 'e2mmap'
```

或者运行

```go
gem install e2mmap
```

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
