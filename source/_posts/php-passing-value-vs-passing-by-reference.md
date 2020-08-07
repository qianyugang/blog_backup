---
title: PHP传值与传引用的区别
tags:
  - PHP
url: 727.html
id: 727
categories:
  - 我是修电脑的
date: 2013-08-20 14:44:27
---

其实关于传值还是传引用这个东西，可以把它当做深入的PHP知识来看待，可以当做初学必知来看待，尤其是初学者在PHP面试当中一定会遇到的面试题之一；其实一开始我也是不明白的，直到我在搜索的过程中发现了一个绝妙的比喻(原文见末尾链接)： 『   传值：跟copy是一样的。打个比方，我有一橦房子，我给你建筑材料，你建了一个根我的房子一模一样的房子，你在你的房子做什么事都不会影响到我，我在我的房子里做什么事也不会影响到你，彼此独立。 传引用：让我想起了上大学时学习C语言的指针了，感觉差不多。打个比方，我有一橦房子，我给你一把钥匙，我们二个都可以进入这个房子，你在房子做什么都会影响到我。』  先看看两者的定义： 

**按值传递：函数范围内对值的任何改变**在函数外部都会被忽略；仅将对象的值传递给目标对象，说白了就相当于copy；系统将为目标对象重新开辟一个完全相同的内存空间。

**按引用传递：函数范围内对值的任何改变**在函数外部也能反映出这些修改，也就是说真正的以地址的方式传递参数传递以后，行参和实参都是同一个对象，只是他们名字不同而已对行参的修改将影响实参的值。 

1，传值：
```
<?php
 $param1=1; //定义变量1
 $param2=2; //定义变量2
 $param2 = $param1; //变量1赋值给变量2
 echo $param2; //显示为1
 ?>
```
2，传引用：
```
<?php
 $param2=1; //定义变量2
 $param1 = &$param2; //将变量2的引用传给变量1
 echo $param2; //显示为1
 $param1 = 2; //把2赋值给变量1
 echo $param2; //显示为2
 ?>
```
3，函数传值：
```
<?php
 //传值
 $param1 = 1; //定义变量1
 function add($param2) //传参数
 {
 $param2=3; //把3赋值给变量2
 }
 $param3=add($param1); //调用方法add，并将变量1传给变量2
 echo '<br>$param1=='.$param1.'<br>'; //显示为$param1==1
 echo '<br>$param2=='.$param2.'<br>'; //显示为$param2== 因为$param2是局部变量，所以不能影响全局
 echo '<br>$param3=='.$param3.'<br>'; //显示为$param3== 因为add方法没有返回值，所以$param3为空
 ?>
```
4，函数传引用：
```
<?php
 //传值
 $param1 = 1; //定义变量1
 function add(&$param2) //传参数
 {
 $param2=3; //把3赋值给变量2
 // return $param2; //返回变量2
 }
 echo '<br>$param1=='.$param1.'<br>'; //显示为$param1==1 没对变量1进行操作
 $param3=add($param1); //调用方法add，并将变量1的引用传给变量2
 echo '<br>$param1=='.$param1.'<br>'; //显示为$param1==3 调用变量过程中，$param2的改变影响变量1，虽然没有return
 echo '<br>$param2=='.$param2.'<br>'; //显示为$param2== 因为$param2局部变量，所以不能影响全局
 echo '<br>$param3=='.$param3.'<br>'; //显示为$param3== 如果把方法里面的return注释去掉的话就为$param3==3
 ?>
```
5，函数传引用2
```
<?php
//传引用
 $param1 = 1;
 function &add(&$param2)
 {
 $param2 = 2;
 return $param2;
 }
 $param3=&add($param1);
 $param4=add($param1);
 echo '<br>$param3=='.$param3.'<br>'; //显示为$param3==2
 echo '<br>$param4=='.$param4.'<br>'; //显示为$param4==2
 echo '<br>$param1=='.$param1.'<br>'; //显示为$param1==2 调用变量过程中，$param2的改变影响变量1
$param3++;
/*下面显示为$param1==3，这是因为$param2和$param1引用到同一个地方，
 \* 返回值前面加了地址符号还是一个引用$param3=&add($param1);
 \* 这样$param3,$param2和$param1引用到同一个地方，当$param3++;时，
 \* $param1会被改变*/
 echo '<br>$param1=='.$param1.'<br>';
$param4++;
 /\* 下面显示为$param1==3,这里为什么是3而不是4呢，这是因为返回值前面没有
 \* 地址符号，它不是一个引用所以当$param4改变时不会影响$param1*/
 echo '<br>$param1=='.$param1.'<br>';
 ?>
```

**关于运行速度：** 其实在少量数据的情况下其实并没有什么太大的影响，但是数据量很大的情况下按值传递时，php必须复制值。特别是对于大型的字符串和对象来说，这将会是一个代价很大的操作。按引用传递则不需要复制值，对于性能提高很有好处。 参考链接 ： [5个php实例，细致说明传值与传引用的区别](http://blog.51yip.com/php/878.html) [案例解析PHP中传值、传引用](http://bbs.phpwinner.com/viewthread.php?tid=611) [PHP传值和传引用、传地址的区别](http://www.cnblogs.com/uooki/archive/2012/08/02/yingyong.html)
