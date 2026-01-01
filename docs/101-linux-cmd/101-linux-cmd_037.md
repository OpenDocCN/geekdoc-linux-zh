# `printenv` 命令

`printenv` 打印指定 [环境变量](https://www.computerhope.com/jargon/e/envivari.htm) 的值。如果没有指定 [*变量*](https://www.computerhope.com/jargon/v/variable.htm)，则打印所有变量的名称和值对。

### 示例：

1.  显示所有环境变量的值。

```sh
      printenv 

```

1.  显示当前用户 [主目录](https://www.computerhope.com/jargon/h/homedir.htm) 的位置。

```sh
      printenv HOME 

```

1.  要将 `--null` 命令行选项用作输出条目之间的终止字符。

```sh
      printenv --null SHELL HOME 

```

*注意：默认情况下，* `printenv` *命令使用换行符作为输出条目之间的终止字符。*

### 语法：

```sh
      printenv [OPTION]... PATTERN... 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-0` | `--null` | 以 **0** 字节而不是 [换行符](https://www.computerhope.com/jargon/n/newline.htm) 结束每行输出。 |
| `--help` | - | 显示帮助信息并退出。 |
