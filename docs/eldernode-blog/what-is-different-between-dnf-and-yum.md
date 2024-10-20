# DNF 和 yum【快评】- Eldernode 有什么不同

> 原文：<https://blog.eldernode.com/what-is-different-between-dnf-and-yum/>

![what is different between DNF and yum [Quick review]](img/50d28513d7c17c2e972a36fff6e62d25.png)

[更新日期:2021-02-02]CentOS 8 发布后，有人问为什么百胜用 DNF 取代了他，DNF 和百胜有什么不同。所以我们决定发表一篇文章，看看 DNF 和百胜之间有什么不同。你可以访问 [Eldernode](https://eldernode.com/) 的套装来购买 [VPS 服务器](https://eldernode.com/vps/)。请继续关注本文的其余部分。首先，你需要知道 DNF 是百胜的下一代，通常来说，它可能比百胜更好。

## 什么是 DNF

DNF 代表 Dandified YUM，是一个基于 RPM 的 Linux 发行版的软件包管理器。它用于在 Fedora/RHEL/ [CentOS](https://blog.eldernode.com/tag/centos/) 操作系统中安装、更新和删除软件包。

它是 Fedora 22、CentOS8 和 RHEL8 的默认软件包管理器。DNF 是百胜的下一代版本，旨在取代百胜基于 RPM 的系统。它功能强大，拥有比 yum 更强大的特性。DNF 使得维护包组变得容易，并且能够自动解决依赖性。

## DNF 和百胜有什么不同

百胜包装经理已被 DNF 包装经理取代，因为百胜的许多长期问题仍未解决。

这些问题包括性能差、内存使用过多、依赖性解析速度慢。

DNF 使用“libsolv”进行依赖关系解析，由 SUSE 开发和维护以提高性能。

它大部分是用 python 编写的，并且有自己处理依赖关系解析的方式。

它的 API 没有完全文档化，它的扩展系统只允许 Python 插件。

Yum 是 rpm 的前端工具，它管理依赖项和存储库，然后使用 RPM 来安装、下载和删除包。

### 为什么用 DNF 而不是百胜？

最终，百胜被分入 DNF 有三个主要原因。这些原因由来已久，也足够严重，百胜不得不离开。原因是:

**1。一个未记录的 API:** 这意味着开发者要做更多的工作。为了让开发人员做他们需要的事情，经常需要浏览 Yum 代码库，以便能够编写一个调用。这意味着开发非常缓慢。

**2。Python 3:** CentOS/RHEL8 即将转向 Python 3，Yum 将无法承受这种变化，而 DNF 可以使用 Python 2 或 3 运行。

**3。依赖求解算法:**这是 Centos 包管理器长久以来的一个致命弱点。DNF 使用最先进的基于可满足性(SAT)的依赖性求解器。这是在 [SUSE](https://www.suse.com/) 和 openSUSE 的 Zypper 中使用的相同类型的依赖解算器。

简单来说，yum 已经过时了，经不起现代 CentOS 8 发行版的严苛考验。您可以访问 Eldernode 中提供的软件包来购买 [CentOS VPS](https://eldernode.com/centos-vps/) 服务器。

### DNF 指挥的好处

你必须从两个不同的角度来看待这个问题:最终用户和开发者。如果你是最终用户，从百胜转向 DNF 意味着一件非常简单的事情:更可靠的体验。这种可靠性来自于 DNFs 高级依赖解决方案。

现在，您安装软件包时，系统无法解析依赖关系的情况非常罕见。这个系统更聪明。

在软件包安装期间，最终用户也将看到更少的内存使用。安装和升级也将更快。最后一点应该特别重要。使用 Yum 工具运行升级开始变得非常慢。

如果你是一名开发人员，转移到 DNF 意味着你将能够更高效、更可靠地工作。所有公开的 API 都有文档记录。对开发人员来说，另一个好处是将实现 C。开发者创造了鹰眼和 librepo。

他们还将在未来发布更多基于 C 的 API。考虑到 C 仍然是如此广泛使用的语言。所以这对于开发者来说应该是一个受欢迎的变化。

在这篇文章的最后，如果你需要学习如何使用 DNF 和 DNF 教程，你可以使用 [DNF 命令教程](https://eldernode.com/dnf-command-on-centos-8/)文章。

## 结论

我们尝试在本文“DNF 和百胜的不同之处”中解释百胜和 DNF 以及它们之间的不同之处，并检查一些 DNF 命令的好处以及 CentOS 8 为什么将此作为默认设置。我们希望这篇文章“DNF 和百胜的不同之处”是有用的。