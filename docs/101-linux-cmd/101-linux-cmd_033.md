# `ps` 命令

`ps` 命令（进程状态）用于显示 Linux 系统上运行进程的信息——例如它们的 PID、内存使用情况、CPU 时间以及关联的用户。

它通常与 `grep` 等命令一起使用，以搜索特定进程或使用 `less` 滚动大量输出。

## 为什么使用 `ps`

想象你的系统感觉变慢或应用程序变得无响应——你可以使用 `ps` 来：

+   识别消耗大量 CPU/内存的进程

+   查找程序的 PID（进程 ID）

+   杀死或调试挂起的进程

+   检查共享系统上谁在运行什么

## 基本语法

```sh
      ps [options] 

```

没有指定任何选项时，`ps` 命令只显示当前终端会话中的进程。

示例：

```sh
      ps 

```

输出：

```sh
      PID TTY          TIME CMD
 4587 pts/0    00:00:00 bash
 4621 pts/0    00:00:00 ps 

```

## essential 使用

**要记住的一个组合：** `ps aux`

+   `a` = 所有进程（所有用户）

+   `u` = 显示用户/所有者信息

+   `x` = 包括没有终端的进程

```sh
      ps aux 

```

输出示例：

```sh
      USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 168208  1100 ?        Ss   10:15   0:02 /sbin/init
myuser    2471  0.5  1.2 431204 24500 ?        Sl   10:17   1:05 code
myuser    2523  2.3  0.7 230940 14860 pts/0    R+   10:22   0:01 ps aux 

```

## 一些更实用的日常示例

### 查找并杀死进程

你想要停止一个冻结的 **OpenShot** 进程。

```sh
      ps aux | grep openshot 

```

输出：

```sh
      myuser   3625  99.9  6.1 1243924 252340 ?  Rl  10:30  25:17 openshot
myuser   3649   0.0  0.0   6348   740 pts/0  S+  10:31   0:00 grep --color=auto openshot 

```

现在，杀死它：

```sh
      kill -9 3625 

```

### 通过用户显示进程

```sh
      ps -u username 

```

输出：

```sh
      PID TTY          TIME CMD
 2284 ?        00:00:00 sshd
 2455 ?        00:00:02 bash 

```

### 过滤和排序输出

显示前 10 个内存消耗最多的进程：

```sh
      ps aux --sort=-%mem | head -10 

```

显示前 10 个 CPU 消耗最多的进程：

```sh
      ps aux --sort=-%cpu | head -10 

```

### 检查父/子进程层次结构

```sh
      ps -ef --forest 

```

这会显示一个树状结构，显示父子关系——在调试服务启动时非常有用。

### 自定义输出格式

要仅显示 PID、用户、内存和命令：

```sh
      ps -eo pid,user,%mem,cmd 

```

## 真实生活中的 DevOps 示例

### 1. 检查哪个进程使用了特定的端口

```sh
      sudo ps -fp $(sudo lsof -t -i:8080) 

```

### 2. 监控 Jenkins、Nginx 或 Docker 进程

```sh
      ps aux | grep nginx
ps aux | grep jenkins
ps aux | grep docker 

```

### 3. 查找僵尸进程

```sh
      ps aux | awk '{ if ($8 == "Z") print $0; }' 

```

## 快速参考的关键选项

| 选项 | 描述 |
| :-- | :-- |
| `aux` | 所有具有详细信息的进程 |
| `-ef` | 完整列表（替代 aux） |
| `-eo format` | 自定义输出列 |
| `--sort` | 按列排序（-%mem, -%cpu） |
| `-p PID` | 显示特定的 PID |
| `-C name` | 通过命令名显示进程 |
| `-u user` | 显示用户的进程 |
| `f` | ASCII 艺术进程树 |

### 其他选项：

| **选项** | **描述** |
| :-- | :-- |
| `a` | 显示具有终端（tty）的所有进程列表 |
| `-A` | 列出所有进程。等同于 `-e` |
| `-a` | 显示除会话领导者和与终端不关联的进程之外的所有进程 |
| `-d` | 选择除会话领导者之外的所有进程 |
| `--deselect` | 显示除满足指定条件之外的进程。等同于 `-N` |
| `-e` | 列出所有进程。等同于 `-A` |
| `-N` | 显示除满足指定条件之外的进程。等同于 `-deselect` |
| `T` | 选择与该终端关联的所有进程。等同于没有参数的 `-t` 选项 |
| `r` | 仅选择运行中的进程 |
| `--help simple` | 显示所有基本选项 |
| `--help all` | 显示所有可用选项 |

## 相关工具

如果你需要 **实时** 监控，请使用：

```sh
      top 

```

或者更用户友好的现代版本：

```sh
      htop 

```
