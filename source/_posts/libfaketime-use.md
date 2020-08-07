---
title: 时间测试神器libfaketime的使用
toc: true
categories: 与代码
tags: 
	-
	- faketime

date: 2019-11-26
---

在做开发测试的时候，时常会遇到一些需要时间设置的问题，通常的时候，我们就是直接修改系统时间来完成，但是由于一般服务器上会跑着很多服务，一旦修改难免会影响到其他的程序，所以我们得找到一个方便的，只对自己需要使用的服务或进程修改时间，而不影响其他的，且修改方便的神器。好在有这么一款好用的：

- https://github.com/wolfcw/libfaketime/

根据其官方介绍如下：

>　libfaketime会拦截程序用于检索的各种系统调用当前日期和时间。然后报告并修改(伪造)的日期和时间(由您用户指定的)到这些程序。这意味着您可以修改系统时间一个程序不需要改变系统范围内的时间。
> libfaketime　允许您指定绝对日期(例如，01/01/2004)和相对日期(如10天前)。
> 例如，libfaketime可以用于各种目的
- 确定构建过程
- 调试与时间相关的问题，如过期的SSL证书
- 测试软件在2038是否依旧合规

>　libfaketime附带一个名为“faketime”的命令行包装，易于使用，但不公开libfaketime的所有功能。如果你的
用例没有被faketime命令覆盖到，请确保查看此命令文档它是否可以通过直接使用libfaketime来实现。

当然还有很多的小伙伴，用它来破解一些macos下的有时间限制的软件。

## 安装使用

```
	git clone https://github.com/wolfcw/libfaketime.git

	cd libfaketime  && make install
```

ubuntu用户也可以使用这个：

```
	sudo apt-get update -y
	sudo apt-get install -y faketime
```

### 使用faketime命令

安装完成之后，就可以直接先查看一下faketime命令:

看一下此时的时间
```
date
2019年 11月 26日 星期二 14:46:25 CST
```
直接指定时间：
```
faketime '2018-03-27 21:04:52' date
2018年 03月 27日 星期二 21:04:52 CST
```
指定时间从10天前开始
```
faketime -f '-10d' date
2019年 11月 16日 星期六 14:48:43 CST
```

### 使用环境变量

这里我们使用一下php的脚本来执行测试一下，把php这个脚本的时间指定到15天前
```
php -r "echo date('Y-m-d H:i:s');"
2019-11-26 14:53:16

LD_PRELOAD=/usr/lib/x86_64-linux-gnu/faketime/libfaketime.so.1 FAKETIME="-15d" php -r "echo date('Y-m-d H:i:s');"
2019-11-11 14:53:16
```

首先这里要找到并确定`libfaketime.so.1`的安装位置，才可以直接赋予环境变量。

一些具体的使用方法可以在https://github.com/wolfcw/libfaketime/blob/master/test/test.sh 中查看。


### 示例

**指定绝对日期2003-01-01 10:00:05作为运行测试程序**
```
LD_PRELOAD=/usr/lib/x86_64-linux-gnu/faketime/libfaketime.so.1 FAKETIME="2003-01-01 10:00:05" php -r "echo date('Y-m-d H:i:s');"

# 该时间会一直保持不变

2003-01-01 10:00:05
```

**在指定开始日期@2005-03-29 14:14:14的情况下运行测试程序**

```
LD_PRELOAD=/usr/lib/x86_64-linux-gnu/faketime/libfaketime.so.1 FAKETIME="@2005-03-29 14:14:14" php -r "echo date('Y-m-d H:i:s');"

# 时间会从这里往后递增

2005-03-29 14:14:14
```

**在指定10天前的情况下运行测试程序**

```
LD_PRELOAD=/usr/lib/x86_64-linux-gnu/faketime/libfaketime.so.1 FAKETIME="-10d" NO_FAKE_STAT=1 php -r "echo date('Y-m-d H:i:s');"

# 时间会到10天前
```

**在指定10天后并有加速因子的情况下运行测试程序**
```
LD_PRELOAD=/usr/lib/x86_64-linux-gnu/faketime/libfaketime.so.1 FAKETIME="+10d x1" NO_FAKE_STAT=1 php -r "echo date('Y-m-d H:i:s'); sleep(10); echo PHP_EOL; echo date('Y-m-d H:i:s');"

2019-12-06 15:05:52
2019-12-06 15:06:02
```



















