---
title: 记一次小故障
tags:
  - PHP
url: 936.html
id: 936
categories:
  - 我是修电脑的
date: 2015-01-12 17:28:13
---

网站后台有一个上传文件的程序，今天产品突然找到我说是不能使用了。我试了试，发现点击完上传按钮之后，老是跳转到链接被重置的页面，如下： ![](http://blog102no.qiniudn.com/20150112171012.png) 立马在本地查看了一下源代码，最开始的反应是文件upload\_max\_filesize小了或者是post\_max\_size的默认设置小了，查看了配置文件之后发现没什么问题，最大子好吃80M，而这个文件才23M，问题到底出在哪呢？然后前端分析了一下，发现这个提醒： ![](http://blog102no.qiniudn.com/1.png) 结果发现没什么用处。就再此时我突然想了一下有没有可能是后端的错误，比如是不是网络问题，服务器的空间大小问题等等，就在此时，我换了个浏览器重现了一遍错误，发现如下提醒：

413 Request Entity Too Large
The requested resource does not allow request data with the requested method or th
e amount of data provided in the request exceeds the capacity limit.

猛然想起，除了程序限制，还有NGINX限制，于是找运维，果然，限制最大为20M，改为50M，问题解决。
