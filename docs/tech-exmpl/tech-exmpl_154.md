# 如何在 Python 中登录 Perforce（p4 命令行）

> 原文：[`techbyexample.com/how-to-login-in-perforcep4-command-line-in-python/`](https://techbyexample.com/how-to-login-in-perforcep4-command-line-in-python/)

要在程序中登录 p4，您可以编写类似下面的脚本，它接受用户名和密码。这分为两个步骤进行。

**步骤 1：设置环境变量。**（在 ~/.bashrc 或 ~/.zshrc 或等效文件中创建条目并使其生效）

```go
export P4USER=myuser
export P4PORT=ssl:my.server.perforce.com:31211
```

source ~/.bashrc 或 ~/.zshrc

**步骤 2：使用‘p4 trust’信任机器并尝试登录**

以下代码中有两个函数，一个是用于运行操作系统命令，另一个是用于执行登录。在脚本中，我们首先检查机器与服务器之间是否建立了信任关系？然后进一步尝试登录。

```go
import os
import sys
import subprocess

#Utility function to run the OS commands
def execute_shell_command(commands, raise_if_error=True, capture_output=True):
    print('Running Command: %s' % ' '.join(commands))

    if capture_output:
        p = subprocess.Popen(commands, cwd=None, stdout=subprocess.PIPE, stdin=subprocess.PIPE, stderr=subprocess.PIPE)
        out = p.stdout.read().strip().decode('utf-8')
        err = p.stderr.read().strip().decode('utf-8')
    else:
        p = subprocess.Popen(commands)

    p.communicate()
    exit_status = p.returncode

    print('Output: Exit Status  : %s' % exit_status)

    if exit_status != 0 and raise_if_error:
        if capture_output:
            raise Exception('Error in running os command: %s. Exit Code: %d, Stdout: `%s`, Stderr: `%s`' % (' '.join(commands), exit_status, out, err))
        else:
            raise Exception('Error in running os command: %s. Exit Code: %d' % (' '.join(commands), exit_status))

    return exit_status

#Function to perform perforce login 
def p4_login(ticket, attempt_login=True):
    #First check trust is established between P4 server and the machine.
    if ticket is not None:
        exitStatus = execute_shell_command(['p4', 'trust', '-y', '-f'], raise_if_error=False)
        if exitStatus != 0:
            sys.exit('Error: p4 trust has been failed. Please check perforce documentation')

        exitStatus = execute_shell_command(['p4', '-P', ticket, 'monitor', 'show'], raise_if_error=False)
        if exitStatus != 0:
            sys.exit('Error: Ticket may be expired. please check.')
    else:
        exitStatus = execute_shell_command(['p4', 'login', '-s'], raise_if_error=False)
        if exitStatus != 0:
            if attempt_login:
                print('Please enter your Password:')
                execute_shell_command(['p4', 'login'], capture_output=False)
                p4_login(ticket, attempt_login=False)
            else:
                sys.exit('Error: You have not logged into Perforce successfully. Please check perforce documentation')

#Now use it something like this
p4_ticket = os.environ.get('P4_TICKET')
p4_login(p4_ticket)
```

**要使用该脚本，将上面的脚本保存在某个文件中，例如‘p4_login.py’，然后在终端中运行以下命令。**

```go
source ~/.bashrc or ~/.zshrc 
python3 p4_login.py
```

+   [p4](https://techbyexample.com/tag/p4/)*   [p4 登录](https://techbyexample.com/tag/p4-login/)*   [perforce](https://techbyexample.com/tag/perforce/)*   [python](https://techbyexample.com/tag/python/)*   [python3](https://techbyexample.com/tag/python3/)
