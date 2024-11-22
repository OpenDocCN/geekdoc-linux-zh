# 如果任何命令失败，终止或退出一个 Shell 脚本

> 原文：[`techbyexample.com/abort-script-command-fails/`](https://techbyexample.com/abort-script-command-fails/)

# **概述**

如果你希望你的 Shell 脚本或 Shell 文件在任何命令返回非零值时退出，你可以在脚本开始时使用 set -e。

有时脚本中的某个命令失败，但脚本继续执行并未失败，导致出现意外的输出，因为它打破了脚本其余部分的假设，这会令人烦恼。

这是 **set -e** 发挥作用的地方

# **示例**

让我们通过一个例子来理解它。我们将首先编写一个没有 set -e 的脚本。下面是一个简单的脚本。我们将其命名为 **demo.sh**，在这个脚本中，p 是未定义的，会导致错误。

**demo.sh**

```go
p
echo "End of Script"
```

让我们运行这个脚本

```go
sh demo.sh
```

以下是输出结果

```go
demo.sh line 1: p: command not found
End of Script
```

即使脚本在第 1 行失败，echo 的最后一个命令仍然执行了。

现在让我们使用 set -e 来运行相同的脚本

**demo.sh**

```go
set -e
p
echo "End of Script"
```

再次运行脚本。以下是输出结果

```go
demo.sh: line 2: p: command not found
```

使用 set -e 时，一旦第一个命令失败，脚本会立即退出。因此，后续的 echo 命令永远不会执行

但是，在使用管道的情况下，set -e 命令不起作用。让我们看一个例子

```go
set -e
p | echo "Hello"
echo "End of Script"
```

它将给出如下输出

```go
demo.sh: line 2: p: command not found
Hello
End of Script
```

从上面的输出可以看到，最后一个命令确实执行了

我们必须额外使用 **“-o pipefail”** 标志，除了使用 **“-e”** 标志以外，还需要告诉 bash 即使命令在管道中失败，也退出脚本。让我们看一个例子

```go
set -eo pipefail
p | echo "Hello"
echo "End of Script"
```

**输出**

```go
test.sh: line 2: p: command not found
Hello
```

在这种情况下，echo 的最后一个命令没有执行，脚本按预期退出。

**注意：** 查看我们的系统设计教程系列 [系统设计问题](https://techbyexample.com/system-design-questions/)
