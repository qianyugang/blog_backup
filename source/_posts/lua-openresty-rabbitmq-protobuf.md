---
title: Lua OpenResty 使用 protobuf 和 rabbitmq AMQP 发送和接收消息
toc: true
categories: 与代码
tags: 
	- OpenResty

date: 2019-06-26
---

项目中有个需求，需要使用Google的protobuf作为压缩协议，然后使用rabbitmq AMQP来发送和接收消息，在研究使用这两个工具中，遇到了有一些坑，之前有写了两篇来介绍分别使用，《 [在 lua 中使用 protobuf](http://www.102no.com/archives/1570)》和《[Lua OpenResty 使用rabbitmq AMQP协议发送和接收消息](http://www.102no.com/archives/1574)》 ，这里我们来结合使用一下，然后顺便解决一下lua的相关库的使用问题。

## protobuf 相关步骤 

### 1.安装  protoc

首先安装依赖库
```
sudo apt-get install autoconf automake libtool curl make g++ unzip
```

下载、解压安装包
```
curl -L -o protobuf-all-3.6.1.tar.gz  https://github.com/protocolbuffers/protobuf/releases/download/v3.6.1/protobuf-all-3.6.1.tar.gz
tar -xzvf protobuf-all-3.6.1.tar.gz
```

进入安装包安装
```
cd protobuf-all-3.6.1
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

### 2.生成pb文件

需要定义你自己的标准proto文件 `xxx.proto` 文件（可能不止一个文件，不止一个目录），定义好了之后，使用`protoc`命令生成`.pb`文件，命令如下：
```
protoc --proto_path=proto --descriptor_set_out=common.pb proto/xxxx/*.proto proto/xxxxx/*.proto
```
其中 `--proto_path` 参数是你的proto文件目录，`--descriptor_set_out` 是你需要输出`.pb`文件的目录，后面几个参数就是具体要引入的`.proto`文件。这里需要注意的是，有时候会有多个proto文件并且多目录import的情况，这个时候，就需要在参数中都体现出来（命令的最后两个参数），这条命令是把所有的 `.proto` 文件生成了一个 `common.pb` 文件方便引入。注意这个`common.pb`的路径，后面需要用到。

### 3.安装lua的protobuf库

这里我们使用的是lua-protobuf库。 lua-protobuf实际上是一个纯C的protobuf协议实现，和对应的Lua绑定。

项目地址：https://github.com/starwing/lua-protobuf

可以使用 luarocks 安装lua-protobuf

```
luarocks install lua-protobuf
```

如果没有安装 luarocks 可以安装一下luarocks。

## rabbitmq AMQP 相关步骤

### 1.安装第三方库

如果想要在openresty中使用AMQP协议发送和接收消息的话，需要使用到一个第三方库。地址为：https://github.com/4mig4/lua-amqp 。这里你会发现，这个库和《[Lua OpenResty 使用rabbitmq AMQP协议发送和接收消息](http://www.102no.com/archives/1574)》文章中介绍使用的库不一样，确实是的，因为在使用过程中我发现，之前的那个库还有一些功能不完善，这里使用的这个库是作者fork了之前的那个项目，并改进了里面许多功能。所以最终我选用了这个库来开发。

安装方式为，使用luarocks安装，由于这个库没有push到https://luarocks.org/ 仓库当中，所以我们需要使用到其中的声明文件安装。把项目clone下来之后，找到这个文件 amqp-1.0-4.rockspec 这个文件，然后执行：
```
luarocks build amqp-1.0-4.rockspec
```
这里直接执行，可能会报错，报错信息为`unrecognized filename extension`，意思是识别不了文件格式之类的，这里需要把这个文件中的
```
source = {
   url = "https://github.com/4mig4/lua-amqp.git",
   tag = "",
}
```
修改为：
```
source = {
   url = "https://github.com/4mig4/lua-amqp",
   tag = "",
}
```

然后继续执行，执行完成之后，就可以在`/usr/local/share/lua/5.1`目录中看到这个第三方库了，如果整理的文件夹名字不是`amqp`，而是一个amqp加版本号，可以手动直接把文件夹修改为`amqp`即可。到此第三方库安装完成。

在使用过程中，我发现一个问题，作者在fork了项目之后，增加了CQUEUES, NGX.SOCKET, SOCKET一些通讯协议功能，但是我本地环境直接运行会报错，查看了一些源码，发现是我的本地环境的`cqueues`支持有点问题，而作者把这个作为最优先的协议，那么我就把源码文件https://github.com/4mig4/lua-amqp/blob/master/amqp/init.lua 中的`local use_cqueues = true`改为`local use_cqueues = false`，即关闭cqueues来使用了。

## 发送和接收消息

假设你的proto结构体是这样的：
```
syntax = "proto3";
package aa.bb;
message EventEnvelope {
  string id = 1; 
  int64 created_ts = 2; 
  string server_hash = 3;
  int64 happened_ts = 4; 
  oneof body {
    aa.bb.LoggedOut logged_out = 1501;
  }
message LoggedOut {
  int64 union_id = 1;
  string app_key = 2;
}
```

### 发送消息

```
local function send_pb()
	
    -- 引入pb库
	local pb   = require "pb"
	-- 加载pb文件
    assert(pb.loadfile("xxxxx/common.pb"))

	-- 这里是你需要发送的pb消息，注意这里是支持嵌套的，如果你的protobuf文件里面有one of，可以直接使用多层嵌套
    -- 这里就根据你自己protobuf结构来就好
    local data    = {
        created_ts  = ngx.now() * 1000,
        server_hash = 'localhost',
        happened_ts = ngx.now() * 1000,
        id          = 8376548368364,
        logged_out  = {
            union_id = 123455666,
            app_key  = xxxxxxxx
        }
    }
    
    -- 生成需要发送的消息，这个消息是proto压缩之后的，这个`EventEnvelope` 就是最外层的结构体
	local messages = assert(pb.encode("aa.bb.EventEnvelope", data))

	-- 引入amqp第三方库
    local amqp        = require "amqp"

	-- 里面的一些参数就不再赘述了，都是rabbitmq的一些参数
    local ctx = amqp.new({
        role        = "publisher",
        exchange    = "exchangexxxx",
        ssl         = false,
        user        = "guest",
        password    = "guest",
        auto_delete = false,
        routing_key = "routing_keyxxxxx",
        passive     = true,
        no_ack      = true,
        no_wait     = false,
    })
    ctx:connect("127.0.0.1", port)
    ctx:setup()
    local ok, err = ctx:publish(messages)
    if not ok then
        ngx.log(ngx.ERR, "[ -- rabbitmq send failed : -- ] " .. err)
    else
        ngx.log(ngx.ERR, "[ -- rabbitmq send success ]")
    end
end

send_pb("this is a message")
```

### 接收消息

可以新建一个`consume_queue.lua`文件，然后如下代码：
```

-- 依旧是引入
local amqp            = require "amqp"
local pb              = require "pb"

-- 这里是回调函数
local function consume_local(f)
    print(f) -- 这里就是消息的所有信息，里面包含了properties ，body，frame等信息 
    print(f.body) -- 这里就是消息的主体
    
   	-- 加载pb文件
    assert(pb.loadfile("xxxxx/common.pb"))
    -- 解析消息
    local data = assert(pb.decode("aa.bb.EventEnvelope", f.body))
	print(data) 
    
end

local amqp        = require "amqp"

local ctx = amqp.new({
    role        = "consumer",
    queue       = "eventbus1", -- 这里可以自定义
    exchange    = "exchangexxxx",
    ssl         = false,
    user        = "guest",
    password    = "guest",
    no_wait     = false,
    routing_key = "routing_keyxxxxx",
    auto_delete = false, -- 是否自动删除消息
    no_ack      = true,
    exclusive   = false, -- 是否为排他队列
    callback    = consume_local, -- 回调函数
    durable     = true,
    passive     = false,
    type        = "topic"

})

ctx:connect("127.0.0.1", port)

local ok, err = ctx:consume()
```

执行命令开始消费消息
```
/usr/local/openresty/bin/resty consume_queue.lua
```

















