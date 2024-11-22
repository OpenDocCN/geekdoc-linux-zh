# 如何在 Linux 中跳转到文件的最后一行

> 原文：[`techbyexample.com/last-line-using-vim-linux/`](https://techbyexample.com/last-line-using-vim-linux/)

## **概述**

当处理非常大的文件时，例如查看日志文件，用户可能希望跳转到文件的最后一行以查看最新的条目。可以使用以下命令跳转到用 `vi` 或 `vim` 命令打开的文件的最后一行：

## **示例**

+   创建一个包含一些内容的临时文件：

```go
# touch temp.txt
# echo $'this is line 1\nthis is line 2\nthis is line 3\nthis is line 4\nThis is last line' > temp.txt
```

+   使用 `cat` 命令查看临时文件的内容：

```go
# cat temp.txt
this is line 1
this is line 2
this is line 3
this is line 4
This is last line
```

+   用 `vi` 或 `vim` 打开临时文件，然后输入 “:$” 跳转到文件的最后一行。注意，光标已经跳到最后一行：

```go
this is line 1
this is line 2
this is line 3
this is line 4
This is last line
~
~
~
~
:$
```
