# `awk`命令

Awk 是一种通用脚本语言，专为高级文本处理设计。它主要用作报告和分析工具。

#### 我们可以用 AWK 做什么？

1.  AWK 操作：(a) 逐行扫描文件 (b) 将每行输入分割成字段 (c) 将输入行/字段与模式比较 (d) 对匹配的行执行动作

1.  适用于：(a) 转换数据文件 (b) 生成格式化报告

1.  编程结构：(a) 格式化输出行 (b) 算术和字符串操作 (c) 条件和循环

#### 语法

```sh
      awk options 'selection _criteria {action }' input-file > output-file 

```

#### 示例

考虑以下文本文件作为以下示例的输入文件：

```sh
      ``
        `
$cat > employee.txt
`
        ``
        ``
        `
ajay manager account 45000
sunil clerk account 25000
varun manager sales 50000
amit manager account 47000
tarun peon sales 15000
`
        `` 

```

1.  Awk 的默认行为：默认情况下，Awk 打印出指定文件中的每一行数据。

```sh
      $ awk '{print}' employee.txt 

```

```sh
      ajay manager account 45000
sunil clerk account 25000
varun manager sales 50000
amit manager account 47000
tarun peon sales 15000 

```

在上述示例中，没有给出模式。因此，这些操作适用于所有行。没有参数的动作默认打印整行，因此它默认打印出文件的所有行而不会失败。

1.  打印出与给定模式匹配的行。

```sh
      awk '/manager/ {print}' employee.txt 

```

```sh
      ajay manager account 45000
varun manager sales 50000
amit manager account 47000 

```

在上述示例中，awk 命令打印出所有与“manager”匹配的行。

1.  将行分割成字段：对于每个记录（即行），awk 命令默认通过空白字符分隔记录，并将其存储在$n 变量中。如果行有 4 个单词，它将分别存储在$1, $2, $3 和$4 中。此外，$0 代表整行。

```sh
      $ awk '{print $1,$4}' employee.txt 

```

```sh
      ajay 45000
sunil 25000
varun 50000
amit 47000
tarun 15000 

```

#### Awk 的内置变量

Awk 的内置变量包括字段变量—$1, $2, $3，等等（$0 是整行）—它们将文本行分割成单独的单词或称为字段的片段。

NR：NR 命令保持当前输入记录数的计数。记住，记录通常是行。awk 命令对文件中的每一行记录执行一次模式/动作语句。NF：NF 命令保持当前输入记录中字段数的计数。FS：FS 命令包含字段分隔符字符，用于在输入行上分割字段。默认是“空白”，意味着空格和制表符字符。FS 可以被重新分配到另一个字符（通常在 BEGIN 中）以更改字段分隔符。RS：RS 命令存储当前记录分隔符字符。由于默认情况下，输入行是输入记录，默认记录分隔符字符是换行符。OFS：OFS 命令存储输出字段分隔符，当 Awk 打印时用于分隔字段。默认是一个空格。每当 print 有几个用逗号分隔的参数时，它将在每个参数之间打印 OFS 的值。ORS：ORS 命令存储输出记录分隔符，当 Awk 打印时用于分隔输出行。默认是换行符字符。print 自动在打印内容的末尾输出 ORS 的内容。
