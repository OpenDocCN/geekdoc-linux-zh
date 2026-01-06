# 第二十三章。进程替换

> 原文：[`tldp.org/LDP/abs/html/process-sub.html`](https://tldp.org/LDP/abs/html/process-sub.html)

管道 命令的 `stdout` 到另一个命令的 `stdin` 是一种强大的技术。但是，如果你需要将多个命令的 `stdout` 进行管道传输呢？这就是 `*进程替换*` 发挥作用的地方。

*进程替换* 将一个 进程（或多个进程）的输出喂入另一个进程的 `stdin`。

**模板**

括号内的命令列表

**>(命令列表)**

**<(命令列表)**

进程替换使用 `/dev/fd/<n>` 文件将括号内进程的结果发送到另一个进程。 [[1]](#FTN.AEN18244)

| ![警告](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | "<" 或 ">" 和括号之间没有空格。那里有空格会给出错误信息。 |
| --- | --- |
```sh
bash$ **echo >(true)**
/dev/fd/63

bash$ **echo <(true)**
/dev/fd/63

bash$ **echo >(true) <(true)**
/dev/fd/63 /dev/fd/62

bash$ **wc <(cat /usr/share/dict/linux.words)**
 483523  483523 4992010 /dev/fd/63

bash$ **grep script /usr/share/dict/linux.words &#124; wc**
 262     262    3601

bash$ **wc <(grep script /usr/share/dict/linux.words)**
 262     262    3601 /dev/fd/63

```  |
| ![注意](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | Bash 创建了一个包含两个 文件描述符，`--fIn` 和 `fOut--` 的管道。true 的 `stdin` 连接到 `fOut`（dup2(fOut, 0)），然后 Bash 将 `/dev/fd/fIn` 参数传递给 **echo**。在缺少 `/dev/fd/<n>` 文件系统的系统上，Bash 可能会使用临时文件。（感谢，S.C.） |

进程替换可以比较两个不同命令的输出，甚至可以比较同一命令不同选项的输出。

```sh
bash$ **comm <(ls -l) <(ls -al)**
total 12
-rw-rw-r--    1 bozo bozo       78 Mar 10 12:58 File0
-rw-rw-r--    1 bozo bozo       42 Mar 10 12:58 File2
-rw-rw-r--    1 bozo bozo      103 Mar 10 12:58 t2.sh
        total 20
        drwxrwxrwx    2 bozo bozo     4096 Mar 10 18:10 .
        drwx------   72 bozo bozo     4096 Mar 10 17:58 ..
        -rw-rw-r--    1 bozo bozo       78 Mar 10 12:58 File0
        -rw-rw-r--    1 bozo bozo       42 Mar 10 12:58 File2
        -rw-rw-r--    1 bozo bozo      103 Mar 10 12:58 t2.sh
```

进程替换可以比较两个目录的内容 -- 看看哪些文件名在一个目录中，但在另一个目录中不存在。

```sh
diff <(ls $first_directory) <(ls $second_directory)
```

进程替换的一些其他用法和用途：

```sh
read -a list < <( od -Ad -w24 -t u2 /dev/urandom )
#  Read a list of random numbers from /dev/urandom,
#+ process with "od"
#+ and feed into stdin of "read" . . .

#  From "insertion-sort.bash" example script.
#  Courtesy of JuanJo Ciarlante.
```
```sh
PORT=6881   # bittorrent

# Scan the port to make sure nothing nefarious is going on.
netcat -l $PORT &#124; tee>(md5sum ->mydata-orig.md5) &#124;
gzip &#124; tee>(md5sum - &#124; sed 's/-$/mydata.lz2/'>mydata-gz.md5)>mydata.gz

# Check the decompression:
  gzip -d<mydata.gz &#124; md5sum -c mydata-orig.md5)
# The MD5sum of the original checks stdin and detects compression issues.

#  Bill Davidsen contributed this example
#+ (with light edits by the ABS Guide author).
```  |
```sh
cat <(ls -l)
# Same as     ls -l &#124; cat

sort -k 9 <(ls -l /bin) <(ls -l /usr/bin) <(ls -l /usr/X11R6/bin)
# Lists all the files in the 3 main 'bin' directories, and sorts by filename.
# Note that three (count 'em) distinct commands are fed to 'sort'.

diff <(command1) <(command2)    # Gives difference in command output.

tar cf >(bzip2 -c > file.tar.bz2) $directory_name
# Calls "tar cf /dev/fd/?? $directory_name", and "bzip2 -c > file.tar.bz2".
#
# Because of the /dev/fd/<n> system feature,
# the pipe between both commands does not need to be named.
#
# This can be emulated.
#
bzip2 -c < pipe > file.tar.bz2&
tar cf pipe $directory_name
rm pipe
#        or
exec 3>&1
tar cf /dev/fd/4 $directory_name 4>&1 >&3 3>&- &#124; bzip2 -c > file.tar.bz2 3>&-
exec 3>&-

# Thanks, Stphane Chazelas
```  |

这里是一种绕过在子 shell 中运行的*echo*管道到*while-read 循环*问题的方法。

**示例 23-1\. 无 fork 的代码块重定向**

```sh
#!/bin/bash
# wr-ps.bash: while-read loop with process substitution.

# This example contributed by Tomas Pospisek.
# (Heavily edited by the ABS Guide author.)

echo

echo "random input" &#124; while read i
do
  global=3D": Not available outside the loop."
  # ... because it runs in a subshell.
done

echo "\$global (from outside the subprocess) = $global"
# $global (from outside the subprocess) =

echo; echo "--"; echo

while read i
do
  echo $i
  global=3D": Available outside the loop."
  # ... because it does NOT run in a subshell.
done < <( echo "random input" )
#    ^ ^

echo "\$global (using process substitution) = $global"
# Random input
# $global (using process substitution) = 3D: Available outside the loop.

echo; echo "##########"; echo

# And likewise . . .

declare -a inloop
index=0
cat $0 &#124; while read line
do
  inloop[$index]="$line"
  ((index++))
  # It runs in a subshell, so ...
done
echo "OUTPUT = "
echo ${inloop[*]}           # ... nothing echoes.

echo; echo "--"; echo

declare -a outloop
index=0
while read line
do
  outloop[$index]="$line"
  ((index++))
  # It does NOT run in a subshell, so ...
done < <( cat $0 )
echo "OUTPUT = "
echo ${outloop[*]}          # ... the entire script echoes.

exit $?
```

这是一个类似的例子。

**示例 23-2\. 将 *进程替换* 的输出重定向到循环中。**

```sh
#!/bin/bash
# psub.bash

# As inspired by Diego Molina (thanks!).

declare -a array0
while read
do
  array0[${#array0[@]}]="$REPLY"
done < <( sed -e 's/bash/CRASH-BANG!/' $0 &#124; grep bin &#124; awk '{print $1}' )
#  Sets the default 'read' variable, $REPLY, by process substitution,
#+ then copies it into an array.

echo "${array0[@]}"

exit $?

# ====================================== #

bash psub.bash

#!/bin/CRASH-BANG! done #!/bin/CRASH-BANG!
```

一位读者发送了以下有趣的进程替换示例。

```sh
# Script fragment taken from SuSE distribution:

# --------------------------------------------------------------#
while read  des what mask iface; do
# Some commands ...
done < <(route -n)  
#    ^ ^  First < is redirection, second is process substitution.

# To test it, let's make it do something.
while read  des what mask iface; do
  echo $des $what $mask $iface
done < <(route -n)  

# Output:
# Kernel IP routing table
# Destination Gateway Genmask Flags Metric Ref Use Iface
# 127.0.0.0 0.0.0.0 255.0.0.0 U 0 0 0 lo
# --------------------------------------------------------------#

#  As Stphane Chazelas points out,
#+ an easier-to-understand equivalent is:
route -n &#124;
  while read des what mask iface; do   # Variables set from output of pipe.
    echo $des $what $mask $iface
  done  #  This yields the same output as above.
        #  However, as Ulrich Gayer points out . . .
        #+ this simplified equivalent uses a subshell for the while loop,
        #+ and therefore the variables disappear when the pipe terminates.

# --------------------------------------------------------------#

#  However, Filip Moritz comments that there is a subtle difference
#+ between the above two examples, as the following shows.

(
route -n &#124; while read x; do ((y++)); done
echo $y # $y is still unset

while read x; do ((y++)); done < <(route -n)
echo $y # $y has the number of lines of output of route -n
)

More generally spoken
(
: &#124; x=x
# seems to start a subshell like
: &#124; ( x=x )
# while
x=x < <(:)
# does not
)

# This is useful, when parsing csv and the like.
# That is, in effect, what the original SuSE code fragment does.
```

### 注意

| [[1]](process-sub.html#AEN18244) | 这与一个 命名管道（临时文件）具有相同的效果，实际上，命名管道曾经被用于进程替换。 |
| --- | --- |
