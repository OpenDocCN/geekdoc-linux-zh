# Bash 条件

在最后一节中，我们介绍了一些最受欢迎的条件表达式。现在我们可以使用它们与标准条件语句，如`if`、`if-else`和`switch case`语句结合使用。

## `if`语句

Bash 中`if`语句的格式如下所示：

```sh
      if [[ some_test ]]
then
    <commands>
fi 

```

这里有一个快速示例，它会要求你输入你的名字，以防你留空：

```sh
      #!/bin/bash # Bash if statement exampleread -p "What is your name? " name

if [[ -z ${name} ]]
then
    echo "Please enter your name!"
fi 

```

## `if Else`语句

使用`if-else`语句，你可以在`if`语句中的条件不匹配的情况下指定一个动作。我们可以将此与上一节中的条件表达式结合起来，如下所示：

```sh
      #!/bin/bash # Bash if statement exampleread -p "What is your name? " name

if [[ -z ${name} ]]
then
    echo "Please enter your name!"
else
    echo "Hi there ${name}"
fi 

```

你可以使用上一章中的所有条件表达式与上述 if 语句结合使用：

```sh
      #!/bin/bash 
admin="devdojo"

read -p "Enter your username? " username

# Check if the username provided is the admin

if [[ "${username}" == "${admin}" ]] ; then
    echo "You are the admin user!"
else
    echo "You are NOT the admin user!"
fi 

```

这里是一个`if`语句的另一个例子，它会检查你的当前`User ID`，并且不允许你以`root`用户身份运行脚本：

```sh
      #!/bin/bash if (( $EUID == 0 )); then
    echo "Please do not run as root"
    exit
fi 

```

如果你将此放在脚本顶部，当 EUID 为 0 时，它会退出，并且不会执行脚本的其他部分。这已在[DigitalOcean 社区论坛](https://www.digitalocean.com/community/questions/how-to-check-if-running-as-root-in-a-bash-script)上讨论过。

你也可以使用`if`语句测试多个条件。在这个例子中，我们想确保用户既不是管理员用户也不是 root 用户，以确保脚本不会造成太大的损害。在这个例子中，我们将使用`or`运算符，记作`||`。这意味着任一条件都需要为真。如果我们使用`&&`运算符，则两个条件都需要为真。

```sh
      #!/bin/bash 
admin="devdojo"

read -p "Enter your username? " username

# Check if the username provided is the admin

if [[ "${username}" != "${admin}" ]] && [[ $EUID != 0 ]] ; then
    echo "You are not the admin or root user, but please be safe!"
else
    echo "You are the admin user! This could be very destructive!"
fi 

```

如果你有多重条件和场景，那么可以使用与`if`和`else`语句结合的`elif`语句。

```sh
      #!/bin/bash read -p "Enter a number: " num

if [[ $num -gt 0 ]] ; then
    echo "The number is positive"
elif [[ $num -lt 0 ]] ; then
    echo "The number is negative"
else
    echo "The number is 0"
fi 

```

## `switch case`语句

就像在其他编程语言中一样，当有多个不同选择时，你可以使用`case`语句来简化复杂条件。所以，而不是使用几个`if`和`if-else`语句，你可以使用一个单独的`case`语句。

Bash `case`语句的语法如下所示：

```sh
      case $some_variable in

  pattern_1)
    commands
    ;;

  pattern_2| pattern_3)
    commands
    ;;

  *)
    default commands
    ;;
esac 

```

结构的简要概述：

+   所有`case`语句都以`case`关键字开头。

+   在`case`关键字相同的行上，你需要指定一个变量或表达式，后跟`in`关键字。

+   之后，你有你的`case`模式，其中你需要使用`)`来标识模式的结束。

+   你可以使用管道`|`分隔多个模式。

+   在模式之后，你指定在模式与指定的变量或表达式匹配时需要执行的命令。

+   所有子句都必须在末尾添加`;;`来终止。

+   你可以通过添加`*`作为模式来添加一个默认语句。

+   要关闭`case`语句，使用`esac`（case 反向拼写）关键字。

这里是一个 Bash `case`语句的示例：

```sh
      #!/bin/bash read -p "Enter the name of your car brand: " car

case $car in

  Tesla)
    echo -n "${car}'s car factory is in the USA."
    ;;

  BMW | Mercedes | Audi | Porsche)
    echo -n "${car}'s car factory is in Germany."
    ;;

  Toyota | Mazda | Mitsubishi | Subaru)
    echo -n "${car}'s car factory is in Japan."
    ;;

  *)
    echo -n "${car} is an unknown car brand"
    ;;

esac 

```

使用这个脚本，我们要求用户输入一个汽车品牌名称，如特斯拉、宝马、奔驰等。

然后使用`case`语句，我们检查品牌名称，如果它与我们的任何模式匹配，那么我们就打印出工厂的位置。

如果品牌名称与我们的任何`case`语句都不匹配，我们将打印出一个默认消息：“一个未知的汽车品牌”。

## 结论

我建议你尝试修改脚本并稍微玩一下，这样你就可以练习你在前两章中学到的内容了！

要了解更多关于 Bash `case`语句的示例，请确保查看第十六章，在那里我们将使用`cases`语句在 Bash 中创建一个交互式菜单来处理用户输入。
