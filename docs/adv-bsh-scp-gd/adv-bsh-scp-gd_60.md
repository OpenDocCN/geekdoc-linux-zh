# G.1. 标准命令行选项

> 原文：[`tldp.org/LDP/abs/html/standard-options.html`](https://tldp.org/LDP/abs/html/standard-options.html)

随着时间的推移，命令行选项标志的含义已经形成了一个松散的标准。GNU 工具比旧版的 UNIX 工具更接近这个“标准”。

传统上，UNIX 命令行选项由一个连字符后跟一个或多个小写字母组成。GNU 工具添加了一个双连字符，后跟一个完整的单词或复合词。

最广泛接受的两个选项是：

+   `-h`

    `--help`

    *帮助*: 显示用法信息并退出。

+   `-v`

    `--version`

    *版本*: 显示程序版本并退出。

其他常见选项包括：

+   `-a`

    `--all`

    *全部*: 显示所有信息或对所有参数进行操作。

+   `-l`

    `--list`

    *列表*: 列出文件或参数而不采取其他操作。

+   `-o`

    *输出* 文件名

+   `-q`

    `--quiet`

    *静音*: 抑制 `stdout` 输出。

+   `-r`

    `-R`

    `--recursive`

    *递归*: 递归操作（向下目录树）。

+   `-v`

    `--verbose`

    *详细*: 向 `stdout` 或 `stderr` 输出额外信息。

+   `-z`

    `--compress`

    *压缩*: 应用压缩（通常是 gzip）。

然而：

+   在 **tar** 和 **gawk** 中：

    `-f`

    `--file`

    *文件*: 文件名随后。

+   在 **cp**、**mv**、**rm** 中：

    `-f`

    `--force`

    *强制*: 强制覆盖目标文件。

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 许多 UNIX 和 Linux 工具偏离了这个“标准”，因此假设某个选项将以标准方式运行是危险的。如有疑问，请始终检查相关命令的 man 页面。 |
| --- | --- |

GNU 工具推荐的完整选项表可在 [GNU 标准页面](http://www.gnu.org/prep/standards/) 上找到。
