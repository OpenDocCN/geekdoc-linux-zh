# 第二十二章\. 受限壳

> 原文：[`tldp.org/LDP/abs/html/restricted-sh.html`](https://tldp.org/LDP/abs/html/restricted-sh.html)

**受限壳中的禁用命令**

**.** 在 *受限模式* 下运行脚本或脚本的一部分将禁用某些在其他情况下可用的命令。这是一个安全措施，旨在限制脚本用户的权限，并最小化运行脚本可能造成的损害。

以下命令和操作被禁用：

+   使用 `*cd*` 来更改工作目录。

+   修改 `*$PATH*`, `*$SHELL*`, `*$BASH_ENV*`, 或 `*$ENV*` 环境变量 的值。

+   读取或更改 `*$SHELLOPTS*`，壳的环境选项。

+   输出重定向。

+   调用包含一个或多个 / 的命令。

+   调用 exec 以替换壳的进程。

+   各种其他命令，这些命令可能会被用于篡改脚本或试图以非预期目的执行脚本。

+   在脚本中退出受限模式。

**示例 22-1\. 在受限模式下运行脚本**

```sh
#!/bin/bash

#  Starting the script with "#!/bin/bash -r"
#+ runs entire script in restricted mode.

echo

echo "Changing directory."
cd /usr/local
echo "Now in `pwd`"
echo "Coming back home."
cd
echo "Now in `pwd`"
echo

# Everything up to here in normal, unrestricted mode.

set -r
# set --restricted    has same effect.
echo "==> Now in restricted mode. <=="

echo
echo

echo "Attempting directory change in restricted mode."
cd ..
echo "Still in `pwd`"

echo
echo

echo "\$SHELL = $SHELL"
echo "Attempting to change shell in restricted mode."
SHELL="/bin/ash"
echo
echo "\$SHELL= $SHELL"

echo
echo

echo "Attempting to redirect output in restricted mode."
ls -l /usr/bin > bin.files
ls -l bin.files    # Try to list attempted file creation effort.

echo

exit 0
```
