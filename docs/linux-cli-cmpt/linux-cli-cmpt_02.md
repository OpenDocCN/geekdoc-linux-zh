# 前言

> 原文：[`learnbyexample.github.io/cli-computing/preface.html`](https://learnbyexample.github.io/cli-computing/preface.html)

本书旨在教授 Linux 命令行工具和 Shell Scripting，面向初学者到中级用户。主要重点是管理您的文件和执行文本处理任务。系统管理和网络等主题将不会讨论。

## 先决条件

您应该熟悉基本的计算机使用，了解诸如文件和目录等基本术语，以及如何安装程序等。您还应该对编程基础，如变量、循环和函数等，感到舒适。

在软件方面，您应该能够访问`GNU bash`外壳和常用的 Linux 命令行工具。这可能是 Linux 发行版的一部分，或者通过其他方式，例如虚拟机、WSL（Windows Subsystem for Linux）等。有关预期工作环境的更多详细信息将在引言章节中讨论。

您还应该熟悉阅读手册、在线搜索、访问提供的进一步阅读的外部链接、尝试示例代码、在遇到困难时寻求帮助等。换句话说，要积极主动和好奇，而不仅仅是被动地消费内容。

查看我整理的[Linux CLI 和 Shell Scripting](https://learnbyexample.github.io/curated_resources/linux_cli_scripting.html)学习资源列表。

## 约定

+   展示的代码片段是从`bash`外壳（版本**5.0.17**）复制粘贴的，并为了展示目的进行了修改。一些命令前面有注释，以提供上下文和解释，添加了空行以提高可读性等。

+   本书提供了外部链接，供您深入了解某些主题。

+   [cli-computing 仓库](https://github.com/learnbyexample/cli-computing)包含了书中使用的所有[示例文件和脚本](https://github.com/learnbyexample/cli-computing/tree/master/example_files)。仓库还包括所有[练习](https://github.com/learnbyexample/cli-computing/blob/master/exercises/exercises.md)作为一个单独的文件，以及一个单独的[解决方案](https://github.com/learnbyexample/cli-computing/blob/master/exercises/exercise-solutions.md)文件。如果您不熟悉`git`命令，请点击网页上的**代码**按钮以获取文件。

+   请参阅设置部分，了解创建用于跟随本书内容的操作环境的说明。

## 致谢

+   [GNU 手册](https://www.gnu.org/manual/manual.html) — 命令行工具和`bash`外壳的文档

+   [Stack Overflow](https://stackoverflow.com/) 和 [Unix Stack Exchange](https://unix.stackexchange.com/) — 用于获取有关 CLI 工具的相关问题的答案

+   [tex.stackexchange](https://tex.stackexchange.com/) — 用于帮助解决[pandoc](https://github.com/jgm/pandoc/)和`tex`相关问题的网站

+   [/r/commandline/](https://old.reddit.com/r/commandline), [/r/linux4noobs/](https://old.reddit.com/r/linux4noobs/), [/r/linuxquestions/](https://old.reddit.com/r/linuxquestions/)和[/r/linux/](https://old.reddit.com/r/linux/) — 有用的论坛

+   [canva](https://www.canva.com/) — 封面图片

+   [警告](https://commons.wikimedia.org/wiki/File:Warning_icon.svg)和[信息](https://commons.wikimedia.org/wiki/File:Info_icon_002.svg)图标由[Amada44](https://commons.wikimedia.org/wiki/User:Amada44)在公共领域下提供

+   [carbon](https://carbon.now.sh/) — 用于创建带有高亮文本的终端截图

+   [oxipng](https://github.com/shssoichiro/oxipng), [pngquant](https://pngquant.org/)和[svgcleaner](https://github.com/RazrFalcon/svgcleaner) — 优化图片

+   [Inkscape](https://inkscape.org/) — 网站图标

+   [mdBook](https://github.com/rust-lang/mdBook) — 用于您目前正在阅读的书的网络版本

    +   [mdBook-pagetoc](https://github.com/JorelAli/mdBook-pagetoc) — 用于为每一页添加目录

    +   [minify-html](https://github.com/wilsonzlin/minify-html) — 用于压缩 html 文件

## 反馈和勘误

如果您能告诉我您对这本书的感受，我将不胜感激。这可以是简单的感谢，指出一个错误，代码片段中的错误，哪些方面对您有用（或没有用！）等等。读者反馈至关重要，尤其是对于自出版作者。

您可以通过以下方式联系我：

+   问题管理器：[`github.com/learnbyexample/cli-computing/issues`](https://github.com/learnbyexample/cli-computing/issues)

+   电子邮件：learnbyexample.net@gmail.com

+   Twitter: [`twitter.com/learn_byexample`](https://twitter.com/learn_byexample)

## 作者信息

苏尼普·阿加瓦尔是一个懒惰的人，他更喜欢只工作到足以维持他简朴的生活方式。他在模拟器件公司担任设计工程师时积累了大量财富，并在二十八岁时从商界退休。不幸的是，他在几年内挥霍了积蓄，不得不挣扎着谋生。面对所有困难，销售编程电子书救了他的懒惰自我，使他不必再次找工作。现在他可以负担得起他想要阅读的所有幻想电子书，并且花费了不健康的时间浏览互联网。

当创作灵感来临时，他可以被发现正在编写另一本编程电子书（通常至少有一个包含正则表达式的示例）。为他的电子书研究材料和日常社交媒体使用淹没了他的书签，所以他为了保持理智，维护了精选的资源列表。他对免费学习资源和开源工具表示感激。他的个人贡献可以在[`github.com/learnbyexample`](https://github.com/learnbyexample)找到。

**书籍列表**：[`learnbyexample.github.io/books/`](https://learnbyexample.github.io/books/)

## 许可

本作品受[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/)许可。

代码片段可在[MIT 许可证](https://github.com/learnbyexample/cli-computing/blob/master/LICENSE)下获取。

上文中提到的资源均可在原始许可下获取。

## 书籍版本

1.1

查看以跟踪书籍版本之间的变更[Version_changes.md](https://github.com/learnbyexample/cli-computing/blob/master/Version_changes.md)。
