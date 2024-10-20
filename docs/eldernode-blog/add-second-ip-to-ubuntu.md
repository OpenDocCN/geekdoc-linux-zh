# 如何添加第二个 IP 到 Ubuntu-temporary 和永久 IP 到 Ubuntu

> 原文：<https://blog.eldernode.com/add-second-ip-to-ubuntu/>

![How to add a second IP to Ubuntu](img/90b31903305bcbdc989847c1095cb53c.png)

对于网络用户来说，需要给操作系统的网卡添加多个 IP 是很平常的事情。所以今天你将学习如何给 Ubuntu 添加第二个 IP。之前我们说了关于在 Ubuntu 上设置 IP 静态的[，在本教程中，我们将回顾并教授期望的](https://eldernode.com/set-ip-static-on-ubuntu-20-04-lts-server-with-netplan/) [Linux 命令](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)。向 Ubuntu 添加第二个 IP 需要输入几个命令。

## 如何给 Ubuntu 添加第二个 IP

你可以通过两种方式向 Ubuntu 添加第二个 IP。

**1**–临时

**2**–永久

### 如何添加第二个临时 IP

### **1**–进入你的 Linux Ubuntu 终端环境。
**2** 。输入以下命令添加第二个 IP。

`sudo ifconfig eth0:0 192.168.1.2 netmask 255.255.255.0 up`

在上面的例子中，你应该将地址 192.168.1.2 添加到 Ubuntu 中作为第二个 IP。

**注:** 上述 IP 命令在 foot 系统重启前有效，在操作系统重启后将被删除。

你需要自己的 Ubuntu VPS 吗？加入我们[这里](https://eldernode.com/ubuntu-vps/)

如何添加第二个永久 IP

### **1**–进入你的 Linux Ubuntu 终端环境。

**2**–用编辑器打开/ **etc/network/interfaces** 文件。

**3**–在打开的文件末尾输入以下短语。

```
nano /etc/network/interfaces
```

输入您想要的 IP 地址，而不是书写的 IP 地址。

```
auto eth0:0  iface eth0:0 inet static  address 192.168.1.2 network 192.168.1.0  netmask 255.255.255.0
```

**注:** 本节重点是添加了 ***eth0:0*** 选项，必须遵循，其余几行就像设置**第一个** IP 一样。

**4**–保存文件并退出。

**5**–关闭/打开添加的虚拟卡一次，命令如下。

在这一部分中，附加的 **IP** 被完成。**观看**为了不失去我们的未来相关文章。

```
ifdown eth0:0 ifup eth 0:0
```

亲爱的用户，我们希望您喜欢本教程，您可以在评论区提出关于本次培训的问题，或者解决[老年节点培训](https://eldernode.com/blog/)领域的其他问题，请参考[提问页面](https://eldernode.com/ask)部分并在其中提出您的问题。

Dear user, we hope you would enjoy this tutorial, you can ask questions about this training in the comments section, or to solve other problems in the field of [Eldernode training](https://eldernode.com/blog/), refer to the [Ask page](https://eldernode.com/ask) section and raise your problems in it.