# `jobs` 命令

`jobs` 命令用于显示当前 shell 会话中活动作业的信息。作业是从 shell 启动的进程，可以使用作业控制命令进行管理。

## 语法

```sh
      jobs [options] [job_spec] 

```

## 选项

一些流行的选项标志包括：

```sh
      -l      List process IDs along with job information
-p      List only process IDs
-n      List only jobs that have changed status since last notification
-r      List only running jobs
-s      List only stopped jobs
-x      Replace job specifications with process IDs in command 

```

## 作业状态

作业可以处于不同的状态：

+   **运行中**：作业目前正在执行

+   **已停止**：作业已挂起（暂停）

+   **完成**：作业已成功完成

+   **已终止**：作业被杀死或异常结束

## 示例

1.  列出所有当前作业

```sh
      jobs 

```

1.  按进程 ID 列出作业

```sh
      jobs -l 

```

1.  仅列出进程 ID

```sh
      jobs -p 

```

1.  仅列出正在运行的作业

```sh
      jobs -r 

```

1.  仅列出已停止的作业

```sh
      jobs -s 

```

1.  显示特定作业的状态

```sh
      jobs %1 

```

## 作业控制示例

1.  启动后台作业

```sh
      sleep 100 & 

```

1.  启动多个后台作业

```sh
      find / -name "*.log" > /tmp/logs.txt 2>/dev/null &
ping google.com > /tmp/ping.txt & 

```

1.  查看所有作业

```sh
      jobs 

```

输出可能如下所示：

```sh
      [1]-  Running    find / -name "*.log" > /tmp/logs.txt 2>/dev/null &
[2]+  Running    ping google.com > /tmp/ping.txt & 

```

1.  停止正在运行的作业（Ctrl+Z）

```sh
      # Start a command
vim myfile.txt
# Press Ctrl+Z to stop it
# Then check jobs
jobs 

```

1.  将作业带到前台

```sh
      fg %1 

```

1.  将作业发送到后台

```sh
      bg %1 

```

1.  杀死特定作业

```sh
      kill %2 

```

## 作业规范

您可以使用不同的格式引用作业：

+   `%1` - 第 1 个作业号

+   `%+` 或 `%%` - 当前作业（最新）

+   `%-` - 前一个作业

+   `%string` - 命令行以字符串开头的作业

+   `%?string` - 命令行包含字符串的作业

## 作业控制示例

1.  启动和管理多个作业

```sh
      # Start some background jobs
sleep 300 &
ping localhost > /dev/null &
find /usr -name "*.conf" > /tmp/configs.txt 2>/dev/null &

# List all jobs
jobs -l

# Bring first job to foreground
fg %1

# Put it back to background (after stopping with Ctrl+Z)
bg %1

# Kill second job
kill %2

# Check remaining jobs
jobs 

```

1.  与已停止作业一起工作

```sh
      # Start a text editor
nano myfile.txt

# Stop it with Ctrl+Z
# Check jobs
jobs

# Resume in background
bg

# Resume in foreground
fg 

```

## 用例

+   **多任务处理**：同时运行多个命令

+   **长时间运行的过程**：管理需要时间才能完成的任务

+   **后台处理**：在处理其他事情的同时运行任务

+   **作业监控**：跟踪运行中的进程

+   **进程管理**：控制和组织 shell 进程

## 相关命令

+   `fg` - 将作业带到前台

+   `bg` - 将作业发送到后台

+   `nohup` - 运行不受挂起的命令

+   `disown` - 从作业表中删除作业

+   `kill` - 终止作业或进程

## 重要注意事项

+   作业特定于当前 shell 会话

+   作业号是按顺序分配的

+   作业在完成或退出 shell 时消失

+   在命令末尾使用 `&` 以在后台运行它

+   按 `Ctrl+Z` 停止（挂起）正在运行的作业

+   使用 `Ctrl+C` 终止正在运行的作业

## 高级示例

1.  在后台运行并取消作业的所有权

```sh
      long_running_script.sh &
disown %1 

```

1.  检查完成的作业

```sh
      jobs -n 

```

1.  杀死所有作业

```sh
      kill $(jobs -p) 

```

`jobs` 命令对于管理多个进程和在 shell 中实施有效的流程管理至关重要。

更多详细信息，请查看手册：`man jobs` 或 `help jobs`
