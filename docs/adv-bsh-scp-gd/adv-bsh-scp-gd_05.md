# 第三章. 特殊字符

> 原文：[`tldp.org/LDP/abs/html/special-chars.html`](https://tldp.org/LDP/abs/html/special-chars.html)

什么使一个字符*特殊*？如果它除了其*字面意义之外还有其他意义，即元意义，那么我们称它为*特殊字符*。与命令和关键字一样，*特殊字符*是 Bash 脚本的构建块。

**脚本和其他地方找到的特殊字符**

#

**注释.** 以#开始的行（除了#!）是注释，将*不会*被执行。

```sh
# This line is a comment.
```

注释也可能出现在命令的末尾之后。

```sh
echo "A comment will follow." # Comment here.
#                            ^ Note whitespace before #
```

注释也可能跟在行首的空白字符之后。

```sh
     # A tab precedes this comment.
```

注释甚至可以嵌入到管道中。

```sh
initial=( `cat "$startfile" &#124; sed -e '/#/d' &#124; tr -d '\n' &#124;\
# Delete lines containing '#' comment character.
           sed -e 's/\./\. /g' -e 's/_/_ /g'` )
# Excerpted from life.sh script
```
| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 一条命令不能在同一行跟在注释之后。没有终止注释的方法，以便在同一行上开始“活代码”。使用新行作为下一个命令。 |

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 当然，在 echo 语句中的引号或转义 # 不会开始一个注释。同样，# 也出现在某些参数替换结构和数值常量表达式中。

&#124;  ```sh echo "The # here does not begin a comment."
echo 'The # here does not begin a comment.'
echo The \# here does not begin a comment.
echo The # here begins a comment.

echo ${PATH#*:}       # Parameter substitution, not a comment.
echo $(( 2#101011 ))  # Base conversion, not a comment.

# Thanks, S.C.
```  &#124;

标准的引号和转义字符(" ' \)会转义#。

某些模式匹配操作也使用#。

;

**命令分隔符[分号].** 允许在同一行上放置两个或更多命令。

```sh
echo hello; echo there

if [ -x "$filename" ]; then    #  Note the space after the semicolon.
#+                   ^^
  echo "File $filename exists."; cp $filename $filename.bak
else   #                       ^^
  echo "File $filename not found."; touch $filename
fi; echo "File test complete."
```

注意，分号（;）有时需要被*转义*。

;;

**case 选项中的终止符[双分号].**

```sh
case "$variable" in
  abc)  echo "\$variable = abc" ;;
  xyz)  echo "\$variable = xyz" ;;
esac
```

;;&, ;&

**终止符在*case*选项中（Bash 的版本 4+）**

.

**"点"命令[句点].** 等同于 source（参见示例 15-22）。这是一个 bash 内建命令。

.

**"点"，作为文件名的一部分.** 当处理文件名时，开头的点是一个“隐藏”文件的前缀，一个 ls 通常不会显示的文件。

```sh
bash$ **touch .hidden-file**
bash$ **ls -l**	      
total 10
 -rw-r--r--    1 bozo      4034 Jul 18 22:04 data1.addressbook
 -rw-r--r--    1 bozo      4602 May 25 13:58 data1.addressbook.bak
 -rw-r--r--    1 bozo       877 Dec 17  2000 employment.addressbook

bash$ **ls -al**	      
total 14
 drwxrwxr-x    2 bozo  bozo      1024 Aug 29 20:54 ./
 drwx------   52 bozo  bozo      3072 Aug 29 20:51 ../
 -rw-r--r--    1 bozo  bozo      4034 Jul 18 22:04 data1.addressbook
 -rw-r--r--    1 bozo  bozo      4602 May 25 13:58 data1.addressbook.bak
 -rw-r--r--    1 bozo  bozo       877 Dec 17  2000 employment.addressbook
 -rw-rw-r--    1 bozo  bozo         0 Aug 29 20:54 .hidden-file

```

当考虑目录名时，*单个点*代表当前工作目录，而*两个点*表示父目录。

```sh
bash$ **pwd**
/home/bozo/projects

bash$ **cd .**
bash$ **pwd**
/home/bozo/projects

bash$ **cd ..**
bash$ **pwd**
/home/bozo/

```

*点*经常出现在文件移动命令的目标（目录）中，在这种情况下意味着*当前目录*。

```sh
bash$ **cp /home/bozo/current_work/junk/* .**

```

将所有“垃圾”文件复制到$PWD。

.

**"点"字符匹配。** 当 匹配字符 作为 正则表达式 的一部分时，一个 "点" 匹配单个字符。

"

**部分引用 [双引号].** *"STRING"* 保留（从解释中）*STRING* 内的大部分特殊字符。参见 第五章。

'

**完全引用 [单引号].** *'STRING'* 保留 *STRING* 内的所有特殊字符。这是一种比 *"STRING"* 更强的引用形式。参见 第五章。

,

**逗号操作符.** 逗号操作符将一系列算术操作连接起来。所有操作都会被评估，但只有最后一个操作的结果会被返回。

```sh
let "t2 = ((a = 9, 15 / 3))"
# Set "a = 9" and "t2 = 15 / 3"
```

逗号操作符也可以连接字符串。

```sh
for file in /{,usr/}bin/*calc
#             ^    Find all executable files ending in "calc"
#+                 in /bin and /usr/bin directories.
do
        if [ -x "$file" ]
        then
          echo $file
        fi
done

# /bin/ipcalc
# /usr/bin/kcalc
# /usr/bin/oidcalc
# /usr/bin/oocalc

# Thank you, Rory Winston, for pointing this out.
```

,, ,

**参数替换中的小写转换（在 Bash 的 版本 4 中添加）。**

\

**转义 [反斜杠].** 单个字符的引用机制。

`**\X**` 会转义字符 *X*。这相当于“引用”*X*，等同于 *'X'*。反斜杠可以用来引用 " 和 '，因此它们会被字面地表达出来。

参见 第五章 以深入了解转义字符。

/

**文件名路径分隔符 [正斜杠].** 分隔文件名的各个组成部分（例如 `/home/bozo/projects/Makefile`）。

这也是除法 算术操作符。

`

**命令替换.** **`command`** 构造使得可以将 **command** 的输出分配给变量。这也被称为 反引号 或反引号。

:

**空命令 [冒号].** 这是 shell 的 "NOP"（*no op*，不执行任何操作）的等价物。它可以被认为是 shell 内置命令 true 的同义词。":" 命令本身是一个 *Bash* 内置命令，其 退出状态 是 *true*（0）。

```sh
:
echo $?   # 0
```

无限循环：

```sh
while :
do
   operation-1
   operation-2
   ...
   operation-n
done

# Same as:
#    while true
#    do
#      ...
#    done
```

在 if/then 测试中的占位符：

```sh
if condition
then :   # Do nothing and branch ahead
else     # Or else ...
   take-some-action
fi
```

在期望二进制操作的地方提供一个占位符，参见 示例 8-2 和 默认参数。

```sh
: ${username=`whoami`}
# ${username=`whoami`}   Gives an error without the leading :
#                        unless "username" is a command or builtin...

: ${1?"Usage: $0 ARGUMENT"}     # From "usage-message.sh example script.
```

在 here document 中期望命令的地方提供一个占位符。参见 示例 19-10。

使用 参数替换 评估变量字符串（如 示例 10-7 所示）。

```sh
: ${HOSTNAME?} ${USER?} ${MAIL?}
#  Prints error message
#+ if one or more of essential environmental variables not set.
```

**变量扩展/子串替换**.

与 > 重定向操作符 结合使用时，将文件截断为零长度，而不改变其权限。如果文件之前不存在，则创建它。

```sh
: > data.xxx   # File "data.xxx" now empty.	      

# Same effect as   cat /dev/null >data.xxx
# However, this does not fork a new process, since ":" is a builtin.
```

参见 示例 16-15。

与 >> 重定向操作符结合使用时，对现有目标文件没有影响（`**: >> target_file**`）。如果文件之前不存在，则创建它。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 这适用于常规文件，而不是管道、符号链接和某些特殊文件。 |
| --- | --- |

可以用来开始注释行，尽管这不被推荐。使用 # 作为注释将关闭该行剩余部分的错误检查，因此几乎任何内容都可以出现在注释中。然而，情况并非如此。

```sh
: This is a comment that generates an error, ( if [ $x -eq 3] ).
```

冒号在 etc/passwd 和 $PATH 变量中充当 字段 分隔符。

```sh
bash$ **echo $PATH**
/usr/local/bin:/bin:/usr/bin:/usr/X11R6/bin:/sbin:/usr/sbin:/usr/games
```

一个 *冒号* 可以 作为函数名接受。

```sh
:()
{
  echo "The name of this function is "$FUNCNAME" "
  # Why use a colon as a function name?
  # It's a way of obfuscating your code.
}

:

# The name of this function is :
```

这不是 可移植 的行为，因此不推荐这样做。事实上，Bash 的较新版本不允许这种用法。尽管如此，下划线 **_** 是有效的。

一个 *冒号* 可以在空函数中充当占位符。

```sh
not_empty ()
{
  :
} # Contains a : (null command), and so is not empty.
```

!

**反转（或否定）测试或退出状态的含义[感叹号]**。! 操作符反转它所应用的命令的 退出状态（参见 示例 6-2）。它还反转测试操作符的含义。例如，可以将 *等于*（ = ）改为 *不等于*（ != ）。! 操作符是 Bash 的 关键字。

在不同的上下文中，! 也出现在 间接变量引用 中。

在另一个上下文中，从 *命令行* 中，! 调用 Bash 的 *历史机制*（参见 附录 L）。请注意，在脚本中，历史机制是禁用的。

*

**通配符[星号]**。* 字符在 通配符 中的文件名扩展中充当 "通配符"。单独使用时，它匹配给定目录中的所有文件名。

```sh
bash$ **echo ***
abs-book.sgml add-drive.sh agram.sh alias.sh

```

* 也代表 任意数量（或零）字符 在 正则表达式 中。

*

**算术操作符**。在算术操作的上下文中，* 表示乘法。

**双星号可以表示 指数 操作符或 扩展文件匹配 *通配符* globbing*。

?

**测试操作符**。在特定的表达式中，? 表示对条件的测试。

在一个双括号构造中，? 可以作为 C 风格的 *三元* 操作符的一个元素。[[2]](#FTN.AEN888)

`condition`**?**`result-if-true`**:**`result-if-false`

```sh
(( var0 = var1<98?9:21 ))
#                ^ ^

# if [ "$var1" -lt 98 ]
# then
#   var0=9
# else
#   var0=21
# fi
```

在 参数替换 表达式中，? 测试变量是否已设置。

?

**通配符**。问号（?）字符在通配符匹配中充当单个字符的“通配符”，同时在扩展正则表达式中表示一个字符（x17129.html#QUEXREGEX）。

$

**变量替换（变量的内容）。**

```sh
var1=5
var2=23skidoo

echo $var1     # 5
echo $var2     # 23skidoo
```

变量名前加$表示变量所持有的*值*。

$

**行尾**。在正则表达式中，$符号指向文本的行尾。

${}

**参数替换。**

$' ... '

**引号字符串展开。** 此结构将单个或多个转义八进制或十六进制值展开为 ASCII [[3]](#FTN.AEN1001) 或 Unicode 字符。

$*, $@

**位置参数。**

$?

**退出状态变量**。$? 变量保存命令、函数或脚本本身的退出状态。

$$

**进程 ID 变量**。$$ 变量保存脚本中出现的*进程 ID* [[4]](#FTN.AEN1071)。

()

**命令组**。

```sh
(a=hello; echo $a)
```

| ![重要](img/d8e2c61a4ebc25a8846b6825b6029167.png) | 在括号内的命令列表开始一个子 shell。括号内的子 shell 中的变量对脚本的其他部分不可见。父进程，即脚本，不能读取子进程中创建的变量，子 shell。

&#124;  ```sh a=123
( a=321; )	      

echo "a = $a"   # a = 123
# "a" within parentheses acts like a local variable.
```  &#124;

|

**数组初始化**。

```sh
Array=(element1 element2 element3)
```

{xxx,yyy,zzz,...}

**花括号展开**。

```sh
echo \"{These,words,are,quoted}\"   # " prefix and suffix
# "These" "words" "are" "quoted"

cat {file1,file2,file3} > combined_file
# Concatenates the files file1, file2, and file3 into combined_file.

cp file22.{txt,backup}
# Copies "file22.txt" to "file22.backup"
```

一个命令可以作用于花括号内逗号分隔的文件规范列表。[[5]](#FTN.AEN1124) 文件名展开（通配符匹配）应用于花括号之间的文件规范。

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 花括号内不允许有空格，除非空格被引号引用或转义。`**echo {file1,file2}\ :{\ A," B",' C'}**``file1 : A file1 : B file1 : C file2 : A file2 : B file2 : C` |
| --- | --- |

{a..z}

**扩展花括号展开**。

```sh
echo {a..z} # a b c d e f g h i j k l m n o p q r s t u v w x y z
# Echoes characters between a and z.

echo {0..3} # 0 1 2 3
# Echoes characters between 0 and 3.

base64_charset=( {A..Z} {a..z} {0..9} + / = )
# Initializing an array, using extended brace expansion.
# From vladz's "base64.sh" example script.
```

*{a..z}* 扩展花括号展开构造是在*Bash*的版本 3 中引入的特性。

{}

**代码块[花括号]。** 也称为*内联组*，此结构实际上创建了一个*匿名函数*（一个没有名称的函数）。然而，与“标准”函数不同，代码块内的变量对脚本的其他部分仍然是可见的。

```sh
bash$ **{ local a;
	      a=123; }**
bash: local: can only be used in a
function

```
```sh
a=123
{ a=321; }
echo "a = $a"   # a = 321   (value inside code block)

# Thanks, S.C.
```  |

花括号内的代码块可以将其输入/输出重定向到它。

**示例 3-1\. 代码块和输入/输出重定向**

```sh
#!/bin/bash
# Reading lines in /etc/fstab.

File=/etc/fstab

{
read line1
read line2
} < $File

echo "First line in $File is:"
echo "$line1"
echo
echo "Second line in $File is:"
echo "$line2"

exit 0

# Now, how do you parse the separate fields of each line?
# Hint: use awk, or . . .
# . . . Hans-Joerg Diers suggests using the "set" Bash builtin.
```

**示例 3-2\. 将代码块的输出保存到文件**

```sh
#!/bin/bash
# rpm-check.sh

#  Queries an rpm file for description, listing,
#+ and whether it can be installed.
#  Saves output to a file.
# 
#  This script illustrates using a code block.

SUCCESS=0
E_NOARGS=65

if [ -z "$1" ]
then
  echo "Usage: `basename $0` rpm-file"
  exit $E_NOARGS
fi  

{ # Begin code block.
  echo
  echo "Archive Description:"
  rpm -qpi $1       # Query description.
  echo
  echo "Archive Listing:"
  rpm -qpl $1       # Query listing.
  echo
  rpm -i --test $1  # Query whether rpm file can be installed.
  if [ "$?" -eq $SUCCESS ]
  then
    echo "$1 can be installed."
  else
    echo "$1 cannot be installed."
  fi  
  echo              # End code block.
} > "$1.test"       # Redirects output of everything in block to file.

echo "Results of rpm test in file $1.test"

# See rpm man page for explanation of options.

exit 0
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 与上面括号内的命令组不同，由大括号包围的代码块通常不会启动 子 shell。[[6]](#FTN.AEN1199)可以使用 非标准 *for-loop* 来迭代代码块。 |

{}

**占位符文本**。在 xargs `-i` (*替换字符串*选项) 后使用。{} 双大括号是输出文本的占位符。

```sh
ls . &#124; xargs -i -t cp ./{} $1
#            ^^         ^^

# From "ex42.sh" (copydir.sh) example.
```

{} \;

**路径名**。主要用于 find 构造。这不是 shell 内置命令。

|

定义：*路径名* 是包含完整 路径 的 *文件名*。例如，`/home/bozo/Notes/Thursday/schedule.txt`。这有时被称为 *绝对路径*。

|

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 分号 (;) 结束了 **find** 命令序列的 `-exec` 选项。需要转义以防止 shell 解释。 |
| --- | --- |

[ ]

**测试**。

测试 表达式在 **[ ]** 之间。请注意 **** 是 shell *内置* [test (及其同义词)，*不是* 对外部命令 `/usr/bin/test` 的链接。

[[ ]]

**测试**。

在 [[ ]] 之间测试表达式。比单括号 [ ] 测试更灵活，这是一个 shell 关键字。

查看关于 [[[ ... ]] 构造](testconstructs.html#DBLBRACKETS) 的讨论。

[ ]

**数组元素**。

在 数组 的上下文中，括号用于标记该数组每个元素的编号。

```sh
Array[1]=slot_1
echo ${Array[1]}
```

[ ]

**字符范围**。

作为正则表达式的一部分，括号定义了一个字符范围以进行匹配。

$[ ... ]

**整数扩展**。

评估 $[ ] 之间的整数表达式。

```sh
a=3
b=7

echo $[$a+$b]   # 10
echo $[$a*$b]   # 21
```

注意，这种用法已被弃用，并由 (( ... )) 构造所取代。

(( ))

**整数扩展**。

展开并评估 (( )) 之间的整数表达式。

查看关于(( ... ))构造的讨论。

> &> >& >> < <>

**重定向**。

`**scriptname >filename**` 将 `scriptname` 的输出重定向到文件 `filename`。如果文件已存在，则覆盖它。

`**command &>filename**` 将 `command` 的 `stdout` 和 `stderr` 都重定向到 `filename`。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 这在测试条件时抑制输出很有用。例如，让我们测试某个命令是否存在。

&#124;  ```sh bash$ **type bogus_command &>/dev/null**
 `bash$` `**echo $?**`
`1` 
```  &#124;

或者在一个脚本中：

&#124;  ```sh command_test () { type "$1" &>/dev/null; }
#                                      ^

cmd=rmdir            # Legitimate command.
command_test $cmd; echo $?   # 0

cmd=bogus_command    # Illegitimate command
command_test $cmd; echo $?   # 1
```  &#124;

|

`**command >&2**` 将 `command` 的 `stdout` 重定向到 `stderr`。

`**scriptname >>filename**` 将 `scriptname` 的输出追加到文件 `filename`。如果 `filename` 已经存在，则创建它。

`**[i]<>filename**` 为文件 `filename` 打开读写权限，并将 文件描述符 i 分配给它。如果 `filename` 不存在，则创建它。

**进程替换。**

`**(命令)>**`

`**<(命令)**`

在另一个上下文中，"<" 和 ">" 字符作为 字符串比较运算符。

在另一个上下文中，"<" 和 ">" 字符作为 整数比较运算符。另见 示例 16-9。

<<

**在 here 文档 中使用的重定向。**

<<<

**在 here 字符串 中使用的重定向。**

<, >

**ASCII 比较操作。**

```sh
veg1=carrots
veg2=tomatoes

if [[ "$veg1" < "$veg2" ]]
then
  echo "Although $veg1 precede $veg2 in the dictionary,"
  echo -n "this does not necessarily imply anything "
  echo "about my culinary preferences."
else
  echo "What kind of dictionary are you using, anyhow?"
fi
```

\<, \>

**正则表达式中的单词边界。**

`bash$` `**grep '\<the\>' textfile**`

|

**管道**。将前一个命令的输出 (`stdout`) 传递到下一个命令的输入 (`stdin`) 或传递到 shell。这是将命令链接在一起的方法。

```sh
echo ls -l &#124; sh
#  Passes the output of "echo ls -l" to the shell,
#+ with the same result as a simple "ls -l".

cat *.lst &#124; sort &#124; uniq
# Merges and sorts all ".lst" files, then deletes duplicate lines.
```

|

管道作为一种经典的进程间通信方法，将一个 进程 的 `stdout` 发送到另一个进程的 `stdin`。在典型情况下，例如 cat 或 echo 命令将数据流传递到 *过滤器*，这是一个将输入转换为处理过程的命令。[[7]](#FTN.AEN1564)

`**cat $filename1 $filename2 &#124; grep $search_word**`

关于使用 UNIX 管道的复杂性的有趣说明，请参阅 [UNIX FAQ，第三部分](http://www.faqs.org/faqs/unix-faq/faq/part3/)。

|

命令或命令的输出可以传递到脚本。

```sh
#!/bin/bash
# uppercase.sh : Changes input to uppercase.

tr 'a-z' 'A-Z'
#  Letter ranges must be quoted
#+ to prevent filename generation from single-letter filenames.

exit 0
```

现在，让我们将 `ls -l` 的输出通过管道传递到这个脚本。

```sh
bash$ **ls -l &#124; ./uppercase.sh**
-RW-RW-R--    1 BOZO  BOZO       109 APR  7 19:49 1.TXT
 -RW-RW-R--    1 BOZO  BOZO       109 APR 14 16:48 2.TXT
 -RW-R--R--    1 BOZO  BOZO       725 APR 20 20:56 DATA-FILE

```

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 管道中每个进程的 `stdout` 必须作为下一个进程的 `stdin` 来读取。如果这不是这种情况，数据流将 *阻塞*，管道将不会按预期工作。

&#124;  ```sh cat file1 file2 &#124; ls -l &#124; sort
# The output from "cat file1 file2" disappears.
```  &#124;

管道作为一个 子进程 运行，因此不能更改脚本变量。

&#124;  ```sh variable="initial_value"
echo "new_value" &#124; read variable
echo "variable = $variable"     # variable = initial_value
```  &#124;

如果管道中的某个命令终止，这将提前终止管道的执行。称为 *损坏的管道*，这种情况会发送一个 `*SIGPIPE*` 信号。 |

>|

**强制重定向（即使设置了 noclobber 选项）。** 这将强制覆盖现有文件。

||

**逻辑或运算符。** 在 测试构造 中，如果 *任一* 链接的测试条件为真，则 || 运算符会导致返回 0（成功）。

&

**在后台运行作业。** 后跟一个 & 的命令将在后台运行。

```sh
bash$ **sleep 10 &**
[1] 850
[1]+  Done                    sleep 10

```

在脚本中，命令甚至 循环 也可以在后台运行。

**示例 3-3\. 在后台运行循环**

```sh
#!/bin/bash
# background-loop.sh

for i in 1 2 3 4 5 6 7 8 9 10            # First loop.
do
  echo -n "$i "
done & # Run this loop in background.
       # Will sometimes execute after second loop.

echo   # This 'echo' sometimes will not display.

for i in 11 12 13 14 15 16 17 18 19 20   # Second loop.
do
  echo -n "$i "
done  

echo   # This 'echo' sometimes will not display.

# ======================================================

# The expected output from the script:
# 1 2 3 4 5 6 7 8 9 10 
# 11 12 13 14 15 16 17 18 19 20 

# Sometimes, though, you get:
# 11 12 13 14 15 16 17 18 19 20 
# 1 2 3 4 5 6 7 8 9 10 bozo $
# (The second 'echo' doesn't execute. Why?)

# Occasionally also:
# 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20
# (The first 'echo' doesn't execute. Why?)

# Very rarely something like:
# 11 12 13 1 2 3 4 5 6 7 8 9 10 14 15 16 17 18 19 20 
# The foreground loop preempts the background one.

exit 0

#  Nasimuddin Ansari suggests adding    sleep 1
#+ after the   echo -n "$i"   in lines 6 and 14,
#+ for some real fun.
```
| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 在脚本中运行的背景命令可能会导致脚本挂起，等待按键。幸运的是，有一个 解决办法。 |

&&

**AND 逻辑运算符。** 在 测试构造 中，&& 运算符仅在 *两个* 链接的测试条件都为真时才返回 0（成功）。

-

**选项，前缀。** 命令或过滤器的选项标志。运算符的前缀。在 参数替换 中 默认参数 的前缀。

`**COMMAND -[Option1][Option2][...]**`

`**ls -al**`

`**sort -dfu $filename**`

```sh
if [ $file1 -ot $file2 ]
then #      ^
  echo "File $file1 is older than $file2."
fi

if [ "$a" -eq "$b" ]
then #    ^
  echo "$a is equal to $b."
fi

if [ "$c" -eq 24 -a "$d" -eq 47 ]
then #    ^              ^
  echo "$c equals 24 and $d equals 47."
fi

param2=${param1:-$DEFAULTVAL}
#               ^
```

**--**

*双横线* `--` 是命令的 *长*（文本）选项的前缀。

`**sort --ignore-leading-blanks**`

与 Bash 内置命令 结合使用时，它表示该特定命令的 *选项结束*。

| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 这提供了一种方便的方法来删除以 *减号* 开头的文件名。

&#124;  ```sh bash$ **ls -l**
-rw-r--r-- 1 bozo bozo 0 Nov 25 12:29 -badname

bash$ **rm -- -badname**

bash$ **ls -l**
total 0
```  &#124;

|

*双横线* 也与 set 结合使用。

`**set -- $variable**`（如 示例 15-18）

-

**从/to `stdin` 或 `stdout` 重定向 [减号]。**

```sh
bash$ **cat -**
**abc**
abc

...

**Ctl-D**
```

如预期，`**cat -**` 将 `stdin`（在这种情况下是键盘输入的用户输入）回显到 `stdout`。但是，使用 **-** 的 I/O 重定向在现实世界中有什么应用？

```sh
(cd /source/directory && tar cf - . ) &#124; (cd /dest/directory && tar xpvf -)
# Move entire file tree from one directory to another
# [courtesy Alan Cox <a.cox@swansea.ac.uk>, with a minor change]

# 1) cd /source/directory
#    Source directory, where the files to be moved are.
# 2) &&
#   "And-list": if the 'cd' operation successful,
#    then execute the next command.
# 3) tar cf - .
#    The 'c' option 'tar' archiving command creates a new archive,
#    the 'f' (file) option, followed by '-' designates the target file
#    as stdout, and do it in current directory tree ('.').
# 4) &#124;
#    Piped to ...
# 5) ( ... )
#    a subshell
# 6) cd /dest/directory
#    Change to the destination directory.
# 7) &&
#   "And-list", as above
# 8) tar xpvf -
#    Unarchive ('x'), preserve ownership and file permissions ('p'),
#    and send verbose messages to stdout ('v'),
#    reading data from stdin ('f' followed by '-').
#
#    Note that 'x' is a command, and 'p', 'v', 'f' are options.
#
# Whew!

# More elegant than, but equivalent to:
#   cd source/directory
#   tar cf - . &#124; (cd ../dest/directory; tar xpvf -)
#
#     Also having same effect:
# cp -a /source/directory/* /dest/directory
#     Or:
# cp -a /source/directory/* /source/directory/.[^.]* /dest/directory
#     If there are hidden files in /source/directory.
```
```sh
bunzip2 -c linux-2.6.16.tar.bz2 &#124; tar xvf -
#  --uncompress tar file--      &#124; --then pass it to "tar"--
#  If "tar" has not been patched to handle "bunzip2",
#+ this needs to be done in two discrete steps, using a pipe.
#  The purpose of the exercise is to unarchive "bzipped" kernel source.
```  |

注意，在这个上下文中，“-” 本身不是一个 Bash 操作符，而是一个由某些 UNIX 实用程序（如 **tar**、**cat** 等）识别的选项，这些实用程序将输出写入 `stdout`。

```sh
bash$ **echo "whatever" &#124; cat -**
whatever 
```

当期望文件名时，`*-*` 将输出重定向到 `stdout`（有时与 `**tar cf**` 一起看到），或从 `stdin` 而不是从文件接受输入。这是一种将面向文件实用程序用作管道中过滤器的使用方法。

```sh
bash$ **file**
Usage: file [-bciknvzL] [-f namefile] [-m magicfiles] file...

```

在命令行上单独使用时，file 会报错。

添加一个 "-" 以获得更有用的结果。这会导致 shell 等待用户输入。

```sh
bash$ **file -**
**abc**
standard input:              ASCII text

bash$ **file -**
**#!/bin/bash**
standard input:              Bourne-Again shell script text executable

```

现在命令接受来自 `stdin` 的输入并进行分析。

“-” 可以用来将 `stdout` 输出到其他命令。这允许进行诸如 向文件中添加行 这样的技巧。

使用 diff 比较一个文件与另一个文件的 *部分*：

`**grep Linux file1 | diff file2 -**`

最后，一个使用 `*-*` 与 tar 的现实世界示例。

**示例 3-4\. 备份过去一天内更改的所有文件**

```sh
#!/bin/bash

#  Backs up all files in current directory modified within last 24 hours
#+ in a "tarball" (tarred and gzipped file).

BACKUPFILE=backup-$(date +%m-%d-%Y)
#                 Embeds date in backup filename.
#                 Thanks, Joshua Tschida, for the idea.
archive=${1:-$BACKUPFILE}
#  If no backup-archive filename specified on command-line,
#+ it will default to "backup-MM-DD-YYYY.tar.gz."

tar cvf - `find . -mtime -1 -type f -print` > $archive.tar
gzip $archive.tar
echo "Directory $PWD backed up in archive file \"$archive.tar.gz\"."

#  Stephane Chazelas points out that the above code will fail
#+ if there are too many files found
#+ or if any filenames contain blank characters.

# He suggests the following alternatives:
# -------------------------------------------------------------------
#   find . -mtime -1 -type f -print0 &#124; xargs -0 tar rvf "$archive.tar"
#      using the GNU version of "find".

#   find . -mtime -1 -type f -exec tar rvf "$archive.tar" '{}' \;
#         portable to other UNIX flavors, but much slower.
# -------------------------------------------------------------------

exit 0
```

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 以“-”开头的文件名与“-”重定向操作符结合时可能会引起问题。脚本应检查此问题，并为这样的文件名添加适当的前缀，例如`./-FILENAME`，`$PWD/-FILENAME`或`$PATHNAME/-FILENAME`。如果变量的值以`*-*`开头，这也可能同样引起问题。

&#124;  ```sh var="-n"
echo $var		
# Has the effect of "echo -n", and outputs nothing.
```  &#124;

|

-

**前一个工作目录**。一个**cd -**命令会切换到前一个工作目录。这使用的是$OLDPWD 环境变量。

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 不要将此意义上的“-”与刚刚讨论的“-”重定向操作符混淆。对“-”的解释取决于其出现的环境。 |
| --- | --- |

-

**减号。** 算术操作中的减号。

=

**等于。** 赋值操作符

```sh
a=28
echo $a   # 28
```

在不同的环境中，等号“=”是一个字符串比较操作符。

+

**加号。** 加法算术操作符。

在不同的环境中，"+"是一个正则表达式操作符。

+

**选项。** 命令或过滤器的选项标志。

某些命令和内置命令使用`+`来启用某些选项，使用`-`来禁用它们。在参数替换中，`+`前缀一个变量扩展到的备选值。

%

**取模。** 取模（除法的余数）算术操作。

```sh
let "z = 5 % 3"
echo $z  # 2
```

在不同的环境中，百分号“%”是一个模式匹配操作符。

~

**主目录[波浪号]。** 这对应于内部变量$HOME。`~bozo`是 bozo 的主目录，**ls ~bozo**列出其内容。`~/`是当前用户的家目录，**ls ~/**列出其内容。

```sh
bash$ **echo ~bozo**
/home/bozo

bash$ **echo ~**
/home/bozo

bash$ **echo ~/**
/home/bozo/

bash$ **echo ~:**
/home/bozo:

bash$ **echo ~nonexistent-user**
~nonexistent-user

```

~+

**当前工作目录**。这对应于内部变量$PWD。

~-

**前一个工作目录**。这对应于内部变量$OLDPWD。

=~

**正则表达式匹配。** 这个操作符是在 Bash 的版本 3 中引入的。

^

**行首。** 在正则表达式中，"^"指向文本的行首。

^, ^^

**大写转换在*参数替换*中（在 Bash 的版本 4 中添加）。**

控制字符

**改变终端或文本显示的行为**。控制字符是 **CONTROL** + **键** 组合（同时按下）。控制字符也可以用 *八进制* 或 *十六进制* 表示法，在 *转义* 后面。

控制字符通常在脚本中不常用。

+   `**Ctl-A**`

    将光标移到文本行的开头（在命令行中）。

+   `**Ctl-B**`

    `**退格**`（非破坏性）。

+   `**Ctl-C**`

    `**Break**`。终止前台作业。

+   `**Ctl-D**`

    从壳中 *登出*（类似于 退出）。

    `**EOF**`（文件结束）。这也会终止来自 `stdin` 的输入。

    当在控制台或 *xterm* 窗口中键入文本时，`**Ctl-D**` 会删除光标下的字符。当没有字符时，`**Ctl-D**` 会像预期的那样登出会话。在 *xterm* 窗口中，这会产生关闭窗口的效果。

+   `**Ctl-E**`

    将光标移到文本行的末尾（在命令行中）。

+   `**Ctl-F**`

    将光标向前移动一个字符位置（在命令行中）。

+   `**Ctl-G**`

    `**BEL**`。在一些老式的电传打字机上，这实际上会响铃。在 *xterm* 中可能会发出蜂鸣声。

+   `**Ctl-H**`

    `**Rubout**`（破坏性退格）。删除光标在退格时覆盖的字符。

    ```sh
    #!/bin/bash
    # Embedding Ctl-H in a string.

    a="^H^H"                  # Two Ctl-H's -- backspaces
                              # ctl-V ctl-H, using vi/vim
    echo "abcdef"             # abcdef
    echo
    echo -n "abcdef$a "       # abcd f
    #  Space at end  ^              ^  Backspaces twice.
    echo
    echo -n "abcdef$a"        # abcdef
    #  No space at end               ^ Doesn't backspace (why?).
                              # Results may not be quite as expected.
    echo; echo

    # Constantin Hagemeier suggests trying:
    # a=$'\010\010'
    # a=$'\b\b'
    # a=$'\x08\x08'
    # But, this does not change the results.

    ########################################

    # Now, try this.

    rubout="^H^H^H^H^H"       # 5 x Ctl-H.

    echo -n "12345678"
    sleep 2
    echo -n "$rubout"
    sleep 2
    ```

+   `**Ctl-I**`

    `**水平制表符**`.

+   `**Ctl-J**`

    `**换行**`（换行符）。在脚本中，也可以用八进制表示法 -- `\012` 或十六进制表示法 -- `\x0a`。

+   `**Ctl-K**`

    `**垂直制表符**`.

    当在控制台或 *xterm* 窗口中键入文本时，`**Ctl-K**` 会从光标下的字符删除到行尾。在脚本中，`**Ctl-K**` 的行为可能不同，如下面的 Lee Lee Maschmeyer 的例子所示。

+   `**Ctl-L**`

    `**换页**`（清除终端屏幕）。在终端中，这具有与 清除 命令相同的效果。当发送到打印机时，一个 `**Ctl-L**` 会导致纸张的结束。

+   `**Ctl-M**`

    `**回车**`。

    ```sh
    #!/bin/bash
    # Thank you, Lee Maschmeyer, for this example.

    read -n 1 -s -p \
    $'Control-M leaves cursor at beginning of this line. Press Enter. \x0d'
               # Of course, '0d' is the hex equivalent of Control-M.
    echo >&2   #  The '-s' makes anything typed silent,
               #+ so it is necessary to go to new line explicitly.

    read -n 1 -s -p $'Control-J leaves cursor on next line. \x0a'
               #  '0a' is the hex equivalent of Control-J, linefeed.
    echo >&2

    ###

    read -n 1 -s -p $'And Control-K\x0bgoes straight down.'
    echo >&2   #  Control-K is vertical tab.

    # A better example of the effect of a vertical tab is:

    var=$'\x0aThis is the bottom line\x0bThis is the top line\x0a'
    echo "$var"
    #  This works the same way as the above example. However:
    echo "$var" &#124; col
    #  This causes the right end of the line to be higher than the left end.
    #  It also explains why we started and ended with a line feed --
    #+ to avoid a garbled screen.

    # As Lee Maschmeyer explains:
    # --------------------------
    #  In the [first vertical tab example] . . . the vertical tab
    #+ makes the printing go straight down without a carriage return.
    #  This is true only on devices, such as the Linux console,
    #+ that can't go "backward."
    #  The real purpose of VT is to go straight UP, not down.
    #  It can be used to print superscripts on a printer.
    #  The col utility can be used to emulate the proper behavior of VT.

    exit 0
    ```

+   `**Ctl-N**`

    从 *历史缓冲区* 中删除一行文本 [[8]](#FTN.AEN2107)（在命令行中）。

+   `**Ctl-O**`

    在命令行中发出 *换行*。

+   `**Ctl-P**`

    从 *历史缓冲区* 中调用最后一条命令（在命令行中）。

+   `**Ctl-Q**`

    恢复（`**XON**`）。

    这会在终端中恢复 `stdin`。

+   `**Ctl-R**`

    在 *历史缓冲区*（在命令行中）中向后搜索文本。

+   `**Ctl-S**`

    暂停 (`**XOFF**`).

    这会在终端中冻结 `stdin`。（使用 Ctl-Q 来恢复输入。）

+   `**Ctl-T**`

    将光标所在位置与之前的字符位置交换（在命令行中）。

+   `**Ctl-U**`

    从输入行的光标位置向后删除整行。在某些设置中，`**Ctl-U**` 会删除整个输入行，*无论光标位置如何*。

+   `**Ctl-V**`

    当输入文本时，`**Ctl-V**` 允许插入控制字符。例如，以下两个是等效的：

    ```sh
    echo -e '\x0a'
    echo <Ctl-V><Ctl-J>
    ```

    `**Ctl-V**` 主要在文本编辑器中使用。

+   `**Ctl-W**`

    在控制台或 xterm 窗口中键入文本时，`**Ctl-W**` 从光标下的字符开始向后删除到第一个 空白 实例。在某些设置中，`**Ctl-W**` 向后删除到第一个非字母数字字符。

+   `**Ctl-X**`

    在某些文字处理程序中，*剪切* 高亮文本并将其复制到 *剪贴板*。

+   `**Ctl-Y**`

    *粘贴* 回之前删除的文本（使用 `**Ctl-U**` 或 `**Ctl-W**`）。

+   `**Ctl-Z**`

    *暂停* 前台作业。

    在某些文字处理应用程序中的 *替换* 操作。

    `**EOF**`（文件结束）字符在 MSDOS 文件系统中。

空白

**作为命令和/或变量之间的分隔符**。空白由 *空格*、*制表符*、*空白行* 或它们的任意组合组成。[[9]](#FTN.AEN2198) 在某些上下文中，例如 变量赋值，不允许使用空白，这会导致语法错误。

空白行对脚本的执行没有影响，因此对于视觉上分隔功能部分很有用。

$IFS，分隔某些命令输入 *字段* 的特殊变量。它默认为空白。

|

`**定义**:` *字段* 是以连续字符字符串形式表示的数据离散块。将每个字段与其相邻字段分开的是 *空白* 或其他指定的字符（通常由 $IFS 确定）。在某些上下文中，字段可能被称为 *记录*。

|

要在字符串或变量中保留 *空白*，请使用 引号。

UNIX 过滤器 可以使用 POSIX（x17129.html#POSIXREF）字符类 [[:space:]](x17129.html#WSPOSIX) 针对和操作 *空白*。

### 注意事项

| [[1]](special-chars.html#AEN612) | 一个 *操作符* 是执行 *操作* 的代理。一些例子包括常见的 算术操作符，**+ - * /**。在 Bash 中，*操作符* 和 关键字 的概念存在一些重叠。 |
| --- | --- |
| [[2]](special-chars.html#AEN888) | 这更常见地被称为 *三元* 操作符。不幸的是，*三元* 是一个丑陋的词。它听起来不悦耳，也不具有说明性。它使事情变得复杂。*三元* 是迄今为止更优雅的使用方式。 |
| [[3]](special-chars.html#AEN1001) | **美国** **标准** **信息** **交换** **代码**。这是一个将文本字符（字母、数字和有限数量的符号）编码为 7 位数字的系统，这些数字可以被计算机存储和处理。许多 ASCII 字符在标准键盘上都有表示。 |
| [[4]](special-chars.html#AEN1071) | *PID*，或 *进程 ID*，是分配给正在运行进程的一个数字。可以使用 ps 命令查看正在运行进程的 *PID*。`**定义**:` *进程* 是当前正在执行的命令（或程序），有时也称为 *作业*。 |
| [[5]](special-chars.html#AEN1124) | shell 执行*花括号展开*。命令本身作用于展开的*结果*。 |

| [[6]](special-chars.html#AEN1199) | 例外：作为管道部分的花括号代码块*可能*作为一个子 shell 运行。

&#124;  ```sh ls &#124; { read firstline; read secondline; }
#  Error. The code block in braces runs as a subshell,
#+ so the output of "ls" cannot be passed to variables within the block.
echo "First line is $firstline; second line is $secondline"  # Won't work.

# Thanks, S.C.
```  &#124;

|

| [[7]](special-chars.html#AEN1564) | 就像在古老时代，一个*药水*代表一种据说具有魔法转换力量的饮料，UNIX 中的*过滤器*也以（大致）类似的方式转换其目标。（能够编写在 Linux 机器上运行的“爱情药水”的程序员可能会赢得赞誉和荣誉。） |
| --- | --- |
| [[8]](special-chars.html#AEN2107) | Bash 将之前从命令行发出的命令列表存储在一个*缓冲区*或内存空间中，以便使用 builtin *历史*命令进行回忆。 |
| [[9]](special-chars.html#AEN2198) | 换行符（*换行*）也是一个空白字符。这也解释了为什么只包含换行符的*空白行*被视为空白字符。 |
