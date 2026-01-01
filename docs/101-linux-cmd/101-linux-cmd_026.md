# `w`命令

`w`命令显示当前在机器上活跃的用户及其[进程](https://www.computerhope.com/jargon/p/process.htm)的详细信息。

### 示例：

1.  在没有[参数](https://www.computerhope.com/jargon/a/argument.htm)的情况下运行`w`命令将显示登录用户及其进程的列表。

```sh
      w 

```

1.  显示名为*hope*的用户的详细信息。

```sh
      w hope 

```

### 语法：

```sh
      finger [-l] [-m] [-p] [-s] [username] 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-h` | `--no-header` | 不打印标题。 |
| `-u` | `--no-current` | 在确定当前进程和 CPU 时间时忽略用户名。*(要查看此示例，请使用`su`切换到 root 用户，然后运行`w`和`w -u`。)* |
| `-s` | `--short` | 显示简化的输出 *(不打印登录时间、JCPU 或 PCPU 时间).* |
| `-f` | `--from` | 切换打印*来自*字段 *(远程主机名)*。默认情况下，不打印来自字段，尽管您的系统管理员或发行版维护者可能已编译了一个默认显示来自字段的版本。 |
| `--help` | - | 显示帮助信息，并退出。 |
| `-V` | `--version` | 显示版本信息，并退出。 |
| `-o` | `--old-style` | 旧式输出 *(对于小于一分钟的空闲时间打印空白空间)*。 |
| *`用户`* | - | 仅显示指定用户的详细信息。 |

### 其他信息

输出[标题](https://www.computerhope.com/jargon/h/header.htm)显示（按此顺序）：当前时间、系统运行时间、当前登录用户数以及过去 1 分钟、5 分钟和 15 分钟的系统[负载](https://www.computerhope.com/jargon/l/load.htm)平均值。

对于每个用户，以下条目将被显示：

+   登录名[终端](https://www.computerhope.com/jargon/t/tty.htm)

+   命名[远程](https://www.computerhope.com/jargon/r/remote.htm)

+   [主机](https://www.computerhope.com/jargon/h/hostcomp.htm)地址

+   从他们登录的时间来计算

+   [空闲](https://www.computerhope.com/jargon/i/idle.htm)时间 JCPU

+   PCPU

+   [命令行](https://www.computerhope.com/jargon/c/commandi.htm)中当前进程

JCPU 时间是连接到 tty 的所有进程所使用的时间。它不包括过去的后台作业，但包括当前正在运行的后台作业。

PCPU 时间是当前进程在“what”字段中命名的时间所使用的时间。
