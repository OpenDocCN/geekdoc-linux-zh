# 了解如何将组策略应用到 Active Directory 中的计算机

> 原文：<https://blog.eldernode.com/apply-group-policy-to-a-computer-in-active-directory/>

![Learn how to apply Group Policy to a computer in Active Directory](img/fcf704ede78b7ee3afc10cfbee64c9ed.png)

【更新日期:2021-01-29】如你所知，GPO 正在应用于计算机。最常见的方法是将计算机的 [GPO](https://en.wikipedia.org/wiki/Group_Policy) 链接到计算机 OU。默认情况下，策略适用于包含 OU 的所有计算机。如果只有少数特定计算机有特定的策略，那么这些计算机应该在 Active Directory 计算机组中。在本文中，我们将**学习如何在 Active Directory** 中对计算机应用组策略。你可以访问 [Eldernode](https://eldernode.com/) 中提供的包来购买 [Windows VPS](https://eldernode.com/windows-vps/) 服务器。加入我们来学习这个教程。

## **教程将组策略应用到 Active Directory 中的计算机**

在本教程中，我们将了解如何将 **GPO** 应用到 Active Directory 中的计算机组。这种方法比为想要这样做的计算机创建一个新的 OU 要有效得多。

### **将组策略应用到 Active Directory 中的计算机**

在本例中，计算机位于名为 asaputra.com 的域中，域控制器安装在 r 2 版的 Windows Server 2012 上。所有客户端计算机都安装了 Windows 10，并处于生产阶段。名为“安全计算机策略”的策略已经创建并链接到名为“Prod”的 OU。

![Active directory users and computers](img/a734ab12eff7fa4fc7f9cdff6c8703d7.png)

***

![Group Policy management](img/322ff5e96627d0e5e56b9ec51e7427d4.png)

### **如何过滤安全计算机策略以应用于 WKS002 和 WKS003**

以下分步说明显示了如何过滤适用于 **WKS002** 和 **WKS003** 的安全计算机策略。

#### **创建群组**

该组应建立在策略所链接的 OU 中。在 **Active Directory** 用户和计算机控制台上打开 **OU** 。

右键点击页面的空白区域，选择**新建** > > **组**。

![Create a group in Active Directory](img/9b140562b82dab36247b6ed6e4ed7bf9.png)

输入组名。

在**全局范围**部分，选择**全局**。

从**组类型**部分，选择**安全**。

![Group name and object type in Active Directory](img/437cdcc16425b5c651b471d38223e4b3.png)

点击**确定**保存设置并创建群组。如下所示，组必须显示在 OU 中。

![Create Group in Active Directory](img/f26128154280cb17b613dd50efb08b53.png)

**添加目标电脑为群组成员**

#### 双击**组名**打开设置。选择**成员**选项卡，点击**添加**按钮。

![Secured computers properties in Active Directory](img/ce132458033b059b4749e30cf30133e0.png)

将打开以下窗口。点击**对象类型**并确保计算机已勾选。

![How to select object types in active directory](img/0c7decf71318e4d4293d78ee951f0a9d.png)

***

![how to select types of objects in active directory](img/91558f813b6e2ef7b23854b4f2c293ee.png)

现在用分号**(；)**把它们分开。然后点击**查看姓名**。如果键入正确，名称将如下所示显示，下面有一个破折号。

![How to enter the object names in active directory](img/da6cbf588d9f17177053a79a593adbfe.png)

确保所有目标计算机都是该组的成员，然后单击 **OK** 确认。

**更改 GPO 安全设置**

#### 登录到组策略控制台。选择您想要更改的策略，然后进入**范围**选项卡。

![scope tab in secured computer policy](img/c64aac9d6a366fae4a072f96909b5fe4.png)

在**安全过滤**部分，选择**认证用户**，点击**移除**。

![security filtering in Active Directory](img/08c28b3993ae3c04c44886be2bdc7f45.png)

在同一个**安全过滤**区域，点击**添加**按钮。

![add or remove in security filtering in Active Directory](img/62644b2dd026e0d6ac7e9f4bcb1cc461.png)

输入在上一步中创建的组的名称。点击**检查姓名**确保输入的姓名正确，然后点击**确定**。

![How to select user in Active Directory](img/a70ad6fe386474b8c828a1c7291e362a.png)

确保该组已添加到列表中。

![security filtering in active directory](img/14077f3c56bce7ec7935d98680f1eb9a.png)

如何检查正确应用的策略

### 我们可以确保政策得到正确应用。在客户端计算机上，以管理员身份运行 **cmd** 并输入命令 **gpresult / r / SCOPE COMPUTER** 。在属于 **SECURED_COMPUTER** 组(即 WKS002 和 WKS003)的计算机上，您会看到结果被正确应用。

![How to apply group policy objects](img/4b3d375c8057e35cdd134eec16bb2c72.png)

但是在组外的计算机上，结果显示没有应用任何策略。

![How to apply group policy objects](img/15246959d109cd39ea70bdcb294e21a1.png)

请记住，将 GPO 应用于计算机组有点令人困惑。如果您看到一个 GPO 尚未应用到作为目标组成员的计算机，那么该计算机可能还没有意识到它是该组的成员。要检查您的计算机成员资格，请使用上面的命令并向下滚动查看下面的信息。

![How to apply group policy objects](img/2d86d42a2c3916a9ff9a6b9f16c127aa.png)

结论

## 在本文中，我们试图全面解释如何使用 Windows Server 2012 中的映像将组策略应用于 Active Directory 中的计算机。如何筛选安全计算机策略以应用于 WKS002 和 WKS003 也是一步一步教授的。最后，对 CMD 环境下的工作结果进行了评估。

In this article, we tried to fully explain how to apply Group Policy to a computer in Active Directory using images in Windows Server 2012\. How to filter Secured Computer policy to apply to WKS002 and WKS003 was also taught step by step. Finally, the results of the work in the CMD environment were evaluated.