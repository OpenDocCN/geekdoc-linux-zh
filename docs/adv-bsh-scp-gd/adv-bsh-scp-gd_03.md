# 第二章. 从 Sha-Bang 开始

> 原文：[`tldp.org/LDP/abs/html/sha-bang.html`](https://tldp.org/LDP/abs/html/sha-bang.html)

|   |  **Shell 脚本编程是 1950 年代的点唱机 . . .**

*--Larry Wall**  |

**目录**

2.1\. 调用脚本

2.2\. 预备练习

在最简单的情况下，脚本不过是一系列存储在文件中的系统命令。至少，这可以节省每次调用时重新输入特定命令序列的麻烦。

**示例 2-1\. *cleanup*：用于清理 /var/log 中日志文件的脚本**

```sh
# Cleanup
# Run as root, of course.

cd /var/log
cat /dev/null > messages
cat /dev/null > wtmp
echo "Log files cleaned up."
```

这里没有什么不寻常的，只是一系列命令，这些命令也可以逐个从控制台或终端窗口的命令行中调用。将命令放在脚本中的优点远远超出了不必一次次重新输入它们。脚本变成了 *程序*——*工具*——并且它可以很容易地修改或定制以适应特定应用。

**示例 2-2\. *cleanup*：改进的清理脚本**

```sh
#!/bin/bash
# Proper header for a Bash script.

# Cleanup, version 2

# Run as root, of course.
# Insert code here to print error message and exit if not root.

LOG_DIR=/var/log
# Variables are better than hard-coded values.
cd $LOG_DIR

cat /dev/null > messages
cat /dev/null > wtmp

echo "Logs cleaned up."

exit #  The right and proper method of "exiting" from a script.
     #  A bare "exit" (no parameter) returns the exit status
     #+ of the preceding command. 
```

现在看起来像是一个真正的脚本了。但我们还可以更进一步 . . .

**示例 2-3\. *cleanup*：上述脚本的增强和通用版本。**

```sh
#!/bin/bash
# Cleanup, version 3

#  Warning:
#  -------
#  This script uses quite a number of features that will be explained
#+ later on.
#  By the time you've finished the first half of the book,
#+ there should be nothing mysterious about it.

LOG_DIR=/var/log
ROOT_UID=0     # Only users with $UID 0 have root privileges.
LINES=50       # Default number of lines saved.
E_XCD=86       # Can't change directory?
E_NOTROOT=87   # Non-root exit error.

# Run as root, of course.
if [ "$UID" -ne "$ROOT_UID" ]
then
  echo "Must be root to run this script."
  exit $E_NOTROOT
fi  

if [ -n "$1" ]
# Test whether command-line argument is present (non-empty).
then
  lines=$1
else  
  lines=$LINES # Default, if not specified on command-line.
fi  

#  Stephane Chazelas suggests the following,
#+ as a better way of checking command-line arguments,
#+ but this is still a bit advanced for this stage of the tutorial.
#
#    E_WRONGARGS=85  # Non-numerical argument (bad argument format).
#
#    case "$1" in
#    ""      ) lines=50;;
#    *[!0-9]*) echo "Usage: `basename $0` lines-to-cleanup";
#     exit $E_WRONGARGS;;
#    *       ) lines=$1;;
#    esac
#
#* Skip ahead to "Loops" chapter to decipher all this.

cd $LOG_DIR

if [ `pwd` != "$LOG_DIR" ]  # or   if [ "$PWD" != "$LOG_DIR" ]
                            # Not in /var/log?
then
  echo "Can't change to $LOG_DIR."
  exit $E_XCD
fi  # Doublecheck if in right directory before messing with log file.

# Far more efficient is:
#
# cd /var/log &#124;&#124; {
#   echo "Cannot change to necessary directory." >&2
#   exit $E_XCD;
# }

tail -n $lines messages > mesg.temp # Save last section of message log file.
mv mesg.temp messages               # Rename it as system log file.

#  cat /dev/null > messages
#* No longer needed, as the above method is safer.

cat /dev/null > wtmp  #  ': > wtmp' and '> wtmp'  have the same effect.
echo "Log files cleaned up."
#  Note that there are other log files in /var/log not affected
#+ by this script.

exit 0
#  A zero return value from the script upon exit indicates success
#+ to the shell.
```

由于你可能不想删除整个系统日志，这个版本的脚本保留了消息日志的最后部分。你将不断发现调整先前编写的脚本以增强效果的方法。

* * *

脚本开头的 *sha-bang* ( #!) [[1]](#FTN.AEN205) 告诉系统这个文件是一组要传递给指定命令解释器的命令。#! 实际上是一个双字节 [[2]](#FTN.AEN214) *魔数*，一个特殊标记，用于指定文件类型，在这种情况下是一个可执行的 shell 脚本（使用 `**man magic**` 获取更多关于这个有趣主题的详细信息）。紧随 *sha-bang* 之后的是 *路径名*。这是解释脚本中命令的程序路径，无论是 shell、编程语言还是实用程序。然后，这个命令解释器从脚本顶部（紧随 *sha-bang* 行之后的行）开始执行命令，并忽略注释。[[3]](#FTN.AEN226)

```sh
#!/bin/sh
#!/bin/bash
#!/usr/bin/perl
#!/usr/bin/tcl
#!/bin/sed -f
#!/bin/awk -f
```

上述每个脚本头部行都调用不同的命令解释器，无论是 `/bin/sh`、Linux 系统中的默认 shell（**bash**）还是其他。[[4]](#FTN.AEN242) 使用 `**#!/bin/sh**`，大多数商业 UNIX 变种的默认 Bourne shell，使得脚本 可移植 到非 Linux 机器上，尽管你 牺牲了 Bash 特定功能。然而，脚本将符合 POSIX [[5]](#FTN.AEN256) **sh** 标准。

注意，“sha-bang”处给出的路径必须正确，否则运行脚本时只会得到错误信息——通常是“命令未找到。” [[6]](#FTN.AEN269)

如果脚本只包含一组通用系统命令，不使用内部 shell 指令，则可以省略#!。上面提到的第二个例子需要初始的#!，因为变量赋值行`**lines=50**`使用了 shell 特定的结构。[[7]](#FTN.AEN279) 再次注意，`**#!/bin/sh**`调用默认的 shell 解释器，在 Linux 机器上默认为`/bin/bash`。

| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 这个教程鼓励采用模块化方法构建脚本。注意并收集可能在未来的脚本中有用的“样板”代码片段。最终，你将建立一个相当广泛的实用例程库。例如，以下脚本序言测试脚本是否以正确的参数数量调用。

| [```sh E_WRONG_ARGS=85
script_parameters="-a -h -m -z"
#                  -a = all, -h = help, etc.

if [ $# -ne $Number_of_expected_args ]
then
  echo "Usage: `basename $0` $script_parameters"
  # `basename $0` is the script's filename.
  exit $E_WRONG_ARGS
fi
```] | 

许多时候，你会编写一个执行特定任务的脚本。本章的第一个脚本就是一个例子。后来，你可能想将脚本泛化以执行其他类似任务。用变量替换字面量（“硬编码”）的常量是朝着这个方向迈出的一步，同样，用函数替换重复的代码块也是这样。

### 注意事项

| [[1]](sha-bang.html#AEN205) | 在文献中更常见的是*she-bang*或*sh-bang*。这源于*sharp* (#)和*bang* (!)这两个标记的连接。 |
| --- | --- |
| [[2]](sha-bang.html#AEN214) | 据说某些 UNIX 版本（基于 4.2 BSD 的版本）采用四个字节的魔数，需要在!后面留空格--`**#! /bin/sh**`。[根据 Sven Mascheck](http://www.in-ulm.de/~mascheck/various/shebang/#details)的说法，这可能是神话。 |

| [[3]](sha-bang.html#AEN226) | 在 shell 脚本中的#!行将是命令解释器（**sh**或**bash**）首先看到的内容。由于这一行以#开头，当命令解释器最终执行脚本时，它将被正确地解释为注释。这一行已经完成了它的任务--调用命令解释器。如果脚本中确实包含一个*额外*的#!行，那么**bash**将把它解释为注释。

| [```sh #!/bin/bash

echo "Part 1 of script."
a=1

#!/bin/bash
# This does *not* launch a new script.

echo "Part 2 of script."
echo $a  # Value of $a stays at 1.
```] | 

|

| [[4]](sha-bang.html#AEN242) | 这允许一些巧妙的技巧。

| [```sh #!/bin/rm
# Self-deleting script.

# Nothing much seems to happen when you run this... except that the file disappears.

WHATEVER=85

echo "This line will never print (betcha!)."

exit $WHATEVER  # Doesn't matter. The script will not exit here.
                # Try an echo $? after script termination.
                # You'll get a 0, not a 85.
```] | 

此外，尝试以`**#!/bin/more**`开始一个`README`文件，并使其可执行。结果是自列文档文件。（使用 cat 的 here document 可能是更好的替代方案--见示例 19-3）。|

| [[5]](sha-bang.html#AEN256) | **P**ortable **O**perating **S**ystem *I*nterface，试图标准化类似 UNIX 的操作系统。POSIX 规范列在[Open Group 网站](http://www.opengroup.org/onlinepubs/007904975/toc.htm)上。 |
| --- | --- |
| [[6]](sha-bang.html#AEN269) | 为了避免这种可能性，脚本可能以一个#!/bin/env bash *sha-bang*行开始。在*bash*不在`/bin`目录的 UNIX 机器上，这可能很有用。 |
| [[7]](sha-bang.html#AEN279) | 如果 *Bash* 是您的默认 shell，那么脚本开头不需要 #!。然而，如果您从不同的 shell 中启动脚本，例如 *tcsh*，那么您*将需要* #!。 |
