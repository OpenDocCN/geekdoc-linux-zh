# `kill` 命令

Linux 中的 `kill` 命令（位于 /bin/kill），是一个内置命令，用于手动终止进程。`kill` 命令向进程发送一个信号，从而终止该进程。如果用户没有指定与 `kill` 命令一起发送的任何信号，则默认发送 *TERM* 信号，该信号将终止进程。

信号可以通过三种方式指定：

+   **按数字（例如 -5**）

+   **带有 SIG 前缀（例如 -SIGkill**）

+   **不带有 SIG 前缀（例如 -kill**）

### 语法

```sh
      kill [OPTIONS] [PID]... 

```

### 示例：

1.  要显示所有可用的信号，可以使用以下命令选项：

```sh
      kill -l 

```

1.  展示如何使用 *PID* 与 `kill` 命令结合使用。

```sh
      $kill pid 

```

1.  展示如何向进程发送信号。

```sh
      kill {-signal | -s signal} pid 

```

1.  指定信号：

+   使用数字作为信号

```sh
      kill -9 pid 

```

+   在信号中使用 SIG 前缀

```sh
      kill -SIGHUP pid 

```

+   在信号中不使用 SIG 前缀

```sh
      kill -HUP pid 

```

### 参数：

要被通知的进程列表可以是名称和 PID 的混合。

```sh
      pid    Each pid can be expressed in one of the following ways:

          n      where n is larger than 0.  The process with PID n is signaled.

          0      All processes in the current process group are signaled.

          -1     All processes with a PID larger than 1 are signaled.

          -n     where n is larger than 1.  All processes in process group  n  are  signaled.
                 When  an  argument  of  the  form '-n' is given, and it is meant to denote a
                 process group, either a signal must be specified first, or the argument must
                 be  preceded  by  a '--' option, otherwise it will be taken as the signal to
                 send.

   name   All processes invoked using this name will be signaled. 

```

### 选项：

```sh
      -s, --signal signal
          The signal to send.  It may be given as a name or a number.

   -l, --list [number]
          Print a list of signal names, or convert the given signal number to  a  name.   The
          signals can be found in /usr/include/linux/signal.h.

   -L, --table
          Similar to -l, but it will print signal names and their corresponding numbers.

   -a, --all
          Do  not  restrict the command-name-to-PID conversion to processes with the same UID
          as the present process.

   -p, --pid
          Only print the process ID (PID) of the named processes, do not send any signals.

   --verbose
          Print PID(s) that will be signaled with kill along with the signal. 

```
