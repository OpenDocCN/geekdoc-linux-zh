# `pidof` 命令

`pidof` 是一个命令行实用程序，允许您查找正在运行的程序的进程 ID。

## 语法

```sh
      pidof [OPTIONS] PROGRAM_NAME 

```

要查看命令的帮助信息和所有选项：

```sh
      [user@home ~]$ pidof -h

 -c           Return PIDs with the same root directory
 -d <sep>     Use the provided character as output separator
 -h           Display this help text
 -n           Avoid using stat system function on network shares
 -o <pid>     Omit results with a given PID
 -q           Quiet mode. Do not display output
 -s           Only return one PID
 -x           Return PIDs of shells running scripts with a matching name
 -z           List zombie and I/O waiting processes. May cause pidof to hang. 

```

## 示例：

要查找 SSH 服务器进程的 PID，您将运行：

```sh
      pidof sshd 

```

如果有与 `sshd` 名称匹配的正在运行的进程，它们的 PID 将显示在屏幕上。如果没有找到匹配项，输出将为空。

```sh
      # Output4382 4368 811 

```

`pidof` 命令在至少有一个正在运行的程序与请求的名称匹配时返回 `0`。否则，退出代码为 `1`。这在编写 shell 脚本时可能很有用。

要确保仅显示您正在搜索的程序 PID，请使用程序的完整路径作为参数。例如，如果您有两个在不同目录中运行的具有相同名称的正在运行的程序，`pidof` 将显示两个正在运行的程序的 PID。

默认情况下，所有匹配的正在运行的程序的 PID 都会显示。使用 `-s` 选项强制 `pidof` 仅显示一个 PID：

```sh
      pidof -s program_name 

```

`-o` 选项允许您从命令输出中排除具有给定 PID 的进程：

```sh
      pidof -o pid program_name 

```

当 `pidof` 使用 `-o` 选项调用时，您可以使用一个特殊的 PID，即 %PPID，它代表调用 shell 或 shell 脚本。

要仅返回具有相同根目录运行的进程的 PID，请使用 `-c` 选项。此选项仅在 `pidof` 以 `root` 或 `sudo` 用户运行时才有效：

```sh
      pidof -c pid program_name 

```

## 结论

`pidof` 命令用于查找特定正在运行的程序的 PID。

`pidof` 是一个简单的命令，没有很多选项。通常，您只会用您正在搜索的程序名称调用 `pidof`。
