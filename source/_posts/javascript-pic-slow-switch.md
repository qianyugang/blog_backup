---
title: javascript实现图片的缓慢切换
tags:
  - javascript
url: 20.html
id: 20
categories:
  - 我是修电脑的
date: 2012-05-21 08:31:13
---

在做图片切换的时候，往往直接的a:hover 看起来不是那么的赏心悦目，下面我贴出来用javascript实现图片的缓慢切换，让鼠标经过的时候看起来更有科技感，代码如下,demo就是[www.102no.com](http://www.102no.com)的首页哪样，具体参数可以自行修改。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
 <html xmlns="http://www.w3.org/1999/xhtml">
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
 <link rel="shortcut icon" type="image/x-icon" href="images/ficon.ico">
 <title>102NO.COM</title>
 <script type="text/javascript" src="js/jquery-1.3.2.min.js"></script>
 <script type="text/javascript">
 $(function() {
var fadeSpeed = ($.browser.safari ? 600 : 450);
$('#logotype').append('<span class="hover"></span>').addClass('noHover');
$('.hover').css('opacity', 0);
$('.hover').parent().hover(function() {
$('.hover', this).stop().animate({
'opacity': 1
},
fadeSpeed)
},
function() {
$('.hover', this).stop().animate({
'opacity': 0
},
fadeSpeed)
});
});
</script>
<style type="text/css">
a#logotype,a#logotype.noHover:hover { background:url(images/102no_off.png) no-repeat top left; display: block; position: relative; height: 47px; width: 502px; text-indent:-999em; overflow:hidden; }
a#logotype .hover { background: url(images/102no_on.png) no-repeat bottom left; display: block; position: absolute; top: 0; left: 0; height: 47px; width: 502px; }
a#logotype:hover { background-position: bottom left; }
</style>
<link href="css/main.css" rel="stylesheet" />
</head>
<body>
 <div class="demo">
 <a href=""><!--<img src="images/102no_off.png" />--></a>
 <a id="logotype" title="www.102no.com,分享生活和技术的地方" href="http://124.173.133.120/wordpress/"></a>
</div>
 <div>
</div>
</body>
 </html>
 ```
