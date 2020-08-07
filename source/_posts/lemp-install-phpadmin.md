---
title: 如何在LEMP服务下安装phpmyadmin
tags:
  - MySql
url: 695.html
id: 695
categories:
  - 我是修电脑的
date: 2013-08-05 08:45:40
---

**关于phpmydmin** phpmyadmin是一款以[PHP为基础](http://zh.wikipedia.org/wiki/PHP "PHP")，以Web-Base方式架构在网站服务器上的[MySQL的数据库](http://zh.wikipedia.org/wiki/MySQL "MySQL")管理工具，让管理者可用Web介面管理MySQL数据库。在安装phpmyadmin之前，一定要确保您的服务器上面安装了LEMP 或者是LAMP等服务，这样才能确保phpmyadmin能正常安装以及运行，等安装完WEB服务之后，你就可以开始安装phpmyadmin了。 

**第一个步骤：安装phpmyadmin** 开始使用apt-get来下载并安装它
```
sudo apt-get install phpmyadmin
```
在安装过程中，phpmyadmin会问你需不需要配置数据库，选择是。然后还会出现一个选择，就是提示你选择一个服务器：Apache或lighttpd的，随意选择其中一个即可。 

**第二个步骤：配置phpmyadmin** 此时此刻，虽然已经把phpmyadmin安装到服务器当中了，但是，还需要多一部来配置它；phpMyAdmin和您的网站的目录之间需要创建一个链接。如果你正在看前面的步骤，它可能仍位于nginx的默认目录，否则就无法使用：
```
sudo ln -s /usr/share/phpmyadmin/ /usr/share/nginx/www
```
重启nginx
```
sudo service nginx restart
```
然后就可以访问你的网址：http://xxxxxxxx/phpmyadmin 

**注意：会有一些意想不到的情况** 有可能当你访问了phpmyadmin之后发现，会出现首页空白的情况，不要慌，打开PHP.ini打开错误提示，看看是不是配置错误，若不是配置错误的话可以尝试一下先卸载phpmyadmin，是卸载，不是删除，然后在重新安装一次，期间需要重启nginx服务。 还有需要注意的地方是：第二个步骤，最好是用ln这个命令来做一个链接，不要直接复制phpmyadmin文件夹到nginx的网站目录下。 

参考链接：[How to Install phpMyAdmin on a LEMP server](https://www.digitalocean.com/community/articles/how-to-install-phpmyadmin-on-a-lemp-server/)
