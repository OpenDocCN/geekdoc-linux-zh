# `vim` 命令

[vim](https://www.vim.org/) 是一个随 Linux、BSD 和 macOS 一起提供的 Unix 文本编辑器。它因其速度快、功能强大而闻名，部分原因是因为它是一个可以在终端中运行的（尽管它有一个图形界面）小型程序。`vim` 文本编辑器由 [Bram Moolenaar](https://en.wikipedia.org/wiki/Bram_Moolenaar) 开发。它支持大多数文件类型，`vim` 编辑器也被称为程序员编辑器。这主要是因为它可以完全通过键盘进行管理，无需菜单或鼠标。

**注意：** 不要将 `vim` 与 `vi` 混淆。`vi` 代表“Visual”，是由 [Bill Joy](https://en.wikipedia.org/wiki/Bill_Joy) 在 1976 年开发的文本编辑器。`vim` 代表“Vi Improved”，是 `vi` 编辑器的改进克隆。

### 关于 `vim` 最常搜索的问题：

`如何退出 `vim` 编辑器？`

关于 `vim` 编辑器最常搜索的问题看起来非常有趣，但确实如此，新用户在使用 `vim` 编辑器时会在一开始就遇到困难。

保存文件并退出 `vim` 编辑器的命令：`:wq` 或 `:x`

不保存文件退出 `vim` 编辑器的命令：`:q!`

#### 有趣的阅读：

这里有一个 [调查](https://stackoverflow.blog/2017/05/23/stack-overflow-helping-one-million-developers-exit-vim/) 关于同样的问题，看看这个，不要想着退出 `vim` 编辑器。

### 安装：

首先检查 `vim` 是否已经安装，输入以下命令：

```sh
      vim --version 

```

如果它已经安装，它将显示其版本，否则我们可以运行以下命令进行安装：

在 Ubuntu/Debian 上：

```sh
      sudo apt-get install vim 

```

在 Arch 上：

```sh
      sudo pacman -S vim 

```

在 CentOS/Fedora 上：

```sh
      sudo yum install vim 

```

如果你想在 CentOS/Fedora 上使用高级功能，你需要安装增强的 `vim` 编辑器，为此请运行以下命令：

```sh
      sudo yum install -y vim-enhanced 

```

在 macOS 上：

```sh
      brew install vim 

```

### 语法：

```sh
      vim [FILE_PATH/FILE_NAME] 

```

### 示例：

1.  要从当前目录打开名为 "demo.txt" 的文件：

```sh
      vim demo.txt 

```

1.  要在特定目录中打开文件：

```sh
      vim {File_Path/filename} 

```

1.  要在文件中从特定行打开文件：

```sh
      vim {File_Path/filename} +LINE_NUMBER 

```

### `vim` 编辑器的模式：

关于 `vim` 有多少种模式的争论，但你最可能使用的模式是 `命令模式` 和 `插入模式`。这些 [模式](https://www.freecodecamp.org/news/vim-editor-modes-explained/) 将允许你完成几乎所有你需要做的事情，包括创建你的文档、保存你的文档，以及进行高级编辑，包括利用搜索和替换功能。

### `vim` 编辑器的流程：

1.  使用 `vim filename` 打开新或现有文件。

1.  输入 `i` 以切换到插入模式，这样你就可以开始编辑文件。

1.  进入或修改你的文件文本。

1.  当你完成时，按 `Esc` 键退出插入模式并返回命令模式。

1.  输入 :w 或 :wq 以保存文件或分别保存并退出文件。

### 在 `vim` 中导航

一些常用命令：

+   `j` : 向下移动一行

+   `k` : 向上移动一行

+   `h` : 向左移动一个字符

+   `l` : 向右移动一个字符

+   `w` : 向前移动一个单词

+   `b` : 向后移动一个单词

+   `e` : 移动到单词末尾

+   `0` : 移动到行首

+   `$` : 移动到行尾

+   `gg` : 跳转到文件开头

+   `G` : 跳转到文件末尾

+   `:linenumber` : 跳转到特定的行号

### 复制、粘贴和删除

1.  复制（剪切）：在 vim 中，复制称为“剪切”：

+   `yy` : 复制（剪切）当前行

+   `2yy` : 复制 2 行

+   `y$` : 从光标处复制到行尾

+   `y^` : 从光标处复制到行开头

+   `yw` : 复制一个单词

+   `y}` : 复制到段落结尾

1.  粘贴：

+   `p` : 在光标后粘贴

+   `P` : 在光标前粘贴

1.  删除：

+   `x` : 删除一个字符

+   `dd` : 删除整行

+   `2dd` : 删除 2 行（或使用任何数字 `n dd`）

+   `d$` : 从光标处删除到行尾

+   `d^` : 从光标处删除到行开头

+   `dG` : 从光标处删除到文件结尾

+   `dgg`: 从光标处删除到文件开头

+   `dw` : 从光标处删除到单词结尾

+   `di"` : 删除双引号内的内容

+   `diw` : 删除内部单词（无空格）

+   `dip` : 删除内部段落（无换行符）

### 选择（视觉模式）

+   `v` : 开始字符选择

+   `V` : 开始行选择

+   `ctrl + v` : 开始块选择

### 交互式训练

在这个交互式教程中，您将学习使用 `vim` 命令的不同方法：

[Open vim 教程](https://www.openvim.com/)

### 其他标志及其功能：

| **标志/选项** | **描述** |
| :-- | :-- |
| `-e` | 以 Ex 模式启动（见 [Ex 模式](http://vimdoc.sourceforge.net/htmldoc/intro.html#Ex-mode)） |
| `-R` | 以只读模式启动 |
| `-R` | 以只读模式启动 |
| `-g` | 以 [GUI](http://vimdoc.sourceforge.net/htmldoc/gui.html#GUI) 模式启动 |
| `-eg` | 以 Ex 模式启动 GUI |
| `-Z` | 类似于 "vim"，但在受限模式下 |
| `-d` | 以 diff 模式启动 [diff 模式](http://vimdoc.sourceforge.net/htmldoc/diff.html#diff-mode) |
| `-h` | 显示用法（帮助）信息并退出 |
| `+NUMBER` | 打开文件并将光标置于由 NUMBER 指定的行号上 |

### 有关 vim 的更多信息：

vim 不能在一天内学会，将其用于日常任务以在实际的 vim 编辑器中练习。

要了解更多关于 `vim` 的信息，请参考以下文章：

[Daniel Miessler 的文章](https://danielmiessler.com/study/vim/)
