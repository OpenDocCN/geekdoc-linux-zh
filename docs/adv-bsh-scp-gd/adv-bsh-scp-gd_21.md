# 第十七章。系统和管理命令

> 原文：[`tldp.org/LDP/abs/html/system.html`](https://tldp.org/LDP/abs/html/system.html)

`/etc/rc.d`中的启动和关闭脚本说明了许多这些命令的用途（及其有用性）。这些通常由**root**调用，用于系统维护或紧急文件系统修复。使用时请谨慎，因为一些命令如果误用可能会损坏您的系统。

**用户和组**

**users**

显示所有已登录用户。这大致等同于**who -q**。

**groups**

列出当前用户及其所属的组。这对应于$GROUPS 内部变量，但给出的是组名，而不是数字。

```sh
bash$ **groups**
bozita cdrom cdwriter audio xgrp

bash$ **echo $GROUPS**
501
```

**chown**、**chgrp**

**chown**命令更改文件或文件的所有权。这是 root 可以将文件所有权从一个用户转移到另一个用户的有用方法。普通用户不能更改文件的所有权，甚至不能更改自己的文件所有权。1

```sh
root# **chown bozo *.txt**

```

**chgrp**命令更改文件或文件的**组**所有权。您必须既是文件的所有者也是目标组的成员（或**root**），才能使用此操作。

```sh
chgrp --recursive dunderheads *.data
#  The "dunderheads" group will now own all the "*.data" files
#+ all the way down the $PWD directory tree (that's what "recursive" means).
```

**useradd**、**userdel**

**useradd**管理命令将用户账户添加到系统，并在指定的情况下为该特定用户创建家目录。相应的**userdel**命令从系统中删除用户账户[2]并删除相关文件。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | **adduser**命令是**useradd**的同义词，通常是一个指向它的符号链接。 |
| --- | --- |

**usermod**

修改用户账户。可以对指定用户的密码、组成员资格、到期日期和其他属性进行更改。使用此命令，可以锁定用户的密码，从而禁用账户。

**groupmod**

修改指定的组。可以使用此命令更改组名和/或 ID 号。

**id**

**id**命令列出与当前进程关联的用户的真实用户 ID、有效用户 ID 和组 ID。这是$UID、$EUID 和$GROUPS 内部 Bash 变量的对应物。

```sh
bash$ **id**
uid=501(bozo) gid=501(bozo) groups=501(bozo),22(cdrom),80(cdwriter),81(audio)

bash$ **echo $UID**
501
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | **id**命令仅在有效 ID 与真实 ID 不同时显示有效 ID。 |

也请参阅示例 9-5。

**lid**

*lid*（列表 ID）命令显示给定用户所属的组，或者，也可以显示属于给定组的用户。只能由 root 调用。

```sh
root# **lid bozo**
 bozo(gid=500)

root# **lid daemon**
 bin(gid=1)
  daemon(gid=2)
  adm(gid=4)
  lp(gid=7)

```

**who**

显示系统上所有已登录的用户。

```sh
bash$ **who**
bozo  tty1     Apr 27 17:45
 bozo  pts/0    Apr 27 17:46
 bozo  pts/1    Apr 27 17:47
 bozo  pts/2    Apr 27 17:49

```

`-m` 仅提供当前用户详细的信息。向 **who** 命令传递任意两个参数等同于 **who -m**，例如 **who am i** 或 **who The Man**。

```sh
bash$ **who -m**
localhost.localdomain!bozo  pts/2    Apr 27 17:49

```

**whoami** 与 **who -m** 类似，但仅列出用户名。

```sh
bash$ **whoami**
bozo

```

**w**

显示所有登录用户及其进程。这是 **who** 命令的扩展版本。**w** 命令的输出可以管道到 grep 以查找特定的用户和/或进程。

```sh
bash$ **w &#124; grep startx**
bozo  tty1     -                 4:22pm  6:41   4.47s  0.45s  startx
```

**logname**

显示当前用户的登录名称（如在 `/var/run/utmp` 中找到的），这是上面提到的 whoami 的近似等效。

```sh
bash$ **logname**
bozo

bash$ **whoami**
bozo
```

然而……

```sh
bash$ **su**
Password: ......

bash# **whoami**
root
bash# **logname**
bozo
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 虽然 **logname** 打印登录用户的名称，但 **whoami** 提供的是当前进程关联的用户的名称。正如我们刚才看到的，有时这些并不相同。 |

**su**

以 **s**ubstitute **u**ser 的身份运行程序或脚本。**su rjones** 以用户 *rjones* 的身份启动 shell。裸露的 **su** 默认为 *root*。请参阅 示例 A-14。

**sudo**

以 *root*（或另一个用户）的身份运行命令。这可以在脚本中使用，从而允许普通用户运行脚本。

```sh
#!/bin/bash

# Some commands.
sudo cp /root/secretfile /home/bozo/secret
# Some more commands.
```

文件 `/etc/sudoers` 存储了被允许调用 **sudo** 的用户名称。

**passwd**

设置、更改或管理用户的密码。

**passwd** 命令可以在脚本中使用，但可能 *不应该* 这样做。

**示例 17-1\. 设置新密码**

```sh
#!/bin/bash
#  setnew-password.sh: For demonstration purposes only.
#                      Not a good idea to actually run this script.
#  This script must be run as root.

ROOT_UID=0         # Root has $UID 0.
E_WRONG_USER=65    # Not root?

E_NOSUCHUSER=70
SUCCESS=0

if [ "$UID" -ne "$ROOT_UID" ]
then
  echo; echo "Only root can run this script."; echo
  exit $E_WRONG_USER
else
  echo
  echo "You should know better than to run this script, root."
  echo "Even root users get the blues... "
  echo
fi  

username=bozo
NEWPASSWORD=security_violation

# Check if bozo lives here.
grep -q "$username" /etc/passwd
if [ $? -ne $SUCCESS ]
then
  echo "User $username does not exist."
  echo "No password changed."
  exit $E_NOSUCHUSER
fi  

echo "$NEWPASSWORD" &#124; passwd --stdin "$username"
#  The '--stdin' option to 'passwd' permits
#+ getting a new password from stdin (or a pipe).

echo; echo "User $username's password changed!"

# Using the 'passwd' command in a script is dangerous.

exit 0
```

**passwd** 命令的 `-l`、`-u` 和 `-d` 选项允许锁定、解锁和删除用户的密码。只有 *root* 可以使用这些选项。

**ac**

显示用户的登录时间，从 `/var/log/wtmp` 中读取。这是 GNU 记账工具之一。

```sh
bash$ **ac**
 total       68.08
```

**last**

列出最后登录的用户，从 `/var/log/wtmp` 中读取。此命令还可以显示远程登录。

例如，要显示系统最近几次重启的时间：

```sh
bash$ **last reboot**
reboot   system boot  2.6.9-1.667      Fri Feb  4 18:18          (00:02)    
 reboot   system boot  2.6.9-1.667      Fri Feb  4 15:20          (01:27)    
 reboot   system boot  2.6.9-1.667      Fri Feb  4 12:56          (00:49)    
 reboot   system boot  2.6.9-1.667      Thu Feb  3 21:08          (02:17)    
 . . .

 wtmp begins Tue Feb  1 12:50:09 2005
```

**newgrp**

不注销即可更改用户的 *组 ID*。这允许访问新组的文件。由于用户可能同时是多个组的成员，此命令的使用范围有限。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 库尔特·格拉泽曼指出，*newgrp* 命令可能有助于为用户写入的文件设置默认组权限。然而，chgrp 命令可能更适合此目的。 |
| --- | --- |

**终端**

**tty**

反射当前用户终端的名称（文件名）。请注意，每个单独的 *xterm* 窗口都被视为不同的终端。

```sh
bash$ **tty**
/dev/pts/1
```

**stty**

显示和/或更改终端设置。这个复杂的命令在脚本中使用可以控制终端行为和输出显示的方式。请参阅 info 页面，并仔细研究。

**示例 17-2\. 设置 *擦除* 字符**

```sh
#!/bin/bash
# erase.sh: Using "stty" to set an erase character when reading input.

echo -n "What is your name? "
read name                      #  Try to backspace
                               #+ to erase characters of input.
                               #  Problems?
echo "Your name is $name."

stty erase '#'                 #  Set "hashmark" (#) as erase character.
echo -n "What is your name? "
read name                      #  Use # to erase last character typed.
echo "Your name is $name."

exit 0

# Even after the script exits, the new key value remains set.
# Exercise: How would you reset the erase character to the default value?
```

**示例 17-3\. *秘密密码*：关闭终端回显**

```sh
#!/bin/bash
# secret-pw.sh: secret password

echo
echo -n "Enter password "
read passwd
echo "password is $passwd"
echo -n "If someone had been looking over your shoulder, "
echo "your password would have been compromised."

echo && echo  # Two line-feeds in an "and list."

stty -echo    # Turns off screen echo.
#   May also be done with
#   read -sp passwd
#   A big Thank You to Leigh James for pointing this out.

echo -n "Enter password again "
read passwd
echo
echo "password is $passwd"
echo

stty echo     # Restores screen echo.

exit 0

# Do an 'info stty' for more on this useful-but-tricky command.
```

**stty** 的一个创造性用法是检测用户按键（无需按 **ENTER**）。

**示例 17-4\. 按键检测**

```sh
#!/bin/bash
# keypress.sh: Detect a user keypress ("hot keys").

echo

old_tty_settings=$(stty -g)   # Save old settings (why?).
stty -icanon
Keypress=$(head -c1)          # or $(dd bs=1 count=1 2> /dev/null)
                              # on non-GNU systems

echo
echo "Key pressed was \""$Keypress"\"."
echo

stty "$old_tty_settings"      # Restore old settings.

# Thanks, Stephane Chazelas.

exit 0
```

还请参阅 示例 9-3 和 示例 A-43。

|

**终端和模式**

通常，终端在 *规范* 模式下工作。当用户按键时，生成的字符不会立即发送到在该终端实际运行的程序。终端本地有一个缓冲区存储按键。当用户按下 **ENTER** 键时，这会将所有存储的按键发送到运行的程序。终端内甚至还有一个基本的行编辑器。

&#124;  ```sh bash$ **stty -a**
speed 9600 baud; rows 36; columns 96; line = 0;
 intr = ^C; quit = ^\; erase = ^H; kill = ^U; eof = ^D; eol = <undef>; eol2 = <undef>;
 start = ^Q; stop = ^S; susp = ^Z; rprnt = ^R; werase = ^W; lnext = ^V; flush = ^O;
 ...
 isig icanon iexten echo echoe echok -echonl -noflsh -xcase -tostop -echoprt

```  &#124;

使用规范模式，可以重新定义本地终端行编辑器的特殊键。

&#124;  ```sh bash$ **cat > filexxx**
**wha<ctl-W>I<ctl-H>foo bar<ctl-U>hello world<ENTER>**
**<ctl-D>**
bash$ **cat filexxx**
hello world		
bash$ **wc -c < filexxx**
12		

```  &#124;

控制终端的过程只接收 12 个字符（11 个字母字符，加上一个换行符），尽管用户按了 26 个键。

在非规范（"raw"）模式下，每个按键（包括如 **ctl-H** 这样的特殊编辑键）都会立即将一个字符发送到控制进程。

Bash 提示符禁用了 `icanon` 和 `echo`，因为它用自己更复杂的行编辑器替换了基本的终端行编辑器。例如，当你在 Bash 提示符下按 **ctl-A** 时，终端不会回显 **^A**，但 Bash 会收到一个 **\1** 字符，解释它并将光标移到行首。

*Stphane Chazelas*

|

**setterm**

设置某些终端属性。此命令将其终端的 `stdout` 写入一个字符串，该字符串会改变该终端的行为。

```sh
bash$ **setterm -cursor off**
bash$

```

**setterm** 命令可以在脚本中使用，以改变写入 `stdout` 的文本外观，尽管确实有 更好的工具 可以用于此目的。

```sh
setterm -bold on
echo bold hello

setterm -bold off
echo normal hello
```

**tset**

显示或初始化终端设置。这是 **stty** 的一个功能较弱的版本。

```sh
bash$ **tset -r**
Terminal type is xterm-xfree86.
 Kill is control-U (^U).
 Interrupt is control-C (^C).

```

**setserial**

设置或显示串行端口参数。此命令必须由 *root* 运行，通常在系统设置脚本中找到。

```sh
# From /etc/pcmcia/serial script:

IRQ=`setserial /dev/$DEVICE &#124; sed -e 's/.*IRQ: //'`
setserial /dev/$DEVICE irq 0 ; setserial /dev/$DEVICE irq $IRQ
```

**getty**，**agetty**

终端初始化过程使用 **getty** 或 **agetty** 为用户设置登录。这些命令在用户 shell 脚本中不被使用。它们的脚本对应物是 **stty**。

**mesg**

启用或禁用当前用户终端的写访问。禁用访问将防止网络上的其他用户 写入 终端。

| ![提示](img/753d054f4c48fb039314a9e5947964cd.png) | 在编辑文本文件时，突然出现订购披萨的消息可能会相当烦人。在多用户网络中，因此你可能希望在需要避免中断时禁用对您的终端的写访问。 |
| --- | --- |

**wall**

这是 "write all" 的缩写，即向网络中每个登录终端的所有用户发送消息。它主要是一个系统管理员的工具，例如，当警告每个人系统将因问题而很快关闭时很有用（见示例 19-1）。

```sh
bash$ **wall System going down for maintenance in 5 minutes!**
Broadcast message from bozo (pts/1) Sun Jul  8 13:53:27 2001...

 System going down for maintenance in 5 minutes!

```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 如果使用 **mesg** 禁用了特定终端的写入访问，则 **wall** 无法向该终端发送消息。 |

**信息和统计**

**uname**

将系统规范（OS、内核版本等）输出到 `stdout`。使用 `-a` 选项调用时，会提供详细的系统信息（见示例 16-5）。使用 `-s` 选项仅显示操作系统类型。

```sh
bash$ **uname**
Linux

bash$ **uname -s**
Linux

bash$ **uname -a**
Linux iron.bozo 2.6.15-1.2054_FC5 #1 Tue Mar 14 15:48:33 EST 2006
 i686 i686 i386 GNU/Linux
```

**arch**

显示系统架构。等同于**uname -m**。见示例 11-27。

```sh
bash$ **arch**
i686

bash$ **uname -m**
i686
```

**lastcomm**

提供有关存储在`/var/account/pacct`文件中的先前命令的信息。可以通过选项指定命令名和用户名。这是 GNU 会计工具之一。

**lastlog**

列出所有系统用户的最后登录时间。这引用了`/var/log/lastlog`文件。

```sh
bash$ **lastlog**
root          tty1                      Fri Dec  7 18:43:21 -0700 2001
 bin                                     **Never logged in**
 daemon                                  **Never logged in**
 ...
 bozo          tty1                      Sat Dec  8 21:14:29 -0700 2001

bash$ **lastlog &#124; grep root**
root          tty1                      Fri Dec  7 18:43:21 -0700 2001

```
| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 如果调用此命令的用户没有对`/var/log/lastlog`文件的读取权限，则该命令将失败。 |

**lsof**

列出打开的文件。此命令输出所有当前打开文件的详细表格，并提供有关其所有者、大小、与之关联的进程等信息。当然，**lsof**可以管道到 grep 和/或 awk 以解析和分析其结果。

```sh
bash$ **lsof**
COMMAND    PID    USER   FD   TYPE     DEVICE    SIZE     NODE NAME
 init         1    root  mem    REG        3,5   30748    30303 /sbin/init
 init         1    root  mem    REG        3,5   73120     8069 /lib/ld-2.1.3.so
 init         1    root  mem    REG        3,5  931668     8075 /lib/libc-2.1.3.so
 cardmgr    213    root  mem    REG        3,5   36956    30357 /sbin/cardmgr
 ...

```

**lsof** 命令是一个有用的、如果复杂的系统管理工具。如果你无法卸载文件系统并收到错误消息，表明它仍在使用中，那么运行 *lsof* 帮助确定该文件系统上仍打开的哪些文件。`-i` 选项列出打开的网络套接字文件，这有助于追踪入侵或黑客攻击尝试。

```sh
bash$ **lsof -an -i tcp**
COMMAND  PID USER  FD  TYPE DEVICE SIZE NODE NAME
 firefox 2330 bozo  32u IPv4   9956       TCP 66.0.118.137:57596->67.112.7.104:http ...
 firefox 2330 bozo  38u IPv4  10535       TCP 66.0.118.137:57708->216.79.48.24:http ...

```

请参阅示例 30-2 了解**lsof**的有效使用。

**strace**

**S**ystem **trace**：用于跟踪 *系统调用* 和信号的诊断和调试工具。此命令和随后的 **ltrace** 命令对于诊断为什么某个程序或包无法运行很有用……可能是因为缺少库或相关原因。

```sh
bash$ **strace df**
execve("/bin/df", ["df"], [/* 45 vars */]) = 0
 uname({sys="Linux", node="bozo.localdomain", ...}) = 0
 brk(0)                                  = 0x804f5e4

 ...

```

这是 Linux 中 Solaris **truss** 命令的等价物。

**ltrace**

**L**ibrary **trace**：跟踪给定命令调用的 *库调用* 的诊断和调试工具。

```sh
bash$ **ltrace df**
__libc_start_main(0x804a910, 1, 0xbfb589a4, 0x804fb70, 0x804fb68 <unfinished ...>:
 setlocale(6, "")                                 = "en_US.UTF-8"
bindtextdomain("coreutils", "/usr/share/locale") = "/usr/share/locale"
textdomain("coreutils")                          = "coreutils"
__cxa_atexit(0x804b650, 0, 0, 0x8052bf0, 0xbfb58908) = 0
getenv("DF_BLOCK_SIZE")                          = NULL

 ...

```

**nc**

**nc** (*netcat*) 实用程序是一个用于连接到和监听 TCP 和 UDP 端口的完整工具包。它作为诊断和测试工具以及简单脚本客户端和服务器组件很有用。

```sh
bash$ **nc localhost.localdomain 25**
220 localhost.localdomain ESMTP Sendmail 8.13.1/8.13.1;
 Thu, 31 Mar 2005 15:41:35 -0700
```

一个真实的使用示例。

**示例 17-5\. 检查远程服务器上的*identd*** 

```sh
#! /bin/sh
## Duplicate DaveG's ident-scan thingie using netcat. Oooh, he'll be p*ssed.
## Args: target port [port port port ...]
## Hose stdout _and_ stderr together.
##
##  Advantages: runs slower than ident-scan, giving remote inetd less cause
##+ for alarm, and only hits the few known daemon ports you specify.
##  Disadvantages: requires numeric-only port args, the output sleazitude,
##+ and won't work for r-services when coming from high source ports.
# Script author: Hobbit <hobbit@avian.org>
# Used in ABS Guide with permission.

# ---------------------------------------------------
E_BADARGS=65       # Need at least two args.
TWO_WINKS=2        # How long to sleep.
THREE_WINKS=3
IDPORT=113         # Authentication "tap ident" port.
RAND1=999
RAND2=31337
TIMEOUT0=9
TIMEOUT1=8
TIMEOUT2=4
# ---------------------------------------------------

case "${2}" in
  "" ) echo "Need HOST and at least one PORT." ; exit $E_BADARGS ;;
esac

# Ping 'em once and see if they *are* running identd.
nc -z -w $TIMEOUT0 "$1" $IDPORT &#124;&#124; \
{ echo "Oops, $1 isn't running identd." ; exit 0 ; }
#  -z scans for listening daemons.
#     -w $TIMEOUT = How long to try to connect.

# Generate a randomish base port.
RP=`expr $$ % $RAND1 + $RAND2`

TRG="$1"
shift

while test "$1" ; do
  nc -v -w $TIMEOUT1 -p ${RP} "$TRG" ${1} < /dev/null > /dev/null &
  PROC=$!
  sleep $THREE_WINKS
  echo "${1},${RP}" &#124; nc -w $TIMEOUT2 -r "$TRG" $IDPORT 2>&1
  sleep $TWO_WINKS

# Does this look like a lamer script or what . . . ?
# ABS Guide author comments: "Ain't really all that bad . . .
#+                            kinda clever, actually."

  kill -HUP $PROC
  RP=`expr ${RP} + 1`
  shift
done

exit $?

#  Notes:
#  -----

#  Try commenting out line 30 and running this script
#+ with "localhost.localdomain 25" as arguments.

#  For more of Hobbit's 'nc' example scripts,
#+ look in the documentation:
#+ the /usr/share/doc/nc-X.XX/scripts directory.
```

当然，还有 Dr. Andrew Tridgell 在 BitKeeper 事件中臭名昭著的一行脚本：

```sh
echo clone &#124; nc thunk.org 5000 > e2fsprogs.dat
```

**free**

以表格形式显示内存和缓存使用情况。此命令的输出便于使用 grep、awk 或**Perl**进行解析。**procinfo**命令显示了**free**命令的所有信息以及更多。

```sh
bash$ free
 total       used       free     shared    buffers     cached
   Mem:         30504      28624       1880      15820       1608       16376
   -/+ buffers/cache:      10640      19864
   Swap:        68540       3128      65412
```

要显示未使用的 RAM 内存：

```sh
bash$ free &#124; grep Mem &#124; awk '{ print $4 }'
1880
```

**procinfo**

从`/proc`伪文件系统（devproc.html#DEVPROCREF）提取并列出信息和统计数据。这提供了非常广泛和详细的列表。

```sh
bash$ **procinfo &#124; grep Bootup**
Bootup: Wed Mar 21 15:15:50 2001    Load average: 0.04 0.21 0.34 3/47 6829
```

**lsdev**

列出设备，即显示已安装的硬件。

```sh
bash$ **lsdev**
Device            DMA   IRQ  I/O Ports
 ------------------------------------------------
 cascade             4     2 
 dma                          0080-008f
 dma1                         0000-001f
 dma2                         00c0-00df
 fpu                          00f0-00ff
 ide0                     14  01f0-01f7 03f6-03f6
 ...

```

**du**

显示（磁盘）文件使用情况，递归地。默认为当前工作目录，除非指定其他目录。

```sh
bash$ du -ach
1.0k    ./wi.sh
 1.0k    ./tst.sh
 1.0k    ./random.file
 6.0k    .
 6.0k    total
```

**df**

以表格形式显示文件系统使用情况。

```sh
bash$ df
Filesystem           1k-blocks      Used Available Use% Mounted on
 /dev/hda5               273262     92607    166547  36% /
 /dev/hda8               222525    123951     87085  59% /home
 /dev/hda7              1408796   1075744    261488  80% /usr
```

**dmesg**

列出所有系统启动消息到`stdout`。这对于调试和确定已安装的设备驱动程序和正在使用的系统中断非常有用。**dmesg**的输出当然可以使用 grep、sed 或 awk 在脚本中进行解析。

```sh
bash$ **dmesg &#124; grep hda**
Kernel command line: ro root=/dev/hda2
 hda: IBM-DLGA-23080, ATA DISK drive
 hda: 6015744 sectors (3080 MB) w/96KiB Cache, CHS=746/128/63
 hda: hda1 hda2 hda3 < hda5 hda6 hda7 > hda4

```

**stat**

对给定文件（甚至目录或设备文件）或文件集提供详细和冗长的*stat*istics 信息。

```sh
bash$ **stat test.cru**
 File: "test.cru"
   Size: 49970        Allocated Blocks: 100          Filetype: Regular File
   Mode: (0664/-rw-rw-r--)         Uid: (  501/ bozo)  Gid: (  501/ bozo)
 Device:  3,8   Inode: 18185     Links: 1    
 Access: Sat Jun  2 16:40:24 2001
 Modify: Sat Jun  2 16:40:24 2001
 Change: Sat Jun  2 16:40:24 2001

```

如果目标文件不存在，**stat**将返回错误信息。

```sh
bash$ **stat nonexistent-file**
nonexistent-file: No such file or directory

```

在脚本中，你可以使用**stat**提取有关文件（和文件系统）的信息，并相应地设置变量。

```sh
#!/bin/bash
# fileinfo2.sh

# Per suggestion of Jol Bourquard and . . .
# http://www.linuxquestions.org/questions/showthread.php?t=410766

FILENAME=testfile.txt
file_name=$(stat -c%n "$FILENAME")   # Same as "$FILENAME" of course.
file_owner=$(stat -c%U "$FILENAME")
file_size=$(stat -c%s "$FILENAME")
#  Certainly easier than using "ls -l $FILENAME"
#+ and then parsing with sed.
file_inode=$(stat -c%i "$FILENAME")
file_type=$(stat -c%F "$FILENAME")
file_access_rights=$(stat -c%A "$FILENAME")

echo "File name:          $file_name"
echo "File owner:         $file_owner"
echo "File size:          $file_size"
echo "File inode:         $file_inode"
echo "File type:          $file_type"
echo "File access rights: $file_access_rights"

exit 0

sh fileinfo2.sh

File name:          testfile.txt
File owner:         bozo
File size:          418
File inode:         1730378
File type:          regular file
File access rights: -rw-rw-r--
```

**vmstat**

显示虚拟内存统计数据。

```sh
bash$ **vmstat**
 procs                      memory    swap          io system         cpu
 r  b  w   swpd   free   buff  cache  si  so    bi    bo   in    cs  us  sy id
 0  0  0      0  11040   2636  38952   0   0    33     7  271    88   8   3 89

```

**uptime**

显示系统运行时长以及相关统计数据。

```sh
bash$ **uptime**
10:28pm  up  1:57,  3 users,  load average: 0.17, 0.34, 0.27
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 负载平均值为 1 或以下表示系统可以立即处理进程。负载平均值大于 1 表示进程正在排队。当负载平均值超过 3（在单核处理器上）时，系统性能将显著下降。 |

**hostname**

列出系统的主机名。此命令在`/etc/rc.d`设置脚本中设置主机名（`/etc/rc.d/rc.sysinit`或类似）。它与**uname -n**等效，与$HOSTNAME 内部变量相对应。

```sh
bash$ **hostname**
localhost.localdomain

bash$ **echo $HOSTNAME**
localhost.localdomain
```

与**hostname**命令类似的是**domainname**、**dnsdomainname**、**nisdomainname**和**ypdomainname**命令。使用这些命令显示或设置系统 DNS 或 NIS/YP 域名。**hostname**的某些选项也执行这些功能。

**hostid**

输出主机机的 32 位十六进制数值标识符。

```sh
bash$ **hostid**
7f0100
```

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 此命令据说为特定系统获取一个“唯一”的序列号。某些产品注册程序使用此数字来标记特定的用户许可证。不幸的是，**hostid**只返回机器的网络地址，以十六进制形式表示，字节对进行了转置。典型非网络化 Linux 机器的网络地址可以在`/etc/hosts`中找到。

&#124;  ```sh bash$ **cat /etc/hosts**
127.0.0.1               localhost.localdomain localhost
```  &#124;

如同所发生的那样，将`**127.0.0.1**`的字节进行转置，我们得到`**0.127.1.0**`，这在十六进制中转换为`**007f0100**`，这与上面**hostid**返回的值完全相同。只有几百万台其他 Linux 机器具有这个相同的*hostid*。

**sar**

调用**sar**（系统活动报告）会提供非常详细的系统统计信息。圣克拉拉操作公司（“旧”SCO）于 1999 年 6 月将**sar**作为开源软件发布。

此命令不是 Linux 基础发行版的一部分，但可以作为[sysstat 实用程序](http://perso.wanadoo.fr/sebastien.godard/)包的一部分获得，该包由 Sebastien Godard 编写。

```sh
bash$ **sar**
Linux 2.4.9 (brooks.seringas.fr) 	09/26/03

10:30:00          CPU     %user     %nice   %system   %iowait     %idle
10:40:00          all      2.21     10.90     65.48      0.00     21.41
10:50:00          all      3.36      0.00     72.36      0.00     24.28
11:00:00          all      1.12      0.00     80.77      0.00     18.11
Average:          all      2.23      3.63     72.87      0.00     21.27

14:32:30          LINUX RESTART

15:00:00          CPU     %user     %nice   %system   %iowait     %idle
15:10:00          all      8.59      2.40     17.47      0.00     71.54
15:20:00          all      4.07      1.00     11.95      0.00     82.98
15:30:00          all      0.79      2.94      7.56      0.00     88.71
Average:          all      6.33      1.70     14.71      0.00     77.26

```

**readelf**

显示指定*elf*二进制文件的信息和统计信息。这是*binutils*包的一部分。

```sh
bash$ **readelf -h /bin/bash**
ELF Header:
   Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00 
   Class:                             ELF32
   Data:                              2's complement, little endian
   Version:                           1 (current)
   OS/ABI:                            UNIX - System V
   ABI Version:                       0
   Type:                              EXEC (Executable file)
   . . .
```

**size**

**size [/path/to/binary]**命令给出二进制可执行文件或归档文件的段大小。这主要对程序员有用。

```sh
bash$ **size /bin/bash**
 text    data     bss     dec     hex filename
  495971   22496   17392  535859   82d33 /bin/bash

```

**系统日志**

**logger**

将用户生成的消息追加到系统日志（`/var/log/messages`）。调用**logger**不需要是*root*用户。

```sh
logger Experiencing instability in network connection at 23:10, 05/21.
# Now, do a 'tail /var/log/messages'.
```

通过在脚本中嵌入**logger**命令，可以将调试信息写入`/var/log/messages`。

```sh
logger -t $0 -i Logging at line "$LINENO".
# The "-t" option specifies the tag for the logger entry.
# The "-i" option records the process ID.

# tail /var/log/message
# ...
# Jul  7 20:48:58 localhost ./test.sh[1712]: Logging at line 3.
```

**logrotate**

此实用程序管理系统日志文件，根据需要旋转、压缩、删除和/或通过电子邮件发送它们。这可以防止`/var/log`目录因旧日志文件而变得杂乱。通常，cron 每天运行**logrotate**。

在`/etc/logrotate.conf`中添加适当的条目，可以管理个人日志文件以及系统范围内的日志文件。

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | Stefano Falsetto 创建了[rottlog](http://www.gnu.org/software/rottlog/)，他认为这是**logrotate**的改进版本。 |
| --- | --- |

**作业控制**

**ps**

`*P*`rocess `*S*`tatistics：按所有者和 PID（进程 ID）列出当前正在执行的过程。通常使用`ax`或`aux`选项调用此命令，并且可以将输出通过管道传递给 grep 或 sed 以搜索特定进程（参见示例 15-14 和示例 29-3）。

```sh
bash$  **ps ax &#124; grep sendmail**
295 ?	   S	  0:00 sendmail: accepting connections on port 25
```

要以图形“树”格式显示系统进程：**ps afjx**或**ps ax --forest**。

**pgrep**, **pkill**

将**ps**命令与 grep 或 kill 结合使用。

```sh
bash$ **ps a &#124; grep mingetty**
2212 tty2     Ss+    0:00 /sbin/mingetty tty2
 2213 tty3     Ss+    0:00 /sbin/mingetty tty3
 2214 tty4     Ss+    0:00 /sbin/mingetty tty4
 2215 tty5     Ss+    0:00 /sbin/mingetty tty5
 2216 tty6     Ss+    0:00 /sbin/mingetty tty6
 4849 pts/2    S+     0:00 grep mingetty

bash$ **pgrep mingetty**
2212 mingetty
 2213 mingetty
 2214 mingetty
 2215 mingetty
 2216 mingetty

```

将**pkill**的行为与 killall 进行比较。

**pstree**

以“树”格式列出当前正在执行的过程。`-p`选项显示 PID 以及进程名称。

**top**

显示大多数 CPU 密集型进程的持续更新显示。`-b`选项以文本模式显示，因此输出可以被解析或从脚本中访问。

```sh
bash$ **top -b**
 8:30pm  up 3 min,  3 users,  load average: 0.49, 0.32, 0.13
 45 processes: 44 sleeping, 1 running, 0 zombie, 0 stopped
 CPU states: 13.6% user,  7.3% system,  0.0% nice, 78.9% idle
 Mem:    78396K av,   65468K used,   12928K free,       0K shrd,    2352K buff
 Swap:  157208K av,       0K used,  157208K free                   37244K cached

   PID USER     PRI  NI  SIZE  RSS SHARE STAT %CPU %MEM   TIME COMMAND
   848 bozo      17   0   996  996   800 R     5.6  1.2   0:00 top
     1 root       8   0   512  512   444 S     0.0  0.6   0:04 init
     2 root       9   0     0    0     0 SW    0.0  0.0   0:00 keventd
   ...  

```

**nice**

以更改后的优先级运行后台作业。优先级从 19（最低）到-20（最高）。只有*root*可以设置负（更高）优先级。相关的命令有**renice**和**snice**，它们可以改变运行进程或进程的优先级，以及**skill**，它向进程或进程发送 kill 信号。

**nohup**

即使用户注销后，也能保持命令运行。除非跟在&后面，否则命令将以前台进程运行。如果你在脚本中使用**nohup**，考虑与 wait 结合使用，以避免创建*孤儿*或僵尸进程。

**pidof**

识别正在运行的作业的*进程 ID (PID)*。由于作业控制命令，如 kill 和 renice 作用于进程的*PID*（而不是其名称），有时需要识别该*PID*。**pidof**命令是内部变量$PPID 的大致对应物。

```sh
bash$ **pidof xclock**
880

```

**Example 17-6\. *pidof* helps kill a process**

```sh
#!/bin/bash
# kill-process.sh

NOPROCESS=2

process=xxxyyyzzz  # Use nonexistent process.
# For demo purposes only...
# ... don't want to actually kill any actual process with this script.
#
# If, for example, you wanted to use this script to logoff the Internet,
#     process=pppd

t=`pidof $process`       # Find pid (process id) of $process.
# The pid is needed by 'kill' (can't 'kill' by program name).

if [ -z "$t" ]           # If process not present, 'pidof' returns null.
then
  echo "Process $process was not running."
  echo "Nothing killed."
  exit $NOPROCESS
fi  

kill $t                  # May need 'kill -9' for stubborn process.

# Need a check here to see if process allowed itself to be killed.
# Perhaps another " t=`pidof $process` " or ...

# This entire script could be replaced by
#        kill $(pidof -x process_name)
# or
#        killall process_name
# but it would not be as instructive.

exit 0
```

**fuser**

识别访问给定文件、文件集或目录的进程（通过 PID）。也可以使用`-k`选项调用，该选项会终止这些进程。这对于系统安全有有趣的含义，尤其是在脚本中防止未经授权的用户访问系统服务时。

```sh
bash$ **fuser -u /usr/bin/vim**
/usr/bin/vim:         3207e(bozo)

bash$ **fuser -u /dev/null**
/dev/null:            3009(bozo)  3010(bozo)  3197(bozo)  3199(bozo)

```

**fuser**的一个重要应用是在物理插入或移除存储介质时，例如 CD ROM 光盘或 USB 闪存驱动器。有时尝试 umount 会失败，显示设备忙碌的错误信息。这意味着某些用户和/或进程正在访问该设备。使用**fuser -um /dev/device_name**可以解开这个谜团，因此你可以终止任何相关进程。

```sh
bash$ **umount /mnt/usbdrive**
umount: /mnt/usbdrive: device is busy

bash$ **fuser -um /dev/usbdrive**
/mnt/usbdrive:        1772c(bozo)

bash$ **kill -9 1772**
bash$ **umount /mnt/usbdrive**

```

使用`-n`选项调用的**fuser**命令可以识别访问*端口*的进程。这特别适用于与 nmap 结合使用。

```sh
root# **nmap localhost.localdomain**
PORT     STATE SERVICE
 25/tcp   open  smtp

root# **fuser -un tcp 25**
25/tcp:               2095(root)

root# **ps ax &#124; grep 2095 &#124; grep -v grep**
2095 ?        Ss     0:00 sendmail: accepting connections

```

**cron**

管理程序调度器，执行诸如清理和删除系统日志文件以及更新 slocate 数据库等任务。这是 at（尽管每个用户可能都有自己的`crontab`文件，可以使用**crontab**命令进行更改）的*超级用户*版本。它作为守护进程运行并执行`/etc/crontab`中的计划条目。

| ![Note](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 一些 Linux 版本运行**crond**，这是 Matthew Dillon 的**cron**版本。 |
| --- | --- |

**Process Control and Booting**

**init**

**init** 命令是所有进程的 父进程。在启动的最后一步被调用，**init** 从 `/etc/inittab` 确定系统的运行级别。通过其别名 **telinit** 和仅由 *root* 调用。

**telinit**

与 **init** 链接，这是一种更改系统运行级别的手段，通常用于系统维护或紧急文件系统修复。仅由 *root* 调用。这个命令可能很危险——在使用之前务必确保你完全理解它！

**runlevel**

显示当前和最后的运行级别，即系统是否已停止（运行级别 `0`），是否处于单用户模式（`1`），多用户模式（`2` 或 `3`），X Windows（`5`），或正在重启（`6`）。此命令访问 `/var/run/utmp` 文件。

**halt**、**shutdown**、**reboot**

用于关闭系统的命令集，通常在断电之前执行。

| ![Warning](img/9d1b374742c319dd43bb1c5d95d6e008.png) | 在某些 Linux 发行版中，**halt** 命令具有 755 权限，因此可以被非 root 用户调用。在终端或脚本中不小心执行 **halt** 命令可能会导致系统关闭！ |
| --- | --- |

**service**

启动或停止系统 *服务*。`/etc/init.d` 和 `/etc/rc.d` 中的启动脚本使用此命令在启动时启动服务。

```sh
root# **/sbin/service iptables stop**
Flushing firewall rules:                                   [  OK  ]
 Setting chains to policy ACCEPT: filter                    [  OK  ]
 Unloading iptables modules:                                [  OK  ]

```

**Network**

**nmap**

**N**etwork **map**per 和端口扫描器。此命令扫描服务器以定位开放的端口和与这些端口关联的服务。它还可以报告有关数据包过滤器和防火墙的信息。这是锁定网络以防止黑客攻击的重要安全工具。

```sh
#!/bin/bash

SERVER=$HOST                           # localhost.localdomain (127.0.0.1).
PORT_NUMBER=25                         # SMTP port.

nmap $SERVER &#124; grep -w "$PORT_NUMBER"  # Is that particular port open?
#              grep -w matches whole words only,
#+             so this wouldn't match port 1025, for example.

exit 0

# 25/tcp     open        smtp
```

**ifconfig**

网络接口配置和调整实用程序。

```sh
bash$ **ifconfig -a**
lo        Link encap:Local Loopback
           inet addr:127.0.0.1  Mask:255.0.0.0
           UP LOOPBACK RUNNING  MTU:16436  Metric:1
           RX packets:10 errors:0 dropped:0 overruns:0 frame:0
           TX packets:10 errors:0 dropped:0 overruns:0 carrier:0
           collisions:0 txqueuelen:0 
           RX bytes:700 (700.0 b)  TX bytes:700 (700.0 b)
```

**ifconfig** 命令通常在启动时用于设置接口，或在重启时关闭它们。

```sh
# Code snippets from /etc/rc.d/init.d/network

# ...

# Check that networking is up.
[ ${NETWORKING} = "no" ] && exit 0

[ -x /sbin/ifconfig ] &#124;&#124; exit 0

# ...

for i in $interfaces ; do
  if ifconfig $i 2>/dev/null &#124; grep -q "UP" >/dev/null 2>&1 ; then
    action "Shutting down interface $i: " ./ifdown $i boot
  fi
#  The GNU-specific "-q" option to "grep" means "quiet", i.e.,
#+ producing no output.
#  Redirecting output to /dev/null is therefore not strictly necessary.

# ...

echo "Currently active devices:"
echo `/sbin/ifconfig &#124; grep ^[a-z] &#124; awk '{print $1}'`
#                            ^^^^^  should be quoted to prevent globbing.
#  The following also work.
#    echo $(/sbin/ifconfig &#124; awk '/^[a-z]/ { print $1 })'
#    echo $(/sbin/ifconfig &#124; sed -e 's/ .*//')
#  Thanks, S.C., for additional comments.
```

参见 示例 32-6。

**netstat**

显示当前网络统计信息和信息，例如路由表和活动连接。此实用程序访问 `/proc/net` 中的信息（第二十九章）。参见 示例 29-4。

**netstat -r** 与 route 相当。

```sh
bash$ **netstat**
Active Internet connections (w/o servers)
 Proto Recv-Q Send-Q Local Address           Foreign Address         State      
 Active UNIX domain sockets (w/o servers)
 Proto RefCnt Flags       Type       State         I-Node Path
 unix  11     [ ]         DGRAM                    906    /dev/log
 unix  3      [ ]         STREAM     CONNECTED     4514   /tmp/.X11-unix/X0
 unix  3      [ ]         STREAM     CONNECTED     4513
 . . .
```
| ![Note](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 一个 **netstat -lptu** 命令显示了正在监听端口的 套接字 以及相关的进程。这有助于确定计算机是否被黑客攻击或被破坏。 |

**iwconfig**

这是配置无线网络的命令集。它是上面 **ifconfig** 的无线等效命令。

**ip**

通用实用程序，用于设置、更改和分析 *IP*（互联网协议）网络和连接的设备。此命令是 *iproute2* 软件包的一部分。

```sh
bash$ **ip link show**
1: lo: <LOOPBACK,UP> mtu 16436 qdisc noqueue 
     link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
 2: eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast qlen 1000
     link/ether 00:d0:59:ce:af:da brd ff:ff:ff:ff:ff:ff
 3: sit0: <NOARP> mtu 1480 qdisc noop 
     link/sit 0.0.0.0 brd 0.0.0.0

bash$ **ip route list**
169.254.0.0/16 dev lo  scope link

```

或者，在脚本中：

```sh
#!/bin/bash
# Script by Juan Nicolas Ruiz
# Used with his kind permission.

# Setting up (and stopping) a GRE tunnel.

# --- start-tunnel.sh ---

LOCAL_IP="192.168.1.17"
REMOTE_IP="10.0.5.33"
OTHER_IFACE="192.168.0.100"
REMOTE_NET="192.168.3.0/24"

/sbin/ip tunnel add netb mode gre remote $REMOTE_IP \
  local $LOCAL_IP ttl 255
/sbin/ip addr add $OTHER_IFACE dev netb
/sbin/ip link set netb up
/sbin/ip route add $REMOTE_NET dev netb

exit 0  #############################################

# --- stop-tunnel.sh ---

REMOTE_NET="192.168.3.0/24"

/sbin/ip route del $REMOTE_NET dev netb
/sbin/ip link set netb down
/sbin/ip tunnel del netb

exit 0
```

**route**

显示关于内核路由表的信息或对其进行更改。

```sh
bash$ **route**
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
 pm3-67.bozosisp *               255.255.255.255 UH       40 0          0 ppp0
 127.0.0.0       *               255.0.0.0       U        40 0          0 lo
 default         pm3-67.bozosisp 0.0.0.0         UG       40 0          0 ppp0

```

**iptables**

**iptables** 命令集是一个用于设置网络防火墙等安全目的的包过滤工具。这是一个复杂的工具，其详细使用说明超出了本文档的范围。[Oskar Andreasson 的教程](http://www.frozentux.net/iptables-tutorial/iptables-tutorial.html)是一个合理的起点。

参见 关闭 *iptables* 和 示例 30-2。

**chkconfig**

检查网络和系统配置。此命令列出并管理在 `/etc/rc?.d` 目录中启动的网络和系统服务。

**chkconfig** 最初是从 IRIX 移植到 Red Hat Linux 的，可能不是某些 Linux 版本的内核安装的一部分。

```sh
bash$ **chkconfig --list**
atd             0:off   1:off   2:off   3:on    4:on    5:on    6:off
 rwhod           0:off   1:off   2:off   3:off   4:off   5:off   6:off
 ...

```

**tcpdump**

网络数据包“嗅探器”。这是一个通过转储匹配指定标准的包头来分析和排除网络故障的工具。

在主机 *bozoville* 和 *caduceus* 之间转储 IP 数据包流量：

```sh
bash$ **tcpdump ip host bozoville and caduceus**

```

当然，**tcpdump** 的输出可以用之前讨论过的某些 文本处理工具 进行解析。

**文件系统**

**mount**

挂载文件系统，通常是在外部设备上，如软盘或 CDROM。`/etc/fstab` 文件提供了一个方便的可用文件系统、分区和设备的列表，包括可自动或手动挂载的选项。`/etc/mtab` 文件显示了当前挂载的文件系统和分区（包括虚拟的，如 `/proc`）。

**mount -a** 挂载 `/etc/fstab` 中列出的所有文件系统和分区，除了那些带有 `noauto` 选项的。在启动时，`/etc/rc.d` 目录中的启动脚本（`rc.sysinit` 或类似）调用此命令以挂载所有内容。

```sh
mount -t iso9660 /dev/cdrom /mnt/cdrom
# Mounts CD ROM. ISO 9660 is a standard CD ROM filesystem.
mount /mnt/cdrom
# Shortcut, if /mnt/cdrom listed in /etc/fstab
```

多功能的 *mount* 命令甚至可以将普通文件挂载到块设备上，该文件将表现得像是一个文件系统。*Mount* 通过将文件与一个 回环设备 关联来实现这一点。一个应用实例是在将 ISO9660 文件系统镜像烧录到 CDR 之前挂载并检查它。3

**示例 17-7\. 检查 CD 图像**

```sh
# As root...

mkdir /mnt/cdtest  # Prepare a mount point, if not already there.

mount -r -t iso9660 -o loop cd-image.iso /mnt/cdtest   # Mount the image.
#                  "-o loop" option equivalent to "losetup /dev/loop0"
cd /mnt/cdtest     # Now, check the image.
ls -alR            # List the files in the directory tree there.
                   # And so forth.
```

**umount**

卸载当前挂载的文件系统。在物理移除之前已挂载的软盘或 CDROM 光盘之前，必须先 **umount**，否则可能会导致文件系统损坏。

```sh
umount /mnt/cdrom
# You may now press the eject button and safely remove the disk.
```
| ![Note](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 如果正确安装，**automount** 实用程序可以挂载和卸载软盘或 CDROM 光盘，当它们被访问或移除时。然而，在具有可交换软盘和光驱的“多磁头”笔记本电脑上，这可能会引起问题。 |

**gnome-mount**

新的 Linux 发行版已经弃用了 **mount** 和 **umount**。其继任者，用于命令行挂载可移动存储设备的是 **gnome-mount**。它可以采用 `-d` 选项通过 `/dev` 中的列表来挂载一个 设备文件。

例如，要挂载一个 USB 闪存驱动器：

```sh
bash$ **gnome-mount -d /dev/sda1**
gnome-mount 0.4

bash$ **df**
. . .
 /dev/sda1                63584     12034     51550  19% /media/disk

```

**sync**

强制将所有更新的数据从缓冲区写入硬盘（同步硬盘与缓冲区）。虽然不是绝对必要的，但一个 **sync** 命令可以确保系统管理员或用户，刚刚更改的数据在突然断电的情况下能够幸存。在旧时代，一个 `**sync; sync**`（两次，只是为了绝对确保）在系统重启前是一个有用的预防措施。

有时，您可能希望强制立即缓冲区刷新，例如在安全删除文件（参见 示例 16-61）或当灯光开始闪烁时。

**losetup**

设置和配置 回环设备。

**示例 17-8\. 在文件中创建文件系统**

```sh
SIZE=1000000  # 1 meg

head -c $SIZE < /dev/zero > file  # Set up file of designated size.
losetup /dev/loop0 file           # Set it up as loopback device.
mke2fs /dev/loop0                 # Create filesystem.
mount -o loop /dev/loop0 /mnt     # Mount it.

# Thanks, S.C.
```

**mkswap**

创建一个交换分区或文件。随后必须使用 **swapon** 启用交换区域。

**swapon**, **swapoff**

启用/禁用交换分区或文件。这些命令通常在启动和关闭时生效。

**mke2fs**

创建一个 Linux *ext2* 文件系统。此命令必须以 *root* 身份调用。

**示例 17-9\. 添加新的硬盘**

```sh
#!/bin/bash

# Adding a second hard drive to system.
# Software configuration. Assumes hardware already mounted.
# From an article by the author of the ABS Guide.
# In issue #38 of _Linux Gazette_, http://www.linuxgazette.com.

ROOT_UID=0     # This script must be run as root.
E_NOTROOT=67   # Non-root exit error.

if [ "$UID" -ne "$ROOT_UID" ]
then
  echo "Must be root to run this script."
  exit $E_NOTROOT
fi  

# Use with extreme caution!
# If something goes wrong, you may wipe out your current filesystem.

NEWDISK=/dev/hdb         # Assumes /dev/hdb vacant. Check!
MOUNTPOINT=/mnt/newdisk  # Or choose another mount point.

fdisk $NEWDISK
mke2fs -cv $NEWDISK1   # Check for bad blocks (verbose output).
#  Note:           ^     /dev/hdb1, *not* /dev/hdb!
mkdir $MOUNTPOINT
chmod 777 $MOUNTPOINT  # Makes new drive accessible to all users.

# Now, test ...
# mount -t ext2 /dev/hdb1 /mnt/newdisk
# Try creating a directory.
# If it works, umount it, and proceed.

# Final step:
# Add the following line to /etc/fstab.
# /dev/hdb1  /mnt/newdisk  ext2  defaults  1 1

exit
```

参见 示例 17-8 和 示例 31-3。

**mkdosfs**

创建一个 DOS *FAT* 文件系统。

**tune2fs**

调整 *ext2* 文件系统。可能用于更改文件系统参数，例如最大挂载次数。此命令必须以 *root* 身份调用。

| ![警告](img/9d1b374742c319dd43bb1c5d95d6e008.png) | 这是一个极其危险的命令。使用时请自行承担风险，因为您可能会意外地破坏您的文件系统。 |
| --- | --- |

**dumpe2fs**

将非常详细的文件系统信息输出到 `stdout`。此命令必须以 *root* 身份调用。

```sh
root# dumpe2fs /dev/hda7 &#124; grep 'ount count'
dumpe2fs 1.19, 13-Jul-2000 for EXT2 FS 0.5b, 95/08/09
 Mount count:              6
 Maximum mount count:      20
```

**hdparm**

列出或更改硬盘参数。此命令必须以 *root* 身份调用，并且如果误用可能很危险。

**fdisk**

在存储设备上创建或更改分区表，通常是硬盘。此命令必须以 *root* 身份调用。

| ![警告](img/9d1b374742c319dd43bb1c5d95d6e008.png) | 使用此命令时请极其小心。如果出现问题，您可能会破坏现有的文件系统。 |
| --- | --- |

**fsck**, **e2fsck**, **debugfs**

文件系统检查、修复和调试命令集。

**fsck**：检查 UNIX 文件系统的前端（可能调用其他实用程序）。实际的文件系统类型通常默认为 *ext2*。

**e2fsck**：ext2 文件系统检查器。

**debugfs**：ext2 文件系统调试器。此多功能但危险的命令的一个用途是（尝试）恢复已删除的文件。仅适用于高级用户！

| ![注意](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 所有这些命令都应作为 *root* 调用，并且如果误用可能会损坏或破坏文件系统。 |
| --- | --- |

**badblocks**

在存储设备上检查坏块（物理媒体缺陷）。此命令在格式化新安装的硬盘或测试备份媒体的完整性时很有用。[[4]](#FTN.AEN16504) 例如，**badblocks /dev/fd0** 测试软盘。

**badblocks**命令可以以破坏性（覆盖所有数据）或非破坏性只读模式调用。如果*root 用户*拥有要测试的设备，通常情况下是这样的，那么*root*必须调用此命令。

**lsusb**，**usbmodules**

**lsusb**命令列出所有 USB（通用串行总线）总线及其连接的设备。

**usbmodules**命令输出有关连接的 USB 设备的驱动模块信息。

```sh
bash$ **lsusb**
Bus 001 Device 001: ID 0000:0000  
 Device Descriptor:
   bLength                18
   bDescriptorType         1
   bcdUSB               1.00
   bDeviceClass            9 Hub
   bDeviceSubClass         0 
   bDeviceProtocol         0 
   bMaxPacketSize0         8
   idVendor           0x0000 
   idProduct          0x0000

   . . .

```

**lspci**

列出现有的*pci*总线。

```sh
bash$ **lspci**
00:00.0 Host bridge: Intel Corporation 82845 845
 (Brookdale) Chipset Host Bridge (rev 04)
 00:01.0 PCI bridge: Intel Corporation 82845 845
 (Brookdale) Chipset AGP Bridge (rev 04)
 00:1d.0 USB Controller: Intel Corporation 82801CA/CAM USB (Hub #1) (rev 02)
 00:1d.1 USB Controller: Intel Corporation 82801CA/CAM USB (Hub #2) (rev 02)
 00:1d.2 USB Controller: Intel Corporation 82801CA/CAM USB (Hub #3) (rev 02)
 00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev 42)

   . . .

```

**mkbootdisk**

创建一个启动软盘，可用于在 MBR（主引导记录）损坏的情况下启动系统。特别值得注意的是`--iso`选项，它使用**mkisofs**创建一个可启动的*ISO9660*文件系统镜像，适合烧录可启动的 CDR。

**mkbootdisk**命令实际上是一个由 Erik Troan 编写的 Bash 脚本，位于`/sbin`目录下。

**mkisofs**

创建一个适合 CDR 镜像的*ISO9660*文件系统。

**chroot**

CHange ROOT directory. 通常命令是从$PATH 获取的，相对于`/`，默认的*root*目录。这会将*root*目录更改为另一个目录（并且也将工作目录更改为那里）。这对于安全目的很有用，例如当系统管理员希望将某些用户，如 telnetting 的用户，限制在文件系统的安全部分时（这有时被称为将访客用户限制在“chroot 监狱”中）。请注意，在**chroot**之后，系统二进制文件的执行路径不再有效。

`**chroot /opt**`会导致对`/usr/bin`的引用被转换为`/opt/usr/bin`。同样，`**chroot /aaa/bbb /bin/ls**`会将未来的**ls**实例重定向到`/aaa/bbb`作为基础目录，而不是通常的`/`。用户在``~/.bashrc`中的**alias XX 'chroot /aaa/bbb ls'**实际上限制了用户可以运行命令"XX"的文件系统部分。

当从紧急启动软盘运行（**chroot**到`/dev/fd0`）或作为从系统崩溃中恢复时**lilo**的选项时，**chroot**命令也很有用。其他用途包括从不同的文件系统安装（rpm 选项）或从 CD ROM 运行只读文件系统。仅作为*root*调用，并小心使用。

| ![Caution](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 可能需要将某些系统文件复制到*chrooted*目录中，因为正常的`$PATH`不能再被依赖。 |
| --- | --- |

**lockfile**

此实用程序是**procmail**包的一部分([www.procmail.org](http://www.procmail.org))。它创建一个*锁文件*，一个控制对文件、设备或资源访问的*信号量*。

|

`**定义：**` 一个 *信号量* 是一个标志或信号。（这种用法起源于铁路，一个彩色旗帜、灯笼或条纹可移动臂 *信号量* 表示特定的轨道是否在使用中，因此不可用于另一列火车。）UNIX 进程可以检查适当的信号量以确定特定资源是否可用/可访问。

|

锁文件作为标志，表明该特定文件、设备或资源正在被一个进程使用（因此是“忙碌”的）。锁文件的存在仅允许对其他进程进行有限的访问（或无访问）。

```sh
lockfile /home/bozo/lockfiles/$0.lock
# Creates a write-protected lockfile prefixed with the name of the script.

lockfile /home/bozo/lockfiles/${0##*/}.lock
# A safer version of the above, as pointed out by E. Choroba.
```

锁文件用于保护系统邮件文件夹免受多个用户同时更改，指示调制解调器端口正在被访问，以及显示 Firefox 的一个实例正在使用其缓存。脚本可能会检查由某个进程创建的锁文件是否存在，以检查该进程是否正在运行。请注意，如果脚本尝试创建一个已经存在的锁文件，脚本可能会挂起。

通常，应用程序在 `/var/lock` 目录中创建和检查锁文件。[[5]](#FTN.AEN16659) 一个脚本可以通过以下方式测试锁文件的存在。

```sh
appname=xyzip
# Application "xyzip" created lock file "/var/lock/xyzip.lock".

if [ -e "/var/lock/$appname.lock" ]
then   #+ Prevent other programs & scripts
       #  from accessing files/resources used by xyzip.
  ...
```

**flock**

与 **lockfile** 命令相比，**flock** 的用途要少得多。它会在文件上设置一个“建议性”锁，然后在锁存在的情况下执行一个命令。这是为了防止其他进程在指定命令完成之前对该文件设置锁。

```sh
flock $0 cat $0 > lockfile__$0
#  Set a lock on the script the above line appears in,
#+ while listing the script to stdout.
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 与 **lockfile** 不同，**flock** 并不会自动创建锁文件。 |

**mknod**

创建块或字符 设备文件（在系统上安装新硬件时可能需要）。**MAKEDEV** 实用程序几乎具有 **mknod** 的所有功能，并且更容易使用。

**MAKEDEV**

创建设备文件的实用程序。必须以 *root* 身份运行，并在 `/dev` 目录下。它是 **mknod** 的高级版本。

**tmpwatch**

自动删除在指定时间内未被访问的文件。通常由 cron 调用来删除旧的日志文件。

**备份**

**dump**, **restore**

**dump** 命令是一个复杂的文件系统备份实用程序，通常用于较大的安装和网络。[[6]](#FTN.AEN16748) 它读取原始磁盘分区，并以二进制格式写入备份文件。要备份的文件可以保存到各种存储介质上，包括磁盘和磁带驱动器。**restore** 命令用于恢复使用 **dump** 创建的备份。

**fdformat**

对软盘（`/dev/fd0*`）执行低级格式化。

**系统资源**

**ulimit**

设置系统资源使用的 *上限*。通常使用 `-f` 选项，该选项设置文件大小限制（**ulimit -f 1000** 将文件限制在 1 兆最大）。[[7]](#FTN.AEN16782) `-t` 选项限制核心转储大小（**ulimit -c 0** 消除核心转储）。通常，**ulimit** 的值会在 `/etc/profile` 和/或 `~/.bash_profile` 中设置（参见 附录 H）。

| ![重要](img/d8e2c61a4ebc25a8846b6825b6029167.png) | 聪明地使用 **ulimit** 可以保护系统免受可怕的 *fork bomb* 的侵害。

&#124;  ```sh #!/bin/bash
# This script is for illustrative purposes only.
# Run it at your own peril -- it WILL freeze your system.

while true  #  Endless loop.
do
  $0 &      #  This script invokes itself . . .
            #+ forks an infinite number of times . . .
            #+ until the system freezes up because all resources exhausted.
done        #  This is the notorious "sorcerer's appentice" scenario.

exit 0      #  Will not exit here, because this script will never terminate.
```  &#124;

`/etc/profile` 中的 **ulimit -Hu XX**（其中 *XX* 是用户进程限制）在超过预设限制时会终止此脚本。|

**quota**

显示用户或组磁盘配额。

**setquota**

从命令行设置用户或组磁盘配额。

**umask**

用户文件创建权限 *掩码*。限制特定用户的默认文件属性。该用户创建的所有文件都将采用由 **umask** 指定的属性。传递给 **umask** 的（八进制）值定义了文件权限 *被禁用*。例如，**umask 022** 确保新文件的最大权限为 755（777 与 022 的交集）。[[8]](#FTN.AEN16847) 当然，用户可以稍后使用 chmod 改变特定文件的属性。通常的做法是在 `/etc/profile` 和/或 `~/.bash_profile` 中设置 **umask** 的值（参见 附录 H）。

**示例 17-10\. 使用 *umask* 隐藏输出文件以防止好奇的目光**

```sh
#!/bin/bash
# rot13a.sh: Same as "rot13.sh" script, but writes output to "secure" file.

# Usage: ./rot13a.sh filename
# or     ./rot13a.sh <filename
# or     ./rot13a.sh and supply keyboard input (stdin)

umask 177               #  File creation mask.
                        #  Files created by this script
                        #+ will have 600 permissions.

OUTFILE=decrypted.txt   #  Results output to file "decrypted.txt"
                        #+ which can only be read/written
                        #  by invoker of script (or root).

cat "$@" &#124; tr 'a-zA-Z' 'n-za-mN-ZA-M' > $OUTFILE 
#    ^^ Input from stdin or a file.   ^^^^^^^^^^ Output redirected to file. 

exit 0
```

**rdev**

获取关于根设备、交换空间或视频模式的信息或进行更改。**rdev** 的功能通常已被 **lilo** 取代，但 **rdev** 仍然在设置 ram 磁盘时很有用。这是一个危险的命令，如果误用。

**模块**

**lsmod**

列出已安装的内核模块。

```sh
bash$ **lsmod**
Module                  Size  Used by
 autofs                  9456   2 (autoclean)
 opl3                   11376   0
 serial_cs               5456   0 (unused)
 sb                     34752   0
 uart401                 6384   0 [sb]
 sound                  58368   0 [opl3 sb uart401]
 soundlow                 464   0 [sound]
 soundcore               2800   6 [sb sound]
 ds                      6448   2 [serial_cs]
 i82365                 22928   2
 pcmcia_core            45984   0 [serial_cs ds i82365]

```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 执行 **cat /proc/modules** 会给出相同的信息。 |

**insmod**

强制安装内核模块（如果可能，请使用 **modprobe**）。必须以 *root* 身份调用。

**rmmod**

强制卸载内核模块。必须以 *root* 身份调用。

**modprobe**

模块加载器，通常在启动脚本中自动调用。必须以 *root* 身份调用。

**depmod**

创建模块依赖文件。通常从启动脚本中调用。

**modinfo**

输出有关可加载模块的信息。

```sh
bash$ **modinfo hid**
filename:    /lib/modules/2.4.20-6/kernel/drivers/usb/hid.o
 description: "USB HID support drivers"
 author:      "Andreas Gal, Vojtech Pavlik <vojtech@suse.cz>"
 license:     "GPL"

```

**杂项**

**env**

运行程序或脚本，并设置或更改特定的 环境变量（而不更改整个系统环境）。`[varname=xxx]` 允许在脚本执行期间更改环境变量 `varname`。如果没有指定选项，则此命令列出所有环境变量设置。[[9]](#FTN.AEN16975)

| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 脚本的第 一行（“sha-bang”行）在不知道 shell 或解释器的路径时可以使用 **env**。

&#124;  ```sh #! /usr/bin/env perl

print "This Perl script will run,\n";
print "even when I don't know where to find Perl.\n";

# Good for portable cross-platform scripts,
# where the Perl binaries may not be in the expected place.
# Thanks, S.C.
```  &#124;

或者甚至 ...

&#124;  ```sh #!/bin/env bash
# Queries the $PATH enviromental variable for the location of bash.
# Therefore ...
# This script will run where Bash is not in its usual place, in /bin.
...
```  &#124;

|

**ldd**

显示可执行文件的共享库依赖关系。

```sh
bash$ **ldd /bin/ls**
libc.so.6 => /lib/libc.so.6 (0x4000c000)
/lib/ld-linux.so.2 => /lib/ld-linux.so.2 (0x80000000)
```

**watch**

重复运行命令，在指定的时间间隔内。

默认为两秒间隔，但可以通过 `-n` 选项进行更改。

```sh
watch -n 5 tail /var/log/messages
# Shows tail end of system log, /var/log/messages, every five seconds.
```
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 不幸的是，将 **watch 命令** 的输出通过 管道 传递给 grep 并不工作。 |

**strip**

从可执行二进制文件中移除调试符号引用。这会减小其大小，但使得调试变得不可能。

此命令通常出现在 Makefile 中，但很少出现在 shell 脚本中。

**nm**

列出未剥离编译二进制文件中的符号。

**xrandr**

用于操作屏幕根窗口的命令行工具。

**示例 17-11\. *背光*：改变（笔记本电脑）屏幕背光的亮度**

```sh
#!/bin/bash
# backlight.sh
# reldate 02dec2011

#  A bug in Fedora Core 16/17 messes up the keyboard backlight controls.
#  This script is a quick-n-dirty workaround, essentially a shell wrapper
#+ for xrandr. It gives more control than on-screen sliders and widgets.

OUTPUT=$(xrandr &#124; grep LV &#124; awk '{print $1}')   # Get display name!
INCR=.05      # For finer-grained control, set INCR to .03 or .02.

old_brightness=$(xrandr --verbose &#124; grep rightness &#124; awk '{ print $2 }')

if [ -z "$1" ]
then
  bright=1    # If no command-line arg, set brightness to 1.0 (default).

  else
    if [ "$1" = "+" ]
    then
      bright=$(echo "scale=2; $old_brightness + $INCR" &#124; bc)   # +.05

  else
    if [ "$1" = "-" ]
    then
      bright=$(echo "scale=2; $old_brightness - $INCR" &#124; bc)   # -.05

  else
    if [ "$1" = "#" ]   # Echoes current brightness; does not change it.
    then
      bright=$old_brightness

  else
    if [[ "$1" = "h" &#124;&#124; "$1" = "H" ]]
    then
      echo
      echo "Usage:"
      echo "$0 [No args]    Sets/resets brightness to default (1.0)."
      echo "$0 +            Increments brightness by 0.5."
      echo "$0 -            Decrements brightness by 0.5."
      echo "$0 #            Echoes current brightness without changing it."
      echo "$0 N (number)   Sets brightness to N (useful range .7 - 1.2)."
      echo "$0 h [H]        Echoes this help message."
      echo "$0 any-other    Gives xrandr usage message."

      bright=$old_brightness

  else
    bright="$1"

      fi
     fi
    fi
  fi
fi

xrandr --output "$OUTPUT" --brightness "$bright"   # See xrandr manpage.
                                                   # As root!
E_CHANGE0=$?
echo "Current brightness = $bright"

exit $E_CHANGE0

# =========== Or, alternately . . . ==================== #

#!/bin/bash
# backlight2.sh
# reldate 20jun2012

#  A bug in Fedora Core 16/17 messes up the keyboard backlight controls.
#  This is a quick-n-dirty workaround, an alternate to backlight.sh.

target_dir=\
/sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0/backlight/acpi_video0
# Hardware directory.

actual_brightness=$(cat $target_dir/actual_brightness)
max_brightness=$(cat $target_dir/max_brightness)
Brightness=$target_dir/brightness

let "req_brightness = actual_brightness"   # Requested brightness.

if [ "$1" = "-" ]
then     # Decrement brightness 1 notch.
  let "req_brightness = $actual_brightness - 1"
else
  if [ "$1" = "+" ]
  then   # Increment brightness 1 notch.
    let "req_brightness = $actual_brightness + 1"
   fi
fi

if [ $req_brightness -gt $max_brightness ]
then
  req_brightness=$max_brightness
fi   # Do not exceed max. hardware design brightness.

echo

echo "Old brightness = $actual_brightness"
echo "Max brightness = $max_brightness"
echo "Requested brightness = $req_brightness"
echo

# =====================================
echo $req_brightness > $Brightness
# Must be root for this to take effect.
E_CHANGE1=$?   # Successful?
# =====================================

if [ "$?" -eq 0 ]
then
  echo "Changed brightness!"
else
  echo "Failed to change brightness!"
fi

act_brightness=$(cat $Brightness)
echo "Actual brightness = $act_brightness"

scale0=2
sf=100 # Scale factor.
pct=$(echo "scale=$scale0; $act_brightness / $max_brightness * $sf" &#124; bc)
echo "Percentage brightness = $pct%"

exit $E_CHANGE1
```

**rdist**

远程分发客户端：同步、克隆或备份远程服务器上的文件系统。

### 备注

| [[1]](system.html#AEN14695) | 在 Linux 机器或具有磁盘配额的 UNIX 系统中，情况就是这样。 |
| --- | --- |
| [[2]](system.html#AEN14727) | 如果要删除的用户仍然登录，则 **userdel** 命令将失败。 |
| [[3]](system.html#AEN16255) | 关于烧录 CDR 的更多详细信息，请参阅 Alex Withers 的文章，[创建 CD](http://www2.linuxjournal.com/lj-issues/issue66/3335.html)，该文章发表在 1999 年 10 月的 [*Linux Journal*](http://www.linuxjournal.com) 期。 |
| [[4]](system.html#AEN16504) | mke2fs 的 `-c` 选项也会调用对坏块的检查。 |
| [[5]](system.html#AEN16659) | 由于 `/var/lock` 目录中只有 *root* 拥有写权限，因此用户脚本不能在那里设置锁文件。 |
| [[6]](system.html#AEN16748) | 单用户 Linux 系统的运营商通常更喜欢备份时使用更简单的工具，例如 **tar**。 |
| [[7]](system.html#AEN16782) | 截至 Bash 的 版本 4 更新，在 POSIX 模式下，`-f` 和 `-c` 选项使用 512 字节块大小。此外，还有两个新选项：`-b` 用于 套接字 缓冲区大小，以及 `-T` 用于 *线程* 数量的限制。 |
| [[8]](system.html#AEN16847) | NAND 是逻辑 **非与** 操作符。其效果与减法有些类似。 |

| [[9]](system.html#AEN16975) | 在 Bash 和其他 Bourne shell 衍生版本中，可以在单个命令的环境中设置变量。

&#124;  ```sh var1=value1 var2=value2 commandXXX
# $var1 and $var2 set in the environment of 'commandXXX' only.
```  &#124;

|
