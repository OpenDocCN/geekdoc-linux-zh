# 第三十一章. 零与空

> 原文：[`tldp.org/LDP/abs/html/zeros.html`](https://tldp.org/LDP/abs/html/zeros.html)

|   |  **完美无瑕，冰冷的规律，完美至极*

*完美的死亡；不再有更多。*

*--阿尔弗雷德·洛德·丁尼生**  |

**`/dev/zero` ... `/dev/null`**

`/dev/null` 的用途

将 `/dev/null` 想象成一个 *黑洞*。它本质上相当于一个只写文件。写入它的所有内容都会消失。尝试读取或从它输出都会导致无结果。尽管如此，`/dev/null` 在命令行和脚本中都可以非常有用。

抑制 `stdout`。

```sh
cat $filename >/dev/null
# Contents of the file will not list to stdout.
```

抑制 `stderr`（参见 示例 16-3）。

```sh
rm $badname 2>/dev/null
#           So error messages [stderr] deep-sixed.
```

抑制 `stdout` 和 `stderr` 的输出。

```sh
cat $filename 2>/dev/null >/dev/null
# If "$filename" does not exist, there will be no error message output.
# If "$filename" does exist, the contents of the file will not list to stdout.
# Therefore, no output at all will result from the above line of code.
#
#  This can be useful in situations where the return code from a command
#+ needs to be tested, but no output is desired.
#
# cat $filename &>/dev/null
#     also works, as Baris Cicek points out.
```

删除文件内容，但保留文件本身及其所有权限（参见 示例 2-1 和 示例 2-3）：

```sh
cat /dev/null > /var/log/messages
#  : > /var/log/messages   has same effect, but does not spawn a new process.

cat /dev/null > /var/log/wtmp
```

自动清空日志文件的内容（特别适合处理那些商业网站发送的讨厌的“cookies”）：

**示例 31-1\. 隐藏 cookie jar**

```sh
# Obsolete Netscape browser.
# Same principle applies to newer browsers.

if [ -f ~/.netscape/cookies ]  # Remove, if exists.
then
  rm -f ~/.netscape/cookies
fi

ln -s /dev/null ~/.netscape/cookies
# All cookies now get sent to a black hole, rather than saved to disk.
```

`/dev/zero` 的用途

与 `/dev/null` 类似，`/dev/zero` 是一个伪设备文件，但它实际上产生的是一串空值（*二进制*零，不是 ASCII 类型的）。写入 `/dev/zero` 的输出会消失，实际上很难读取那里发出的空值，尽管可以使用 od 或十六进制编辑器做到。`/dev/zero` 的主要用途是创建一个预定长度的初始化虚拟文件，用作临时交换文件。

**示例 31-2\. 使用 `/dev/zero` 设置交换文件**

```sh
#!/bin/bash
# Creating a swap file.

#  A swap file provides a temporary storage cache
#+ which helps speed up certain filesystem operations.

ROOT_UID=0         # Root has $UID 0.
E_WRONG_USER=85    # Not root?

FILE=/swap
BLOCKSIZE=1024
MINBLOCKS=40
SUCCESS=0

# This script must be run as root.
if [ "$UID" -ne "$ROOT_UID" ]
then
  echo; echo "You must be root to run this script."; echo
  exit $E_WRONG_USER
fi  

blocks=${1:-$MINBLOCKS}          #  Set to default of 40 blocks,
                                 #+ if nothing specified on command-line.
# This is the equivalent of the command block below.
# --------------------------------------------------
# if [ -n "$1" ]
# then
#   blocks=$1
# else
#   blocks=$MINBLOCKS
# fi
# --------------------------------------------------

if [ "$blocks" -lt $MINBLOCKS ]
then
  blocks=$MINBLOCKS              # Must be at least 40 blocks long.
fi  

######################################################################
echo "Creating swap file of size $blocks blocks (KB)."
dd if=/dev/zero of=$FILE bs=$BLOCKSIZE count=$blocks  # Zero out file.
mkswap $FILE $blocks             # Designate it a swap file.
swapon $FILE                     # Activate swap file.
retcode=$?                       # Everything worked?
#  Note that if one or more of these commands fails,
#+ then it could cause nasty problems.
######################################################################

#  Exercise:
#  Rewrite the above block of code so that if it does not execute
#+ successfully, then:
#    1) an error message is echoed to stderr,
#    2) all temporary files are cleaned up, and
#    3) the script exits in an orderly fashion with an
#+      appropriate error code.

echo "Swap file created and activated."

exit $retcode
```

`/dev/zero` 的另一个应用是将指定大小的文件“清零”，用于特殊目的，例如在 回环设备 上挂载文件系统（参见 示例 17-8）或“安全”地删除文件（参见 示例 16-61）。

**示例 31-3\. 创建 ramdisk**

```sh
#!/bin/bash
# ramdisk.sh

#  A "ramdisk" is a segment of system RAM memory
#+ which acts as if it were a filesystem.
#  Its advantage is very fast access (read/write time).
#  Disadvantages: volatility, loss of data on reboot or powerdown,
#+                less RAM available to system.
#
#  Of what use is a ramdisk?
#  Keeping a large dataset, such as a table or dictionary on ramdisk,
#+ speeds up data lookup, since memory access is much faster than disk access.

E_NON_ROOT_USER=70             # Must run as root.
ROOTUSER_NAME=root

MOUNTPT=/mnt/ramdisk           # Create with mkdir /mnt/ramdisk.
SIZE=2000                      # 2K blocks (change as appropriate)
BLOCKSIZE=1024                 # 1K (1024 byte) block size
DEVICE=/dev/ram0               # First ram device

username=`id -nu`
if [ "$username" != "$ROOTUSER_NAME" ]
then
  echo "Must be root to run \"`basename $0`\"."
  exit $E_NON_ROOT_USER
fi

if [ ! -d "$MOUNTPT" ]         #  Test whether mount point already there,
then                           #+ so no error if this script is run
  mkdir $MOUNTPT               #+ multiple times.
fi

##############################################################################
dd if=/dev/zero of=$DEVICE count=$SIZE bs=$BLOCKSIZE  # Zero out RAM device.
                                                      # Why is this necessary?
mke2fs $DEVICE                 # Create an ext2 filesystem on it.
mount $DEVICE $MOUNTPT         # Mount it.
chmod 777 $MOUNTPT             # Enables ordinary user to access ramdisk.
                               # However, must be root to unmount it.
##############################################################################
# Need to test whether above commands succeed. Could cause problems otherwise.
# Exercise: modify this script to make it safer.

echo "\"$MOUNTPT\" now available for use."
# The ramdisk is now accessible for storing files, even by an ordinary user.

#  Caution, the ramdisk is volatile, and its contents will disappear
#+ on reboot or power loss.
#  Copy anything you want saved to a regular directory.

# After reboot, run this script to again set up ramdisk.
# Remounting /mnt/ramdisk without the other steps will not work.

#  Suitably modified, this script can by invoked in /etc/rc.d/rc.local,
#+ to set up ramdisk automatically at bootup.
#  That may be appropriate on, for example, a database server.

exit 0
```

除了上述所有内容外，`/dev/zero` 还被 ELF（*可执行和链接格式*）UNIX/Linux 二进制文件所需要。
