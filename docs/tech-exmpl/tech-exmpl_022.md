# 修复 Ruby 2.7.0 中的 LoadError：无法加载文件 — scanf

> 原文：[`techbyexample.com/scanf-load-error-ruby/`](https://techbyexample.com/scanf-load-error-ruby/)

# **概述**

**scanf** gem 不再是 Ruby 安装时随附的捆绑 gem。它必须单独安装。只需将以下行添加到你的 Gemfile 中，然后运行‘bundle’

```go
gem 'scanf'
```

或者运行

```go
gem install scanf
```

**注意：** 请查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
