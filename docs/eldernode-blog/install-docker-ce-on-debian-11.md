# 教程在 Debian 11[牛眼] - Eldernode 博客上安装 Docker CE

> 原文：<https://blog.eldernode.com/install-docker-ce-on-debian-11/>

![Tutorial Install Docker CE on Debian 11 [Bullseye]](img/cf2af14fb61946c7d51119d57840910b.png)

Docker Community Edition 或 Docker CE 是一个免费的开源工具，具有用于应用程序容器化的容器执行环境。在本文中，我们将一步步教你如何在 Debian 11【牛眼】上安装 Docker CE。另外，如果你想购买一个 [**Linux VPS**](https://eldernode.com/linux-vps/) 主机，你可以访问 [Eldernode](https://eldernode.com/) 中的软件包。

## **如何在 Debian 11 上安装 Docker CE【牛眼】**

Docker 是一个工具，旨在使使用容器创建、使用和执行应用程序变得容易。容器允许开发人员将应用程序与它需要的所有部分、组件打包在一起，并作为一个包进行传输。

### **先决条件在 Debian 上安装 Docker CE:**

需要一台高性能的 [Debian 11](https://blog.eldernode.com/initial-server-setup-on-debian-11/) 服务器(推荐 SSD 或 Nvme 存储)

具有 SSH 或控制台访问权限的服务器

### **在 Debian 11 Linux 上安装所需的依赖项**

首先，用下面的命令更新 Debian 服务器。

```
sudo apt update
```

然后，您应该使用下面的命令安装 Docker 在 debian linux 服务器上运行所需的依赖项。

```
sudo apt install apt-transport-https lsb-release ca-certificates curl gnupg -y
```

### **在 Debian 服务器上安装 Docker 资源库**

使用下面的链接下载 Docker GPG 密钥，然后添加它。

```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

然后使用以下命令添加下载的存储库:

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### **在 Debian** 上安装 Docker 引擎

```
sudo apt -y install docker-ce docker-ce-cli containerd.io
```

然后，用下面的命令来验证安装和 Docker 版本。

```
sudo docker version
```

在这一步中，使用下面的命令使 Docker 服务在系统引导时启动。

```
sudo systemctl enable docker
```

最后，检查 Docker 服务状态，查看 Docker 是启用还是禁用。

```
sudo systemctl status docker
```

### **如何考 Docker**

首先使用以下命令运行入门 docker 容器:

```
sudo docker run -d -p 80:80 docker/getting-started
```

然后在浏览器中输入服务器的 IP 地址并检查状态。

```
http://server_ip
```

## 结论

在本文中，我们试图讨论如何在您的 Debian 11 服务器上安装 Docker CE。我们希望本教程对你有所帮助。