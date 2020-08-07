---
title:  在 Lua 中使用 protobuf
toc: true
categories: 与代码
tags: 
	- Lua
	- OpenResty

date: 2019-05-23
---

## 安装protobuf
具体步骤可以参考官方文档https://github.com/protocolbuffers/protobuf/blob/master/src/README.md 

这里只列出一些其中比较的重要的步骤。

先安装一些依赖的库
```
sudo apt-get install autoconf automake libtool curl make g++ unzip
```
然后下载需要的安装包
```
curl -L -o protobuf-all-3.6.1.tar.gz  https://github.com/protocolbuffers/protobuf/releases/download/v3.6.1/protobuf-all-3.6.1.tar.gz
```
接着解压
```
tar -xzvf protobuf-all-3.6.1.tar.gz
```
解压完成后进目录
```
cd protobuf-all-3.6.1
```
执行安装命令  
```
./configure
make
make check
sudo make install
sudo ldconfig
```
最后检查是否安装成功
```
$ protoc --version
libprotoc 3.6.1
```

## 安装lua-protobuf

[lua-protobuf](https://zhuanlan.zhihu.com/p/26014103) 实际上是一个纯C的protobuf协议实现，和对应的Lua绑定。

项目地址：https://github.com/starwing/lua-protobuf

可以使用 luarocks 安装lua-protobuf  
```
luarocks install lua-protobuf
```
如果没有安装 luarocks 可以安装一下luarocks。

## 使用prortobuf

首先，需要定义你自己的标准proto文件 xxx.proto 文件（可能不止一个文件，不止一个目录），定义好了之后，使用protoc生成pb文件，命令如下：
```
protoc --proto_path=proto --descriptor_set_out=common.pb proto/xxxx/*.proto proto/xxxxx/*.proto
```

这里需要注意的是，有时候会有多个proto文件并且多目录import的情况，这个时候，就需要在参数中都体现出来（命令的最后两个参数），这条命令是把所有的 .proto 文件生成了一个 common.pb 文件方便引入。

最后就可以在lua代码中如下使用
```
-- 引入pb库
local pb = require "pb"

-- load pb文件
assert(pb.loadfile "common.pb" )

-- lua table data
local data = {
    aaa = 123456,
    bbb = "bbbbb"
}

-- 把一个lua table数据encode成二进制文件
local bytes = assert(pb.encode("dd01.account", data))
print(pb.tohex(bytes))

-- 把一个二进制文件decode为lua table
local data2 = assert(pb.decode("dd01.account", bytes))
print(cjson_encode(data2))
```

如果能看到正常打印出来，说明就成功了。

## 参考链接

- [lua-protobuf 使用说明](https://zhuanlan.zhihu.com/p/26014103)
- [新的lua-protobuf发布啦](https://zhuanlan.zhihu.com/p/33379595)
- [Proto Buffers in Lua - 云风的blog](https://blog.codingnow.com/2010/08/proto_buffers_in_lua.html)



