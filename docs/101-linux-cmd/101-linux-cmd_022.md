# `finger` 命令

`finger` 命令通过查询 `/etc/passwd`、`/var/run/utmp` 和 `/var/log/wtmp` 等文件来显示本地系统用户的信息。这是一个本地命令，不依赖于任何服务或守护进程来运行。此命令有助于快速检索与登录时间、空闲状态和其他系统信息相关的用户相关细节。

### 示例：

1.  查看特定用户的详细信息。

```sh
      finger abc 

```

*输出*

```sh
      Login: abc                          Name: (null)
Directory: /home/abc                Shell: /bin/bash
On since Mon Nov  1 18:45 (IST) on :0 (messages off)
On since Mon Nov  1 18:46 (IST) on pts/0 from :0.0
New mail received Fri May  7 10:33 2013 (IST)
Unread since Sat Jun  7 12:59 2003 (IST)
No Plan. 

```

1.  查看用户的登录详情和空闲状态。

```sh
      finger -s root 

```

*输出*

```sh
      Login     Name       		Tty      Idle  Login Time   Office     Office Phone
root         root           *1    19d Wed 17:45
root         root           *2     3d Fri 16:53
root         root           *3        Mon 20:20
root         root           *ta    2  Tue 15:43
root         root           *tb    2  Tue 15:44 

```

### 语法：

```sh
      finger [-l] [-m] [-p] [-s] [username] 

```

### 其他标志及其功能：

| **标志** | **描述** |
| :-- | :-- |
| `-l` | 强制长输出格式。 |
| `-m` | 仅匹配用户名（不是名字或姓氏）。 |
| `-p` | 在长格式打印输出中抑制打印 .plan 文件。 |
| `-s` | 强制短输出格式。 |

### 其他信息：

**默认格式**：

默认格式包括登录名、全名、终端名和写入状态等项目。该命令提供有关空闲时间、登录时间和特定站点信息的详细信息。

**长格式**：

在长格式中，命令会添加有关用户主目录、登录外壳以及 `.plan` 和 `.project` 文件内容的详细信息。

* * *

## 隐私考虑

虽然 `finger` 命令对于检索系统用户信息很有用，但它可能在共享或多用户环境中暴露敏感细节：

1.  **用户名和登录时间**：显示登录时间，可用于跟踪用户活动。

1.  **主目录**：暴露用户主目录的路径。

1.  **空闲状态**：显示用户多久没有活动，可能表明他们是否正在积极使用系统。

1.  **邮件状态**：显示邮件信息，可能无意中透露用户参与情况。

### 潜在风险：

在不受信任用户的环境中，`finger` 命令暴露的信息可能被用于：

+   **社会工程攻击**：恶意行为者可能利用这些信息来制作个性化的钓鱼攻击。

+   **时间攻击**：知道用户何时空闲或活跃可能会给攻击者在时间上尝试攻击带来优势。

+   **针对性攻击**：了解用户主目录的知识可以集中攻击那些位置。

### 缓解隐私风险：

为了缓解这些风险，考虑在用户隐私重要的环境中限制对 `finger` 命令的访问。

* * *

## `in.fingerd` 服务

区分 `finger` 命令和 **`in.fingerd` 服务** 很重要。`finger` 命令是本地的，而 `in.fingerd` 是一个网络守护进程，允许远程查询用户信息。由于潜在的安全风险，此服务在现代系统中通常默认禁用。

如果启用，`in.fingerd` 服务可能会通过网络暴露用户信息，这可能被攻击者利用。为了缓解这种风险，系统管理员应确保在不需要该服务时禁用该服务。

### 禁用 `in.fingerd` 服务：

如果您担心远程查询，您可以禁用`in.fingerd`服务：

```sh
      sudo systemctl disable in.fingerd
sudo systemctl stop in.fingerd 

```

通过禁用`in.fingerd`服务，您可以防止远程查询用户信息，从而增强系统安全性。
