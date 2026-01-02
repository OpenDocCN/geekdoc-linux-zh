# 密码生成器 Bash 脚本

需要生成随机密码以用于任何软件安装或注册任何网站的情况并不少见。

有很多选项可以实现这一点。您可以使用密码管理器/保险库，通常您有随机生成密码或让网站代为生成密码的选项。

您也可以在终端（命令行）中使用 Bash 生成一个您可以快速使用的密码。有很多方法可以实现这一点，我会确保涵盖其中的一些，并让您选择最适合您需求的方法。

## :warning: 安全

**此脚本旨在练习您的 bash 脚本编写技能。您可以在使用 BASH 进行简单项目时享受乐趣，但安全不是玩笑，所以请确保您不要将密码以纯文本形式保存在本地文件中，也不要将它们手写在一张纸上。**

**我强烈建议每个人都使用安全和可信的提供商来生成和保存密码。**

## 脚本摘要

让我先快速总结一下我们的脚本将要做什么：

1.  当脚本执行时，我们将有选择密码字符长度的选项。

1.  脚本将生成 5 个随机密码，其长度在步骤 1 中指定

## 前提条件

您需要一个 bash 终端和一个文本编辑器。您可以使用任何文本编辑器，如 vi、vim、nano 或 Visual Studio Code。

我在本地的 Linux 笔记本电脑上运行此脚本，但如果您使用 Windows PC，您可以通过 ssh 连接到您选择的任何服务器并执行该脚本。

虽然脚本很简单，但具备一些基本的 BASH 脚本知识将帮助您更好地理解脚本及其工作原理。

## 生成随机密码

Linux 的一个巨大优势是您可以使用不同的方法做很多事情。当涉及到生成随机字符串时，情况也是如此。

您可以使用多个命令来生成随机字符串。我会介绍其中的一些，并提供一些示例。

+   使用`date`命令。`date`命令将输出当前日期和时间。然而，我们进一步操作输出，以便将其用作随机生成的密码。我们可以使用 md5、sha 或直接通过 base64。以下是一些示例：

```sh
      date | md5sum
94cb1cdecfed0699e2d98acd9a7b8f6d  - 

```

使用 sha256sum：

```sh
      date | sha256sum
30a0c6091e194c8c7785f0d7bb6e1eac9b76c0528f02213d1b6a5fbcc76ceff4  - 

```

使用 base64：

```sh
      date | base64
0YHQsSDRj9C90YMgMzAgMTk6NTE6NDggRUVUIDIwMjEK 

```

+   我们也可以使用 openssl 来生成伪随机字节，并将输出通过 base64 编码。一个示例输出将是：

```sh
      openssl rand -base64 10
9+soM9bt8mhdcw== 

```

请记住，openssl 可能没有安装到您的系统上，因此您可能需要首先安装它才能使用它。

+   最受欢迎的方式是使用伪随机数生成器 - /dev/urandom，因为它适用于大多数加密目的。我们还需要使用`tr`来操作输出。一个示例命令是：

```sh
      tr -cd '[:alnum:]' < /dev/urandom | fold -w10 | head -n 1 

```

使用这个命令，我们将来自/dev/urandom 的输出通过`tr`进行转换，同时使用所有字母和数字，并打印出所需数量的字符。

## 脚本

首先，我们通过 shebang 开始脚本。我们使用它来告诉操作系统使用哪个解释器来解析文件的其余部分。

```sh
      #!/bin/bash 

```

我们可以继续并要求用户输入一些信息。在这种情况下，我们想知道密码需要多少个字符：

```sh
      # Ask user for password length
clear
printf "\n"
read -p "How many characters you would like the password to have? " pass_length
printf "\n" 

```

生成密码，然后打印出来，以便用户可以使用它。

```sh
      # This is where the magic happens!# Generate a list of 10 strings and cut it to the desired value provided from the userfor i in {1..10}; do (tr -cd '[:alnum:]' < /dev/urandom | fold -w${pass_length} | head -n 1); done

# Print the strings
printf "$pass_output\n"
printf "Goodbye, ${USER}\n" 

```

## 完整的脚本：

```sh
      #!/bin/bash#=======================================# Password generator with login option#=======================================# Ask user for the string length
clear
printf "\n"
read -p "How many characters you would like the password to have? " pass_length
printf "\n"

# This is where the magic happens!
# Generate a list of 10 strings and cut it to the desired value provided from the user

for i in {1..10}; do (tr -cd '[:alnum:]' < /dev/urandom | fold -w${pass_length} | head -n 1); done

# Print the strings
printf "$pass_output\n"
printf "Goodbye, ${USER}\n" 

```

## 结论

这基本上就是您可以使用简单的 bash 脚本来生成随机密码的方法。

:warning: **如前所述，请确保使用强密码以确保您的账户受到保护。同时，尽可能使用双因素认证，因为这将为您的账户提供额外的安全层。**

虽然脚本运行良好，但它期望用户会提供所需输入。为了防止任何问题，您需要对用户输入进行一些更高级的检查，以确保即使提供的输入不符合我们的需求，脚本也能继续正常工作。

## 贡献者

[Alex Georgiev](https://twitter.com/alexgeorgiev17)
