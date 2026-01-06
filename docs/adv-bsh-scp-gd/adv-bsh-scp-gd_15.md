# 第十二章\. 命令替换

> 原文：[`tldp.org/LDP/abs/html/commandsub.html`](https://tldp.org/LDP/abs/html/commandsub.html)

**命令替换**重新分配命令[[1]](#FTN.AEN7205)或多个命令的输出；它实际上是将命令输出插入到另一个上下文中。[2]](#FTN.AEN7211)

命令替换的经典形式使用*反引号*(`...`)。反引号（backticks）内的命令生成命令行文本。

```sh
script_name=`basename $0`
echo "The name of this script is $script_name."
```

**命令的输出可以用作另一个命令的参数，用于设置变量，甚至用于在 for 循环中生成参数列表。**

```sh
rm `cat filename`   # "filename" contains a list of files to delete.
#
# S. C. points out that "arg list too long" error might result.
# Better is              xargs rm -- < filename 
# ( -- covers those cases where "filename" begins with a "-" )

textfile_listing=`ls *.txt`
# Variable contains names of all *.txt files in current working directory.
echo $textfile_listing

textfile_listing2=$(ls *.txt)   # The alternative form of command substitution.
echo $textfile_listing2
# Same result.

# A possible problem with putting a list of files into a single string
# is that a newline may creep in.
#
# A safer way to assign a list of files to a parameter is with an array.
#      shopt -s nullglob    # If no match, filename expands to nothing.
#      textfile_listing=( *.txt )
#
# Thanks, S.C.
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 命令替换调用一个子 shell。 |

| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 命令替换可能会导致单词分割。|

&#124;  ```sh COMMAND `echo a b`     # 2 args: a and b

COMMAND "`echo a b`"   # 1 arg: "a b"

COMMAND `echo`         # no arg

COMMAND "`echo`"       # one empty arg

# Thanks, S.C.
```  &#124;

即使没有单词分割，命令替换也可以删除尾随的新行。

&#124;  ```sh # cd "`pwd`"  # This should always work.
# However...

mkdir 'dir with trailing newline
'

cd 'dir with trailing newline
'

cd "`pwd`"  # Error message:
# bash: cd: /tmp/file with trailing newline: No such file or directory

cd "$PWD"   # Works fine.

old_tty_setting=$(stty -g)   # Save old terminal setting.
echo "Hit a key "
stty -icanon -echo           # Disable "canonical" mode for terminal.
                             # Also, disable *local* echo.
key=$(dd bs=1 count=1 2> /dev/null)   # Using 'dd' to get a keypress.
stty "$old_tty_setting"      # Restore old setting. 
echo "You hit ${#key} key."  # ${#variable} = number of characters in $variable
#
# Hit any key except RETURN, and the output is "You hit 1 key."
# Hit RETURN, and it's "You hit 0 key."
# The newline gets eaten in the command substitution.

#Code snippet by Stphane Chazelas.
```  &#124;

|

| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 使用**echo**输出通过命令替换设置的*未引用*变量会从重新分配的命令的输出中删除尾随的新行字符。这可能会导致不愉快的惊喜。|

&#124;  ```sh dir_listing=`ls -l`
echo $dir_listing     # unquoted

# Expecting a nicely ordered directory listing.

# However, what you get is:
# total 3 -rw-rw-r-- 1 bozo bozo 30 May 13 17:15 1.txt -rw-rw-r-- 1 bozo
# bozo 51 May 15 20:57 t2.sh -rwxr-xr-x 1 bozo bozo 217 Mar 5 21:13 wi.sh

# The newlines disappeared.

echo "$dir_listing"   # quoted
# -rw-rw-r--    1 bozo       30 May 13 17:15 1.txt
# -rw-rw-r--    1 bozo       51 May 15 20:57 t2.sh
# -rwxr-xr-x    1 bozo      217 Mar  5 21:13 wi.sh
```  &#124;

|

命令替换甚至允许使用重定向或 cat 命令将变量设置为文件的内容。

```sh
variable1=`<file1`      #  Set "variable1" to contents of "file1".
variable2=`cat file2`   #  Set "variable2" to contents of "file2".
                        #  This, however, forks a new process,
                        #+ so the line of code executes slower than the above version.

#  Note that the variables may contain embedded whitespace,
#+ or even (horrors), control characters.

#  It is not necessary to explicitly assign a variable.
echo "` <$0`"           # Echoes the script itself to stdout.
```
```sh
#  Excerpts from system file, /etc/rc.d/rc.sysinit
#+ (on a Red Hat Linux installation)

if [ -f /fsckoptions ]; then
        fsckoptions=`cat /fsckoptions`
...
fi
#
#
if [ -e "/proc/ide/${disk[$device]}/media" ] ; then
             hdmedia=`cat /proc/ide/${disk[$device]}/media`
...
fi
#
#
if [ ! -n "`uname -r &#124; grep -- "-"`" ]; then
       ktag="`cat /proc/version`"
...
fi
#
#
if [ $usb = "1" ]; then
    sleep 5
    mouseoutput=`cat /proc/bus/usb/devices 2>/dev/null&#124;grep -E "^I.*Cls=03.*Prot=02"`
    kbdoutput=`cat /proc/bus/usb/devices 2>/dev/null&#124;grep -E "^I.*Cls=03.*Prot=01"`
...
fi
```  |

| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 除非你有非常充分的理由，否则不要将变量设置为*长*文本文件的内容。不要将变量设置为*二进制*文件的内容，即使是为了开玩笑。|

**示例 12-1\. 愚蠢的脚本技巧**

&#124;  ```sh #!/bin/bash
# stupid-script-tricks.sh: Don't try this at home, folks.
# From "Stupid Script Tricks," Volume I.

exit 99  ### Comment out this line if you dare.

dangerous_variable=`cat /boot/vmlinuz`   # The compressed Linux kernel itself.

echo "string-length of \$dangerous_variable = ${#dangerous_variable}"
# string-length of $dangerous_variable = 794151
# (Newer kernels are bigger.)
# Does not give same count as 'wc -c /boot/vmlinuz'.

# echo "$dangerous_variable"
# Don't try this! It would hang the script.

#  The document author is aware of no useful applications for
#+ setting a variable to the contents of a binary file.

exit 0
```  &#124;

注意，不会发生*缓冲区溢出*。这是解释语言（如 Bash）比编译语言提供更多保护以防止程序员错误的一个例子。|

命令替换允许将变量设置为循环的输出。这个问题的关键是在循环中捕获一个 echo 命令的输出。

**示例 12-2\. 从循环生成变量**

```sh
#!/bin/bash
# csubloop.sh: Setting a variable to the output of a loop.

variable1=`for i in 1 2 3 4 5
do
  echo -n "$i"                 #  The 'echo' command is critical
done`                          #+ to command substitution here.

echo "variable1 = $variable1"  # variable1 = 12345

i=0
variable2=`while [ "$i" -lt 10 ]
do
  echo -n "$i"                 # Again, the necessary 'echo'.
  let "i += 1"                 # Increment.
done`

echo "variable2 = $variable2"  # variable2 = 0123456789

#  Demonstrates that it's possible to embed a loop
#+ inside a variable declaration.

exit 0
```

|

命令替换使得扩展 Bash 可用的工具集成为可能。这仅仅是一个编写输出到`stdout`（就像一个表现良好的 UNIX 工具应该做的那样）的程序或脚本，并将该输出分配给变量的简单问题。

&#124;  ```sh #include <stdio.h>

/*  "Hello, world." C program  */		

int main()
{
  printf( "Hello, world.\n" );
  return (0);
}
```  &#124;

&#124;  ```sh bash$ **gcc -o hello hello.c**

```  &#124;

&#124;  ```sh #!/bin/bash
# hello.sh		

greeting=`./hello`
echo $greeting
```  &#124;

&#124;  ```sh bash$ **sh hello.sh**
Hello, world.

```  &#124;

|

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | **$(...)**形式已经取代了反引号用于命令替换。|

&#124;  ```sh output=$(sed -n /"$1"/p $file)   # From "grp.sh"	example.

# Setting a variable to the contents of a text file.
File_contents1=$(cat $file1)      
File_contents2=$(<$file2)        # Bash permits this also.
```  &#124;

命令替换的**$(...)**形式与**`...`**处理双反斜杠的方式不同。

&#124;  ```sh bash$ **echo `echo \\`**
 `bash$` `**echo $(echo \\)**`
`\` 
```  &#124;

命令替换的**$(...)**形式允许嵌套。[[3]](#FTN.AEN7308)

&#124;  ```sh word_count=$( wc -w $(echo * &#124; awk '{print $8}') )
```  &#124;

或者，对于更复杂的情况……

**示例 12-3. 寻找字母表异序词**

&#124;  ```sh #!/bin/bash
# agram2.sh
# Example of nested command substitution.

#  Uses "anagram" utility
#+ that is part of the author's "yawl" word list package.
#  http://ibiblio.org/pub/Linux/libs/yawl-0.3.2.tar.gz
#  http://bash.deta.in/yawl-0.3.2.tar.gz

E_NOARGS=86
E_BADARG=87
MINLEN=7

if [ -z "$1" ]
then
  echo "Usage $0 LETTERSET"
  exit $E_NOARGS         # Script needs a command-line argument.
elif [ ${#1} -lt $MINLEN ]
then
  echo "Argument must have at least $MINLEN letters."
  exit $E_BADARG
fi

FILTER='.......'         # Must have at least 7 letters.
#       1234567
Anagrams=( $(echo $(anagram $1 &#124; grep $FILTER) ) )
#          $(     $(  nested command sub.    ) )
#        (              array assignment         )

echo
echo "${#Anagrams[*]}  7+ letter anagrams found"
echo
echo ${Anagrams[0]}      # First anagram.
echo ${Anagrams[1]}      # Second anagram.
                         # Etc.

# echo "${Anagrams[*]}"  # To list all the anagrams in a single line . . .

#  Look ahead to the Arrays chapter for enlightenment on
#+ what's going on here.

# See also the agram.sh script for an exercise in anagram finding.

exit $?
```  &#124;

|

在 shell 脚本中命令替换的示例：

1.  示例 11-8

1.  示例 11-27

1.  示例 9-16

1.  示例 16-3

1.  示例 16-22

1.  示例 16-17

1.  示例 16-54

1.  示例 11-14

1.  示例 11-11

1.  示例 16-32

1.  示例 20-8

1.  示例 A-16

1.  示例 29-3

1.  示例 16-47

1.  示例 16-48

1.  示例 16-49

### 备注

| [[1]](commandsub.html#AEN7205) | 为了**命令替换**的目的，一个**命令**可能是一个外部系统命令，一个内部脚本内置函数，甚至是一个脚本函数。 |
| --- | --- |
| [[2]](commandsub.html#AEN7211) | 在更严格的技术意义上，**命令替换**提取命令的`stdout`，然后使用等号运算符将其赋值给变量。 |

| [[3]](commandsub.html#AEN7308) | 实际上，使用反引号嵌套也是可能的，但只能通过转义内部的反引号，正如 John Default 所指出的。

&#124;  ```sh word_count=` wc -w \`echo * &#124; awk '{print $8}'\` `
```  &#124;

|
