---
title: Kong初探 (插件开发以及使用)
toc: true
categories: 与代码
tags: 
	- api网关
	- Kong
	- Kong教程
date: 2018-11-17
---

## Kong已有插件介绍
Kong Plugins中列出了已有的所有插件，有些插件只能在企业版使用，有些插件是社区成员开发的，大部分是Kong公司开发，并集成到社区版中。下面是社区版集成的、Kong公司维护的插件(2018-09-30 14:33:03)：

**认证插件：**
- Basic Auth
- HMAC Auth
- JWT Auth
- Key Auth
- LDAP Auth
- OAuth 2.0 Auth

**安全插件：**
- Bot Detection (机器人爬虫检测)
- CORS (跨域请求)
- IP Restriction (IP限制)

**流控插件：**
- ACL (访问控制）
- Rate Limiting （请求速率限制）
- Request Size Limiting（返请求大小限制）
- Request Termination （请求终止）
- Response Rate Limiting (返回限速)

**微服务插件：**
- AWS Lambda（无服务器计算平台）
- Azure Functions（一个无服务器计算服务，使用它可以按需运行代码，而无需显式预配或管理基础结构。）
- Apache OpenWhisk（开源的、事件驱动的计算平台）
- Serverless Functions（无服务器架构）

**分析和监控插件：**
- Datadog（记录API Metric如请求次数、请求大小、响应状态和延迟，可视化API Metric）
- Prometheus（监控报警平台）
- Zipkin（一款开源的分布式实时数据追踪系统。其主要功能是聚集来自各个异构系统的实时监控数据，用来追踪微服务架构下的系统延时问题）

**内容修改插件：**
- Correlation ID(使用http头的唯一id传输来关联请求和返回）
- Request Transformer（在转发到upstream之前修改请求）
- Response Transformer（在upstream响应返回给客户端之前修改响应）

**日志插件：**
- File Log
- HTTP Log
- Loggly
- StatsD
- Syslog
- TCP Log
- UDP Log

## 开发一个自己的插件
**官方插件开发指南 **

文档：https://docs.konghq.com/0.14.x/plugin-development/

首先查看第一篇文章，保证你已经成功的安装以及启动了kong的服务，接下来第一件事情，就是要找到你的kong插件所在的位置
一般会在如下文件中，此文件夹就存放的是所有kong的插件位置，当然，你要写的插件也在这里
```
/usr/local/share/lua/5.1/kong/plugins/
```
在此文件下新建你的插件文件夹
```
touch /usr/local/share/lua/5.1/kong/plugins/testplugin
```
然后新建两个必须的文件，这两个文件是一个插件最基本的组成部分，其中，handler.lua 是插件核心，它是一个接口实现，其中每个函数将在请求生命周期中的期望时刻运行。schema.lua 用于定义插件配置
```
handler.lua  schema.lua
```

### 插件配置
Kong 插件通过schema.lua文件定义配置。schema.lua 返回一个Table类型，包含no_consumer、fields、self_check三个属性：

| 属性名 | Lua 类型 |	默认值 |	描述 |
| ------ | ------ | ------ |
| no_consumer |	Boolean |	false |	如果为true将不能应用此插件至指定消费者，只能被应用到 Services 或者 Routes, 例如：认证插件
| fields |	Table |	{} | 插件的 schema，使用一个键值对定义可用属性和他们的规则
| self_check |	Function |	nil |	如果在接受插件配置之前需要进行自定义验证，需要实现此函数

schema.lua 文件样本如下：
```
return {
  no_consumer = true, -- this plugin will only be applied to Services or Routes, 此插件是否用于所有用户
  fields = {
    -- Describe your plugin's configuration's schema here. 描述你的配置文件
  },
  self_check = function(schema, plugin_t, dao, is_updating)
    -- perform any custom verification 执行任何自定义的验证
    return true
  end
}
```
### 逻辑实现

Kong 插件可以在请求/响应生命周期中的几个入口点注入自定义逻辑，插件实现者必须在 handler.lua 中实现 base_plugin.lua 文件中的一个或多个接口。 base_plugin.lua 文件中的几个方法如下：

| 函数名 | LUA-NGINX-MODULE Context | 描述 |
| ------ | ------ | ------ |
| :init_worker() | init_worker_by_lua | 在每个 Nginx 工作进程启动时执行 |
| :certificate() | ssl_certificate_by_lua | 在SSL握手阶段的SSL证书服务阶段执行 |
| :rewrite() | rewrite_by_lua | 从客户端接收作为重写阶段处理程序的每个请求执行。在这个阶段，无论是API还是消费者都没有被识别，因此这个处理器只在插件被配置为全局插件时执行 |
| :access() | access_by_lua | 为客户的每一个请求而执行，并在它被代理到上游服务之前执行 |
| :header_filter() | header_filter_by_lua | 从上游服务接收到所有响应头字节时执行 |
| :body_filter() | 	body_filter_by_lua | 从上游服务接收的响应体的每个块时执行。由于响应流回客户端，它可以超过缓冲区大小，因此，如果响应较大，该方法可以被多次调用 |
| :log() | 	log_by_lua | 当最后一个响应字节已经发送到客户端时执行 |

接下来找到你的conf文件https://docs.konghq.com/0.14.x/plugin-development/
```
vim /etc/kong/kong.conf
```
找到其中这一行，去掉开头的注释并修改如下
```
plugins = bundled, testplugin
```
在testplugin目录执行如下命令
```
luarocks write_rockspec
```
会在本目录下生成一个 `testplugin-scm-1.rockspec` 文件  

所以，一个最基本款的插件的结构将会是这样
```
testplugin/
├── handler.lua
├── schema.lua
└── testplugin-scm-1.rockspec
```
接着重启kong
```
kong stop
kong start -c /etc/kong/kong.conf
```
再到 [http://localhost:8080](http://localhost:8080) 控制面板的插件列表中就能看到你的插件了。

### 日志

日志路径：

```
/usr/local/kong/logs/error.log
```

当然也可以在 /etc/kong/kong.conf  中找到配置日志路径修改即可

## 参考链接
- [Kong 插件开发指南](https://ms2008.github.io/2018/06/19/kong-plugin-development/)
- [Kong 系列教程](https://www.lijiaocn.com/tags/class.html)
- [开源API网关系统（Kong教程）入门到精通](https://www.jianshu.com/p/a68e45bcadb6)
- [Kong Api网关简介(二) 插件](http://www.yuxiumin.com/2018/07/09/kong-api-gateway-plugin/)
- [Writing a Plugin for Kong API Gateway 0.14.x](https://www.jerney.io/header-echo-kong-plugin/)
- [Kong源码分析](http://cyukang.com/2017/07/02/kong-intro.html)
- [Kong源码解读](http://techblog.ppdai.com/2018/04/16/20180416/)


