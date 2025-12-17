# 排序内容

> 原文：[`learnbyexample.github.io/cli-computing/sorting-stuff.html`](https://learnbyexample.github.io/cli-computing/sorting-stuff.html)

在本章中，你将学习如何根据各种标准对输入进行排序。然后，你将了解通常需要排序输入来执行查找唯一条目、逐行比较两个文件等操作的工具。

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章中使用的示例输入文件。

## sort

如其名所示，此命令用于对输入文件的内容进行排序。字母排序和数字排序？可能。按特定列排序？可能。优先排序多个排序顺序？可能。随机化？唯一？此强大命令支持许多功能。

### 常用选项

下面显示了常用选项。示例将在后面的章节中讨论。

+   `-n` 数字排序

+   `-g` 通用数字排序

+   `-V` 版本排序（了解文本中的数字）

+   `-h` 排序人类可读的数字（例如：4K、3M、12G 等）

+   `-k` 通过键（列排序）

+   `-t` 单字节字符作为字段分隔符（默认是非空白到空白的转换）

+   `-u` 唯一排序

+   `-R` 随机排序

+   `-r` 反转排序输出

+   `-o` 将排序结果重定向到指定的文件名（例如：用于原地排序）

### 默认排序

默认情况下，`sort` 按字典顺序以升序对输入进行排序。你可以使用 `-r` 选项来反转结果。

```sh
# default sort
$ printf 'banana\ncherry\napple' | sort
apple
banana
cherry

# sort and then display the results in reversed order
$ printf 'peace\nrest\nquiet' | sort -r
rest
quiet
peace 
```

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 如果你想忽略大小写，请使用 `-f` 选项。另请参阅[coreutils FAQ：排序不按正常顺序排序！](https://www.gnu.org/software/coreutils/faq/#Sort-does-not-sort-in-normal-order_0021)。

### 数字排序

处理包含不同类型数字的输入有几种方法：

```sh
$ printf '20\n2\n-3\n111\n3.14' | sort -n
-3
2
3.14
20
111

# sorting human readable numbers
$ sort -hr file_size.txt
1.4G    games
316M    projects
746K    report.log
104K    power.log
20K     sample.txt

# version sort
$ sort -V timings.txt
3m20.058s
3m42.833s
4m3.083s
4m11.130s
5m35.363s 
```

### 唯一排序

`-u` 选项将仅保留被认为是相等的行的第一个副本。

```sh
# -f option ignores case differences
$ printf 'CAT\nbat\ncat\ncar\nbat\n' | sort -fu
bat
car
CAT 
```

### 列排序

`-k` 选项允许你根据特定列而不是整个输入行进行排序。默认情况下，非空白字符和空白字符之间的空字符串被视为分隔符。此选项接受各种形式的参数。你可以指定由逗号分隔的起始和结束列号。如果你只指定起始列，则最后一个列将用作结束列。通常你只想按单个列排序，在这种情况下，起始和结束列都指定为相同的数字。以下是一个示例：

```sh
$ cat shopping.txt
apple   50
toys    5
Pizza   2
mango   25
Banana  10

# sort based on the 2nd column numbers
$ sort -k2,2n shopping.txt
Pizza   2
toys    5
Banana  10
mango   25
apple   50 
```

> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 你可以使用 `-t` 选项指定单个字节字符作为字段分隔符。使用 `\0` 指定 ASCII NUL 作为分隔符。
> 
> ![信息](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 `-s` 选项在认为两行或多行相等时保留输入行的原始顺序。您仍然可以使用多个键来指定自己的解决冲突的方法，`-s` 只能防止最后的比较手段。

## uniq

此命令帮助您识别和删除重复项。通常与排序输入一起使用，因为比较仅限于相邻行。

### 常见选项

常用选项如下。示例将在后面的章节中讨论。

+   `-u` 仅显示唯一条目

+   `-d` 仅显示重复条目

+   `-D` 显示所有重复条目的副本

+   `-c` 前缀计数

+   `-i` 在确定重复项时忽略大小写

+   `-f` 跳过前`N`个字段（分隔符是空格/制表符字符）

+   `-s` 跳过前`N`个字符

+   `-w` 限制比较到前`N`个字符

### 默认唯一值

默认情况下，`uniq` 只保留重复行的单个副本：

```sh
# same as sort -u for this case
$ printf 'brown\nbrown\nbrown\ngreen\nbrown\nblue\nblue' | sort | uniq
blue
brown
green

# can't use sort -n -u here
$ printf '2 balls\n13 pens\n2 pins\n13 pens\n' | sort -n | uniq
2 balls
2 pins
13 pens 
```

### 唯一和重复条目

`-u` 选项将仅显示唯一条目。也就是说，只有当一行不出现多次时。

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

$ sort purchases.txt | uniq -u
soap
toothpaste
washing powder 
```

`-d` 选项将仅显示重复条目。也就是说，只有当一行被看到多次时。要显示所有重复条目的副本，请使用 `-D` 选项。

```sh
$ sort purchases.txt | uniq -d
coffee
tea

$ sort purchases.txt | uniq -D
coffee
coffee
tea
tea
tea 
```

### 前缀计数

如果您想知道一行重复了多少次，请使用 `-c` 选项。这将作为前缀添加。

```sh
$ sort purchases.txt | uniq -c
      2 coffee
      1 soap
      3 tea
      1 toothpaste
      1 washing powder

$ sort purchases.txt | uniq -dc
      2 coffee
      3 tea

# sorting by number of occurrences
$ sort purchases.txt | uniq -c | sort -nr
      3 tea
      2 coffee
      1 washing powder
      1 toothpaste
      1 soap 
```

### 部分匹配

`uniq` 有三个选项可以更改匹配标准为输入行的部分。这些选项不如 `sort -k` 选项强大，但在某些用例中很有用。

```sh
# compare only the first 2 characters
$ printf '1) apple\n1) almond\n2) banana\n3) cherry\n3) cup' | uniq -w2
1) apple
2) banana
3) cherry

# -f1 skips the first field
# -s2 then skips two characters (including the blank character)
# -w2 uses the next two characters for comparison ('bl' and 'ch' in this example)
$ printf '2 @blue\n10 :black\n5 :cherry\n3 @chalk' | uniq -f1 -s2 -w2
2 @blue
5 :cherry 
```

## comm

`comm` 命令在两个排序文件之间查找公共和唯一的行。默认情况下，您将得到一个包含三列的表格输出：

+   第一列包含第一个文件中唯一的行

+   第二列包含第二个文件中唯一的行

+   第三列包含两个文件都有的行

```sh
# side by side view of already sorted sample files
$ paste c1.txt c2.txt
Blue    Black
Brown   Blue
Orange  Green
Purple  Orange
Red     Pink
Teal    Red
White   White

# default three column output
$ comm c1.txt c2.txt
        Black
                Blue
Brown
        Green
                Orange
        Pink
Purple
                Red
Teal
                White 
```

您可以使用以下选项之一来抑制列：

+   `-1` 抑制第一个文件中唯一的行

+   `-2` 抑制第二个文件中唯一的行

+   `-3` 抑制两个文件都有的行

```sh
# only the common lines
$ comm -12 c1.txt c2.txt
Blue
Orange
Red
White

# lines unique to the second file
$ comm -13 c1.txt c2.txt
Black
Green
Pink 
```

## join

默认情况下，`join` 命令根据第一个字段的内容（也称为**键**）组合两个文件。只有具有公共键的行才会成为输出的一部分。

关键字段将首先在输出中显示（如果第一个字段不是键，这种区别将发挥作用）。其余行将按顺序显示来自第一个和第二个文件的剩余字段。一个或多个空白（空格或制表符）将被视为输入字段分隔符，单个空格将用作输出字段分隔符。如果存在，输入行开头的空白字符将被忽略。

```sh
# sample sorted input files
$ cat shopping_jan.txt
apple   10
banana  20
soap    3
tshirt  3
$ cat shopping_feb.txt
banana  15
fig     100
pen     2
soap    1

# combine common lines based on the first field
$ join shopping_jan.txt shopping_feb.txt
banana 20 15
soap 3 1 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 注意，用于 `join` 的排序顺序应与用于 `sort` 输入文件的顺序相同。使用 `join -i` 忽略大小写，类似于 `sort -f` 的用法。

如果同一输入文件中存在多个字段值，则所有可能的组合都将出现在输出中。如下所示，`join` 还会确保即使输入中没有，也会添加一个最后的换行符。

```sh
$ join <(printf 'a f1_x\na f1_y') <(printf 'a f2_x\na f2_y')
a f1_x f2_x
a f1_x f2_y
a f1_y f2_x
a f1_y f2_y 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 还有许多其他功能，例如指定字段分隔符、从每个输入文件中选择特定顺序的字段、为不匹配的行填充字段等。请参阅 [join 章节](https://learnbyexample.github.io/cli_text_processing_coreutils/join.html) 以及我的 [使用 GNU Coreutils 的 CLI 文本处理](https://github.com/learnbyexample/cli_text_processing_coreutils) 电子书中的解释和示例。

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用 [example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files) 目录中的输入文件进行以下练习。

**1)** 默认的 `sort` 不适用于数字。更正以下命令：

```sh
# wrong output
$ printf '100\n10\n20\n3000\n2.45\n' | sort
10
100
20
2.45
3000

# expected output
$ printf '100\n10\n20\n3000\n2.45\n' | sort # ???
2.45
10
20
100
3000 
```

**2)** 哪个 `sort` 选项可以帮助你忽略大小写？

```sh
$ printf 'Super\nover\nRUNE\ntea\n' | LC_ALL=C sort # ???
over
RUNE
Super
tea 
```

**3)** 阅读排序手册，并使用适当的选项以获得以下所示输出。

```sh
# wrong output
$ printf '+120\n-1.53\n3.14e+4\n42.1e-2' | sort -n
-1.53
+120
3.14e+4
42.1e-2

# expected output
$ printf '+120\n-1.53\n3.14e+4\n42.1e-2' | sort # ???
-1.53
42.1e-2
+120
3.14e+4 
```

**4)** 使用第二字段的内容按数值升序排序 `scores.csv` 文件。应保留标题行作为第一行，如下所示。*提示*：参见 Shell 特性 章节。

```sh
# ???
Name,Maths,Physics,Chemistry
Lin,78,83,80
Cy,97,98,95
Ith,100,100,100 
```

**5)** 按第四列数字降序排序 `duplicates.txt` 的内容。仅保留具有相同数字的第一行副本。

```sh
# ???
dark red,sky,rose,555
blue,ruby,water,333
dark red,ruby,rose,111
brown,toy,bread,42 
```

**6)** 如果输入未排序，`uniq` 会抛出错误吗？你认为以下输入的输出会是什么？

```sh
$ printf 'red\nred\nred\ngreen\nred\nblue\nblue' | uniq
# ??? 
```

**7)** 仅保留基于输入行前两个字符的唯一条目。如有必要，对输入进行排序。

```sh
$ printf '3) cherry\n1) apple\n2) banana\n1) almond\n'
3) cherry
1) apple
2) banana
1) almond

$ printf '3) cherry\n1) apple\n2) banana\n1) almond\n' | # ???
2) banana
3) cherry 
```

**8)** 计算输入行重复的次数，并按以下格式显示结果。

```sh
$ printf 'brown\nbrown\nbrown\ngreen\nbrown\nblue\nblue' | # ???
      1 green
      2 blue
      4 brown 
```

**9)** 使用 `comm` 命令显示 `c1.txt` 中存在但不在 `c2.txt` 中的行。假设输入文件已经排序。

```sh
# ???
Brown
Purple
Teal 
```

**10)** 使用适当的选项以获得以下所示预期输出。

```sh
# wrong usage, no output
$ join <(printf 'apple 2\nfig 5') <(printf 'Fig 10\nmango 4')

# expected output
# ???
fig 5 10 
```

**11)** 如果有的话，`sort -u` 和 `uniq -u` 选项之间有什么区别？
