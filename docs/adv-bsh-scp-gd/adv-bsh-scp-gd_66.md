# 附录 L. 历史命令

> 原文：[`tldp.org/LDP/abs/html/histcommands.html`](https://tldp.org/LDP/abs/html/histcommands.html)

Bash shell 提供了用于编辑和操作用户 *命令历史* 的命令行工具。这主要是为了方便，是一种节省按键的方法。

Bash 历史命令：

1.  **history**

1.  **fc**

```sh
bash$ **history**
 1  mount /mnt/cdrom
    2  cd /mnt/cdrom
    3  ls
     ...

```

与 Bash 历史命令关联的内部变量：

1.  $HISTCMD

1.  $HISTCONTROL

1.  $HISTIGNORE

1.  $HISTFILE

1.  $HISTFILESIZE

1.  $HISTSIZE

1.  $HISTTIMEFORMAT (Bash，版本 3.0 或更高)

1.  !!

1.  !$

1.  !#

1.  !N

1.  !-N

1.  !STRING

1.  !?STRING?

1.  ^STRING^string^

不幸的是，Bash 历史工具在脚本编写中找不到用途。

```sh
#!/bin/bash
# history.sh
# A (vain) attempt to use the 'history' command in a script.

history                      # No output.

var=$(history); echo "$var"  # $var is empty.

#  History commands are, by default, disabled within a script.
#  However, as dhw points out,
#+ set -o history
#+ enables the history mechanism.

set -o history
var=$(history); echo "$var"   # 1  var=$(history)
```
```sh
bash$ **./history.sh**
(no output)	      

```  |

[在 Bash Shell 中进步](http://samrowe.com/wordpress/advancing-in-the-bash-shell/) 网站提供了关于在 Bash 中使用历史命令的良好介绍。
