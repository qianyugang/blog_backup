---
title: 微服务网关 APISIX 初探
toc: true
categories: 与代码
tags: 
	- APISIX
	- api网关

date: 2020-01-08
---

APISIX 是一个高性能的微服务API网关，之前在使用Kong的时候有了解过这个产品。如今这个项目已经进入了Apache开始孵化。这个网关的作者是编写了OpenResty的著名教程《OpenResty 最佳实践》的温铭和王院生。都是业界大牛，而且和kong一样都是基于 OpenResty。

- 项目地址：https://github.com/apache/incubator-apisix
- 文档入口：https://github.com/apache/incubator-apisix/blob/master/doc/install-dependencies.md
- docker入口：https://github.com/apache/incubator-apisix-docker

## 安装

## 数据库etcd安装并启动

etcd是一个基于Go语言实现的高可用的分布式键值(key-value)数据库，性能应该是很不错的。
直接安装:
```
sudo apt-get install etcd
```
然后启动数据库服务:
```
sudo service etcd start
```

## APISIX安装

安装文档中有很多种方式安装，选择自己适合的安装方式即可，由于之前已经安装过kong了，所以openresty的环境已经有了，我们就使用最简便的luarocks来安装：

从luarocks网站中找到：
- https://luarocks.org/modules/membphis/apisix

然后直接执行安装：
```
luarocks install apisix
```
注意这里如果你的openresty没有添加到环境变量中，也有可能是不会成功的，记得添加一下。出现这句话安装成功：
```
apisix master-0 is now installed in /usr/local (license: Apache License 2.0)
```

## APISIX启动

上面都安排妥当之后，直接启动APISIX
```
sudo apisix start
```

