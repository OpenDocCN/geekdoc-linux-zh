# 如何在 Python 中运行操作系统命令

> 原文：[`techbyexample.com/how-to-run-os-commands-in-python/`](https://techbyexample.com/how-to-run-os-commands-in-python/)

## **概述**

有很多方法可以实现，但我将讨论两种流行的方法。

+   **os.system**

+   **subprocess.Popen**

## **os.system**

它通过调用标准 C 函数 **system()** 在子 shell 中执行字符串命令。它有一些限制，例如 **sys.stdin** 等的更改不会反映在执行命令的环境中。它仅返回 **退出码**。

**0** – 退出码为 0 表示命令执行成功。

**非 0** – 退出码为 0 以外的值表示命令产生了某些错误。

注意：在 Unix 上，返回值是进程的退出状态，按 wait 方法指定的格式编码。在 Windows 上，返回值由系统 shell 返回。

**语法**：

```go
os.command(cmd);
```

**示例：运行操作系统命令**

```go
import os
p=os.system("ls -l")
print(p)
```

## **subprocess 模块**

所以当您需要命令的输出时，可以使用 subprocess 模块。它内置了多种方法，如 call、Popen，具有更强大的功能。

**subprocess.Popen** – 在新进程中执行子程序。

本模块允许您生成新的进程，连接其输入/输出/错误管道，并获取它们的 **输出、错误和返回码**。

**语法**：

```go
class subprocess.Popen(args, bufsize=0, executable=None, stdin=None, stdout
=None, stderr=None, preexec_fn=None, close_fds=False, shell=False, cwd=Non
e, env=None, universal_newlines=False, startupinfo=None, creationflags=0)
```

+   ***args*** – 程序参数的序列或一个字符串

+   ***bufsize*** – 在遇到性能问题时增加缓冲区大小，您可以将 *bufsize* 设置为 -1 或一个足够大的正值（例如 4096）。

+   ***preexec_fn*** – 该对象将在子进程执行之前被调用。（仅限 Unix）

+   ***close_fds*** (**True/False**) – 如果为 True，则在子进程执行之前，关闭所有文件描述符，除了 **0**、**1** 和 **2**。（仅限 Unix）。注意，在 Windows 上，您不能将 *close_fds* 设置为 true。

+   ***cwd*** – 更改子进程的当前工作目录。

+   ***env*** – 如果您不想继承当前进程的环境（默认行为），可以使用此参数将环境传递给子进程。

+   ***universal_newlines*** (**True/False**) – 如果为 True，则 stderr 和 stout 将采用通用换行符格式处理。

**注意：** 传递 **shell=True** 可能会带来安全风险，并且在运行命令之前需要使用 shell 类型的格式，因此需要小心。

**示例 1：读取命令的输出：**

```go
import subprocess

cmd="uname"

'''
subprocess.PIPE - Special value that can be used as the 
stdin, stdout or stderr argument to Popen and indicates
 that a pipe to the standard stream should be opened.
'''
#calling method with OS command
process = subprocess.Popen(cmd, stdout=subprocess.PIPE, stdin=subprocess.PIPE, stderr=subprocess.PIPE)
output = process.stdout.read().strip()
error = process.stderr.read().strip()
process.communicate()
exit_code = process.returncode

#you can process the output like below
if len(output) > 0:
    print('Output     : {}'.format(output))

#you can pprocess the error like below.
if len(error) > 0:
    print('Error      : {}'.format(error))

#you can take decision ased on command succcess or failureofthe command like below
print('Exit Code      : {}'.format(exit_code))

if exit_code != 0:
    raise Exception('Error executing command: {}. Exit Code: {}, Stdout: `{}`, Stderr: `{}`'.format(
        ' '.join(command_components), exit_code, output, error)
```

**示例 2：我们可以在列表中传递带有参数的命令**

```go
p = subprocess.Popen(["p4", "login", "-s"], stderr=subprocess.PIPE, stdout=subprocess.PIPE)
p.communicate()
if p.returncode != 0:
         sys.exit('Error: not logged into Perforce');
```

**示例 3：我们可以直接在字符串中传递命令。**

```go
p = subprocess.Popen("git ls-files", shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
stdout, stderr = p.communicate()
if p.returncode != 0:
	sys.exit('Error: could not get repo files - %s' % stderr)
```

+   [os](https://techbyexample.com/tag/os/)*   [python](https://techbyexample.com/tag/python/)
