# 如何用 Virtualizor 创建备份服务

> 原文：<https://blog.eldernode.com/create-backup-service-with-virtualizor/>

![How to Create Backup Service with Virtualizor](img/1600836291b9d51ad7e5596f434f6bc3.png)

Virtualizor 是一个强大的基于 web 的 VPS 控制面板，允许用户一键部署和管理 VPS 服务器。Virtualizor 支持 KVM 虚拟化技术以及其他一些技术。在本文中，我们将教你如何用 Virtualizor 创建备份服务。需要注意的是，如果你想买一个[专用服务器](https://eldernode.com/dedicated-server/)，可以访问 Eldernode 网站上提供的平面图和位置。

## **虚拟人简介**

Virtualizor 是 Softaculous 开发的一个方便易用的控制面板。它还拥有强大的 API 和实时统计数据，以及许多其他功能。管理员只需点击一个按钮就可以创建 VPS。VPS 用户可以使用高度先进的基于 web 的 GUI 来启动、停止、重启和管理他们的 VPS。您可以通过 Virtualizor 从管理面板管理备份、媒体和格式。请继续关注这篇文章，学习如何使用 Virtualizor 创建备份服务。

### **如何用虚拟器添加备份服务器**

在熟悉了 [Virtualizor](https://blog.eldernode.com/tag/virtualizor/) 控制面板之后，在本节中，我们想学习如何添加备份服务器。您可能知道，Virtualizor 支持三种类型的备份:本地、SSH 和 FTP。

您应该注意的一点是，在本地备份的情况下，VPS 映像备份在节点服务器自己的目录中。还应该注意，对于远程备份，您需要添加备份服务器的详细信息，这可以使用 Add Backup Server API 来完成。

要添加备份服务器，你需要去 **Virtualizor 管理面板>服务器>添加备份服务器**。

成功添加备份服务器后，您需要确保将其添加到需要备份的 VPS 文件的备份计划中。或者，在添加备份服务器后，您应该使用测试备份连接 API 来测试到备份服务器的连接。
最后，当您在添加备份服务器时传递 gen_key，您将在 API 响应中收到公钥和私钥，如以下命令所示:

```
[keys] => Array  (  [public_key] => ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFvWP35.... [[email protected]](/cdn-cgi/l/email-protection)    [private_key] => -----BEGIN RSA PRIVATE KEY-----  MIIEowIBAAKCAQEAxb1j9+b66jRBBPRtVq+znA9nlqiGtW1WgcDN+0T0PTBb8Py5  CuWiUFQ4spQyqkRXi9Y1JT4g3+tAFHEpyCjZttoZivWE5gF/sL8IpG4pUmcVOwut  yjiWW7yglPSVLsqIyCP4ooFJGQ8Bokx7k+zORlrLfAJC+F6fwIe/RyxjZOdD88S9......    [path] => /var/virtualizor/ssh-keys/  )
```

下一步，您需要将公钥添加到备份服务器的 **/root/中。ssh/authorized_keys** 喜欢下面的命令:

```
<?php    require_once('/usr/local/virtualizor/sdk/admin.php');    $key = 'your_api_key';  $pass = 'your_api_pass';  $ip = 'your_server_ip';    $admin = new Virtualizor_Admin_API($ip, $key, $pass);    $post = array();  $post['hostname'] = 'testhost';  $post['name'] = 'backupstest';  $post['type'] = 'SSH';  $post['username'] = 'test';  $post['password'] = 'test123';  $post['gen_key'] = 1; //pass only if you want to generate new keys  $post['ssh_key'] = 1; //pass only if you want to use keys to authenticate  $post['sshpub_key'] = 'YOUR PUBLIC KEY'; //pass only if ssh_key is set  $post['sshpri_key'] = 'YOUR PRIVATE KEY'; //pass only if ssh_key is set  $post['port'] = 22;    $output = $admin->addbackupserver($post);    print_r(json_encode($output));  ?>
```

## **在虚拟器中创建备份服务**

如下图所示，添加备份服务器后，您需要创建一个备份计划。

**Virtualizor 管理面板> >计划(菜单)> >添加备份计划**

![Add_Backup_Plan-with-virtualizor](img/baccf82376fbdc8c84054e03f00615f7.png)

在下一步中，您需要将备份计划分配给 VPS。有趣的是，可以将备份计划分配给单个 VPS 或 VPS 计划。

要将备份程序分配给特定的 VPS，您可以通过参考以下路径来编辑 VPS:

**Virtualizor 管理面板> >虚拟服务器(菜单)> >列表全部- > >编辑**

如下所示，打开高级选项块，然后选择之前创建的备份计划:

![2Add_Backup_Plan](img/1e056cefb77f7585ee88f0f3c9bf67fd.png)

请注意，为了将备份计划分配给 VPS 计划，您必须参考以下路径来编辑 VPS 计划:

**Virtualizor 管理面板> >计划(菜单)> >列出计划> >编辑**

![3Add_Backup_Plan](img/df72ade4f41a5f31874be32b683c0f5f.png)

### **如何用虚拟器配置备份服务**

在这一节中，我们将解释 Virtualizor 中的备份设置。这个描述是基于本帖的第一张图片。

**–禁用:**禁用备份程序。请注意，禁用备份程序不会从 cron 运行它。此外，最终用户不能备份/恢复分配给此计划的所有 VP。

**–备份服务器:**上传 VPS 备份的备份服务器的名称。

**–目录:**这是 VPS 备份在备份服务器上存储的目录。

**–备份频率:**通过选择此部分中的一个选项，可以设置每小时、每天、每周、每月或手动备份过程。

**–备份轮换:**设置备份轮换。例如，如果 rotation 设置为 2，VPS 的最后两个备份将保留在服务器上。

**–最终用户备份:**创建尽可能多的备份。您可以设置备份副本数量的限制。

**–最终用户恢复:**根据用户需要创建任意数量的恢复副本。您可以设置恢复副本数量的限制。

**–进程优先级:**决定备份进程的优先级。较高的值表示低优先级，较低的负值表示高优先级。

**–进程 I/O 优先级:**优先级越高，磁盘时间越长。

**–禁用压缩:**这会减少 CPU 的使用，但会增加备份过程中的 IO 使用。需要注意的重要一点是，选择此选项会增加 VPS 备份的大小。

最后，您可以从以下路径搜索备份过程日志文件的故障排除/报告:

**/var/virtualizor/log/backup/VPS id . log**

**/var/virtualizor/log/restore/VPS id . log**

## **如何启动备份**

要开始备份过程，您必须仔细遵循前面几节中提到的所有步骤。然后从控制台运行以下命令，以便立即完成备份过程:

```
/usr/local/emps/bin/php /usr/local/virtualizor/scripts/vpsbackups_plan.php -b<backup-plan-id> [options]
```

**选项:**

-f 强制备份，即使备份计划被禁用

## 结论

注意，Virtualizor 本身并不是一个虚拟器。Virtualizor 只是一个控制面板。也就是说，它使用虚拟化技术来创建虚拟机或 VPS，并自动执行分配它的步骤。由于使用此控制面板的重要性，有必要学习如何通过 Virtualizor 进行备份。在本文中，我们试图教你如何用 Virtualizor 创建备份服务。如果你有任何问题，你可以在评论区和我们分享。