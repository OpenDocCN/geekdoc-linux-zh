# 如何在 Linux 上将命令的输出/错误重定向到文件？

> 原文：[`techbyexample.com/redirect-output-command/`](https://techbyexample.com/redirect-output-command/)

## **文件描述符**：

这些是你在终端运行任何命令时会自动打开的文件。

+   0 – 标准输入（STDIN）

+   1 – 标准输出（STDOUT）

+   2 – 标准错误（STDERR）

## **重定向（ > 或 < ）操作符**：

**>** – 这个操作符用于将命令的输出重定向到一个文件或设备（其实它也是一个文件）。此外，如果文件已有内容，这个操作符会覆盖文件内容。

**<** – 这个操作符用于将文件中的输入传递给命令。

**>>** – 这个操作符将内容追加到文件中。

**如何将命令的输出重定向到文件？**

```go
ls -l > results.out
```

**如何将命令的输出和错误重定向到文件？**

```go
ls -l > results.out 2>&1
#Above command simply telling that first redirect output to a file and then pass the error to output. 
```

**如何将命令的输出和错误分别重定向到两个不同的文件？**

```go
ls -l > results.out 2> results.err
```
