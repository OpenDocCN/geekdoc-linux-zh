# Bash 用户输入

在之前的脚本中，我们定义了一个变量，并使用`echo $name`在屏幕上输出变量的值。

现在，让我们继续要求用户输入。为此，再次使用你喜欢的文本编辑器打开文件，并按照以下方式更新脚本：

```sh
      #!/bin/bash echo "What is your name?"
read name

echo "Hi there $name"
echo "Welcome to DevDojo!" 

```

如此一来，将提示用户输入，并将该输入作为字符串/文本存储在变量中。

然后，我们可以使用变量并回显一条消息给他们。

上述脚本的输出将是：

+   首先运行脚本：

```sh
      ./devdojo.sh 

```

+   然后，你会被提示输入你的名字：

```sh
      What is your name?
Bobby 

```

+   一旦你输入了你的名字，只需按回车键，你将得到以下输出：

```sh
      Hi there Bobby
Welcome to DevDojo! 

```

为了减少代码量，我们可以将第一个`echo`语句替换为`read -p`，使用带有`-p`标志的`read`命令会在提示用户输入之前打印一条消息：

```sh
      #!/bin/bash read -p "What is your name? " name

echo "Hi there $name"
echo "Welcome to DevDojo!" 

```

确保亲自测试一下！
