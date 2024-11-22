# 清除 Mac 上非滚动终端的屏幕

> 原文：[`techbyexample.com/non-scroll-terminal-clear-mac/`](https://techbyexample.com/non-scroll-terminal-clear-mac/)

## **概述**

以下快捷键可用于清除 Mac 上的终端屏幕

```go
cmd + k 
```

执行上述快捷键后，终端屏幕将被清除。此外，屏幕将无法滚动，且无法恢复之前的内容

## **示例**

假设下面是屏幕的当前状态

```go
/ $ pwd
/
/ $ 
```

现在按下

```go
cmd + k 
```

屏幕将被清除

```go
/ $
```

尝试滚动。你将无法滚动到之前的命令。按下上述快捷键后，**‘pwd’** 命令将不再显示。
