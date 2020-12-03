---
title: Kong初探 (安装以及启动)
toc: true
tags:
  - api网关
  - Kong
  - Kong教程
id: 1316
categories:
  - 我是修电脑的
date: 2018-11-13 19:48:05
---

Kong是由Mashape出品的一款开源的api gateway服务器,官方解释为API Gateway, or API Middleware,由lua语言编写. kong的主要功能:

*   集中管理api,各个业务模块可以在kong上注册,集中访问
*   权限控制,提供丰富的插件,包括Base Authentication, Oauth2, HMAC Authentication, LDAP, JWT等等
*   安全管理
*   访问监控
*   数据分析
*   过滤数据和转换
*   日志集中收集和报告
*   丰富的可插拔的插件
*   自定义开发插件

安装PostgreSQL数据库
---------------

Kong依赖于数据库PostgreSQL 或者Cassandra ，这里我们只说一下 PostgreSQL，注意安装的版本，官方建议PostgreSQL 9.5+，Cassandra 3.x.x。 PostgreSQL：世界上最先进的开源关系数据库 官网网站：[https://www.postgresql.org/](https://www.postgresql.org/)

### 一、安装PostgreSQL

首先，安装PostgreSQL客户端。
```
    sudo apt-get postgresql-client
```
然后，安装PostgreSQL服务器。
```
sudo apt-get install postgresql
```
正常情况下，安装完成后，PostgreSQL服务器会自动在本机的5432端口开启。 如果还想安装图形管理界面，可以运行下面命令
```
    sudo apt-get install pgadmin3
```
### 二、添加新用户和新数据库

初次安装后，默认生成一个名为postgres 的数据库和一个名为postgres 的数据库用户。这里需要注意的是，同时还生成了一个名为postgres 的Linux系统用户。 首先，使用PostgreSQL控制台，新建一个Linux新用户，可以取你想要的名字，这里为dbuser。
```
    sudo adduser dbuser
```
然后，切换到postgres用户。
```
    sudo su - postgres
```
下一步，使用psql命令登录PostgreSQL控制台。
```
    psql
```
建立数据库给kong用,先创建用户
```
    CREATE USER kong_user WITH PASSWORD 'kong_pass';
```
创建数据库,并给用户授权
```
create database "kong_db";

GRANT ALL PRIVILEGES ON DATABASE "kong\_db" to kong\_user;
```
安装kong
------

到[官网文档](https://docs.konghq.com/install/ubuntu/?_ga=2.145684023.2090752991.1542082707-315260935.1541058156)下载kong对应的安装包 。 然后执行如下代码安装
```
sudo apt-get update
sudo apt-get install openssl libpcre3 procps perl
sudo dpkg -i kong-community-edition-0.14.1.*.deb
```
然后找到kong的配置文件，复制一份出来
```
cp /etc/kong/kong.conf.default /etc/kong/kong.conf
```
修改这个配置文件
```
vim /etc/kong/kong.conf
```
找到如下配置修改为上述的配置
```
pg_host = 127.0.0.1             # The PostgreSQL host to connect to.
pg_port = 5432                  # The port to connect to.
pg\_user = kong\_user             # The username to authenticate if required.
pg\_password = kong\_pass         # The password to authenticate if required.
pg\_database = kong\_db           # The database name to connect to.
```
启动kong，执行信息迁移数据
```
kong migrations up -c /etc/kong/kong.conf
```
出现以下信息即可算成功
```
migrating core for database kong_db
core migrated up to: 2015-01-12-175310_skeleton
core migrated up to: 2015-01-12-175310\_init\_schema
core migrated up to: 2015-11-23-817313_nodes
core migrated up to: 2016-02-29-142793_ttls
core migrated up to: 2016-09-05-212515_retries
core migrated up to: 2016-09-16-141423_upstreams
core migrated up to: 2016-12-14-172100\_move\_ssl\_certs\_to_core
core migrated up to: 2016-11-11-151900\_new\_apis\_router\_1
core migrated up to: 2016-11-11-151900\_new\_apis\_router\_2

...
...
...

rate-limiting migrated up to: 2015-08-03-132400\_init\_ratelimiting
rate-limiting migrated up to: 2016-07-25-471385\_ratelimiting\_policies
rate-limiting migrated up to: 2017-11-30-120000\_add\_route\_and\_service_id
migrating oauth2 for database kong_db
oauth2 migrated up to: 2015-08-03-132400\_init\_oauth2
oauth2 migrated up to: 2016-07-15-oauth2\_code\_credential_id
oauth2 migrated up to: 2016-12-22-283949\_serialize\_redirect_uri
oauth2 migrated up to: 2016-09-19-oauth2\_api\_id
oauth2 migrated up to: 2016-12-15-set\_global\_credentials
oauth2 migrated up to: 2017-04-24-oauth2\_client\_secret\_not\_unique
oauth2 migrated up to: 2017-10-19-set\_auth\_header\_name\_default
oauth2 migrated up to: 2017-10-11-oauth2\_new\_refresh\_token\_ttl\_config\_value
oauth2 migrated up to: 2018-01-09-oauth2\_pg\_add\_service\_id
67 migrations ran
```
然后执行启动kong
```
sudo kong start -c /etc/kong/kong.conf
```
验证kong是否安装成功,执行如下命令
```
curl -i http://localhost:8001/
```
或者在浏览器直接访问[http://localhost:8001/](http://localhost:8001/)也可，出现相关信息即安装成功

Kong UI管理工具
-----------

kong的管理实际上是使用api进行管理的，但是也有很多ui控制面板可以使用，这里我们使用的是[PGBI/kong-dashboard](https://github.com/PGBI/kong-dashboard)，此工具以来nodejs 和npm ，所以先执行一下命令安装
```
sudo apt update
sudo apt install npm
sudo apt install nodejs
```
然后安装相关包
```
 npm install -g kong-dashboard
```
安装完毕后可看到树状结构 然后启动，启动有很多种方式，这里只介绍最简单的一种，剩下方式可以去面板github查看
```
kong-dashboard start --kong-url http://localhost:8001
```
执行完毕看到如下返回代表启动成功
```
Connecting to Kong on http://localhost:8001 ...
Connected to Kong on http://localhost:8001.
Kong version is 0.14.1
Starting Kong Dashboard on port 8080
Kong Dashboard has started on port 8080
```
访问[http://localhost:8080](http://localhost:8080)查看面板，大功告成 日志文件存放在如下路径中
```
/usr/local/kong/logs
```
nginx配置修改路径如下
```
/usr/local/kong/nginx-kong.conf
```
参考链接
----

*   [PostgreSQL新手入门](http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html)
*   [ubuntu14.04 上kong服务器的搭建](http://www.wantchalk.com/c/server/2016/06/22/ubuntu1404-kong-server.html)
*   [Kong Api 初体验、Kong安装教程](https://blog.csdn.net/jiangyu1013/article/details/79894185)
*   [官方文档 Ubuntu Installation](https://docs.konghq.com/install/ubuntu/?_ga=2.145684023.2090752991.1542082707-315260935.1541058156)
*   [API gateway 之 kong 安装 （二）](https://www.cnblogs.com/chenjinxi/p/8723558.html)
*   [系列教程](https://www.lijiaocn.com/tags/class.html)
