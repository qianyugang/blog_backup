---
title: 一个APP的诞生(二)
tags:
  - 记录
url: 942.html
id: 942
categories:
  - 我是修电脑的
date: 2015-01-20 16:49:38
---

经过很久很久很久的一系列时间开发；终于算是完成了，我也从头到尾，从前端到后端经历了一个完整的APP开发，我在这个APP中主要做的工作是后端接口的提供，前端内嵌页面的实现，还有一些辅助功能的开发iOS和Android兼具的完整应用。 应用的名称叫做“飞越疯人院”，恩，一群没有节操的人开发出来的所以就叫这个名字了；稍微介绍一下这个APP；一共有三个大功能，第一个就是查看公告，也就是有后台发布一些和年会相关的公告；然后第二个功能是投票，就是给年会上表演的节目做投票；还有一个就是照片墙，把公司所有员工的信息都展示出来。今天写的这篇差不多一个月前写的这篇[一个APP的诞生](http://www.102no.com/wordpress/archives/924 "一个APP的诞生")的第二篇，也是完结篇来了，完整的APP效果图如下（这里只放iOS版本的）： ![](http://qiniu.102no.com/nianhuiapp22.jpg) 其实这个APP还是有诸多不完善的地方，比如后端来说有一些接口设计的不合理，有一些数据可以直接整合到一起却分开了，前端来说还有一些体验做的不是很好，而且还有一些bug；不过Anyway，不管怎么说这个也是一个完成度很高的APP了，毕竟利用业余时间，这么短的时间内从设计到开发到上线，也算是小有成就感，在开发这个APP里面遇到了一些问题，我在这里记录一下； 后端：

*   首先遇到的一个问题是关于整合json字符串的问题，在包登陆接口的时候，出现了一个安卓不能使用字符串但是苹果可以使用的问题，然后解决方案是 安卓多加一个参数，然后我回传另一个json字符串，原因我后来分析，应该是我这边封装的json不太标准，数组的格式有问题。
*   然后，还是登陆问题，需要一个单点登陆，也就是需要这个APP支持一次只支持一个人登陆；在我咨询了后端工程师之后，得到的答案是必须使用TCP长连接，不然无法跳线。很悲剧的是TCP长连接很难调试，时间又太短，于是乎，我想了个比较原始的方法：客户端每五分钟请求一次登陆接口，请求的时候带上手机的imei参数，然后我新建一个用户池，每个用户绑定一个imei，每次登陆请求我就查询一次，这样就可以基本保证这个功能的使用了。但是有五分钟的时差，可以忽略不计了。（注，因为公司只有500多个人，只是内部使用app，没有考虑并发情况，如果并发太大，可能会造成用户池的写入和读取太频繁，造成数据库锁表等问题。），不过这个功能最后砍掉了。
*   还有关于点赞设计，其实很简单，建一个表，记录时间线、操作的人、操作的动作、然后计算like的时候就算数字就可以了，获取莫个人对这个动态的like状态的时候就直接获取时间线最新的那条即可，不过我这边有个特殊需求，就是会有个集赞活动，所以不管是评论，还是个人的赞都必须放在一起，所以在user表内我加了一个like字段，用来计算数字，每次update一下，这个其实是不规范的，也会存在并发问题，若不是内部使用，建议不要做update数字这个步骤。

前端：

*   适配问题，但凡是有安卓手机，就必然有适配问题，网页在开始制作的时候就考量了使用百分比来标注宽度，也算是一种响应式，用来兼容大屏幕小屏幕。需要注意的是分辨率是320左右的时候会出现一些意想不到的差错。
*   手机浏览器里面有一个常见的BUG，就是输入框的下面，如果没有足够的长度用来容纳虚拟键盘，那么在弹出虚拟键盘的时候，就会遮住输入框，简单粗暴的解决方案就是加长网页，当然也可以让客户端在内置浏览器呼起虚拟键盘的时候缩进网页。

当然遇到的问题可能不止这么多，还有一些协调问题啊，还有一些人员分配问题啊，等等等等，所以开发一个APP是不容易的，而且在后端只有你一个人的情况下；这也算是一段经历吧，以此记录下来。