# 附录 N. 将 DOS 批处理文件转换为 Shell 脚本

> 原文：[`tldp.org/LDP/abs/html/dosbatch.html`](https://tldp.org/LDP/abs/html/dosbatch.html)

许多程序员在运行 DOS 的 PC 上学习脚本编写。即使功能受限的 DOS 批处理文件语言也允许编写一些相当强大的脚本和应用程序，尽管它们通常需要大量的修正和解决方案。偶尔，仍然需要将旧的 DOS 批处理文件转换为 UNIX shell 脚本。这通常不难，因为 DOS 批处理文件操作符只是等效 shell 脚本操作符的一个子集。

**表 N-1. 批处理文件关键字/变量/操作符及其 shell 等效**

| 批处理文件操作符 | Shell 脚本等效 | 含义 |
| --- | --- | --- |
| `%` | $ | 命令行参数前缀 |
| `/` | - | 命令选项标志 |
| `\` | / | 目录路径分隔符 |
| `==` | = | (等于) 字符串比较测试 |
| `!==!` | != | (不等于) 字符串比较测试 |
| `&#124;` | &#124; | 管道 |
| `@` | set `+v` | 不回显当前命令 |
| `*` | * | 文件名 "通配符" |
| `>` | > | 文件重定向（覆盖） |
| `>>` | >> | 文件重定向（追加） |
| `<` | < | 重定向 `stdin` |
| `%VAR%` | $VAR | 环境变量 |
| `REM` | # | 注释 |
| `NOT` | ! | 取消后续测试 |
| `NUL` | `/dev/null` | "黑洞"用于隐藏命令输出 |
| `ECHO` | echo | echo（Bash 中有更多选项） |
| `ECHO.` | echo | 输出空行 |
| `ECHO OFF` | set `+v` | 不回显后续命令 |
| `FOR %%VAR IN (LIST) DO` | for var in [list]; do | "for" 循环 |
| `:LABEL` | none (unnecessary) | 标签 |
| `GOTO` | none (use a function) | 跳转到脚本中的另一个位置 |
| `PAUSE` | sleep | 暂停或等待一段时间 |
| `CHOICE` | case 或 select | 菜单选择 |
| `IF` | if | if 测试 |
| `` IF EXIST `*FILENAME*` `` | if [ -e filename ] | 测试文件是否存在 |
| `IF !%N==!` | if [ -z "$N" ] | 如果可替换参数 "N" 不存在 |
| `CALL` | source 或 . (点操作符) | "包含"另一个脚本 |
| `COMMAND /C` | source 或 . (点操作符) | "包含"另一个脚本（与 CALL 相同） |
| `SET` | export | 设置环境变量 |
| `SHIFT` | shift | 左移命令行参数列表 |
| `SGN` | -lt 或 -gt | 符号（整数） |
| `ERRORLEVEL` | $? | 退出状态 |
| `CON` | `stdin` | "控制台"（`stdin`） |
| `PRN` | `/dev/lp0` | (通用) 打印机设备 |
| `LPT1` | `/dev/lp0` | 第一台打印机设备 |
| `COM1` | `/dev/ttyS0` | 第一台串行端口 |

批处理文件通常包含 DOS 命令。这些命令必须翻译成它们的 UNIX 等效命令，以便将批处理文件转换为 shell 脚本。

**表 N-2. DOS 命令及其 UNIX 等效** 

| DOS 命令 | UNIX 等效命令 | 影响 |
| --- | --- | --- |
| `ASSIGN` | ln | 链接文件或目录 |
| `ATTRIB` | chmod | 更改文件权限 |
| `CD` | cd | 改变目录 |
| `CHDIR` | cd | 改变目录 |
| `CLS` | clear | 清屏 |
| `COMP` | diff, comm, cmp | 文件比较 |
| `COPY` | cp | 文件复制 |
| `Ctl-C` | Ctl-C | 中断（信号） |
| `Ctl-Z` | Ctl-D | 文件结束（EOF） |
| `DEL` | rm | 删除文件（们） |
| `DELTREE` | rm -rf | 递归删除目录 |
| `DIR` | ls -l | 目录列表 |
| `ERASE` | rm | 删除文件（们） |
| `EXIT` | exit | 退出当前进程 |
| `FC` | comm, cmp | 文件比较 |
| `FIND` | grep | 在文件中查找字符串 |
| `MD` | mkdir | 创建目录 |
| `MKDIR` | mkdir | 创建目录 |
| `MORE` | more | 文本文件分页过滤器 |
| `MOVE` | mv | 移动 |
| `PATH` | $PATH | 可执行文件路径 |
| `REN` | mv | 重命名（移动） |
| `RENAME` | mv | 重命名（移动） |
| `RD` | rmdir | 删除目录 |
| `RMDIR` | rmdir | 删除目录 |
| `SORT` | sort | 排序文件 |
| `TIME` | date | 显示系统时间 |
| `TYPE` | cat | 输出文件到`stdout` |
| `XCOPY` | cp | （扩展）文件复制 |
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 几乎所有 UNIX 和 shell 操作员和命令都比它们的 DOS 和批处理文件对应物有更多的选项和增强功能。许多 DOS 批处理文件依赖于辅助工具，例如**ask.com**，这是 read 的受损对应物。DOS 只支持一个非常有限且不兼容的文件名通配符扩展子集，仅识别*和?字符。 |

将 DOS 批处理文件转换为 shell 脚本通常很简单，而且结果往往比原始文件读起来更好。

**示例 N-1\. VIEWDATA.BAT: DOS 批处理文件**

```sh
REM VIEWDATA

REM INSPIRED BY AN EXAMPLE IN "DOS POWERTOOLS"
REM                           BY PAUL SOMERSON

@ECHO OFF

IF !%1==! GOTO VIEWDATA
REM  IF NO COMMAND-LINE ARG...
FIND "%1" C:\BOZO\BOOKLIST.TXT
GOTO EXIT0
REM  PRINT LINE WITH STRING MATCH, THEN EXIT.

:VIEWDATA
TYPE C:\BOZO\BOOKLIST.TXT &#124; MORE
REM  SHOW ENTIRE FILE, 1 PAGE AT A TIME.

:EXIT0
```

脚本转换是一种某种程度的改进。[[1]](#FTN.AEN24713)

**示例 N-2\. *viewdata.sh*: VIEWDATA.BAT 的 Shell 脚本转换**

```sh
#!/bin/bash
# viewdata.sh
# Conversion of VIEWDATA.BAT to shell script.

DATAFILE=/home/bozo/datafiles/book-collection.data
ARGNO=1

# @ECHO OFF                 Command unnecessary here.

if [ $# -lt "$ARGNO" ]    # IF !%1==! GOTO VIEWDATA
then
  less $DATAFILE          # TYPE C:\MYDIR\BOOKLIST.TXT &#124; MORE
else
  grep "$1" $DATAFILE     # FIND "%1" C:\MYDIR\BOOKLIST.TXT
fi  

exit 0                    # :EXIT0

#  GOTOs, labels, smoke-and-mirrors, and flimflam unnecessary.
#  The converted script is short, sweet, and clean,
#+ which is more than can be said for the original.
```

Ted Davis 的[PC 上的 Shell 脚本](http://www.maem.umr.edu/batch/)网站有一套关于旧式批处理文件编程艺术的全面教程。不幸的是，该页面已经无影无踪。

### 注意事项

| [[1]](dosbatch.html#AEN24713) | 众多读者建议对上述批处理文件进行修改以使其更美观、更紧凑和更高效。在*ABS 指南*作者的看法中，这是徒劳的努力。Bash 脚本可以访问 DOS 文件系统，甚至可以通过[ntfs-3g](http://www.ntfs-3g.org)帮助访问 NTFS 分区来进行批处理或脚本操作。 |
| --- | --- |
