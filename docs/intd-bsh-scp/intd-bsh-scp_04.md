# Bash 变量

就像任何其他编程语言一样，你可以在 Bash 脚本中使用变量。然而，没有数据类型，Bash 中的变量可以包含数字以及字符。

要给变量赋值，你只需要使用 `=` 符号：

```sh
      name="DevDojo" 

```

> **注意：**作为一个重要的注意事项，你不能在 `=` 符号前后有空格。

之后，要访问变量，你必须使用 `$` 并像下面那样引用它：

```sh
      echo $name 

```

在花括号中包裹变量名不是必需的，但被认为是一种良好的实践，我建议你尽可能使用它们：

```sh
      echo ${name} 

```

上述代码会输出：`DevDojo`，因为这正是我们的 `name` 变量的值。

接下来，让我们更新我们的 `devdojo.sh` 脚本，并在其中包含一个变量。

再次，你可以用你喜欢的文本编辑器打开 `devdojo.sh` 文件，我在这里使用 nano 打开文件：

```sh
      nano devdojo.sh 

```

在文件中添加我们的 `name` 变量，并添加一条欢迎信息。现在我们的文件看起来是这样的：

```sh
      #!/bin/bash 
name="DevDojo"

echo "Hi there $name" 

```

保存文件并使用以下命令运行文件：

```sh
      ./devdojo.sh 

```

你会在屏幕上看到以下输出：

```sh
      Hi there DevDojo 

```

这里是文件中编写的脚本的概述：

+   `#!/bin/bash` - 首先，我们指定了我们的 shebang。

+   `name=DevDojo` - 然后，我们定义了一个名为 `name` 的变量，并给它赋值。

+   `echo "Hi there $name"` - 最后，我们通过使用 `echo` 将变量的内容作为欢迎信息输出到屏幕上。

你也可以在文件中添加多个变量，如下所示：

```sh
      #!/bin/bash 
name="DevDojo"
greeting="Hello"

echo "$greeting $name" 

```

保存文件并再次运行它：

```sh
      ./devdojo.sh 

```

你会在屏幕上看到以下输出：

```sh
      Hello DevDojo 

```

注意，你不必在每一行的末尾添加分号 `;`。它两种方式都有效，有点像其他编程语言，比如 JavaScript！

你也可以在命令行外部添加变量，它们可以作为参数读取：

```sh
      ./devdojo.sh Bobby buddy! 

```

此脚本接受两个参数 `Bobby` 和 `buddy!`，它们由空格分隔。在 `devdojo.sh` 文件中，我们有以下内容：

```sh
      #!/bin/bash echo "Hello there" $1 

```

`$1` 是命令行中的第一个输入（`Bobby`）。同样，可能有更多的输入，它们都通过 `$` 符号和它们各自的输入顺序来引用。这意味着 `buddy!` 是通过 `$2` 来引用的。读取变量的另一种有用方法是 `$@`，它读取所有输入。

因此，现在让我们将 `devdojo.sh` 文件进行更改，以便更好地理解：

```sh
      #!/bin/bash echo "Hello there" $1

# $1 : first parameter

echo "Hello there" $2

# $2 : second parameter

echo "Hello there" $@

# $@ : all 

```

对于以下输出：

```sh
      ./devdojo.sh Bobby buddy! 

```

将会是以下内容：

```sh
      Hello there Bobby
Hello there buddy!
Hello there Bobby buddy! 

```
