# 附录 F. I/O 和 I/O 重定向的详细介绍

> 原文：[`tldp.org/LDP/abs/html/ioredirintro.html`](https://tldp.org/LDP/abs/html/ioredirintro.html)

*由 Stphane Chazelas 撰写，并由文档作者修订*

一个命令期望前三个 文件描述符 可用。第一个，*fd 0*（标准输入，`stdin`），用于读取。其他两个（*fd 1*，`stdout` 和 *fd 2*，`stderr`）用于写入。

每个命令都与 `stdin`、`stdout` 和 `stderr` 相关联。`**ls 2>&1**` 表示临时将 **ls** 命令的 `stderr` 连接到与 shell 的 `stdout` 相同的“资源”。

按照惯例，一个命令从 fd 0 (`stdin`) 读取输入，将正常输出打印到 fd 1 (`stdout`)，将错误输出打印到 fd 2 (`stderr`)。如果这三个文件描述符中的任何一个没有打开，你可能会遇到问题：

```sh
bash$ **cat /etc/passwd >&-**
cat: standard output: Bad file descriptor

```

例如，当 **xterm** 运行时，它首先初始化自己。在运行用户的 shell 之前，**xterm** 会三次打开终端设备 (/dev/pts/<n> 或类似的东西)。

在这一点上，Bash 继承了这三个文件描述符，并且每个由 Bash 运行的命令（子进程）依次继承它们，除非你重定向了命令。重定向意味着将一个文件描述符重新分配给另一个文件（或管道，或任何允许的）。文件描述符可以在本地重新分配（对于命令、命令组、子 shell、while 或 if 或 case 或 for 循环…），或者全局地，对于 shell 的剩余部分（使用 exec）。

`**ls > /dev/null**` 表示运行 **ls** 时将其 fd 1 连接到 `/dev/null`。

```sh
bash$ **lsof -a -p $$ -d0,1,2**
COMMAND PID     USER   FD   TYPE DEVICE SIZE NODE NAME
 bash    363 bozo        0u   CHR  136,1         3 /dev/pts/1
 bash    363 bozo        1u   CHR  136,1         3 /dev/pts/1
 bash    363 bozo        2u   CHR  136,1         3 /dev/pts/1

bash$ **exec 2> /dev/null**
bash$ **lsof -a -p $$ -d0,1,2**
COMMAND PID     USER   FD   TYPE DEVICE SIZE NODE NAME
 bash    371 bozo        0u   CHR  136,1         3 /dev/pts/1
 bash    371 bozo        1u   CHR  136,1         3 /dev/pts/1
 bash    371 bozo        2w   CHR    1,3       120 /dev/null

bash$ **bash -c 'lsof -a -p $$ -d0,1,2' &#124; cat**
COMMAND PID USER   FD   TYPE DEVICE SIZE NODE NAME
 lsof    379 root    0u   CHR  136,1         3 /dev/pts/1
 lsof    379 root    1w  FIFO    0,0      7118 pipe
 lsof    379 root    2u   CHR  136,1         3 /dev/pts/1

bash$ **echo "$(bash -c 'lsof -a -p $$ -d0,1,2' 2>&1)"**
COMMAND PID USER   FD   TYPE DEVICE SIZE NODE NAME
 lsof    426 root    0u   CHR  136,1         3 /dev/pts/1
 lsof    426 root    1w  FIFO    0,0      7520 pipe
 lsof    426 root    2w  FIFO    0,0      7520 pipe
```

这适用于不同类型的重定向。

`**练习：**` 分析以下脚本。

```sh
#! /usr/bin/env bash

mkfifo /tmp/fifo1 /tmp/fifo2
while read a; do echo "FIFO1: $a"; done < /tmp/fifo1 & exec 7> /tmp/fifo1
exec 8> >(while read a; do echo "FD8: $a, to fd7"; done >&7)

exec 3>&1
(
 (
  (
   while read a; do echo "FIFO2: $a"; done < /tmp/fifo2 &#124; tee /dev/stderr \
   &#124; tee /dev/fd/4 &#124; tee /dev/fd/5 &#124; tee /dev/fd/6 >&7 & exec 3> /tmp/fifo2

   echo 1st, to stdout
   sleep 1
   echo 2nd, to stderr >&2
   sleep 1
   echo 3rd, to fd 3 >&3
   sleep 1
   echo 4th, to fd 4 >&4
   sleep 1
   echo 5th, to fd 5 >&5
   sleep 1
   echo 6th, through a pipe &#124; sed 's/.*/PIPE: &, to fd 5/' >&5
   sleep 1
   echo 7th, to fd 6 >&6
   sleep 1
   echo 8th, to fd 7 >&7
   sleep 1
   echo 9th, to fd 8 >&8

  ) 4>&1 >&3 3>&- &#124; while read a; do echo "FD4: $a"; done 1>&3 5>&- 6>&-
 ) 5>&1 >&3 &#124; while read a; do echo "FD5: $a"; done 1>&3 6>&-
) 6>&1 >&3 &#124; while read a; do echo "FD6: $a"; done 3>&-

rm -f /tmp/fifo1 /tmp/fifo2

# For each command and subshell, figure out which fd points to what.
# Good luck!

exit 0
```
