# 如何使用 find 命令更有效地搜索目录

> 原文：<https://blog.eldernode.com/use-find-command-search-directories/>

![How to use find Command to search Directories more efficiently](img/4ebf29fefde47069b75aa84ce77e97f5.png)

一个 Linux 系统管理员需要知道一些 [Linux 的小技巧](https://eldernode.com/tag/linux-tricks/) 。在这篇文章中，你将学习**如何使用 find 命令更有效地搜索目录。**

一切都是一个文件，包括 Linux 中的目录。因此，按照本教程来看看不同的方式找到一个目录。Linux 用户在命令行中经常做的事情之一是搜索文件或目录。

**查找**、**定位、**和**哪个**命令是用于搜索文件的几种不同方法和实用程序之一。但是最后一个实用程序( **which** )仅用于定位命令。

[**购买 Linux 虚拟专用服务器**](https://eldernode.com/linux-vps/)

## 如何使用 find 命令更有效地搜索目录

为了完成本指南，我们将关注 find 实用程序，它在一个活动的 Linux 文件系统上搜索文件，与 **locate** 相比，它更加高效和可靠。

但是让我们解释一下 **locate 的缺点。**它读取一个或多个由 **updatedb** 创建的数据库，它不会搜索一个活动的文件系统。此外，它也没有提供从哪里(起点)搜索的灵活性。

### 使用‘查找’命令的方式

看看 find 命令是如何运行的。

```
locate [option] [search-pattern] 
```

现在，我们想向你演示一下**定位**的缺点，让我们假设我们正在当前工作目录中搜索一个名为 **pkg** 的目录。

```
locate --basename '\pkg' 
```

**请注意** 选项–basename或 -b 告诉 **locate** 只匹配文件(目录)basename(确切地说是 **pkg** )而不匹配路径( **/path/to/pkg** )。其中 \ 是一个 globbing 字符，它禁止用 ***pkg*** 隐式替换 **pkg** 。

您可以按照下面的简化语法使用 **find** 。

```
find starting-point options [expression] 
```

为了涉及更多，让我们给你看更多的例子。

运行以下命令，其中 **-name** 标志读取表达式(在本例中是目录基名)，在当前工作目录中搜索相同的目录**(pkg****)**。

```
find . -name "pkg"
```

如果您遇到任何“**权限被拒绝**的错误，请使用 [sudo](https://eldernode.com/sudoers-configurations-setting-sudo/) 命令。

```
sudo find . -name "pkg" 
```

您还可以通过使用 -type 标志来指定文件的类型，从而防止 find 搜索除目录以外的其他文件类型。

```
sudo find . -type d -name "pkg" 
```

然后，如果您想以长列表格式列出目录，请使用动作开关 **-ls** 。

```
sudo find . -type d -name "pkg" -ls 
```

然后，选项 **-iname** 将启用不区分大小写的搜索:

```
sudo find . -type d -iname "pkg"   sudo find . -type d -iname "PKG"
```

请注意，通过阅读 **find** 和 **locate** 的手册页，您将能够找到更多有趣和高级的使用信息。

```
man find  man locate
```

**好样的** ！你都准备好了。在本文的最后，我们希望再次提到，与 **locate** 命令相比，find 命令在 Linux 系统中搜索文件时更加可靠和高效。

亲爱的用户，我们希望本教程如何使用“查找”命令更有效地搜索目录对您有所帮助，要问任何问题或查看我们的用户关于本文的对话，请访问 [提问页面](https://eldernode.com/ask) 。也是为了提高自己的见识，准备了这么多有用的教程给 [Eldernode 培训](https://eldernode.com/blog/) 。