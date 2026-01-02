# 在 Bash 中创建交互式菜单

在这个教程中，我将向你展示如何在 Bash 中创建一个多选菜单，以便你的用户可以选择要执行的操作！

我们将重用前一章中的一些代码，所以如果你还没有阅读它，请确保阅读。

## 规划功能

让我们再次回顾脚本的主要功能：

+   检查当前磁盘使用情况

+   检查当前 CPU 使用情况

+   检查当前 RAM 使用情况

+   检查确切的内核版本

如果你手头没有，这里就是脚本本身：

```sh
      #!/bin/bash ### BASH menu script that checks:#   - Memory usage#   - CPU load#   - Number of TCP connections#   - Kernel version##

server_name=$(hostname)

function memory_check() {
    echo ""
	echo "Memory usage on ${server_name} is: "
	free -h
	echo ""
}

function cpu_check() {
    echo ""
	echo "CPU load on ${server_name} is: "
    echo ""
	uptime
    echo ""
}

function tcp_check() {
    echo ""
	echo "TCP connections on ${server_name}: "
    echo ""
	cat  /proc/net/tcp | wc -l
    echo ""
}

function kernel_check() {
    echo ""
	echo "Kernel version on ${server_name} is: "
	echo ""
	uname -r
    echo ""
}

function all_checks() {
	memory_check
	cpu_check
	tcp_check
	kernel_check
} 

```

我们将构建一个允许用户选择要执行哪个函数的菜单。

当然，你可以根据需要调整函数或添加新的函数。

## 添加一些颜色

为了使菜单更易于阅读和一眼就能抓住，我们将添加一些颜色函数。

在你的脚本开头添加以下颜色函数：

```sh
      ### Color  Variables##
green='\e32m'
blue='\e[34m'
red='\e[31m'
clear='\e[0m'

##
# Color Functions
##

ColorGreen(){
	echo -ne $green$1$clear
}
ColorBlue(){
	echo -ne $blue$1$clear
} 

```

你可以使用颜色函数如下：

```sh
      echo -ne $(ColorBlue 'Some text here') 

```

上述代码将输出 `Some text here` 字符串，并且它将是蓝色！

# 添加菜单

最后，为了添加我们的菜单，我们将创建一个带有菜单选项的 case switch 的单独函数：

```sh
      menu(){
echo -ne "
My First Menu
$(ColorGreen '1)') Memory usage
$(ColorGreen '2)') CPU load
$(ColorGreen '3)') Number of TCP connections
$(ColorGreen '4)') Kernel version
$(ColorGreen '5)') Check All
$(ColorGreen '0)') Exit
$(ColorBlue 'Choose an option:') "
        read a
        case $a in
	        1) memory_check ; menu ;;
	        2) cpu_check ; menu ;;
	        3) tcp_check ; menu ;;
	        4) kernel_check ; menu ;;
	        5) all_checks ; menu ;;
		0) exit 0 ;;
		*) echo -e "${red}Wrong option.${clear}"; menu ;;
        esac
} 

```

### 代码快速概述

首先，我们只是用一些颜色打印菜单选项：

```sh
      echo -ne "
My First Menu
$(ColorGreen '1)') Memory usage
$(ColorGreen '2)') CPU load
$(ColorGreen '3)') Number of TCP connections 
$(ColorGreen '4)') Kernel version
$(ColorGreen '5)') Check All
$(ColorGreen '0)') Exit
$(ColorBlue 'Choose an option:') " 

```

然后我们读取用户的答案并将其存储在变量 `$a` 中：

```sh
      read a 

```

最后，我们有一个根据变量 `$a` 的值触发不同函数的 switch case：

```sh
      case $a in
	        1) memory_check ; menu ;;
	        2) cpu_check ; menu ;;
	        3) tcp_check ; menu ;;
	        4) kernel_check ; menu ;;
	        5) all_checks ; menu ;;
		0) exit 0 ;;
		*) echo -e "${red}Wrong option.${clear}"; menu ;;
        esac 

```

最后，我们需要调用菜单函数来实际打印菜单：

```sh
      # Call the menu function
menu 

```

## 测试脚本

最后，你的脚本将看起来像这样：

```sh
      #!/bin/bash ### BASH menu script that checks:#   - Memory usage#   - CPU load#   - Number of TCP connections #   - Kernel version##

server_name=$(hostname)

function memory_check() {
    echo ""
	echo "Memory usage on ${server_name} is: "
	free -h
	echo ""
}

function cpu_check() {
    echo ""
	echo "CPU load on ${server_name} is: "
    echo ""
	uptime
    echo ""
}

function tcp_check() {
    echo ""
	echo "TCP connections on ${server_name}: "
    echo ""
	cat  /proc/net/tcp | wc -l
    echo ""
}

function kernel_check() {
    echo ""
	echo "Kernel version on ${server_name} is: "
	echo ""
	uname -r
    echo ""
}

function all_checks() {
	memory_check
	cpu_check
	tcp_check
	kernel_check
}

##
# Color  Variables
##
green='\e[32m'
blue='\e[34m'
red='\e[31m'
clear='\e[0m'

##
# Color Functions
##

ColorGreen(){
	echo -ne $green$1$clear
}
ColorBlue(){
	echo -ne $blue$1$clear
}

menu(){
echo -ne "
My First Menu
$(ColorGreen '1)') Memory usage
$(ColorGreen '2)') CPU load
$(ColorGreen '3)') Number of TCP connections 
$(ColorGreen '4)') Kernel version
$(ColorGreen '5)') Check All
$(ColorGreen '0)') Exit
$(ColorBlue 'Choose an option:') "
        read a
        case $a in
	        1) memory_check ; menu ;;
	        2) cpu_check ; menu ;;
	        3) tcp_check ; menu ;;
	        4) kernel_check ; menu ;;
	        5) all_checks ; menu ;;
		0) exit 0 ;;
		*) echo -e "${red}Wrong option.${clear}"; menu ;;
        esac
}

# Call the menu function
menu 

```

为了测试脚本，创建一个新的文件，扩展名为 `.sh`，例如：`menu.sh`，然后运行它：

```sh
      bash menu.sh 

```

你会得到的输出将如下所示：

```sh
      My First Menu
1) Memory usage
2) CPU load
3) Number of TCP connections
4) Kernel version
5) Check All
0) Exit
Choose an option: 

```

你将能够从列表中选择不同的选项，每个数字都会调用脚本中的不同函数：

![漂亮的 Bash 交互式菜单

## 结论

现在，你已经知道如何创建 Bash 菜单并在你的脚本中实现它，以便用户可以选择不同的值！

> **注意：** 此内容最初发布在 [DevDojo.com](https://devdojo.com/bobbyiliev/how-to-work-with-json-in-bash-using-jq)
