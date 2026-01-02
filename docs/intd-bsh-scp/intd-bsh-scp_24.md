# 使用 BASH 在 LAMP 安装上自动安装 WordPress

这里是一个完整的 LAMP 和 WordPress 安装示例，它适用于任何基于 Debian 的机器。

# 前提条件

+   基于 Debian 的机器（Ubuntu、Debian、Linux Mint 等）

# 规划功能

让我们再次回顾脚本的主要功能：

**LAMP 安装**

+   更新包管理器

+   安装防火墙（ufw）

+   允许 SSH、HTTP 和 HTTPS 流量

+   安装 Apache2

+   安装并配置 MariaDB

+   安装 PHP 和所需的插件

+   启用所有必需的 Apache2 模块

**Apache 虚拟主机设置**

+   在 `/var/www` 下创建一个目录

+   配置目录权限

+   在 `/etc/apache2/sites-available` 下创建 `$domain` 文件并追加所需的虚拟主机内容

+   启用站点

+   重启 Apache2

**SSL 配置**

+   生成 OpenSSL 证书

+   将 SSL 证书追加到 `ssl-params.conf` 文件中

+   将 SSL 配置追加到虚拟主机文件中

+   启用 SSL

+   重新加载 Apache2

**数据库配置**

+   创建数据库

+   创建用户

+   刷新权限

**WordPress 配置**

+   安装所需的 WordPress PHP 插件

+   安装 WordPress

+   将所需信息追加到 `wp-config.php` 文件中

不再拖延，让我们开始编写脚本。

# 脚本

我们首先设置变量并要求用户输入他们的域名：

```sh
      echo 'Please enter your domain of preference without www:'
read DOMAIN
echo "Please enter your Database username:"
read DBUSERNAME
echo "Please enter your Database password:"
read DBPASSWORD
echo "Please enter your Database name:"
read DBNAME

ip=`hostname -I | cut -f1 -d' '` 

```

我们现在准备好开始编写我们的函数。首先创建 `lamp_install()` 函数。在其内部，我们将更新系统，安装 ufw，允许 SSH、HTTP 和 HTTPS 流量，安装 Apache2，安装 MariaDB 和 PHP。我们还将启用所有必需的 Apache2 模块。

```sh
      lamp_install () {
	apt update -y
	apt install ufw
	ufw enable
	ufw allow OpenSSH
	ufw allow in "WWW Full"

	apt install apache2 -y
	apt install mariadb-server
	mysql_secure_installation -y
	apt install php libapache2-mod-php php-mysql -y
	sed -i "2d" /etc/apache2/mods-enabled/dir.conf
	sed -i "2i\\\tDirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm" /etc/apache2/mods-enabled/dir.conf
	systemctl reload apache2

} 

```

接下来，我们将创建 `apache_virtualhost_setup()` 函数。在其内部，我们将在 `/var/www` 下创建一个目录，配置目录权限，在 `/etc/apache2/sites-available` 下创建 `$domain` 文件并追加所需的虚拟主机内容，启用站点并重启 Apache2。

```sh
      apache_virtual_host_setup () {
	mkdir /var/www/$DOMAIN
	chown -R $USER:$USER /var/www/$DOMAIN

	echo "<VirtualHost *:80>" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tServerName $DOMAIN" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tServerAlias www.$DOMAIN" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tServerAdmin webmaster@localhost" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tDocumentRoot /var/www/$DOMAIN" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e '\tErrorLog ${APACHE_LOG_DIR}/error.log' >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e '\tCustomLog ${APACHE_LOG_DIR}/access.log combined' >> /etc/apache2/sites-available/$DOMAIN.conf
	echo "</VirtualHost>" >> /etc/apache2/sites-available/$DOMAIN.conf
	a2ensite $DOMAIN
	a2dissite 000-default
	systemctl reload apache2

} 

```

接下来，我们将创建 `ssl_config()` 函数。在其内部，我们将生成 OpenSSL 证书，将 SSL 证书追加到 `ssl-params.conf` 文件中，将 SSL 配置追加到虚拟主机文件中，启用 SSL 并重新加载 Apache2。

```sh
      ssl_config () {
	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt

	echo "SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLProtocol All -SSLv2 -SSLv3 -TLSv1 -TLSv1.1" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLHonorCipherOrder On" >> /etc/apache2/conf-available/ssl-params.conf
	echo "Header always set X-Frame-Options DENY" >> /etc/apache2/conf-available/ssl-params.conf
	echo "Header always set X-Content-Type-Options nosniff" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLCompression off" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLUseStapling on" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLStaplingCache \"shmcb:logs/stapling-cache(150000)\"" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLSessionTickets Off" >> /etc/apache2/conf-available/ssl-params.conf

	cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak
	sed -i "s/var\/www\/html/var\/www\/$DOMAIN/1" /etc/apache2/sites-available/default-ssl.conf
	sed -i "s/etc\/ssl\/certs\/ssl-cert-snakeoil.pem/etc\/ssl\/certs\/apache-selfsigned.crt/1" /etc/apache2/sites-available/default-ssl.conf
	sed -i "s/etc\/ssl\/private\/ssl-cert-snakeoil.key/etc\/ssl\/private\/apache-selfsigned.key/1" /etc/apache2/sites-available/default-ssl.conf
	sed -i "4i\\\t\tServerName $ip" /etc/apache2/sites-available/default-ssl.conf
	sed -i "22i\\\tRedirect permanent \"/\" \"https://$ip/\"" /etc/apache2/sites-available/000-default.conf
	a2enmod ssl
	a2enmod headers
	a2ensite default-ssl
	a2enconf ssl-params
	systemctl reload apache2
} 

```

接下来，我们将创建 `db_setup()` 函数。在其内部，我们将创建数据库，创建用户并授予用户所有权限。

```sh
      db_config () {
	mysql -e "CREATE DATABASE $DBNAME;"
	mysql -e "GRANT ALL ON $DBNAME.* TO '$DBUSERNAME'@'localhost' IDENTIFIED BY '$DBPASSWORD' WITH GRANT OPTION;"
	mysql -e "FLUSH PRIVILEGES;"
} 

```

接下来，我们将创建 `wordpress_config()` 函数。在其内部，我们将下载 WordPress 的最新版本，将其解压到 `/var/www/$DOMAIN` 目录中，创建 `wp-config.php` 文件并将其所需内容追加到其中。

```sh
      wordpress_config () {
	db_config

	apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y
	systemctl restart apache2
	sed -i "8i\\\t<Directory /var/www/$DOMAIN/>" /etc/apache2/sites-available/$DOMAIN.conf
	sed -i "9i\\\t\tAllowOverride All" /etc/apache2/sites-available/$DOMAIN.conf
	sed -i "10i\\\t</Directory>" /etc/apache2/sites-available/$DOMAIN.conf

	a2enmod rewrite
	systemctl restart apache2

	apt install curl
	cd /tmp
	curl -O https://wordpress.org/latest.tar.gz
	tar xzvf latest.tar.gz
	touch /tmp/wordpress/.htaccess
	cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
	mkdir /tmp/wordpress/wp-content/upgrade
	cp -a /tmp/wordpress/. /var/www/$DOMAIN
	chown -R www-data:www-data /var/www/$DOMAIN
	find /var/www/$DOMAIN/ -type d -exec chmod 750 {} \;
	find /var/www/$DOMAIN/ -type f -exec chmod 640 {} \;
	curl -s https://api.wordpress.org/secret-key/1.1/salt/ >> /var/www/$DOMAIN/wp-config.php
	echo "define('FS_METHOD', 'direct');" >> /var/www/$DOMAIN/wp-config.php
	sed -i "51,58d" /var/www/$DOMAIN/wp-config.php
	sed -i "s/database_name_here/$DBNAME/1" /var/www/$DOMAIN/wp-config.php
	sed -i "s/username_here/$DBUSERNAME/1" /var/www/$DOMAIN/wp-config.php
	sed -i "s/password_here/$DBPASSWORD/1" /var/www/$DOMAIN/wp-config.php
} 

```

最后，我们将创建 `execute()` 函数。在其内部，我们将调用上面创建的所有函数。

```sh
      execute () {
	lamp_install
	apache_virtual_host_setup
	ssl_config
	wordpress_config
} 

```

通过这种方式，脚本已经准备好，你可以运行它。如果你需要完整的脚本，可以在下一节找到。

# 完整的脚本

```sh
      #!/bin/bash echo 'Please enter your domain of preference without www:'
read DOMAIN
echo "Please enter your Database username:"
read DBUSERNAME
echo "Please enter your Database password:"
read DBPASSWORD
echo "Please enter your Database name:"
read DBNAME

ip=`hostname -I | cut -f1 -d' '`

lamp_install () {
	apt update -y
	apt install ufw
	ufw enable
	ufw allow OpenSSH
	ufw allow in "WWW Full"

	apt install apache2 -y
	apt install mariadb-server
	mysql_secure_installation -y
	apt install php libapache2-mod-php php-mysql -y
	sed -i "2d" /etc/apache2/mods-enabled/dir.conf
	sed -i "2i\\\tDirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm" /etc/apache2/mods-enabled/dir.conf
	systemctl reload apache2

}

apache_virtual_host_setup () {
	mkdir /var/www/$DOMAIN
	chown -R $USER:$USER /var/www/$DOMAIN

	echo "<VirtualHost *:80>" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tServerName $DOMAIN" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tServerAlias www.$DOMAIN" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tServerAdmin webmaster@localhost" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e "\tDocumentRoot /var/www/$DOMAIN" >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e '\tErrorLog ${APACHE_LOG_DIR}/error.log' >> /etc/apache2/sites-available/$DOMAIN.conf
	echo -e '\tCustomLog ${APACHE_LOG_DIR}/access.log combined' >> /etc/apache2/sites-available/$DOMAIN.conf
	echo "</VirtualHost>" >> /etc/apache2/sites-available/$DOMAIN.conf
	a2ensite $DOMAIN
	a2dissite 000-default
	systemctl reload apache2

}

ssl_config () {
	openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt

	echo "SSLCipherSuite EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLProtocol All -SSLv2 -SSLv3 -TLSv1 -TLSv1.1" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLHonorCipherOrder On" >> /etc/apache2/conf-available/ssl-params.conf
	echo "Header always set X-Frame-Options DENY" >> /etc/apache2/conf-available/ssl-params.conf
	echo "Header always set X-Content-Type-Options nosniff" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLCompression off" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLUseStapling on" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLStaplingCache \"shmcb:logs/stapling-cache(150000)\"" >> /etc/apache2/conf-available/ssl-params.conf
	echo "SSLSessionTickets Off" >> /etc/apache2/conf-available/ssl-params.conf

	cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/default-ssl.conf.bak
	sed -i "s/var\/www\/html/var\/www\/$DOMAIN/1" /etc/apache2/sites-available/default-ssl.conf
	sed -i "s/etc\/ssl\/certs\/ssl-cert-snakeoil.pem/etc\/ssl\/certs\/apache-selfsigned.crt/1" /etc/apache2/sites-available/default-ssl.conf
	sed -i "s/etc\/ssl\/private\/ssl-cert-snakeoil.key/etc\/ssl\/private\/apache-selfsigned.key/1" /etc/apache2/sites-available/default-ssl.conf
	sed -i "4i\\\t\tServerName $ip" /etc/apache2/sites-available/default-ssl.conf
	sed -i "22i\\\tRedirect permanent \"/\" \"https://$ip/\"" /etc/apache2/sites-available/000-default.conf
	a2enmod ssl
	a2enmod headers
	a2ensite default-ssl
	a2enconf ssl-params
	systemctl reload apache2
}

db_config () {
	mysql -e "CREATE DATABASE $DBNAME;"
	mysql -e "GRANT ALL ON $DBNAME.* TO '$DBUSERNAME'@'localhost' IDENTIFIED BY '$DBPASSWORD' WITH GRANT OPTION;"
	mysql -e "FLUSH PRIVILEGES;"
}

wordpress_config () {
	db_config

	apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y
	systemctl restart apache2
	sed -i "8i\\\t<Directory /var/www/$DOMAIN/>" /etc/apache2/sites-available/$DOMAIN.conf
	sed -i "9i\\\t\tAllowOverride All" /etc/apache2/sites-available/$DOMAIN.conf
	sed -i "10i\\\t</Directory>" /etc/apache2/sites-available/$DOMAIN.conf

	a2enmod rewrite
	systemctl restart apache2

	apt install curl
	cd /tmp
	curl -O https://wordpress.org/latest.tar.gz
	tar xzvf latest.tar.gz
	touch /tmp/wordpress/.htaccess
	cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
	mkdir /tmp/wordpress/wp-content/upgrade
	cp -a /tmp/wordpress/. /var/www/$DOMAIN
	chown -R www-data:www-data /var/www/$DOMAIN
	find /var/www/$DOMAIN/ -type d -exec chmod 750 {} \;
	find /var/www/$DOMAIN/ -type f -exec chmod 640 {} \;
	curl -s https://api.wordpress.org/secret-key/1.1/salt/ >> /var/www/$DOMAIN/wp-config.php
	echo "define('FS_METHOD', 'direct');" >> /var/www/$DOMAIN/wp-config.php
	sed -i "51,58d" /var/www/$DOMAIN/wp-config.php
	sed -i "s/database_name_here/$DBNAME/1" /var/www/$DOMAIN/wp-config.php
	sed -i "s/username_here/$DBUSERNAME/1" /var/www/$DOMAIN/wp-config.php
	sed -i "s/password_here/$DBPASSWORD/1" /var/www/$DOMAIN/wp-config.php
}

execute () {
	lamp_install
	apache_virtual_host_setup
	ssl_config
	wordpress_config
} 

```

## 摘要

脚本执行以下操作：

+   安装 LAMP

+   创建一个虚拟主机

+   配置 SSL

+   安装 WordPress

+   配置 WordPress

话虽如此，我希望您喜欢这个例子。
