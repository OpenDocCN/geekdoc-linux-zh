# 如何在 Ubuntu 20.04 上安装 Ansible-在 Ubuntu 上安装 ansi ble 配置

> 原文：<https://blog.eldernode.com/install-ansible-on-ubuntu-20/>

![How to install Ansible on Ubuntu 20.04](img/d29e554391a5a55975cddfa71a03a51a.png)

作为管理员或运营团队，你需要选择配置管理系统，在这篇文章中你将学习**如何在 Ubuntu 20.04** 上安装 Ansible。这些系统旨在简化控制大量服务器的流程。它们还允许您从一个中心位置自动控制许多不同的系统。在流行的配置管理工具之间，如 Chef 和 Puppet， [Ansible](https://www.ansible.com/) 通过提供一个架构，是一个很好的选择，它不需要在节点上安装特殊的软件，使用 SSH 来执行自动化任务，使用 YAML 文件来定义配置细节。

**先决条件**

如果您知道以下内容，本教程可能会更有用:

## 如何在 Ubuntu 20.04 上安装 ansi ble

### 1- 安装 Ansible

要安装 Ansible，首先，刷新您系统的软件包索引

```
sudo apt update 
```

更新后，安装 Ansible 软件

```
sudo apt install ansible 
```

如果出现提示，按 Y 确认安装。 为了管理您的主机，Ansible 控制节点现在已经安装了所有必需的软件。

### 2- 建立库存档案

您将能够使用 Ansible 管理有关主机的信息，该信息在清单文件中提供。此外，您可以将主机组织成组和子组。清单文件能够包含一到数百台服务器，并设置变量，这些变量仅在用于行动手册和模板时对特定主机或组有效。一些变量如ansi ble _ python _ interpreter可能会影响剧本的运行方式。

打开 /etc/ansible/hosts 文件 在您的 ansible 控制节点 上，通过您的文本编辑器编辑您的默认 Ansible 库存的内容

下面的例子定义了一个名为【服务器】的组，其中有三个不同的服务器，每个服务器都有一个自定义别名:**服务器 1** 、**服务器 2** 和**服务器**。Ansible 安装提供的默认清单文件包含许多示例，您可以使用这些示例作为设置清单的参考。当您运行该命令时，请确保将突出显示的 IP 替换为您的主机的 IP 地址。

```
[servers]  server1 ansible_host=1.1.1.1  server2 ansible_host=1.1.1.2  server3 ansible_host=1.1.1.3    [all:vars]  ansible_python_interpreter=/usr/bin/python3 
```

按下 CTRL+X 保存并关闭文件，然后按下 Y 和 ENTER 确认您的更改。如果您想检查您的库存，运行以下命令。

```
ansible-inventory --list -y 
```

输出应该如下所示。

输出

```
all:    children:      servers:        hosts:          server1:            ansible_host: 1.1.1.1            ansible_python_interpreter: /usr/bin/python3          server2:            ansible_host: 1.1.1.2            ansible_python_interpreter: /usr/bin/python3          server3:            ansible_host: 1.1.1.3            ansible_python_interpreter: /usr/bin/python3      ungrouped: {}
```

[购买 Linux 虚拟私有服务器](https://eldernode.com/linux-vps/)

3- 测试连接

### 是时候检查 Ansible 是否能够通过 SSH 连接到这些服务器并运行命令了。由于 Ubuntu **root** 帐户通常是新创建的服务器上默认可用的唯一帐户，所以在这个过程中使用它。但是如果您的 Ansible 主机已经创建了一个常规的 sudo 用户，那么我们鼓励您使用这个帐户。要指定远程系统用户，请使用 -u 参数。如果没有提供，Ansible 将尝试在控制节点上作为您的当前系统用户进行连接。

从您的本地计算机或 Ansible 控制节点，您可以运行:

上面的命令将使用 Ansible 的内置 ping 模块在默认清单中的所有节点上运行连通性测试，以 **root** 身份连接。 ping 模块将测试:

```
ansible all -m ping -u root 
```

1.主机是否能够使用 Python 运行 Ansible 模块。

2.你是否有有效的 SSH 凭证。

3.主机是否可访问。

然后，输出应该显示如下:

输出

如果您是第一次通过 SSH 连接到这些服务器，您应该确认您通过 Ansible 连接到的主机的真实性。当询问您时，键入 yes 然后输入确认。当你从主机得到一个 pong 回复时，你可以在那个服务器上运行 Ansible 命令和剧本。

```
server1 | SUCCESS => {      "changed": false,       "ping": "pong"  }  server2 | SUCCESS => {      "changed": false,       "ping": "pong"  }  server3 | SUCCESS => {      "changed": false,       "ping": "pong"  }
```

4- 运行临时命令

### 当您的 ansible 控制节点能够与您的主机通信时，开始在您的服务器上运行临时命令和行动手册。让我们看一个例子，您可以使用以下命令检查所有服务器上的磁盘使用情况:

输出

```
ansible all -a "df -h" -u root
```

您可以用突出显示的命令 df -h. 替换您喜欢的命令。为了测试连接，您可以通过特别命令执行 Ansible 模块。例如，您可以使用 apt 模块在清单中的所有服务器上安装最新版本的 vim:

```
server1 | CHANGED | rc=0 >>  Filesystem      Size  Used Avail Use% Mounted on  udev            3.9G     0  3.9G   0% /dev  tmpfs           798M  624K  798M   1% /run  /dev/vda1       159G  2.3G  157G   2% /  tmpfs           3.9G     0  3.9G   0% /dev/shm  tmpfs           5.0M     0  5.0M   0% /run/lock  tmpfs           3.9G     0  3.9G   0% /sys/fs/cgroup  /dev/vda15      105M  3.6M  101M   4% /boot/efi  tmpfs           768M     0  768M   0% /run/user/0    server2 | CHANGED | rc=0 >>  Filesystem      Size  Used Avail Use% Mounted on  udev            2.0G     0  2.0G   0% /dev  tmpfs           395M  608K  394M   1% /run  /dev/vda1        88G  2.2G   86G   3% /  tmpfs           2.0G     0  2.0G   0% /dev/shm  tmpfs           5.0M     0  5.0M   0% /run/lock  tmpfs           1.0G     0  1.0G   0% /sys/fs/cgroup  /dev/vda15      115M  3.6M  111M   4% /boot/efi  tmpfs           375M     0  375M   0% /run/user/0    ...
```

运行 Ansible 命令时，您还可以将单个主机以及组和子组作为目标。检查服务器组中主机的正常运行时间。

```
ansible all -m apt -a "name=vim state=latest" -u root 
```

通过用冒号分隔来指定多个主机:

```
`ansible servers -a "uptime" -u root` 
```

```
```ansible server1:server2 -m ping -u root``` 
```

```亲爱的用户，我们希望这篇关于如何在 Ubuntu 20.04 上安装 Ansible 的教程能对你有所帮助，如果你有任何问题或者想查看我们用户关于这篇文章的对话，请访问[提问页面](https://eldernode.com/ask)。也为了提高你的知识，有这么多有用的教程为[老年节点培训](https://eldernode.com/blog/)准备。```

```亦作，见```

```[在 Ubuntu 20.04 上设置的初始服务器](https://eldernode.com/initial-server-set-up-on-ubuntu-20-04-lts/)```

```[教程 linux 上连接 ssh](https://eldernode.com/tutorial-connect-to-ssh-on-linux/)```

```[Tutorial connect to ssh on linux](https://eldernode.com/tutorial-connect-to-ssh-on-linux/)```