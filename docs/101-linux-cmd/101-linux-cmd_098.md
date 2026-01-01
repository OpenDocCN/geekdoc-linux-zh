# `cut` 命令

`cut` 命令让您可以从文件的每一行中删除部分内容。从每个文件中打印所选行的部分到标准输出。如果没有文件，或者当文件为 - 时，读取标准输入。

### 使用方法和示例：

1.  在文件中选择特定字段

```sh
      cut -d "delimiter" -f (field number) file.txt 

```

1.  选择特定字符：

```sh
      cut -c [(k)-(n)/(k),(n)/(n)] filename 

```

在这里，**k** 表示字符的起始位置，**n** 表示字符的结束位置，如果 *k* 和 *n* 由“-”分隔，否则它们仅表示文件输入中每行字符的位置。

1.  选择特定字节：

```sh
      cut -b 1,2,3 filename 			//select bytes 1,2 and 3
cut -b 1-4 filename				//select bytes 1 through 4
cut -b 1- filename				//select bytes 1 through the end of file
cut -b -4 filename				//select bytes from the beginning till the 4th byte 

```

**制表符和退格符**被视为 1 个字节的字符。

### 语法：

```sh
      cut OPTION... [FILE]... 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-b` | `--bytes=LIST` | 仅选择这些字节 |
| `-c` | `--characters=LIST` | 仅选择这些字符 |
| `-d` | `--delimiter=DELIM` | 使用 DELIM 作为字段分隔符，而不是制表符 |
| `-f` | `--fields` | 仅选择这些字段；还打印任何不包含分隔符字符的行，除非指定了 -s 选项 |
| `-s` | `--only-delimited` | 不打印不包含分隔符的行 |
| `-z` | `--zero-terminated` | 行分隔符是 NUL，而不是换行符 |
