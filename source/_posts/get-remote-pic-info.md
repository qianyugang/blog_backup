---
title: 如何高效的获取远程图片的长宽、格式、文件大小
tags:
  - PHP
url: 1027.html
id: 1027
categories:
  - 我是修电脑的
date: 2015-12-18 17:13:11
---

最近在做一个图片抓取系统，其中有一个地方是需要用到获取远程图片的长宽，大小，和格式信息；不过一提到获取这样的信息，那么脑海里马上就会浮现 `getimagesize()` 这个函数，但是，在真正用到的时候才发现，这个函数很笨，效率很低，一方面是它会把这个图片直接下载下来，然后再获取其中的信息，而我们又知道，下载东西，CURL肯定是迅猛无比的；那么能不能把这两者结合起来呢~ 但是问题又出现了，如果我用CURL的方式，就不太容易获取图片长宽信息了，不够方便，于是乎我就翻阅了文档中和图片有关的函数，就找到了这么一个[_getimagesizefromstring()_](http://php.net/manual/zh/function.getimagesizefromstring.php)，刚好符合我的要求！于是乎你只需要下面这个函数就可以比较高效的获取啦！

```
public function get\_remote\_filesize($url){
       $ch = curl_init ();
       curl\_setopt ( $ch, CURLOPT\_CUSTOMREQUEST, 'GET' );
       curl\_setopt ( $ch, CURLOPT\_SSL_VERIFYPEER, false );
       curl\_setopt ( $ch, CURLOPT\_URL, $url );
       ob_start ();
       curl_exec ( $ch );
       $return\_content = ob\_get_contents ();
       ob\_end\_clean ();
       $return\_code = curl\_getinfo ( $ch, CURLINFO\_HTTP\_CODE );
       $len = floatval( round(strlen($return_content)/1024/1024,2) );
       echo $len;
       var\_dump(getimagesizefromstring($return\_content));
}
```

说点别的，其实获取远程图片信息有很多种方法，比如`_file\_get\_content()`，比如使用`_get_header()`，获取页面头中的信息来判定图片，但如果对方服务器不允许获取header信息，那就不行了。所以相比而下依旧是CURL效率最快，而且CURL可以模拟各种请求，高效而强大，在批量下载文档，或者图片之类的，CURL方法仍是首选。
