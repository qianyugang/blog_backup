---
title: 这些日子遇到的令人难过的BUG
tags:
  - PHP
url: 762.html
id: 762
categories:
  - 我是修电脑的
date: 2014-02-20 07:26:26
---

最近一直都是在做还算精细的字符串处理，但是有遇到一些当时花费了好久才都没解决但是最后发现是一个php的bug的事情~ **1、strtotime的BUG** 在用strtotime('-x month')，函数往前推月份的时候，有时候会出现月份不准的情况；这是PHP的一个官方BUG，详情见此：[https://bugs.php.net/bug.php?id=27793](https://bugs.php.net/bug.php?id=27793 " Bug #27793	strtotime behaving erraticly in month change")； 尤其是如果涉及到的月份是31天或者是28,29天就很容易跑偏，比如：

```
<?php
date\_default\_timezone_set('Asia/Shanghai');
$t = time();
print_r(array(
            date('Y年m月',$t),
            date('Y年m月',strtotime('- 1 month',$t)),
            date('Y年m月',strtotime('- 2 month',$t)),
));
```

你想要的结果是这样子：
```
Array
(
    \[0\] => 2011年08月
    \[1\] => 2011年07月
    \[2\] => 2011年06月
)
```
但是结果却是这样子：
```
Array
(
    \[0\] => 2011年08月
    \[1\] => 2011年07月
    \[2\] => 2011年07月
)
```
是不是很恼火（当时就为这个推算花了一下午折腾，哭），那么解决方法呢？获取需要知道月份的第一天的时间就可以了； 代码如下：
```
<?php
date\_default\_timezone_set('Asia/Shanghai');
$first\_day\_of_month = date('Y-m',time()) . '-01 00:00:01';
$t = strtotime($first\_day\_of_month);
print_r(array(
            date('Y年m月',$t),
            date('Y年m月',strtotime('- 1 month',$t)),
            date('Y年m月',strtotime('- 2 month',$t)),
));
```
输出
```
Array
(
    \[0\] => 2011年08月
    \[1\] => 2011年07月
    \[2\] => 2011年06月
)
```
**2、大小写转换函数strtolower()；strtoupper()** 这个是在处理验证码的时候发现的；其实这个不算是一个BUG，是一个字符编码的问题，通常验证码输入是不分大小写的，所以需要函数转换一下即可，但是不知道为什么在我本地环境是没什么问题的，上传到服务器就会发现和session里面的验证码对比不准确的奇怪事情，有时候相等有时候不相等，思来想去本地和服务器唯一不同的就是环境了，服务器是linux的，于是我当下就查阅了一下，发现一个官方的BUG：[https://bugs.php.net/bug.php?id=19257](https://bugs.php.net/bug.php?id=19257 "Bug #19257	strtolower & strtoupper does not work for UTF-8 strings") 但是这个貌似不是我遇到的问题；那还有不一样的是PHP的版本，于是有查阅了，还有个版本不一样和字符集不一样的问题： 在　PHP Version 4.3.2　中，`测试`==strtolower('测试') 在　PHP Version 4.3.4　中，`测试`!=strtolower('测试') 然后经过查阅问题的原因是本地字符集不是中文而导致的。如果是linux系统，你只要：
```
export LANG=zh_CN.GB18030
```
就可以了。 好吧，我不管这些了；直接说最后的解决方案吧：好的就是 [strncasecmp()](http://www.php.net/manual/en/function.strncasecmp.php "strncasecmp — Binary safe case-insensitive string comparison of the first n characters") 你没跑了，二进制安全比较字符串开头的若干个字符，自定义长度就好了；验证码就指着这个活了。 **3、字符串截取函数：substr()** 可能有人问了，这么基础的函数也尼玛有BUG ? 擦，真有，昨天在处理字符串的时候，需要取出一个二进制数字从右往左数的第十六位，看看是否为“1”，问题来了：因为二进制是十进制转换来的，有一些数字专函过来之后是没有十六位的，那么在window下默认就是为“0”了，但是放到linux上面就不是了，放到linux下的话，如果没有到第十六位的话它获取的字符就是最左边的那个数字，比如这样：
```
$action = '1000000000';//（转换为二进制之后的）
var_dump (substr($action, -16, 1) );
```
在本地window输出为"bool(false)"，但是放到linux下就输出为string(1) "1" ，快哭了好么，就这个也折腾了不少时间； 但是字符还是要取出来啊，怎么办，写一个函数解决吧，翻转一下字符串正着数总好了吧：
```
function getbit($num, $bit) {
    $num = decbin ( $num );
     $num = strrev ( $num );
     if (substr ( $num, ($bit - 1), 1 )) {
         return 1;
     } else {
         return 0;
     }
}
```
参考资料：

1.  [http://spamer.iteye.com/blog/1162625](http://spamer.iteye.com/blog/1162625)
2.  [http://bbs.csdn.net/topics/60020464](http://bbs.csdn.net/topics/60020464)
3.  [https://bugs.php.net/bug.php?id=27793](https://bugs.php.net/bug.php?id=27793)
4.  [https://bugs.php.net/bug.php?id=19257](https://bugs.php.net/bug.php?id=19257)
