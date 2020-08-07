---
title: Lua OpenResty 使用rabbitmq AMQP协议发送和接收消息
toc: true
categories: 与代码
tags: 
	- Lua
	- OpenResty

date: 2019-05-30
---

如果想要在openresty中使用AMQP协议发送和接收消息的话，需要使用到一个第三方库。地址为：https://github.com/mengz0/amqp 。
当然，首先你得在自己的环境中安装一个rabbitmq，或者使用远程的rabbitmq也可以。
openresty也是要安装一下的，这里就不啰嗦了。

## 第一步：安装第三方库

```
luarocks install amqp
```

如果没有安装 luarocks 可以安装一下luarocks。

## 第二步：使用amqp发送&接收消息

可以直接参考https://github.com/mengz0/amqp 中的例子，但是这个库的例子写的非常简略，导致很多参数需要你自己去源码中查看，我在这个例子中列举了一些常用的参数，可以参考使用一下。

### 发送消息
```
local function send_pb(messages)
	local amqp        = require "amqp"

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

### 接收消息&消费队列

可以新建一个`consume_queue.lua`文件，然后如下代码：
```
local function consume_local(body)
	print(body) -- 这里就是消息的主体
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
    callback    = consume_local,
    durable     = true,
    passive     = false,
    type        = "topic"

})

ctx:connect("127.0.0.1", port)

local ok, err = ctx:consume()
```

然后使用命令`/usr/local/openresty/bin/resty  consume_queue.lua`，就可以看到发送的消息了。

### 发现问题

在使用这个库的时候，发现了一点小问题，就是在消费这个队列的时候，发现诸如`passive`、`auto_delete`、`durable`等等参数设置的全都无效了，但是感觉自己的使用姿势没有问题啊，按照 https://github.com/mengz0/amqp#typical-use-cases 给的例子来也没问题。随后我就查看并断点了一下源码的https://github.com/mengz0/amqp/blob/master/amqp.lua 文件，发现在执行到 664行的`
function amqp:queue_declare(opts)` 函数和 687行的`function amqp:queue_bind(opts)`函数的时候，`opts`参数根本没有传进来。

### 解决问题

所以要把`function amqp:queue_declare(opts)`函数中的`opts.`改为`self.opts.`，如下：
```
f.method = {
      queue = opts.queue or self.opts.queue,
      passive = opts.passive or false,
      durable = opts.durable or false,
      exclusive = opts.exclusive or false,
      auto_delete = opts.auto_delete or true,
      no_wait = self.opts.no_wait or true
   }
```
修改为

```
f.method = {
      queue = opts.queue or self.opts.queue,
      passive = self.opts.passive or false,
      durable = self.opts.durable or false,
      exclusive = self.opts.exclusive or false,
      auto_delete = self.opts.auto_delete or true,
      no_wait = self.opts.no_wait or true
   }
```

`function amqp:queue_bind(opts)`函数也同理，这里就不再写一次了，修改完了之后，传进去的参数都生效了。

打完收工。














