---
title: MacBook Pro 修复之路
toc: true
comments: true
categories: 我是修电脑的
tags: 
	- 生活经验

date: 2022-07-22
---

最开始使用MacBook是在2015年，那时候的公司给每个开发都发了2015款的13寸MacBook Pro作为办公笔记本，后来2017年的时候我自己在小闷那里买了2016款的15寸MacBook Pro，花了￥13500，用到现在也五年了，这五年笔记本也都出现过或大或小的问题，今天这篇文章就汇总一下我这台MacBook Pro出现过的软硬件问题以及解决方案。

## 屏幕问题

这个问题就是昨晚出现的，用着用着就突然黑屏了，按键背光和touch Bar都还亮着，我第一反应是不是死机了，于是乎强行重启了几次，发现依旧没有用，就怀疑是不是硬件问题。一瞬间想到，这款Mac好像是有屏幕背光问题，我不会是也遇到了吧，我就用手机的手电筒照着屏幕一看，竟然还能看到文字内容，这下确定了，屏幕的显示面板没问题，但是背光坏掉了。

### 尝试修复

重启大法不好使了之后，就立即联系了苹果的电话售后，售后给了几个解决方案，让我挨个试试，还帮忙预约了天才吧，如果这几个方案不好使，就直接去售后。

- [如何重置 Mac 的 SMC](https://support.apple.com/zh-cn/HT201295)
- [重置 Mac 上的 NVRAM 或 PRAM](https://support.apple.com/zh-cn/HT204063)

我直接使用了重置SMC的方案，重启之后，竟然恢复了，心想有可能是什么驱动问题，重置竟然解决了，欣喜了一晚上之后。第二天一开电脑，擦，又挂了，看来还是硬件问题，还是得去售后。

### 苹果服务计划

于是我一顿搜索，发现这个问题还真的是通病，而且苹果还针对其突出了服务计划[13 英寸 MacBook Pro 显示屏背光服务计划](https://support.apple.com/zh-cn/13-inch-macbook-pro-display-backlight-service)，一看内容，坑爹的是15寸的竟然不在列表之内。但是天才吧已经预约了，还是去碰一碰，看看给不给免费修，如果不给免费修，我就打算自己来或者丢去华强北修，毕竟也是一个老机器了，以苹果的尿性，自费修复估计也是换总成得花大几千，这个电脑残值可能都还不值这么多。

这个通病的原因，搜了一圈有说是因为2016年开始用了新模具之后，排线设计过短的问题，在多次弯折之后就会触发这个硬件问题。除了黑屏，暗屏，还有可能舞台灯效果屏等等症状。2018之后的很少有这个问题，因为加长了排线。

~~截止目前还没有送去天才吧，后续更新。~~

去了天才吧，天才直接直接说我们这里修要换整个屏幕，报价是￥5100，我心说这价格比我这电脑残值都高了。天才也直接说不推荐在苹果官方修理，而且马上这个型号的电脑就超过五年了，超过五年之后也无法修复，没有配件了。

我说你们不是有个什么屏幕维修计划吗，然后天才当面打开了显示屏背光服务计划的网页，说你看这是13寸才给维修，你这是15寸的，我说15寸也有这个毛病啊为啥不给修，天才说目前政策是这样。

直接转战华强北，问了几家都是￥500左右，找了最近的一个小时修复完毕，更换排线以及一个供电IC。其实如果自己动手能力比较强而且确认是屏幕排线问题的话可以自己修复，淘宝配件在￥100左右。

## 按键问题

按键问题也是个老问题了，2016款是第一代的蝴蝶键盘，稍微进一点点灰就会卡住，每隔一段时间都要用气吹吹一吹，不然就会不知道哪个按键被卡住。

然后终于又一次整个command按键直接坏掉了，按下去起不来，应该是一个卡扣断掉了，我直接淘宝买了一个按键￥35，直接自己换上完事儿。

## 音响问题

也算是一个老问题（怎么都是老问题），左边的喇叭📢破音了，音量不敢开太大，尤其是高音必破，这个我都没管它，现在还存在，直接长期使用右声道，或者插上耳机使用。

也不知道是硬件问题还是软件问题，有说升级系统可破，暂时还没尝试，苹果论坛也有同型号问题反馈

- [MacBook Pro扬声器爆音，破音，轰鸣](https://discussionschinese.apple.com/thread/253460648)

## 转轴异响

这个问题从买来没多久就出现过了，当时直接拿到天才吧检测了，天才当时给的说法是不保证找到问题，可能要换主板、换这换那的，还要让我备份电脑之类的，当时嫌麻烦，而且电脑资料也迁移了不少。于是当时就没修复。

主要症状就是运行一段时间之后，在转轴处会出现「啪」的一声，我自己也没有找到什么特别的规律，有时候很久都不出现，有时候出现的频繁。

- [苹果论坛类似反馈：2020款MacBook Pro转轴异响](https://discussionschinese.apple.com/thread/251695076)
- [也有老哥受不了了自行动手解决：bilibili.com](https://www.bilibili.com/video/av796212156/)

## 一些软件问题

除了硬件，有时候还会出现一些软件问题

### 睡眠唤醒没声音

就是直接合上盖子，再次打开的时候电脑没有声音了。这个好像是音频核心进程的 bug，只要杀掉这个进程即可，它会再次自动启动，执行如下命令：

```
sudo killall coreaudiod
```

### WiFi链接卡顿转圈

就是在点击 WiFi 图标，选择 WiFi 的时候 cpu 突然飙升，巨卡无比，然后就开始转彩虹菊花，这个查询是因为 Airpotd 的问题，可以执行如下命令解决：

```
sudo killall airportd
```






