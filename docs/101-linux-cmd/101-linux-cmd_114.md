# `which` 命令

`which` 命令识别当你向 shell 发出命令时启动的可执行二进制文件。如果你在计算机上安装了同一程序的不同版本，你可以使用 `which` 来找出 shell 将使用哪个版本。

它有 3 种返回状态如下：

```sh
      0 : If all specified commands are found and executable.
1 : If one or more specified commands is nonexistent or not executable.
2 : If an invalid option is specified. 

```

### 示例

1.  要查找 ls 命令的完整路径，请输入以下内容：

```sh
      which ls 

```

1.  我们可以向 `which` 命令提供多个参数：

```sh
      which netcat uptime ping 

```

`which` 命令从左到右搜索，如果在 PATH 环境变量中列出的目录中找到多个匹配项，它将只打印第一个。

1.  要显示指定命令的所有路径：

```sh
      which [filename] -a 

```

1.  要显示 node 可执行文件的路径，执行以下命令：

```sh
      which node 

```

1.  要显示 Java 可执行文件的路径，执行：

```sh
      which java 

```

### 语法

```sh
      which [filename1] [filename2] ... 

```

你可以向 `which` 传递多个程序和命令，并且它会按顺序检查它们。

例如：

`which ping cat uptime date head`

### 选项

-a : 列出找到的所有可执行实例（而不是每个实例的第一个）。

-s : 如果找到所有可执行文件，则无输出，只返回 0；如果某些文件未找到，则返回 1。
