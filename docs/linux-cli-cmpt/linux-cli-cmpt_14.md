# 各式各样的文本处理工具

> [`learnbyexample.github.io/cli-computing/assorted-text-processing-tools.html`](https://learnbyexample.github.io/cli-computing/assorted-text-processing-tools.html)

专业的文本处理工具实在太多了。本章将讨论一些在前几章中没有涉及到的命令。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章中使用的示例输入文件。

## seq

`seq` 命令是一个便捷的工具，可以生成升序或降序的数字序列。支持整数和浮点数。您还可以自定义数字的格式和它们之间的分隔符。

您需要三个数字来生成等差数列——**起始值**、**步长**和**结束值**。当您只传递一个数字作为结束值时，默认的起始值和步长值是 `1`。传递两个数字被视为起始值和结束值（按此顺序）。

```sh
# start=1, step=1 and stop=3
$ seq 3
1
2
3

# start=25434, step=1 and stop=25437
$ seq 25434 25437
25434
25435
25436
25437

# start=-5, step=1 and stop=-3
$ seq -5 -3
-5
-4
-3

# start=0.25, step=0.33 and stop=1.12
$ seq 0.25 0.33 1.12
0.25
0.58
0.91 
```

通过使用负步长值，您可以生成降序序列。

```sh
$ seq 3 -1 1
3
2
1 
```

您可以使用 `-s` 选项更改序列数字之间的分隔符。始终在最后一个数字之后添加单个换行符。

```sh
$ seq -s' - ' 4
1 - 2 - 3 - 4

$ seq -s: 1.2e2 0.752 1.22e2
120.000:120.752:121.504 
```

`-w` 选项将使用前导零使输出数字的宽度相等。起始和结束值之间的最大宽度将被使用。

```sh
$ seq -w 8 10
08
09
10

$ seq -w 0003
0001
0002
0003 
```

您可以使用 `-f` 选项为 `printf` 风格的浮点数格式化。

```sh
$ seq -f'%g' -s: 1 0.75 3
1:1.75:2.5

$ seq -f'%.4f' -s: 1 0.75 3
1.0000:1.7500:2.5000

$ seq -f'%.3e' 1.2e2 0.752 1.22e2
1.200e+02
1.208e+02
1.215e+02 
```

## shuf

默认情况下，`shuf` 将随机化输入行的顺序。您可以使用 `-n` 选项来限制输出行的数量。

```sh
$ printf 'apple\nbanana\ncherry\nfig\nmango' | shuf
banana
cherry
mango
apple
fig

$ printf 'apple\nbanana\ncherry\nfig\nmango' | shuf -n2
mango
cherry 
```

您可以使用 `-e` 选项将多个输入行指定为命令的参数。如果您想允许输入行重复，则 `-r` 选项很有帮助。此选项通常与 `-n` 配对，以限制输出中的行数。

```sh
$ shuf -n4 -r -e brown green blue
green
brown
blue
green 
```

`-i` 选项可以帮助您生成随机正整数。

```sh
$ shuf -n3 -i 100-200
170
112
148 
```

## cut

`cut` 是许多字段处理用例的便捷工具。与 `awk` 和 `perl` 命令相比，其功能有限，但较小的范围也导致处理速度更快。

默认情况下，`cut` 根据制表符将输入内容拆分为字段，您可以使用 `-d` 选项来更改它。`-f` 选项允许您从每行输入中选择所需的字段。要提取多个字段，请指定由逗号字符分隔的选择。默认情况下，不包含输入分隔符的行仍然是输出的一部分。您可以使用 `-s` 选项来抑制此类行。

```sh
# second field
$ printf 'apple\tbanana\tcherry\n' | cut -f2
banana

# first and third fields
$ printf 'apple\tbanana\tcherry\n' | cut -f1,3
apple   cherry

# setting -d automatically changes the output delimiter as well
$ echo 'one;two;three;four;five' | cut -d';' -f2,5
two;five 
```

您可以使用 `-` 字符来指定字段范围。起始或结束字段编号可以跳过，但不能同时跳过。

```sh
# 2nd, 3rd and 4th fields
$ printf 'apple\tbanana\tcherry\tdates\n' | cut -f2-4
banana  cherry  dates

# all fields from the start till the 3rd field
$ printf 'apple\tbanana\tcherry\tdates\n' | cut -f-3
apple   banana  cherry

# 1st field and all fields from the 3rd field till the end
$ printf 'apple\tbanana\tcherry\tdates\n' | cut -f1,3-
apple   cherry  dates 
```

使用 `--output-delimiter` 选项可以自定义输出分隔符为任何您选择的字符串。

```sh
# same as: tr '\t' ','
$ printf 'apple\tbanana\tcherry\n' | cut --output-delimiter=, -f1-
apple,banana,cherry

# multicharacter example
$ echo 'one;two;three;four' | cut -d';' --output-delimiter=' : ' -f1,3-
one : three : four 
```

`--complement` 选项允许您反转字段选择。

```sh
# except the second field
$ printf 'apple ball cat\n1 2 3 4 5' | cut --complement -d' ' -f2
apple cat
1 3 4 5

# except the first and third fields
$ printf 'apple ball cat\n1 2 3 4 5' | cut --complement -d' ' -f1,3
ball
2 4 5 
```

您可以使用 `-b` 或 `-c` 选项从每行输入中选择指定的字节。语法与 `-f` 选项相同。`-c` 选项旨在用于多字节字符选择，但到目前为止它的工作方式与 `-b` 选项完全相同。

```sh
$ printf 'apple\tbanana\tcherry\n' | cut -c2,8,11
pan

$ printf 'apple\tbanana\tcherry\n' | cut -c2,8,11 --output-delimiter=-
p-a-n

$ printf 'apple\tbanana\tcherry\n' | cut --complement -c13-
apple   banana

$ printf 'cat-bat\ndog:fog' | cut -c5-
bat
fog 
```

## column

`column` 命令是一个巧妙的工具，可以按列对齐输入数据。默认情况下，使用空白字符作为输入分隔符。空格字符用于对齐输出列，因此制表符等空白字符将被转换为空格。

```sh
$ printf 'one two three\nfour five six\nseven eight nine\n'
one two three
four five six
seven eight nine

$ printf 'one two three\nfour five six\nseven eight nine\n' | column -t
one    two    three
four   five   six
seven  eight  nine 
```

您可以使用 `-s` 选项自定义输入分隔符。请注意，输出分隔符仍然仅由空格组成。

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

$ column -s, -t scores.csv
Name  Maths  Physics  Chemistry
Ith   100    100      100
Cy    97     98       95
Lin   78     83       80

$ printf '1:-:2:-:3\napple:-:banana:-:cherry\n' | column -s:-: -t
1      2       3
apple  banana  cherry 
```

> ![警告](img/b5314b4a0acf0f436c4bf59486793342.png) 输入应在末尾包含换行符，否则您将得到错误：
> 
> ```sh
> $ printf '1 2 3\na   b   c' | column -t
> column: line too long
> 1  2  3 
> ```

## tr

`tr` 帮助您将一组字符映射到另一组字符。诸如范围、重复、字符集、压缩、补集等功能使其成为必须了解的文本处理工具。

`tr` 仅在 `stdin` 数据上工作，因此您需要使用 shell 输入重定向来进行文件输入。以下是一些基本示例：

```sh
# 'l' maps to '1', 'e' to '3', 't' to '7' and 's' to '5'
$ echo 'leet speak' | tr 'lets' '1375'
1337 5p3ak

# example with shell metacharacters
$ echo 'apple;banana;cherry' | tr ';' ':'
apple:banana:cherry

# swap case
$ echo 'Hello World' | tr 'a-zA-Z' 'A-Za-z'
hELLO wORLD

$ tr 'a-z' 'A-Z' &LTgreeting.txt
HI THERE
HAVE A NICE DAY 
```

您可以使用 `-d` 选项指定要删除的一组字符。`-c` 选项将反转第一组字符。以下是一些示例：

```sh
$ echo '2021-08-12' | tr -d '-'
20210812

$ s='"Hi", there! How *are* you? All fine here.'
$ echo "$s" | tr -d '[:punct:]'
Hi there How are you All fine here

# retain alphabets, whitespaces, period, exclamation and question mark
$ echo "$s" | tr -cd 'a-zA-Z.!?[:space:]'
Hi there! How are you? All fine here. 
```

`-s` 选项将连续重复的字符更改为该字符的单个副本。

```sh
# squeeze lowercase alphabets
$ echo 'HELLO... hhoowwww aaaaaareeeeee yyouuuu!!' | tr -s 'a-z'
HELLO... how are you!!

# translate and squeeze
$ echo 'hhoowwww aaaaaareeeeee yyouuuu!!' | tr -s 'a-z' 'A-Z'
HOW ARE YOU!!

# delete and squeeze
$ echo 'hhoowwww aaaaaareeeeee yyouuuu!!' | tr -sd '!' 'a-z'
how are you

# squeeze other than lowercase alphabets
$ echo 'apple    noon     banana!!!!!' | tr -cs 'a-z'
apple noon banana! 
```

## paste

`paste` 通常用于按列合并两个或多个文件。它还具有序列化数据的有用功能。默认情况下，`paste` 在输入文件的对应行之间添加制表符。

```sh
$ cat colors_1.txt
Blue
Brown
Orange
Purple
$ cat colors_2.txt
Black
Blue
Green
Orange

$ paste colors_1.txt colors_2.txt
Blue    Black
Brown   Blue
Orange  Green
Purple  Orange 
```

您可以使用 `-d` 选项更改列之间的分隔符。即使某些输入文件的数据已耗尽，也会添加分隔符。

```sh
$ paste -d'|' <(seq 3) <(seq 4 5) <(seq 6 8)
1|4|6
2|5|7
3||8

# note that the space between -d and the empty string is necessary here
$ paste -d '' <(seq 3) <(seq 6 8)
16
27
38

$ paste -d'\n' <(seq 11 12) <(seq 101 102)
11
101
12
102 
```

您可以使用空文件来在列之间获得多字符分隔。

```sh
$ paste -d' : ' <(seq 3) /dev/null /dev/null <(seq 4 6)
1 : 4
2 : 5
3 : 6 
```

如果您多次使用 `-`，每次遇到 `-` 时 `paste` 都会从 `stdin` 数据中消耗一行。这与多次使用相同文件名不同，在这种情况下，它们被视为单独的输入。

```sh
# five columns
$ seq 10 | paste -d: - - - - -
1:2:3:4:5
6:7:8:9:10

# use redirection for file input
$ &LTcolors_1.txt paste -d: - - -
Blue:Brown:Orange
Purple:: 
```

`-s` 选项允许您使用给定的分隔符将文件中的所有输入行合并为单行。多个输入文件被视为单独处理。`paste` 将确保添加一个最后的换行符，即使输入中不存在也是如此。

```sh
# &LTcolors_1.txt tr '\n' ',' will give you a trailing comma
$ paste -sd, colors_1.txt
Blue,Brown,Orange,Purple

# multiple file example
$ paste -sd: colors_1.txt colors_2.txt
Blue:Brown:Orange:Purple
Black:Blue:Green:Orange 
```

## pr

> 分页或按列打印文件。

如上所述的手册中的引言中所述，`pr` 命令主要用于两项任务。本节将仅讨论列对齐功能和一些杂项任务。如果您想进一步探索，这里有一个分页示例。`pr` 命令将添加空白行、标题等，使其适合打印。

```sh
$ pr greeting.txt | head -n8

2024-05-17 10:48                   greeting.txt                   Page 1

Hi there
Have a nice day 
```

`--columns` 和 `-a` 选项可以用来以两种不同的方式合并输入行：

+   分割输入文件，然后按列合并它们

+   合并连续行，类似于 `paste` 命令

这里有一个开始示例。请注意，`-N` 与使用 `--columns=N` 相同，其中 `N` 是你希望在输出中拥有的列数。默认页面宽度是 `72`，这意味着每列只能有最多 `72/N` 个字符（包括分隔符）。制表符和空格字符将按需用于填充列。你可以使用 `-J` 选项来防止 `pr` 截断较长的列。这里使用 `-t` 选项来关闭分页功能。

```sh
# split input into three parts
# each column width is 72/3 = 24 characters max
$ seq 9 | pr -3t
1                       4                       7
2                       5                       8
3                       6                       9 
```

你可以使用 `-s` 选项来自定义分隔符。默认是制表符，你可以将其更改为任何其他字符串值。`-s` 选项还关闭行截断，因此不需要 `-J` 选项。使用 `-a` 选项合并连续行，类似于前面看到的 `paste` 命令示例。

```sh
# tab is the default separator when no argument is passed to the -s option
$ seq 9 | pr -3ts
1       4       7
2       5       8
3       6       9

# multicharacter custom separator example
$ seq 9 | pr -3ats' : '
1 : 2 : 3
4 : 5 : 6
7 : 8 : 9

# unlike paste, pr doesn't add separators if the last row has less columns to fill
$ seq 10 | pr -4ats,
1,2,3,4
5,6,7,8
9,10 
```

然而，默认页面宽度 `72` 仍然可能引起问题，你可以通过使用 `-w` 选项来防止这些问题。`-w` 选项覆盖了 `-s` 选项对行截断的影响，所以除非你真的需要截断，否则也要使用 `-J` 选项。

```sh
$ seq 6 | pr -J -w10 -3ats'::::'
pr: page width too narrow

$ seq 6 | pr -J -w11 -3ats'::::'
1::::2::::3
4::::5::::6 
```

可以使用 `-m` 选项按列合并两个或更多输入文件。如前所述，需要 `-t` 选项来忽略分页功能，而 `-s` 可以用来自定义分隔符。

```sh
# same as: paste -d' : ' <(seq 3) /dev/null /dev/null <(seq 4 6)
$ pr -mts' : ' <(seq 3) <(seq 4 6)
1 : 4
2 : 5
3 : 6 
```

## rev

`rev` 命令按字符逐行反转每个输入行。如果输入中没有换行符，则不会添加到末尾。以下是一些示例：

```sh
$ echo 'This is a sample text' | rev
txet elpmas a si sihT

$ printf 'apple\nbanana\ncherry\n' | rev
elppa
ananab
yrrehc

$ printf 'malayalam\nnoon\n' | rev
malayalam
noon 
```

## split

`split` 命令根据行数、字节、文件大小等将输入分割成更小的部分。你还可以在保存结果之前对分割的部分执行另一个命令。一个示例用法是将大文件作为多个部分发送，作为在线传输大小限制的解决方案。

默认情况下，`split` 命令每次将输入的 `1000` 行分割。换行符是默认的行分隔符。你可以传递单个文件或 `stdin` 数据作为输入。如果你需要连接多个输入源，请使用 `cat`。默认情况下，输出文件将被命名为 `xaa`、`xab`、`xac` 等等（其中 `x` 是前缀）。如果文件名用尽，将附加两个更多字母，并按需继续该模式。如果输入行数不能被均匀分割，最后一个文件将包含少于 `1000` 行。

```sh
# divide input 1000 lines at a time
$ seq 10000 | split

# output filenames
$ ls x*
xaa  xab  xac  xad  xae  xaf  xag  xah  xai  xaj

# preview of some of the output files
$ head -n1 xaa xab xae xaj
==> xaa <==
1

==> xab <==
1001

==> xae <==
4001

==> xaj <==
9001 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 更多示例、自定义选项和其他详细信息，请参阅我的 [CLI text processing with GNU Coreutils](https://github.com/learnbyexample/cli_text_processing_coreutils) 电子书中的 [split 章节](https://learnbyexample.github.io/cli_text_processing_coreutils/split.html)。

## csplit

`csplit` 命令可以根据行号和正则表达式模式将输入分割成更小的部分。

您可以根据特定的行号将输入分割成两部分。为此，指定输入源（文件名或 `stdin` 数据）之后的行号。第一个输出文件将包含给定行号之前的输入行，第二个输出文件将包含其余内容。默认情况下，输出文件将被命名为 `xx00`、`xx01`、`xx02` 等等（其中 `xx` 是前缀）。如果需要，数值后缀将自动使用更多数字。

```sh
# split input into two based on line number 2
# the -q option suppresses output showing number of bytes written for each file
$ seq 4 | csplit -q - 2

# first output file will have the first line
# second output file will have the rest
$ head xx*
==> xx00 <==
1

==> xx01 <==
2
3
4 
```

您还可以根据匹配给定正则表达式的行来分割输入。生成的输出将根据用于包围正则表达式的 `//` 或 `%%` 分隔符而变化。当使用 `/regexp/` 时，输出类似于基于行号分割。第一个输出文件将包含匹配给定正则表达式的第一个出现之前的输入行，第二个输出文件将包含其余内容。

考虑以下示例输入文件：

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
```

下面是一个使用 `/regexp/` 语法分割输入文件的示例：

```sh
# match a line containing 't' followed by zero or more characters and then 'p'
# 'toothpaste' is the only match for this input file
$ csplit -q purchases.txt '/t.*p/'

$ head xx*
==> xx00 <==
coffee
tea
washing powder
coffee

==> xx01 <==
toothpaste
tea
soap
tea 
```

当使用 `%regexp%` 时，匹配行之前的行不会是输出的一部分。只有匹配给定正则表达式的行及其余内容将是单个输出文件的一部分。

```sh
$ csplit -q purchases.txt '%t.*p%'

$ cat xx00
toothpaste
tea
soap
tea 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 对于更多示例、自定义选项和其他细节，请参阅我的 [CLI text processing with GNU Coreutils](https://github.com/learnbyexample/cli_text_processing_coreutils) 电子书中的 [csplit 章节](https://learnbyexample.github.io/cli_text_processing_coreutils/csplit.html)。

## xargs

默认情况下，`xargs` 会执行从 `stdin` 数据（或通过 `-a` 选项的文件输入）提取的 `echo` 命令。`-n` 选项有助于自定义每次应该传递多少个参数。结合这些功能，可以像以下示例中所示的那样重新塑造由空格分隔的数据：

```sh
$ printf '  apple   banana cherry\n\t\tdragon unicorn   \n'
  apple   banana cherry
                dragon unicorn   
$ printf '  apple   banana cherry\n\t\tdragon unicorn   \n' | xargs -n2
apple banana
cherry dragon
unicorn

$ cat ip.txt
deep blue
light orange
blue delight
$ xargs -a ip.txt -n3
deep blue light
orange blue delight 
```

您可以使用 `-L` 选项来指定每次应组合多少输入行：

```sh
# same as: pr -3ats' ' or paste -d' ' - - -
$ seq 9 | xargs -L3
1 2 3
4 5 6
7 8 9

$ xargs -a ip.txt -L2
deep blue light orange
blue delight

# you can also use -l instead of -L1
$ printf '  apple   banana cherry\n\t\tdragon unicorn   \n' | xargs -L1
apple banana cherry
dragon unicorn 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 注意，`xargs -L1` 与 `awk '{$1=$1} 1'` 不相同，因为 `xargs` 会忽略空白行。此外，尾随的空白字符会导致下一行被视为当前行的一部分。例如：
> 
> ```sh
> # no trailing blanks
> $ printf 'xerox apple\nregex   go  sea\n' | xargs -L1
> xerox apple
> regex go sea
> 
> # with trailing blanks
> $ printf 'xerox apple  \nregex   go  sea\n' | xargs -L1
> xerox apple regex go sea 
> ```

您可以使用 `-d` 选项来指定一个自定义的单字符输入分隔符。例如：

```sh
$ printf '1,2,3,4,5,6' | xargs -d, -n3
1 2 3
4 5 6 
```

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录作为以下练习中使用的输入文件。

**1)** 生成以下序列。

```sh
# ???
100
95
90
85
80 
```

**2)** 以下序列是否可以用 `seq` 生成？如果是，如何生成？

```sh
# ???
01.5,02.5,03.5,04.5,05.5 
```

**3)** 显示 `/usr/share/dict/words`（或等效的词典单词文件）中包含 `s`、`e` 和 `t` 的三个随机单词，这些单词的顺序可以是任意的。下面的输出只是一个示例。

```sh
# ???
supplemental
foresight
underestimates 
```

**4)** 简要描述`shuf`命令选项`-i`、`-e`和`-r`的目的。

**5)** 为什么以下命令没有按预期工作？在这种情况下，你可以使用哪些其他工具？

```sh
# not working as expected
$ echo 'apple,banana,cherry,dates' | cut -d, -f3,1,3
apple,cherry

# expected output
# ???
cherry,apple,cherry 
```

**6)** 显示除第二个字段外的格式，如下所示。你能构造两种不同的解决方案吗？

```sh
$ echo 'apple,banana,cherry,dates' | cut # ???
apple cherry dates

$ echo '2,3,4,5,6,7,8' | cut # ???
2 4 5 6 7 8 
```

**7)** 从输入行中提取前三个字符，如下所示。你也能使用`head`命令来完成这个任务吗？如果不能，为什么？

```sh
$ printf 'apple\nbanana\ncherry\ndates\n' | cut # ???
app
ban
che
dat 
```

**8)** 仅显示`scores.csv`输入文件的前两列和第三列，格式如下所示。注意，两列之间只有空格字符，没有制表符。

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

# ???
Name  Physics
Ith   100
Cy    98
Lin   83 
```

**9)** 以以下格式显示`table.txt`的内容。

```sh
# ???
brown   bread   mat     hair   42
blue    cake    mug     shirt  -7
yellow  banana  window  shoes  3.14 
```

**10)** 使用`tr`命令实现[ROT13](https://en.wikipedia.org/wiki/ROT13)密码。

```sh
$ echo 'Hello World' | tr # ???
Uryyb Jbeyq

$ echo 'Uryyb Jbeyq' | tr # ???
Hello World 
```

**11)** 仅保留字母、数字和空白字符。

```sh
$ echo 'Apple_42 cool,blue Dragon:army' | # ???
Apple42 coolblue Dragonarmy 
```

**12)** 使用`tr`命令获取以下输出。

```sh
$ echo '!!hhoowwww !!aaaaaareeeeee!! yyouuuu!!' | tr # ???
how are you 
```

**13)** `paste -s`对多个输入文件分别工作。如果你需要将所有输入文件视为单个源，你会如何解决这个问题？

```sh
# this works individually for each input file
$ paste -sd, fruits.txt ip.txt
banana,papaya,mango
deep blue,light orange,blue delight

# expected output
# ???
banana,papaya,mango,deep blue,light orange,blue delight 
```

**14)** 使用适当的选项获取以下预期的输出。

```sh
# default output
$ paste fruits.txt ip.txt
banana  deep blue
papaya  light orange
mango   blue delight

# expected output
$ paste # ???
banana
deep blue
papaya
light orange
mango
blue delight 
```

**15)** 使用`pr`命令获取以下预期的输出。

```sh
$ seq -w 16 | pr # ???
01,02,03,04
05,06,07,08
09,10,11,12
13,14,15,16

$ seq -w 16 | pr # ???
01,05,09,13
02,06,10,14
03,07,11,15
04,08,12,16 
```

**16)** 使用`pr`命令将输入文件`fruits.txt`和`ip.txt`连接起来，如下所示。

```sh
# ???
banana : deep blue
papaya : light orange
mango : blue delight 
```

**17)** `cut`命令不支持选择最后`N`个字段的方法。本章中介绍的哪个工具可以与`cut`结合使用以获取以下输出？

```sh
# last two characters from each line
$ printf 'apple\nbanana\ncherry\ndates\n' | # ???
le
na
ry
es 
```

**18)** 阅读`split`文档，并使用适当的选项获取以下输出，对于输入文件`purchases.txt`。

```sh
# split input by 3 lines (max) at a time
# ???

$ head xa?
==> xaa <==
coffee
tea
washing powder

==> xab <==
coffee
toothpaste
tea

==> xac <==
soap
tea

$ rm xa? 
```

**19)** 阅读文档`split`，并使用适当的选项获取以下输出。

```sh
$ echo 'apple,banana,cherry,dates' | split # ???

$ head xa?
==> xaa <==
apple,
==> xab <==
banana,
==> xac <==
cherry,
==> xad <==
dates

$ rm xa? 
```

**20)** 将输入文件`purchases.txt`分割，使得包含`powder`行的文本是第一个文件的一部分，其余的是第二个文件的一部分，如下所示。

```sh
# ???

$ head xx0?
==> xx00 <==
coffee
tea

==> xx01 <==
washing powder
coffee
toothpaste
tea
soap
tea

$ rm xx0? 
```

**21)** 编写一个通用的解决方案，用于转换逗号分隔的数据。以下是一个示例输入/输出。你可以使用本书中介绍的任何工具。

```sh
$ cat scores.csv
Name,Maths,Physics,Chemistry
Ith,100,100,100
Cy,97,98,95
Lin,78,83,80

# ???
Name,Ith,Cy,Lin
Maths,100,97,78
Physics,100,98,83
Chemistry,100,95,80 
```

**22)** 将`table.txt`的内容重塑为以下预期的输出。

```sh
$ cat table.txt
brown bread mat hair 42
blue cake mug shirt -7
yellow banana window shoes 3.14

# ???
brown   bread  mat     hair
42      blue   cake    mug
shirt   -7     yellow  banana
window  shoes  3.14 
```
