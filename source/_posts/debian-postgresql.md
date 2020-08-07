---
title: 解决在debian 9 中安装 PostgreSQL 9.6 中No such file or directory问题
toc: true
categories: 与代码
tags: 
	- PostgreSQL

date: 2019-07-23
---

最近需要在debian 9 中安装 PostgreSQL 9.6 数据库，按照常规步骤：
先更新源
```
sudo apt-get update
sudo apt upgrade
```
然后安装PostgreSQL
```
sudo apt-get install postgresql-9.6
```

就在安装的时候突然报如下错误并中断：
```
Setting up postgresql-client-9.6 (9.6.13-0+deb9u1) ...
update-alternatives: using /usr/share/postgresql/9.6/man/man1/psql.1.gz to provide /usr/share/man/man1/psql.1.gz (psql.1.gz) in auto mode
update-alternatives: error: error creating symbolic link '/usr/share/man/man1/psql.1.gz.dpkg-tmp': No such file or directory
```
安装失败，然后发现是一个路径找不到，需要手动创建一下，然后google之后找到了相同问题：https://github.com/debuerreotype/debuerreotype/issues/10

于是乎采纳一下解决方案，执行如下代码新建一下相关文件夹：
```
for i in $(seq 1 8); do mkdir -p "/usr/share/man/man${i}"; done
```
执行完之后可以去`/usr/share/man/`目录下看一下是不是有8个man文件，执行成功就可以继续安装即可。
