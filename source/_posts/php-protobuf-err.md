---
title: 解决 protobuf 3 PHP 中枚举值报错问题
toc: true
categories: 与代码
tags: 
	- PHP

date: 2019-11-15
---

前两天在项目中突然遇到一个protobuf php库报错，报错为：
```
"Undefined offset: 8","context"
```
这个一看就是直接是索引下标，找不到值就超出报错了，再往前回溯一下报错，发现这个错误是
```
Google\\Protobuf\\Internal\\Message
```
报出的。后来看了项目的.proto文件，发现是其他项目的同事更新了这个proto文件，把一个结构体的枚举值添加了一个，导致我们这边项目在收到这个结构体的时候无法识别。此时其实只要更新proto文件就可以解决问题，但是这样并不能一劳永逸，下一次遇到了枚举值增加的时候依旧会出现。

不过按说谷歌的protobuf php库用了这么多年应该没什么大问题，回溯一下代码发现在
```
https://github.com/protocolbuffers/protobuf/blob/master/php/src/Google/Protobuf/Internal/EnumDescriptor.php
```
这行代码的问题，这里直接使用的是，没有做异常处理。
```
  return $this->value[$number];
  ```
再接着经过同事的发现，这个问题已经解决了，也已经有人提了pr
```
https://github.com/protocolbuffers/protobuf/pull/6155#issuecomment-493795218
```
只需要把依赖升级一下即可，之前使用的是`3.6.*`，直接升级到`3.10.*`就可以解决问题。
