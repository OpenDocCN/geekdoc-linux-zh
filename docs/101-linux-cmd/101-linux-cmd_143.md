# `fg` 命令

`fg` 命令用于将后台或停止的作业带到前台，使其成为终端中的活动进程。它是类 Unix shell 中作业控制的一个基本部分。

## 语法

```sh
      fg [job_spec] 

```

如果没有提供作业指定，`fg` 将操作当前作业（最近的作业）。

## 作业指定

您可以使用不同的格式引用作业：

+   `%1` - 第 1 个作业

+   `%+` 或 `%%` - 当前作业（最近的）

+   `%-` - 前一个作业

+   `%string` - 命令行以字符串开头的作业

+   `%?string` - 命令行包含字符串的作业

## 示例

1.  将当前作业带到前台

```sh
      fg 

```

1.  将特定作业带到前台

```sh
      fg %1 

```

1.  通过部分命令名来调用作业

```sh
      fg %vim 

```

1.  调用包含特定文本的作业

```sh
      fg %?backup 

```

## 完整的作业控制工作流程

这里是一个展示 `fg` 命令使用的典型工作流程：

1.  启动一个后台作业

```sh
      ping google.com > /tmp/ping.txt & 

```

1.  启动另一个作业并停止它

```sh
      vim myfile.txt
# Press Ctrl+Z to stop 

```

1.  检查当前作业

```sh
      jobs 

```

输出：

```sh
      [1]-  Running    ping google.com > /tmp/ping.txt &
[2]+  Stopped    vim myfile.txt 

```

1.  将 vim 带到前台

```sh
      fg %2 

```

1.  在 vim 中工作，然后再次停止（Ctrl+Z）并将 ping 带到前台

```sh
      fg %1 

```

## 实际示例

1.  与编辑器一起工作

```sh
      # Start editing
nano config.txt
# Stop with Ctrl+Z
# Do other work
ls -la
# Return to editor
fg 

```

1.  管理多个开发任务

```sh
      # Start compilation in background
make all > build.log 2>&1 &

# Start editing source code
vim main.c
# Stop editor (Ctrl+Z)

# Check build progress
fg %make
# Stop build monitoring (Ctrl+Z)

# Return to editing
fg %vim 

```

1.  交互式调试会话

```sh
      # Start debugger
gdb ./myprogram
# Stop debugger (Ctrl+Z)

# Check core dumps or logs
ls -la core.*

# Return to debugger
fg %gdb 

```

1.  与多个终端/会话一起工作

```sh
      # Start SSH session
ssh user@remote-server
# Stop SSH (Ctrl+Z)

# Do local work
ps aux | grep myprocess

# Return to SSH session
fg %ssh 

```

## 高级用法

1.  在多个停止的作业之间切换

```sh
      # Start several editors
vim file1.txt
# Ctrl+Z
vim file2.txt
# Ctrl+Z
nano file3.txt
# Ctrl+Z

# Check all jobs
jobs

# Switch between them
fg %1    # vim file1.txt
# Ctrl+Z
fg %2    # vim file2.txt
# Ctrl+Z
fg %3    # nano file3.txt 

```

1.  在脚本中使用作业控制

```sh
      #!/bin/bash# Start background monitoring
tail -f /var/log/syslog &
MONITOR_PID=$!

# Do main work
./main_script.sh

# Bring monitor to foreground for review
fg %tail

# Or kill it
kill $MONITOR_PID 

```

## 相关命令

+   `bg` - 将作业放入后台

+   `jobs` - 列出活动作业

+   `kill` - 终止作业

+   `Ctrl+Z` - 停止（挂起）当前作业

+   `Ctrl+C` - 终止当前作业

## 用例

+   **代码编辑**：在多个打开的编辑器之间切换

+   **开发**：在编译和编辑之间交替

+   **系统监控**：在监控工具之间切换

+   **远程会话**：恢复 SSH 或其他远程连接

+   **交互程序**：返回暂停的交互式应用程序

+   **调试**：恢复调试会话

## 重要提示

+   当作业被带到前台时，它成为活动进程

+   您一次只能有一个前台作业

+   前台作业可以接收键盘输入

+   使用 Ctrl+Z 停止（挂起）前台作业

+   使用 Ctrl+C 终止前台作业

+   后台作业即使在不在前台时也会继续运行

## 错误处理

`fg` 的常见问题：

+   作业不存在：`fg: %3: no such job`

+   没有可用的作业：`fg: no current job`

+   作业已在前台

## 有效使用技巧

1.  **使用作业编号**：比部分名称更可靠

1.  **首先检查作业**：始终运行 `jobs` 来查看当前状态

1.  **一致的流程**：为作业切换制定常规

1.  **重定向输出**：后台作业应重定向输出以避免干扰

```sh
      # Good practice
tail -f /var/log/messages > monitor.out 2>&1 &
vim script.sh
# Ctrl+Z
fg %tail  # Review logs
# Ctrl+Z
fg %vim   # Continue editing 

```

## 与其他工具的集成

`fg` 与以下工具配合使用：

+   **终端多路复用器**：`screen`、`tmux`

+   **开发环境**：IDE、编辑器

+   **系统监控**：`top`、`htop`、`tail`

+   **网络工具**：`ssh`、`ping`、`netstat`

`fg` 命令对于高效的终端多任务处理至关重要，并提供了在不同任务之间无缝切换的能力。

更多详细信息，请查看手册：`help fg`
