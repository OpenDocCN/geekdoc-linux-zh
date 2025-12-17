# Shell 功能

> 原文：[`learnbyexample.github.io/cli-computing/shell-features.html`](https://learnbyexample.github.io/cli-computing/shell-features.html)

本章重点介绍 Bash shell 功能，如引用机制、通配符、重定向、命令分组、进程替换、命令替换等。其他内容将在后续章节中讨论。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章中使用的脚本和示例输入文件。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 本章中的一些示例使用了将在后续章节中讨论的命令。已在此处添加了此类命令的基本描述，你将在本章的其余部分看到更多示例。

## 引用机制

本节将引用 (*嘿*) 来自 [bash 手册](https://www.gnu.org/software/bash/manual/bash.html#Quoting) 的相关定义，并为四种机制中的每一种提供一些示例。

*1)* **转义字符**

> 未引用的反斜杠 `\` 是 Bash 的转义字符。它保留下一个字符的原始值，除了换行符。
> 
> **元字符**：一个未引用时分隔单词的字符。元字符是空格、制表符、换行符或以下字符之一：`|`、`&`、`;`、`(`、`)`、`<` 或 `>`。

下面是一个示例，其中未引用的 shell 元字符导致错误：

```sh
$ echo apple;cherry
apple
cherry: command not found

# '\;' escapes the ';' character, thus losing the metacharacter meaning
$ echo apple\;cherry
apple;cherry 
```

下面是一个示例，其中微妙的问题可能一开始并不明显：

```sh
# this will create two files named 'new' and 'file.txt'
# aim was to create a single file named 'new file.txt'
$ touch new file.txt
$ ls new*txt
ls: cannot access 'new*txt': No such file or directory
$ rm file.txt new

# escaping the space will create a single file named 'new file.txt'
$ touch new\ file.txt
$ ls new*txt
'new file.txt'
$ rm new\ file.txt 
```

*2)* **单引号**

> 将字符放在单引号 (`'`) 内会保留引号内每个字符的原始值。单引号之间不能出现单引号，即使前面有反斜杠也是如此。

单引号字符串内没有特殊字符。下面是一个示例：

```sh
$ echo 'apple;cherry'
apple;cherry 
```

你可以将由不同引用机制表示的字符串放在一起，以将它们连接起来。下面是一个示例：

```sh
# concatenation of four strings
# 1: '@fruits = '
# 2: \'
# 3: 'apple and banana'
# 4: \'
$ echo '@fruits = '\''apple and banana'\'
@fruits = 'apple and banana' 
```

*3)* **双引号**

> 将字符放在双引号 (`"`) 内会保留引号内所有字符的原始值，除了 `$`、`` ` ``、`\` 以及当启用历史扩展时 `!`。

下面是一个示例，展示了双引号内的变量插值：

```sh
$ qty='5'

# as seen earlier, no character is special within single quotes
$ echo 'I bought $qty apples'
I bought $qty apples

# a typical use of double quotes is to enable variable interpolation
$ echo "I bought $qty apples"
I bought 5 apples 
```

除非你明确希望 shell 解释变量的内容，否则你应该始终引用变量，以避免由于 shell 元字符的存在而产生的问题。

```sh
$ f='new file.txt'

# same as: echo 'apple banana' > new file.txt
$ echo 'apple banana' > $f
bash: $f: ambiguous redirect

# same as: echo 'apple banana' > 'new file.txt'
$ echo 'apple banana' > "$f"
$ cat "$f"
apple banana
$ rm "$f" 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 参见 [unix.stackexchange: Why does my shell script choke on whitespace or other special characters?](https://unix.stackexchange.com/q/131766/109046)。

*4)* **ANSI-C 引用**

> 形式为 `$'string'` 的单词被特别处理。该单词扩展为字符串，其中反斜杠转义字符按照 ANSI C 标准进行替换。

这种引号形式可以帮助你使用转义序列，如 `\t` 用于制表符，`\n` 用于换行符等。你也可以使用八进制和十六进制格式表示字符的代码点值。

```sh
# can also use echo -e 'fig:\t42' or printf 'fig:\t42\n'
$ echo $'fig:\t42'
fig:    42

# \x27 represents the single quote character in hexadecimal format
$ echo $'@fruits = \x27apple and banana\x27'
@fruits = 'apple and banana'

# 'grep' helps you to filter lines based on the given pattern
# but it doesn't recognize escapes like '\t' for tab characters
$ printf 'fig\t42\napple 100\nball\t20\n' | grep '\t'
# in such cases, one workaround is use to ANSI-C quoting
$ printf 'fig\t42\napple 100\nball\t20\n' | grep $'\t'
fig     42
ball    20 
```

`printf` 是一个 shell 内置命令，你可以用它来格式化参数（类似于 `C` 编程语言中的 `printf()` 函数）。这个命令将在接下来的更多示例中使用。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 查看 [bash 手册：ANSI-C 引用](https://www.gnu.org/software/bash/manual/bash.html#ANSI_002dC-Quoting) 以获取支持的转义序列的完整列表。查看 `man ascii` 以获取 ASCII 字符及其数值表示的表格。

## 通配符

当文件数量较少时，指定完整的文件名作为命令参数相对容易。你可以使用像制表符补全和中鼠标按钮点击（粘贴最后高亮文本）这样的功能来辅助这种情况。

但如果你必须处理成百上千个文件（甚至更多）怎么办？如果适用，一种方法是根据它们文件名中的公共模式匹配所有文件，例如像 `.py`、`.txt` 等扩展名。在这种情况下，通配符（glob）会有所帮助。这个功能由 shell 提供，因此单个命令无需担心实现它们。通配符支持的匹配模式与正则表达式有些相似，但它们之间存在根本性和语法上的差异。

下面列出了一些常用的通配符：

+   `*` 匹配任何字符，零次或多次

    +   作为特殊情况，`*` 不会匹配隐藏文件开头的 `.`，除非设置了 `dotglob` shell 选项

+   `?` 匹配任何字符正好一次

+   `[set149]` 匹配这些字符中的任意一个

+   `[^set149]` 匹配除了给定字符集之外的任何字符

    +   你也可以使用 `[!set149]` 来否定字符集

+   `[a-z]` 匹配从 `a` 到 `z` 的字符范围

+   `[0-9a-fA-F]` 匹配任何十六进制字符

下面是一些示例：

```sh
# change to the 'scripts' directory and source the 'globs.sh' script
$ source globs.sh
$ ls
100.sh   f1.txt      f4.txt    hi.sh   math.h         report-02.log
42.txt   f2_old.txt  f7.txt    ip.txt  notes.txt      report-04.log
calc.py  f2.txt      hello.py  main.c  report-00.log  report-98.log

# beginning with 'c' or 'h' or 't'
$ ls [cht]*
calc.py  hello.py  hi.sh

# only hidden files and directories
$ ls -d .*
.  ..  .hidden  .somerc

# ending with '.c' or '.py'
$ ls *.c *.py
calc.py  hello.py  main.c

# containing 'o' as well as 'x' or 'y' or 'z' afterwards
$ ls *o*[xyz]*
f2_old.txt  hello.py  notes.txt

# ending with '.' and two more characters
$ ls *.??
100.sh  calc.py  hello.py  hi.sh

# shouldn't start with 'f' and ends with '.txt'
$ ls [^f]*.txt
42.txt  ip.txt  notes.txt

# containing digits '1' to '5' and ending with 'log'
$ ls *[1-5]*log
report-02.log  report-04.log 
```

由于字符类内部的一些字符是特殊的，你需要特殊的位置来将它们视为普通字符：

+   `-` 应该是集合中的第一个或最后一个字符

+   `^` 应该不是第一个字符

+   `]` 应该是第一个字符

```sh
$ ls *[ns-]*
100.sh  main.c     report-00.log  report-04.log
hi.sh   notes.txt  report-02.log  report-98.log

$ touch 'a^b' 'mars[planet].txt'
$ rm -i *[]^]*
rm: remove regular empty file 'a^b'? y
rm: remove regular empty file 'mars[planet].txt'? y 
```

**命名字符集** 由 `[:` 和 `:]` 之间的名称定义，并且必须在字符类 `[]` 中使用，以及任何其他所需的字符。

| 命名集 | 描述 |
| --- | --- |
| `[:digit:]` | `[0-9]` |
| `[:lower:]` | `[a-z]` |
| `[:upper:]` | `[A-Z]` |
| `[:alpha:]` | `[a-zA-Z]` |
| `[:alnum:]` | `[0-9a-zA-Z]` |
| `[:word:]` | `[0-9a-zA-Z_]` |
| `[:xdigit:]` | `[0-9a-fA-F]` |
| `[:cntrl:]` | 控制字符 — 前面的 32 个 ASCII 字符和第 127 个（DEL） |
| `[:punct:]` | 所有标点符号 |
| `[:graph:]` | `[:alnum:]` 和 `[:punct:]` |
| `[:print:]` | `[:alnum:]`、`[:punct:]` 和空格 |
| `[:ascii:]` | 所有 ASCII 字符 |
| `[:blank:]` | 空格和制表符字符 |
| `[:space:]` | 空白字符 |

```sh
# starting with a digit character, same as: [0-9]*
$ ls [[:digit:]]*
100.sh  42.txt

# starting with a digit character or 'c'
# same as: [0-9c]*
$ ls [[:digit:]c]*
100.sh  42.txt  calc.py

# starting with a non-alphabet character
$ ls [^[:alpha:]]*
100.sh  42.txt 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如前所述，您可以使用 `echo` 来测试在使用命令对匹配的文件进行操作之前，通配符将如何展开。例如，在使用 `rm *.txt` 这样的命令之前，先执行 `echo *.txt`。与 `ls` 相比的一个不同之处在于，如果没有任何匹配项，`echo` 将显示通配符本身而不是显示错误。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 更多详细信息，包括本地化信息等，请参阅 [bash 手册：模式匹配](https://www.gnu.org/software/bash/manual/bash.html#Pattern-Matching)。

## 花括号展开

这不是通配符功能，您只是得到展开的字符串。花括号展开有两个减少输入的机制：

+   从多个字符串中提取共同部分

+   生成字符范围

假设您想创建两个名为 `test_x.txt` 和 `test_y.txt` 的文件。这两个字符串在开头和结尾处有共同点。您可以将独特的部分指定为逗号分隔的字符串，放在一对大括号内，并将共同的部分放在大括号周围。根据需要可以使用多个大括号。使用 `echo` 进行测试。

```sh
$ mkdir practice_brace
$ cd practice_brace

# same as: touch ip1.txt ip3.txt ip7.txt
$ touch ip{1,3,7}.txt
$ ls ip*txt
ip1.txt  ip3.txt  ip7.txt

# same as: mv ip1.txt ip_a.txt
$ mv ip{1,_a}.txt
$ ls ip*txt
ip3.txt  ip7.txt  ip_a.txt

$ echo adders/{half,full}_adder.v
adders/half_adder.v adders/full_adder.v

$ echo file{0,1}.{txt,log}
file0.txt file0.log file1.txt file1.log

# empty alternate is allowed too
$ echo file{,1}.txt
file.txt file1.txt

# example with nested braces
$ echo file.{txt,log{,.bkp}}
file.txt file.log file.log.bkp 
```

要生成一个范围，指定由 `..` 分隔的数字或单个字符，以及可选的第三个参数作为步长值。以下是一些示例：

```sh
$ echo {1..4}
1 2 3 4
$ echo {4..1}
4 3 2 1

$ echo {1..2}{a..b}
1a 1b 2a 2b

$ echo file{1..4}.txt
file1.txt file2.txt file3.txt file4.txt

$ echo file{1..10..2}.txt
file1.txt file3.txt file5.txt file7.txt file9.txt

$ echo file_{x..z}.txt
file_x.txt file_y.txt file_z.txt

$ echo {z..j..-3}
z w t q n k

# '0' prefix
$ echo {008..10}
008 009 010 
```

如果大括号的使用不符合展开语法，它将保持原样：

```sh
$ echo file{1}.txt
file{1}.txt

$ echo file{1-4}.txt
file{1-4}.txt 
```

## 扩展和递归通配符

从 `man bash`：

| 扩展通配符 | 描述 |
| --- | --- |
| `?(pattern-list)` | 匹配给定模式零次或一次出现 |
| `*(pattern-list)` | 匹配给定模式零次或多次出现 |
| `+(pattern-list)` | 匹配给定模式一次或多次出现 |
| `@(pattern-list)` | 匹配给定模式之一 |
| `!(pattern-list)` | 匹配除了给定模式之一的所有内容 |

扩展通配符默认是禁用的。您可以使用 `shopt` 内置命令来设置/取消设置 **sh**ell **opt**ions，如 `extglob`、`globstar` 等。您还可以检查这些选项的当前状态。

```sh
$ shopt extglob
extglob         off

# set extglob
$ shopt -s extglob
$ shopt extglob
extglob         on

# unset extglob
$ shopt -u extglob
$ shopt extglob
extglob         off 
```

以下是一些示例，假设已经设置了 `extglob` 选项：

```sh
# change to the 'scripts' directory and source the 'globs.sh' script
$ source globs.sh
$ ls
100.sh   f1.txt      f4.txt    hi.sh   math.h         report-02.log
42.txt   f2_old.txt  f7.txt    ip.txt  notes.txt      report-04.log
calc.py  f2.txt      hello.py  main.c  report-00.log  report-98.log

# one or more digits followed by '.' and then zero or more characters
$ ls +([0-9]).*
100.sh  42.txt

# same as: ls *.c *.sh
$ ls *.@(c|sh)
100.sh  hi.sh  main.c

# not ending with '.txt'
$ ls !(*.txt)
100.sh   hello.py  main.c  report-00.log  report-04.log
calc.py  hi.sh     math.h  report-02.log  report-98.log

# not ending with '.txt' or '.log'
$ ls *.!(txt|log)
100.sh  calc.py  hello.py  hi.sh  main.c  math.h 
```

如果您启用 `globstar` 选项，您可以在指定路径内递归地匹配文件名。

```sh
# change to the 'scripts' directory and source the 'ls.sh' script
$ source ls.sh

# with 'find' command (this will be explained in a later chapter)
$ find -name '*.txt'
./todos/books.txt
./todos/outing.txt
./ip.txt

# with 'globstar' enabled
$ shopt -s globstar
$ ls **/*.txt
ip.txt  todos/books.txt  todos/outing.txt

# another example
$ ls -1 **/*.@(py|html)
backups/bookmarks.html
hello_world.py
projects/tictactoe/game.py 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如果您想在终端启动时应用这些设置，请将 `shopt` 调用添加到 `~/.bashrc` 中。这将在 Shell 自定义 章节中讨论。

## set

`set` 内置命令可以帮助您设置或取消设置 shell 选项和位置参数的值。以下是一些 shell 选项的示例：

```sh
# disables logging command history from this point onwards
$ set +o history
# enable history logging
$ set -o history

# use vi-style CLI editing interface
$ set -o vi
# use emacs-style interface, this is usually the default
$ set -o emacs 
```

您将在后面的章节中看到更多示例（例如，`set -x`）。有关文档，请参阅 [bash 手册：Set 内置](https://www.gnu.org/software/bash/manual/bash.html#The-Set-Builtin)。

## 管道

管道控制操作符`|`帮助你将一个命令的输出作为另一个命令的输入。这个操作符大大减少了临时中间文件的需求。如前所述，在 Unix 哲学部分，命令行工具通常专注于单一任务。如果你可以将问题分解成更小的任务，管道操作符将经常派上用场。以下是一些示例：

```sh
# change to the 'scripts' directory and source the 'du.sh' script
$ source du.sh

# list of files
$ ls
projects  report.log  todos
# count the number of files
# you can also use: printf '%q\n' * | wc -l
$ ls -q | wc -l
3

# report the size of files/folders in human readable format
# and then sort them based on human readable sizes in ascending order
$ du -sh * | sort -h
8.0K    todos
48K     projects
7.4M    report.log 
```

在上述示例中，`ls`和`du`分别执行显示文件列表和显示文件大小的任务。之后，`wc`和`sort`命令分别负责计数和排序行。在这种情况下，管道操作符可以节省你处理临时数据的问题。

注意，`printf`中的`%q`格式说明符可以帮助你以 shell 可识别的方式引用参数。`ls`的`-q`选项将文件名中的非图形字符替换为`?`字符。这两个都是防止由于文件名中的换行符等字符而导致计数过程偏离的解决方案。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 管道控制操作符`|&`将在本章后面讨论。

### tee

有时，你可能希望同时在终端上显示命令输出并需要结果以供以后使用。在这种情况下，你可以使用`tee`命令：

```sh
$ du -sh * | tee sizes.log
48K     projects
7.4M    report.log
8.0K    todos

$ cat sizes.log
48K     projects
7.4M    report.log
8.0K    todos

$ rm sizes.log 
```

## 重定向

来自[bash 手册：重定向](https://www.gnu.org/software/bash/manual/bash.html#Redirections)：

> 在执行命令之前，可以使用特殊符号通过 shell 进行输入和输出的重定向。重定向允许命令的文件句柄被复制、打开、关闭、指向不同的文件，并且可以改变命令读取和写入的文件。重定向还可以用于修改当前 shell 执行环境中的文件句柄。

有三个标准数据流：

+   **标准输入** (`stdin` — 文件描述符 0)

+   **标准输出** (`stdout` — 文件描述符 1)

+   **标准错误** (`stderr` — 文件描述符 2)

默认情况下，标准输出和错误流都会在终端上显示。当命令使用出现问题时，会使用`stderr`流。上述三个流都如前所述有预定义的[文件描述符](https://en.wikipedia.org/wiki/File_descriptor)。在本节中，你将了解如何重定向这三个流。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 重定向可以放置在任何位置，但通常用于命令的开始或结束部分。例如，以下两个命令是等效的：
> 
> ```sh
> >op.txt grep 'error' report.log
> 
> grep 'error' report.log >op.txt 
> ```
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 在重定向操作符和文件名之间添加空格字符是可选的。

### 重定向输出

你可以使用 `>` 操作符将命令的标准输出重定向到文件。可以在 `>` 操作符前添加一个数字前缀来与特定的文件描述符一起使用。默认是 `1`（回想一下，`stdout` 的文件描述符是 `1`），所以 `1>` 和 `>` 执行相同的操作。使用 `>>` 将输出追加到文件。

提供给 `>` 和 `>>` 操作符的文件名将在不存在时创建。如果文件已存在，`>` 将覆盖该文件，而 `>>` 将追加内容。

```sh
# change to the 'example_files/text_files' directory for this section

# save first three lines of 'sample.txt' to 'op.txt'
$ head -n3 sample.txt > op.txt
$ cat op.txt
 1) Hello World
 2) 
 3) Hi there

# append last two lines of 'sample.txt' to 'op.txt'
$ tail -n2 sample.txt >> op.txt
$ cat op.txt
 1) Hello World
 2) 
 3) Hi there
14) He he he
15) Adios amigo

$ rm op.txt 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你可以使用 `/dev/null` 作为文件名来丢弃输出，为命令提供空文件作为输入等。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你可以使用 `set noclobber` 来防止覆盖已存在的文件。当设置了 `noclobber` 选项时，你仍然可以通过使用 `>|` 而不是 `>` 操作符来覆盖文件。

### 重定向输入

一些命令如 `tr` 和 `datamash` 只能处理来自标准输入的数据。当你从另一个命令中管道数据时，这不是一个问题，例如：

```sh
# filter lines containing 'the' from the input file 'greeting.txt'
# and then display the results in uppercase using the 'tr' command
$ grep 'the' greeting.txt | tr 'a-z' 'A-Z'
HI THERE 
```

如果你想要将数据从文件传递给这样的命令，可以使用 `<` 重定向操作符。这里的默认前缀是 `0`，它是 `stdin` 数据的文件描述符。以下是一个示例：

```sh
$ tr 'a-z' 'A-Z' &LTgreeting.txt
HI THERE
HAVE A NICE DAY 
```

在某些情况下，一个工具在处理 `stdin` 数据时与文件输入的行为不同。以下是一个使用 `wc -l` 报告输入中总行数的示例：

```sh
# line count, filename is part of the output as well
$ wc -l purchases.txt
8 purchases.txt

# filename won't be part of the output for stdin data
# helpful for assigning the number to a variable for scripting purposes
$ wc -l &LTpurchases.txt
8 
```

有时，你需要将 `stdin` 数据以及其他文件输入传递给一个命令。在这种情况下，你可以使用 `-` 来表示来自标准输入的数据。以下是一个示例：

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

# insert a column at the start
$ printf 'ID\n1\n2\n3' | paste -d, - scores.csv
ID,Name,Maths,Physics,Chemistry
1,Ith,100,100,100
2,Cy,97,98,95
3,Lin,78,83,80 
```

即使命令可以直接接受文件输入作为参数，重定向也可以帮助进行交互式使用。以下是一个示例：

```sh
# display only the third field
$ &LTscores.csv cut -d, -f3
Physics
100
98
83

# later, you realize that you need the first field too
# use 'up' arrow key to bring the previous command
# and modify the argument easily at the end
# if you had used cut -d, -f3 scores.csv instead,
# you'd have to navigate past the filename to modify the argument
$ &LTscores.csv cut -d, -f1,3
Name,Physics
Ith,100
Cy,98
Lin,83 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) ![warning](img/b5314b4a0acf0f436c4bf59486793342.png) 不要使用 `cat filename | cmd` 来将文件内容作为 `stdin` 数据传递，除非你需要连接多个输入文件中的数据。有关详细信息，请参阅 [wikipedia: UUOC](https://en.wikipedia.org/wiki/Cat_(Unix)#Useless_use_of_cat) 和 [Useless Use of Cat Award](https://porkmail.org/era/unix/award.html)。

### 重定向错误

回想一下，`stderr` 的文件描述符是 `2`。因此，你可以使用 `2>` 来将标准错误重定向到文件。如果需要追加内容，请使用 `2>>`。以下是一个示例：

```sh
# assume 'abcdxyz' doesn't exist as a shell command
$ abcdxyz
abcdxyz: command not found

# the error in such cases will be part of the stderr stream, not stdout
# so, you'll need to use 2> here
$ abcdxyz 2> cmderror.log
$ cat cmderror.log
abcdxyz: command not found

$ rm cmderror.log 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如果你需要丢弃结果，请使用 `/dev/null` 作为文件名。

### 合并 stdout 和 stderr

新版本的 Bash 提供了这些方便的快捷方式：

+   `&>` 同时重定向 `stdout` 和 `stderr`（如果文件已存在则覆盖）

+   `&>>` 同时重定向 `stdout` 和 `stderr`（如果文件已存在则追加）

+   `|&` 将 `stdout` 和 `stderr` 作为输入传递给另一个命令

下面是一个假设 `xyz.txt` 不存在的示例，这会导致错误：

```sh
# using '>' will redirect only the stdout stream
# stderr will be displayed on the terminal
$ grep 'log' file_size.txt xyz.txt > op.txt
grep: xyz.txt: No such file or directory

# using '&>' will redirect both the stdout and stderr streams
$ grep 'log' file_size.txt xyz.txt &> op.txt
$ cat op.txt
file_size.txt:104K    power.log
file_size.txt:746K    report.log
grep: xyz.txt: No such file or directory

$ rm op.txt 
```

下面是一个使用 `|&` 运算符的示例：

```sh
# filter lines containing 'log' from the given file arguments
# and then filter lines containing 'or' from the combined stdout and stderr
$ grep 'log' file_size.txt xyz.txt |& grep 'or'
file_size.txt:746K    report.log
grep: xyz.txt: No such file or directory 
```

对于较早的 Bash 版本，您必须手动重定向流：

+   `1>&2` 将文件描述符 `1` (`stdout`) 重定向到文件描述符 `2` (`stderr`)。

+   `2>&1` 将文件描述符 `2` (`stderr`) 重定向到文件描述符 `1` (`stdout`)。

下面是一些示例：

```sh
# note that the order of redirections is important here
# you can also use: 2> op.txt 1>&2
$ grep 'log' file_size.txt xyz.txt > op.txt 2>&1
$ cat op.txt
file_size.txt:104K    power.log
file_size.txt:746K    report.log
grep: xyz.txt: No such file or directory
$ rm op.txt

$ grep 'log' file_size.txt xyz.txt 2>&1 | grep 'or'
file_size.txt:746K    report.log
grep: xyz.txt: No such file or directory 
```

### 等待 stdin

有时，您可能会不小心输入一个命令而没有提供输入。而不是得到一个错误，您会看到光标耐心地等待。这并不是 shell 在挂起您。命令正在等待您输入数据，以便它可以执行其任务。

假设您输入了 `cat` 并按下了 Enter 键。看到闪烁的光标，您输入一些文本并再次按下 Enter 键。您将看到您刚刚输入的文本被作为 `stdout` 反射回来（这是 `cat` 命令的功能）。这会一直重复，直到您告诉 shell 您已完成。如何做到这一点？在空白行上按 `Ctrl+d` 或者在行尾按两次 `Ctrl+d`。在后一种情况下，您不会在数据末尾得到换行符。

```sh
# press Enter and Ctrl+d after typing all the required characters
$ cat
knock knock
knock knock
anybody here?
anybody here?

# 'tr' command here translates lowercase to uppercase
$ tr 'a-z' 'A-Z'
knock knock
KNOCK KNOCK
anybody here?
ANYBODY HERE? 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 在每条输入行后立即获取输出取决于命令的功能。像 `sort` 和 `shuf` 这样的命令会在产生输出之前等待整个输入数据。
> 
> ```sh
> # press Ctrl+d after the third input line
> $ sort
> lion
> zebra
> bee
> bee
> lion
> zebra 
> ```

下面是一个包含输出重定向的示例：

```sh
# press Ctrl+d after the line containing 'histogram'
# filter lines containing 'is'
$ grep 'is' > op.txt
hi there
this is a sample line
have a nice day
histogram

$ cat op.txt
this is a sample line
histogram

$ rm op.txt 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 参见 [unix.stackexchange: Ctrl+c 和 Ctrl+d 的区别](https://unix.stackexchange.com/q/16333/109046)。

### Here Documents

Here Documents 是提供 `stdin` 数据的另一种方式。在这种情况下，终止条件是匹配 `<<` 重定向运算符后指定的预定义字符串的行。这对于自动化特别有帮助，因为交互式地按 `Ctrl+d` 并不理想。下面是一个示例：

```sh
# EOF is typically used as the special string
$ cat << 'EOF' > fruits.txt
> banana 2
> papaya 3
> mango  10
> EOF

$ cat fruits.txt
banana 2
papaya 3
mango  10
$ rm fruits.txt 
```

在上面的示例中，终止字符串被单引号包围，这是一种良好的实践。这样做可以防止参数扩展、命令替换等。您也可以使用 `\string` 来实现这个目的。如果您使用 `<&LT-` 而不是 `<<`，可以在输入行的开头添加前导制表符，而不会成为实际数据的一部分。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 就像 `$` 和一个空格代表主要提示符（`PS1` shell 变量）一样，行首的 `>` 和一个空格代表次要提示符 `PS2`（适用于多行命令）。在 shell 脚本中使用 Here Documents 时，不要输入这些字符。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 参见 [bash 手册：Here Documents](https://www.gnu.org/software/bash/manual/bash.html#Here-Documents) 和 [stackoverflow：here documents](https://stackoverflow.com/q/2953081/4082052) 以获取更多示例和详细信息。

### Here Strings

这与 Here Documents 类似，但字符串是在 `<<<` 重定向运算符之后作为参数传递的。以下是一些示例：

```sh
$ tr 'a-z' 'A-Z' <<< hello
HELLO
$ tr 'a-z' 'A-Z' <<< 'hello world'
HELLO WORLD

$ greeting='hello world'
$ tr 'a-z' 'A-Z' > op.txt <<< "$greeting"
$ cat op.txt
HELLO WORLD
$ rm op.txt 
```

### 进一步阅读

+   [关于 shell 重定向的简短介绍](https://mywiki.wooledge.org/BashGuide/InputAndOutput#Redirection)

+   [图解重定向教程](https://web.archive.org/web/20221231120128/https://wiki.bash-hackers.org/howto/redirection_tutorial)

+   [stackoverflow：使用 >& 将流重定向到另一个文件描述符](https://stackoverflow.com/q/818255/4082052)

+   [2>&1 >foo 和 >foo 2>&1 之间的区别](https://mywiki.wooledge.org/BashFAQ/055)

+   [stackoverflow：将标准输出和标准错误重定向到文件](https://stackoverflow.com/q/876239/4082052)

+   [unix.stackexchange：<> 重定向示例](https://unix.stackexchange.com/q/164391/109046)

## 命令分组

您可以使用 `(list)` 和 `{ list; }` 复合命令来重定向多个命令的内容。前者在子 shell 中执行，而后者在当前 shell 上下文中执行。`()` 周围的空格对于 `{}` 版本是可选的，但对于 `{}` 版本则是必要的。更多信息请参考 [bash 手册：命令列表](https://www.gnu.org/software/bash/manual/bash.html#Lists)。

> `list` 是由一个或多个管道序列组成，这些序列由运算符 `;`、`&`、`&&` 或 `||` 分隔，并且可以由 `;`、`&` 或换行符终止。

下面是一些命令分组的示例：

```sh
# change to the 'example_files/text_files' directory for this section

# the 'sed' command here gives the first line of the input
# rest of the lines are then processed by the 'sort' command
# thus, the header will always be the first line in the output
$ (sed -u '1q' ; sort) < scores.csv
Name,Maths,Physics,Chemistry
Cy,97,98,95
Ith,100,100,100
Lin,78,83,80

# save first three and last two lines from 'sample.txt' to 'op.txt'
$ { head -n3 sample.txt; tail -n2 sample.txt; } > op.txt
$ cat op.txt
 1) Hello World
 2) 
 3) Hi there
14) He he he
15) Adios amigo
$ rm op.txt 
```

您可能会想知道为什么第二个命令没有使用 `< sample.txt` 而是重复了文件名两次。原因是某些命令可能会读取比所需更多的内容（出于缓冲目的），从而给后续命令造成问题。在 `sed+sort` 示例中，`-u` 选项确保 `sed` 不会读取超过所需的数据。更多示例和详细信息请参阅 [unix.stackexchange：sort but keep header line at the top](https://unix.stackexchange.com/q/11856/109046)。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 您不需要 `()` 或 `{}` 分组来在终端上查看多个命令的结果。只需在命令之间使用 `;` 分隔符即可。有关更多信息，请参阅 [bash 手册：命令执行环境](https://www.gnu.org/software/bash/manual/bash.html#Command-Execution-Environment)。
> 
> ```sh
> $ head -n1 sample.txt ; echo 'have a nice day'
>  1) Hello World
> have a nice day 
> ```

## 列表控制运算符

您可以使用这些运算符根据第一个命令的退出状态来控制后续命令的执行。更多信息请参考 [bash 手册：命令列表](https://www.gnu.org/software/bash/manual/bash.html#Lists)。

> AND 和 OR 列表是由控制运算符 `&&` 和 `||` 分隔的一个或多个管道序列，分别执行时具有左结合性。

对于 AND 列表，只有当第一个命令以 `0` 状态退出时，第二个命令才会被执行。

```sh
# first command succeeds here, so the second command is also executed
$ echo 'hello' && echo 'have a nice day'
hello
have a nice day

# assume 'abcdxyz' doesn't exist as a shell command
# the second command will not be executed
$ abcdxyz && echo 'have a nice day'
abcdxyz: command not found

# if you use ';' instead, the second command will still be executed
$ abcdxyz ; echo 'have a nice day'
abcdxyz: command not found
have a nice day 
```

对于 OR 列表，只有当第一个命令没有以 `0` 状态退出时，第二个命令才会被执行。

```sh
# since the first command succeeds, the second one won't run
$ echo 'hello' || echo 'have a nice day'
hello

# assume 'abcdxyz' doesn't exist as a shell command
# since the first command fails, the second one will run
$ abcdxyz || echo 'have a nice day'
abcdxyz: command not found
have a nice day 
```

## 命令替换

命令替换允许你将一个命令的标准输出作为另一个命令的一部分使用。如果有的话，尾随的新行将被删除。你可以使用较新且更受欢迎的语法`$(command)`或较旧的语法`` `command` ``。以下是一些示例：

```sh
# sample input
$ printf 'hello\ntoday is: \n'
hello
today is:
# append output from the 'date' command to the line containing 'today'
$ printf 'hello\ntoday is: \n' | sed '/today/ s/$/'"$(date +%A)"'/'
hello
today is: Monday

# save the output of 'wc' command to a variable
# same as: line_count=`wc -l &LTsample.txt`
$ line_count=$(wc -l &LTsample.txt)
$ echo "$line_count"
15 
```

这里有一个嵌套替换的示例：

```sh
# dirname removes the trailing path component
$ dirname projects/tictactoe/game.py
projects/tictactoe
# basename removes the leading directory component
$ basename projects/tictactoe
tictactoe

$ proj=$(basename $(dirname projects/tictactoe/game.py))
$ echo "$proj"
tictactoe 
```

以下是从[bash 手册：命令替换](https://www.gnu.org/software/bash/manual/bash.html#Command-Substitution)中引用的两种语法类型的区别：

> 当使用旧式的反引号替换形式时，反斜杠保留其字面意义，除非其后跟有`$`、`` ` ``或`\`。第一个没有反斜杠的前面的反引号终止命令替换。当使用$(command)形式时，括号之间的所有字符组成命令；没有字符被特殊处理。
> 
> 命令替换可以是嵌套的。当使用反引号形式时，使用反斜杠转义内部的反引号。

## 进程替换

除了文件参数外，你可以使用进程替换来使用命令输出。语法是`<(list)`。shell 将负责传递一个包含那些命令标准输出的文件名。以下是一个示例：

```sh
# change to the 'example_files/text_files' directory for this section

$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

# can also use: paste -d, <(echo 'ID'; seq 3) scores.csv
$ paste -d, <(printf 'ID\n1\n2\n3') scores.csv
ID,Name,Maths,Physics,Chemistry
1,Ith,100,100,100
2,Cy,97,98,95
3,Lin,78,83,80 
```

对于上述示例，你也可以使用`-`来表示`stdin`管道数据，如前述章节所示。这里有一个使用两个替换的示例。这实际上帮助你避免管理多个临时文件，类似于`|`管道操作符对单个临时文件的帮助。

```sh
# side-by-side view of sample input files
$ paste f1.txt f2.txt
1       1
2       hello
3       3
world   4

# this command gives the common lines between two files
# the files have to be sorted for the command to work properly
$ comm -12 <(sort f1.txt) <(sort f2.txt)
1
3 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 请参阅[这个 unix.stackexchange 线程](https://unix.stackexchange.com/q/609375/109046)以获取使用`>(list)`形式的示例。

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用`globs.sh`脚本来处理与通配符相关的练习，除非另有说明。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 为可能需要创建一些文件的练习创建一个临时目录。之后你可以删除这样的练习目录。

**1)** 使用`echo`命令以如下所示的方式显示文本。根据需要使用适当的引号。

```sh
# ???
that's    great! $x = $y + $z 
```

**2)** 使用`echo`命令以下面的格式显示三个变量的值。

```sh
$ n1=10
$ n2=90
$ op=100

# ???
10 + 90 = 100 
```

**3)** 以下命令的输出将是什么？

```sh
$ echo $'\x22apple\x22: \x2710\x27' 
```

**4)** 列出以数字字符开头的文件名。

```sh
# change to the 'scripts' directory and source the 'globs.sh' script
$ source globs.sh

# ???
100.sh  42.txt 
```

**5)** 列出扩展名不以`t`或`l`开头的文件名。假设扩展名至少有一个字符。

```sh
# ???
100.sh  calc.py  hello.py  hi.sh  main.c  math.h 
```

**6)** 列出扩展名只有一个字符的文件名。

```sh
# ???
main.c  math.h 
```

**7)** 列出扩展名不是`txt`的文件名。

```sh
# ???
100.sh   hello.py  main.c  report-00.log  report-04.log
calc.py  hi.sh     math.h  report-02.log  report-98.log 
```

**8)** 描述以下命令中使用的通配符模式。

```sh
$ ls *[^[:word:]]*.*
report-00.log  report-02.log  report-04.log  report-98.log 
```

**9)** 列出扩展名前只有小写字母的文件名。

```sh
# ???
calc.py  hello.py  hi.sh  ip.txt  main.c  math.h  notes.txt 
```

**10)** 列出以`ma`、`he`或`hi`开头的文件名。

```sh
# ???
hello.py  hi.sh  main.c  math.h 
```

**11)** 你会使用哪些命令来获取以下显示的输出？假设你不知道子目录的深度。

```sh
# change to the 'scripts' directory and source the 'ls.sh' script
$ source ls.sh

# filenames ending with '.txt'
# ???
ip.txt  todos/books.txt  todos/outing.txt

# directories starting with 'c' or 'd' or 'g' or 'r' or 't'
# ???
backups/dot_files/
projects/calculator/
projects/tictactoe/
todos/ 
```

**12)** 创建并切换到一个空目录。然后，使用花括号展开和相关命令来获取以下显示的结果。

```sh
# ???
$ ls report*
report_2020.txt  report_2021.txt  report_2022.txt

# use the 'cp' command here
# ???
$ ls report*
report_2020.txt  report_2021.txt  report_2021.txt.bkp  report_2022.txt 
```

**13)** `set` 内置命令的作用是什么？

**14)** `|` 管道操作符的作用是什么？你会在什么情况下添加 `tee` 命令？

**15)** 你能推断出以下命令的作用吗？*提示*：查看 `help printf`。

```sh
$ printf '%s\n' apple car dragon
apple
car
dragon 
```

**16)** 使用花括号展开和相关命令以及 shell 功能来获取以下显示的结果。*提示*：查看前一个问题。

```sh
$ ls ip.txt
ls: cannot access 'ip.txt': No such file or directory

# ???
$ cat ip.txt
item_10
item_12
item_14
item_16
item_18
item_20 
```

**17)** 在 `ip.txt` 包含之前问题中显示的文本的情况下，使用花括号展开和相关命令来获取以下显示的结果。

```sh
# ???
$ cat ip.txt
item_10
item_12
item_14
item_16
item_18
item_20
apple_1_banana_6
apple_1_banana_7
apple_1_banana_8
apple_2_banana_6
apple_2_banana_7
apple_2_banana_8
apple_3_banana_6
apple_3_banana_7
apple_3_banana_8 
```

**18)** 如果有的话，`<` 和 `|` shell 操作符之间有什么区别？

**19)** 通常用哪个字符来表示将 `stdin` 数据作为文件参数？

**20)** 以下操作符的作用是什么？

*a)* `1>`

*b)* `2>`

*c)* `&>`

*d)* `&>>`

*e)* `|&`

**21)** 如果你使用以下 `grep` 命令，`op.txt` 的内容将会是什么？

```sh
# press Ctrl+d after the line containing 'histogram'
$ grep 'hi' > op.txt
hi there
this is a sample line
have a nice day
histogram

$ cat op.txt 
```

**22)** 如果你使用以下命令，`op.txt` 的内容将会是什么？

```sh
$ qty=42
$ cat << end > op.txt
> dragon
> unicorn
> apple $qty
> ice cream
> end

$ cat op.txt 
```

**23)** 修正以下命令以获取以下显示的预期输出。

```sh
$ books='cradle piranesi soulhome bastion'

# something is wrong with this command
$ sed 's/\b\w/\u&/g' <<< '$books'
$Books

# ???
Cradle Piranesi Soulhome Bastion 
```

**24)** 修正以下命令以获取以下显示的预期输出。

```sh
# something is wrong with this command
$ echo 'hello' ; seq 3 > op.txt
hello
$ cat op.txt
1
2
3

# ???
$ cat op.txt
hello
1
2
3 
```

**25)** 以下命令的输出将会是什么？

```sh
$ printf 'hello' | tr 'a-z' 'A-Z' && echo ' there'

$ printf 'hello' | tr 'a-z' 'A-Z' || echo ' there' 
```

**26)** 修正以下命令以获取以下显示的预期输出。

```sh
# something is wrong with these commands
$ nums=$(seq 3)
$ echo $nums
1 2 3

# ???
1
2
3 
```

**27)** 以下两个命令会产生等效的输出吗？如果不，为什么？

```sh
$ paste -d, <(seq 3) <(printf '%s\n' item_{1..3})

$ printf '%s\n' {1..3},item_{1..3} 
```
