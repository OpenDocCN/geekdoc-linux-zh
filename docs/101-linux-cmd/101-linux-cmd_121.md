# `Wait`命令

wait 命令是一个 shell 内建命令，它会在特定的后台进程或所有正在运行的后台子进程完成之前暂停脚本执行。

它的主要目的是同步任务，确保脚本在所有先决条件后台作业完成之前不会继续到下一步。后台进程是一个以与号（&）结尾运行的命令，这告诉 shell 在不等待它完成的情况下运行它。

## 语法

```sh
      $ wait [PID] 

```

[PID] - 可选的等待进程 ID。如果没有提供 PID，wait 将等待所有活动的子进程完成。

## 示例

### 1. 等待特定进程

本例展示了如何启动单个后台进程并等待其完成。

### 脚本：

```sh
      #!/bin/bashecho "This process will run in the background..." &
process_id=$!

echo "Script is now waiting for process ID: $process_id"
wait $process_id
echo "Process $process_id has finished."
echo "The script exited with status: $?" 

```

### 解释：

+   &: 与号运行 echo 命令在后台，允许脚本立即继续到下一行。

+   $!: 这是一个特殊的 shell 变量，它保存了最近执行的后台命令的进程 ID（PID）。我们将其保存到 process_id 变量中。

+   wait $process_id: 这是关键命令。脚本在这里暂停，直到具有该特定 ID 的进程完成。

+   $?: 这个变量保存了最后一个完成的命令的退出状态。退出状态为 0 表示成功。

### 输出：

```sh
      $ bash wait_example.sh
Script is now waiting for process ID: 12345
This process will run in the background...
Process 12345 has finished.
The script exited with status: 0 

```

### 2. 等待所有后台进程

这是最常见的使用场景。在这里，我们启动几个后台任务，然后使用单个 wait 命令暂停，直到它们全部完成。

### 脚本：

```sh
      #!/bin/bash echo "Starting multiple background jobs..."
sleep 3 &
sleep 1 &
sleep 2 &

echo "Waiting for all sleep commands to finish."
wait
echo "All jobs are done. Continuing with the rest of the script." 

```

### 输出：

```sh
      $ bash wait_all_example.sh
Starting multiple background jobs...
Waiting for all sleep commands to finish.
(after about 3 seconds)
All jobs are done. Continuing with the rest of the script. 

```
