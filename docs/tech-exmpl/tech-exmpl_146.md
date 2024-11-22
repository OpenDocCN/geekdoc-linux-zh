# 如何使用 vi 或 vim（Linux）设置文件行号

> 原文：[`techbyexample.com/set-line-number-vi-vim/`](https://techbyexample.com/set-line-number-vi-vim/)

## **概述**

通常，使用**vi**或**vim**打开 Linux 文件时，不会显示行号。可以使用以下命令在**vi**或**vim**编辑器中显示行号：

```go
:set nu + Enter key
```

## **示例**

以下是设置文件行号的示例：

+   创建一个包含一些内容的临时文件：

```go
~ $ touch temp.txt
~ $ echo $'this is line 1\nthis is line 2\nthis is line 3\nthis is line 4' > temp.txt
```

+   使用**cat**命令检查临时文件的内容：

```go
~ $ cat temp.txt
this is line 1
this is line 2
this is line 3
this is line 4
```

+   现在使用**vi**或**vim**打开临时文件，然后输入**“:set nu”**，按下**Enter**键。这将在打开的编辑器中显示行号，如下所示：

```go
1 this is line 1
2 this is line 2
3 this is line 3
4 this is line 4
~
~
~
~
:set nu
```

用户可以使用上述方法跳转到所需的行号。操作完成后，可以使用**“:q!”**（退出文件而不保存更改）或**“:wq”**（保存更改后退出文件）来关闭文件。
