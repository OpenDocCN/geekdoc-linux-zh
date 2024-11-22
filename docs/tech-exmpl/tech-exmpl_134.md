# 如何在 bash 或终端中处理命令失败

> 原文：[`techbyexample.com/handle-failure-linux-terminal/`](https://techbyexample.com/handle-failure-linux-terminal/)

# **概览**

如果命令失败不应停止程序的执行，则可以使用以下方法。

```go
command && echo "Success" || echo "Failure"
```

+   如果命令失败，则输出将是“Success”。

+   如果上面的命令通过，则输出将是“Failure”

# **示例**

***   成功案例

在以下示例中，命令是**‘pwd’**，这是一个有效命令

```go
export command=pwd
$command && echo "Success" || echo "Failure"
```

**输出**

```go
{It will print the current directory}
Success
```

+   失败案例

在以下示例中，命令是**‘pwdd’**，这是一个无效命令

```go
export command=pwdd
$command && echo "Success" || echo "Failure"
```

**输出**

```go
-bash: pwdd: command not found
Failure
```

它打印**Failure**，因为**pwdd**是一个无效命令

+   [bash](https://techbyexample.com/tag/bash/)*   [linux](https://techbyexample.com/tag/linux/)*   [terminal](https://techbyexample.com/tag/terminal/)**
