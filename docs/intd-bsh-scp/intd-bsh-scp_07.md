# Bash 参数

当你执行 shell 脚本时，你可以传递参数。要传递一个参数，你只需将其直接写在脚本名称之后。例如：

```sh
      ./devdojo.com your_argument 

```

在脚本中，我们可以使用 `$1` 来引用我们指定的第一个参数。

如果我们传递第二个参数，它将作为 `$2` 可用，依此类推。

让我们创建一个名为 `arguments.sh` 的简短脚本作为示例：

```sh
      #!/bin/bash echo "Argument one is $1"
echo "Argument two is $2"
echo "Argument three is $3" 

```

保存文件并使其可执行：

```sh
      chmod +x arguments.sh 

```

然后运行文件并传递 **3** 个参数：

```sh
      ./arguments.sh dog cat bird 

```

你会得到的输出将是：

```sh
      Argument one is dog
Argument two is cat
Argument three is bird 

```

要引用所有参数，你可以使用 `$@`：

```sh
      #!/bin/bash echo "All arguments: $@" 

```

如果你再次运行脚本：

```sh
      ./arguments.sh dog cat bird 

```

你将得到以下输出：

```sh
      All arguments: dog cat bird 

```

另一件你需要记住的事情是 `$0` 用于引用脚本本身。

如果你需要自我销毁文件或者只是获取脚本的名称，这是一个非常好的方法。

例如，让我们创建一个脚本，该脚本打印出文件名并在之后删除该文件：

```sh
      #!/bin/bash echo "The name of the file is: $0 and it is going to be self-deleted."

rm -f $0 

```

在自我删除之前，你需要小心处理自我删除，并确保你有脚本的备份。
