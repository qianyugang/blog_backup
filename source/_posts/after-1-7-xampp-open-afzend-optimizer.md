---
title: 1.7后的Xampp如何开启Zend Optimizer？
tags:
  - PHP
url: 32.html
id: 32
categories:
  - 我是修电脑的
date: 2012-05-23 07:12:56
---
Xampp1.7.3如何开启Zend Optimizer？ 

在XAMPP中开启Zend Optimizer XAMPP中默认是安装了Zend Optimizer，但是默认并没有打开Zend Optimizer，要想打开Zend Optimizer，你必须将XAMPP中的以下两个文件：\xampp\php\php.ini和\xampp\apache\bin\php.ini中的zend_optimizer.enable_loader = 0改成1，重启Apache，Zend Optimizer就启动了。

以上的方法只适合旧版的XAMPP，Xampp1.7后的PHP是5.3的，而Zend Optimizer目前只支持到PHP5.2，官方网站上有写。

我去百度搜索过了，像根目录中的xampp\apache\bin\php.ini文件之说，根本不存在，还有修改XAMPP/APACHE/BIN中PHP.INI的文件也不存在。这方面的答案就不要发了。

的确，现在xampp 1.7.3中用的php版本都在5.3.1以上了，而zend optimizer 3.3.3却只能支持到5.2.x，这样，如果想在这上面运行一些通过zend guard加密的程序估计是不行了，反正我是不成功了！

XAMPP 1.7.2 默认PHP加速是使用eaccelerator加速的，功能上相当于Zend Optimizer，但是缺少ZEND OPTIMIZER的网页加密解析功能。

最新的Zend Optimizer 3.3.3不支持PHP 5.3x，最高到PHP 5.2.x，估计稍后Zend Optimizer发布新版本的时候才能支持，所以如果大家仍然想使用Zend Optimizer，可以采用以下方法：

1、不使用XAMPP，全部手动安装PHP、APACHE、MYSQL和Zend Optimizer。
2、使用XAMPP的早期版本，网上也能搜索到，早期版本默认支持ZEND OPTIMIZER。
3、Zend Optimizer 3.3安装的时候会自动为PHP.INI文件增加Optimizer引擎接口。
4、如果大家不使用网页加密只使用PHP加速，就是用eaccelerator就可以了。
5、最后一点，就是xampp仅作开发环境使用，请不要用于服务器环境，因为xampp有很多安全问题未作处理，官方也特别做这个声明。
