# 如何在 bash 或终端（Linux）中取消设置一个变量

> 原文：[`techbyexample.com/unset-env-variable-terminal/`](https://techbyexample.com/unset-env-variable-terminal/)

**unset** 命令可用于在终端中取消设置一个变量。

让我们来看一个示例

```go
test=some_value
echo "Value before unset=$test"
unset test
echo "Value after unset=$test"
```

**输出**

```go
Value before unset=some_value
Value after unset=
```

如上面的输出所示，在执行 unset 命令后，变量 test 的值变为空。

+   [env](https://techbyexample.com/tag/env/)*   [unset](https://techbyexample.com/tag/unset/)
