# zsh: 找不到匹配项：HEAD^

> 原文：[`techbyexample.com/zsh-no-matches-found-head/`](https://techbyexample.com/zsh-no-matches-found-head/)

## **概述**

^ 字符是 zsh 中文件名扩展的特殊字符。有几种方法可以解决这个问题

# **解决方法 1**

转义 ^ 字符

示例

```go
git reset HEAD\^ --soft
```

或使用引号

```go
git reset 'HEAD^' --soft
```

# **解决方法 2（永久修复）**

这是由于**EXTENDED_GLOB**选项，zsh 允许 ^ 否定文件模式。正如这里引用的那样

[`zsh.sourceforge.io/Doc/Release/Options.html`](https://zsh.sourceforge.io/Doc/Release/Options.html)

**EXTENDED_GLOB**

将‘#’，‘~’和‘^’字符视为文件名生成等模式的一部分。（一个未加引号的初始‘~’总是会生成命名目录扩展。）

所以只需在终端中**取消设置**此选项

```go
unsetopt EXTENDED_GLOB
git reset HEAD\^ --soft
```

要永久修复，可以在 .zshrc 文件中写下这一行。这告诉 .zsh 在模式匹配失败时不要打印错误，并按原样使用命令

```go
unsetopt NOMATCH
```

PS：在 .zshrc 中**取消设置 EXTENDED_GLOB** 不是一个好主意，否则你将无法使用该行为。当关闭 NOMATCH 时，它在模式匹配失败时并不会简单地打印错误。

NOMATCH 的描述可以在这里找到

[`zsh.sourceforge.io/Doc/Release/Options.html`](https://zsh.sourceforge.io/Doc/Release/Options.html)

**NOMATCH (+3) <C> <Z>**

如果文件名生成的模式没有匹配项，则打印错误，而不是让其在参数列表中保持不变。这也适用于初始‘~’或‘=’的文件扩展。

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
