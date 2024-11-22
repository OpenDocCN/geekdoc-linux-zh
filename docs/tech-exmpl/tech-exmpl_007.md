# `set -e` 命令的作用是什么

> 原文：[`techbyexample.com/set-e-command/`](https://techbyexample.com/set-e-command/)

# **概览**

如果你希望在脚本中的任何命令返回非零值时，脚本能够退出，那么可以在脚本开头使用 `set -e`。

有时候，当脚本中的某个命令失败时，脚本依然继续执行，这会导致意外输出，因为它打破了脚本中其他部分的假设。

这时，**set -e** 就派上用场了

# **示例**

让我们通过一个例子来理解它。我们将首先编写一个没有使用 `set -e` 的脚本。下面是一个简单的脚本。我们把它命名为 **demo.sh**，在这个脚本中，`p` 未定义，导致错误。

**demo.sh**

```go
p
echo "End of Script"
```

让我们运行这个脚本

```go
sh demo.sh
```

下面是输出结果

```go
demo.sh line 1: p: command not found
End of Script
```

即使脚本在第 1 行失败，但最后的 `echo` 命令还是执行了。

现在我们用 `set -e` 运行相同的脚本

**demo.sh**

```go
set -e
p
echo "End of Script"
```

再次运行脚本。下面是输出结果

```go
demo.sh: line 2: p: command not found
```

使用 `set -e` 后，脚本在第一个命令失败时立即退出。因此后续的 `echo` 命令不会被执行

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
