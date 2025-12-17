# 多用途文本处理工具

> 原文：[`learnbyexample.github.io/cli-computing/multipurpose-text-processing-tools.html`](https://learnbyexample.github.io/cli-computing/multipurpose-text-processing-tools.html)

许多命令行界面（CLI）文本处理工具已经存在了大约半个世纪。而且，为了解决不断扩大的文本处理问题，新的工具正在被编写。仅仅知道某个工具的存在，或者在尝试编写自己的解决方案之前搜索工具，就可以节省时间。此外，流行的工具可能已经针对速度进行了优化，由于广泛使用而增强了抗错误能力，在论坛上进行了讨论，等等。

`grep`已经在搜索文件和文件名章节中介绍过了。此外，`sed`、`awk`和`perl`是解决从命令行处理各种文本问题的基本工具。在本章中，你将学习字段处理，使用正则表达式进行搜索和替换，根据多行和文件执行操作等。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 本章中提供的示例仅涵盖了一些功能。我已经编写了单独的书籍，以更详细地解释这些工具，包括示例和练习。有关这些书籍的链接，请参阅[`learnbyexample.github.io/books/`](https://learnbyexample.github.io/books/)。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 本章中使用的示例输入文件位于[example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files)目录中。

## sed

命令名`sed`来源于**s**tream **ed**itor。在这里，stream 指的是通过 shell 管道传递的数据。因此，该命令的主要功能是作为**stdin**数据的文本编辑器，其输出目标为**stdout**。如果需要，你也可以编辑文件输入并将更改保存回同一文件。

### 替换

`sed`有各种命令来操作文本输入。其中**替换**命令是最常用的，其语法为`s/REGEXP/REPLACEMENT/FLAGS`。以下是一些基本示例：

```sh
# for each input line, change only the first ',' to '-'
$ printf '1,2,3,4\na,b,c,d\n' | sed 's/,/-/'
1-2,3,4
a-b,c,d

# change all matches by adding the 'g' flag
$ printf '1,2,3,4\na,b,c,d\n' | sed 's/,/-/g'
1-2-3-4
a-b-c-d 
```

这是一个使用文件输入的示例：

```sh
$ cat greeting.txt
Hi there
Have a nice day

# change 'day' to 'weekend'
$ sed 's/day/weekend/g' greeting.txt
Hi there
Have a nice weekend 
```

如果你想发出多个替换命令（或使用几个其他`sed`命令），这取决于所使用的命令。以下是一个示例，你可以使用`-e`选项或使用`;`字符分隔命令。

```sh
# change all occurrences of 'day' to 'weekend'
# add '.' to the end of each line
$ sed 's/day/weekend/g; s/$/./' greeting.txt
Hi there.
Have a nice weekend.

# same thing with the -e option
$ sed -e 's/day/weekend/g' -e 's/$/./' greeting.txt
Hi there.
Have a nice weekend. 
```

### 原地编辑

你可以使用`-i`选项进行原地编辑。向此选项传递一个参数以将原始输入保存为备份。

```sh
$ cat ip.txt
deep blue
light orange
blue delight

# output from sed is written back to 'ip.txt'
# original file is preserved in 'ip.txt.bkp'
$ sed -i.bkp 's/blue/green/g' ip.txt
$ cat ip.txt
deep green
light orange
green delight 
```

### 过滤功能

`sed`命令还具有基于搜索模式（如`grep`）过滤行的功能。并且可以根据需要对这些过滤行应用其他`sed`命令。

```sh
# the -n option disables automatic printing
# the 'p' command prints the contents of the pattern space
# same as: grep 'at'
$ printf 'sea\neat\ndrop\n' | sed -n '/at/p'
eat

# the 'd' command deletes the matching lines
# same as: grep -v 'at'
$ printf 'sea\neat\ndrop\n' | sed '/at/d'
sea
drop

# change commas to hyphens only if the input line contains '2'
$ printf '1,2,3,4\na,b,c,d\n' | sed '/2/ s/,/-/g'
1-2-3-4
a,b,c,d

# change commas to hyphens if the input line does NOT contain '2'
$ printf '1,2,3,4\na,b,c,d\n' | sed '/2/! s/,/-/g'
1,2,3,4
a-b-c-d 
```

你可以使用`q`和`Q`命令在找到匹配行后退出`sed`：

```sh
# quit after a line containing 'st' is found
$ printf 'apple\nsea\neast\ndust' | sed '/st/q'
apple
sea
east

# the matching line won't be printed in this case
$ printf 'apple\nsea\neast\ndust' | sed '/st/Q'
apple
sea 
```

除了正则表达式之外，还可以根据行号、地址范围等进行过滤。

```sh
# perform substitution only for the second line
# use '$' instead of a number to indicate the last input line
$ printf 'gates\nnot\nused\n' | sed '2 s/t/*/g'
gates
no*
used

# address range example, same as: sed -n '3,8!p'
# you can also use regexp to construct address ranges
$ seq 15 24 | sed '3,8d'
15
16
23
24 
```

如果需要对过滤行执行多个命令，可以将这些命令组合在 `{}` 字符内。以下是一个示例：

```sh
# for lines containing 'e', replace 's' with '*' and 't' with '='
# note that the second line isn't changed as there's no 'e'
$ printf 'gates\nnot\nused\n' | sed '/e/{s/s/*/g; s/t/=/g}'
ga=e*
not
u*ed 
```

### 正则表达式替换

这里有一些基于正则表达式的替换示例。`-E` 选项启用 **ERE**（默认是 **BRE**）。在 `grep` 命令的 正则表达式 部分中讨论的大多数语法也适用于 `sed`。

```sh
# replace all sequences of non-digit characters with '-'
$ echo 'Sample123string42with777numbers' | sed -E 's/[⁰-9]+/-/g'
-123-42-777-

# replace numbers >= 100 which can have optional leading zeros
$ echo '0501 035 154 12 26 98234' | sed -E 's/\b0*[1-9][0-9]{2,}\b/X/g'
X 035 X 12 26 X

# reduce \\ to single \ and delete if it is a single \
$ echo '\[\] and \\w and \[a-zA-Z0-9\_\]' | sed -E 's/(\\?)\\/\1/g'
[] and \w and [a-zA-Z0-9_]

# remove two or more duplicate words that are separated by a space character
# \b prevents false matches like 'the theatre', 'sand and stone' etc
$ echo 'aa a a a 42 f_1 f_1 f_13.14' | sed -E 's/\b(\w+)( \1)+\b/\1/g'
aa a 42 f_1 f_13.14

# & backreferences the matched portion
# \u changes the next character to uppercase
$ echo 'hello there. how are you?' | sed 's/\b\w/\u&/g'
Hello There. How Are You?

# replace only the third matching occurrence
$ echo 'apple:123:banana:fig' | sed 's/:/-/3'
apple:123:banana-fig
# change all ':' to ',' only from the second occurrence
$ echo 'apple:123:banana:fig' | sed 's/:/,/2g'
apple:123,banana,fig 
```

`/` 字符通常用作正则表达式的分隔符。但除了反斜杠和换行符之外，任何其他字符都可以用作替代。这有助于避免或减少对分隔符字符进行转义的需求。

```sh
$ echo '/home/learnbyexample/reports' | sed 's#/home/learnbyexample/#~/#'
~/reports

$ echo 'home path is:' | sed 's,$, '"$HOME"','
home path is: /home/learnbyexample 
```

### 进一步阅读

+   我的电子书 [使用 GNU sed 进行 CLI 文本处理](https://github.com/learnbyexample/learn_gnused)

    +   参见我的博客文章 [GNU BRE/ERE 技巧表](https://learnbyexample.github.io/gnu-bre-ere-cheatsheet/)

+   [unix.stackexchange: 使用 sed 和其他工具的常见搜索和替换示例](https://unix.stackexchange.com/q/112023/109046)

## awk

`awk` 是一种编程语言，广泛用于从命令行执行文本处理任务。`awk` 提供了与 `grep` 和 `sed` 命令支持的过滤功能类似的功能，以及一些更实用的特性。类似于许多命令行实用程序，`awk` 可以从 `stdin` 和文件接受输入。

### 正则表达式过滤

为了便于从命令行使用编程功能，有一些快捷方式，例如：

+   `awk '/regexp/'` 是 `awk '$0 ~ /regexp/{print $0}'` 的快捷方式

+   `awk '!/regexp/'` 是 `awk '$0 !~ /regexp/{print $0}'` 的快捷方式

```sh
# same as: grep 'at' and sed -n '/at/p'
$ printf 'gate\napple\nwhat\nkite\n' | awk '/at/'
gate
what

# same as: grep -v 'e' and sed -n '/e/!p'
$ printf 'gate\napple\nwhat\nkite\n' | awk '!/e/'
what

# lines containing 'e' followed by zero or more characters and then 'y'
$ awk '/e.*y/' greeting.txt
Have a nice day 
```

### Awk 特殊变量

下面简要描述了一些特殊变量的用法：

+   `$0` 包含输入记录内容

+   `$1` 第一字段

+   `$2` 第二字段以及后续字段

+   `FS` 输入字段分隔符

+   `OFS` 输出字段分隔符

+   `NF` 字段数

+   `RS` 输入记录分隔符

+   `ORS` 输出记录分隔符

+   `NR` 整个输入的记录数（即行号）

+   `FNR` 每个文件中的记录数

### 默认字段处理

`awk` 会自动根据一个或多个由空格、制表符或换行符组成的序列将输入拆分为字段。此外，输入开头或结尾的任何这三个字符都会被修剪，不会成为字段内容的一部分。字段可以通过 `$N` 访问，其中 `N` 是您需要的字段编号。您也可以传递一个表达式而不是数字字面量来指定所需的字段。

这里有一些示例：

```sh
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14

# print the second field of each input line
$ awk '{print $2}' table.txt
bread
cake
banana

# print lines only if the last field is a negative number
$ awk '$NF&LT0' table.txt
blue cake mug shirt -7 
```

这里是一个应用特定字段替换操作的示例。

```sh
# delete lowercase vowels only from the first field
# gsub() is like the sed substitution command with the 'g' flag
# use sub() if you need to change only the first match
# 1 is a true condition, and thus prints the contents of $0
$ awk '{gsub(/[aeiou]/, "", $1)} 1' table.txt
brwn bread mat hair 42
bl cake mug shirt -7
yllw banana window shoes 3.14 
```

### 条件和动作

到目前为止的示例使用了构建典型 `awk` 单行脚本的一些不同方法。如果您还没有掌握语法，这个通用结构可能有所帮助：

```sh
awk 'cond1{action1} cond2{action2} ... condN{actionN}' 
```

如果没有提供条件，则始终执行操作。在块内，你可以提供多个用分号字符分隔的语句。如果没有提供操作，则默认情况下，如果条件评估为 *true*，则打印 `$0` 变量的内容。在单行中，通常使用 `1` 来表示 `true` 条件，作为打印 `$0` 内容的快捷方式（如前例所示）。如果没有操作，则可以使用分号来终止条件并开始另一个 `condX{actionX}` 片段。

当你需要读取输入之前执行某些操作时，可以使用 `BEGIN{}` 块，而在所有输入处理完毕后执行某些操作时，可以使用 `END{}` 块。

```sh
$ seq 2 | awk 'BEGIN{print "---"} 1; END{print "%%%"}'
---
1
2
%%% 
```

### 正则表达式字段处理

如前所述，`awk` 会自动根据空格、制表符或换行符将输入拆分为字段，这些字段可以通过 `$N` 访问，其中 `N` 是你需要字段编号。你可以使用 `-F` 选项或分配 `FS` 变量来设置基于正则表达式的输入字段分隔符。使用 `OFS` 变量来设置输出字段分隔符。

```sh
$ echo 'goal:amazing:whistle:kwality' | awk -F: '{print $1}'
goal
# one or more alphabets will be considered as the input field separator
$ echo 'Sample123string42with777numbers' | awk -F'[a-zA-Z]+' '{print $2}'
123

$ s='Sample123string42with777numbers'
# -v option helps you set a value for the given variable
$ echo "$s" | awk -F'[0-9]+' -v OFS=, '{print $1, $(NF-1)}'
Sample,with 
```

`FS` 变量允许你定义输入字段 *分隔符*。相比之下，`FPAT`（字段模式）允许你定义字段应由什么组成。

```sh
# lowercase whole words starting with 'b'
$ awk -v FPAT='\\&LTb[a-z]*\\>' -v OFS=, '{$1=$1} 1' table.txt
brown,bread
blue
banana

# fields enclosed within double quotes or made up of non-comma characters
$ s='eagle,"fox,42",bee,frog'
$ echo "$s" | awk -v FPAT='"[^"]*"|[^,]*' '{print $2}'
"fox,42" 
```

### 记录分隔符

默认情况下，换行符用作输入和输出记录分隔符。你可以使用 `RS` 和 `ORS` 变量来更改它们。

```sh
# print records containing 'i' as well as 't'
$ printf 'Sample123string42with777numbers' | awk -v RS='[0-9]+' '/i/ && /t/'
string
with

# empty RS is paragraph mode, uses two or more newlines as the separator
$ printf 'apple\nbanana\nfig\n\n\n123\n456' | awk -v RS= 'NR==1'
apple
banana
fig

# change ORS depending on some condition
$ seq 9 | awk '{ORS = NR%3 ? "-" : "\n"} 1'
1-2-3
4-5-6
7-8-9 
```

### 状态机

`condX{actionX}` 简略语法使得编写状态机更加简洁。这对于解决依赖于多个记录内容的问题非常有用。

以下是一个示例，打印匹配的行以及随后 `c` 行：

```sh
# same as: grep --no-group-separator -A1 'blue'
# print matching line as well as the one that follows it
$ printf 'red\nblue\ngreen\nteal\n' | awk -v c=1 '/blue/{n=c+1} n && n--'
blue
green

# print matching line as well as two lines that follow
$ printf 'red\nblue\ngreen\nteal\n' | awk -v c=2 '/blue/{n=c+1} n && n--'
blue
green
teal 
```

考虑以下具有不同标记（包含 `start` 和 `end` 的行）的输入文件：

```sh
$ cat uniform.txt
mango
icecream
--start 1--
1234
6789
**end 1**
how are you
have a nice day
--start 2--
a
b
c
**end 2**
par,far,mar,tar 
```

以下是一些处理此类有界记录的示例：

```sh
# same as: sed -n '/start/,/end/p' uniform.txt
$ awk '/start/{f=1} f; /end/{f=0}' uniform.txt
--start 1--
1234
6789
**end 1**
--start 2--
a
b
c
**end 2**

# you can re-arrange and invert the conditions to create other combinations
# for example, exclude the ending match
$ awk '/start/{f=1} /end/{f=0} f' uniform.txt
--start 1--
1234
6789
--start 2--
a
b
c 
```

以下是一个示例，仅当第一个记录包含 `ar` 且第二个记录包含 `nice` 时打印两个连续的记录：

```sh
$ awk 'p ~ /ar/ && /nice/{print p ORS $0} {p=$0}' uniform.txt
how are you
have a nice day 
```

### 两个文件处理

本节重点介绍依赖于两个或多个文件内容的问题解决方法。这些通常基于比较记录和字段。以下示例将使用这两个文件：

```sh
$ paste c1.txt c2.txt
Blue    Black
Brown   Blue
Orange  Green
Purple  Orange
Red     Pink
Teal    Red
White   White 
```

用于在两个文件之间查找公共行的 *关键* 功能：

+   对于两个文件作为输入，`NR==FNR` 仅在处理第一个文件时为 *true*

    +   `FNR` 是记录号，类似于 `NR`，但会为每个输入文件重置

+   `next` 将跳过其余代码并获取下一个记录

+   `a[$0]` 本身是一个有效的语句，在数组 `a` 中创建一个以 `$0` 为键的未初始化元素（如果键尚不存在）

+   `$0 in a` 检查给定的字符串（此处为 `$0`）是否作为键存在于数组 `a` 中

```sh
# common lines, same as: grep -Fxf c1.txt c2.txt
$ awk 'NR==FNR{a[$0]; next} $0 in a' c1.txt c2.txt
Blue
Orange
Red
White

# lines present in c2.txt but not in c1.txt
$ awk 'NR==FNR{a[$0]; next} !($0 in a)' c1.txt c2.txt
Black
Green
Pink 
```

> ![warning](img/b5314b4a0acf0f436c4bf59486793342.png) 注意，如果第一个文件为空，则 `NR==FNR` 逻辑将失败。有关解决方案，请参阅 [这个 unix.stackexchange 线程](https://unix.stackexchange.com/a/237110/109046)。

### 移除重复项

`awk '!a[$0]++'` 是最著名的 `awk` 单行命令之一。它在保留输入顺序的同时消除基于行的重复项。以下示例展示了这一功能的使用，以及逻辑是如何工作的。

```sh
$ cat purchases.txt
coffee
tea
washing powder
coffee
toothpaste
tea
soap
tea

$ awk '{print +a[$0] "\t" $0; a[$0]++}' purchases.txt
0       coffee
0       tea
0       washing powder
1       coffee
0       toothpaste
1       tea
0       soap
2       tea

# only those entries with zero in the first column will be retained
$ awk '!a[$0]++' purchases.txt
coffee
tea
washing powder
toothpaste
soap 
```

### 进一步阅读

+   我的电子书 [使用 GNU awk 进行 CLI 文本处理](https://github.com/learnbyexample/learn_gnuawk)

    +   参见我的博客文章 [GNU BRE/ERE 技巧表](https://learnbyexample.github.io/gnu-bre-ere-cheatsheet/)

+   [在线 gawk 手册](https://www.gnu.org/software/gawk/manual/)

+   我的博客文章 [使用 GNU datamash 进行 CLI 计算](https://learnbyexample.github.io/cli-computation-gnu-datamash/)

## perl

Perl 是一种具有众多内置功能和强大生态系统的脚本语言。Perl 单行命令可用于文本处理，类似于 `grep`、`sed`、`awk` 等。类似于许多命令行实用程序，`perl` 可以从 `stdin` 和文件参数接受输入。

### 基本单行命令

```sh
# print all lines containing 'at'
# same as: grep 'at' and sed -n '/at/p' and awk '/at/'
$ printf 'gate\napple\nwhat\nkite\n' | perl -ne 'print if /at/'
gate
what

# print all lines NOT containing 'e'
# same as: grep -v 'e' and sed -n '/e/!p' and awk '!/e/'
$ printf 'gate\napple\nwhat\nkite\n' | perl -ne 'print if !/e/'
what 
```

`-e` 选项接受代码作为命令行参数。有许多快捷方式可以减少输入的字符数。在上面的示例中，已使用正则表达式来过滤输入。当未指定输入字符串时，测试是对特殊变量 `$_` 执行的，它包含当前输入行的内容。`$_` 也是许多函数（如 `print` 和 `length`）的默认参数。为了总结：

+   `/REGEXP/FLAGS` 是 `$_ =~ m/REGEXP/FLAGS` 的快捷方式

+   `!/REGEXP/FLAGS` 是 `$_ !~ m/REGEXP/FLAGS` 的快捷方式

在下面的示例中，使用 `-p` 选项代替 `-n`。这有助于在处理每行输入后自动打印 `$_` 的值。

```sh
# same as: sed 's/:/-/' and awk '{sub(/:/, "-")} 1'
$ printf '1:2:3:4\na:b:c:d\n' | perl -pe 's/:/-/'
1-2:3:4
a-b:c:d

# same as: sed 's/:/-/g' and awk '{gsub(/:/, "-")} 1'
$ printf '1:2:3:4\na:b:c:d\n' | perl -pe 's/:/-/g'
1-2-3-4
a-b-c-d 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 与 `sed` 类似，你可以使用 `-i` 选项进行原地编辑。

### Perl 特殊变量

以下简要描述了一些特殊变量的用途：

+   `$_` 包含输入记录内容

+   `@F` 包含字段内容的数组（使用 `-a` 和 `-F` 选项）

    +   `$F[0]` 第一个字段

    +   `$F[1]` 第二字段等等

    +   `$F[-1]` 最后一个字段

    +   `$F[-2]` 第二个最后一个字段等等

    +   `$#F` 最后一个字段的索引

+   `$.` 记录数（即行号）

+   `$1` 对第一个捕获组的反向引用

+   `$2` 对第二个捕获组的反向引用等等

+   `$&` 对整个匹配部分的反向引用

你将在接下来的部分中看到使用此类变量的示例。

### 自动拆分

这里有一些基于特定字段而不是整行的示例。`-a` 选项将导致输入行根据空白字符分割，字段内容可以使用特殊数组变量 `@F` 访问。将抑制前导和尾随空白字符，因此不可能有空字段。

```sh
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14

# same as: awk '{print $2}' table.txt
$ perl -lane 'print $F[1]' table.txt
bread
cake
banana

# same as: awk '$NF&LT0' table.txt
$ perl -lane 'print if $F[-1] < 0' table.txt
blue cake mug shirt -7

# same as: awk '{gsub(/b/, "B", $1)} 1' table.txt
$ perl -lane '$F[0] =~ s/b/B/g; print "@F"' table.txt
Brown bread mat hair 42
Blue cake mug shirt -7
yellow banana window shoes 3.14 
```

当你在双引号内使用数组（如上面示例中的 `"@F"`），字段之间将用空格字符打印。`join` 函数是打印数组内容并使用自定义字段分隔符的一种方法。以下是一个示例：

```sh
# print contents of @F array with colon as the separator
$ perl -lane 'print join ":", @F' table.txt
brown:bread:mat:hair:42
blue:cake:mug:shirt:-7
yellow:banana:window:shoes:3.14 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 在上述示例中，使用了 `-l` 选项来从输入行中移除记录分隔符（默认为换行符）。当使用 `print` 函数时，移除的记录分隔符将被添加回。

### 正则表达式字段分隔符

你可以使用 `-F` 选项指定用于输入字段分隔的正则表达式模式。

```sh
$ echo 'apple,banana,cherry' | perl -F, -lane 'print $F[1]'
banana

$ s='Sample123string42with777numbers'
$ echo "$s" | perl -F'\d+' -lane 'print join ",", @F'
Sample,string,with,numbers 
```

### 强大功能

当我需要强大的正则表达式功能并使用大量的内置函数和库时，我选择使用 Perl 而不是 `grep`、`sed` 和 `awk`。

这里有一些示例展示了 BRE/ERE 中没有的正则表达式功能：

```sh
# reverse lowercase alphabets at the end of input lines
# the 'e' flag allows you to use Perl code in the replacement section
$ echo 'fig 42apples' | perl -pe 's/[a-z]+$/reverse $&/e'
fig 42selppa

# replace arithmetic expressions with their results
$ echo '42*10 200+100 22/7' | perl -pe 's|\d+[+/*-]\d+|$&|gee'
420 300 3.14285714285714

# exclude terms in the search pattern
$ s='orange apple appleseed'
$ echo "$s" | perl -pe 's#\bapple\b(*SKIP)(*F)|\w+#($&)#g'
(orange) apple (appleseed) 
```

下面是一些展示内置功能的示例：

```sh
# filter fields containing 'in' or 'it' or 'is'
$ s='goal:amazing:42:whistle:kwality:3.14'
$ echo "$s" | perl -F: -lane 'print join ":", grep {/i[nts]/} @F'
amazing:whistle:kwality

# sort numbers in ascending order
# use {$b <=> $a} for descending order
$ echo '23 756 -983 5' | perl -lane 'print join " ", sort {$a <=> $b} @F'
-983 5 23 756

# sort strings in ascending order
$ s='floor bat to dubious four'
$ echo "$s" | perl -lane 'print join ":", sort @F'
bat:dubious:floor:four:to

# unique fields, maintains input order of elements
# -M option helps you load modules
$ s='3,b,a,3,c,d,1,d,c,2,2,2,3,1,b'
$ echo "$s" | perl -MList::Util=uniq -F, -lane 'print join ",", uniq @F'
3,b,a,c,d,1,2 
```

### 进一步阅读

+   [perldoc: Perl 简介](https://perldoc.perl.org/perlintro)

+   [perldoc: 正则表达式教程](https://perldoc.perl.org/perlretut)

+   我的电子书 [Perl One-Liners Guide](https://github.com/learnbyexample/learn_perl_oneliners)

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录中的输入文件进行以下练习。

**1)** 对于给定的输入，将所有出现的 `0xA0` 替换为 `0x50`，将 `0xFF` 替换为 `0x7F`。

```sh
$ printf 'a1:0xA0, a2:0xA0A1\nb1:0xFF, b2:0xBE\n'
a1:0xA0, a2:0xA0A1
b1:0xFF, b2:0xBE

$ printf 'a1:0xA0, a2:0xA0A1\nb1:0xFF, b2:0xBE\n' | sed # ???
a1:0x50, a2:0x50A1
b1:0x7F, b2:0xBE 
```

**2)** 仅从给定输入中删除第三行。

```sh
$ seq 34 37 | # ???
34
35
37 
```

**3)** 对于输入文件 `sample.txt`，显示包含 `it` 但不包含 `do` 的所有行。

```sh
# ???
 7) Believe it 
```

**4)** 对于输入文件 `purchases.txt`，删除包含 `tea` 的所有行。也将所有出现的 `coffee` 替换为 `milk`。将更改写回输入文件本身。原始内容应保存到 `purchases.txt.orig`。之后，从该备份文件恢复内容。

```sh
# make the changes
# ???
$ ls purchases*
purchases.txt  purchases.txt.orig
$ cat purchases.txt
milk
washing powder
milk
toothpaste
soap

# restore the contents
# ???
$ ls purchases*
purchases.txt
$ cat purchases.txt
coffee
tea
washing powder
coffee
toothpaste
tea
soap
tea 
```

**5)** 对于输入文件 `sample.txt`，显示从文件开始到首次出现 `are` 的所有行。

```sh
# ???
 1) Hello World
 2) 
 3) Hi there
 4) How are you 
```

**6)** 删除从包含 `start` 的行到包含 `end` 的行之间的所有行组，对于 `uniform.txt` 输入文件。

```sh
# ???
mango
icecream
how are you
have a nice day
par,far,mar,tar 
```

**7)** 将所有出现的 `42` 替换为 `[42]`，除非它位于单词的边缘。

```sh
$ echo 'hi42bye nice421423 bad42 cool_4242a 42c' | sed # ???
hi[42]bye nice[42]1[42]3 bad42 cool_[42][42]a 42c 
```

**8)** 将所有以相同单词字符开头和结尾的整个单词替换为 `X`。

```sh
$ echo 'oreo not a _oh_ pip RoaR took 22 Pop' | sed # ???
X not X X X X took X Pop 
```

**9)** 对于输入文件 `anchors.txt`，将 markdown 锚点转换为如下所示的超链接。

```sh
$ cat anchors.txt
# &LTa name="regular-expressions">&LT/a>Regular Expressions
## &LTa name="subexpression-calls">&LT/a>Subexpression calls
## &LTa name="the-dot-meta-character">&LT/a>The dot meta character

$ sed # ???
Regular Expressions
Subexpression calls
The dot meta character 
```

**10)** 将所有出现的 `e` 替换为 `3`，除了前两个匹配项。

```sh
$ echo 'asset sets tests site' | sed # ???
asset sets t3sts sit3

$ echo 'sample item teem eel' | sed # ???
sample item t33m 33l 
```

**11)** 以下示例字符串使用`,`作为分隔符，字段值也可以为空。使用`sed`仅替换第三个字段为`42`。

```sh
$ echo 'lion,,ant,road,neon' | sed # ???
lion,,42,road,neon

$ echo ',,,' | sed # ???
,,42, 
```

**12)** 对于输入文件`table.txt`，计算并显示每行最后一个字段中数字的乘积。考虑空格作为此文件的字段分隔符。

```sh
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14

# ???
-923.16 
```

**13)** 从每条输入行中提取`()`或`)(`之间的内容。假设每行中`()`字符只会出现一次。

```sh
$ printf 'apple(ice)pie\n(almond)pista\nyo)yoyo(yo\n'
apple(ice)pie
(almond)pista
yo)yoyo(yo

$ printf 'apple(ice)pie\n(almond)pista\nyo)yoyo(yo\n' | awk # ???
ice
almond
yoyo 
```

**14)** 对于输入文件`scores.csv`，按照以下格式显示`Name`和`Physics`字段。

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

# ???
Name:Physics
Ith:100
Cy:98
Lin:83 
```

**15)** 按照以下格式提取并显示第三和第一个单词。

```sh
$ echo '%whole(Hello)--{doubt}==ado==' | # ???
doubt:whole

$ echo 'just,\joint*,concession_42<=nice' | # ???
concession_42:just 
```

**16)** 对于输入文件`scores.csv`，添加一个名为**GP**的列，该列通过将 50%的权重分配给数学，物理和化学各 25%来计算。

```sh
$ awk # ???
Name,Maths,Physics,Chemistry,GP
Ith,100,100,100,100
Cy,97,98,95,96.75
Lin,78,83,80,79.75 
```

**17)** 从`para.txt`输入文件中，显示包含任何数字字符的所有段落。

```sh
$ cat para.txt
hi there
how are you

2 apples
12 bananas

blue sky
yellow sun
brown earth

$ awk # ???
2 apples
12 bananas 
```

**18)** 输入具有 ASCII NUL 字符作为记录分隔符。将其更改为点和新行字符，如下所示。

```sh
$ printf 'apple\npie\0banana\ncherry\0' | awk # ???
apple
pie.
banana
cherry. 
```

**19)** 对于输入文件`sample.txt`，如果在前两行中找到`you`，则打印包含`do`的匹配行。例如，如果`do`在第 10 行找到，而第 8 行包含`you`，则第 10 行应被打印。

```sh
# ???
 6) Just do-it 
```

**20)** 对于输入文件`blocks.txt`，从包含恰好`%=%=`的行中提取内容，直到但不包括下一个这样的行。要提取的块由通过`-v`选项传递的变量`n`指示。

```sh
$ cat blocks.txt
%=%=
apple
banana
%=%=
brown
green

$ awk -v n=1 # ???
%=%=
apple
banana
$ awk -v n=2 # ???
%=%=
brown
green 
```

**21)** 使用`awk`命令显示`c1.txt`中存在但不在`c2.txt`中的行。

```sh
$ awk # ???
Brown
Purple
Teal 
```

**22)** 通过匹配`names.txt`文件中的名称列表来显示`scores.csv`中的行。

```sh
$ printf 'Ith\nLin\n' > names.txt

$ awk # ???
Ith,100,100,100
Lin,78,83,80

$ rm names.txt 
```

**23)** 仅保留`duplicates.txt`输入文件中的第一个重复行副本。仅使用最后一个字段的内容来确定重复项。

```sh
$ cat duplicates.txt
brown,toy,bread,42
dark red,ruby,rose,111
blue,ruby,water,333
dark red,sky,rose,555
yellow,toy,flower,333
white,sky,bread,111
light red,purse,rose,333

# ???
brown,toy,bread,42
dark red,ruby,rose,111
blue,ruby,water,333
dark red,sky,rose,555 
```

**24)** 对于输入文件`table.txt`，如果第二个字段以`b`开头，则打印输入行。使用`awk`和`perl`构建解决方案。

```sh
$ awk # ???
brown bread mat hair 42
yellow banana window shoes 3.14

$ perl # ???
brown bread mat hair 42
yellow banana window shoes 3.14 
```

**25)** 对于输入文件`table.txt`，仅保留第二个最后字段。将更改写回输入文件本身。原始内容应保存到`table.txt.bkp`。之后，从该备份文件恢复内容。

```sh
# make the changes
$ perl # ???
$ ls table*
table.txt  table.txt.bkp
$ cat table.txt
hair
shirt
shoes

# restore the contents
# ???
$ ls table*
table.txt
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14 
```

**26)** 反转`table.txt`输入文件的第一字段内容。

```sh
# ???
nworb bread mat hair 42
eulb cake mug shirt -7
wolley banana window shoes 3.14 
```

**27)** 按字典顺序对给定的逗号分隔输入进行排序。将输出字段分隔符更改为冒号字符。

```sh
$ ip='floor,bat,to,dubious,four'
$ echo "$ip" | perl # ???
bat:dubious:floor:four:to 
```

**28)** 过滤包含数字字符的字段。

```sh
$ ip='5pearl 42 east 1337 raku_6 lion 3.14'
$ echo "$ip" | perl # ???
5pearl 42 1337 raku_6 3.14 
```

**29)** 下面的输入中有几个以数字结尾的单词。将包含`test`的单词更改为与下面的输出匹配。也就是说，将匹配的部分重新编号为`1`、`2`等。不包含`test`的单词不应更改。

```sh
$ ip='test_12:test123\nanother_test_4,no_42\n'
$ printf '%b' "$ip"
test_12:test123
another_test_4,no_42

$ printf '%b' "$ip" | perl # ???
test_1:test2
another_test_3,no_42 
```

**30)** 对于输入文件`table.txt`，将第三个字段的内容更改为全部大写。使用`sed`、`awk`和`perl`构建解决方案。

```sh
$ sed # ???
brown bread MAT hair 42
blue cake MUG shirt -7
yellow banana WINDOW shoes 3.14

$ awk # ???
brown bread MAT hair 42
blue cake MUG shirt -7
yellow banana WINDOW shoes 3.14

$ perl # ???
brown bread MAT hair 42
blue cake MUG shirt -7
yellow banana WINDOW shoes 3.14 
```
