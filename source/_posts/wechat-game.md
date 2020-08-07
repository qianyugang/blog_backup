---
title: 微信点击小游戏
tags:
  - 记录
url: 902.html
id: 902
categories:
  - 我是修电脑的
date: 2014-12-01 11:58:51
---

最近在开发过程中有一个需求，是要做一个小游戏，游戏的主要玩法就是在规定的时间内快速的点击一个按钮，然后计算次数，点的越多越好。 其实实现起来也是比较简单的，如果用jquery的话，代码还算是比较简单的。 首先要两张点击按钮的图片，一张是点击前，一张是点击后的；也可以直接用CSS画出图标来，写好样式，或者直接替换图片。 首先定义一个按钮点击的方法，如下：
```
    var qiuku = 0;//定义一个基数
    $('#clickqiuku').on('touchstart', function () {
    if (qiuku == 0){
        intervalId = setInterval("countDown()",1000);//触发倒计时
    }
    $('#qiukuimg').attr("src","http://res.uxin.com/market_res/2014123310522490.png");
    //改变图片
});
```
再定义一个按钮点击完的方法
```
    var qiuku = 0;
    	$('#clickqiuku').on('touchend', function () {
	qiuku++;//点击数量
	$("#qiuku").html(qiuku);
   		$('#qiukuimg').attr("src","http://res.uxin.com/market_res/20141233105223884.png");
	})
    });
```
然后再定义一个倒计时的方法，如下：
```
//倒计时
var leftSeconds = 10;
var intervalId;

function countDown(){//定义一个倒计时函数，在文档加载后执行，并且每隔1秒执行一次
    if(leftSeconds <=0){
        $("#clickqiuku").attr('href', 'javascript:void(0);');
        clearInterval(intervalId);//取消由setInterval()设置的timeout
        alert("恭喜您抢到了"+data+"条秋裤");
        return;
    }
    leftSeconds--;
    $("#time").text(leftSeconds);
}
```
差不多这样就好了，如果有什么和后台通讯就用
```
$.post
```
就可以了。 需要注意的是，touchend这个属性可能在一些手机浏览器上不是特别支持，也就是有不触发的情况，也就是touchmove起了冲突， 这时候需要用到
```
e.preventDefault(); // 可触发
```
但是这时候要确保你的屏幕是整个一屏的，不能再滚动了。
