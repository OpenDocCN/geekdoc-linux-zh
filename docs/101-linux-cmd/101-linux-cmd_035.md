# `killall` 命令

`killall` 向运行指定命令的所有进程发送信号。如果没有指定信号名称，则发送 `SIGTERM`。通常，`killall` 命令通过进程名称来杀死所有进程。

信号可以通过名称（例如 `-HUP` 或 `-SIGHUP`）、数字（例如 `-1`）或选项 `-s` 来指定。

如果命令名称不是正则表达式（选项 `-r`）且包含斜杠（`/`），则将选择执行该特定文件的进程进行杀死，无论其名称如何。

如果每个列出的命令至少有一个进程被杀死，或者没有列出命令但至少有一个进程匹配 `-u` 和 `-Z` 搜索标准，则 `killall` 返回零退出代码。否则返回非零。

`killall` 进程永远不会杀死自己（但可能会杀死其他 `killall` 进程）。

### 示例：

1.  使用 `SIGTERM` 杀死匹配名称 `conky` 的所有进程：

```sh
      killall conky
# OR
killall -SIGTERM conky
# OR
killall -15 conky 

```

您也可以用这种方法杀死 Wine 进程（在 Linux 上运行的 Windows 可执行文件）。

```sh
      killall TQ.exe 

```

1.  列出所有支持的信号：

```sh
      $ killall -l
HUP INT QUIT ILL TRAP ABRT BUS FPE KILL USR1 SEGV USR2 PIPE ALRM TERM STKFLT
CHLD CONT STOP TSTP TTIN TTOU URG XCPU XFSZ VTALRM PROF WINCH POLL PWR SYS 

```

关于数字。

```sh
      $ for s in $(killall -l); do echo -n "$s " && kill -l $s; done
HUP 1
INT 2
QUIT 3
ILL 4
TRAP 5
ABRT 6
BUS 7
FPE 8
KILL 9
USR1 10
SEGV 11
USR2 12
PIPE 13
ALRM 14
TERM 15
STKFLT 16
CHLD 17
CONT 18
STOP 19
TSTP 20
TTIN 21
TTOU 22
URG 23
XCPU 24
XFSZ 25
VTALRM 26
PROF 27
WINCH 28
POLL 29
PWR 30
SYS 31 

```

1.  在杀死之前询问，以防止意外杀死：

```sh
      $ killall -i conky
Kill conky(1685) ? (y/N) 

```

1.  杀死所有进程并等待进程结束。

```sh
      killall -w conky 

```

1.  根据时间杀死：

```sh
      # Kill all firefox younger than 2 minutes
killall -y 2m  firefox

# Kill all firefox older than 2 hours
killall -o 2h  firefox 

```

### 语法：

```sh
      killall [OPTION]... [--] NAME...
killall -l, --list
killall -V, --version 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-e` | `--exact` | 对于非常长的名称要求精确匹配 |
| `-I` | `--ignore-case` | 不区分大小写的进程名称匹配 |
| `-g` | `--process-group` | 杀死进程组而不是进程 |
| `-y` | `--younger-than` | 杀死比指定时间年轻的进程 |
| `-o` | `--older-than` | 杀死超过指定时间的进程 |
| `-i` | `--interactive` | 在杀死进程之前提示，以避免意外终止。 |
| `-l` | `--list` | 列出所有已知的信号名称 |
| `-q` | `--quiet` | 不打印抱怨信息 |
| `-r` | `--regexp` | 将名称解释为扩展正则表达式 |
| `-s` | `--signal SIGNAL` | 发送此信号而不是 SIGTERM |
| `-u` | `--user USER` | 只杀死以用户身份运行的进程 |
| `-v` | `--verbose` | 报告信号是否成功发送 |
| `-w` | `--wait` | 等待进程结束 |
| `-n` | `--ns PID` | 匹配属于指定 PID 的同一命名空间的进程。 |
| `-Z` | `--context` | REGEXP 只杀死具有上下文的进程（必须先于其他参数） |

### 相关命令

kill, `pidof`
