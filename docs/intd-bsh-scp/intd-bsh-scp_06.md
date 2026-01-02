# Bash 注释

就像任何其他编程语言一样，你可以在你的脚本中添加注释。注释用于通过代码留下自己的笔记。

在 Bash 中，你需要在一行的开头添加 `#` 符号。注释永远不会在屏幕上显示。

这里是一个注释的例子：

```sh
      # This is a comment and will not be rendered on the screen 

```

让我们继续给我们的脚本添加一些注释：

```sh
      #!/bin/bash # Ask the user for their nameread -p "What is your name? " name

# Greet the user
echo "Hi there $name"
echo "Welcome to DevDojo!" 

```

注释是直接在脚本中描述一些更复杂功能的好方法，这样其他人就可以轻松地了解你的代码。
