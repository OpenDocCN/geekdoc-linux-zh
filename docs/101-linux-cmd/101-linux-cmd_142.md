# `bg` 命令

使用 `bg` 命令可以将停止的作业放入后台，允许它们在您使用终端进行其他任务时继续运行。这是类 Unix shell 中的作业控制功能的一部分。

## 语法

```sh
      bg [job_spec] 

```

如果没有提供作业说明，`bg` 将作用于当前作业（最近的作业）。

## 职位说明

您可以使用不同的格式引用作业：

+   `%1` - 第 1 个作业号

+   `%+` 或 `%%` - 当前作业（最近的）

+   `%-` - 前一个作业

+   `%string` - 命令行以字符串开头的作业

+   `%?string` - 包含字符串的命令行作业

## 示例

1.  将当前停止的作业放入后台

```sh
      bg 

```

1.  将特定作业放入后台

```sh
      bg %1 

```

1.  将多个作业放入后台

```sh
      bg %1 %2 %3 

```

## 完整的作业控制工作流程

这里是一个典型的流程，展示了 `bg` 的使用：

1.  启动长时间运行的命令

```sh
      find / -name "*.log" > /tmp/findlogs.txt 2>/dev/null 

```

1.  使用 Ctrl+Z 停止作业

```sh
      ^Z
[1]+  Stopped    find / -name "*.log" > /tmp/findlogs.txt 2>/dev/null 

```

1.  检查作业

```sh
      jobs 

```

输出：

```sh
      [1]+  Stopped    find / -name "*.log" > /tmp/findlogs.txt 2>/dev/null 

```

1.  将停止的作业放入后台

```sh
      bg %1 

```

输出：

```sh
      [1]+ find / -name "*.log" > /tmp/findlogs.txt 2>/dev/null & 

```

1.  验证作业是否在后台运行

```sh
      jobs 

```

输出：

```sh
      [1]+  Running    find / -name "*.log" > /tmp/findlogs.txt 2>/dev/null & 

```

## 实际示例

1.  使用文本编辑器工作

```sh
      # Start editing a file
vim myfile.txt

# Stop with Ctrl+Z
# Put it in background
bg

# Now you can run other commands while vim runs in background
ls -la

# Bring vim back to foreground when needed
fg %1 

```

1.  管理多个后台任务

```sh
      # Start several tasks and stop them
ping google.com > /tmp/ping1.txt
# Ctrl+Z
sleep 300
# Ctrl+Z
tar czf backup.tar.gz /home/user/documents
# Ctrl+Z

# Check all stopped jobs
jobs

# Put all in background
bg %1
bg %2
bg %3

# Or put specific ones
bg %ping    # Job starting with "ping" 

```

1.  直接在后台启动命令与使用 bg

```sh
      # Method 1: Start directly in background
find /usr -name "*.conf" > /tmp/configs.txt &

# Method 2: Start normally, stop, then background
find /usr -name "*.conf" > /tmp/configs.txt
# Ctrl+Z
bg 

```

## 相关命令

+   `fg` - 将作业带到前台

+   `jobs` - 列出活动作业

+   `kill` - 终止作业

+   `nohup` - 运行不受挂起影响的命令

+   `disown` - 从作业表中删除作业

## 用例

+   **多任务处理**：同时运行多个任务

+   **长时间进程**：在处理其他事情的同时运行耗时任务

+   **交互式程序**：暂时将编辑器或交互式工具放入后台

+   **开发**：编码时的后台编译

+   **系统管理**：在执行其他任务的同时进行后台监控

## 重要提示

+   使用 `bg` 放入后台的作业仍然连接到终端

+   如果您关闭终端，后台作业可能会被终止

+   使用 `nohup` 或 `disown` 进行持久后台进程

+   后台作业无法从 stdin（键盘输入）读取

+   您可以使用 `fg` 将后台作业恢复到前台

+   后台作业将继续写入 stdout/stderr，除非重定向

## 错误处理

如果 `bg` 失败，常见原因包括：

+   作业不存在

+   作业已经在运行

+   作业无法放入后台（某些交互式程序）

## 小贴士

+   在使用 `bg` 之前和之后，始终使用 `jobs` 检查作业状态

+   将后台作业的输出重定向以避免终端混乱

+   负责任地使用作业控制以避免系统资源问题

+   考虑使用 `screen` 或 `tmux` 等终端多路复用器进行持久会话

`bg` 命令对于在 shell 环境中有效地进行多任务处理和作业管理至关重要。

更多详细信息，请查看手册：`help bg`
