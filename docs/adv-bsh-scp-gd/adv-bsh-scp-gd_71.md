# O.2. 编写脚本

> 原文：[`tldp.org/LDP/abs/html/writingscripts.html`](https://tldp.org/LDP/abs/html/writingscripts.html)

编写一个脚本，执行以下每个任务。

**简单**

**自复制脚本**

编写一个脚本，将其自身备份，即将其复制到名为`backup.sh`的文件中。

提示：使用 cat 命令和适当的位置参数。

**主目录列表**

在用户的主目录上执行递归目录列表并将信息保存到文件中。压缩文件，让脚本提示用户插入 U 盘，然后按**回车键**。最后，在确认 U 盘已正确挂载（通过解析 df 的输出）后，将文件保存到 U 盘中。注意，在移除 U 盘之前必须将其**卸载**。

**将 for 循环转换为 while 和 until 循环**

将示例 11-1 中的*for 循环*转换为*while 循环*。提示：将数据存储在数组中并遍历数组元素。

已经完成了“繁重的工作”，现在将示例中的循环转换为*直到循环*。

**更改文本文件的行间距**

编写一个脚本，读取目标文件的每一行，然后将该行写回`stdout`，但后面跟一个额外的空白行。这会产生**双倍空格**的效果。

包含所有必要的代码来检查脚本是否获取了必要的命令行参数（一个文件名），以及指定的文件是否存在。

当脚本正确运行时，修改它以将目标文件**三倍空格**。

最后，编写一个脚本，从目标文件中删除所有空白行，使其**单倍空格**。

**反向列表**

编写一个脚本，将自身输出到`stdout`，但**反向**。

**自动解压文件**

给定一个文件名列表作为输入，此脚本查询每个目标文件（解析文件命令的输出）上使用的压缩类型。然后脚本自动调用适当的解压命令（**gunzip**、**bunzip2**、**unzip**、**uncompress**或任何其他）。如果目标文件未压缩，脚本会发出警告消息，但对该特定文件不采取其他操作。

**唯一系统 ID**

为您的计算机生成一个“独特”的 6 位十六进制标识符。**不要**使用有缺陷的 hostid 命令。提示：**md5sum `/etc/passwd`**，然后选择输出的前 6 位数字。

**备份**

将您主目录树（`/home/your-name`）中在过去 24 小时内已修改的所有文件存档为“tarball”（`*.tar.gz`文件）。提示：使用 find。

可选：您可以使用此作为**备份**脚本的基础。

**检查进程是否仍在运行**

给定一个进程 ID (*PID*)作为参数，此脚本将在用户指定的间隔内检查指定的进程是否仍在运行。您可以使用 ps 和 sleep 命令。

**质数**

打印（到`stdout`）60000 到 63000 之间的所有质数。输出应该格式良好，按列排列（提示：使用 printf）。

**彩票号码**

一种彩票涉及在 1-50 的范围内选择五个不同的数字。编写一个脚本，生成这个范围内的五个伪随机数，*没有重复*。脚本将提供将数字输出到`stdout`或保存到文件的选项，包括特定数字集生成的日期和时间。（如果您的脚本持续生成*中奖*的彩票号码，那么您可以用这些收益退休，并将 shell 脚本留给那些必须谋生的人。）

**中级**

**整数或字符串**

编写一个函数，确定传递给它的参数是整数还是字符串。如果传递整数，函数将返回 TRUE（0），如果传递字符串，则返回 FALSE（1）。

提示：以下表达式在`$1`不是整数时返回什么？

`expr $1 + 0`

**ASCII 转整数**

**C**中的*atoi*函数将字符串字符转换为整数。编写一个 shell 脚本函数执行相同的操作。同样，编写一个 shell 脚本函数执行相反的操作，类似于**C**中的*itoa*函数，该函数将整数转换为 ASCII 字符。

**管理磁盘空间**

逐个列出`/home/username`目录树中所有大于 100K 的文件。给用户选择删除或压缩文件，然后继续显示下一个文件。将所有已删除文件的名称和删除时间写入日志文件。

**横幅**

在脚本中模拟已弃用的 banner 命令的功能。

**删除非活动账户**

网络服务器上的非活动账户浪费磁盘空间，可能成为安全风险。编写一个管理脚本（由*root*或 cron 守护进程调用），检查并删除在过去 90 天内未被访问的用户账户。

**强制磁盘配额**

编写一个用于多用户系统的脚本，检查用户的磁盘使用情况。如果用户在`/home/username`目录中超过预设的限制（例如 500MB），则脚本将自动发送给她一个“过度使用”警告电子邮件。

该脚本将使用 du 和 mail 命令。作为选项，它将允许使用 quota 和 setquota 命令设置和强制配额。

**已登录用户信息**

对于所有已登录用户，显示他们的真实姓名以及他们上次登录的时间和日期。

提示：使用 who，lastlog 和解析`/etc/passwd`。

**安全删除**

实现一个“安全”的删除命令，`sdel.sh`。传递给此脚本的命令行参数的文件名不会被删除，而是如果尚未压缩（使用 file 检查），则会被 gzip 压缩，然后移动到`~/TRASH`目录。在调用脚本时，脚本会检查`~/TRASH`目录中是否有过期 48 小时以上的文件，并将它们永久删除。（更好的替代方案可能是有一个第二个脚本处理此任务，定期由 cron 守护进程调用。）

*额外加分:* 编写脚本使其能够递归地处理文件和目录递归。这将使其具有“安全删除”整个目录结构的能力。

**改变货币**

使用流通中的硬币（最多 25 美分）以最有效的方式为 1.68 美元找零是什么？答案是 6 个 25 美分，1 个 10 美分，1 个 5 美分和 3 个 1 美分。

给定任意命令行输入（以美元和美分表示，$*.??），使用最少的硬币计算零钱。如果你的家乡不是美国，你可以使用你当地的货币单位。脚本需要解析命令行输入，然后将其转换为最小货币单位（美分或任何其他单位）的倍数。提示：查看示例 24-8。

**二次方程**

解一个形式为`*Ax² + Bx + C = 0*`的*二次*方程。让脚本接受系数`**A**`，`**B**`和`**C**`作为参数，并以五位小数返回解。

提示：将系数通过 bc 管道传输，使用已知的公式，`*x = ( -B +/- sqrt( B² - 4AC ) ) / 2A*`。

**对数表**

使用 bc 和 printf 命令，以良好的格式打印出 0.00 到 100.00 之间的八位自然对数表，步长为.01。

提示：*bc*需要使用`-l`选项来加载数学库。

**Unicode 表**

使用示例 T-1 作为模板，编写一个脚本，将一个完整的 Unicode 表格打印到文件中。

提示：使用 echo 的`-e`选项：**echo -e '\uXXXX'**，其中`*XXXX*`是 Unicode 数值字符标识。这需要 Bash 的版本 4.2 或更高版本。

**匹配数字之和**

找出所有包含以下集合中的两个数字（4，5，6）的五位数（10000 - 99999）的总和。这些数字可以在同一个数字中重复出现，如果这样，每个出现都只计算一次。

一些*匹配数字*的例子是 42057，74638 和 89515。

**幸运数字**

一个*幸运数字*是指其各个位数相加等于 7 的数字，在连续相加中。例如，62431 是一个*幸运数字*（6 + 2 + 4 + 3 + 1 = 16，1 + 6 = 7）。找出 1000 到 10000 之间的所有*幸运数字*。

**骰子游戏**

从 Example A-40 借用 ASCII 图形，编写一个脚本，玩著名的赌场游戏*骰子游戏*。脚本将接受一个或多个玩家的赌注，掷骰子，并记录胜负以及每个玩家的银行余额。

**井字棋**

编写一个脚本，与人类玩家玩儿童游戏*井字棋*。脚本将让人类玩家选择是否先走。脚本将遵循最佳策略，因此永远不会输。为了简化问题，你可以使用 ASCII 图形：

```sh
   o &#124; x &#124;
   ----------
     &#124; x &#124;
   ----------
     &#124; o &#124;

   Your move, human (row, column)?
```

**字符串排序**

按 ASCII 顺序对从命令行读取的任意字符串进行排序。

**解析**

解析``/etc/passwd`，并以易于阅读的表格形式输出其内容。

**记录登录**

解析`/var/log/messages`以生成一个格式良好的用户登录和登录时间文件。脚本可能需要以*root*身份运行。（提示：搜索字符串"LOGIN"。）

**美化打印数据文件**

某些数据库和电子表格软件包使用以逗号分隔的字段保存文件，通常称为*逗号分隔值*或 CSV。其他应用程序通常需要解析这些文件。

给定一个以逗号分隔的字段（字段）的数据文件，形式如下：

```sh
Jones,Bill,235 S. Williams St.,Denver,CO,80221,(303) 244-7989
Smith,Tom,404 Polk Ave.,Los Angeles,CA,90003,(213) 879-5612
...
```

重新格式化数据，并按标签和均匀间隔的列打印到`stdout`。

**对齐**

给定来自`stdin`或文件的 ASCII 文本输入，调整单词间距以将每一行右对齐到用户指定的行宽，然后将输出发送到`stdout`。

**邮件列表**

使用 mail 命令，编写一个脚本，管理一个简单的邮件列表。脚本会自动将每月的公司通讯稿从指定的文本文件中读取，并发送到邮件列表上的所有地址，该列表由脚本从另一个指定的文件中读取。

**生成密码**

生成使用范围[0-9]、[A-Z]、[a-z]中的字符的伪随机 8 位密码。每个密码必须包含至少两个数字。

**监控用户**

你怀疑网络上的某个特定用户一直在滥用她的权限，并可能试图黑客攻击系统。编写一个脚本，在用户登录时自动监控和记录她的活动。日志文件将保存前一周的条目，并删除超过七天的条目。

你可以使用 last、lastlog 和 lastcomm 来帮助监视被怀疑的恶棍。

**检查断链**

使用带有`-traversal`选项的 lynx，编写一个脚本来检查网站上的断链。

**困难**

**测试密码**

编写一个脚本来检查和验证密码。目标是标记“弱”或容易被猜到的密码候选者。

将一个试验密码作为命令行参数输入到脚本中。要被认为是可接受的，密码必须满足以下最低要求：

+   最小长度为 8 个字符

+   必须包含至少一个数字字符

+   必须包含以下非字母字符之一：@, #, $, %, &, *, +, -, =

可选：

+   对密码测试中至少四个连续字母字符的每个序列进行字典检查。这将消除包含标准字典中发现的“单词”的密码。

+   使脚本能够检查系统上的所有密码。这些密码不位于`/etc/passwd`中。

这个练习测试了对正则表达式的掌握。

**交叉引用**

编写一个脚本，生成目标文件的*交叉引用*（*索引*）。输出将是目标文件中所有单词出现的列表，以及每个单词出现的行号。传统上，此类应用会使用*链表*结构。因此，你应该在这次练习中研究数组。示例 16-12 可能*不是*一个好的开始点。

**平方根**

编写一个脚本，使用*牛顿法*计算数字的平方根。

这个算法，用 Bash 伪代码表示，如下所示：

```sh
#  (Isaac) Newton's Method for speedy extraction
#+ of square roots.

guess = $argument
#  $argument is the number to find the square root of.
#  $guess is each successive calculated "guess" -- or trial solution --
#+ of the square root.
#  Our first "guess" at a square root is the argument itself.

oldguess = 0
# $oldguess is the previous $guess.

tolerance = .000001
# To how close a tolerance we wish to calculate.

loopcnt = 0
# Let's keep track of how many times through the loop.
# Some arguments will require more loop iterations than others.

while [ ABS( $guess $oldguess ) -gt $tolerance ]
#       ^^^^^^^^^^^^^^^^^^^^^^^ Fix up syntax, of course.

#      "ABS" is a (floating point) function to find the absolute value
#+      of the difference between the two terms.
#             So, as long as difference between current and previous
#+            trial solution (guess) exceeds the tolerance, keep looping.

do
   oldguess = $guess  # Update $oldguess to previous $guess.

#  =======================================================
   guess = ( $oldguess + ( $argument / $oldguess ) ) / 2.0
#        = 1/2 ( ($oldguess **2 + $argument) / $oldguess )
#  equivalent to:
#        = 1/2 ( $oldguess + $argument / $oldguess )
#  that is, "averaging out" the trial solution and
#+ the proportion of argument deviation
#+ (in effect, splitting the error in half).
#  This converges on an accurate solution
#+ with surprisingly few loop iterations . . .
#+ for arguments > $tolerance, of course.
#  =======================================================

   (( loopcnt++ ))     # Update loop counter.
done
```

这是一个足够简单的配方，乍一看似乎足够容易转换成可工作的 Bash 脚本。然而，问题是 Bash 没有对浮点数的原生支持。因此，脚本编写者需要使用 bc 或可能 awk 来转换数字并进行计算。这可能会变得相当混乱……

**记录文件访问**

记录单日内对`/etc`中文件的访问。这些信息应包括文件名、用户名和访问时间。如果文件有任何更改，将会标记出来。将这些数据以表格（制表符分隔）格式记录在日志文件中。

**监控进程**

编写一个脚本，持续监控所有运行进程，并跟踪每个父进程产生的子进程数量。如果一个进程产生了超过五个子进程，那么脚本将向系统管理员（或*root*）发送包含所有相关信息（包括时间、父进程 PID、子进程 PID 等）的电子邮件。脚本每十分钟将报告追加到日志文件中。

**删除注释**

从命令行指定的名称的 shell 脚本中移除所有注释。请注意，初始 #! 行 不能被移除。

**移除 HTML 标签**

从指定的 HTML 文件中移除所有 HTML 标签，然后将其重新格式化为长度在 60 到 75 个字符之间的行。根据需要重置段落和块间距，并将 HTML 表格转换为它们的近似文本等价物。

**XML 转换**

将 XML 文件转换为 HTML 和文本格式。

可选：一个将 Docbook/SGML 转换为 XML 的脚本。

**追踪垃圾邮件发送者**

编写一个脚本，通过在标题中的 IP 地址上执行 DNS 查询来分析垃圾邮件，以识别中继主机以及原始 ISP。该脚本将未修改的垃圾邮件消息转发给负责的 ISP。当然，将需要过滤掉 *你自己的 ISP 的 IP 地址*，这样你就不会对自己进行投诉。

如有必要，使用适当的 网络分析命令。

对于一些想法，请参阅 示例 16-41 和 示例 A-28。

可选：编写一个脚本，通过指定过滤器删除电子邮件列表中的垃圾邮件。

**创建 man 页面**

编写一个自动化创建 man 页面过程的脚本。

给定一个包含要格式化为 man 页面信息的文本文件，该脚本将读取文件，然后调用适当的 groff 命令，将相应的 *man 页面* 输出到 `stdout`。文本文件包含在标准 *man 页面* 标题下的信息块，例如 NAME、SYNOPSIS、DESCRIPTION 等。

示例 A-39 是一个有教育意义的初步步骤。

**十六进制转储**

对脚本作为参数指定的二进制文件进行十六进制转储。输出应整齐地显示在表格字段中，第一个字段显示地址，接下来的 8 个字段是 4 字节十六进制数，最后一个字段是前 8 个字段的 ASCII 等价物。

显然，接下来的步骤是将十六进制转储脚本扩展为反汇编器。使用查找表或其他巧妙的小技巧，将十六进制值转换为 80x86 操作码。

**模拟移位寄存器**

以 示例 27-15 为灵感，编写一个脚本，模拟一个 64 位移位寄存器作为 数组。实现加载寄存器、左移、右移和旋转寄存器的功能。最后，编写一个函数，将寄存器内容解释为八个 8 位 ASCII 字符。

**计算行列式**

编写一个脚本，通过递归地 展开 *余子式* 来计算行列式。使用 4x4 行列式作为测试案例。

**隐藏文字**

编写一个“单词寻找”谜题生成器，一个脚本，将 10 个输入单词隐藏在 10 x 10 的随机字母数组中。单词可以隐藏在横排、竖排或对角线上。

可选：编写一个脚本，*解决*单词寻找谜题。为了使问题不过于困难，解决方案脚本将只查找水平和垂直的单词。（提示：将每一行和每一列视为一个字符串，并搜索子字符串。）

**字母表**

四字母输入的字母表。例如，*word*的字母表是：*do or rod row word*。你可以使用`/usr/share/dict/linux.words`作为参考列表。

**单词梯子**

“单词梯子”是一系列单词，其中序列中的每个后续单词与前面的单词只差一个字母。

例如，从*mark*到*vase*的“梯子”：

```sh
mark --> park --> part --> past --> vast --> vase
         ^           ^       ^      ^           ^
```

编写一个解决单词梯子谜题的脚本。给定一个起始单词和一个结束单词，脚本将列出“梯子”中的所有中间步骤。请注意，序列中的*所有*单词都必须是合法的词典单词。

**雾度指数**

文本段落的“雾度指数”估计其阅读难度，作为一个大致对应于学校年级水平的数字。例如，雾度指数为 12 的段落应该对接受过 12 年学校教育的人是可理解的。

Gunning 版本的雾度指数使用以下算法。

1.  选择一段至少 100 个单词长度的文本。

1.  计算句子数量（文本部分被文本边界截断的部分算作一个句子）。

1.  找出每句话的平均单词数。

    AVE_WDS_SEN = TOTAL_WORDS / SENTENCES

1.  计算段落中“困难”单词的数量——那些包含至少 3 个音节的单词。将这个数量除以总单词数，得到困难单词的比例。

    PRO_DIFF_WORDS = LONG_WORDS / TOTAL_WORDS

1.  Gunning 雾度指数是上述两个数量的总和，乘以 0.4，然后四舍五入到最接近的整数。

    G_FOG_INDEX = int ( 0.4 * ( AVE_WDS_SEN + PRO_DIFF_WORDS ) )

第 4 步是这个练习中最困难的部分。存在各种算法用于估计单词的音节数量。一个经验公式可能会考虑单词中的字母数量和元音辅音混合。

严格解释 Gunning 雾度指数不计入复合词和专有名词为“困难”单词，但这将极大地复杂化脚本。

**使用蒲丰针计算π**

18 世纪法国数学家 de Buffon 提出了一个新颖的实验。反复将长度为`*n*`的针掉落在由长而窄的平行木板组成的木地板上。相隔固定距离`*d*`的木板裂缝是固定的。记录总的掉落次数和针与地板裂缝相交的次数。这两个数量的比率最终被证明是π的分数倍数。

按照示例 16-50 的精神，编写一个运行 *布丰针实验* 的蒙特卡洛模拟脚本。为了简化问题，将针的长度设置为裂缝之间的距离，`*n = d*`。

提示：实际上有两个关键变量：针中心到最近裂缝的距离，以及针相对于该裂缝的倾斜角度。你可以使用 bc 来处理计算。

**Playfair 密码**

在脚本中实现 Playfair (Wheatstone) 密码。

Playfair 密码通过替换 *双字母组*（2 个字母的组合）来加密文本。传统上使用 5 x 5 个字母的打乱顺序 *密钥方阵* 进行加密和解密。

```sh
   C O D E S
   A B F G H
   I K L M N
   P Q R T U
   V W X Y Z

Each letter of the alphabet appears once, except "I" also represents
"J". The arbitrarily chosen key word, "CODES" comes first, then all
the rest of the alphabet, in order from left to right, skipping letters
already used.

To encrypt, separate the plaintext message into digrams (2-letter
groups). If a group has two identical letters, delete the second, and
form a new group. If there is a single letter left over at the end,
insert a "null" character, typically an "X."

THIS IS A TOP SECRET MESSAGE

TH IS IS AT OP SE CR ET ME SA GE

For each digram, there are three possibilities.
-----------------------------------------------

1) Both letters will be on the same row of the key square:
   For each letter, substitute the one immediately to the right, in that
   row. If necessary, wrap around left to the beginning of the row.

or

2) Both letters will be in the same column of the key square:
   For each letter, substitute the one immediately below it, in that
   row. If necessary, wrap around to the top of the column.

or

3) Both letters will form the corners of a rectangle within the key square:
   For each letter, substitute the one on the other corner the rectangle
   which lies on the same row.

The "TH" digram falls under case #3.
G H
M N
T U           (Rectangle with "T" and "H" at corners)

T --> U
H --> G

The "SE" digram falls under case #1.
C O D E S     (Row containing "S" and "E")

S --> C  (wraps around left to beginning of row)
E --> S

=========================================================================

To decrypt encrypted text, reverse the above procedure under cases #1
and #2 (move in opposite direction for substitution). Under case #3,
just take the remaining two corners of the rectangle.

Helen Fouche Gaines' classic work, ELEMENTARY CRYPTANALYSIS (1939), gives a
fairly detailed description of the Playfair Cipher and its solution methods.
```

此脚本将包含三个主要部分

1.  根据用户输入的关键词生成 *密钥方阵*。

1.  加密 *明文* 消息。

1.  解密加密文本。

脚本将大量使用 数组 和 函数。你可以将 示例 A-56 作为灵感来源。

--

请不要向作者发送这些练习的解决方案。有更多合适的方式来展示你的聪明才智，例如提交错误修复和改进书籍的建议。

### 注意事项

| [[1]](writingscripts.html#AEN25254) | 对于所有那些未能通过中级代数考试的你聪明人，*行列式*是与多维 *矩阵*（数组）相关联的数值。

&#124;  ```sh For the simple case of a 2 x 2 determinant:

  &#124;a  b&#124;
  &#124;b  a&#124;

The solution is a*a - b*b, where "a" and "b" represent numbers.
```  &#124;

|
