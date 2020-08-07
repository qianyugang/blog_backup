---
title: iOS如何直接安装.ipa文件
tags:
  - iOS
url: 948.html
id: 948
categories:
  - 我是修电脑的
date: 2015-01-22 18:14:51
---

在很多时候，我们发布一个iOS应用，其实是没必要一定走Appstore的，审核啊什么的比较费时，需要时间，比如这次的年会APP，完全不需要，那么怎么办呢，怎么让大家安装iOS应用呢，这里就给出一个流程方法，可以实现，不过，首先要提醒的是，必须是企业用户，非企业用户是坚决不能的。

*   首先，需要编辑一个.plist文件，这个文件的作用也就是把你的下载信息，还有iOS工程文件信息，文件内容如下，有几个地方需要注意：

![](http://blog102no.qiniudn.com/nianhuiapp2015-01-22_180212.png) 需要注意的是

http://xxxx.com/NewYearApp(ios)-2015-01-20.ipa

这里的地址是你的.ipa文件地址；

bundle-identifier

这里必须跟你的APP内部的工程文件bundle一样以便于验证；剩下的就是一些版本啊内容信息，按照规则填写即可。

*   然后，把这个.plist文件上传到HTTPS服务器，记住，必须是HTTPS服务器，这里可能需要运维格格帮忙。
*   其次，获取到这个文件的地址；
*   最后，组装连接，如下：

 <a href="[itms-services://?action=download-manifest&url=https://www.xxxx.com/nianhui.plist](itms-services://?action=download-manifest&url=https://resssl.guoling.com/hz/zhuhao/nianhuidown.plist)">点击我下载</a>

然后就可以做一个页面，做一个超链接，内容如上就可以下载了，注意url后面跟随的地址一定是HTTPS的，下载的时候需要下载的人确认授权弹窗就完成了，有时候会出现无法安装的情况，那么有可能是iOS在选择系统版本兼容的时候选的过高，一般5.0+就可以了。
