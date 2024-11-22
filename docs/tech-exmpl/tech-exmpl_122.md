# 在 Ruby 语言中打印/输出斜体文本

> 原文：[`techbyexample.com/print-italic-ruby/`](https://techbyexample.com/print-italic-ruby/)

# **概述**

[ANSI 转义码](http://en.wikipedia.org/wiki/ANSI_escape_code) 可用于在控制台中输出粗体文本。请注意

+   在 MAC/Linux 系统终端支持 ANSI 转义码

+   Windows 命令提示符不支持它。在 Windows 上，你可以安装 Cygwin，ANSI 转义码在该环境下可正常工作。

# **程序**

```go
class String
    def italic
        "\e3m#{self}\e[23m"
     end
end

puts "Italic String".italic
```

**输出**

![
