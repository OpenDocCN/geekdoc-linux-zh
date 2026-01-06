# 第六章. 退出和退出状态

> 原文：[`tldp.org/LDP/abs/html/exit-status.html`](https://tldp.org/LDP/abs/html/exit-status.html)

|   |  **... 在 Bourne shell 中存在一些暗角，人们都使用了它们。**

*--Chet Ramey**  |

**exit** 命令终止脚本，就像在 **C** 程序中一样。它也可以返回一个值，该值对脚本的父进程可用。

每个命令都返回一个 *退出状态*（有时也称为 *返回状态* 或 *退出码*）。成功的命令返回 0，而不成功的命令返回非零值，通常可以解释为 *错误码*。行为良好的 UNIX 命令、程序和实用程序在成功完成后返回 0 退出码，尽管有一些例外。

同样，脚本内的 函数 和脚本本身也会返回一个退出状态。函数或脚本中最后执行的命令决定了退出状态。在脚本中，可以使用 `**exit `*nnn*`**` 命令向 shell 传递 `*nnn*` 退出状态（*nnn* 必须是 0 - 255 范围内的整数）。

| ![Note](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | 当脚本以没有参数的 **exit** 结束时，脚本的退出状态是脚本中最后执行的命令的退出状态（在 **exit** 之前）。

&#124;  ```sh #!/bin/bash

COMMAND_1

. . .

COMMAND_LAST

# Will exit with status of last command.

exit
```  &#124;

等效于裸露的 **exit** 是 **exit $?** 或甚至可以省略 **exit**。

&#124;  ```sh #!/bin/bash

COMMAND_1

. . .

COMMAND_LAST

# Will exit with status of last command.

exit $?
```  &#124;

&#124;  ```sh #!/bin/bash

COMMAND1

. . . 

COMMAND_LAST

# Will exit with status of last command.
```  &#124;

|

`$?` 读取最后执行的命令的退出状态。函数返回后，`$?` 提供函数中最后执行的命令的退出状态。这是 Bash 为函数提供“返回值”的方式。1

在管道执行之后，`$?` 提供最后执行的命令的退出状态。

脚本终止后，命令行中的 `$?` 提供脚本的退出状态，即脚本中最后执行的命令，按照惯例，成功时为 `**0**`，错误时为 1 - 255 范围内的整数。

**示例 6-1\. exit / exit status**

```sh
#!/bin/bash

echo hello
echo $?    # Exit status 0 returned because command executed successfully.

lskdf      # Unrecognized command.
echo $?    # Non-zero exit status returned -- command failed to execute.

echo

exit 113   # Will return 113 to shell.
           # To verify this, type "echo $?" after script terminates.

#  By convention, an 'exit 0' indicates success,
#+ while a non-zero exit value means an error or anomalous condition.
#  See the "Exit Codes With Special Meanings" appendix.
```

$? 在测试脚本中命令的结果时特别有用（参见示例 16-35 和 示例 16-20)。

| ![Note](img/068cd6dc5ba56f1437f9ae6e0cb52ede.png) | !，即 *逻辑非* 修饰符，反转测试或命令的结果，并影响其 退出状态。

**示例 6-2\. 使用 ! 取消条件**

&#124;  ```sh true    # The "true" builtin.
echo "exit status of \"true\" = $?"     # 0

! true
echo "exit status of \"! true\" = $?"   # 1
# Note that the "!" needs a space between it and the command.
#    !true   leads to a "command not found" error
#
# The '!' operator prefixing a command invokes the Bash history mechanism.

true
!true
# No error this time, but no negation either.
# It just repeats the previous command (true).

# =========================================================== #
# Preceding a _pipe_ with ! inverts the exit status returned.
ls &#124; bogus_command     # bash: bogus_command: command not found
echo $?                # 127

! ls &#124; bogus_command   # bash: bogus_command: command not found
echo $?                # 0
# Note that the ! does not change the execution of the pipe.
# Only the exit status changes.
# =========================================================== #

# Thanks, Stphane Chazelas and Kristopher Newsome.
```  &#124;

|

| ![Caution](img/05aa79b283e0d53b5a94a522ee0b6cfe.png) | 某些退出状态码有 保留含义 并且不应在脚本中由用户指定。 |
| --- | --- |

### 备注

| [[1]](exit-status.html#AEN2981) | 在那些没有 返回 终止函数的实例中。 |
| --- | --- |
