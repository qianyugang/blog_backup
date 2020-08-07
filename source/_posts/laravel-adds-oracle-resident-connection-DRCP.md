---
title: Laravel 添加 Oracle 数据库常驻连接（ DRCP ）
tags:
  - PHP
url: 994.html
id: 994
categories:
  - 我是修电脑的
date: 2015-11-05 15:54:52
---

由于以前一直接触的是互联网业务，所以使用 Mysql 数据库的比较多，也比较熟悉。但是现有的金融公司一般使用的都是比较稳定的 Oracle 解决方案，所以在用户量较大的情况下会遇到一个比较麻烦的问题：Oracle 下的高并发。 问题描述：由于同时访问某个应用的用户量非常大；导致后端不断查询数据库，虽然每个 sql 语句的查询时间很短，但是由于要不断的连接->查询->断开，所以Oracle服务器会发现很多的查询进程，一个接一个，由于每个sql执行的时候，服务器会分配一些资源和内存给这个查询，所以在并发非常大的时候，悲剧就产生了，CPU和内存的使用率马上被推上峰值，负载马上压到最大，然后所有的 sql 查询全部变慢，变慢导致的后果是，应用服务器下的 Apache 进程时间越来越长，由于用户还在不断的查询，所以后面进入的apache进程会一直处于等待状态，而正在执行的进程缺越来越慢，果不其然的是很快，应用服务器马上也被拖垮。 解决方案：实际上这样的情况有很多方法优化，此处就着重描述一下在有限的服务器资源的状况下引入的一个数据库驻留连接池的特性，当然，这个特性 JAVA 已经自带了，而 PHP 需要修改一些配置，也可以使用。

> 数据库驻留连接池是 Oracle Database 11g 的一个新特性。对 PHP，它允许 Web 应用程序随着站点吞吐量的增长对连接数进行扩充。它还支持多台计算机上的多个 Apache 进程共享一个小规模的数据库服务器进程池。没有 DRCP，标准 PHP 连接必须启动和终止一个服务器进程。一个非 DRCP 持久性连接即使空闲时也将保留数据库服务器资源。

那么如何如何在不编写和更改任何应用程序逻辑的情况下使用这个DRCP呢？ 一定要做的步骤： 要在 Oracle 中启用 DRCP，登录到数据库服务器并启动连接池：
```
SQL> execute dbms\_connection\_pool.start_pool();
PL/SQL procedure successfully completed.
```
通过查询特定的 DBA\_CPOOL\_INFO 视图确认该池已启动：
```
QL> SELECT CONNECTION_POOL, STATUS, MAXSIZE
  2  FROM DBA\_CPOOL\_INFO;

CONNECTION_POOL             STATUS		    MAXSIZE
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-
SYS\_DEFAULT\_CONNECTION_POOL ACTIVE		    40
```
OK,启动成功！接下来就是PHP的设置： 首先，打开你的php.ini文件找到
```
oci8.connection_class = MYPHPAPP
```
取消注释，起个名字； 由于这次使用的是laravel框架，所以接下来就不详细描述原生 PHP 的使用方法了（基本使用方法请[猛戳这篇文章](http://blog.fabrichina.net/archives/250)），直接进入框架使用。想要连接 Oracle 数据库，你需要安装 jfelder/laravel-oracledb 插件，代码和安装方法请[猛戳这里](https://github.com/jfelder/laravel-oracledb) 。安装完成了之后，你只需要找到下面这个文件
```
D:\\work\\www\\baiqian\_fqg\\baiqian\_bqfqg_wx\\vendor\\jfelder\\oracledb\\src\\Jfelder\\OracleDB\\Connectors\\OracleConnector.php
```
然后在这个文件中的这段代码：
```
(CONNECT_DATA =  (SID = {$config\['database'\]})
```
中加上` _(SERVER = POOLED)_` ，变成如下代码：
```
(CONNECT_DATA = (SERVER = POOLED) (SID = {$config\['database'\]})
```
此举是为了声明在连接到数据库服务器的时候使用常驻池连接 接着找到这个文件
```
D:\\work\\www\\baiqian\_fqg\\baiqian\_bqfqg\_wx\\vendor\\jfelder\\oracledb\\src\\Jfelder\\OracleDB\\OCI\_PDO\\OCI.php
```
在文件中找到这段代码
```
    protected $attributes = array(\\PDO::ATTR_AUTOCOMMIT => 1,
        \\PDO::ATTR_ERRMODE => 0,
        \\PDO::ATTR_CASE => 0,
        \\PDO::ATTR\_ORACLE\_NULLS => 0
    );
```
此代码中加入 `\\PDO::ATTR_PERSISTENT => 1`, 变成如下代码
```
    protected $attributes = array(\\PDO::ATTR_AUTOCOMMIT => 1,
        \\PDO::ATTR_ERRMODE => 0,
        \\PDO::ATTR_CASE => 0,
        \\PDO::ATTR\_ORACLE\_NULLS => 0,
        \\PDO::ATTR_PERSISTENT => 1,
    );
```
此举是为了开启长连接，也可以让它默认使用oci_pconnect()函数来做为基本连接函数。 到此为止，全部更改和设置均已完成，接下来我们可以看看效果： 使用Apache自带的Apache Benchmark小工具来做个压力测试，命令如下：
```
ab -c 150 -t 30 http://test.php   
```
Apache Benchmark的介绍和用法以及参数请[猛戳这里](http://mo2g.com/view/38/)，然后使用如下语句来查询Oracle的进程数，会发现，使用常驻连接池之后比使用之前少了很多。
```
select username, program from v$session where username = 'PHPHOL';
```
（本文描述较为粗略，有一些步骤省略掉了，不过可以借此机会来涨一涨姿势，我就不啰嗦了。） 

**参考文档和连接：**

- **Oracle官方文档：**
[http://www.oracle.com/webfolder/technetwork/tutorials/obe/db/oow10/php\_db/php\_db.htm#s1](http://www.oracle.com/webfolder/technetwork/tutorials/obe/db/oow10/php_db/php_db.htm#s1)
- **Oracle官方文档（中文版）**：[http://www.oracle.com/technetwork/cn/tutorials/229068-zhs.htm](http://www.oracle.com/technetwork/cn/tutorials/229068-zhs.htm) 
- **基础使用：**[http://mo2g.com/view/38/](http://mo2g.com/view/38/) 
- **Apache Benchmark：**[http://blog.fabrichina.net/archives/250](http://blog.fabrichina.net/archives/250) 
- **laravel-oracledb：**[https://github.com/jfelder/laravel-oracledb](https://github.com/jfelder/laravel-oracledb)
