# 第二十一章。子 shell

> 原文：[`tldp.org/LDP/abs/html/subshells.html`](https://tldp.org/LDP/abs/html/subshells.html)

运行 shell 脚本会启动一个新的进程，一个*子 shell*。

|

`**定义：**` 一个*子 shell*是由 shell（或*shell 脚本*）启动的子进程。

|

子 shell 是命令处理器的独立实例——在控制台或*xterm*窗口中给你提示的*shell*。就像你的命令在命令行提示符下被解释一样，脚本也会批量处理一系列命令。每个运行的 shell 脚本实际上都是父 shell 的子进程（*子进程*）。

一个 shell 脚本本身可以启动子进程。这些*子 shell*允许脚本进行并行处理，实际上同时执行多个子任务。

```sh
#!/bin/bash
# subshell-test.sh

(
# Inside parentheses, and therefore a subshell . . .
while [ 1 ]   # Endless loop.
do
  echo "Subshell running . . ."
done
)

#  Script will run forever,
#+ or at least until terminated by a Ctl-C.

exit $?  # End of script (but will never get here).

Now, run the script:
sh subshell-test.sh

And, while the script is running, from a different xterm:
ps -ef &#124; grep subshell-test.sh

UID       PID   PPID  C STIME TTY      TIME     CMD
500       2698  2502  0 14:26 pts/4    00:00:00 sh subshell-test.sh
500       2699  2698 21 14:26 pts/4    00:00:24 sh subshell-test.sh

          ^^^^

Analysis:
PID 2698, the script, launched PID 2699, the subshell.

Note: The "UID ..." line would be filtered out by the "grep" command,
but is shown here for illustrative purposes.
```

通常，脚本中的外部命令会派生一个子进程，[[1]](#FTN.AEN18102)，而 Bash 的内建命令则不会。因此，内建命令比它们的外部命令等效物执行得更快，使用的系统资源也更少。

**括号内的命令列表**

( command1; command2; command3; ... )

嵌入在`*括号*`之间的命令列表作为子 shell 运行。

子 shell 中的变量*不在*子 shell 代码块外部可见。它们不能被父进程或启动子 shell 的 shell 访问。实际上，这些是*子进程*的局部变量。

**示例 21-1\. 子 shell 中的变量作用域**

```sh
#!/bin/bash
# subshell.sh

echo

echo "We are outside the subshell."
echo "Subshell level OUTSIDE subshell = $BASH_SUBSHELL"
# Bash, version 3, adds the new         $BASH_SUBSHELL variable.
echo; echo

outer_variable=Outer
global_variable=
#  Define global variable for "storage" of
#+ value of subshell variable.

(
echo "We are inside the subshell."
echo "Subshell level INSIDE subshell = $BASH_SUBSHELL"
inner_variable=Inner

echo "From inside subshell, \"inner_variable\" = $inner_variable"
echo "From inside subshell, \"outer\" = $outer_variable"

global_variable="$inner_variable"   #  Will this allow "exporting"
                                    #+ a subshell variable?
)

echo; echo
echo "We are outside the subshell."
echo "Subshell level OUTSIDE subshell = $BASH_SUBSHELL"
echo

if [ -z "$inner_variable" ]
then
  echo "inner_variable undefined in main body of shell"
else
  echo "inner_variable defined in main body of shell"
fi

echo "From main body of shell, \"inner_variable\" = $inner_variable"
#  $inner_variable will show as blank (uninitialized)
#+ because variables defined in a subshell are "local variables".
#  Is there a remedy for this?
echo "global_variable = "$global_variable""  # Why doesn't this work?

echo

# =======================================================================

# Additionally ...

echo "-----------------"; echo

var=41                                                 # Global variable.

( let "var+=1"; echo "\$var INSIDE subshell = $var" )  # 42

echo "\$var OUTSIDE subshell = $var"                   # 41
#  Variable operations inside a subshell, even to a GLOBAL variable
#+ do not affect the value of the variable outside the subshell!

exit 0

#  Question:
#  --------
#  Once having exited a subshell,
#+ is there any way to reenter that very same subshell
#+ to modify or access the subshell variables?
```

参见$BASHPID 和示例 34-2。

|

`**定义：**` 变量的*作用域*是它有意义的上下文，其中它有一个*值*可以被引用。例如，局部变量作用域仅限于定义它的函数、代码块或子 shell 内部，而全局变量的作用域是它出现的整个脚本。

|

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 虽然$BASH_SUBSHELL 内部变量指示子 shell 的嵌套级别，但$SHLVL 变量在子 shell 内部*没有变化*。

&#124;  ```sh echo " \$BASH_SUBSHELL outside subshell       = $BASH_SUBSHELL"           # 0
  ( echo " \$BASH_SUBSHELL inside subshell        = $BASH_SUBSHELL" )     # 1
  ( ( echo " \$BASH_SUBSHELL inside nested subshell = $BASH_SUBSHELL" ) ) # 2
# ^ ^                           *** nested ***                        ^ ^

echo

echo " \$SHLVL outside subshell = $SHLVL"       # 3
( echo " \$SHLVL inside subshell  = $SHLVL" )   # 3 (No change!)
```  &#124;

|

在子 shell 中做出的目录更改不会传递到父 shell。

**示例 21-2\. 列出用户配置文件**

```sh
#!/bin/bash
# allprofs.sh: Print all user profiles.

# This script written by Heiner Steven, and modified by the document author.

FILE=.bashrc  #  File containing user profile,
              #+ was ".profile" in original script.

for home in `awk -F: '{print $6}' /etc/passwd`
do
  [ -d "$home" ] &#124;&#124; continue    # If no home directory, go to next.
  [ -r "$home" ] &#124;&#124; continue    # If not readable, go to next.
  (cd $home; [ -e $FILE ] && less $FILE)
done

#  When script terminates, there is no need to 'cd' back to original directory,
#+ because 'cd $home' takes place in a subshell.

exit 0
```

子 shell 可以用来为命令组设置一个“专用环境”。

```sh
COMMAND1
COMMAND2
COMMAND3
(
  IFS=:
  PATH=/bin
  unset TERMINFO
  set -C
  shift 5
  COMMAND4
  COMMAND5
  exit 3 # Only exits the subshell!
)
# The parent shell has not been affected, and the environment is preserved.
COMMAND6
COMMAND7
```

如此看来，退出命令只终止它正在运行的子 shell，*不会*终止父 shell 或脚本。

这种“专用环境”的一个应用是测试变量是否已定义。

```sh
if (set -u; : $variable) 2> /dev/null
then
  echo "Variable is set."
fi     #  Variable has been set in current script,
       #+ or is an an internal Bash variable,
       #+ or is present in environment (has been exported).

# Could also be written [[ ${variable-x} != x &#124;&#124; ${variable-y} != y ]]
# or                    [[ ${variable-x} != x$variable ]]
# or                    [[ ${variable+x} = x ]]
# or                    [[ ${variable-x} != x ]]
```

另一个应用是检查锁文件：

```sh
if (set -C; : > lock_file) 2> /dev/null
then
  :   # lock_file didn't exist: no user running the script
else
  echo "Another user is already running that script."
exit 65
fi

#  Code snippet by Stphane Chazelas,
#+ with modifications by Paulo Marcel Coelho Aragao.
```

+

在不同的子壳中，进程可能可以并行执行。这允许将复杂任务分解为可以并行处理的子组件。

**示例 21-3\. 在子壳中运行并行进程**

```sh
	(cat list1 list2 list3 &#124; sort &#124; uniq > list123) &
	(cat list4 list5 list6 &#124; sort &#124; uniq > list456) &
	# Merges and sorts both sets of lists simultaneously.
	# Running in background ensures parallel execution.
	#
	# Same effect as
	#   cat list1 list2 list3 &#124; sort &#124; uniq > list123 &
	#   cat list4 list5 list6 &#124; sort &#124; uniq > list456 &

	wait   # Don't execute the next command until subshells finish.

	diff list123 list456
```

将 I/O 重定向到子壳使用 "|" 管道操作符，例如 `**ls -al | (command)**`。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 在 花括号 之间的代码块 *不会* 启动子壳。{ command1; command2; command3; . . . commandN; }

&#124;  ```sh var1=23
echo "$var1"   # 23

{ var1=76; }
echo "$var1"   # 76
```  &#124;

|

### 备注

| [[1]](subshells.html#AEN18102) | 使用 exec 调用的外部命令 *通常* 不会（通常）派生子进程/子壳。 |
| --- | --- |
