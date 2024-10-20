# 如何在 Kali Linux - Eldernode 博客上安装和使用 Hashdeep

> 原文：<https://blog.eldernode.com/install-and-use-hashdeep-on-kali-linux/>

![How-to-Install-and-Use-Hashdeep-on-Kali-Linux](img/0c3b29a68024745d4a7dcb110f950379.png)

与检查和法律调查相关的工具之一是 Hashdeep。该工具使用不同的算法计算哈希摘要。在本文中，您将了解 Hashdeep，然后学习如何在 Kali Linux 上安装和使用 Hashdeep。如果你想购买的话，可以在 [Eldernode](https://eldernode.com/) 网站上查看 [**Linux VPS**](https://eldernode.com/linux-vps/) 服务。

## **教程在 Kali Linux 上安装使用 hash deep**

### **哈希迪普**简介

Hashdeep 是一套用于计算 MD5、SHA1、SHA256、tiger 和 whirlpool hashsums 的工具，可以同时用多种算法计算哈希返回。它还可以像来自 md5deep 家族的程序一样以强大的方式执行匹配操作。

### **Hashdeep**的特性

–将哈希与已知哈希列表进行比较。

–工具能够显示与列表匹配的项目或不匹配的项目。

–显示处理大文件的预计时间。

–块散列功能(以任意大小的块输入文件)。

–能够用于法医调查。

## **在 Kali Linux 上安装 hash deep**

在本文的后续部分，我们将介绍在 [Kali Linux](https://blog.eldernode.com/tag/kali-linux/) 上安装 Hashdeep 的三种方法:

1 –>如何使用 apt-get 安装 Hashdeep

2->如何使用 apt 安装 Hashdeep

3->如何使用 aptitude 安装 Hashdeep

### **如何使用 apt-get 在 Kali Linux 上安装 hash deep**

首先**使用以下命令更新 apt 数据库**:

```
sudo apt-get update
```

然后使用下面的命令**安装 Hashdeep** :

```
sudo apt-get -y install hashdeep
```

### **如何使用 apt** 在 Kali Linux 上安装 hash deep

使用以下命令**更新 apt 数据库**:

```
sudo apt update
```

然后运行下面的命令来**安装 Hashdeep** :

```
sudo apt -y install hashdeep
```

### **如何使用资质**在 Kali Linux 上安装 hash deep

首先**使用以下命令更新 apt 数据库**:

```
sudo aptitude update
```

然后**使用以下命令安装哈希德普**:

```
sudo aptitude -y install hashdeep
```

## **如何在 Kali Linux 上使用 Hashdeep 工具**

在下文中，我们将回顾 Hashdeep 的不同工具。

Hashdeep 计算、比较或审核多个消息的摘要。运行以下命令来使用此工具:

```
hashdeep -h
```

您可以使用带有以下命令的 **MD5deep 工具**来计算和比较 MD5 消息摘要:

```
md5deep -h
```

运行以下命令，使用下面的 **SHA1deep 工具**计算和比较 SHA1 消息摘要:

```
sha1deep -h
```

使用以下命令计算和比较 **SHA256 消息摘要**:

```
sha256deep -h
```

使用 **Tigerdeep 命令**计算和比较 Tiger 消息摘要:

```
tigerdeep -h
```

运行 **Whirlpooldeep 命令**计算和比较 Whirlpool 消息摘要:

```
whirlpooldeep -h
```

### **如何删除 Hashdeep 配置、数据及其所有依赖关系**

您可以使用以下命令删除 Hashdeep 配置、数据及其所有依赖项:

```
sudo apt-get -y autoremove --purge hashdeep
```

## 常见问题解答

[sp _ easy agreement]

## 结论

在本教程中，我们介绍了使用不同算法计算散列摘要的 Hashdeep，并回顾了它的各种特性。您还将学习在 Kali Linux 上安装 Hashdeep 的不同方法。