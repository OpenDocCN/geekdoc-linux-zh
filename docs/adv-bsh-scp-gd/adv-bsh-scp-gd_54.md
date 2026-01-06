# C.1\. Sed

> 原文：[`tldp.org/LDP/abs/html/x23170.html`](https://tldp.org/LDP/abs/html/x23170.html)

*sed*是一个非交互式[[1]](#FTN.AEN23174) **流** **编辑器**。它接收文本输入，无论是来自`stdin`还是来自文件，对输入的指定行执行某些操作，一次一行，然后将结果输出到`stdout`或文件。在 shell 脚本中，sed 通常是管道中的几个工具组件之一。

*sed*确定它将操作其输入的哪些行，从传递给它的地址范围。[[2]](#FTN.AEN23185) 通过行号或匹配模式指定此地址范围。例如，`*3d*`指示 sed 删除输入的第 3 行，而`*/Windows/d*`告诉 sed 您想要删除包含"Windows"匹配的每一行输入。

在 sed 工具包的所有操作中，我们将主要关注三个最常用的操作。这些是打印（到`stdout`）、删除和替换。

**表 C-1\. 基本 sed 操作符**

| 操作符 | 名称 | 效果 |
| --- | --- | --- |
| `[地址范围]/p` | 打印 | 打印指定的地址范围 |
| `[地址范围]/d` | 删除 | 删除指定的地址范围 |
| `s/模式 1/模式 2/` | 替换 | 在行的第一个模式 1 实例中替换为模式 2 |
| `[地址范围]/s/模式 1/模式 2/` | 替换 | 在行的第一个模式 1 实例中替换为模式 2，在`*地址范围*`内进行 |
| `[地址范围]/y/模式 1/模式 2/` | 转换 | 在`*地址范围*`内将模式 1 中的任何字符替换为模式 2 中的对应字符（相当于**tr**） |
| `[地址] i 模式 文件名` | 插入 | 在文件名中指定的地址插入模式。通常与`-i` `*就地*`选项一起使用。 |
| `g` | 全局 | 在每个匹配行的每个匹配模式上操作 |
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 除非将`g`（*全局*）操作符附加到替换命令，否则替换操作仅在每行的模式匹配的第一个实例上操作。 |

在命令行和 shell 脚本中，sed 操作可能需要引号和某些选项。

```sh
sed -e '/^$/d' $filename
# The -e option causes the next string to be interpreted as an editing instruction.
#  (If passing only a single instruction to sed, the "-e" is optional.)
#  The "strong" quotes ('') protect the RE characters in the instruction
#+ from reinterpretation as special characters by the body of the script.
# (This reserves RE expansion of the instruction for sed.)
#
# Operates on the text contained in file $filename.
```

在某些情况下，sed 编辑命令不会与单引号一起使用。

```sh
filename=file1.txt
pattern=BEGIN

  sed "/^$pattern/d" "$filename"  # Works as specified.
# sed '/^$pattern/d' "$filename"    has unexpected results.
#        In this instance, with strong quoting (' ... '),
#+      "$pattern" will not expand to "BEGIN".
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | *sed*使用`-e`选项指定以下字符串是指令或指令集。如果字符串中只包含一个指令，则可以省略。 |
```sh
sed -n '/xzy/p' $filename
# The -n option tells sed to print only those lines matching the pattern.
# Otherwise all input lines would print.
# The -e option not necessary here since there is only a single editing instruction.
```  |

**表 C-2\. sed 操作符示例**

| 符号 | 效果 |
| --- | --- | --- |
| `8d` | 删除输入的第 8 行。 |
| `/^$/d` | 删除所有空白行。 |
| `1,/^$/d` | 删除 | 从输入的开始删除，包括第一行空白行。 |
| `/Jones/p` | 仅打印包含"Jones"（使用-n 选项）的行。 |
| `s/Windows/Linux/` | 将每个输入行中找到的第一个"Windows"替换为"Linux"。 |
| `s/BSOD/stability/g` | 将每行中找到的每个 "BSOD" 替换为 "stability" |
| `s/ *$//` | 删除每行末尾的所有空格 |
| `s/00*/0/g` | 将所有连续的零压缩成一个零 |
| `echo "Working on it." &#124; sed -e '1i How far are you along?'` | 打印 "How far are you along?" 作为第一行，"Working on it" 作为第二行 |
| `5i 'Linux is great.' file.txt` | 在文件 file.txt 的第 5 行插入 'Linux is great.' |
| `/GUI/d` | 删除包含 "GUI" 的所有行 |
| `s/GUI//g` | 删除所有 "GUI" 实例，保留每行的其余部分 |

用零长度字符串替换另一个字符串相当于在输入行的字符串中删除该字符串。这会保留行的其余部分。将 `**s/GUI//**` 应用到该行

```sh
**The most important parts of any application are its GUI and sound effects**
```

结果为

```sh
The most important parts of any application are its  and sound effects
```

反斜杠强制 *sed* 替换命令继续到下一行。这相当于使用第一行末尾的 *换行符* 作为 *替换字符串*。

```sh
s/^  */\
/g
```

这个替换将行首的空格替换为换行符。最终结果是替换段落缩进为段落之间的空白行。

地址范围后面跟一个或多个操作可能需要开闭花括号，以及适当的新行。

```sh
/[0-9A-Za-z]/,/^$/{
/^$/d
}
```

这只删除了每组连续空行中的第一行。这可能对单倍空行文本文件有用，但保留段落之间的空行。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | *sed* 通常使用的分隔符是 /. 然而，*sed* 允许使用其他分隔符，例如 %. 当 / 是替换字符串的一部分时，这很有用，例如在文件路径名中。参见 示例 11-10 和 示例 16-32。 |
| --- | --- |
| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 快速将文本文件双倍空行的方法是 `**sed G filename**` |

关于 shell 脚本中 sed 的示例，请参阅：

1.  示例 36-1

1.  示例 36-2

1.  示例 16-3

1.  示例 A-2

1.  示例 16-17

1.  示例 16-27

1.  示例 A-12

1.  示例 A-16

1.  示例 A-17

1.  示例 16-32

1.  示例 11-10

1.  示例 16-48

1.  示例 A-1

1.  示例 16-14

1.  示例 16-12

1.  示例 A-10

1.  示例 19-12

1.  示例 16-19

1.  示例 A-29

1.  示例 A-31

1.  示例 A-24

1.  示例 A-43

1.  示例 A-55

若想更深入地了解 *sed*，请参阅 *参考文献* 中的相关 参考资料。

### 备注

| [[1]](x23170.html#AEN23174) | *Sed* 在没有用户干预的情况下执行。 |
| --- | --- |
| [[2]](x23170.html#AEN23185) | 如果未指定地址范围，则默认为 *所有* 行。 |
