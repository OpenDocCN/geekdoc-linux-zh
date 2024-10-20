# 如何在 Ubuntu 20.04[Focal Fossa]-elder node 上配置 Gulp.js

> 原文：<https://blog.eldernode.com/configure-gulp-js-on-ubuntu-20-04/>

![How to Configure Gulp.js on Ubuntu 20.04 [Focal Fossa]](img/93d7650eabaa7ecf12656c8076c133e4.png)

一步步学习如何在 Ubuntu 20.04 【焦窝】上**配置 Gulp.js。Gulp 是 Eric Schoffstall 用 JavaScript 编写的免费开源工具。Gulp 是[节点的构建系统或任务管理器。Js](https://blog.eldernode.com/install-node-js-centos-8/) 环境和 npm 包管理器。该工具用于**前端**的编程和开发。在 Gulp 的帮助下，一系列重复耗时的任务都可以自动完成。通过这种方式，程序员的工作变得稍微容易一点，项目执行的速度大大提高。在本文中，我们将教你如何在 Ubuntu 20.04 上配置 Gulp.js。如果需要购买 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 服务器，还可以在 [Eldernode](https://eldernode.com/) 中看到可用的包。**

### 什么是 Gulp.js，有什么用途？

Gulp.js 是一个任务运行器，使用它你可以加快网页设计和开发中许多重复的任务和过程以及时间，并且享受网站设计！这个工具不只是前端程序员用的，它的包也可以用于服务器端编程。Gulp 使用小型、单一用途的包来执行各种任务，每个部分在一个包中完成工作。Gulp 目前有近 4000 种不同的包装。

比如代码压缩、优化、单元测试、将 Sass 文件转换为 CSS、创建 HTML 模板、压缩图像、创建本地主机环境等。可以尽可能容易地用吞咽来完成。只要安装 Gulp，这个工具就会自动为你做这些事情。

正如我们所说，Gulp 是一个基于 JavaScript 的工具，它基于节点流，可以在软件开发过程中自动化许多常见的任务。虽然 Gulp 有西兰花、Grunt 等竞争对手。，由于其流式传输和速度，它比竞争对手具有更高的价值。一般来说，使用 Gulp 您可以执行以下操作:

**–**轻松移动文件。(例如，从项目文件夹到 Web 文件夹)

**–**轻松合并文件。

**–**更改文件类型。(例如将 Sass 文件转换为 CSS)

**–**优化文件。(包括 CSS 文件、JavaScript、图片等。)

## 教程在 Ubuntu 20.04 焦窝上配置 gulp . js

在 Node.js 中编码时，Gulp 会帮助你自动识别并完成痛苦的编码段。它还通过不留下分号等来防止可能的错误。，并最终组织和优化您的代码。

要安装 Gulp，必须先安装 Node.js，然后再安装。因此，使用以下命令，首先**在你的 [Ubuntu](https://blog.eldernode.com/tag/ubuntu/) 20.04 上安装 Node.js** ，然后安装 Gulp:

***注意:*** 要在 Ubuntu 20.04 上安装 Gulp，需要 root 权限或者 Sudo 用户。因此，如果不使用 root 用户输入命令，则必须在下一个命令之前输入 Sudo 命令。

### 如何在 Ubuntu 20.04 上安装 Node.js【焦点窝】

首先输入以下命令安装 **[Python](https://blog.eldernode.com/install-python-3-ubuntu-20/) 软件属性包**:

```
apt-get install python-software-properties
```

现在输入以下命令下载并安装 Node.js:

```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
```

```
apt-get install nodejs
```

您必须输入以下命令来显示 **NPM** 和**节点**版本。如果在输入这两个命令后都得到了正确的答案，这意味着这两个软件包都已正确安装。

```
node --version
```

```
npm --version
```

***Node.js 在 Ubuntu Focal Fossa 上安装成功完成。***

现在，在本教程的继续中，我们将安装 Gulp。

### 如何在 Ubuntu 20.04 上安装 Gulp.js【焦窝】

输入以下命令开始下载并安装 Gulp:

```
npm install -g gulp
```

安装后，输入以下命令以了解系统上安装的正确性和版本:

```
gulp --version
```

## 结论

今天，前端编程和站点实现已经经历了许多变化。单纯依靠 HTML 和 CSS 进行前端编程的网站比较少。今天，用户界面设计和编程技术如此先进，以至于一些程序员只专注于一个领域，比如前端。Gulp 是一种新技术，它让开发人员不必做重复性的、有时是乏味的工作。由于 Gulp 的重要性和应用性，我们在本文中尝试教大家如何在 Ubuntu 20.04 [Focal Fossa]上安装和配置 Gulp.js。