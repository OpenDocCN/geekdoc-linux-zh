# `banner` 命令

`banner` 命令将 ASCII 字符串以大写字母的形式写入标准输出。输出中的每一行可以包含最多 10 个大写或小写字符。在输出时，所有字符都显示为大写，小写输入字符看起来比大写输入字符小。

注意：如果你使用 sleep 命令定义了多个数字，那么此命令将延迟为这些值的总和。

### 示例：

1.  要在工作站上显示 banner，请输入：

```sh
      banner LINUX! 

```

1.  要在一行上显示多个单词，请将文本用引号括起来，如下所示：

```sh
      banner "Intro to" Linux 

```

> 这将在一行上显示 Intro to，并在下一行显示 Linux

1.  以大写字母打印“101LinuxCommands”。

```sh
      banner 101LinuxCommands 

```

> 它只会打印 101LinuxCo，因为 banner 的默认容量为 10

* * *
