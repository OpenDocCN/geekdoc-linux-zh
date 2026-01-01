# `shuf` 命令

Linux 中的 `shuf` 命令将输入行的随机排列写入标准输出。它以与洗牌相同的方式伪随机化输入。它是 GNU Coreutils 的一部分，不是 POSIX 的一部分。此命令从文件或 bash 的标准输入读取，随机化这些输入行并显示输出。

## 语法

```sh
      # file shuf
shuf [OPTION] [FILE]

# list shuf
shuf -e [OPTION]... [ARG]

# range shuf
shuf -i LO-HI [OPTION] 

```

与其他 Linux 命令一样，`shuf` 命令带有 `-–help` 选项：

```sh
      [user@home ~]$ shuf --help
Usage: shuf [OPTION]... [FILE]
  or:  shuf -e [OPTION]... [ARG]...
  or:  shuf -i LO-HI [OPTION]...
Write a random permutation of the input lines to standard output.

With no FILE, or when FILE is -, read standard input.

Mandatory arguments to long options are mandatory for short options too.
  -e, --echo                treat each ARG as an input line
  -i, --input-range=LO-HI   treat each number LO through HI as an input line
  -n, --head-count=COUNT    output at most COUNT lines
  -o, --output=FILE         write result to FILE instead of standard output
      --random-source=FILE  get random bytes from FILE
  -r, --repeat              output lines can be repeated
  -z, --zero-terminated     line delimiter is NUL, not newline 

```

## 示例：

### 无选项或参数的 shuf 命令。

```sh
      shuf 

```

当 `shuf` 命令在命令行中没有任何参数时，它会从用户那里获取输入，直到输入 `CTRL-D` 以终止输入集。它以打乱的形式显示输入行。如果输入行是 `1, 2, 3, 4 和 5`，那么它将在输出中以随机顺序生成 `1, 2, 3, 4 和 5`，如下面的插图所示：

```sh
      [user@home ~]$ shuf
1
2
3
4
5

4
5
1
2
3 

```

考虑一个从管道获取输入的例子：

```sh
      {
seq 5 | shuf
} 

```

`seq 5` 返回从 `1` 到 `5` 的连续整数，而 `shuf` 命令将其作为输入并打乱内容，即从 `1` 到 `5` 的整数。因此，以随机顺序显示 `1` 到 `5` 作为输出。

```sh
      [user@home ~]$ {
> seq 5 | shuf
> }
5
4
2
3
1 

```

### 文件 shuf

当 `shuf` 命令使用时没有 `-e` 或 `-i` 选项，那么它作为文件 shuf 运行，即它打乱文件的内容。`<file_name>` 是 `shuf` 命令的最后一个参数，如果没有提供，则必须从 shell 或管道提供输入。

考虑一个从文件获取输入的例子：

```sh
      shuf file.txt 

```

假设 file.txt 包含 6 行，那么 shuf 命令会以随机顺序显示输入行作为输出。

```sh
      [user@home ~]$ cat file.txt
line-1
line-2
line-3
line-4
line-5

[user@home ~]$ shuf file.txt
line-5
line-4
line-1
line-3
line-2 

```

可以使用 `-n` 选项随机化任意数量的行。

```sh
      shuf -n 2 file.txt 

```

这将显示文件中的任意两行。

```sh
      line-5
line-2 

```

### 列出 shuf 命令

当使用 `-e` 选项与 shuf 命令一起时，它作为列表 shuf 运行。命令的参数被视为 shuf 的输入行。

考虑一个例子：

```sh
      shuf -e A B C D E 

```

它将 `A, B, C, D, E` 作为输入行，并将它们打乱以显示输出。

```sh
      A
C
B
D
E 

```

可以使用 `-n` 选项与 `-e` 选项一起显示任意数量的输入行。

```sh
      shuf -e -n 2 A B C D E 

```

这将显示任意两个输入。

```sh
      E
A 

```

### 范围 shuf

当与 `shuf` 命令一起使用 `-i` 选项时，它作为 `range shuf` 运行。它需要一个输入范围作为输入，其中 `L0` 是下限，而 `HI` 是上限。它以打乱的形式显示从 `L0-HI` 的整数。

```sh
      [user@home ~]$ shuf -i 1-5
4
1
3
2
5 

```

## 结论

`shuf` 命令可以帮助您随机化输入行。并且有功能来限制输出行的数量、重复行甚至生成随机正整数。一旦您完成了在这里讨论的练习，请转到工具的 [man 页面](https://linux.die.net/man/1/shuf) 以了解更多信息。
