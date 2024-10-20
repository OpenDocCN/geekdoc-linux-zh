# 如何在 Fedora [PPA 方式] - Eldernode 博客上安装 Node.js

> 原文：<https://blog.eldernode.com/install-node-js-on-fedora/>

![How to Install Node.js on Fedora [PPA way]](img/d27f2d55714d36121cda1dbbcbee3fec.png)

JavaScript 是一种被称为 Web 内核的高级解释语言。web 开发人员最重要的愿望之一就是能够使用 JavaScript 进行服务器端编码，这样他们就不必使用 PHP、Python 等语言。实现应用服务器。随着 Node.js 的出现，开发人员的这一愿望得以实现，现在可以很容易地在服务器端使用 JavaScript 及其独特的功能。在本文中，我们将一步一步地教你如何在 Fedora 上安装 Node.js。如果你想买一个 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务器，你可以在 [Eldernode](https://eldernode.com/) 看到可用的软件包。

## **教程在 Fedora 上安装 node . js【PPA 方式】**

### **node . js 是什么？**

2009 年，一个叫 Ryan Dahl 的人介绍了 Node.js，但是 Node.js 的创建有什么故事呢？达尔先生首先使用 Ruby 语言进行编程。但是在看到 Flickr 上的进度条无法正常工作后，他认为通过将这个进度条连接到服务器上，他可以在瞬间从服务器上获取确切的进度，并将其显示在进度条上。为此，开始了 node js 的开发，并引入了 Node js。

Node.js 是一个基于 Google Chrome V8 JavaScript 引擎的服务器端平台。Node.js 的创建，证明了 JavaScript 比这几个字更强大，可以轻松在服务器上运行 JavaScript 程序。Node.js 也非常适合实现大型高消耗的应用，性能最好。

### **Node.js 功能**

Node.js 的一些最重要的特性是:

1_ 高效快速

2_ Node.js 是跨平台的

3_ Node.js 和微服务关系很好

4_ 创建水疗项目的能力(单页)

5_ 创建实时程序的能力

6_ 创建聊天程序和在线游戏的能力

## **使用 PPA 方式在 Fedora 上安装 node . js**

在这一节，我们将教你如何使用 PPA 方法在 Fedora 上安装 Node.js。在开始之前，您需要知道您必须是服务器上的非 root 用户，并且拥有 Sudo 特权。然后你就可以按照下面的步骤依次进行了。

首先，您可以运行以下命令来确保 Fedora 包的安装程序 yum 拥有最新的信息:

```
sudo yum check-update
```

```
sudo yum install
```

下一步，你需要**安装卷曲工具**。请注意，如果 Curl 工具包含在您的系统软件包中，则不需要安装它。卷曲工具是用来加载和安装 PPA。在下面的命令中，你可以用其他选项替换“ **16.x** ”:

```
sudo curl -sL https://rpm.nodesource.com/setup_16.x | bash -
```

最后，应该注意的是，如果您需要从 npm 编译和安装本机插件，您可以使用以下命令安装构建工具:

```
yum install gcc-c++ make
```

或者

```
yum groupinstall 'Development Tools'
```

成功安装 Node.js 后，可以通过运行以下命令来检查安装的版本:

```
node --version
```

### **如何运行演示 HTTP 服务器**

在本节中，我们将创建一个 web 服务器，文本为“ **Welcome to Node.js** ”。为此，您必须创建一个 **demo_server.js** 文件。因此，用您想要的文本编辑器打开配置文件:

```
vim http_demo_server.js
```

然后，您需要将以下内容添加到文件中:

```
var http = require('http');  http.createServer(function (req, res) {  res.writeHead(200, {'Content-Type': 'text/plain'});  res.end('Welcome to Node.js');  }).listen(3001, "127.0.0.1");  console.log('Server running at http://127.0.0.1:3001/');
```

最后，您可以通过运行以下命令来启动 web 服务器:

```
node --inspect http_demo_server.js
```

## 结论

Node.js 遵循事件驱动的模型。此外，这个模型根本不会阻塞输入和输出进程，因此使用这样的模型可以使 Node.js 运行时环境变得轻便而高效。在本文中，在完整介绍了 Node.js 及其特性之后，我们试图教您如何在 Fedora 上安装 node . js。如果你愿意，可以访问如何在 [Ubuntu](https://blog.eldernode.com/install-and-config-node-js-on-ubuntu-20-04/) 、 [Debian](https://blog.eldernode.com/install-node-js-on-debian-10/) 和 [CentOS](https://blog.eldernode.com/install-node-js-centos-7/) 上安装 Node.js。