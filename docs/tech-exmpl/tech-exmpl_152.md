# BASH shell 或终端中 “./”、 “.” 和 “source” 的区别（Linux）

> 原文：[`techbyexample.com/difference-dot-slash-source/`](https://techbyexample.com/difference-dot-slash-source/)

让我们在示例**test.sh**文件的上下文中理解这些概念。

## **使用 “./”**

脚本所做的更改会在脚本结束后被丢弃，因为这会通过分叉一个独立的进程来运行脚本。例子：

```go
./test.sh
```

## **使用 “.”**

脚本所做的更改会持续到终端保持开启，因为脚本在同一个 bash 进程中执行。例子：

```go
. test.sh
```

## **使用 “source”**

“.” 和 “.” 是同义词，表示相同的意思。“.” 是 POSIX 标准的一部分。例子：

```go
source test.sh
```
