# 第十九章。Here 文档

> 原文：[`tldp.org/LDP/abs/html/here-docs.html`](https://tldp.org/LDP/abs/html/here-docs.html)

|    |  **现在就做，孩子们。**

*--Aldous Huxley, *Island***  |

*here document* 是一种特殊用途的代码块。它使用一种 I/O 重定向 的形式将命令列表传递给交互式程序或命令，例如 ftp、cat 或 *ex* 文本编辑器。

```sh
COMMAND <<InputComesFromHERE
...
...
...
InputComesFromHERE
```

*limit string* 界定了（框架）命令列表。特殊符号 << 在限制字符串之前。这具有将命令块输出重定向到程序或命令的 `stdin` 的效果。它类似于 `**interactive-program < command-file**`，其中 `command-file` 包含

```sh
command #1
command #2
...
```

*here document* 的等效形式如下所示：

```sh
interactive-program <<LimitString
command #1
command #2
...
LimitString
```

选择一个足够不寻常的 *limit string*，这样它就不会出现在命令列表中并引起混淆。

注意，*here document* 有时与非交互式实用程序和命令一起使用，效果很好，例如，例如 wall。

**示例 19-1\. *广播*: 向所有登录用户发送消息**

```sh
#!/bin/bash

wall <<zzz23EndOfMessagezzz23
E-mail your noontime orders for pizza to the system administrator.
    (Add an extra dollar for anchovy or mushroom topping.)
# Additional message text goes here.
# Note: 'wall' prints comment lines.
zzz23EndOfMessagezzz23

# Could have been done more efficiently by
#         wall <message-file
#  However, embedding the message template in a script
#+ is a quick-and-dirty one-off solution.

exit
```

即使像 *vi* 文本编辑器这样的不太可能的候选者也适合使用 *here document*。

**示例 19-2\. *dummyfile*: 创建一个两行的虚拟文件**

```sh
#!/bin/bash

# Noninteractive use of 'vi' to edit a file.
# Emulates 'sed'.

E_BADARGS=85

if [ -z "$1" ]
then
  echo "Usage: `basename $0` filename"
  exit $E_BADARGS
fi

TARGETFILE=$1

# Insert 2 lines in file, then save.
#--------Begin here document-----------#
vi $TARGETFILE <<x23LimitStringx23
i
This is line 1 of the example file.
This is line 2 of the example file.
^[
ZZ
x23LimitStringx23
#----------End here document-----------#

#  Note that ^[ above is a literal escape
#+ typed by Control-V <Esc>.

#  Bram Moolenaar points out that this may not work with 'vim'
#+ because of possible problems with terminal interaction.

exit
```

上述脚本同样可以用 **ex** 实现，而不是 **vi**。包含 **ex** 命令列表的 *here document* 足够常见，可以形成一个自己的类别，称为 *ex 脚本*。

```sh
#!/bin/bash
#  Replace all instances of "Smith" with "Jones"
#+ in files with a ".txt" filename suffix. 

ORIGINAL=Smith
REPLACEMENT=Jones

for word in $(fgrep -l $ORIGINAL *.txt)
do
  # -------------------------------------
  ex $word <<EOF
  :%s/$ORIGINAL/$REPLACEMENT/g
  :wq
EOF
  # :%s is the "ex" substitution command.
  # :wq is write-and-quit.
  # -------------------------------------
done
```

与 "ex 脚本" 类似的是 *cat 脚本*。

**示例 19-3\. 使用 *cat* 的多行消息**

```sh
#!/bin/bash

#  'echo' is fine for printing single line messages,
#+  but somewhat problematic for for message blocks.
#   A 'cat' here document overcomes this limitation.

cat <<End-of-message
-------------------------------------
This is line 1 of the message.
This is line 2 of the message.
This is line 3 of the message.
This is line 4 of the message.
This is the last line of the message.
-------------------------------------
End-of-message

#  Replacing line 7, above, with
#+   cat > $Newfile <<End-of-message
#+       ^^^^^^^^^^
#+ writes the output to the file $Newfile, rather than to stdout.

exit 0

#--------------------------------------------
# Code below disabled, due to "exit 0" above.

# S.C. points out that the following also works.
echo "-------------------------------------
This is line 1 of the message.
This is line 2 of the message.
This is line 3 of the message.
This is line 4 of the message.
This is the last line of the message.
-------------------------------------"
# However, text may not include double quotes unless they are escaped.
```

用于标记 here 文档限制字符串的 `-` 选项 (`**<<-LimitString**`) 抑制输出中的前导制表符（但不包括空格）。这可能在使脚本更易读时很有用。

**示例 19-4\. 多行消息，抑制了制表符**

```sh
#!/bin/bash
# Same as previous example, but...

#  The - option to a here document <<-
#+ suppresses leading tabs in the body of the document,
#+ but *not* spaces.

cat <<-ENDOFMESSAGE
	This is line 1 of the message.
	This is line 2 of the message.
	This is line 3 of the message.
	This is line 4 of the message.
	This is the last line of the message.
ENDOFMESSAGE
# The output of the script will be flush left.
# Leading tab in each line will not show.

# Above 5 lines of "message" prefaced by a tab, not spaces.
# Spaces not affected by   <<-  .

# Note that this option has no effect on *embedded* tabs.

exit 0
```

*here document* 支持参数和命令替换。因此，可以将不同的参数传递给 *here document* 的主体，从而相应地改变其输出。

**示例 19-5\. 带有可替换参数的 here 文档**

```sh
#!/bin/bash
# Another 'cat' here document, using parameter substitution.

# Try it with no command-line parameters,   ./scriptname
# Try it with one command-line parameter,   ./scriptname Mortimer
# Try it with one two-word quoted command-line parameter,
#                           ./scriptname "Mortimer Jones"

CMDLINEPARAM=1     #  Expect at least command-line parameter.

if [ $# -ge $CMDLINEPARAM ]
then
  NAME=$1          #  If more than one command-line param,
                   #+ then just take the first.
else
  NAME="John Doe"  #  Default, if no command-line parameter.
fi  

RESPONDENT="the author of this fine script"  

cat <<Endofmessage

Hello, there, $NAME.
Greetings to you, $NAME, from $RESPONDENT.

# This comment shows up in the output (why?).

Endofmessage

# Note that the blank lines show up in the output.
# So does the comment.

exit
```

这是一个有用的脚本，其中包含带有参数替换的 *here document*。

**示例 19-6\. 将文件对上传到 *Sunsite* 入口目录**

```sh
#!/bin/bash
# upload.sh

#  Upload file pair (Filename.lsm, Filename.tar.gz)
#+ to incoming directory at Sunsite/UNC (ibiblio.org).
#  Filename.tar.gz is the tarball itself.
#  Filename.lsm is the descriptor file.
#  Sunsite requires "lsm" file, otherwise will bounce contributions.

E_ARGERROR=85

if [ -z "$1" ]
then
  echo "Usage: `basename $0` Filename-to-upload"
  exit $E_ARGERROR
fi  

Filename=`basename $1`           # Strips pathname out of file name.

Server="ibiblio.org"
Directory="/incoming/Linux"
#  These need not be hard-coded into script,
#+ but may instead be changed to command-line argument.

Password="your.e-mail.address"   # Change above to suit.

ftp -n $Server <<End-Of-Session
# -n option disables auto-logon

user anonymous "$Password"       #  If this doesn't work, then try:
                                 #  quote user anonymous "$Password"
binary
bell                             # Ring 'bell' after each file transfer.
cd $Directory
put "$Filename.lsm"
put "$Filename.tar.gz"
bye
End-Of-Session

exit 0
```

引用或转义 here 文档头部的 "limit string" 禁止其主体内的参数替换。原因是 *引用/转义 limit string* 有效地 转义 了 $、`、\ 特殊字符，并使它们被字面地解释。（感谢 Allen Halsey 指出这一点。）

**示例 19-7\. 关闭参数替换**

```sh
#!/bin/bash
#  A 'cat' here-document, but with parameter substitution disabled.

NAME="John Doe"
RESPONDENT="the author of this fine script"  

cat <<'Endofmessage'

Hello, there, $NAME.
Greetings to you, $NAME, from $RESPONDENT.

Endofmessage

#   No parameter substitution when the "limit string" is quoted or escaped.
#   Either of the following at the head of the here document would have
#+  the same effect.
#   cat <<"Endofmessage"
#   cat <<\Endofmessage

#   And, likewise:

cat <<"SpecialCharTest"

Directory listing would follow
if limit string were not quoted.
`ls -l`

Arithmetic expansion would take place
if limit string were not quoted.
$((5 + 3))

A a single backslash would echo
if limit string were not quoted.
\\

SpecialCharTest

exit
```

关闭参数替换允许输出文本。生成脚本甚至程序代码是这种用途之一。

**示例 19-8\. 生成另一个脚本的脚本**

```sh
#!/bin/bash
# generate-script.sh
# Based on an idea by Albert Reiner.

OUTFILE=generated.sh         # Name of the file to generate.

# -----------------------------------------------------------
# 'Here document containing the body of the generated script.
(
cat <<'EOF'
#!/bin/bash

echo "This is a generated shell script."
#  Note that since we are inside a subshell,
#+ we can't access variables in the "outside" script.

echo "Generated file will be named: $OUTFILE"
#  Above line will not work as normally expected
#+ because parameter expansion has been disabled.
#  Instead, the result is literal output.

a=7
b=3

let "c = $a * $b"
echo "c = $c"

exit 0
EOF
) > $OUTFILE
# -----------------------------------------------------------

#  Quoting the 'limit string' prevents variable expansion
#+ within the body of the above 'here document.'
#  This permits outputting literal strings in the output file.

if [ -f "$OUTFILE" ]
then
  chmod 755 $OUTFILE
  # Make the generated file executable.
else
  echo "Problem in creating file: \"$OUTFILE\""
fi

#  This method also works for generating
#+ C programs, Perl programs, Python programs, Makefiles,
#+ and the like.

exit 0
```

可以从 *here document* 的输出中设置变量。这实际上是一种命令替换的狡猾形式。

```sh
variable=$(cat <<SETVAR
This variable
runs over multiple lines.
SETVAR
)

echo "$variable"
```

*here document* 可以向同一脚本中的函数提供输入。

**示例 19-9\. Here documents 和函数**

```sh
#!/bin/bash
# here-function.sh

GetPersonalData ()
{
  read firstname
  read lastname
  read address
  read city 
  read state 
  read zipcode
} # This certainly appears to be an interactive function, but . . .

# Supply input to the above function.
GetPersonalData <<RECORD001
Bozo
Bozeman
2726 Nondescript Dr.
Bozeman
MT
21226
RECORD001

echo
echo "$firstname $lastname"
echo "$address"
echo "$city, $state $zipcode"
echo

exit 0
```

可以使用 : 作为接受 *here document* 输出的虚拟命令。这实际上创建了一个 "匿名" *here document*。

**示例 19-10\. "匿名" Here Document**

```sh
#!/bin/bash

: <<TESTVARIABLES
${HOSTNAME?}${USER?}${MAIL?}  # Print error message if one of the variables not set.
TESTVARIABLES

exit $?
```
| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 上述技术的变体允许 "注释" 代码块。 |

**示例 19-11\. 注释代码块**

```sh
#!/bin/bash
# commentblock.sh

: <<COMMENTBLOCK
echo "This line will not echo."
This is a comment line missing the "#" prefix.
This is another comment line missing the "#" prefix.

&*@!!++=
The above line will cause no error message,
because the Bash interpreter will ignore it.
COMMENTBLOCK

echo "Exit value of above \"COMMENTBLOCK\" is $?."   # 0
# No error shown.
echo

#  The above technique also comes in useful for commenting out
#+ a block of working code for debugging purposes.
#  This saves having to put a "#" at the beginning of each line,
#+ then having to go back and delete each "#" later.
#  Note that the use of of colon, above, is optional.

echo "Just before commented-out code block."
#  The lines of code between the double-dashed lines will not execute.
#  ===================================================================
: <<DEBUGXXX
for file in *
do
 cat "$file"
done
DEBUGXXX
#  ===================================================================
echo "Just after commented-out code block."

exit 0

######################################################################
#  Note, however, that if a bracketed variable is contained within
#+ the commented-out code block,
#+ then this could cause problems.
#  for example:

#/!/bin/bash

  : <<COMMENTBLOCK
  echo "This line will not echo."
  &*@!!++=
  ${foo_bar_bazz?}
  $(rm -rf /tmp/foobar/)
  $(touch my_build_directory/cups/Makefile)
COMMENTBLOCK

$ sh commented-bad.sh
commented-bad.sh: line 3: foo_bar_bazz: parameter null or not set

# The remedy for this is to strong-quote the 'COMMENTBLOCK' in line 49, above.

  : <<'COMMENTBLOCK'

# Thank you, Kurt Pfeifle, for pointing this out.
```
| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 这个巧妙技巧的另一个变化使得 "自文档" 脚本成为可能。 |

**示例 19-12\. 自文档脚本**

```sh
#!/bin/bash
# self-document.sh: self-documenting script
# Modification of "colm.sh".

DOC_REQUEST=70

if [ "$1" = "-h"  -o "$1" = "--help" ]     # Request help.
then
  echo; echo "Usage: $0 [directory-name]"; echo
  sed --silent -e '/DOCUMENTATIONXX$/,/^DOCUMENTATIONXX$/p' "$0" &#124;
  sed -e '/DOCUMENTATIONXX$/d'; exit $DOC_REQUEST; fi

: <<DOCUMENTATIONXX
List the statistics of a specified directory in tabular format.
---------------------------------------------------------------
The command-line parameter gives the directory to be listed.
If no directory specified or directory specified cannot be read,
then list the current working directory.

DOCUMENTATIONXX

if [ -z "$1" -o ! -r "$1" ]
then
  directory=.
else
  directory="$1"
fi  

echo "Listing of "$directory":"; echo
(printf "PERMISSIONS LINKS OWNER GROUP SIZE MONTH DAY HH:MM PROG-NAME\n" \
; ls -l "$directory" &#124; sed 1d) &#124; column -t

exit 0
```

使用 cat 脚本是完成此任务的另一种方法。

```sh
DOC_REQUEST=70

if [ "$1" = "-h"  -o "$1" = "--help" ]     # Request help.
then                                       # Use a "cat script" . . .
  cat <<DOCUMENTATIONXX
List the statistics of a specified directory in tabular format.
---------------------------------------------------------------
The command-line parameter gives the directory to be listed.
If no directory specified or directory specified cannot be read,
then list the current working directory.

DOCUMENTATIONXX
exit $DOC_REQUEST
fi
```

参见示例 A-28、示例 A-40、示例 A-41 和示例 A-42 以获取更多自文档脚本示例。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 在这里，文档会创建临时文件，但这些文件在打开后会被删除，并且无法被任何其他进程访问。

&#124;  ```sh bash$ **bash -c 'lsof -a -p $$ -d0' << EOF**
> **EOF**
lsof    1213 bozo    0r   REG    3,5    0 30386 /tmp/t1213-0-sh (deleted)

```  &#124;

|

| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 一些实用工具在 *here document* 内部可能无法正常工作。 |
| --- | --- |

| ![注意](img/9d1b374742c319dd43bb1c5d95d6e008.png) | 在 *here document* 的最后一行中的 *限制字符串* 必须从 *第一个* 字符位置开始。*不能有前导空格*。限制字符串后面的空格同样会导致意外的行为。空格阻止了限制字符串被识别。[[1]](#FTN.AEN17822)|

&#124;  ```sh #!/bin/bash

echo "----------------------------------------------------------------------"

cat <<LimitString
echo "This is line 1 of the message inside the here document."
echo "This is line 2 of the message inside the here document."
echo "This is the final line of the message inside the here document."
     LimitString
#^^^^Indented limit string. Error! This script will not behave as expected.

echo "----------------------------------------------------------------------"

#  These comments are outside the 'here document',
#+ and should not echo.

echo "Outside the here document."

exit 0

echo "This line had better not echo."  # Follows an 'exit' command.
```  &#124;

|

| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 有些人非常巧妙地使用单个 ! 作为限制字符串。但，这不一定是个好主意。|

&#124;  ```sh # This works.
cat <<!
Hello!
! Three more exclamations !!!
!

# But . . .
cat <<!
Hello!
Single exclamation point follows!
!
!
# Crashes with an error message.

# However, the following will work.
cat <<EOF
Hello!
Single exclamation point follows!
!
EOF
# It's safer to use a multi-character limit string.
```  &#124;

|

对于过于复杂的 *here document* 任务，考虑使用专门为向交互式程序输入输入而设计的 `*expect*` 脚本语言。

### 注意事项

| [[1]](here-docs.html#AEN17822) | 除了，正如 Dennis Benzinger 指出的，如果使用 **<<-** 来抑制制表符。 |
| --- | --- |
