# 如何在 Debian 10 [Z shell] - Eldernode 上安装和使用 ZSH

> 原文：<https://blog.eldernode.com/install-and-use-zsh-on-debian-10/>

教程**如何在 Debian 10** [Linux](https://blog.eldernode.com/tag/linux/) 服务器上安装使用 ZSH。Z shell 是 Unix 的一个可编程 shell，它是 Bourne Shel 的扩展版本，为我们提供了几个特性，如支持超过 275 个插件和大约 150 个主题。首先，购买 [Linux VPS](https://eldernode.com/linux-vps/) 以便能够在你的服务器上使用它的所有好处。

为了让本教程更好地发挥作用，请考虑以下**先决条件**:

拥有 **sudo** 权限的非 root 用户。
进行设置，按照我们的[用 Debian 10](https://blog.eldernode.com/initial-setup-with-debian-10/) 进行初始设置。

## 如何在 Debian 10 上安装和使用 ZSH【Z shell】

加入我们这个教程来回顾如何在 Debian 10 中安装 ZSH，看看你的终端在那之前从来不够好。在接下来的内容中，你将学习安装，你还会看到一些最常用的插件来帮助你启动 Zsh。

## 教程在 Debian 10 Linux 服务器上安装和使用 ZSH

### Debian 10 上的 Zsh 特性

**更好的**标签完成

**轻松的**目录导航

支持大量的**主题**和**插件**

**语法**高亮显示

自动**完成**

交互**配置**

颜色**定制**

你可以用你的包管理器安装 Zsh。关于软呢帽，RHEL 和 CentOS:

```
sudo dnf install zsh
```

此外，您可以检查当前正在使用的任何 shells:

```
echo SHELL /usr/bin/bash
```

键入下面的命令来安装在 Debian 上:

```
sudo apt install zsh
```

**或**

打开终端窗口。将以下内容复制并粘贴到终端窗口，然后点击**回车键**。可能会提示您输入密码。

```
sudo apt update sudo apt upgrade sudo apt install zsh 
```

### 如何在 Debian 10 上设置 Zsh 和使用 ZSH

因为 Zsh 不是一个终端模拟器，而是一个运行在终端模拟器内部的 shell，所以您需要启动一个终端窗口来启动 Zsh。终端窗口，如 [Gnome](https://blog.eldernode.com/install-gnome-environment-on-debian-8/) 终端、Konsole、terminal、iTerm2、rxvt 或您喜欢的其他终端。一旦您确保启动其中一个，ypu 就可以使用下面的命令来启动 Zsh。

```
zsh
```

万一这是你第一次启动 Zsh，你会被提示选择一些配置选项。

**注意**:这些配置都可以在以后更改，所以您可以按 **1** 继续。

```
This is the Z Shell configuration function for new users, zsh-newuser-install.  (q)  Quit and do nothing.  (0)  Exit, creating the file ~/.zshrc  (1)  Continue to the main menu.
```

### 如何使用 Zsh

一旦安装完成，您可以清楚地看到 Zsh 感觉很像使用 Bash。因为 Bash 和 Tcsh 之间有很大的区别，所以如果您学习如何在 Bash 和 Zsh 之间切换，如果您必须在工作中或服务器上使用 Bash，那么在家里使用它会很容易。

### 如何用 Zsh 搜索

预计将使用 [**find**](https://blog.eldernode.com/use-find-command-search-directories/) 或 [**locate**](https://blog.eldernode.com/linux-commands-with-examples/#locate_Command) 命令来查找使用普通 shell 的文件，甚至使用 **ls -R** 来递归列出一组目录。但是，Zsh 允许您在当前或任何其他子目录中查找文件。例如，假设您有两个名为 **elder.txt** 的文件。一个位于你当前的目录下，另一个在一个叫做**前辈**的子目录下。在 Bash shell 中，您可以使用以下命令列出当前目录中的文件:

```
ls elder.txt
```

你可以明确地列出另一个:

```
ls elder elder.txt
```

无论如何，要列出这两者，你必须使用 **-R** 开关，也许与 **grep** 结合使用:

```
ls -R | grep elder.txt elder.txt elder.txt
```

但是在 Zsh 中，您可以使用 ****** 简写:

```
% ls **/elder.txt elder.txt elder.txt
```

请注意，您可以对任何命令使用该语法，而不仅仅是对 **ls。**

### 如何在 Zsh 中使用通配符

要最大化您在图书馆数据库中的搜索结果，您可以使用通配符，这是一种高级搜索技术。通配符**在搜索词中使用**来表示一个或多个其他字符。此外，在 Zsh 中，通配符的行为与 Bash 用户习惯的不同。要列出当前目录中的所有文件夹，请使用修改后的通配符:

```
% ls dir0   dir1   dir2   file0   file1 % ls *(/) dir0   dir1   dir2
```

在本例中，(/)限定了通配符的结果，因此 Zsh 将只显示目录。要仅列出文件，请使用(。).要列出符号链接，请使用(@)。要列出可执行文件，请使用(*)。

```
% ls ~/bin/*(*) fop  exify  tt
```

### Zsh 插件

要通过启用各种功能来为您的 shell 添加这些功能，您需要使用

[Zsh plugins](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins#plugins)

。它们分别记录在各自的自述文件中

**plugins/**

文件夹。要启用插件，您需要将它们的名称添加到

**plugins**

数组在您的

**.zshrc**

文件。

例如，这将依次启用 **rails** 、 **git** 和 **ruby** 插件、**:**

**如果您需要检查当前的插件，更新或安装新的插件，您可以运行以下命令:**

```
`zplug <option>` 
```

****检查** —如果所有包都已安装，返回 true，否则返回 false】**

**清理—删除不再受管理的存储库**

****清除** —移除缓存文件**

****env** —【自定义】zplug 的环境变量**

****info** —显示给定包的源 URL 和标签值等信息**

****安装** —并行安装包**

****list** —列出已安装的软件包(更具体地说，查看关联数组$zplugs)**

****load**—Source installed plugins 并添加 installed commands 到＄PATH**

****状态** —检查远程存储库是否是最新的**

****更新** —并行更新已安装的软件包**

### **如何将 Zsh 设置为默认 Shell**

**如果您发现 Zsh 适合您的云服务器，那么让它成为您的用户的默认 shell，让它在您登录时加载到 Zsh 会话中。你将不再需要在你的 [VPS](https://eldernode.com/linux-vps/) 中输入“zsh”来访问 zsh。**

```
`chsh -s $(which zsh)`
```

### **如何配置 Zsh**

**Zsh 配置文件的位置在您的主目录中(例如. ~/)。zshrc)。以下配置文件将帮助您开始。**

**创建~/。zshrc 具有以下内容:**

```
`# Set path if required #export PATH=$GOPATH/bin:/usr/local/go/bin:$PATH  # Aliases alias ls='ls --color=auto' alias ll='ls -lah --color=auto' alias grep='grep --color=auto' alias ec="$EDITOR $HOME/.zshrc" # edit .zshrc alias sc="source $HOME/.zshrc"          # reload zsh configuration  # Set up the prompt - if you load Theme with zplugin as in this example, this will be overiden by the Theme. If you comment out the Theme in zplugins, this will be loaded. autoload -Uz promptinit promptinit prompt adam1            # see Zsh Prompt Theme below  # Use vi keybindings even if our EDITOR is set to vi bindkey -e  setopt histignorealldups sharehistory  # Keep 1000 lines of history within the shell and save it to ~/.zsh_history: HISTSIZE=5000 SAVEHIST=5000 HISTFILE=~/.zsh_history  # Use modern completion system autoload -Uz compinit compinit  # zplug - manage plugins source /usr/share/zplug/init.zsh zplug "plugins/git", from:oh-my-zsh zplug "plugins/sudo", from:oh-my-zsh zplug "plugins/command-not-found", from:oh-my-zsh zplug "zsh-users/zsh-syntax-highlighting" zplug "zsh-users/zsh-autosuggestions" zplug "zsh-users/zsh-history-substring-search" zplug "zsh-users/zsh-completions" zplug "junegunn/fzf" zplug "themes/robbyrussell", from:oh-my-zsh, as:theme   # Theme  # zplug - install/load new plugins when zsh is started or reloaded if ! zplug check --verbose; then     printf "Install? [y/N]: "     if read -q; then         echo; zplug install     fi fi zplug load --verbose`
```

### **如何改变主题**

**打开**就能发现。zshrc** 文件。默认情况下，Oh-my-zsh 使用“robbyrussell”主题。**

**.zshrc**

```
`# If you come from bash you might have to change your $PATH. # export PATH=$HOME/bin:/usr/local/bin:$PATH  # Path to your oh-my-zsh installation. export ZSH=$HOME/.oh-my-zsh  # Set name of the theme to load --- if set to "random", it will # load a random theme each time oh-my-zsh is loaded, in which case, # to know which specific one was loaded, run: echo $RANDOM_THEME # See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes ZSH_THEME="robbyrussell"` 
```

**此外，您还可以在~ **/中找到许多其他主题。oh-my-zsh/themes/** 目录。**

```
`ls ~/.oh-my-zsh/themes`
```

**万一你需要改变默认主题，你应该编辑**。zshr** c 文件，更改默认主题，并通过运行以下命令保存更改:**

```
`source .zshrc`
```

**结论**

**在本文中，您了解了如何在 Debian 10 上安装和使用 ZSH。从现在开始你可以使用 Zsh 并享受它的特性。另外，你可以阅读更多关于 Linux 服务器监控命令的内容。**

**Conclusion**

**In this article, you learned How To Install And Use ZSH On Debian 10\. From now on you can use Zsh and enjoy its features. Also, you can read more on [Linux Server Monitoring Commands](https://blog.eldernode.com/linux-server-monitoring-commands/).**