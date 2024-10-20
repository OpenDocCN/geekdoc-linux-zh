# 如何在 Ubuntu 20.04 中安装带有 Apache 的 WordPress-elder node

> 原文：<https://blog.eldernode.com/install-wordpress-apache-ubuntu-20/>

![How to install WordPress with Apache in Ubuntu 20.04](img/8058877897f1dca9173bfb7ae5cba736.png)

之前，你学习了用 NginxT3 安装 WordPress 的 [。在这篇文章中，你将学习**如何在 Ubuntu 20.04 中安装带有 Apache 的 WordPress。**当然，你知道 WordPress，这是一个免费开源的流行网站平台，因为易于安装、学习和使用、高度可插拔和可定制而被广泛使用。](https://eldernode.com/wordpress-installation-nginx-ubuntu20/)

## 如何在 Ubuntu 20.04 中用 Apache 安装 WordPress

由于我们将介绍如何在 **Ubuntu 20.04** 中安装最新版本的 **WordPress** 和 **Apache** ，你需要安装 **LAMP** 栈并为托管网站进行良好配置。如果您尚未安装，请参阅我们的指南:

[教程在 Ubuntu 20.04 Linux 上安装最新的 phpMyAdmin](https://eldernode.com/install-the-latest-phpmyadmin-on-ubuntu-20/)

### 在 Ubuntu 20.04 中安装 WordPress

安装好**灯** ( **阿帕奇**、**玛利亚 DB、**和 **PHP** 栈后，开始下载最新版本的 WordPress

```
wget -c http://wordpress.org/latest.tar.gz 
```

然后，提取存档的文件。

```
tar -xzvf latest.tar.gz
```

一旦你提取了 WordPress 目录，将它移动到你的文档根目录下，即 /var/www/html/ 和你的网站下。要创建一个 mysite.com 的**目录并将 WordPress 文件移动到它下面，运行下面的命令。**

**注** :将【mysite.com】的**替换为你的网站名称或域名。**

```
ls -l  sudo cp -R wordpress /var/www/html/mysite.com  ls -l /var/www/html/
```

现在，您可以在网站( **/var/www/html/mysite.com** )目录上设置适当的权限。

**注** :应该是属于 **Apache2** 用户和名为 **www-data** 的群组。

```
sudo chown -R www-data:www-data /var/www/html/mysite.com  sudo chmod -R 775 /var/www/html/mysite.com
```

### `为网站创建 WordPress 数据库`

`在这个步骤中，通过使用带有 -u 标志的 **MySQL** 命令来提供用户名，该用户名应该是 **root** 和 -p 来输入您在安装 MariaDB 软件时为 MySQL root 帐户设置的密码，登录到 MariaDB [数据库](https://en.wikipedia.org/wiki/Database) shell。`

```
`sudo mysql -u root -p` 
```

`要创建您站点的数据库和具有权限的数据库用户，请在登录后运行以下命令。`

`**注**:替换“**我的网站**”、“**我的网站管理员**”、 **[【邮件保护】](/cdn-cgi/l/email-protection)！**"用你的数据库名，数据库用户名，和用户密码`

```
`MariaDB [(none)]> CREATE DATABASE **mysite**;`
```

```
`MariaDB [(none)]> GRANT ALL PRIVILEGES ON **mysite**.* TO '**mysiteadmin**'@'localhost' IDENTIFIED BY '**[[email protected]](/cdn-cgi/l/email-protection)!**';` 
```

```
`MariaDB [(none)]> FLUSH PRIVILEGES;`
```

```
`MariaDB [(none)]> EXIT`
```

`现在，从提供的示例配置文件创建一个**wp-config.php**文件，移动到您网站的文档根目录。`

```
`cd /var/www/html/mysite.com  sudo mv wp-config-sample.php wp-config.php`
```

`接下来，打开**wp-config.php**配置文件进行编辑。`

```
`sudo vim wp-config.php`
```

`然后，您需要更新上面创建的数据库名称、数据库用户和用户密码的参数。`

`![ Update the database connection parameters](img/901a660fc087e23f31b26aad28bef0d4.png)`

### 

`[购买 Linux 虚拟私有服务器](https://eldernode.com/linux-vps/)`

### `为 WordPress 网站创建 Apache 虚拟主机`

`是时候配置 Apache webserver 来服务你的站点了。您需要在 Apache 配置下为它创建一个虚拟主机。它应该与您的完全合格的域名。`

`在下面的代码中，要在**/etc/Apache 2/sites-available/**目录下创建一个新文件，需要创建并激活一个新的虚拟主机。`

`**注** :在本教程中，你将调用文件 **mysite.com.conf** ，它应该以结尾。确认扩展)`

```
`sudo vim /etc/apache2/sites-available/mysite.com.conf`
```

`然后，用您的值替换 **ServerName** 和 **ServerAdmin** 电子邮件，并将以下配置复制并粘贴到其中。`

```
`<VirtualHost *:80>  	ServerName **mysite.com**  	ServerAdmin **[[email protected]](/cdn-cgi/l/email-protection)**  	DocumentRoot **/var/www/html/mysite.com**  	ErrorLog ${APACHE_LOG_DIR}/error.log  	CustomLog ${APACHE_LOG_DIR}/access.log combined  </VirtualHost>` 
```

`![Apache WordPress virtual host](img/56c50a6ae024e0a0d97d7d7fc09a9d91.png)`

`您现在可以保存并关闭它。`

`然后，为了能够启用新站点并重新加载 apache2 服务以应用新的更改，您应该检查 apache 配置的语法是否正确。`

```
`apache2ctl -t  sudo a2ensite mysite.com.conf  sudo systemctl reload apache2`
```

`我们还应该提到，要让您的新站点从 web 浏览器正确加载，您需要禁用默认的虚拟主机。`

```
`sudo a2dissite 000-default.conf  sudo systemctl reload apache2` 
```

### `通过 web 界面完成 WordPress 安装`

`现在，你将学习如何使用 web 安装程序完成 WordPress 的安装。打开浏览器，使用站点的域名进行导航。`

```
`http://mysite.com.`
```

`在加载 WordPress web installer 之后，你可以做的下一件事就是选择你希望安装使用的语言，然后点击**继续**。`

`现在，您可以设置站点标题(管理用户名、密码和电子邮件)来管理您的站点内容。然后点击**安装 WordPress** 。`

`如此清晰，以至于在安装完 WordPress 后，你会点击 **Log** 进入你网站的管理登录页面。`

`**最后** ，你将能够使用你的管理凭证登录到你的新 **WordPress** 网站，并开始从**仪表板**定制你的网站。`

`亲爱的用户，我们希望你能喜欢这个教程，你可以在评论区提出关于这个培训的问题，或者要解决 [Eldernode](https://eldernode.com/) 培训领域的其他问题，请参考 [A sk page](https://eldernode.com/ask) 部分并在其中提出你的问题。`

`**同样，阅读**`

`[如何在 Ubuntu 20.04 上安装 lamp](https://eldernode.com/how-to-install-lamp-on-ubuntu-20-04/)`

`[How to install lamp on Ubuntu 20.04](https://eldernode.com/how-to-install-lamp-on-ubuntu-20-04/)`