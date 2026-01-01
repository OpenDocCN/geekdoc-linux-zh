# `help` 命令

`help` 命令显示有关内置命令的信息。显示有关内置命令的信息。

如果指定了 `PATTERN`，则此命令会提供与 `PATTERN` 匹配的所有命令的详细帮助，否则会打印可用的帮助主题列表。

> **快速提示**：`help` 命令仅适用于本身是 Bash shell 内置的命令（如 `cd`、`pwd`、`echo`、`read`）。它对独立程序（如 `ls`、`grep` 或 `find`）不起作用。要获取这些程序的帮助，应使用 `man` 命令（例如，`man ls`）。

## 语法

```sh
      $ help [-dms] [PATTERN ...] 

```

## 选项

| **选项** | **描述** |
| :-- | :-- |
| `-d` | 为每个主题输出简短描述。 |
| `-m` | 以伪手册页格式显示用法。 |
| `-s` | 仅输出与提供的 `PATTERN` 匹配的每个主题的简短用法摘要。 |

## 使用示例：

1.  我们可以获取 `cd` 命令的完整信息

```sh
      $ help cd
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.

    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.
...
(additional details about options and exit status follow) 

```

1.  我们可以获取关于 `pwd` 命令的简短描述

```sh
      $ help -d pwd
pwd: pwd [-LP]
    Print the name of the current working directory. 

```

1.  我们可以获取 `cd` 命令的语法

```sh
      $ help -s cd
cd [-L|[-P [-e]] [-@]] [dir] 

```
