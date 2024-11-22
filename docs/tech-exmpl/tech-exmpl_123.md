# 在 Ruby 语言中打印/输出粗体文本

> 原文：[`techbyexample.com/print-text-bold-ruby/`](https://techbyexample.com/print-text-bold-ruby/)

# **概述**

[ANSI 转义码](http://en.wikipedia.org/wiki/ANSI_escape_code) 可以用于在控制台输出粗体文本。请注意

+   MAC/Linux 系统的终端支持 ANSI 转义码

+   Windows 命令提示符不支持 ANSI 转义码。在 Windows 上，你可以安装 Cygwin，ANSI 转义码在该环境下可以正常工作。

# **程序**

```go
class String
    def bold
        "\e1m#{self}\e[22m" 
     end
end

puts "Bold String".bold
```

**输出**

![
