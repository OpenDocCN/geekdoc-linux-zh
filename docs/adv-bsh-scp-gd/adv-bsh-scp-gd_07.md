# 第五章. 引号

> 原文：[`tldp.org/LDP/abs/html/quoting.html`](https://tldp.org/LDP/abs/html/quoting.html)

**目录**

5.1\. 引号变量

5.2\. 转义

引号的意思就是用引号括起一个字符串。这会使得字符串中的 特殊字符 免受 shell 或 shell 脚本的重新解释或扩展。（如果一个字符除了其字面意义外还有其他解释，则该字符是“特殊”的。例如，星号 * 在 通配符匹配 和 正则表达式 中代表通配符。）

```sh
bash$ **ls -l [Vv]***
-rw-rw-r--    1 bozo  bozo       324 Apr  2 15:05 VIEWDATA.BAT
 -rw-rw-r--    1 bozo  bozo       507 May  4 14:25 vartrace.sh
 -rw-rw-r--    1 bozo  bozo       539 Apr 14 17:11 viewdata.sh

bash$ **ls -l '[Vv]*'**
ls: [Vv]*: No such file or directory
```

|

在日常口语或写作中，当我们“引用”一个短语时，我们会将其区分开来并赋予它特殊的意义。在 Bash 脚本中，当我们 *引用* 一个字符串时，我们会将其区分开来并保护其 *字面* 意义。

|

某些程序和实用工具会重新解释或扩展引号字符串中的特殊字符。引号的一个重要用途是保护命令行参数免受 shell 的影响，同时仍然允许调用程序对其进行扩展。

```sh
bash$ **grep '[Ff]irst' *.txt**
file1.txt:This is the first line of file1.txt.
 file2.txt:This is the First line of file2.txt.
```

注意，未引用的 `**grep [Ff]irst *.txt**` 在 Bash shell 下是有效的。[[1]](#FTN.AEN2609)

引号还可以抑制 echo 的 对新行的“渴望”。

```sh
bash$ **echo $(ls -l)**
total 8 -rw-rw-r-- 1 bo bo 13 Aug 21 12:57 t.sh -rw-rw-r-- 1 bo bo 78 Aug 21 12:57 u.sh

bash$ **echo "$(ls -l)"**
total 8
 -rw-rw-r--  1 bo bo  13 Aug 21 12:57 t.sh
 -rw-rw-r--  1 bo bo  78 Aug 21 12:57 u.sh
```

### 备注

| [[1]](quoting.html#AEN2609) | 除非当前工作目录中存在名为 `first` 的文件。这是引用的另一个原因。（感谢 Harald Koenig 指出这一点。 |
| --- | --- |
