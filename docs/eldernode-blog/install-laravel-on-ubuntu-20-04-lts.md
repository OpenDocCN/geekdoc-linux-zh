# 如何在 Ubuntu 20.04 LTS 上安装 Laravel 完整] - Eldernode

> 原文：<https://blog.eldernode.com/install-laravel-on-ubuntu-20-04-lts/>

![How to install Laravel on Ubuntu 20.04 LTS [complete]](img/3a829b06bbc54b2c2aa9600f55ca6fa0.png)

要了解 web 应用程序框架，请加入我们来回顾安装 Laravel。有了这个工具，您会发现一些常见的任务变得更加简单，比如身份验证、路由、会话和缓存。选择 Laravel 的主要原因是为了在不牺牲应用程序功能的情况下开发一个令开发者满意的开发过程。在本文中，我们试着向你**学习如何在 Ubuntu 20.04 LTS** 【完整】上安装 Laravel。你可以在 [Eldernode](https://eldernode.com/) 查看可用的软件包，购买一台 [Ubuntu VPS](https://eldernode.com/ubuntu-vps/) 服务器。

## **教程在 Ubuntu 20.04 上安装 Laravel**

为了让本教程更好地发挥作用，请考虑以下**先决条件**:

一个拥有 sudo 权限
的非 root 用户进行设置，遵循我们在 Ubuntu 20.04 LTS 上设置的 [初始服务器。](https://eldernode.com/initial-server-set-up-on-ubuntu-20-04-lts/)

## **如何在 Ubuntu 20.04 LTS 上安装 Laravel【完整】**

Laravel 是为 web 应用开发设计的 PHP 语言框架之一，基于 MVC 工作。让我们通过本文的步骤来学习如何在 Ubuntu 20.04 上安装 Laravel。

### 第一步:安装灯组

你必须首先在 Ubuntu 上安装[灯组。](https://blog.eldernode.com/how-to-install-lamp-on-ubuntu-20-04/)

**注意** : Laravel 安装需要 PHP 7.2 或更高版本。

现在，使用下面的命令[安装 PHP](https://blog.eldernode.com/install-and-configure-php-on-ubuntu-20-04/) 。

```
sudo apt install software-properties-common  sudo add-apt-repository ppa:ondrej/php  sudo apt install -y php7.4 php7.4-gd php7.4-mbstring php7.4-xml 
```

并且，[通过运行下面的命令安装 Apache](https://blog.eldernode.com/apache-web-server-ubuntu-20/) 2。

```
sudo apt install apache2 libapache2-mod-php7.4 
```

接下来，[用下面的命令安装 MySQL](https://blog.eldernode.com/installing-mysql-on-ubuntu-20/) :

```
sudo apt install mysql-server php7.4-mysql 
```

### 第二步:安装 Composer

使用以下命令安装 composer

```
curl -sS https://getcomposer.org/installer | php  sudo mv composer.phar /usr/local/bin/composer  sudo chmod +x /usr/local/bin/composer 
```

### 第三步:下载并安装 Laravel

现在该安装 Laravel 了，下载最新版本，用下面的命令安装。

```
cd /var/www  git clone https://github.com/laravel/laravel.git 
```

然后按照下面的说明操作:

```
cd /var/www/laravel  sudo composer install 
```

此外，请遵循以下说明:

```
chown -R www-data.www-data /var/www/laravel  chmod -R 755 /var/www/laravel  chmod -R 777 /var/www/laravel/storage 
```

### 步骤 4:创建环境设置

现在用下面的命令创建环境设置:

```
mv .env.example .env 
```

然后按照下面的说明操作:

```
php artisan key:generate    Application key set successfully. 
```

此外，请遵循以下说明:

```
nano .env 
```

```
APP_NAME=Laravel  APP_ENV=local  APP_KEY=base64:WrdferHu7dfkc2OdfLdfBPuxdfHqq2BdfQYhd  APP_DEBUG=true  APP_URL=http://localhost  ... 
```

### 第五步:创建 MySQL 用户和数据库

```
CREATE DATABASE laravel;  CREATE USER 'eldernode'@'localhost' IDENTIFIED BY 'secret';  GRANT ALL ON eldernode.* to 'eldernode'@'localhost';  FLUSH PRIVILEGES;  quit 
```

现在编辑。env 文件并更新数据库设置。

```
DB_CONNECTION=mysql  DB_HOST=127.0.0.1  DB_PORT=3306  DB_DATABASE=eldernode  DB_USERNAME=eldernode  DB_PASSWORD=eldernode 
```

### 步骤 6 : Apache 配置

```
nano /etc/apache2/sites-enabled/avalable.conf 
```

注意:在运行之前，请确保服务器上安装了 nano

使用以下命令更新配置:

```
<VirtualHost *:80>    ServerAdmin [[email protected]](/cdn-cgi/l/email-protection)  DocumentRoot /var/www/laravel/public    <Directory />  Options FollowSymLinks  AllowOverride None  </Directory>  <Directory /var/www/laravel>  AllowOverride All  </Directory>    ErrorLog ${APACHE_LOG_DIR}/error.log  CustomLog ${APACHE_LOG_DIR}/access.log combined    </VirtualHost> 
```

现在用下面的命令重新加载 Apache2 配置:

```
sudo systemctl restart apache2
```

### 第七步:在浏览器上访问 Laravel

恭喜，安装完成。您现在可以在浏览器中输入 IP 并查看 Laravel 页面。

## 结论

在本教程中，您通过了 7 个步骤，并成功设置了一个运行在 Ubuntu 20.04 上的新 Laravel 应用程序。因此，请尽情享受这款易于访问且功能强大的工具，它为大型、健壮的应用程序提供了所需的强大工具。