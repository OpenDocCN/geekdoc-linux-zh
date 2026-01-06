# 第四部分。命令

> 原文：[`tldp.org/LDP/abs/html/part4.html`](https://tldp.org/LDP/abs/html/part4.html)

掌握 Linux 机器上的命令是编写有效 shell 脚本不可或缺的序曲。

本节涵盖以下命令：

+   . (See also source)

+   ac

+   adduser

+   agetty

+   agrep

+   ar

+   arch

+   at

+   autoload

+   awk (See also Using **awk** for math operations)

+   badblocks

+   banner

+   basename

+   batch

+   bc

+   bg

+   bind

+   bison

+   builtin

+   bzgrep

+   bzip2

+   cal

+   caller

+   cat

+   cd

+   chattr

+   chfn

+   chgrp

+   chkconfig

+   chmod

+   chown

+   chroot

+   cksum

+   clear

+   clock

+   cmp

+   col

+   colrm

+   column

+   comm

+   command

+   compgen

+   complete

+   compress

+   coproc

+   cp

+   cpio

+   cron

+   crypt

+   csplit

+   cu

+   cut

+   date

+   dc

+   dd

+   debugfs

+   declare

+   depmod

+   df

+   dialog

+   diff

+   diff3

+   diffstat

+   dig

+   dirname

+   dirs

+   disown

+   dmesg

+   doexec

+   dos2unix

+   du

+   dump

+   dumpe2fs

+   e2fsck

+   echo

+   egrep

+   enable

+   enscript

+   env

+   eqn

+   eval

+   exec

+   exit (相关主题：退出状态)

+   expand

+   export

+   expr

+   factor

+   false

+   fdformat

+   fdisk

+   fg

+   fgrep

+   file

+   find

+   finger

+   flex

+   flock

+   fmt

+   fold

+   free

+   fsck

+   ftp

+   fuser

+   getfacl

+   getopt

+   getopts

+   gettext

+   getty

+   gnome-mount

+   grep

+   groff

+   groupmod

+   groups (相关主题：变量$GROUPS)

+   gs

+   gzip

+   halt

+   hash

+   hdparm

+   head

+   help

+   hexdump

+   host

+   hostid

+   hostname (相关主题：变量$HOSTNAME)

+   hwclock

+   iconv

+   id (相关主题：变量$UID)

+   ifconfig

+   info

+   infocmp

+   init

+   insmod

+   install

+   ip

+   ipcalc

+   iptables

+   iwconfig

+   jobs

+   join

+   jot

+   kill

+   killall

+   last

+   lastcomm

+   lastlog

+   ldd

+   less

+   let

+   lex

+   lid

+   ln

+   locate

+   lockfile

+   logger

+   logname

+   logout

+   logrotate

+   look

+   losetup

+   lp

+   ls

+   lsdev

+   lsmod

+   lsof

+   lspci

+   lsusb

+   ltrace

+   lynx

+   lzcat

+   lzma

+   m4

+   mail

+   mailstats

+   mailto

+   make

+   MAKEDEV

+   man

+   mapfile

+   mcookie

+   md5sum

+   合并

+   mesg

+   mimencode

+   mkbootdisk

+   mkdir

+   mkdosfs

+   mke2fs

+   mkfifo

+   mkisofs

+   mknod

+   mkswap

+   mktemp

+   mmencode

+   modinfo

+   modprobe

+   more

+   mount

+   msgfmt

+   mv

+   nc

+   netconfig

+   netstat

+   newgrp

+   nice

+   nl

+   nm

+   nmap

+   nohup

+   nslookup

+   objdump

+   od

+   openssl

+   passwd

+   paste

+   patch (相关主题：diff)

+   pathchk

+   pax

+   pgrep

+   pidof

+   ping

+   pkill

+   popd

+   pr

+   printenv

+   printf

+   procinfo

+   ps

+   pstree

+   ptx

+   pushd

+   pwd (相关主题：变量$PWD)

+   quota

+   rcp

+   rdev

+   rdist

+   read

+   readelf

+   readlink

+   readonly

+   重启

+   recode

+   renice

+   reset

+   resize

+   恢复

+   rev

+   rlogin

+   删除

+   rmdir

+   rmmod

+   route

+   rpm

+   rpm2cpio

+   rsh

+   rsync

+   runlevel

+   run-parts

+   rx

+   rz

+   sar

+   scp

+   script

+   sdiff

+   sed

+   seq

+   service

+   set

+   setfacl

+   setquota

+   setserial

+   setterm

+   sha1sum

+   shar

+   shopt

+   shred

+   shutdown

+   size

+   skill

+   sleep

+   slocate

+   snice

+   sort

+   source

+   sox

+   split

+   sq

+   ssh

+   stat

+   strace

+   strings

+   strip

+   stty

+   su

+   sudo

+   sum

+   suspend

+   swapoff

+   swapon

+   sx

+   sync

+   sz

+   tac

+   tail

+   tar

+   tbl

+   tcpdump

+   tee

+   telinit

+   telnet

+   Tex

+   texexec

+   time

+   times

+   tmpwatch

+   top

+   touch

+   tput

+   tr

+   traceroute

+   true

+   tset

+   tsort

+   tty

+   tune2fs

+   type

+   typeset

+   ulimit

+   umask

+   umount

+   uname

+   unarc

+   unarj

+   uncompress

+   unexpand

+   uniq

+   units

+   unlzma

+   unrar

+   unset

+   unsq

+   unzip

+   uptime

+   usbmodules

+   useradd

+   userdel

+   usermod

+   users

+   usleep

+   uucp

+   uudecode

+   uuencode

+   uux

+   vacation

+   vdir

+   vmstat

+   vrfy

+   w

+   wait

+   wall

+   watch

+   wc

+   wget

+   whatis

+   whereis

+   which

+   who

+   whoami

+   whois

+   write

+   xargs

+   xrandr

+   xz

+   yacc

+   yes

+   zcat

+   zdiff

+   zdump

+   zegrep

+   zfgrep

+   zgrep

+   zip

**目录**

15\. 内部命令和内置命令

15.1\. 作业控制命令

16\. 外部过滤器、程序和命令

16.1\. 基本命令

16.2\. 复杂命令

16.3\. 时间和日期命令

16.4\. 文本处理命令

16.5\. 文件和归档命令

16.6\. 通信命令

16.7\. 终端控制命令

16.8\. 数学命令

16.9\. 杂项命令

17\. 系统和管理命令

17.1\. 分析系统脚本
