---
title:  微服务 API 网关 Kong 插件开发添加自定义配置文件
toc: true
categories: 与代码
tags: 
	- Kong
	- Kong中文文档
	- Kong插件开发
date: 2019-02-19
---

由于在 Kong 的插件开发中，需要添加一些自定义的配置文件，而且是一些插件公用的配置，但是又不方便都写在插件的 `schema.lua` 中，那么就考虑引入常规的配置文件，这里以`.env`文件为例，写一下添加和使用过程。
首先需要了解的是，Kong 的插件使用了一个叫 Classic 的 class 机制。所有的插件都是从 `base_plugin.lua` 基类上继承而来。`base_plugin.lua` 定义了插件在各个阶段被执行的方法名：，所以我们就从这里入手，以添加redis配置信息为例。

## 添加配置文件
进入到插件目录，我这里是`/usr/local/share/lua/5.1/kong/plugins`，然后新建一个`.env`文件，写入：
```
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=
REDIS_PORT=6379
```

## 添加获取配置文件内容方法
然后编辑`base_plugin.lua`，（需要注意的是，这里可能会有权限问题），在

```
return BasePlugin
```
这一行上加上获取`.env`的方法

```
-- 获取配置文件
function BasePlugin:load_ini()

    if config_data then
        return config_data
    end

    config_data = {}

    local info = debug.getinfo(1, "S")
    local path = info.source
    path = string.sub(path, 2, -1) -- 去掉开头的"@"
    path = string.match(path, "^.*/") -- 捕获最后一个 "/" 之前的部分 就是我们最终要的目录部分

    local conf, err = resty_ini.parse_file(path .. ".env")

    if not conf then
        ngx_log( ngx.ERR, "[ -- can not find file .env -- ]" .. tostring(err))
        return
    end

    for section, values in pairs(conf) do
        for k, v in pairs(values) do
            config_data[k] = v
        end
    end

    return config_data
end
```

把配置文件中的内容以键值对的形式放在一个名为`config_data`的table中。

## 使用配置内容

在`base_plugin.lua`里加好了之后，回到自己的自定义插件开发中。获取配置文件内容如下：

```
local ini_conf = BasePlugin:load_ini()
if not ini_conf then
    ngx.log(ngx.ERR, "[ -- redis-log -- ] failed to read .env ", err)
end
```
使用配置文件的内容来链接并验证redis

```
function connectme()

    local red = redis:new()

    red:set_timeout(1000)

    if not ini_conf then
        ngx_log(ngx.ERR, "[ -- redis-log -- ] failed to read .env ", err)
    end

    local redis_host = ini_conf['REDIS_HOST']
    local redis_port = ini_conf['REDIS_PORT']
    local redis_password = ini_conf['REDIS_PASSWORD']

    local ok, err = red:connect(redis_host, redis_port)

    if not ok then
        ngx_log(ngx.ERR, "[ -- redis-log -- ] failed to connect to Redis: ", err)
        return
    end

    if redis_password then
        local ok, err = red:auth(redis_password)
        if not ok then
            ngx_log(ngx.ERR, "[ -- redis-log -- ] failed to auth to Redis: ", err)
            return
        end
    end

    return red
end
```

最后需要说明的是，每次修改了配置文件，需要重新加载一下整个插件，也就是可能需要重新启动一下kong。  
如果有更好更更合理的方法，欢迎留言。




