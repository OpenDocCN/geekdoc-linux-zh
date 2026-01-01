# `locate`命令

`locate`命令通过由`updatedb`命令生成的数据库文件搜索文件系统中的文件和目录，其名称与给定的模式匹配。

### 示例：

1.  运行`locate`命令搜索名为`.bashrc`的文件。

```sh
      locate .bashrc 

```

*输出*

```sh
      /etc/bash.bashrc
/etc/skel/.bashrc
/home/linuxize/.bashrc
/usr/share/base-files/dot.bashrc
/usr/share/doc/adduser/examples/adduser.local.conf.examples/bash.bashrc
/usr/share/doc/adduser/examples/adduser.local.conf.examples/skel/dot.bashrc 

```

`/root/.bashrc`文件将不会显示，因为我们是以普通用户身份运行命令的，该用户没有访问`/root`目录的权限。

如果结果列表很长，为了更好的可读性，可以将输出通过管道传递到`[`less](https://linuxize.com/post/less-command-in-linux/)`命令：

```sh
      locate .bashrc | less 

```

1.  搜索系统中的所有`.md`文件

```sh
      locate *.md 

```

1.  搜索所有`.py`文件并仅显示 10 个结果

```sh
      locate -n 10 *.py 

```

1.  执行不区分大小写的搜索。

```sh
      locate -i readme.md 

```

*输出*

```sh
      /home/linuxize/p1/readme.md
/home/linuxize/p2/README.md
/home/linuxize/p3/ReadMe.md 

```

1.  返回所有包含`.bashrc`在名称中的文件的数量。

```sh
      locate -c .bashrc 

```

*输出*

```sh
      6 

```

1.  以下命令将仅返回文件系统中现有的`.json`文件。

```sh
      locate -e *.json 

```

1.  要运行更复杂的搜索，使用`-r`（`--regexp`）选项。要在您的系统上搜索所有`.mp4`和`.avi`文件并忽略大小写。

```sh
      locate --regex -i "(\.mp4|\.avi)" 

```

### 语法：

```sh
      1\.  locate [OPTION]... PATTERN... 

```

### 其他标志及其功能：

| **短标志** | **长标志** | **描述** |
| :-- | :-- | :-- |
| `-A` | `--all` | 用于仅显示与所有 PATTERNs 匹配的条目，而不是仅要求其中之一匹配。 |
| `-b` | `--basename` | 用于仅匹配指定的模式与基本名称。 |
| `-c` | `--count` | 用于写入匹配条目的数量，而不是在标准输出上写入文件名。 |
| `-d` | `--database DBPATH` | 用于用 DBPATH 替换默认数据库。 |
| `-e` | `--existing` | 用于在命令执行期间仅显示引用现有文件的条目。 |
| `-L` | `--follow` | 如果指定了`--existing`选项，用于检查文件是否存在并跟随符号链接。它将省略输出中的损坏符号链接。这是默认行为。可以使用`--nofollow`选项指定相反的行为。 |
| `-h` | `--help` | 用于显示包含可用选项摘要的帮助文档。 |
| `-i` | `--ignore-case` | 用于忽略指定模式的区分大小写。 |
| `-p` | `--ignore-spaces` | 用于在匹配模式时忽略标点符号和空格。 |
| `-t` | `--transliterate` | 用于在匹配模式时忽略重音符号，使用 iconv 转写。 |
| `-l` | `--limit, -n LIMIT` | 如果指定了此选项，则命令在找到 LIMIT 条目后成功退出。 |
| `-m` | `--mmap` | 用于忽略与 BSD 和 GNU locate 的兼容性。 |
| `-0` | `--null` | 用于使用 ASCII NUL 字符在输出中分隔条目，而不是在单独的一行上写入每个条目。 |
| `-S` | `--statistics` | 用于将每个读取数据库的统计信息写入标准输出，而不是搜索文件。 |
| `-r` | `--regexp REGEXP` | 用于搜索基本正则表达式 REGEXP。 |
| `--regex` | - | 它用于将所有模式描述为扩展正则表达式。 |
| `-V` | `--version` | 它用于显示版本和许可信息。 |
| `-w` | `--wholename` | 它用于匹配指定模式中仅整个路径名。 |
