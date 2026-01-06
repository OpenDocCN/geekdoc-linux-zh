# 第三十二章\. 调试

> 原文：[`tldp.org/LDP/abs/html/debugging.html`](https://tldp.org/LDP/abs/html/debugging.html)

|   |  **调试比最初编写代码难两倍。因此，如果你尽可能聪明地编写代码，那么按照定义，你不够聪明来调试它。**

*--Brian Kernighan**  |

Bash shell 不包含内置的调试器，只有一些基本的调试特定命令和结构。脚本中的语法错误或明显的错误会导致难以理解的错误信息，这些信息在调试无法正常工作的脚本时通常没有帮助。

**示例 32-1\. 一个有缺陷的脚本**

```sh
#!/bin/bash
# ex74.sh

# This is a buggy script.
# Where, oh where is the error?

a=37

if [$a -gt 27 ]
then
  echo $a
fi  

exit $?   # 0! Why?
```

脚本的输出：

```sh
./ex74.sh: 37: command not found
```

上面的脚本有什么问题？提示：在 *if* 之后。

**示例 32-2\. 缺少的 [关键字**

```sh
#!/bin/bash
# missing-keyword.sh
# What error message will this script generate? And why?

for a in 1 2 3
do
  echo "$a"
# done     # Required keyword 'done' commented out in line 8.

exit 0     # Will not exit here!

# === #

# From command line, after script terminates:
  echo $?    # 2
```

脚本的输出：

```sh
missing-keyword.sh: line 10: syntax error: unexpected end of file

```

注意，错误信息不一定引用错误发生的行，而是 Bash 解释器最终意识到错误的行。

当报告语法错误的行号时，错误信息可能会忽略脚本中的注释行。

如果脚本执行了，但不符合预期呢？这是过于熟悉的逻辑错误。

**示例 32-3\. *test24*: 另一个有缺陷的脚本**

```sh
#!/bin/bash

#  This script is supposed to delete all filenames in current directory
#+ containing embedded spaces.
#  It doesn't work.
#  Why not?

badname=`ls &#124; grep ' '`

# Try this:
# echo "$badname"

rm "$badname"

exit 0
```

通过取消注释 `**echo "$badname"**` 行来找出示例 32-3 中有什么问题。回显语句有助于查看你期望的实际上是否得到。

在这个特定的情况下，`**rm "$badname"**` 不会给出期望的结果，因为 `$badname` 不应该被引号括起来。将其放在引号中确保 **rm** 只有一个参数（它将匹配一个文件名）。一个部分修复方法是取消 `$badname` 的引号，并将 `$IFS` 重置为只包含换行符，`**IFS=$'\n'**`。然而，有更简单的方法可以做到这一点。

```sh
# Correct methods of deleting filenames containing spaces.
rm *\ *
rm *" "*
rm *' '*
# Thank you. S.C.
```

总结有缺陷脚本的症状，

1.  它会因“语法错误”信息而崩溃，

1.  它运行了，但不符合预期（逻辑错误）。

1.  它运行了，按预期工作，但有一些讨厌的副作用（逻辑炸弹）。

调试无法正常工作的脚本的工具包括

1.  在脚本的关键点插入 echo 语句以跟踪变量，并在其他情况下提供一个正在发生的事情的快照。

    | ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 更好的是当 *调试* 开启时才回显的 **echo**。

    &#124;  ```sh ### debecho (debug-echo), by Stefano Falsetto ###
    ### Will echo passed parameters only if DEBUG is set to a value. ###
    debecho () {
      if [ ! -z "$DEBUG" ]; then
         echo "$1" >&2
         #         ^^^ to stderr
      fi
    }

    DEBUG=on
    Whatever=whatnot
    debecho $Whatever   # whatnot

    DEBUG=
    Whatever=notwhat
    debecho $Whatever   # (Will not echo.)
    ```  &#124;

    |

1.  使用 tee 过滤器在关键点检查进程或数据流。

1.  设置选项标志 `-n -v -x`

    `**sh -n scriptname**` 在实际运行脚本之前检查语法错误。这相当于在脚本中插入 `**set -n**` 或 `**set -o noexec**`。注意，某些类型的语法错误可能会绕过此检查。

    `**sh -v scriptname**` 在执行之前会回显每个命令。这相当于在脚本中插入 `**set -v**` 或 `**set -o verbose**`。

    `-n` 和 `-v` 标志配合使用效果很好。`**sh -nv scriptname**` 提供详细的语法检查。

    `**sh -x scriptname**` 以简化的方式回显每条命令的结果。这相当于在脚本中插入 `**set -x**` 或 `**set -o xtrace**`。

    在脚本中插入 `**set -u**` 或 `**set -o nounset**` 并运行它，但会显示未绑定变量的错误消息并终止脚本。

    ```sh
    set -u   # Or   set -o nounset

    # Setting a variable to null will not trigger the error/abort.
    # unset_var=

    echo $unset_var   # Unset (and undeclared) variable.

    echo "Should not echo!"

    # sh t2.sh
    # t2.sh: line 6: unset_var: unbound variable
    ```

1.  使用“assert”函数在脚本的关键点测试变量或条件。（这是从 C 中借鉴的一个想法。）

    **示例 32-4\. 使用 *assert* 测试条件**

    ```sh
    #!/bin/bash
    # assert.sh

    #######################################################################
    assert ()                 #  If condition false,
    {                         #+ exit from script
                              #+ with appropriate error message.
      E_PARAM_ERR=98
      E_ASSERT_FAILED=99

      if [ -z "$2" ]          #  Not enough parameters passed
      then                    #+ to assert() function.
        return $E_PARAM_ERR   #  No damage done.
      fi

      lineno=$2

      if [ ! $1 ] 
      then
        echo "Assertion failed:  \"$1\""
        echo "File \"$0\", line $lineno"    # Give name of file and line number.
        exit $E_ASSERT_FAILED
      # else
      #   return
      #   and continue executing the script.
      fi  
    } # Insert a similar assert() function into a script you need to debug.    
    #######################################################################

    a=5
    b=4
    condition="$a -lt $b"     #  Error message and exit from script.
                              #  Try setting "condition" to something else
                              #+ and see what happens.

    assert "$condition" $LINENO
    # The remainder of the script executes only if the "assert" does not fail.

    # Some commands.
    # Some more commands . . .
    echo "This statement echoes only if the \"assert\" does not fail."
    # . . .
    # More commands . . .

    exit $?
    ```

1.  使用 $LINENO 变量和 caller 内置函数。

1.  退出时的捕获。

    脚本中的 exit 命令触发信号 0，终止进程，即脚本本身。 [[1]](#FTN.AEN19460) 通常情况下，捕获 `exit`，强制变量“打印”，例如。`trap` 必须是脚本中的第一个命令。

**捕获信号**

**trap**

在收到信号时指定动作；也适用于调试。

|

*信号* 是发送给进程的消息，无论是内核还是另一个进程，告诉它执行某些指定的动作（通常是为了终止）。例如，按下 Control-C 会发送用户中断，即 INT 信号，给正在运行的程序。

|

*一个简单的例子：*

```sh
trap '' 2
# Ignore interrupt 2 (Control-C), with no action specified. 

trap 'echo "Control-C disabled."' 2
# Message when Control-C pressed.
```

**示例 32-5\. 退出时的捕获**

```sh
#!/bin/bash
# Hunting variables with a trap.

trap 'echo Variable Listing --- a = $a  b = $b' EXIT
#  EXIT is the name of the signal generated upon exit from a script.
#
#  The command specified by the "trap" doesn't execute until
#+ the appropriate signal is sent.

echo "This prints before the \"trap\" --"
echo "even though the script sees the \"trap\" first."
echo

a=39

b=36

exit 0
#  Note that commenting out the 'exit' command makes no difference,
#+ since the script exits in any case after running out of commands.
```

**示例 32-6\. 处理 **Control-C** 后的清理**

```sh
#!/bin/bash
# logon.sh: A quick 'n dirty script to check whether you are on-line yet.

umask 177  # Make sure temp files are not world readable.

TRUE=1
LOGFILE=/var/log/messages
#  Note that $LOGFILE must be readable
#+ (as root, chmod 644 /var/log/messages).
TEMPFILE=temp.$$
#  Create a "unique" temp file name, using process id of the script.
#     Using 'mktemp' is an alternative.
#     For example:
#     TEMPFILE=`mktemp temp.XXXXXX`
KEYWORD=address
#  At logon, the line "remote IP address xxx.xxx.xxx.xxx"
#                      appended to /var/log/messages.
ONLINE=22
USER_INTERRUPT=13
CHECK_LINES=100
#  How many lines in log file to check.

trap 'rm -f $TEMPFILE; exit $USER_INTERRUPT' TERM INT
#  Cleans up the temp file if script interrupted by control-c.

echo

while [ $TRUE ]  #Endless loop.
do
  tail -n $CHECK_LINES $LOGFILE> $TEMPFILE
  #  Saves last 100 lines of system log file as temp file.
  #  Necessary, since newer kernels generate many log messages at log on.
  search=`grep $KEYWORD $TEMPFILE`
  #  Checks for presence of the "IP address" phrase,
  #+ indicating a successful logon.

  if [ ! -z "$search" ] #  Quotes necessary because of possible spaces.
  then
     echo "On-line"
     rm -f $TEMPFILE    #  Clean up temp file.
     exit $ONLINE
  else
     echo -n "."        #  The -n option to echo suppresses newline,
                        #+ so you get continuous rows of dots.
  fi

  sleep 1  
done  

#  Note: if you change the KEYWORD variable to "Exit",
#+ this script can be used while on-line
#+ to check for an unexpected logoff.

# Exercise: Change the script, per the above note,
#           and prettify it.

exit 0

# Nick Drage suggests an alternate method:

while true
  do ifconfig ppp0 &#124; grep UP 1> /dev/null && echo "connected" && exit 0
  echo -n "."   # Prints dots (.....) until connected.
  sleep 2
done

# Problem: Hitting Control-C to terminate this process may be insufficient.
#+         (Dots may keep on echoing.)
# Exercise: Fix this.

# Stephane Chazelas has yet another alternative:

CHECK_INTERVAL=1

while ! tail -n 1 "$LOGFILE" &#124; grep -q "$KEYWORD"
do echo -n .
   sleep $CHECK_INTERVAL
done
echo "On-line"

# Exercise: Discuss the relative strengths and weaknesses
#           of each of these various approaches.
```

**示例 32-7\. 进度条的简单实现**

```sh
#! /bin/bash
# progress-bar2.sh
# Author: Graham Ewart (with reformatting by ABS Guide author).
# Used in ABS Guide with permission (thanks!).

# Invoke this script with bash. It doesn't work with sh.

interval=1
long_interval=10

{
     trap "exit" SIGUSR1
     sleep $interval; sleep $interval
     while true
     do
       echo -n '.'     # Use dots.
       sleep $interval
     done; } &         # Start a progress bar as a background process.

pid=$!
trap "echo !; kill -USR1 $pid; wait $pid"  EXIT        # To handle ^C.

echo -n 'Long-running process '
sleep $long_interval
echo ' Finished!'

kill -USR1 $pid
wait $pid              # Stop the progress bar.
trap EXIT

exit $?
```

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | `DEBUG` 参数传递给 `trap` 会使得在脚本中的每条命令执行后执行指定的动作。这允许跟踪变量，例如。

**示例 32-8\. 跟踪一个变量**

&#124;  ```sh #!/bin/bash

trap 'echo "VARIABLE-TRACE> \$variable = \"$variable\""' DEBUG
# Echoes the value of $variable after every command.

variable=29; line=$LINENO

echo "  Just initialized \$variable to $variable in line number $line."

let "variable *= 3"; line=$LINENO
echo "  Just multiplied \$variable by 3 in line number $line."

exit 0

#  The "trap 'command1 . . . command2 . . .' DEBUG" construct is
#+ more appropriate in the context of a complex script,
#+ where inserting multiple "echo $variable" statements might be
#+ awkward and time-consuming.

# Thanks, Stephane Chazelas for the pointer.

Output of script:

VARIABLE-TRACE> $variable = ""
VARIABLE-TRACE> $variable = "29"
  Just initialized $variable to 29.
VARIABLE-TRACE> $variable = "29"
VARIABLE-TRACE> $variable = "87"
  Just multiplied $variable by 3.
VARIABLE-TRACE> $variable = "87"
```  &#124;

|

当然，`trap` 命令除了调试之外还有其他用途，例如在脚本中禁用某些按键（参见 示例 A-43）。

**示例 32-9\. 运行多个进程（在 SMP 箱子上）**

```sh
#!/bin/bash
# parent.sh
# Running multiple processes on an SMP box.
# Author: Tedman Eng

#  This is the first of two scripts,
#+ both of which must be present in the current working directory.

LIMIT=$1         # Total number of process to start
NUMPROC=4        # Number of concurrent threads (forks?)
PROCID=1         # Starting Process ID
echo "My PID is $$"

function start_thread() {
        if [ $PROCID -le $LIMIT ] ; then
                ./child.sh $PROCID&
                let "PROCID++"
        else
           echo "Limit reached."
           wait
           exit
        fi
}

while [ "$NUMPROC" -gt 0 ]; do
        start_thread;
        let "NUMPROC--"
done

while true
do

trap "start_thread" SIGRTMIN

done

exit 0

# ======== Second script follows ========

#!/bin/bash
# child.sh
# Running multiple processes on an SMP box.
# This script is called by parent.sh.
# Author: Tedman Eng

temp=$RANDOM
index=$1
shift
let "temp %= 5"
let "temp += 4"
echo "Starting $index  Time:$temp" "$@"
sleep ${temp}
echo "Ending $index"
kill -s SIGRTMIN $PPID

exit 0

# ======================= SCRIPT AUTHOR'S NOTES ======================= #
#  It's not completely bug free.
#  I ran it with limit = 500 and after the first few hundred iterations,
#+ one of the concurrent threads disappeared!
#  Not sure if this is collisions from trap signals or something else.
#  Once the trap is received, there's a brief moment while executing the
#+ trap handler but before the next trap is set.  During this time, it may
#+ be possible to miss a trap signal, thus miss spawning a child process.

#  No doubt someone may spot the bug and will be writing 
#+ . . . in the future.

# ===================================================================== #

# ----------------------------------------------------------------------#

#################################################################
# The following is the original script written by Vernia Damiano.
# Unfortunately, it doesn't work properly.
#################################################################

#!/bin/bash

#  Must call script with at least one integer parameter
#+ (number of concurrent processes).
#  All other parameters are passed through to the processes started.

INDICE=8        # Total number of process to start
TEMPO=5         # Maximum sleep time per process
E_BADARGS=65    # No arg(s) passed to script.

if [ $# -eq 0 ] # Check for at least one argument passed to script.
then
  echo "Usage: `basename $0` number_of_processes [passed params]"
  exit $E_BADARGS
fi

NUMPROC=$1              # Number of concurrent process
shift
PARAMETRI=( "$@" )      # Parameters of each process

function avvia() {
         local temp
         local index
         temp=$RANDOM
         index=$1
         shift
         let "temp %= $TEMPO"
         let "temp += 1"
         echo "Starting $index Time:$temp" "$@"
         sleep ${temp}
         echo "Ending $index"
         kill -s SIGRTMIN $$
}

function parti() {
         if [ $INDICE -gt 0 ] ; then
              avvia $INDICE "${PARAMETRI[@]}" &
                let "INDICE--"
         else
                trap : SIGRTMIN
         fi
}

trap parti SIGRTMIN

while [ "$NUMPROC" -gt 0 ]; do
         parti;
         let "NUMPROC--"
done

wait
trap - SIGRTMIN

exit $?

: <<SCRIPT_AUTHOR_COMMENTS
I had the need to run a program, with specified options, on a number of
different files, using a SMP machine. So I thought [I'd] keep running
a specified number of processes and start a new one each time . . . one
of these terminates.

The "wait" instruction does not help, since it waits for a given process
or *all* process started in background. So I wrote [this] bash script
that can do the job, using the "trap" instruction.
  --Vernia Damiano
SCRIPT_AUTHOR_COMMENTS
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | `**trap '' SIGNAL**`（两个相邻的单引号）禁用脚本剩余部分的 SIGNAL。`**trap SIGNAL**` 再次恢复 SIGNAL 的功能。这有助于保护脚本的关键部分免受不希望的干扰。 |
```sh
	trap '' 2  # Signal 2 is Control-C, now disabled.
	command
	command
	command
	trap 2     # Reenables Control-C

```  |

|

Bash 的 版本 3 为调试器添加了以下 内部变量 以供使用。

1.  `$BASH_ARGC`

    传递给脚本的命令行参数数量，类似于 `${#}`。

1.  `$BASH_ARGV`

    传递给脚本的最终命令行参数，相当于 `${!#}`。

1.  `$BASH_COMMAND`

    当前正在执行的命令。

1.  `$BASH_EXECUTION_STRING`

    Bash 中 `-c` 选项 后跟的 *选项字符串*。

1.  `$BASH_LINENO`

    在一个函数中，表示函数调用的行号。

1.  `$BASH_REMATCH`

    与 **=~** 条件正则表达式匹配 相关的数组变量。

1.  `$BASH_SOURCE`

    这是脚本的名称，通常与 $0 相同。

1.  `$BASH_SUBSHELL`

|

### 备注

| [[1]](debugging.html#AEN19460) | 按照惯例，`*信号 0*` 被分配给 退出 命令。 |
| --- | --- |
