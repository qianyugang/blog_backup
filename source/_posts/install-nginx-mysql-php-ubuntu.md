---
title: '如何安装Linux,nginx,Mysql,PHP(LEMP) 于 Ubuntu'
tags:
  - MySql
  - Nginx
  - PHP
url: 683.html
id: 683
categories:
  - 我是修电脑的
date: 2013-08-05 05:50:50
---
**关于LEMP** LEMP是一组运行在WEB服务器上面的开源软件，是Linux,nginx(由于英文发音中其实是Engine x，所以首字母为E),Mysql和PHP的首字母缩写，由于服务器已经运行Ubuntu，也就是说Linux部分已经安装完成，这里就教大家如何安装剩下的服务。 

**关于设置** 在本教程的步骤中需要用户有ROOT权限。你可以在初始服务器安装教程中的步骤3和4中看到如何设置。 

**第一个步骤：Step One—Update Apt-Get** 在这个教程当中，我们将会使用apt-get来进行服务器程序安装，在2012年的5月8日PHP被发现严重的漏洞，重要的是我们需要下载最新的补丁来保护我们的服务器。 让我们来做一次更新吧：
```
sudo apt-get update
```
**第二个步骤：安装Mysql** Mysql是一个强大的用于组织和管理检索数据的数据库管理系统。 安装先打开终端，输入这些命令：
```
sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
```
在安装的过程中，Mysql会要求你设置root密码。如果你在安装的时候错过来设置密码的机会，没关系，在安装完成后也是非常容易设置的。 一旦你安装来Mysql，我们要用这个命令激活它：
```
sudo mysql\_install\_db
```
最后通过运行MySQL建立脚本:
```
sudo /usr/bin/mysql\_secure\_installation
```
然后会提示你输入当前的跟密码，然后输入它
```
Enter current password for root (enter for none): 
OK, successfully used password, moving on...
```
然后提示会问你，如果你想更改根密码，选择“N”到下一个步骤 这是最简单的应答所有的选项，最后，将会重新加载和执行Mysql的新变化
```
By default, a MySQL installation has an anonymous user, allowing anyone

to log into MySQL without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? \[Y/n\] y                                            
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? \[Y/n\] y
... Success!

By default, MySQL comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? \[Y/n\] y
 \- Dropping test database...
 ... Success!
 \- Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? \[Y/n\] y
 ... Success!

Cleaning up...
```
完成了之后你就可以完成安装“PHP” 

**第三个步骤：安装NGINX** 一旦安装好来Mysql，我们就可以在VPS上面安装NGINX
```
sudo apt-get install nginx
```
nginx不是自动启动的，想要让nginx运行，输入命令
```
sudo service nginx start
```
您可以确认您的WEB服务器上面已经安装了nginx，您可以通过浏览器输入您的IP地址，你可以运行以下命令来显示您的VPS的IP地址。（若在本地安装此步骤可以酌情省下）
```
ifconfig eth0 | grep inet | awk '{ print $2 }'
```
**第四个步骤：安装PHP** 安装PHP，先要打开终端，输入以下命令，下一步，我们将设置nginx和php的详细配置
```
sudo apt-get install php5-fpm
```
**第五个步骤：配置PHP** 我们需要对PHP配置文件做一点小小的改动，打开php.ini
```
sudo nano /etc/php5/fpm/php.ini
```
找到这一行：`cgi.fix_pathinfo=1` a并且把1 改为 0
```
cgi.fix_pathinfo=0
```
如果这个数字保持为“1”，那么，PHP解释器就会尽其所能的处理文件尽可能靠近可能请求的文件。这是一个可能的安全风险。如果这个数字为“0”，相反，解释器只会处理精确的文件路径并且更加安全。保存并且退出。 我们需要对PHP5- fpm配置文件做一点小小的改动，打开www.conf
```
sudo nano /etc/php5/fpm/pool.d/www.conf
```
找到这一行“ listen = 127.0.0.1:9000,”a然后把“127.0.0.1:9000”改为“ /var/run/php5-fpm.sock“。
```
listen = /var/run/php5-fpm.sock
```
保存并且退出。 重启php-fpm:
```
sudo service php5-fpm restart
```
**第六个步骤：配置nginx** 打开默认的虚拟主机文件
```
sudo nano /etc/nginx/sites-available/default
```
配置的变化应包括如下（细节上的变化是根据配置信息）： 更新：Ubuntu的新版本默认创建一个称为'HTML'，而不是'www'的默认目录。如果/ usr /share/ nginx的/ WWW不存在，它可能称为HTML。请确保您更新您的配置是否正确（一致即可）。

```
server {
        listen   80;

        root /usr/share/nginx/www;
        index index.php index.html index.htm;

        server_name example.com;

        location / {
                try_files $uri $uri/ /index.html;
        }

        error_page 404 /404.html;

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
              root /usr/share/nginx/www;
        }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        location ~ \\.php$ {
                #fastcgi_pass 127.0.0.1:9000;
                # With php5-fpm:
                fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                fastcgi\_param SCRIPT\_FILENAME $document\_root$fastcgi\_script_name;
                include fastcgi_params;

        }

}

```
这里有一些细节的变化：

添加index.php文件到索引行。

从本地主机服务器名更改你的域名或IP地址（在配置替换example.com）

更改争取的行“location ~ \\.php$ {“ 的一部分

保存然后推出 

**第七个步骤：创建一个PHP详细信息页** 我们可以很快看到新的PHP配置的细节。  要对此进行设置，首先创建一个新的文件：
```
sudo nano /usr/share/nginx/www/info.php
```
键入以下代码在文件中：
```
<php? phpinfo(); ?>
```
保存然后退出，重启NGINX
```
sudo service nginx restart
```
现在你可以看到nginx和PHP-FPM配置的详细信息，请访问http://youripaddress/info.php 现在您的LEMP服务已经配置到你呢的服务器上面了。

**关于更多**
--------

安装完 LEMP之后, 您可以安装 [WordPress](https://www.digitalocean.com/community/articles/how-to-install-wordpress-with-nginx-on-ubuntu-12-04),继续做更多的MySQL (一个基本的[MySQL 教程](https://www.digitalocean.com/community/articles/a-basic-mysql-tutorial)) 或者安装 [phpMyAdmin](https://www.digitalocean.com/community/articles/how-to-install-phpmyadmin-on-a-lemp-server/), 创建一个 [SSL Certificate](https://www.digitalocean.com/community/articles/how-to-create-a-ssl-certificate-on-nginx-for-ubuntu-12-04),证书，或者安装一个[FTP Server](https://www.digitalocean.com/community/articles/how-to-set-up-vsftpd-on-ubuntu-12-04)ftp服务器. 参考文章：[How to Install Linux, nginx, MySQL, PHP (LEMP) stack on Ubuntu 12.04](https://www.digitalocean.com/community/articles/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04)
