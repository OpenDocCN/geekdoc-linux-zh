# 第十五章. 内部命令和内置命令

> 原文：[`tldp.org/LDP/abs/html/internal.html`](https://tldp.org/LDP/abs/html/internal.html)

一个*内置命令*是包含在 Bash 工具集中的**命令**，字面上是*内置的*。这可能是出于性能原因——内置命令比通常需要*派生*（[[1]](#FTN.AEN8607)一个单独的进程的外部命令）执行得更快——或者因为某个特定的内置命令需要直接访问 shell 内部。

|

当一个命令或 shell 本身启动（或*生成*）一个新的子进程来执行一个任务时，这被称为*派生*。这个新进程是*子进程*，而派生它的*父进程*仍在执行。

注意，虽然*父进程*会得到*子进程*的*进程 ID*，因此可以向它传递参数，但*反之则不然*。这可能会造成微妙且难以追踪的问题。

**示例 15-1. 生成多个自身实例的脚本**

&#124;  ```sh #!/bin/bash
# spawn.sh

PIDS=$(pidof sh $0)  # Process IDs of the various instances of this script.
P_array=( $PIDS )    # Put them in an array (why?).
echo $PIDS           # Show process IDs of parent and child processes.
let "instances = ${#P_array[*]} - 1"  # Count elements, less 1.
                                      # Why subtract 1?
echo "$instances instance(s) of this script running."
echo "[Hit Ctl-C to exit.]"; echo

sleep 1              # Wait.
sh $0                # Play it again, Sam.

exit 0               # Not necessary; script will never get to here.
                     # Why not?

#  After exiting with a Ctl-C,
#+ do all the spawned instances of the script die?
#  If so, why?

# Note:
# ----
# Be careful not to run this script too long.
# It will eventually eat up too many system resources.

#  Is having a script spawn multiple instances of itself
#+ an advisable scripting technique.
#  Why or why not?
```  &#124;

通常情况下，Bash 内置命令在脚本中执行时不会派生子进程。脚本中的外部系统命令或过滤器通常*会*派生子进程。

|

一个内置命令可能是一个同义词，与同名的系统命令，但 Bash 内部重新实现了它。例如，Bash 的**echo**命令与**/bin/echo**不同，尽管它们的行为几乎相同。

```sh
#!/bin/bash

echo "This line uses the \"echo\" builtin."
/bin/echo "This line uses the /bin/echo system command."
```

一个*关键字*是一个*保留*的词、标记或运算符。关键字对 shell 有特殊的意义，实际上构成了 shell 语法的构建块。例如，*for*、*while*、*do*和*!*都是关键字。与内置命令类似，关键字是硬编码到 Bash 中的，但与*内置命令*不同，关键字本身不是一个命令，而是*命令构造的子单元*。[[2]](#FTN.AEN8650)

**I/O**

**echo**

打印（到`stdout`）一个表达式或变量（参见示例 4-1）。

```sh
echo Hello
echo $a
```

要打印转义字符，**echo**需要`-e`选项。参见示例 5-2。

通常，每个**echo**命令都会打印一个终端换行符，但`-n`选项会抑制这个行为。

| ![Note](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 可以使用**echo**将一系列命令传递到管道中。

&#124;  ```sh if echo "$VAR" &#124; grep -q txt   # if [[ $VAR = *txt* ]]
then
  echo "$VAR contains the substring sequence \"txt\""
fi
```  &#124;

|

| ![Note](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 结合命令替换，**echo**可以设置变量。``**a=`echo "HELLO" &#124; tr A-Z a-z`**``参见示例 16-22、示例 16-3、示例 16-47 和示例 16-48。 |
| --- | --- |

注意，**echo `command`**会删除`*command*`输出产生的任何换行符。

$IFS（内部字段分隔符）变量通常包含 \n（换行符）作为其集合中的 空白字符 之一。因此，Bash 会将 `*command*` 的输出在换行符处拆分为 **echo** 的参数。然后 **echo** 输出这些参数，并用空格分隔。

```sh
bash$ **ls -l /usr/share/apps/kjezz/sounds**
-rw-r--r--    1 root     root         1407 Nov  7  2000 reflect.au
 -rw-r--r--    1 root     root          362 Nov  7  2000 seconds.au

bash$ **echo `ls -l /usr/share/apps/kjezz/sounds`**
total 40 -rw-r--r-- 1 root root 716 Nov 7 2000 reflect.au -rw-r--r-- 1 root root ...

```

那么，我们如何在 echoed 字符串中嵌入换行符呢？

```sh
# Embedding a linefeed?
echo "Why doesn't this string \n split on two lines?"
# Doesn't split.

# Let's try something else.

echo

echo $"A line of text containing
a linefeed."
# Prints as two distinct lines (embedded linefeed).
# But, is the "$" variable prefix really necessary?

echo

echo "This string splits
on two lines."
# No, the "$" is not needed.

echo
echo "---------------"
echo

echo -n $"Another line of text containing
a linefeed."
# Prints as two distinct lines (embedded linefeed).
# Even the -n option fails to suppress the linefeed here.

echo
echo
echo "---------------"
echo
echo

# However, the following doesn't work as expected.
# Why not? Hint: Assignment to a variable.
string1=$"Yet another line of text containing
a linefeed (maybe)."

echo $string1
# Yet another line of text containing a linefeed (maybe).
#                                    ^
# Linefeed becomes a space.

# Thanks, Steve Parker, for pointing this out.
```

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 此命令是 shell 内置命令，与 `/bin/echo` 不同，尽管其行为相似。

&#124;  ```sh bash$ **type -a echo**
echo is a shell builtin
 echo is /bin/echo

```  &#124;

|

**printf**

**printf**，格式化打印命令，是 **echo** 的增强版。它是 *C* 语言 `printf()` 库函数的有限变体，其语法略有不同。

**printf** `*format-string*`... `*parameter*`...

这是 Bash 的 *内置* 版本的 `/bin/printf` 或 `/usr/bin/printf` 命令。有关系统命令的深入内容，请参阅 **printf** 手册页。

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 旧版本的 Bash 可能不支持 **printf**。 |
| --- | --- |

**示例 15-2\. **printf** 的实际应用**

```sh
#!/bin/bash
# printf demo

declare -r PI=3.14159265358979     # Read-only variable, i.e., a constant.
declare -r DecimalConstant=31373

Message1="Greetings,"
Message2="Earthling."

echo

printf "Pi to 2 decimal places = %1.2f" $PI
echo
printf "Pi to 9 decimal places = %1.9f" $PI  # It even rounds off correctly.

printf "\n"                                  # Prints a line feed,
                                             # Equivalent to 'echo' . . .

printf "Constant = \t%d\n" $DecimalConstant  # Inserts tab (\t).

printf "%s %s \n" $Message1 $Message2

echo

# ==========================================#
# Simulation of C function, sprintf().
# Loading a variable with a formatted string.

echo 

Pi12=$(printf "%1.12f" $PI)
echo "Pi to 12 decimal places = $Pi12"      # Roundoff error!

Msg=`printf "%s %s \n" $Message1 $Message2`
echo $Msg; echo $Msg

#  As it happens, the 'sprintf' function can now be accessed
#+ as a loadable module to Bash,
#+ but this is not portable.

exit 0
```

格式化错误信息是 **printf** 的一个有用应用。

```sh
E_BADDIR=85

var=nonexistent_directory

error()
{
  printf "$@" >&2
  # Formats positional params passed, and sends them to stderr.
  echo
  exit $E_BADDIR
}

cd $var &#124;&#124; error $"Can't cd to %s." "$var"

# Thanks, S.C.
```

参见 示例 36-17。

**read**

“从 `stdin` 读取变量的值”，即交互式地从键盘获取输入。`-a` 选项允许 **read** 获取数组变量（参见 示例 27-6）。

**示例 15-3\. 变量赋值，使用 **read****

```sh
#!/bin/bash
# "Reading" variables.

echo -n "Enter the value of variable 'var1': "
# The -n option to echo suppresses newline.

read var1
# Note no '$' in front of var1, since it is being set.

echo "var1 = $var1"

echo

# A single 'read' statement can set multiple variables.
echo -n "Enter the values of variables 'var2' and 'var3' "
echo =n "(separated by a space or tab): "
read var2 var3
echo "var2 = $var2      var3 = $var3"
#  If you input only one value,
#+ the other variable(s) will remain unset (null).

exit 0
```

没有相关变量的 **read** 将其输入分配给专用变量 $REPLY。

**示例 15-4\. 当 **read** 没有变量时会发生什么**

```sh
#!/bin/bash
# read-novar.sh

echo

# -------------------------- #
echo -n "Enter a value: "
read var
echo "\"var\" = "$var""
# Everything as expected here.
# -------------------------- #

echo

# ------------------------------------------------------------------- #
echo -n "Enter another value: "
read           #  No variable supplied for 'read', therefore...
               #+ Input to 'read' assigned to default variable, $REPLY.
var="$REPLY"
echo "\"var\" = "$var""
# This is equivalent to the first code block.
# ------------------------------------------------------------------- #

echo
echo "========================="
echo

#  This example is similar to the "reply.sh" script.
#  However, this one shows that $REPLY is available
#+ even after a 'read' to a variable in the conventional way.

# ================================================================= #

#  In some instances, you might wish to discard the first value read.
#  In such cases, simply ignore the $REPLY variable.

{ # Code block.
read            # Line 1, to be discarded.
read line2      # Line 2, saved in variable.
  } <$0
echo "Line 2 of this script is:"
echo "$line2"   #   # read-novar.sh
echo            #   #!/bin/bash  line discarded.

# See also the soundcard-on.sh script.

exit 0
```

通常，输入一个 `**\**` 会抑制对 **read** 的输入中的换行符。`-r` 选项会导致输入的 `**\**` 被字面地解释。

**示例 15-5\. **read** 的多行输入**

```sh
#!/bin/bash

echo

echo "Enter a string terminated by a \\, then press <ENTER>."
echo "Then, enter a second string (no \\ this time), and again press <ENTER>."

read var1     # The "\" suppresses the newline, when reading $var1.
              #     first line \
              #     second line

echo "var1 = $var1"
#     var1 = first line second line

#  For each line terminated by a "\"
#+ you get a prompt on the next line to continue feeding characters into var1.

echo; echo

echo "Enter another string terminated by a \\ , then press <ENTER>."
read -r var2  # The -r option causes the "\" to be read literally.
              #     first line \

echo "var2 = $var2"
#     var2 = first line \

# Data entry terminates with the first <ENTER>.

echo 

exit 0
```

**read** 命令有一些有趣的可选参数，允许回显提示并甚至读取按键而不按 **ENTER**。

```sh
# Read a keypress without hitting ENTER.

read -s -n1 -p "Hit a key " keypress
echo; echo "Keypress was "\"$keypress\""."

# -s option means do not echo input.
# -n N option means accept only N characters of input.
# -p option means echo the following prompt before reading input.

# Using these options is tricky, since they need to be in the correct order.
```

`-n` 选项对 **read** 命令也允许检测 **箭头键** 和某些其他不寻常的键。

**示例 15-6\. 检测箭头键**

```sh
#!/bin/bash
# arrow-detect.sh: Detects the arrow keys, and a few more.
# Thank you, Sandro Magi, for showing me how.

# --------------------------------------------
# Character codes generated by the keypresses.
arrowup='\[A'
arrowdown='\[B'
arrowrt='\[C'
arrowleft='\[D'
insert='\[2'
delete='\[3'
# --------------------------------------------

SUCCESS=0
OTHER=65

echo -n "Press a key...  "
# May need to also press ENTER if a key not listed above pressed.
read -n3 key                      # Read 3 characters.

echo -n "$key" &#124; grep "$arrowup"  #Check if character code detected.
if [ "$?" -eq $SUCCESS ]
then
  echo "Up-arrow key pressed."
  exit $SUCCESS
fi

echo -n "$key" &#124; grep "$arrowdown"
if [ "$?" -eq $SUCCESS ]
then
  echo "Down-arrow key pressed."
  exit $SUCCESS
fi

echo -n "$key" &#124; grep "$arrowrt"
if [ "$?" -eq $SUCCESS ]
then
  echo "Right-arrow key pressed."
  exit $SUCCESS
fi

echo -n "$key" &#124; grep "$arrowleft"
if [ "$?" -eq $SUCCESS ]
then
  echo "Left-arrow key pressed."
  exit $SUCCESS
fi

echo -n "$key" &#124; grep "$insert"
if [ "$?" -eq $SUCCESS ]
then
  echo "\"Insert\" key pressed."
  exit $SUCCESS
fi

echo -n "$key" &#124; grep "$delete"
if [ "$?" -eq $SUCCESS ]
then
  echo "\"Delete\" key pressed."
  exit $SUCCESS
fi

echo " Some other key pressed."

exit $OTHER

# ========================================= #

#  Mark Alexander came up with a simplified
#+ version of the above script (Thank you!).
#  It eliminates the need for grep.

#!/bin/bash

  uparrow=$'\x1bA'
  downarrow=$'\x1b[B'
  leftarrow=$'\x1b[D'
  rightarrow=$'\x1b[C'

  read -s -n3 -p "Hit an arrow key: " x

  case "$x" in
  $uparrow)
     echo "You pressed up-arrow"
     ;;
  $downarrow)
     echo "You pressed down-arrow"
     ;;
  $leftarrow)
     echo "You pressed left-arrow"
     ;;
  $rightarrow)
     echo "You pressed right-arrow"
     ;;
  esac

exit $?

# ========================================= #

# Antonio Macchi has a simpler alternative.

#!/bin/bash

while true
do
  read -sn1 a
  test "$a" == `echo -en "\e"` &#124;&#124; continue
  read -sn1 a
  test "$a" == "[" &#124;&#124; continue
  read -sn1 a
  case "$a" in
    A)  echo "up";;
    B)  echo "down";;
    C)  echo "right";;
    D)  echo "left";;
  esac
done

# ========================================= #

#  Exercise:
#  --------
#  1) Add detection of the "Home," "End," "PgUp," and "PgDn" keys.
```
| ![注意 | **read** 的 `-n` 选项不会检测 **ENTER**（换行符）键。 |**read** 的 `-t` 选项允许定时输入（参见 示例 9-4 和 示例 A-41）。`-u` 选项接受目标文件的 文件描述符。**read** 命令也可能从重定向到 `stdin` 的文件中“读取”其变量的值 重定向。如果文件包含多于一行，只有第一行会被分配给变量。如果 **read** 有多个参数，那么这些变量中的每一个都会分配一个连续的 空白分隔 字符串。注意！**示例 15-7\. 使用 *read* 与 文件重定向**```sh
#!/bin/bashread var1 <data-fileecho "var1 = $var1"# var1 set to the entire first line of the input file "data-file"read var2 var3 <data-fileecho "var2 = $var2   var3 = $var3"# Note non-intuitive behavior of "read" here.# 1) Rewinds back to the beginning of input file.# 2) Each variable is now set to a corresponding string,#    separated by whitespace, rather than to an entire line of text.# 3) The final variable gets the remainder of the line.# 4) If there are more variables to be set than whitespace-terminated strings#    on the first line of the file, then the excess variables remain empty.echo "------------------------------------------------"# How to resolve the above problem with a loop:while read linedo  echo "$line"done <data-file# Thanks, Heiner Steven for pointing this out.echo "------------------------------------------------"# Use $IFS (Internal Field Separator variable) to split a line of input to# "read", if you do not want the default to be whitespace.echo "List of all users:"OIFS=$IFS; IFS=:       # /etc/passwd uses ":" for field separator.while read name passwd uid gid fullname ignoredo  echo "$name ($fullname)"done </etc/passwd   # I/O redirection.IFS=$OIFS              # Restore original $IFS.# This code snippet also by Heiner Steven.#  Setting the $IFS variable within the loop itself#+ eliminates the need for storing the original $IFS#+ in a temporary variable.#  Thanks, Dim Segebart, for pointing this out.echo "------------------------------------------------"echo "List of all users:"while IFS=: read name passwd uid gid fullname ignoredo  echo "$name ($fullname)"done </etc/passwd   # I/O redirection.echoecho "\$IFS still $IFS"exit 0```  || --- || ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 将输出通过 管道 输送到一个 *读取*，使用 echo 来设置变量 将会失败。然而，将 cat 的输出通过管道 *似乎* 可以工作。

&#124;  ```sh cat file1 file2 &#124;
while read line
do
echo $line
done
```  &#124;

然而，正如 Bjn Eriksson 所展示的：

**示例 15-8\. 从管道读取的问题**

&#124;  ```sh #!/bin/sh
# readpipe.sh
# This example contributed by Bjon Eriksson.

### shopt -s lastpipe

last="(null)"
cat $0 &#124;
while read line
do
    echo "{$line}"
    last=$line
done

echo
echo "++++++++++++++++++++++"
printf "\nAll done, last: $last\n" #  The output of this line
                                   #+ changes if you uncomment line 5.
                                   #  (Bash, version -ge 4.2 required.)

exit 0  # End of code.
        # (Partial) output of script follows.
        # The 'echo' supplies extra brackets.

#############################################

./readpipe.sh 

{#!/bin/sh}
{last="(null)"}
{cat $0 &#124;}
{while read line}
{do}
{echo "{$line}"}
{last=$line}
{done}
{printf "nAll done, last: $lastn"}

All done, last: (null)

The variable (last) is set within the loop/subshell
but its value does not persist outside the loop.
```  &#124;

*gendiff* 脚本通常在许多 Linux 发行版的 `/usr/bin` 中找到，它将 find 的输出通过管道输送到一个 *while read* 构造。

&#124;  ```sh find $1 \( -name "*$2" -o -name ".*$2" \) -print &#124;
while read f; do
. . .
```  &#124;

|

| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 可以将文本 *粘贴* 到 *read* 的输入字段中（但不能粘贴多行！）。参见 示例 A-38。 |
| --- | --- |

**文件系统**

**cd**

熟悉的 **cd** 改变目录命令在脚本中很有用，其中执行命令需要位于指定的目录中。

```sh
(cd /source/directory && tar cf - . ) &#124; (cd /dest/directory && tar xpvf -)
```

来自 [之前引用 的 Alan Cox 示例]

**cd** 的 `-P`（物理）选项会导致它忽略符号链接。

**cd -** 将目录更改为 $OLDPWD，即上一个工作目录。

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 当遇到两个正斜杠时，**cd** 命令的行为可能不符合预期。

&#124;  ```sh bash$ **cd //**
bash$ **pwd**
//

```  &#124;

当然，输出应该是 `/`。这在命令行和脚本中都是一个问题。|

**pwd**

打印工作目录。这会给出用户（或脚本）的当前目录（参见 示例 15-9）。效果等同于读取内置变量 $PWD 的值。

**pushd**, **popd**, **dirs**

这个命令集是一个用于标记工作目录的机制，是一种有序地在目录间来回移动的方法。使用一个下推 栈 来跟踪目录名称。选项允许对目录栈进行各种操作。

`**pushd dir-name**` 将路径 `*dir-name*` 压入目录栈（到栈的 *顶部*）并同时将当前工作目录更改为 `*dir-name*`

**popd** 从目录栈中移除（弹出）顶部目录路径名称，并同时将当前工作目录更改为栈顶的目录。

**dirs** 列出目录栈的内容（与 $DIRSTACK 变量进行比较）。成功的 **pushd** 或 **popd** 将自动调用 **dirs**。

需要对当前工作目录进行各种更改而不硬编码目录名称更改的脚本可以利用这些命令。请注意，隐含的 `$DIRSTACK` 数组变量，可以从脚本内部访问，包含目录栈的内容。

**示例 15-9\. 更改当前工作目录**

```sh
#!/bin/bash

dir1=/usr/local
dir2=/var/spool

pushd $dir1
# Will do an automatic 'dirs' (list directory stack to stdout).
echo "Now in directory `pwd`." # Uses back-quoted 'pwd'.

# Now, do some stuff in directory 'dir1'.
pushd $dir2
echo "Now in directory `pwd`."

# Now, do some stuff in directory 'dir2'.
echo "The top entry in the DIRSTACK array is $DIRSTACK."
popd
echo "Now back in directory `pwd`."

# Now, do some more stuff in directory 'dir1'.
popd
echo "Now back in original working directory `pwd`."

exit 0

# What happens if you don't 'popd' -- then exit the script?
# Which directory do you end up in? Why?
```

**变量**

**let**

**let** 命令在变量上执行 *算术* 操作。 [[3]](#FTN.AEN9009) 在许多情况下，它作为一个更简单的 expr 版本。

**示例 15-10\. 让 *let* 执行算术运算。**

```sh
#!/bin/bash

echo

let a=11            # Same as 'a=11'
let a=a+5           # Equivalent to  let "a = a + 5"
                    # (Double quotes and spaces make it more readable.)
echo "11 + 5 = $a"  # 16

let "a <<= 3"       # Equivalent to  let "a = a << 3"
echo "\"\$a\" (=16) left-shifted 3 places = $a"
                    # 128

let "a /= 4"        # Equivalent to  let "a = a / 4"
echo "128 / 4 = $a" # 32

let "a -= 5"        # Equivalent to  let "a = a - 5"
echo "32 - 5 = $a"  # 27

let "a *=  10"      # Equivalent to  let "a = a * 10"
echo "27 * 10 = $a" # 270

let "a %= 8"        # Equivalent to  let "a = a % 8"
echo "270 modulo 8 = $a  (270 / 8 = 33, remainder $a)"
                    # 6

# Does "let" permit C-style operators?
# Yes, just as the (( ... )) double-parentheses construct does.

let a++             # C-style (post) increment.
echo "6++ = $a"     # 6++ = 7
let a--             # C-style decrement.
echo "7-- = $a"     # 7-- = 6
# Of course, ++a, etc., also allowed . . .
echo

# Trinary operator.

# Note that $a is 6, see above.
let "t = a<7?7:11"   # True
echo $t  # 7

let a++
let "t = a<7?7:11"   # False
echo $t  #     11

exit
```

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 在某些情况下，*let* 命令可能会返回一个令人惊讶的 退出状态。

&#124;  ```sh # Evgeniy Ivanov points out:

var=0
echo $?     # 0
            # As expected.

let var++
echo $?     # 1
            # The command was successful, so why isn't $?=0 ???
            # Anomaly!

let var++
echo $?     # 0
            # As expected.

# Likewise . . .

let var=0
echo $?     # 1
            # The command was successful, so why isn't $?=0 ???

#  However, as Jeff Gorak points out,
#+ this is part of the design spec for 'let' . . .
# "If the last ARG evaluates to 0, let returns 1;
#  let returns 0 otherwise." ['help let']
```  &#124;

|

**eval**

`**eval arg1 [arg2] ... [argN]**`

将表达式或表达式列表中的参数合并，并 `*评估*` 它们。表达式中的任何变量都会被展开。最终结果是 **将字符串转换为命令**。

| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | *eval* 命令可以用于从命令行或脚本中生成代码。 |
| --- | --- |
```sh
bash$ **command_string="ps ax"**
bash$ **process="ps ax"**
bash$ **eval "$command_string" &#124; grep "$process"**
26973 pts/3    R+     0:00 grep --color ps ax
 26974 pts/3    R+     0:00 ps ax

```  |

每次调用 *eval* 都会强制重新评估其参数。

```sh
a='$b'
b='$c'
c=d

echo $a             # $b
                    # First level.
eval echo $a        # $c
                    # Second level.
eval eval echo $a   # d
                    # Third level.

# Thank you, E. Choroba.
```

**示例 15-11\. 展示 *eval* 的效果**

```sh
#!/bin/bash
# Exercising "eval" ...

y=`eval ls -l`  #  Similar to y=`ls -l`
echo $y         #+ but linefeeds removed because "echoed" variable is unquoted.
echo
echo "$y"       #  Linefeeds preserved when variable is quoted.

echo; echo

y=`eval df`     #  Similar to y=`df`
echo $y         #+ but linefeeds removed.

#  When LF's not preserved, it may make it easier to parse output,
#+ using utilities such as "awk".

echo
echo "==========================================================="
echo

eval "`seq 3 &#124; sed -e 's/.*/echo var&=ABCDEFGHIJ/'`"
# var1=ABCDEFGHIJ
# var2=ABCDEFGHIJ
# var3=ABCDEFGHIJ

echo
echo "==========================================================="
echo

# Now, showing how to do something useful with "eval" . . .
# (Thank you, E. Choroba!)

version=3.4     #  Can we split the version into major and minor
                #+ part in one command?
echo "version = $version"
eval major=${version/./;minor=}     #  Replaces '.' in version by ';minor='
                                    #  The substitution yields '3; minor=4'
                                    #+ so eval does minor=4, major=3
echo Major: $major, minor: $minor   #  Major: 3, minor: 4
```

**示例 15-12\. 使用 *eval* 在变量之间进行选择**

```sh
#!/bin/bash
# arr-choice.sh

#  Passing arguments to a function to select
#+ one particular variable out of a group.

arr0=( 10 11 12 13 14 15 )
arr1=( 20 21 22 23 24 25 )
arr2=( 30 31 32 33 34 35 )
#       0  1  2  3  4  5      Element number (zero-indexed)

choose_array ()
{
  eval array_member=\${arr${array_number}[element_number]}
  #                 ^       ^^^^^^^^^^^^
  #  Using eval to construct the name of a variable,
  #+ in this particular case, an array name.

  echo "Element $element_number of array $array_number is $array_member"
} #  Function can be rewritten to take parameters.

array_number=0    # First array.
element_number=3
choose_array      # 13

array_number=2    # Third array.
element_number=4
choose_array      # 34

array_number=3    # Null array (arr3 not allocated).
element_number=4
choose_array      # (null)

# Thank you, Antonio Macchi, for pointing this out.
```

**示例 15-13\. **回显** 命令行参数***

```sh
#!/bin/bash
# echo-params.sh

# Call this script with a few command-line parameters.
# For example:
#     sh echo-params.sh first second third fourth fifth

params=$#              # Number of command-line parameters.
param=1                # Start at first command-line param.

while [ "$param" -le "$params" ]
do
  echo -n "Command-line parameter "
  echo -n \$$param     #  Gives only the *name* of variable.
#         ^^^          #  $1, $2, $3, etc.
                       #  Why?
                       #  \$ escapes the first "$"
                       #+ so it echoes literally,
                       #+ and $param dereferences "$param" . . .
                       #+ . . . as expected.
  echo -n " = "
  eval echo \$$param   #  Gives the *value* of variable.
# ^^^^      ^^^        #  The "eval" forces the *evaluation*
                       #+ of \$$
                       #+ as an indirect variable reference.

(( param ++ ))         # On to the next.
done

exit $?

# =================================================

$ sh echo-params.sh first second third fourth fifth
Command-line parameter $1 = first
Command-line parameter $2 = second
Command-line parameter $3 = third
Command-line parameter $4 = fourth
Command-line parameter $5 = fifth
```

**示例 15-14\. 强制注销**

```sh
#!/bin/bash
# Killing ppp to force a log-off.
# For dialup connection, of course.

# Script should be run as root user.

SERPORT=ttyS3
#  Depending on the hardware and even the kernel version,
#+ the modem port on your machine may be different --
#+ /dev/ttyS1 or /dev/ttyS2.

killppp="eval kill -9 `ps ax &#124; awk '/ppp/ { print $1 }'`"
#                     -------- process ID of ppp -------  

$killppp                     # This variable is now a command.

# The following operations must be done as root user.

chmod 666 /dev/$SERPORT      # Restore r+w permissions, or else what?
#  Since doing a SIGKILL on ppp changed the permissions on the serial port,
#+ we restore permissions to previous state.

rm /var/lock/LCK..$SERPORT   # Remove the serial port lock file. Why?

exit $?

# Exercises:
# ---------
# 1) Have script check whether root user is invoking it.
# 2) Do a check on whether the process to be killed
#+   is actually running before attempting to kill it.   
# 3) Write an alternate version of this script based on 'fuser':
#+      if [ fuser -s /dev/modem ]; then . . .
```

**示例 15-15\. 一种 *rot13* 版本**

```sh
#!/bin/bash
# A version of "rot13" using 'eval'.
# Compare to "rot13.sh" example.

setvar_rot_13()              # "rot13" scrambling
{
  local varname=$1 varvalue=$2
  eval $varname='$(echo "$varvalue" &#124; tr a-z n-za-m)'
}

setvar_rot_13 var "foobar"   # Run "foobar" through rot13.
echo $var                    # sbbone

setvar_rot_13 var "$var"     # Run "sbbone" through rot13.
                             # Back to original variable.
echo $var                    # foobar

# This example by Stephane Chazelas.
# Modified by document author.

exit 0
```

这里是使用 *eval* 来 *评估* 一个复杂表达式的另一个例子，这个例子来自 YongYe 的 [俄罗斯方块游戏脚本](https://github.com/yongye/shell/blob/master/Tetris_Game.sh) 的早期版本。

```sh
eval ${1}+=\"${x} ${y} \"
```

示例 A-53 使用 *eval* 将 数组 元素转换为命令列表。

*eval* 命令出现在 间接引用 的旧版本中。

```sh
eval var=\$$var
```
| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | *eval* 命令可以用于 参数化花括号扩展。 |
| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | *eval* 命令可能存在风险，通常在存在合理替代方案时应该避免使用。`**eval $COMMANDS**` 执行 `*COMMANDS*` 的内容，可能包含诸如 **rm -rf *** 这样的不愉快惊喜。在未知作者编写的未知代码上运行 **eval** 是非常危险的。 |

**set**

**set** 命令更改内部脚本变量/选项的值。这种用法之一是切换 选项标志，这有助于确定脚本的行为。另一个应用是重置脚本视为命令（``**set `command`**``）结果的 位置参数。然后脚本可以解析命令输出的 字段。

**示例 15-16\. 使用 *set* 与位置参数**

```sh
#!/bin/bash
# ex34.sh
# Script "set-test"

# Invoke this script with three command-line parameters,
# for example, "sh ex34.sh one two three".

echo
echo "Positional parameters before  set \`uname -a\` :"
echo "Command-line argument #1 = $1"
echo "Command-line argument #2 = $2"
echo "Command-line argument #3 = $3"

set `uname -a` # Sets the positional parameters to the output
               # of the command `uname -a`

echo
echo +++++
echo $_        # +++++
# Flags set in script.
echo $-        # hB
#                Anomalous behavior?
echo

echo "Positional parameters after  set \`uname -a\` :"
# $1, $2, $3, etc. reinitialized to result of `uname -a`
echo "Field #1 of 'uname -a' = $1"
echo "Field #2 of 'uname -a' = $2"
echo "Field #3 of 'uname -a' = $3"
echo \#\#\#
echo $_        # ###
echo

exit 0
```

更多关于位置参数的乐趣。

**示例 15-17\. 反转位置参数**

```sh
#!/bin/bash
# revposparams.sh: Reverse positional parameters.
# Script by Dan Jacobson, with stylistic revisions by document author.

set a\ b c d\ e;
#     ^      ^     Spaces escaped 
#       ^ ^        Spaces not escaped
OIFS=$IFS; IFS=:;
#              ^   Saving old IFS and setting new one.

echo

until [ $# -eq 0 ]
do          #      Step through positional parameters.
  echo "### k0 = "$k""     # Before
  k=$1:$k;  #      Append each pos param to loop variable.
#     ^
  echo "### k = "$k""      # After
  echo
  shift;
done

set $k  #  Set new positional parameters.
echo -
echo $# #  Count of positional parameters.
echo -
echo

for i   #  Omitting the "in list" sets the variable -- i --
        #+ to the positional parameters.
do
  echo $i  # Display new positional parameters.
done

IFS=$OIFS  # Restore IFS.

#  Question:
#  Is it necessary to set an new IFS, internal field separator,
#+ in order for this script to work properly?
#  What happens if you don't? Try it.
#  And, why use the new IFS -- a colon -- in line 17,
#+ to append to the loop variable?
#  What is the purpose of this?

exit 0

$ ./revposparams.sh

### k0 = 
### k = a b

### k0 = a b
### k = c a b

### k0 = c a b
### k = d e c a b

-
3
-

d e
c
a b
```

不带任何选项或参数调用 **set** 仅列出所有已初始化的 环境 和其他变量。

```sh
bash$ **set**
AUTHORCOPY=/home/bozo/posts
 BASH=/bin/bash
 BASH_VERSION=$'2.05.8(1)-release'
 ...
 XAUTHORITY=/home/bozo/.Xauthority
 _=/etc/bashrc
 variable22=abc
 variable23=xzy

```

使用 `--` 选项与 **set** 显式地将变量的内容分配给位置参数。如果没有变量跟在 `--` 后面，它将 *取消设置* 位置参数。

**示例 15-18\. 重新分配位置参数**

```sh
#!/bin/bash

variable="one two three four five"

set -- $variable
# Sets positional parameters to the contents of "$variable".

first_param=$1
second_param=$2
shift; shift        # Shift past first two positional params.
# shift 2             also works.
remaining_params="$*"

echo
echo "first parameter = $first_param"             # one
echo "second parameter = $second_param"           # two
echo "remaining parameters = $remaining_params"   # three four five

echo; echo

# Again.
set -- $variable
first_param=$1
second_param=$2
echo "first parameter = $first_param"             # one
echo "second parameter = $second_param"           # two

# ======================================================

set --
# Unsets positional parameters if no variable specified.

first_param=$1
second_param=$2
echo "first parameter = $first_param"             # (null value)
echo "second parameter = $second_param"           # (null value)

exit 0
```

参见 示例 11-2 和 示例 16-56。

**取消设置**

**unset** 命令删除壳变量，实际上将其设置为 *null*。请注意，此命令不会影响位置参数。

```sh
bash$ **unset PATH**

bash$ **echo $PATH**

bash$ 
```

**示例 15-19\. "取消设置" 变量**

```sh
#!/bin/bash
# unset.sh: Unsetting a variable.

variable=hello                       #  Initialized.
echo "variable = $variable"

unset variable                       #  Unset.
                                     #  In this particular context,
                                     #+ same effect as:   variable=
echo "(unset) variable = $variable"  #  $variable is null.

if [ -z "$variable" ]                #  Try a string-length test.
then
  echo "\$variable has zero length."
fi

exit 0
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 在大多数情况下，一个未声明的变量和一个已被取消设置的变量是等效的。然而，${parameter:-default} 参数替换结构可以区分这两个变量。 |

**export**

**export** [[4]](#FTN.AEN9199) 命令使运行脚本或壳的所有子进程可用变量。**export** 命令的一个重要用途是在 启动文件 中，初始化并使后续用户进程可访问 环境变量。

| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 很遗憾，没有方法可以将变量导回父进程，即调用或调用脚本的进程或壳。 |
| --- | --- |

**示例 15-20\. 使用 *export* 将变量传递给嵌入的 *awk* 脚本**

```sh
#!/bin/bash

#  Yet another version of the "column totaler" script (col-totaler.sh)
#+ that adds up a specified column (of numbers) in the target file.
#  This uses the environment to pass a script variable to 'awk' . . .
#+ and places the awk script in a variable.

ARGS=2
E_WRONGARGS=85

if [ $# -ne "$ARGS" ] # Check for proper number of command-line args.
then
   echo "Usage: `basename $0` filename column-number"
   exit $E_WRONGARGS
fi

filename=$1
column_number=$2

#===== Same as original script, up to this point =====#

export column_number
# Export column number to environment, so it's available for retrieval.

# -----------------------------------------------
awkscript='{ total += $ENVIRON["column_number"] }
END { print total }'
# Yes, a variable can hold an awk script.
# -----------------------------------------------

# Now, run the awk script.
awk "$awkscript" "$filename"

# Thanks, Stephane Chazelas.

exit 0
```

| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 可以在同一个操作中初始化和导出变量，例如 **export var1=xxx**。然而，正如 Greg Keraunen 指出的，在某些情况下，这可能会产生与设置变量然后导出它的不同效果。|

&#124;  ```sh bash$ **export var=(a b); echo ${var[0]}**
(a b)

bash$ **var=(a b); export var; echo ${var[0]}**
a

```  &#124;

|

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 要导出的变量可能需要特殊处理。参见 示例 M-2。 |
| --- | --- |

**声明**, **排版**

The declare 和 typeset 命令指定和/或限制变量的属性。

**只读**

与 declare -r 相同，将变量设置为只读，或者实际上作为常量。尝试更改变量的操作将失败并显示错误消息。这是 shell 中 *C* 语言 **const** 类型限定符的类似物。

**getopts**

这个强大的工具解析传递给脚本的命令行参数。这是 Bash 中 getopt 外部命令和 *C* 程序员熟悉的 *getopt* 库函数的类似物。它允许将多个选项 [[5]](#FTN.AEN9289) 和相关参数传递并连接到脚本（例如 `**scriptname -abc -e /usr/local**`）。

**getopts** 构造使用两个隐含变量。`$OPTIND` 是参数指针（*OPTion INDex*），而 `$OPTARG`（*OPTion ARGument*）是与选项关联的（可选的）参数。在声明标签中的冒号后面，该选项被标记为具有关联的参数。

**getopts** 构造通常包含在一个 while 循环 中，它逐个处理选项和参数，然后递增隐含的 `$OPTIND` 变量以指向下一个。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) |
| --- |

1.  从命令行传递给脚本的参数必须以短横线（`-`）开头。正是这个前缀 `-` 让 **getopts** 能够识别命令行参数为 *选项*。实际上，**getopts** 不会处理没有前缀 `-` 的参数，并且会在遇到第一个缺少它们的参数时终止选项处理。

1.  **getopts** 模板与标准 while 循环 略有不同，因为它缺少条件括号。

1.  **getopts** 构造是传统 getopt 外部命令的高度功能性替代品。

|

```sh
while getopts ":abcde:fg" Option
# Initial declaration.
# a, b, c, d, e, f, and g are the options (flags) expected.
# The : after option 'e' shows it will have an argument passed with it.
do
  case $Option in
    a ) # Do something with variable 'a'.
    b ) # Do something with variable 'b'.
    ...
    e)  # Do something with 'e', and also with $OPTARG,
        # which is the associated argument passed with option 'e'.
    ...
    g ) # Do something with variable 'g'.
  esac
done
shift $(($OPTIND - 1))
# Move argument pointer to next.

# All this is not nearly as complicated as it looks <grin>.
```

**示例 15-21\. 使用 *getopts* 读取传递给脚本的选项/参数**

```sh
#!/bin/bash
# ex33.sh: Exercising getopts and OPTIND
#          Script modified 10/09/03 at the suggestion of Bill Gradwohl.

# Here we observe how 'getopts' processes command-line arguments to script.
# The arguments are parsed as "options" (flags) and associated arguments.

# Try invoking this script with:
#   'scriptname -mn'
#   'scriptname -oq qOption' (qOption can be some arbitrary string.)
#   'scriptname -qXXX -r'
#
#   'scriptname -qr'
#+      - Unexpected result, takes "r" as the argument to option "q"
#   'scriptname -q -r' 
#+      - Unexpected result, same as above
#   'scriptname -mnop -mnop'  - Unexpected result
#   (OPTIND is unreliable at stating where an option came from.)
#
#  If an option expects an argument ("flag:"), then it will grab
#+ whatever is next on the command-line.

NO_ARGS=0 
E_OPTERROR=85

if [ $# -eq "$NO_ARGS" ]    # Script invoked with no command-line args?
then
  echo "Usage: `basename $0` options (-mnopqrs)"
  exit $E_OPTERROR          # Exit and explain usage.
                            # Usage: scriptname -options
                            # Note: dash (-) necessary
fi  

while getopts ":mnopq:rs" Option
do
  case $Option in
    m     ) echo "Scenario #1: option -m-   [OPTIND=${OPTIND}]";;
    n &#124; o ) echo "Scenario #2: option -$Option-   [OPTIND=${OPTIND}]";;
    p     ) echo "Scenario #3: option -p-   [OPTIND=${OPTIND}]";;
    q     ) echo "Scenario #4: option -q-\
                  with argument \"$OPTARG\"   [OPTIND=${OPTIND}]";;
    #  Note that option 'q' must have an associated argument,
    #+ otherwise it falls through to the default.
    r &#124; s ) echo "Scenario #5: option -$Option-";;
    *     ) echo "Unimplemented option chosen.";;   # Default.
  esac
done

shift $(($OPTIND - 1))
#  Decrements the argument pointer so it points to next argument.
#  $1 now references the first non-option item supplied on the command-line
#+ if one exists.

exit $?

#   As Bill Gradwohl states,
#  "The getopts mechanism allows one to specify:  scriptname -mnop -mnop
#+  but there is no reliable way to differentiate what came
#+ from where by using OPTIND."
#  There are, however, workarounds.
```

**脚本行为**

**source**，. (点 命令)

当从命令行调用此命令时，它将执行一个脚本。在脚本内部，一个 `**source file-name**` 加载文件 `file-name`。*Sourcing* 一个文件（点命令）*导入*代码到脚本中，附加到脚本（与 *C* 程序中的 `**#include**` 指令具有相同的效果）。最终结果是，如果源代码行物理上存在于脚本主体中，效果是相同的。这在多个脚本使用公共数据文件或函数库的情况下非常有用。

**示例 15-22\. “包含”数据文件**

```sh
#!/bin/bash
#  Note that this example must be invoked with bash, i.e., bash ex38.sh
#+ not  sh ex38.sh !

. data-file    # Load a data file.
# Same effect as "source data-file", but more portable.

#  The file "data-file" must be present in current working directory,
#+ since it is referred to by its basename.

# Now, let's reference some data from that file.

echo "variable1 (from data-file) = $variable1"
echo "variable3 (from data-file) = $variable3"

let "sum = $variable2 + $variable4"
echo "Sum of variable2 + variable4 (from data-file) = $sum"
echo "message1 (from data-file) is \"$message1\""
#                                  Escaped quotes
echo "message2 (from data-file) is \"$message2\""

print_message This is the message-print function in the data-file.

exit $?
```

上面的 示例 15-22 中的文件 `data-file`。必须在同一目录中存在。

```sh
# This is a data file loaded by a script.
# Files of this type may contain variables, functions, etc.
# It loads with a 'source' or '.' command from a shell script.

# Let's initialize some variables.

variable1=23
variable2=474
variable3=5
variable4=97

message1="Greetings from *** line $LINENO *** of the data file!"
message2="Enough for now. Goodbye."

print_message ()
{   # Echoes any message passed to it.

  if [ -z "$1" ]
  then
    return 1 # Error, if argument missing.
  fi

  echo

  until [ -z "$1" ]
  do             # Step through arguments passed to function.
    echo -n "$1" # Echo args one at a time, suppressing line feeds.
    echo -n " "  # Insert spaces between words.
    shift        # Next one.
  done  

  echo

  return 0
}
```

如果源文件本身是一个可执行脚本，那么它将运行，然后返回控制权给调用它的脚本。一个源可执行脚本可以使用 return 来实现此目的。

参数可以作为（可选的）位置参数传递给源文件。positional parameters。

```sh
source $filename $arg1 arg2
```

脚本甚至可以 *源* 自身，尽管这似乎没有实际应用。

**示例 15-23\. 一个（无用的）自引用脚本**

```sh
#!/bin/bash
# self-source.sh: a script sourcing itself "recursively."
# From "Stupid Script Tricks," Volume II.

MAXPASSCNT=100    # Maximum number of execution passes.

echo -n  "$pass_count  "
#  At first execution pass, this just echoes two blank spaces,
#+ since $pass_count still uninitialized.

let "pass_count += 1"
#  Assumes the uninitialized variable $pass_count
#+ can be incremented the first time around.
#  This works with Bash and pdksh, but
#+ it relies on non-portable (and possibly dangerous) behavior.
#  Better would be to initialize $pass_count to 0 before incrementing.

while [ "$pass_count" -le $MAXPASSCNT ]
do
  . $0   # Script "sources" itself, rather than calling itself.
         # ./$0 (which would be true recursion) doesn't work here. Why?
done  

#  What occurs here is not actually recursion,
#+ since the script effectively "expands" itself, i.e.,
#+ generates a new section of code
#+ with each pass through the 'while' loop',
#  with each 'source' in line 20.
#
#  Of course, the script interprets each newly 'sourced' "#!" line
#+ as a comment, and not as the start of a new script.

echo

exit 0   # The net effect is counting from 1 to 100.
         # Very impressive.

# Exercise:
# --------
# Write a script that uses this trick to actually do something useful.
```

**exit**

无条件终止一个脚本。[[6]](#FTN.AEN9393) **exit** 命令可以可选地接受一个整数参数，该参数作为脚本的 退出状态 返回给 shell。在所有但最简单的脚本中结束 `**exit 0**` 是一个好习惯，表示成功运行。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 如果一个脚本在没有参数的情况下终止 **exit**，则脚本的退出状态是脚本中最后执行的命令的退出状态，不包括 **exit**。这相当于 **exit $?**。 |
| --- | --- |
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | **exit** 命令也可以用来终止一个 子 shell。 |

**exec**

这个 shell 内建命令用指定的命令替换当前进程。通常，当 shell 遇到命令时，它会 派生 一个子进程来实际执行该命令。使用 **exec** 内建命令时，shell 不会派生，被 *exec* 的命令替换了 shell。因此，在脚本中使用时，当 *exec* 的命令终止时，它强制脚本退出。[[7]](#FTN.AEN9425)

**示例 15-24\. *exec*** 的影响

```sh
#!/bin/bash

exec echo "Exiting \"$0\" at line $LINENO."   # Exit from script here.
# $LINENO is an internal Bash variable set to the line number it's on.

# ----------------------------------
# The following lines never execute.

echo "This echo fails to echo."

exit 99                       #  This script will not exit here.
                              #  Check exit value after script terminates
                              #+ with an 'echo $?'.
                              #  It will *not* be 99.
```

**示例 15-25\. 自 **exec** 的脚本**

```sh
#!/bin/bash
# self-exec.sh

# Note: Set permissions on this script to 555 or 755,
#       then call it with ./self-exec.sh or sh ./self-exec.sh.

echo

echo "This line appears ONCE in the script, yet it keeps echoing."
echo "The PID of this instance of the script is still $$."
#     Demonstrates that a subshell is not forked off.

echo "==================== Hit Ctl-C to exit ===================="

sleep 1

exec $0   #  Spawns another instance of this same script
          #+ that replaces the previous one.

echo "This line will never echo!"  # Why not?

exit 99                            # Will not exit here!
                                   # Exit code will not be 99!
```

**exec** 也可以用于 重新分配文件描述符。例如，`**exec <zzz-file**` 将 `stdin` 替换为文件 `zzz-file`。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | `find` 的 `-exec` 选项与 **exec** 命令内建功能 **不相同**。 |
| --- | --- |

**shopt**

这个命令允许动态更改 *shell 选项*（参见 示例 25-1 和 示例 25-2）。它通常出现在 Bash 启动文件 中，但在脚本中也有其用途。需要 Bash 的 版本 2 或更高版本。

```sh
shopt -s cdspell
# Allows minor misspelling of directory names with 'cd'
# Option -s sets, -u unsets.

cd /hpme  # Oops! Mistyped '/home'.
pwd       # /home
          # The shell corrected the misspelling.
```

**caller**

在 函数 中放置 **caller** 命令会向 `stdout` 输出有关该函数 *caller* 的信息。

```sh
#!/bin/bash

function1 ()
{
  # Inside function1 ().
  caller 0   # Tell me about it.
}

function1    # Line 9 of script.

# 9 main test.sh
# ^                 Line number that the function was called from.
#   ^^^^            Invoked from "main" part of script.
#        ^^^^^^^    Name of calling script.

caller 0     # Has no effect because it's not inside a function.
```

**caller** 命令还可以从另一个脚本中 源 的脚本中返回 *caller* 信息。类似于函数，这是一个“子程序调用”。

你可能会在调试时发现这个命令很有用。

**命令**

**true**

一个返回成功（零）退出状态 但没有做其他事情的命令。

```sh
bash$ **true**
bash$ **echo $?**
0

```
```sh
# Endless loop
while true   # alias for ":"
do
   operation-1
   operation-2
   ...
   operation-n
   # Need a way to break out of loop or script will hang.
done
```  |

**false**

一个返回不成功 退出状态 但没有做其他事情的命令。

```sh
bash$ **false**
bash$ **echo $?**
1

```
```sh
# Testing "false" 
if false
then
  echo "false evaluates \"true\""
else
  echo "false evaluates \"false\""
fi
# false evaluates "false"

# Looping while "false" (null loop)
while false
do
   # The following code will not execute.
   operation-1
   operation-2
   ...
   operation-n
   # Nothing happens!
done   
```  |

**type [cmd]**

与外部命令 which 类似，**type cmd** 识别 "cmd"。与 **which** 不同，**type** 是 Bash 内置命令。**type** 的有用 `-a` 选项识别 `*keywords*` 和 `*builtins*`，并且还能定位具有相同名称的系统命令。

```sh
bash$ **type ''**
[ is a shell builtin
bash$ **type -a '['**
[ is a shell builtin
 [ is /usr/bin/[

bash$ **type type**
type is a shell builtin

```

**type** 命令可以用于 [测试某个命令是否存在。

**hash [cmds**]

记录指定命令的 *路径* 名称——在 shell 的 *哈希表* [[8]](#FTN.AEN9591) 中——这样在后续调用这些命令时，shell 或脚本就不需要搜索 $PATH 了。当 **hash** 不带参数调用时，它简单地列出已哈希的命令。`-r` 选项重置哈希表。

**绑定**

**bind** 内置命令显示或修改 *readline* [[9]](#FTN.AEN9621) 键绑定。

**帮助**

获取 shell 内置命令的简短用法摘要。这是 whatis 的对应物，但用于内置命令。*帮助* 信息的显示在 Bash 的 版本 4 发布 中得到了急需的更新。

```sh
bash$ **help exit**
exit: exit [n]
    Exit the shell with a status of N.  If N is omitted, the exit status
    is that of the last command executed.

```

### **笔记**

| [[1]](internal.html#AEN8607) | 正如内森·考尔特所指出的，“虽然进程分叉是一个低成本的操作，但在新分叉的子进程中执行新的程序会增加更多的开销。” |
| --- | --- |
| [[2]](internal.html#AEN8650) | 这里的一个例外是 time 命令，在官方 Bash 文档中被列为关键字（“保留词”）。 |
| [[3]](internal.html#AEN9009) | 注意，*let* 不能用于设置 *字符串* 变量。 |
| [[4]](internal.html#AEN9199) | 要 *导出* 信息，就是使其在更广泛的环境中可用。另请参阅 作用域。 |
| [[5]](internal.html#AEN9289) | *选项* 是一个作为标志的参数，用于切换脚本的开启或关闭行为。与特定选项关联的参数表示该选项（标志）切换开启或关闭的行为。 |
| [[6]](internal.html#AEN9393) | 技术上，**exit** 只会终止它正在运行的进程（或 shell），*不会* 终止 *父进程*。 |
| [[7]](internal.html#AEN9425) | 除非使用 **exec** 来 重新分配文件描述符。 |
| [[8]](internal.html#AEN9591) | *哈希* 是为存储在表中的数据创建查找键的方法。*数据项本身* 被使用多种简单的数学 *算法*（方法或配方）进行“打乱”以创建键。*哈希* 的一个优点是它速度快。一个缺点是可能出现 *冲突* —— 即单个键映射到多个数据项。有关哈希的示例，请参阅 示例 A-20 和 示例 A-21。 |
| [[9]](internal.html#AEN9621) | Bash 用于在交互式 shell 中读取输入的 *readline* 库。 |
