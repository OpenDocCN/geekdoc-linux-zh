# `RPM` 命令

`rpm` - RPM 软件包管理器

`rpm` 是一个强大的 **软件包管理器**，可用于构建、安装、查询、验证、更新和擦除单个软件包。**软件包**由用于安装和擦除存档文件的文件存档和元数据组成。元数据包括辅助脚本、文件属性和有关软件包的描述性信息。软件包有两种类型：二进制软件包，用于封装要安装的软件，和源软件包，包含生成二进制软件包所需的源代码和配方。

必须选择以下基本模式之一：**查询、验证、签名检查、安装/升级/刷新、卸载、初始化数据库、重建数据库、重新签名、添加签名、设置所有者/组、显示查询标签和显示配置。**

**通用选项**

这些选项可以在所有不同的模式下使用。

| 短标志 | 长标志 | 描述 |
| --- | --- | --- |
| -? | --help | 打印比正常更长的使用消息。 |
| - | --version | 打印使用中的 rpm 版本号的单行信息。 |
| - | --quiet | 尽可能少地打印信息 - 通常只显示错误消息。 |
| -v | - | 打印详细信息 - 通常显示常规进度消息。 |
| -vv | - | 打印大量丑陋的调试信息。 |
| - | --rcfile FILELIST | rpm 将按顺序读取冒号分隔的 FILELIST 中的每个文件以获取配置信息。列表中的第一个文件必须存在，波浪号将被展开为 $HOME 的值。默认的 FILELIST 是 /usr/lib/rpm/rpmrc:/usr/lib/rpm/redhat/rpmrc:/etc/rpmrc:~/.rpmrc。 |
| - | --pipe CMD | 将 rpm 的输出管道传输到命令 CMD。 |
| - | --dbpath DIRECTORY | 使用 DIRECTORY 中的数据库而不是默认路径 /var/lib/rpm |
| - | --root DIRECTORY | 使用根目录为 DIRECTORY 的文件系统树进行所有操作。请注意，这意味着 DIRECTORY 内的数据库将用于依赖性检查，并且任何脚本（例如，如果安装则运行 %post，如果构建则运行 %prep 的软件包）将在 chroot(2) 到 DIRECTORY 之后运行。 |
| -D | --define='MACRO EXPR' | 定义具有值 EXPR 的宏 MACRO。 |
| -E | --eval='EXPR' | 打印 EXPR 的宏展开。 |

# 概述

## 查询和验证软件包：

```sh
      rpm {-q|--query} [select-options] [query-options]

rpm {-V|--verify} [select-options] [verify-options]

rpm --import PUBKEY ...

rpm {-K|--checksig} [--nosignature] [--nodigest] PACKAGE_FILE ... 

```

## 安装、升级和删除软件包：

```sh
      rpm {-i|--install} [install-options] PACKAGE_FILE ...

rpm {-U|--upgrade} [install-options] PACKAGE_FILE ...

rpm {-F|--freshen} [install-options] PACKAGE_FILE ...

rpm {-e|--erase} [--allmatches] [--nodeps] [--noscripts] [--notriggers] [--test] PACKAGE_NAME ... 

```

## 杂项：

```sh
      rpm {--initdb|--rebuilddb}

rpm {--addsign|--resign} PACKAGE_FILE...

rpm {--querytags|--showrc}

rpm {--setperms|--setugids} PACKAGE_NAME . 

```

### 查询选项

```sh
      [--changelog] [-c,--configfiles] [-d,--docfiles] [--dump]
[--filesbypkg] [-i,--info] [--last] [-l,--list]
[--provides] [--qf,--queryformat QUERYFMT]
[-R,--requires] [--scripts] [-s,--state]
[--triggers,--triggerscripts] 

```

### 验证选项

```sh
      [--nodeps] [--nofiles] [--noscripts]
[--nodigest] [--nosignature]
[--nolinkto] [--nofiledigest] [--nosize] [--nouser]
[--nogroup] [--nomtime] [--nomode] [--nordev]
[--nocaps] 

```

### 安装选项

```sh
      [--aid] [--allfiles] [--badreloc] [--excludepath OLDPATH]
[--excludedocs] [--force] [-h,--hash]
[--ignoresize] [--ignorearch] [--ignoreos]
[--includedocs] [--justdb] [--nodeps]
[--nodigest] [--nosignature] [--nosuggest]
[--noorder] [--noscripts] [--notriggers]
[--oldpackage] [--percent] [--prefix NEWPATH]
[--relocate OLDPATH=NEWPATH]
[--replacefiles] [--replacepkgs]
[--test] 

```
