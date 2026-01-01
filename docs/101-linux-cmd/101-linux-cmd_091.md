# `crontab` 命令

`crontab` 用于维护个别用户的 crontab 文件（Vixie Cron）

crontab 是用于在 Vixie Cron 中驱动 cron(8) 守护进程所使用的表的安装、卸载或列出的程序。每个用户都可以有自己的 crontab，尽管这些文件位于 `/var/spool/cron/crontabs` 中，但它们并不是直接编辑的。

### 语法：

```sh
      crontab [ -u user ] file
crontab [ -u user ] [ -i ] { -e | -l | -r } 

```

### 示例：

1.  `-l` 选项会导致当前 crontab 在标准输出上显示。

```sh
      crontab -l 

```

1.  `-r` 选项会导致当前 crontab 被移除。

```sh
      crontab -r 

```

1.  `-e` 选项用于使用由 VISUAL 或 EDITOR 环境变量指定的编辑器编辑当前 crontab。退出编辑器后，修改后的 crontab 将会自动安装。如果这两个环境变量都没有定义，则默认使用 /usr/bin/editor 编辑器。

```sh
      crontab -e 

```

1.  您可以指定要编辑 crontab 的用户。每个用户都有自己的 crontab。假设您有一个 `www-data` 用户，实际上这是 Apache 默认运行的用户。如果您想编辑此用户的 crontab，可以运行以下命令

```sh
      crontab -u www-data -e 

```

### 帮助命令

运行以下命令以查看 `crontab` 命令的完整指南。

```sh
      man crontab 

```
