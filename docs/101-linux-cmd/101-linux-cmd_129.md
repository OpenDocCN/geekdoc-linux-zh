# `nl`命令

“nl”命令用于对文件中的行进行编号。以不同的方式查看文件内容，“nl”命令对于许多任务非常有用。

## 语法

```sh
      nl [ -b Type ] [ -f Type ] [ -h Type ] [ -l Number ] [ -d Delimiter ] [ -i Number ] [ -n Format ] [ -v Number ] [ -w Number ] [ -p ] [ -s Separator ] [ File ] 

```

## 示例：

1.  要对所有行进行编号：

```sh
      nl  -ba  chap1 

```

1.  显示所有文本行：

```sh
      [server@ssh ~]$ nl states
1 Alabama
2 Alaska
3 Arizona
4 Arkansas
5 California
6 Colorado
7 Connecticut.
8 Delaware 

```

1.  指定不同的行号格式

```sh
      nl  -i10  -nrz  -s::  -v10  -w4  chap1 

```

在命令行中只能指定一个文件。你可以按任何顺序列出标志和文件名。
