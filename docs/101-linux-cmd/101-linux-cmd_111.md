# `basename` 命令

`basename` 是一个命令行工具，用于从给定的文件名中移除目录。可选地，它还可以移除任何尾随的后缀。这是一个简单的命令，只接受少数几个选项。

### 示例

最基本的示例是打印移除前导目录后的文件名：

```sh
      basename /etc/bar/foo.txt 

```

输出将包括文件名：

```sh
      foo.txt 

```

如果您在指向目录的路径字符串上运行 `basename`，您将获得路径的最后一个部分。在这个例子中，/etc/bar 是一个目录。

```sh
      basename /etc/bar 

```

输出

```sh
      bar 

```

`basename` 命令会移除任何尾随的 `/` 字符：

```sh
      basename /etc/bar/foo.txt/ 

```

输出

```sh
      foo.txt 

```

### 选项

1.  默认情况下，每行输出以换行符结束。要使用 NUL 结束行，请使用 `-z` (`--zero`) 选项。

```sh
      $ basename -z /etc/bar/foo.txt
foo.txt$ 

```

1.  `basename` 命令可以接受多个名称作为参数。要这样做，请使用 `-a` (`--multiple`) 选项调用命令，后跟由空格分隔的文件列表。例如，要获取 `/etc/bar/foo.txt` 和 `/etc/spam/eggs.docx` 的文件名，您将运行：

```sh
      basename -a /etc/bar/foo.txt /etc/spam/eggs.docx 

```

```sh
      foo.txt
eggs.docx 

```

### 语法

`basename` 命令支持两种语法格式：

```sh
      basename NAME [SUFFIX]
basename OPTION... NAME... 

```

### 额外功能

**移除尾随后缀**：要从文件名中移除任何尾随后缀，请将后缀作为第二个参数传递：

```sh
      basename /etc/hostname name
host 

```

通常，此功能用于去除文件扩展名

### 帮助命令

运行以下命令以查看 `basename` 命令的完整指南。

```sh
      man basename 

```
