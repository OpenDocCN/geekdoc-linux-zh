# Shell 脚本

> 原文：[`learnbyexample.github.io/cli-computing/shell-scripting.html`](https://learnbyexample.github.io/cli-computing/shell-scripting.html)

本章将介绍使用 `bash` 的 Shell 脚本基础知识。您将学习如何声明变量、控制结构、处理传递给脚本的参数、获取用户输入等等。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含了本章讨论的所有 Shell 脚本。然而，建议您使用您喜欢的文本编辑器手动输入脚本，并在必要时才参考 `example_files/shell_scripting` 目录。

## 脚本需求

来自 [wikipedia: Scripting language](https://en.wikipedia.org/wiki/Scripting_language):

> 脚本语言或脚本语言是一种用于运行时系统的编程语言，它自动化了本应由人类操作员单独执行的任务。脚本语言通常在运行时解释，而不是编译。
> 
> 典型的脚本语言旨在非常快速地学习和编写，无论是作为短源代码文件还是通过交互式地读取-评估-打印循环（REPL，语言壳）。这通常意味着相对简单的语法和语义；通常，“脚本”（用脚本语言编写的代码）从头到尾作为“脚本”执行，没有明确的入口点。

来自 [wikipedia: Shell script](https://en.wikipedia.org/wiki/Shell_script):

> Shell 脚本是一种为 Unix Shell（一种命令行解释器）设计的计算机程序。Shell 脚本的多种方言被认为是脚本语言。Shell 脚本通常执行的操作包括文件操作、程序执行和打印文本。设置环境、运行程序以及进行任何必要的清理或记录的脚本被称为包装器。

参见 [脚本与编程语言之间的区别](https://stackoverflow.com/q/17253545/4082052)。

## 可执行脚本

您可以从文件中执行命令的几种方式。本节展示了创建可执行脚本的一个示例。考虑以下示例脚本，它保存在名为 `hello.sh` 的文件中：

```sh
#!/bin/bash

echo "Hello $USER"
echo "Today is $(date -u +%A)"
echo 'Have a nice day' 
```

上面的脚本的第一行包含两部分：

+   `/bin/bash` 是 `bash` 解释器的路径

    +   您可以使用 `type bash` 来获取系统上的路径

+   `#!`被称为[shebang 或 hashbang](https://en.wikipedia.org/wiki/Shebang_(Unix))，它指示程序加载器使用提供的解释器路径

    +   参见 [stackoverflow: #!/usr/bin/env 与 #!/bin/bash 之间的比较？](https://stackoverflow.com/q/21612980/4082052)

    +   `#`字符开始一个注释，`#!`仅在脚本的开始处是特殊的

使用 `chmod` 为文件添加可执行权限，然后运行脚本：

```sh
$ chmod +x hello.sh

$ ./hello.sh
Hello learnbyexample
Today is Wednesday
Have a nice day 
```

如果您只想使用脚本名称来执行它，该文件必须位于 `PATH` 文件夹之一中。否则，您必须提供脚本路径（绝对或相对），以便执行（如上图所示）。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) `.sh` 通常用作 shell 脚本的文件扩展名。不使用扩展名也很常见，尤其是对于可执行脚本。

## 传递文件参数给 bash

您还可以将常规文件作为参数传递给 `bash` 命令。在这种情况下，不需要 shebang（尽管它也不会造成任何问题，因为它将被视为注释）。

```sh
$ cat greeting.sh
echo 'hello'
echo 'have a nice day'

$ bash greeting.sh
hello
have a nice day 
```

## source 脚本

另一种执行脚本的方法是使用 `source`（或 `.`）内置命令来 *source* 它。与之前的方法相比，主要区别在于脚本是在当前 shell 环境上下文中执行，而不是在子 shell 中执行。一个常见的用例是 source `~/.bashrc` 和别名/函数（如果它们保存在单独的文件中）。

这里有一个示例：

```sh
$ cat prev_cmd.sh
prev=$(fc -ln -2 | sed 's/^\s*//; q')
echo "$prev"

# 'echo' here is just a sample command for illustration purposes
$ echo 'hello'
hello
# sourcing the script correctly gives the previous command
$ source prev_cmd.sh
echo 'hello'

$ echo 'hello'
hello
# no output when the script is executed in a sub-shell
$ bash prev_cmd.sh 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) `fc` 是一个用于操作从终端使用的命令历史的内置命令。有关更多详细信息，请参阅 [bash 手册：历史内置命令](https://www.gnu.org/software/bash/manual/bash.html#Bash-History-Builtins)。

## 注释

单行注释可以插入到 `#` 字符之后，可以是行的开头或指令之后。

```sh
$ cat comments.sh
# this is a comment on its own line
echo 'hello' # and this is a comment after a command

$ bash comments.sh
hello 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 有关模拟多行注释的信息，请参阅 [unix.stackexchange 线程](https://unix.stackexchange.com/q/37411/109046)。

## 变量

这里有一个基本示例，展示如何分配变量和访问其值：

```sh
# note that there cannot be any space characters around the = operator
$ name='learnbyexample'

$ echo "$name"
learnbyexample 
```

如上所示，在访问存储在变量中的值时，需要使用 `$` 前缀。您可以使用 `${variable}` 语法来区分变量和字符串的其他部分。除非必要，否则建议使用适当的引号。

您可以使用 `+=` 运算符向变量追加内容。以下是一个示例：

```sh
$ colors='blue'
$ echo "$colors"
blue

$ colors+=' green'
$ echo "$colors"
blue green 
```

您可以使用 `declare` 内置命令向变量添加属性。例如，`-i` 选项用于将变量作为整数处理，`-r` 选项用于只读等。这些属性可以改变这些变量的 `=` 和 `+=` 运算符的行为。有关更多详细信息，请参阅 [bash 手册：Shell 参数](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameters) 和 [bash 手册：declare](https://www.gnu.org/software/bash/manual/bash.html#index-declare)。

```sh
$ declare -i num=5
$ echo "$num"
5
$ num+=42
$ echo "$num"
47

$ declare -r color='brown'
$ echo "$color"
brown
$ color+=' green'
bash: color: readonly variable 
```

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) ![警告](img/b5314b4a0acf0f436c4bf59486793342.png) 赋值变量是错误最常见的来源之一。与大多数编程语言不同，`=` 符号周围不允许有空格。这是因为空格是一个 shell 元字符。另一个常见问题是使用（或不使用）引号来包围值。以下是一些示例：
> 
> ```sh
> $ num = 42
> num: command not found
> 
> $ greeting=hello world
> world: command not found
> $ greeting='hello world'
> $ echo "$greeting"
> hello world
> 
> # using quotes is NOT desirable here
> $ dir_path=~/reports
> $ echo "$dir_path"
> /home/learnbyexample/reports
> $ dir_path='~/reports'
> $ echo "$dir_path"
> ~/reports 
> ```

## 数组

来自 [bash 手册：数组](https://www.gnu.org/software/bash/manual/bash.html#Arrays)：

> Bash 提供了一维索引和关联数组变量。任何变量都可以用作索引数组；`declare` 内置命令将显式声明一个数组。数组的大小没有最大限制，成员也不需要连续索引或分配。索引数组使用整数引用，且从零开始；关联数组使用任意字符串。

下面是一个分配索引数组和访问元素的各种方式的示例：

```sh
$ fruits=('apple' 'fig' 'mango')

# first element
$ echo "${fruits[0]}"
apple

# last element
$ echo "${fruits[-1]}"
mango

# all elements (example with for loop will be discussed later on)
$ echo "${fruits[@]}"
apple fig mango
$ printf '%s\n' "${fruits[@]}"
apple
fig
mango 
```

## 参数扩展

Bash 提供了几种有用的方法来提取和修改参数和变量（包括数组）的内容。本节将讨论这些功能的一些内容。

*1)* 使用 `${parameter:offset}` 语法提取从给定索引开始的所有字符：

```sh
$ city='Lucknow'

# all characters from index 4 onwards
# indexing starts from 0
$ echo "${city:4}"
now

# last two characters
# space before the negative sign is compulsory here,
# since ${parameter:-word} is a different feature
$ echo "${city: -2}"
ow 
```

当应用于数组时，子字符串提取将给出以下元素：

```sh
$ fruits=('apple' 'fig' 'mango')

# all elements from index 1
$ echo "${fruits[@]:1}"
fig mango 
```

*2)* 使用 `${parameter:offset:length}` 语法提取特定数量的字符，从给定的索引开始：

```sh
$ city='Lucknow'

# 4 characters starting from index 0
# can also use: echo "${city::4}"
$ echo "${city:0:4}"
Luck

# 2 characters starting from index -4 (4th character from the end)
$ echo "${city: -4:2}"
kn

# except the last 2 characters
$ echo "${city::-2}"
Luckn 
```

*3)* `${#parameter}` 将给出字符串的长度，而 `${#array[@]}` 将给出数组中的元素数量：

```sh
$ city='Lucknow'
$ echo "${#city}"
7

$ fruits=('apple' 'fig' 'mango')
$ echo "${#fruits[@]}"
3 
```

*4)* `${parameter#glob}` 将从字符串的开始处移除最短的匹配项。如果您通过 `shopt` 内置命令启用了扩展通配符，您也可以使用扩展通配符。`${parameter##glob}` 将从字符串的开始处移除最长的匹配项。以下是一些示例：

```sh
$ s='this is his life history'

# shortest match is deleted
$ echo "${s#*is}"
 is his life history
# longest match is deleted
$ echo "${s##*is}"
tory

# assuming extglob is already enabled
$ echo "${s#+([^ ])}"
his is his life history
$ echo "${s##+([^ ])}"
 is his life history

# for arrays, the processing is applied to each element
$ fruits=('apple' 'fig' 'mango')
$ echo "${fruits[@]#*[aeiou]}"
pple g ngo 
```

*5)* 您可以使用 `${parameter%glob}` 从字符串的末尾移除最短的匹配项。`${parameter%%glob}` 将从字符串的末尾移除最长的匹配项。

```sh
$ s='this is his life history'

$ echo "${s%is*}"
this is his life h
$ echo "${s%%is*}"
th

$ fruits=('apple' 'fig' 'mango')
$ echo "${fruits[@]%[aeiou]*}"
appl f mang 
```

*6)* `${parameter/glob/string}` 将替换第一个匹配项为给定的替换字符串，而 `${parameter//glob/string}` 将替换所有匹配项。当您想要删除匹配项时，可以省略 `/string` 部分。`glob` 将匹配最长部分，类似于正则表达式中的贪婪行为。以下是一些示例：

```sh
$ ip='this is a sample string'

# first occurrence of 'is' is replaced with '123'
$ echo "${ip/is/123}"
th123 is a sample string
# all occurrences of 'is' are replaced with '123'
$ echo "${ip//is/123}"
th123 123 a sample string

# replace all occurrences of 'am' or 'in' with '-'
$ echo "${ip//@(am|in)/-}"
this is a s-ple str-g

# matches from the first 'is' to the last 's' in the input
$ echo "${ip/is*s/ X }"
th X tring

# delete the first occurrence of 's'
$ echo "${ip/s}"
thi is a sample string
# delete all the occurrences of 's'
$ echo "${ip//s}"
thi i a ample tring 
```

*7)* 您可以使用 `${parameter/#glob/string}` 仅在字符串的开始处进行匹配，并使用 `${parameter/%glob/string}` 仅在字符串的末尾进行匹配。

```sh
$ ip='spare'

# remove only from the start of the string
$ echo "${ip/#sp}"
are
$ echo "${ip/#par}"
spare
# example with replacement string
$ echo "${ip/#sp/fl}"
flare

# remove only from the end of the string
$ echo "${ip/%re}"
spa
$ echo "${ip/%par}"
spare 
```

*8)* `${parameter^glob}` 如果匹配 glob，则只能将第一个字符转换为大写。`${parameter^^glob}` 将所有匹配的字符转换为大写（输入字符串的任何位置）。您应该提供一个只匹配一个字符长度的 glob。如果省略 glob，则整个参数将被匹配。这些规则也适用于稍后讨论的小写和大小写转换版本。

```sh
$ fruit='apple'

# uppercase the first character
$ echo "${fruit^}"
Apple
# uppercase the entire parameter
$ echo "${fruit^^}"
APPLE

# first character doesn't match the 'g-z' range, so no change
$ echo "${fruit^[g-z]}"
apple
# uppercase all letters in the 'g-z' range
$ echo "${fruit^^[g-z]}"
aPPLe
# uppercase all letters in the 'a-e' or 'j-m' ranges
$ echo "${fruit^^[a-ej-m]}"
AppLE

# this won't work since 'sky-' is not a single character
$ color='sky-rose'
$ echo "${color^^*-}"
sky-rose 
```

*9)* 要将字符转换为小写，使用 `,` 和 `,,` 如下所示：

```sh
$ fruit='APPLE'

$ echo "${fruit,}"
aPPLE
$ echo "${fruit,,}"
apple

$ echo "${fruit,,[G-Z]}"
ApplE 
```

*10)* 要交换大小写，使用 `~` 和 `~~` 如下所示。注意，这似乎已被弃用，因为它不再在 `bash` 手册中提及。

```sh
$ fruit='aPPle'

# swap case only the first character
$ echo "${fruit~}"
APPle
# swap case all the characters
$ echo "${fruit~~}"
AppLE

# swap case characters matching the given character set
$ echo "${fruit~~[g-zG-Z]}"
appLe 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 有关更多详细信息和其他类型的展开，请参阅 [bash 手册：Shell 参数展开](https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion)。

## 命令行参数

传递给脚本（或函数）的命令行参数将保存为以 `1`、`2`、`3` 等开始的定位参数。`0` 包含 shell 或 shell 脚本名称。`@` 包含从 `1` 开始的所有定位参数。使用 `#` 获取定位参数的数量。类似于变量，您需要使用 `$` 前缀来获取这些参数中存储的值。如果参数编号需要超过单个数字，则必须将它们括在 `{}` 中（例如，`${12}` 以获取第十二个参数的值）。

这里有一个接受两个参数的示例脚本：

```sh
$ cat command_line_arguments.sh
echo "No. of lines in '$1' is $(wc -l < "$1")"
echo "No. of lines in '$2' is $(wc -l < "$2")"

$ seq 12 > 'test file.txt'

$ bash command_line_arguments.sh hello.sh test\ file.txt
No. of lines in 'hello.sh' is 5
No. of lines in 'test file.txt' is 12 
```

**进一步阅读**

+   [unix.stackexchange：shell 脚本在空白或其他特殊字符上卡住](https://unix.stackexchange.com/q/131766/109046)

+   [bash 手册：特殊参数](https://www.gnu.org/software/bash/manual/bash.html#Special-Parameters)

## 条件表达式

您可以在 `[[` 和 `]]` 内测试条件，以获取成功（`0`）或失败（`1` 或更高）的退出状态，并据此采取行动。Bash 提供了您可以使用的一些选项和运算符。在此复合命令中，`[[` 和 `]]` 之间需要有空格。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 在本节中，将使用运算符 `;`、`&&` 和 `||` 以使示例更简洁。`if-else` 和其他控制结构将在稍后讨论。

### 选项

`-e` 选项检查给定的路径参数是否存在。添加一个 `!` 前缀以否定条件。

```sh
# change to the 'example_files/shell_scripting' directory for this section

$ [[ -e hello.sh ]] && echo 'found' || echo 'not found'
found

$ [[ -e xyz.txt ]] && echo 'found' || echo 'not found'
not found

# exit status
$ [[ -e hello.sh ]] ; echo $?
0
$ [[ -e xyz.txt ]] ; echo $?
1
$ [[ ! -e xyz.txt ]] ; echo $?
0 
```

您可以使用 `-d` 和 `-f` 选项分别检查路径是否为有效目录和文件。`-s` 选项检查文件是否存在且大小大于零。`-x` 选项检查文件是否存在且可执行。有关此类选项的完整列表，请参阅 `help test` 和 [bash 手册：条件表达式](https://www.gnu.org/software/bash/manual/bash.html#Bash-Conditional-Expressions)。

### 字符串比较

+   `s1 = s2` 或 `s1 == s2` 检查两个字符串是否相等

    +   `s2`未引用的部分在测试与`s1`时将被视为通配符

    +   对于此类比较，`extglob`将被视为已启用

+   `s1 != s2`检查字符串是否**不**相等

    +   `s2`未引用的部分在测试与`s1`时将被视为通配符

    +   对于此类比较，`extglob`将被视为已启用

+   `s1 < s2`检查`s1`在字典序上是否排在`s2`之前

+   `s1 > s2`检查`s1`在字典序上是否排在`s2`之后

+   `s1 =~ s2`检查`s1`是否与`s2`提供的 POSIX 扩展正则表达式匹配

    +   如果`s2`不是一个有效的正则表达式，则退出状态将为`2`

以下是一些等于和不等于比较的示例：

```sh
$ fruit='apple'
$ [[ $fruit == 'apple' ]] && echo 'true' || echo 'false'
true
$ [[ $fruit == 'banana' ]] && echo 'true' || echo 'false'
false

# glob should be constructed to match the entire string
$ [[ hello == h* ]] && echo 'true' || echo 'false'
true
# don't quote the glob!
$ [[ hello == 'h*' ]] && echo 'true' || echo 'false'
false

# another example to emphasize that the glob should match the entire string
$ [[ hello == e*o ]] && echo 'true' || echo 'false'
false
$ [[ hello == *e*o ]] && echo 'true' || echo 'false'
true

$ [[ hello != *a* ]] && echo 'true' || echo 'false'
true
$ [[ hello != *e* ]] && echo 'true' || echo 'false'
false 
```

以下是一些大于和小于比较的示例：

```sh
$ [[ apple < banana ]] && echo 'true' || echo 'false'
true
$ [[ par < part ]] && echo 'true' || echo 'false'
true

$ [[ mango > banana ]] && echo 'true' || echo 'false'
true
$ [[ sun > moon && fig < papaya ]] && echo 'true' || echo 'false'
true

# don't use this to compare numbers!
$ [[ 20 > 3 ]] && echo 'true' || echo 'false'
false
# -gt and other such operators will be discussed later
$ [[ 20 -gt 3 ]] && echo 'true' || echo 'false'
true 
```

以下是一些正则表达式比较的示例。你可以使用特殊的数组`BASH_REMATCH`来检索匹配的字符串的特定部分。索引`0`给出整个匹配部分，`1`给出第一个捕获组匹配的部分，依此类推。

```sh
$ fruit='apple'
$ [[ $fruit =~ ^a ]] && echo 'true' || echo 'false'
true
$ [[ $fruit =~ ^b ]] && echo 'true' || echo 'false'
false

# entire matched portion
$ [[ $fruit =~ a.. ]] && echo "${BASH_REMATCH[0]}"
app
# portion matched by the first capture group
$ [[ $fruit =~ a(..) ]] && echo "${BASH_REMATCH[1]}"
pp 
```

### Numeric comparisons

+   `n1 -eq n2`检查两个数字是否相等

+   `n1 -ne n2`检查两个数字是否**不**相等

+   `n1 -gt n2`检查`n1`是否大于`n2`

+   `n1 -ge n2`检查`n1`是否大于或等于`n2`

+   `n1 -lt n2`检查`n1`是否小于`n2`

+   `n1 -le n2`检查`n1`是否小于或等于`n2`

这些运算符仅支持整数比较。

```sh
$ [[ 20 -gt 3 ]] && echo 'true' || echo 'false'
true

$ n1='42'
$ n2='25'
$ [[ $n1 -gt 30 && $n2 -lt 12 ]] && echo 'true' || echo 'false'
false 
```

在`((`和`))`复合命令中也可以执行数值算术运算和比较。以下是一些示例比较：

```sh
$ (( 20 > 3 )) && echo 'true' || echo 'false'

$ n1='42'
$ n2='25'
$ (( n1 > 30 && n2 < 12 )) && echo 'true' || echo 'false'
false 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 注意，在上面的示例中未使用`$`前缀来表示变量。有关更多详细信息，请参阅[bash 手册：Shell 算术](https://www.gnu.org/software/bash/manual/bash.html#Shell-Arithmetic)。

## Accepting user input interactively

你可以使用内置的`read`命令来交互式地接受用户输入。如果将多个变量作为`read`命令的参数，则默认根据空白分隔分配值。任何挂起的值都将分配给最后一个变量。以下是一些示例：

```sh
# press 'Enter' after the 'read' command
# and also after you've finished entering the input
$ read color
light green
$ echo "$color"
light green

# example with multiple variables
$ read fruit qty
apple 10
$ echo "${fruit}: ${qty}"
apple: 10 
```

`-p`选项可以帮助你添加用户提示。以下是从用户那里获取两个参数的示例：

```sh
$ cat user_input.sh
read -p 'Enter two integers separated by spaces: ' num1 num2
sum=$(( num1 + num2 ))
echo "$num1 + $num2 = $sum"

$ bash user_input.sh
Enter two integers separated by spaces: -2 42
-2 + 42 = 40 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你可以使用`-a`选项来分配数组，使用`-d`选项来指定自定义分隔符以代替换行符来终止用户输入等。有关更多详细信息，请参阅`help read`和[bash 手册：内置命令](https://www.gnu.org/software/bash/manual/bash.html#Bash-Builtins)。

## if then else

构造`if`控制结构所需的关键字是`if`、`then`、`fi`，可选的还有`else`和`elif`。你可以使用`[[`和`((`这样的复合命令来提供测试条件。你也可以直接使用命令的退出状态。以下是一个示例脚本：

```sh
$ cat if_then_else.sh
if (( $# != 1 )) ; then
    echo 'Error! One file argument expected.' 1>&2
    exit 1
else
    if [[ ! -f $1 ]] ; then
        printf 'Error! %q is not a valid file\n' "$1" 1>&2
        exit 1
    else
        echo "No. of lines in '$1' is $(wc -l < "$1")"
    fi
fi 
```

在上述脚本中，`1>&2` 用于将错误消息重定向到 `stderr` 流。以下是一些脚本调用示例：

```sh
$ bash if_then_else.sh
Error! One file argument expected.
$ echo $?
1

$ bash if_then_else.sh xyz.txt
Error! xyz.txt is not a valid file
$ echo $?
1

$ bash if_then_else.sh hello.sh
No. of lines in 'hello.sh' is 5
$ echo $?
0 
```

有时你只需要知道预期的命令操作是否成功，然后根据结果采取行动。在这种情况下，你可以在 `if` 关键字之后直接提供命令。请注意，除非使用适当的选项重定向或抑制，否则命令的 `stdout` 和 `stderr` 仍然会保持活跃。

例如，`grep` 命令支持 `-q` 选项来抑制 `stdout`。以下是一个使用该功能的脚本示例：

```sh
$ cat search.sh
read -p 'Enter a search pattern: ' search

if grep -q "$search" hello.sh ; then
    echo "match found"
else
    echo "match not found"
fi 
```

上述脚本的调用示例：

```sh
$ bash search.sh
Enter a search pattern: echo
match found

$ bash search.sh
Enter a search pattern: xyz
match not found 
```

## for 循环

要构建一个 `for` 循环，你需要 `for`、`do` 和 `done` 关键字。以下是一些示例：

```sh
# iterate over numbers generated using brace expansion
$ for num in {2..4}; do echo "$num"; done
2
3
4

# iterate over files matched using wildcards
# echo is used here for dry run testing
$ for file in [gh]*.sh; do echo mv "$file" "$file.bkp"; done
mv greeting.sh greeting.sh.bkp
mv hello.sh hello.sh.bkp 
```

如上述示例所示，在 `in` 关键字之后提供的空格分隔的参数将在每次迭代时自动分配给 `for` 关键字之后提供的变量。

以下是对上一个示例的修改版本，它接受用户提供的命令行参数：

```sh
$ cat for_loop.sh
for file in "$@"; do
    echo mv "$file" "$file.bkp"
done

$ bash for_loop.sh [gh]*.sh
mv greeting.sh greeting.sh.bkp
mv hello.sh hello.sh.bkp

$ bash for_loop.sh report.log ip.txt fruits.txt
mv report.log report.log.bkp
mv ip.txt ip.txt.bkp
mv fruits.txt fruits.txt.bkp 
```

这是一个遍历数组的示例：

```sh
$ files=('report.log' 'pass_list.txt')
$ for f in "${files[@]}"; do echo "$f"; done
report.log
pass_list.txt 
```

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你可以使用 `continue` 和 `break` 根据特定条件改变循环流程。请参阅 [bash 手册：Bourne Shell 内置命令](https://www.gnu.org/software/bash/manual/bash.html#Bourne-Shell-Builtins) 以获取更多详细信息。
> 
> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) `for file;` 与 `for file in "$@";` 相同，因为 `in "$@"` 是默认值。我建议使用显式版本。

## while 循环

这是一个简单的 `while` 循环结构。你将在本章后面看到更实用的示例。

```sh
$ cat while_loop.sh
i="$1"
while (( i > 0 )) ; do
    echo "$i"
    (( i-- ))
done

$ bash while_loop.sh 3
3
2
1 
```

## 读取文件

将 `while` 循环与内置的 `read` 命令结合使用可以处理文件的正文内容。以下是一个逐行读取输入内容的示例：

```sh
$ cat read_file_lines.sh
while IFS= read -r line; do
    # do something with each line
    wc -l "$line"
done < "$1"

$ printf 'hello.sh\ngreeting.sh\n' > files.txt
$ bash read_file_lines.sh files.txt
5 hello.sh
2 greeting.sh 
```

在上述脚本中的意图是逐字处理每个输入行。因此，将 `IFS`（输入字段分隔符）特殊变量设置为空字符串，以防止删除前导和尾随空格。`read` 命令的 `-r` 选项允许输入中的 `\` 被逐字处理。请注意，输入文件名作为第一个命令行参数接受，并重定向为 `stdin` 到 `while` 循环。你还需要确保输入的最后一行以换行符结束，否则最后一行将不会被处理。

你可以将 `IFS` 改变以将输入行分割成不同的字段，并指定适当的变量数量给 `read` 命令。以下是一个示例：

```sh
$ cat read_file_fields.sh
while IFS=' : ' read -r field1 field2; do
    echo "$field2,$field1"
done < "$1"

$ bash read_file_fields.sh <(printf 'apple : 3\nfig : 100\n')
3,apple
100,fig 
```

你可以向 `read` 命令的 `-n` 选项传递一个数字，以便每次处理这么多字符的输入。以下是一个示例：

```sh
$ while read -r -n2 ip; do echo "$ip"; done <<< '\word'
\w
or
d 
```

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) `xargs` 命令也可以用于上述讨论的一些情况。请参阅 [unix.stackexchange: 将文本文件的每一行作为命令参数解析](https://unix.stackexchange.com/q/149726/109046) 以获取示例。

## 函数

来自 [bash 手册：Shell 函数](https://www.gnu.org/software/bash/manual/bash.html#Shell-Functions)：

> Shell 函数是一种将命令分组以便以后使用单个名称执行的方法。它们就像“常规”命令一样执行。当使用 shell 函数的名称作为简单命令名称时，将执行与该函数名称关联的命令列表。Shell 函数在当前 shell 上下文中执行；不会创建新进程来解释它们。

您可以使用以下任一语法来声明函数：

```sh
fname () compound-command [ redirections ]

function fname [()] compound-command [ redirections ] 
```

函数的参数传递方式与之前讨论的 shell 脚本中的方式相同。以下是一个例子：

```sh
$ cat functions.sh
add_border ()
{
    size='10'
    color='grey'
    if (( $# == 1 )) ; then
        ip="$1"
    elif (( $# == 2 )) ; then
        if [[ $1 =~ ^[0-9]+$ ]] ; then
            size="$1"
        else
            color="$1"
        fi
        ip="$2"
    else
        size="$1"
        color="$2"
        ip="$3"
    fi

    op="${ip%.*}_border.${ip##*.}"
    echo convert -border "$size" -bordercolor "$color" "$ip" "$op"
}

add_border flower.png
add_border 5 insect.png
add_border red lake.png
add_border 20 blue sky.png 
```

在上述例子中，`echo` 用于显示将要执行的命令。如果您想使此脚本实际使用给定参数创建新图像，请删除 `echo`。该函数接受一到三个参数，并在某些参数未传递时使用默认值。以下是输出：

```sh
$ bash functions.sh
convert -border 10 -bordercolor grey flower.png flower_border.png
convert -border 5 -bordercolor grey insect.png insect_border.png
convert -border 10 -bordercolor red lake.png lake_border.png
convert -border 20 -bordercolor blue sky.png sky_border.png 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如果您想就地修改输入图像而不是创建新图像，请使用 `mogrify` 而不是 `convert`。这些图像处理命令是 [ImageMagick](https://imagemagick.org/) 套件的一部分。作为练习，修改上述函数以在传递的参数与预期用法不匹配时生成错误。您还可以接受一个输出图像名称（或可能是一个不同的后缀）作为附加参数。

shell 脚本和用户定义的函数（这些函数可能又会调用自身或另一个函数）都可以有位置参数。在这种情况下，shell 会确保在函数完成任务后恢复位置参数到早期状态。

函数也有退出状态，默认情况下基于最后执行的命令。您可以使用 `return` 内置命令提供自己的自定义退出状态。

## 调试

您可以使用以下 `bash` 选项进行调试：

+   `-x` 在执行时打印命令及其参数

+   `-v` 详细选项，打印读取的 shell 输入行

这里有一个使用 `bash -x` 选项的例子：

```sh
$ bash -x search.sh
+ read -p 'Enter a search pattern: ' search
Enter a search pattern: xyz
+ grep -q xyz hello.sh
+ echo 'match not found'
match not found 
```

以 `+` 开头的行显示了如果适用（例如，`search` 变量用于 `grep -q`）正在执行的命令及其展开值。如果有多个展开，将使用多个 `+`。以下是 `bash -xv` 对同一脚本的行为：

```sh
$ bash -xv search.sh
read -p 'Enter a search pattern: ' search
+ read -p 'Enter a search pattern: ' search
Enter a search pattern: xyz

if grep -q "$search" hello.sh ; then
    echo "match found"
else
    echo "match not found"
fi
+ grep -q xyz hello.sh
+ echo 'match not found'
match not found 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 您也可以在脚本内部使用 `set -x` 或 `set -v` 或 `set -xv` 从特定点开始进行调试。您可以通过使用 `+` 而不是 `-` 作为选项前缀来关闭此类调试（例如，`set +x`）。

## shellcheck

[shellcheck](https://www.shellcheck.net/) 是一个静态分析工具，为脚本提供警告和建议。您可以在网上使用它，或者安装工具以供离线使用。鉴于各种 `bash` 的陷阱，此工具非常推荐初学者和高级用户使用。

考虑以下脚本：

```sh
$ cat bad_script.sh
#!/bin/bash

greeting = 'hello world'
echo "$greeting" 
```

这是 `shellcheck` 报告问题的方法：

```sh
$ shellcheck bad_script.sh

In bad_script.sh line 3:
greeting = 'hello world'
         ^-- SC1068: Don't put spaces around the = in assignments
                     (or quote to make it literal).

For more information:
  https://www.shellcheck.net/wiki/SC1068 -- Don't put spaces around the = in ... 
```

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如果脚本没有 shebang，您可以使用 `-s` 选项（例如 `shellcheck -s bash`）来指定 shell 应用程序。
> 
> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) ![警告](img/b5314b4a0acf0f436c4bf59486793342.png) 注意，`shellcheck` 不会捕获所有类型的问题。并且，如果没有理解在给定上下文中是否合理，就不应盲目接受建议。

## 资源列表

这里有一些更多的学习资源：

**Shell 脚本**

+   [Bash 指南](https://mywiki.wooledge.org/BashGuide) — 致力于教授使用 Bash 的良好实践技术，以及编写简单的脚本

+   [Bash 脚本教程](https://ryanstutorials.net/bash-scripting-tutorial/) — 在如何编写 Bash 脚本方面打下坚实的基础，以便让计算机为您完成复杂、重复的任务

+   [bash-handbook](https://github.com/denysdovhan/bash-handbook) — 对于那些不想深入研究 Bash 的人来说

+   [严肃的 Shell 编程](https://freebsdfrau.gitbook.io/serious-shell-programming/) — 专注于 POSIX 兼容的 Bourne Shell 以实现可移植性

**实用工具、技巧和参考**

+   [shellcheck](https://www.shellcheck.net/) — 检查工具，以避免常见错误并提高您的脚本质量

+   [Bash 参考速查表](https://devmanual.gentoo.org/tools-reference/bash/index.html) — 格式良好，解释得当

+   [Bash 脚本速查表](https://devhints.io/bash) — Bash 脚本入门的快速参考

+   在 `mywiki.wooledge.org` 网站上的综合列表：

    +   [Bash FAQ](https://mywiki.wooledge.org/BashFAQ)

    +   [Bash 实践](https://mywiki.wooledge.org/BashGuide/Practices)

    +   [Bash 陷阱](https://mywiki.wooledge.org/BashPitfalls)

+   [Google Shell 风格指南](https://google.github.io/styleguide/shellguide.html)

+   可靠性和健壮性

    +   [在 bash 中安全做事的方法](https://github.com/anordal/shellharden/blob/master/how_to_do_things_safely_in_bash.md)

    +   [更好的脚本编写](https://robertmuth.blogspot.com/2012/08/better-bash-scripting-in-15-minutes.html)

    +   [健壮的脚本编写](https://www.davidpashley.com/articles/writing-robust-shell-scripts/)

**特定主题**

+   读取文件

    +   [针对各种用例的健壮的读取文件方法](https://mywiki.wooledge.org/BashFAQ/001)

    +   [并行遍历两个文件的行](https://unix.stackexchange.com/q/82541/109046)

+   [数组](https://mywiki.wooledge.org/BashGuide/Arrays)

+   [nameref](https://unix.stackexchange.com/q/288886/109046)

    +   也请参阅此 [FAQ](https://mywiki.wooledge.org/BashFAQ/006)

+   getopts

    +   [getopts 教程](https://web.archive.org/web/20221226035414/https://wiki.bash-hackers.org/howto/getopts_tutorial)

    +   [处理命令行参数](https://mywiki.wooledge.org/BashFAQ/035)

    +   [stackoverflow: getopts 示例](https://stackoverflow.com/q/16483119/4082052)

+   [发送和捕获信号](https://mywiki.wooledge.org/SignalTrap)

## 练习

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 在尝试练习之前使用临时工作目录。之后可以删除此类练习目录。

**1)** 下面的脚本有什么问题？如果你使用 `bash try.sh` 替代，错误会消失吗？

```sh
$ printf '    \n!#/bin/bash\n\necho hello\n' > try.sh
$ chmod +x try.sh
$ ./try.sh
./try.sh: line 2: !#/bin/bash: No such file or directory
hello

# expected output
$ ./try.sh
hello 
```

**2)** 以下命令会工作吗？如果是，输出会是什么？

```sh
$ echo echo hello | bash 
```

**3)** 在什么情况下你会使用 `source` 脚本而不是使用 `bash` 或通过 shebang 创建可执行文件？

**4)** 你会如何显示带有 `shake` 后缀的变量的内容？

```sh
$ fruit='banana'

$ echo # ???
bananashake 
```

**5)** 你会如何修改以下代码以获得预期的输出？

```sh
# default behavior
$ n=100
$ n+=100
$ echo "$n"
100100

# expected output
$ echo "$n"
200 
```

**6)** 以下代码是否有效？如果是，`echo` 命令的输出会是什么？

```sh
$ declare -a colors
$ colors[3]='green'
$ colors[1]='blue'

$ echo "${colors[@]}"
# ??? 
```

**7)** 你会如何获取一个变量的最后三个字符？

```sh
$ fruit='banana'

# ???
ana 
```

**8)** 第二个 `echo` 命令会出错吗？如果没有，输出会是什么？

```sh
$ fruits=('apple' 'fig' 'mango')
$ echo "${#fruits[@]}"
3

$ echo "${#fruits}"
# ??? 
```

**9)** 对于给定的数组，使用参数扩展来删除直到第一个/最后一个空格的字符。

```sh
$ colors=('green' 'dark brown' 'deep sky blue white')

# remove till the first space
$ printf '%s\n' # ???
green
brown
sky blue white

# remove till the last space
$ printf '%s\n' # ???
green
brown
white 
```

**10)** 使用参数扩展来获取以下显示的预期输出。

```sh
$ ip='apple:banana:cherry:dragon'

$ echo # ???
apple:banana:cherry

$ echo # ???
apple 
```

**11)** 是否可以使用参数扩展来实现以下预期的输出？如果是，如何实现？

```sh
$ ip1='apple:banana:cherry:dragon'
$ ip2='Cradle:Mistborn:Piranesi'

$ echo # ???
apple 42 dragon
$ echo # ???
Cradle 42 Piranesi

$ echo # ???
fig:banana:cherry:dragon
$ echo # ???
fig:Mistborn:Piranesi

$ echo # ???
apple:banana:cherry:end
$ echo # ???
Cradle:Mistborn:end 
```

**12)** 对于给定的输入，按照以下显示的预期输出更改大小写。

```sh
$ ip='This is a Sample STRING'

$ echo # ???
THIS IS A SAMPLE STRING

$ echo # ???
this is a sample string

$ echo # ???
tHIS IS A sAMPLE string 
```

**13)** 为什么下面的条件表达式失败了？

```sh
$ touch ip.txt
$ [[-f ip.txt]] && echo 'file exists'
[[-f: command not found 
```

**14)** `==` 和 `=~` 字符串比较操作符之间的区别是什么？

**15)** 为什么下面的条件表达式两次都显示 `failed`？修改表达式，使得第一个正确显示 `matched` 而不是 `failed`。

```sh
$ f1='1234.txt'
$ f2='report_2.txt'

$ [[ $f1 == '+([0-9]).txt' ]] && echo 'matched' || echo 'failed'
failed
$ [[ $f2 == '+([0-9]).txt' ]] && echo 'matched' || echo 'failed'
failed 
```

**16)** 提取给定变量内容中 `:` 字符后面的数字。

```sh
$ item='chocolate:50'
# ???
50

$ item='50 apples, fig:100, books-12'
# ???
100 
```

**17)** 如何修改以下表达式以正确报告 `true` 而不是 `false`？

```sh
$ num=12345
$ [[ $num > 3 ]] && echo 'true' || echo 'false'
false 
```

**18)** 编写一个名为 `array.sh` 的 shell 脚本，该脚本接受用户输入的数组，然后是另一个作为索引的输入。显示该索引处的对应值。以下显示了几个示例。

```sh
$ bash array.sh
enter array elements: apple banana cherry
enter array index: 1
element at index '1' is: banana

$ bash array.sh
enter array elements: dragon unicorn centaur
enter array index: -1
element at index '-1' is: centaur 
```

**19)** 编写一个名为 `case.sh` 的 shell 脚本，该脚本接受恰好两个命令行参数。第一个参数可以是 `lower`、`upper` 或 `swap`，并应用于转换第二个参数的内容。以下显示了脚本调用示例，包括如果命令行参数不符合脚本预期会发生什么。

```sh
$ ./case.sh upper 'how are you?'
HOW ARE YOU?

$ ./case.sh lower PineAPPLE
pineapple

$ ./case.sh swap 'HeLlo WoRlD'
hElLO wOrLd

$ ./case.sh lower
Error! Two arguments expected.
$ echo $?
1

$ ./case.sh upper apple fig
Error! Two arguments expected.

$ ./case.sh lowercase DRAGON
Error! 'lowercase' command not recognized.
$ echo $?
1

$ ./case.sh apple lower 2> /dev/null
$ echo $?
1 
```

**20)** 编写一个名为 `loop.sh` 的 shell 脚本，该脚本显示作为命令行参数传递的每个文件的行数。

```sh
$ printf 'apple\nbanana\ncherry\n' > items_1.txt
$ printf 'dragon\nowl\nunicorn\ntroll\ncentaur\n' > items_2.txt

$ bash loop.sh items_1.txt
number of lines in 'items_1.txt' is: 3

$ bash loop.sh items_1.txt items_2.txt
number of lines in 'items_1.txt' is: 3
number of lines in 'items_2.txt' is: 5 
```

**21)** 编写一个名为 `read_file.sh` 的 shell 脚本，该脚本逐行读取文件并将其作为参数传递给 `paste -sd,` 命令。你能否也使用 `xargs` 命令而不是脚本编写一个解决方案？

```sh
$ printf 'apple\nbanana\ncherry\n' > items_1.txt
$ printf 'dragon\nowl\nunicorn\ntroll\ncentaur\n' > items_2.txt
$ printf 'items_1.txt\nitems_2.txt\n' > list.txt

$ bash read_file.sh list.txt
apple,banana,cherry
dragon,owl,unicorn,troll,centaur

$ xargs # ???
apple,banana,cherry
dragon,owl,unicorn,troll,centaur 
```

**22)** 编写一个名为 `add_path` 的函数，该函数将当前工作目录的路径添加到它接收的参数之前，并显示结果。以下是一些示例。

```sh
$ add_path() # ???

$ cd
$ pwd
/home/learnbyexample
$ add_path ip.txt report.log
/home/learnbyexample/ip.txt /home/learnbyexample/report.log

$ cd cli-computing
$ pwd
/home/learnbyexample/cli-computing
$ add_path f1
/home/learnbyexample/cli-computing/f1 
```

**23)** 选项 `bash -x` 和 `bash -v` 的作用是什么？

**24)** 什么是 `shellcheck` 以及你会在什么情况下使用它？
