# 文件属性

> 原文：[`learnbyexample.github.io/cli-computing/file-properties.html`](https://learnbyexample.github.io/cli-computing/file-properties.html)

在本章中，您将学习如何查看文件详情，如行数和单词数、文件和磁盘大小、文件类型、提取文件路径的一部分等。您还将学习如何更改文件属性，如时间戳和权限。

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) [example_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files) 目录包含本章中使用的脚本和示例输入文件。

## wc

`wc` 命令通常用于计算给定输入的行数、单词数和字符数。以下是一些基本示例：

```sh
# change to the 'example_files/text_files' directory
$ cat greeting.txt
Hi there
Have a nice day

# by default, wc gives the newline/word/byte count (in that order)
$ wc greeting.txt
 2  6 25 greeting.txt

# get only the specified counts
$ wc -l greeting.txt
2 greeting.txt
$ wc -w greeting.txt
6 greeting.txt
$ wc -c greeting.txt
25 greeting.txt
$ wc -wc greeting.txt
 6 25 greeting.txt 
```

对于 stdin 数据，不会打印文件名。这对于将结果保存到变量中以供脚本使用很有帮助。

```sh
$ wc -l &LTgreeting.txt
2 
```

单词计数基于空白字符分隔。您可以在预处理输入时防止某些非空白字符影响结果。`tr` 命令可以用来删除特定的字符集（此命令将在杂项文本处理工具章节中讨论）。

```sh
$ echo 'apple ; banana ; cherry' | wc -w
5

# remove characters other than alphabets and whitespace
# -d option is for deleting, -c option complements the given set
$ echo 'apple ; banana ; cherry' | tr -cd 'a-zA-Z[:space:]'
apple  banana  cherry
$ echo 'apple ; banana ; cherry' | tr -cd 'a-zA-Z[:space:]' | wc -w
3 
```

如果您向 `wc` 命令传递多个文件，则每个文件的计数值将分别显示。您还会在最后得到一个总结，它汇总了所有输入文件的相应计数。

```sh
$ wc greeting.txt fruits.txt sample.txt
  2   6  25 greeting.txt
  3   3  20 fruits.txt
 15  38 183 sample.txt
 20  47 228 total 
```

您可以使用 `-L` 选项来报告输入中最长行的长度（不包括行的换行符）。请注意，`-L` 不会计算不可打印字符，制表符会被转换为等价的空格。多字节字符和[图形群组](https://unicode.org/reports/tr29/#Grapheme_Cluster_Boundaries)将被各自计为 `1`（根据区域设置，它们也可能成为不可打印的）。

```sh
$ echo 'apple' | wc -L
5

$ echo 'αλεπού cag̈e' | wc -L
11

$ wc -L &LTgreeting.txt
15 
```

如果输入包含多字节字符，请使用 `-m` 选项而不是 `-c`。

```sh
$ printf 'αλεπού' | wc -c
12

$ printf 'αλεπού' | wc -m
6 
```

## du

`du` 命令帮助您估计文件和目录的大小。

默认情况下，大小以 1024 字节为单位给出。所有目录和子目录都会递归报告，但文件会被忽略。如果您希望也报告文件，可以使用 `-a` 选项。`du` 是需要显式选项（在这种情况下为 `-L`）的命令之一，如果您想跟随符号链接。

```sh
# change to the 'scripts' directory and source the 'du.sh' script
$ source du.sh

# n * 1024 bytes
$ du
28      ./projects/scripts
48      ./projects
8       ./todos
7536    . 
```

使用 `-s` 选项来显示总目录大小，而不进入子目录。添加 `-c` 选项也可以在最后显示总大小。

```sh
$ du -s projects report.log
48      projects
7476    report.log

$ du -sc projects report.log
48      projects
7476    report.log
7524    total 
```

以下是一些示例，说明大小格式化选项：

```sh
# number of bytes
$ du -b report.log
7654321 report.log

# n * 1024 bytes
$ du -k report.log
7476    report.log

# n * 1024 * 1024 bytes
$ du -m report.log
8       report.log 
```

`-h` 选项以人类可读的格式报告大小（使用 1024 的幂）。使用 `--si` 选项以 1000 的幂获取结果。如果您使用 `du -h`，可以将输出通过 `sort -h` 进行排序以进行排序。

```sh
$ du -sh *
48K     projects
7.4M    report.log
8.0K    todos

$ du -sh * | sort -h
8.0K    todos
48K     projects
7.4M    report.log 
```

## df

`df` 命令为您提供文件系统的空间使用情况。没有路径参数的 `df` 将提供有关所有当前挂载的文件系统的信息。

```sh
$ df .
Filesystem     1K-blocks     Used Available Use% Mounted on
/dev/sda1       98298500 58563816  34734748  63% / 
```

使用`-h`选项以人类可读的尺寸显示。`-B`选项允许您按指定量缩放尺寸。使用`--si`以 1000 的幂次表示尺寸，而不是 1024。

```sh
$ df -h .
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        94G   56G   34G  63% / 
```

使用`--output`选项仅报告感兴趣的特定字段：

```sh
$ df -h --output=size,used,file / /media/learnbyexample/projs
 Size  Used File
  94G   56G /
  92G   35G /media/learnbyexample/projs

# 'awk' here excludes first line and matches lines with first field >= 30
$ df -h --output=pcent,fstype,target | awk 'NR>1 && $1>=30'
 63% ext3     /
 38% ext4     /media/learnbyexample/projs
 51% ext4     /media/learnbyexample/backups 
```

## stat

`stat`命令非常有用，可以获取诸如文件类型、大小、inode、权限、最后访问和修改时间戳等详细信息。默认情况下，您将获得所有这些详细信息。可以使用`-c`和`--printf`选项仅以特定格式显示所需详细信息。

```sh
# change to the 'scripts' directory and source the 'stat.sh' script
$ source stat.sh

# %x gives the last accessed timestamp
$ stat -c '%x' ip.txt
2022-06-01 13:25:18.693823117 +0530

# %y gives the last modified timestamp
$ stat -c '%y' ip.txt
2022-05-24 14:39:41.285714934 +0530

# %s gives the file size in bytes
# \n is used to insert a newline
# %i gives the inode value
# same as: stat --printf='%s\n%i\n' ip.txt
$ stat -c $'%s\n%i' ip.txt
10
787224

# %N gives quoted filenames
# if the input is a link, the path it points to is also displayed
$ stat -c '%N' words.txt
'words.txt' -> '/usr/share/dict/words' 
```

您还可以传递多个文件参数：

```sh
# %s gives the file size in bytes
# %n gives filenames
$ stat -c '%s %n' ip.txt hi.sh
10 ip.txt
21 hi.sh 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) ![warning](img/b5314b4a0acf0f436c4bf59486793342.png) 应优先使用`stat`命令，而不是解析`ls -l`输出的文件详细信息。请参阅[mywiki.wooledge: 避免解析 ls 输出](https://mywiki.wooledge.org/ParsingLs)和[unix.stackexchange: 为什么不解析 ls?](https://unix.stackexchange.com/q/128985/109046)以获取解释和其他替代方案。

## touch

如前所述，`touch`命令可以帮助您更改文件的时间戳。您可以根据当前时间戳、传递参数、从另一个文件复制值等方式进行操作。

默认情况下，`touch`更新访问和修改时间戳到当前时间。您可以使用`-a`选项仅更改访问时间戳，使用`-m`仅更改修改时间戳。

```sh
# change to the 'scripts' directory and source the 'touch.sh' script
$ source touch.sh

# last access and modification timestamps
$ stat -c $'%x\n%y' fruits.txt
2017-07-19 17:06:01.523308599 +0530
2017-07-13 13:54:03.576055933 +0530

# update the access and modification values to the current time
$ touch fruits.txt
$ stat -c $'%x\n%y' fruits.txt
2024-05-14 13:01:25.921205889 +0530
2024-05-14 13:01:25.921205889 +0530 
```

您可以使用`-r`选项将时间戳信息从一个文件复制到另一个文件。`-d`和`-t`选项将允许您直接在命令中指定时间戳。

```sh
$ stat -c '%y' hi.sh
2022-06-14 13:00:46.170416890 +0530

# copy the modified timestamp from 'ip.txt' to 'hi.sh'
$ touch -m -r ip.txt hi.sh
$ stat -c '%y' hi.sh
2022-05-24 14:39:41.285714934 +0530

# pass timestamp as an argument
$ touch -m -d '2000-01-01 00:00:01' hi.sh
$ stat -c '%y' hi.sh
2000-01-01 00:00:01.000000000 +0530 
```

如管理文件和目录章节中所述，`touch`命令在目标文件不存在时创建新文件。您可以使用`-c`选项防止此行为。

```sh
$ ls report.txt
ls: cannot access 'report.txt': No such file or directory
$ touch report.txt
$ ls report.txt
report.txt

$ touch -c xyz.txt
$ ls xyz.txt
ls: cannot access 'xyz.txt': No such file or directory 
```

## file

`file`命令可以帮助您识别文本编码（ASCII、UTF-8 等），文件是否可执行等。

下面是一些示例，说明`file`命令对不同类型的处理方式：

```sh
# change to the 'scripts' directory and source the 'file.sh' script
$ source file.sh
$ ls -F
hi.sh*  ip.txt  moon.png  sunrise.jpg

$ file ip.txt hi.sh
ip.txt: ASCII text
hi.sh: Bourne-Again shell script, ASCII text executable

$ printf 'αλεπού\n' | file -
/dev/stdin: UTF-8 Unicode text

$ printf 'hi\r\n' | file -
/dev/stdin: ASCII text, with CRLF line terminators 
```

下面是一个关于图像文件的示例：

```sh
# output of 'sunrise.jpg' wrapped for illustration purposes
$ file sunrise.jpg moon.png
sunrise.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density
    96x96, segment length 16, baseline, precision 8, 76x76, components 3
moon.png:    PNG image data, 76 x 76, 8-bit colormap, non-interlaced 
```

您可以使用`-b`选项避免在输出中包含文件名：

```sh
$ file -b ip.txt
ASCII text 
```

下面是如何查找特定类型的文件，例如图像文件的方法：

```sh
# assuming filenames do not contain ':' or newline characters
# awk here helps to print the first field of lines containing 'image data'
$ find -type f -exec file {} + | awk -F: '/\&LTimage data\>/{print $1}'
./sunset.jpg
./moon.png 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 还可参阅`identify`命令，该命令“描述一个或多个图像文件的格式和特性”。

## basename

默认情况下，`basename`命令将从给定的路径参数中删除前导目录组件。在确定要提取的部分之前，任何尾随斜杠都将被删除。

```sh
$ basename /home/learnbyexample/example_files/scores.csv
scores.csv

# quote the arguments as needed
$ basename 'path with spaces/report.log'
report.log 
```

您可以使用`-s`选项从文件名中删除后缀。通常用于删除文件扩展名。

```sh
$ basename -s'.csv' /home/learnbyexample/example_files/scores.csv
scores

# suffix will be removed only once
$ basename -s'.txt' purchases.txt.txt
purchases.txt 
```

`basename`命令需要`-a`或`-s`（这隐含了`-a`）才能与多个参数一起使用。

```sh
$ basename -a /backups/jan_2021.tar.gz /home/learnbyexample/report.log
jan_2021.tar.gz
report.log

# -a is implied when -s is used
$ basename -s'.txt' logs/purchases.txt logs/report.txt
purchases
report 
```

## dirname

默认情况下，`dirname`命令将删除尾随路径组件（在删除任何尾随斜杠之后）。

```sh
$ dirname /home/learnbyexample/example_files/scores.csv
/home/learnbyexample/example_files

# one or more trailing slashes will not affect the output
$ dirname /home/learnbyexample/example_files/
/home/learnbyexample

# unlike basename, multiple arguments are accepted by default
$ dirname /home/learnbyexample/example_files/scores.csv ../report/backups/
/home/learnbyexample/example_files
../report 
```

您可以使用 shell 功能，如命令替换，来组合 `basename` 和 `dirname` 命令的效果。

```sh
# extract the second last path component
$ basename $(dirname /home/learnbyexample/example_files/scores.csv)
example_files 
```

## chmod

您可以使用 `chmod` 命令来更改权限。考虑以下示例：

```sh
$ mkdir practice_chmod
$ cd practice_chmod
$ echo 'learnbyexample' > ip.txt

# this info can also be seen in the first column of the 'ls -l' output
$ stat -c '%A' ip.txt
-rw-rw-r-- 
```

在上述输出中，最后一行显示的 10 个字符与文件类型和权限相关。第一个字符表示 **文件类型**。下面列出了最常见的类型：

+   `-` 普通文件

+   `d` 目录

+   `l` 符号链接

其他九个字符代表三组 **文件权限**，分别对应于 *用户* (`u`)、*组* (`g`) 和 *其他人* (`o`)，顺序如下。

+   *用户* — 文件所有者

+   *组* — 作为组的一部分具有文件访问权限的用户

+   *其他人* — 除了所有者、组和其他人之外的所有人

本节只讨论 `rwx` 文件属性。对于其他类型的属性，请参阅 [coreutils 手册：文件权限](https://www.gnu.org/software/coreutils/manual/coreutils.html#File-permissions)。

**文件权限参考表：**

| 字符 | 含义 | 值 |
| --- | --- | --- |
| `r` | 读取 | `4` |
| `w` | 写入 | `2` |
| `x` | 执行 | `1` |
| `-` | 无权限 | `0` |

这里有一个示例，显示了文件权限的 `rwx` 和数值表示：

```sh
$ stat -c '%A' ip.txt
-rw-rw-r--

# r(4) + w(2) + 0 = 6
# r(4) + 0 + 0 = 4
$ stat -c '%a' ip.txt
664 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 注意，目录的权限并不容易理解。如果一个目录只有 `x` 权限，您可以使用 `cd` 进入它，但不能读取内容（例如使用 `ls`）。如果一个目录只有 `r` 权限，您不能进入它，但您将能够读取内容（包括“无法访问”错误）。因此，`rx` 权限几乎总是同时启用/禁用。`w` 权限允许您添加或删除内容，前提是 `x` 是激活的。

**为所有三个类别更改权限**

您可以为 `ugo` 提供数字（按此顺序）以更改权限。以下是一些示例，以更好地理解：

```sh
$ printf '#!/bin/bash\n\necho hi\n' > hi.sh
$ stat -c '%a %A' hi.sh
664 -rw-rw-r--

# r(4) + w(2) + x(1) = 7
# r(4) + 0 + x(1) = 5
$ chmod 755 hi.sh
$ stat -c '%a %A' hi.sh
755 -rwxr-xr-x 
```

下面是一个目录的示例：

```sh
$ mkdir dot_files
$ stat -c '%a %A' dot_files
775 drwxrwxr-x

$ chmod 700 dot_files
$ stat -c '%a %A' dot_files
700 drwx------ 
```

您也可以使用 `mkdir -m` 而不是上面看到的 `mkdir+chmod` 组合。`-m` 选项的参数接受与 `chmod` 相同的语法（包括将要讨论的格式）。

```sh
$ mkdir -m 750 backups
$ stat -c '%a %A' backups
750 drwxr-x--- 
```

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 您可以使用 `chmod -R` 递归地更改权限。如果您只想对满足某些条件的文件应用更改，请使用 `find+exec`。

**为特定类别更改权限**

您可以通过使用这些符号后跟一个或多个 `rwx` 权限来分配（`=`）、添加（`+`）或删除（`-`）权限。这取决于 `umask` 的值：

```sh
$ umask
0002 
```

`umask` 值为 `0002` 表示：

+   read and execute permissions without `ugo` prefix affects all three categories

+   write permissions without `ugo` prefix affects only the `user` and `group` categories

下面是一些不带 `ugo` 前缀的例子：

```sh
# remove execute permission for all three categories
$ chmod -x hi.sh

# add write permission only for 'user' and 'group'
$ chmod +w ip.txt

$ touch sample.txt
$ chmod 702 sample.txt
# give only read permission for all three categories
# write/execute permissions, if any, will be removed
$ chmod =r sample.txt
$ stat -c '%a %A' sample.txt
444 -r--r--r--

# give read and write permissions for 'user' and 'group'
# and read permission for 'others'
# execute permissions, if any, will be removed
$ chmod =rw hi.sh 
```

下面是一些带有 `ugo` 前缀的例子。您可以使用 `a` 来指代所有三个类别。例如，`a+w` 与 `ugo+w` 相同。

```sh
# remove read and write permissions only for 'others'
$ chmod o-rw sample.txt

# add execute permission for 'group' and 'others'
$ chmod go+x hi.sh

# give read and write permissions for all three categories
# execute permissions, if any, will be removed
$ chmod a=rw hi.sh 
```

您可以使用`,`分隔多个权限：

```sh
# remove execute permission for 'group' and 'others'
# remove write permission for 'others'
$ chmod go-x,o-w hi.sh 
```

**进一步阅读**

+   [Linux 权限入门](https://web.archive.org/web/20220930214830/https://danielmiessler.com/study/unixlinux_permissions/)

+   [unix.stackexchange: why chmod +w filename not giving write permission to other](https://unix.stackexchange.com/q/429421/109046)

## 练习

> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 使用[example_files/text_files](https://github.com/learnbyexample/cli-computing/tree/master/example_files/text_files)目录中的输入文件，除非另有说明。
> 
> ![info](img/147d5d96e6e103c258c8b5f99e9505a9.png) 为可能需要创建一些文件和目录的练习创建一个临时目录。之后您可以删除这样的练习目录。

**1)** 将`greeting.txt`输入文件的行数保存到`lines`shell 变量中。

```sh
# ???
$ echo "$lines"
2 
```

**2)** 您认为以下命令的输出会是什么？

```sh
$ echo 'dragons:2 ; unicorns:10' | wc -w 
```

**3)** 使用适当的选项和参数来获取以下输出。

```sh
$ printf 'apple\nbanana\ncherry' | wc # ???
     15     183 sample.txt
      2      19 -
     17     202 total 
```

**4)** 阅读`wc`手册，并使用适当的选项和参数来获取以下输出。

```sh
$ printf 'greeting.txt\0scores.csv' | wc # ???
2 6 25 greeting.txt
4 4 70 scores.csv
6 10 95 total 
```

**5)** `wc -c`和`wc -m`选项之间的区别是什么？您会使用哪个选项来获取最长行长度？

**6)** 查找以`.log`结尾的文件名，并以可读格式报告它们的大小。对于第一种情况，使用`find+du`组合，对于第二种情况，使用`ls`命令（带有适当的 shell 功能）。

```sh
# change to the 'scripts' directory and source the 'du.sh' script
$ source du.sh

# ??? find+du
16K     ./projects/errors.log
7.4M    ./report.log

# ??? ls and shell features
 16K projects/errors.log
7.4M report.log 
```

**7)** 在当前路径中报告文件/目录的大小，以`1000`的幂次表示，不进入子目录。最后显示总数。

```sh
# change to the 'scripts' directory and source the 'du.sh' script
$ source du.sh

# ???
50k     projects
7.7M    report.log
8.2k    todos
7.8M    total 
```

**8)** `du --apparent-size`选项的作用是什么？

**9)** 在什么情况下您会使用`df`命令而不是`du`？哪个`df`命令选项将帮助您仅报告感兴趣的特定字段？

**10)** 按以下格式显示`scores.csv`和`timings.txt`文件的大小。

```sh
$ stat # ???
scores.csv: 70
timings.txt: 49 
```

**11)** 哪个`touch`选项可以帮助您防止文件不存在时创建文件？

**12)** 假设`new_file.txt`在当前工作目录中不存在。下面显示的`stat`命令的输出会是什么？

```sh
$ touch -t '202010052010.05' new_file.txt
$ stat -c '%y' new_file.txt
# ??? 
```

**13)** 以下`touch`命令是否有效？如果是，随后的`stat`命令的输出会是什么？

```sh
# change to the 'scripts' directory and source the 'touch.sh' script
$ source touch.sh

$ stat -c '%n: %y' fruits.txt
fruits.txt: 2017-07-13 13:54:03.576055933 +0530

$ touch -r fruits.txt f{1..3}.txt
$ stat -c '%n: %y' f*.txt
# ??? 
```

**14)** 使用适当的选项来获取以下输出。

```sh
$ printf 'αλεπού\n' | file -
/dev/stdin: UTF-8 Unicode text

$ printf 'αλεπού\n' | file # ???
UTF-8 Unicode text 
```

**15)** 以下命令是否有效？如果是，输出会是什么？

```sh
$ basename -s.txt ~///test.txt///
# ??? 
```

**16)** 给定 shell 变量`p`中的文件路径，您如何获得以下输出？

```sh
$ p='~/projects/square_tictactoe/python/game.py'
$ dirname # ???
~/projects/square_tictactoe 
```

**17)** 解释以下`stat`命令输出中的每个字符的含义。

```sh
$ stat -c '%A' ../scripts/
drwxrwxr-x 
```

**18)** 下面的第二个`stat`命令的输出会是什么？

```sh
$ touch new_file.txt
$ stat -c '%a %A' new_file.txt
664 -rw-rw-r--

$ chmod 546 new_file.txt
$ stat -c '%a %A' new_file.txt
# ??? 
```

**19)** 您将如何使用`mkdir`命令指定目录权限？

```sh
# instead of this
$ mkdir back_up
$ chmod 750 back_up
$ stat -c '%a %A' back_up
750 drwxr-x---
$ rm -r back_up

# do this
$ mkdir # ???
$ stat -c '%a %A' back_up
750 drwxr-x--- 
```

**20)** 将`book_list.txt`的文件权限更改为与下面显示的第二个`stat`命令的输出相匹配。不要使用数字`220`，用`rwx`字符指定更改。

```sh
$ touch book_list.txt
$ stat -c '%a %A' book_list.txt
664 -rw-rw-r--

# ???
$ stat -c '%a %A' book_list.txt
220 --w--w---- 
```

**21)** 将`test_dir`的权限更改为与下面显示的第二个`stat`命令的输出相匹配。不要使用数字`757`，而要用`rwx`字符来指定更改。

```sh
$ mkdir test_dir
$ stat -c '%a %A' test_dir
775 drwxrwxr-x

# ???
$ stat -c '%a %A' test_dir
757 drwxr-xrwx 
```
