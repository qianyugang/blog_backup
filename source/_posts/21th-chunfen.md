---
title: 第二十一年春分
tags:
  - 思想汇报
url: 56.html
id: 56
categories:
  - 说过的话
date: 2012-02-21 22:01:19
---

**2012**年正月还没怎么落下帷幕，老菜远在郑州，电话说他在家呆不住了，初十就想去武汉，去年年底的打算就是回家过个好年，来年了再找个地方工作，学点经验长点知识，给将来的孩子攒点奶粉钱神马的，稍微的一商量，正月十一的就直奔武汉。

来了之后没地方住，就在刘超这里蹭着，蹭啊蹭，蹭了半个多月，在这半个多月之间，面试了有大约五六家公司吧，薪水也都就那个级别，印象最深的一个公司就是叫神马斯瑞德科技公司，那个老总在互联网里面涉及较深，问了我很多我并不知晓的问题，b/s ,c/s（后来其实了解这就是比较基本的客户端对服务器和浏览器对服务器，也就是软件和web）那个老总说了一些我印象比较深的话，反正意思就是我不行，让我再回学校学个半年再过来，不过他交我这个朋友。

那是我去面试的第一家公司，出师不利，我打了个电话问老蔡，问他面试的怎么样，他那边还好，就是跟他的要求比较起来有一些差别，我就让他来这边试一下，结果，他拿到offer，试用一周，我有面试了一家有自己运营网站的公司，感觉也不怎么良好，叫做奇米网络有限公司，期间问了几个问题，有些答上来了，有些没答上来，这些都还好，就是感觉面试我的那个人比我还要紧张一些。最后，我进了老蔡推荐给我的一家公司，也就是说，我们俩进了互相推荐的公司。结果，结果，结果就是，那家斯瑞德是软件公司，老菜是网络部的唯一一个人，老总提的问题我俩都没答对的原因是那都是涉及到软件的，也就是说基本不是一个行业的，感觉是有点坑爹的。（插播：同样是程序猿，为什么做网站的都是单休朝九晚六的，做软件的都是朝九晚五双休呢？答案是：因为软件是孙子，网站是儿子，姥爷总是更疼孙子。）于是，三天后，我们俩都到了那个老蔡当初推荐我的那个公司，也就是呆到现在的公司。

OK，流水账记到这里就告一段落。

正文开始：我一共在这里呆了两周，第一周，主要解决问题是用在虚拟机里面用centos搭建一个比较强大的组合：[Nginx 0.8.x + PHP 5.2.13](http://www.102no.com/wordpress/?p=26 "详细阅读 Nginx 0.8.x + PHP 5.2.13（FastCGI）搭建胜过Apache十倍的Web服务器")，公司的一个比较资深的程序员给我一片文章让我尝试着搭建，就是这篇[Nginx 0.8.x + PHP 5.2.13（FastCGI）搭建胜过Apache十倍的Web服务器](http://www.102no.com/wordpress/?p=26 "Nginx 0.8.x + PHP 5.2.13（FastCGI）搭建胜过Apache十倍的Web服务器")，是张宴老师写的，我照着上面的步骤来，麻烦挺多的，后来装到php那步实在是卡住了，后来我就着重了解了一下这几个东西，php和nginx是互相不认识的，所以搭建比较麻烦，它们俩的组合，跟apache不是一个级别的。搭建这个东西的目的是解决一个问题，就是加载视频的时候要实现缓冲自由拖动，就用到一个yamdi的处理命令。也就是给视频加上一个Matadate，也就是流媒体关键帧注入。就可以实现了。第二周，因为公司数要是做视频网站的，我们就用到了ccvms，下载了一个，因为我电脑本身的环境是xamp，需要安装一个Zend Optimizer，但是怎么都安装不上，经过不断地百度，谷歌，原因是，xampp已经封装了这个东西，需要开启。可是我怎么都无法开启，后来了解是版本问题，解决方案：[Xampp1.7.3如何开启Zend Optimizer？ ](http://www.102no.com/wordpress/?p=79 "Xampp1.7.3如何开启Zend Optimizer？  ") 。这几天在改版升级一个网站，使用ccvms的，感觉还好，跟得上脚步，学到了之前不了解的东西。这几天也抽空把这个www.102no.com弄起来了。第一篇就这么写吧，凑合着看，凑合着先~~~