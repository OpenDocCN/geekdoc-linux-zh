# O.1\. 分析脚本

> 原文：[`tldp.org/LDP/abs/html/scriptanalysis.html`](https://tldp.org/LDP/abs/html/scriptanalysis.html)

检查以下脚本。运行它，然后解释它做了什么。注释脚本，并以更紧凑、更优雅的方式重写它。

```sh
#!/bin/bash

MAX=10000

  for((nr=1; nr<$MAX; nr++))
  do

    let "t1 = nr % 5"
    if [ "$t1" -ne 3 ]
    then
      continue
    fi

    let "t2 = nr % 7"
    if [ "$t2" -ne 4 ]
    then
      continue
    fi

    let "t3 = nr % 9"
    if [ "$t3" -ne 5 ]
    then
      continue
    fi

  break   # What happens when you comment out this line? Why?

  done

  echo "Number = $nr"

exit 0
```

---

解释以下脚本做了什么。它实际上只是一个参数化的命令行管道。

```sh
#!/bin/bash

DIRNAME=/usr/bin
FILETYPE="shell script"
LOGFILE=logfile

file "$DIRNAME"/* &#124; fgrep "$FILETYPE" &#124; tee $LOGFILE &#124; wc -l

exit 0
```

---

检查并解释以下脚本。提示可能参考 find 和 stat 的列表。

```sh
#!/bin/bash

# Author:  Nathan Coulter
# This code is released to the public domain.
# The author gave permission to use this code snippet in the ABS Guide.

find -maxdepth 1 -type f -printf '%f\000'  &#124; {
   while read -d $'\000'; do
      mv "$REPLY" "$(date -d "$(stat -c '%y' "$REPLY") " '+%Y%m%d%H%M%S'
      )-$REPLY"
   done
}

# Warning: Test-drive this script in a "scratch" directory.
# It will somehow affect all the files there.
```

---

有读者发送了以下代码片段。

```sh
while read LINE
do
  echo $LINE
done < `tail -f /var/log/messages`
```

他希望编写一个跟踪系统日志文件`/var/log/messages`更改的脚本。不幸的是，上述代码块挂起并且没有任何有用的操作。为什么？修复它，让它能够工作。（提示：与其重定向循环的`stdin`，尝试使用管道。）

---

分析以下由 Rory Winston 提供的“一行脚本”（为了清晰起见分成两行）：

```sh
export SUM=0; for f in $(find src -name "*.java");
do export SUM=$(($SUM + $(wc -l $f &#124; awk '{ print $1 }'))); done; echo $SUM
```

提示：首先，将脚本分解成小块。然后，仔细检查其使用双括号算术、export 命令、find 命令、wc 命令和 awk。

---

分析示例 A-10，并以更简化、更逻辑的方式重新组织。看看可以消除多少变量，并尝试优化脚本以加快其执行时间。

修改脚本，使其接受任何普通 ASCII 文本文件作为其初始“生成”的输入。脚本将读取前`*$ROW*$COL*`个字符，并将元音的出现设置为“活细胞”。提示：务必将输入文件中的空格转换为下划线字符。
