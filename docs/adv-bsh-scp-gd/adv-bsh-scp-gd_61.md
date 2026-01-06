# G.2\. Bash 命令行选项

> 原文：[`tldp.org/LDP/abs/html/bash-options.html`](https://tldp.org/LDP/abs/html/bash-options.html)

*Bash 本身有许多命令行选项。以下是一些更有用的选项。*

+   `-c`

    *从以下字符串中读取命令并将任何参数分配给位置参数。*

    ```sh
    bash$ **bash -c 'set a b c d; IFS="+-;"; echo "$*"'**
    a+b+c+d

    ```

+   `-r`

    `--restricted`

    *以受限模式运行 shell 或脚本。*

+   `--posix`

    *强制 Bash 符合 POSIX 模式。*

+   `--version`

    *显示 Bash 版本信息并退出。*

+   `--`

    *选项结束。命令行上的任何其他内容都是参数，而不是选项。*
