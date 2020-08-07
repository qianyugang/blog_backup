---
title: Kong 无法使用 lua-openssl
toc: true
categories: 与代码
tags: 
	- Kong
	- Kong中文教程

date: 2019-09-10
---

目前在写一个kong的插件的时候想要使用一下 lua-openssl 的ecc加密功能，ecc是一个相对较新的加密算法（椭圆加密算法）。

lua-openssl 的项目地址为：https://github.com/zhaozg/lua-openssl 然后按照文档中的步骤，先安装：
```
luarocks install openssl
```
然后直接根据 https://github.com/zhaozg/lua-openssl/blob/master/test/ec.lua 中的例子来使用：
```
local openssl = require "openssl"
local pkey = require "openssl.pkey"
    
local nec = {'ec',"prime256v1"}
local ec = pkey.new(unpack(nec))
local k1 = pkey.get_public(ec)
print(k1)
```
执行然后发现报错：
```
/usr/local/share/lua/5.1/kong/plugins/init/handler.lua:33: bad argument #2 to 'new' (invalid option 'prime256v1')
```
发现这个是不支持这个算法，但是不对啊，分明已经按照文档上的来了为什么不可以，然后就怀疑，问题出在`local openssl = require "openssl"`这个上面。然后一顿google发现，原因是kong里面也有自带一个类似的项目，叫做 luaossl，然而，他们的包名称都叫做`openssl`，所以其实虽然我以为我引用了的是lua-openssl这个项目，但其实使用的还是kong自带的 luaossl（项目地址：https://github.com/wahern/luaossl） 。但是 luaossl 暂时还没有支持ecc这个比较新的算法，这个功能就用不了。

真相大白，原来是资源包名称相同导致的。详情可点击此链接：https://github.com/zhaozg/lua-openssl/issues/72 。目前暂时还没有很好的解决方案，lua-openssl的作者说联系 luaossl 商量改名的事宜，但暂还没有下文。

那么最后的解决方案就是先在lua中调用系统命令生成ecc相关密钥。



