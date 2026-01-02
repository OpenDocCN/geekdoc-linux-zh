# Bash Hello World

一旦我们创建了`devdojo.sh`文件，并在第一行指定了 bash 的 shebang，我们就准备好创建我们的第一个`Hello World` bash 脚本。

要做到这一点，请再次打开`devdojo.sh`文件，并在`#!/bin/bash`行之后添加以下内容：

```sh
      #!/bin/bash echo "Hello World!" 

```

保存文件并退出。

之后，通过以下命令使脚本可执行：

```sh
      chmod +x devdojo.sh 

```

之后，执行该文件：

```sh
      ./devdojo.sh 

```

你将在屏幕上看到一条“Hello World”消息。

运行脚本的另一种方式是：

```sh
      bash devdojo.sh 

```

由于 bash 可以用于交互式操作，你可以在终端中直接运行以下命令，并将得到相同的结果：

```sh
      echo "Hello DevDojo!" 

```

当你需要将多个命令组合在一起时，编写脚本是有用的。
