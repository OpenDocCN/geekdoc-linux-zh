# 第三十三章。选项

> 原文：[`tldp.org/LDP/abs/html/options.html`](https://tldp.org/LDP/abs/html/options.html)

选项是改变 shell 和/或脚本行为的设置。

The set 命令允许在脚本中启用选项。在脚本中你想让选项生效的点，使用 **set -o option-name** 或简短形式 **set -option-abbrev**。这两种形式是等效的。

```sh
      #!/bin/bash

      set -o verbose
      # Echoes all commands before executing.

```
```sh
      #!/bin/bash

      set -v
      # Exact same effect as above.

```  |
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 要在脚本中 *禁用* 选项，使用 **set +o option-name** 或 **set +option-abbrev**。 |
```sh
      #!/bin/bash

      set -o verbose
      # Command echoing on.
      command
      ...
      command

      set +o verbose
      # Command echoing off.
      command
      # Not echoed.

      set -v
      # Command echoing on.
      command
      ...
      command

      set +v
      # Command echoing off.
      command

      exit 0

```  |

在脚本中启用选项的另一种方法是立即在 `*#!*` 脚本标题之后指定它们。

```sh
      #!/bin/bash -x
      #
      # Body of script follows.

```

还可以从命令行启用脚本选项。一些与 **set** 不兼容的选项可以通过这种方式使用。其中一些是 `*-i*`，强制脚本以交互式方式运行。

`**bash -v script-name**`

`**bash -o verbose script-name**`

以下是一些有用的选项列表。它们可以是缩写形式（以单个短横线开头）或完整名称（以*双*短横线或`-o`开头）。

**表 33-1\. Bash 选项**

| 缩写 | 名称 | 影响 |
| --- | --- | --- |
| `-B` | brace expansion | *启用* 花括号展开（默认设置 = *开启*） |
| `+B` | 花括号展开 | *禁用* 花括号展开 |
| `-C` | noclobber | 防止通过重定向覆盖文件（可能被 >&#124; 覆盖） |
| `-D` | (无) | 列出以 $ 开头的双引号字符串，但在脚本中不执行命令 |
| `-a` | allexport | 导出所有定义的变量 |
| `-b` | 通知 | 当在后台运行的任务终止时通知（在脚本中不太有用） |
| `-c ...` | (无) | 从 **...** 读取命令 |
| `checkjobs` |   | 在 shell 退出时通知用户任何打开的 作业。在 Bash 的 版本 4 中引入，并且仍然是“实验性的”。*用法:* shopt -s checkjobs (*注意:* 可能会挂起!) |
| `-e` | errexit | 在命令以非零状态退出时（除了在 until 或 while loops, if-tests, list constructs 中）终止脚本 |
| `-f` | noglob | 禁用文件名展开（通配符） |
| `globstar` | *通配符星号匹配* | 启用 **通配符 操作符 (version 4+ 的 Bash)。*用法:* shopt -s globstar |
| `-i` | interactive | 脚本以 *交互式* 模式运行 |
| `-n` | noexec | 读取脚本中的命令，但不执行它们（语法检查） |
| `-o Option-Name` | (无) | 调用 *Option-Name* 选项 |
| `-o posix` | POSIX | 将 Bash 或调用的脚本的行为更改为符合 POSIX 标准。 |
| `-o pipefail` | 管道失败 | 导致管道返回管道中最后一个返回非零返回值的命令的退出状态 |
| `-p` | 特权 | 脚本以 "suid" 运行（注意！） |
| `-r` | 限制 | 脚本在 *限制* 模式下运行（见第二十二章） |
| `-s` | 标准输入 | 从 `stdin` 读取命令 |
| `-t` | (无) | 执行第一个命令后退出 |
| `-u` | nounset | 尝试使用未定义的变量会输出错误信息，并强制退出 |
| `-v` | 详细 | 在执行之前将每个命令打印到 `stdout` |
| `-x` | 跟踪 | 与 `-v` 类似，但会展开命令 |
| `-` | (无) | 选项标志的结束。所有其他参数都是位置参数 |
| `--` | (无) | 取消位置参数。如果提供了参数（`*-- arg1 arg2*`），则位置参数设置为参数 |
