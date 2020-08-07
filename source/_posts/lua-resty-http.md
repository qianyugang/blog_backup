---
title: Openresty 第三方库 Lua-resty-http 使用教程
toc: true
categories: 与代码
tags: 
	- Lua
	- OpenResty

date: 2018-12-21
---

lua_resty_http是一个第三方 openresty 库，基于 Openresty/ngx_lua 的HTTP客户端，支持POST方法上传数据.  
刚好项目中用到需要从网关中发起请求，于是就用到这个库，把使用方式在这里分享一下。

## 安装第三方库lua_resty_http

### 第一步
首先找到项目地址:[https://github.com/pintsized/lua-resty-http](https://github.com/pintsized/lua-resty-http)

### 第二步
然后将 `lua-resty-http/lib/resty/` 目录下的 `http.lua` 和 `http_headers.lua` 两个文件拷贝到 `/usr/local/openresty/lualib/resty` 目录下即可， `OpenResty` 安装目录为 `/usr/local/openresty`）。不需要重启。（少数需要清空 Openresty shared_dict 数据的情况需要重启 ）。


## 代码示例

```
local function HttpUtil(jwt_data)
	-- 引入第三方库
    local http = require "resty.http"
    local httpc = http.new()
    -- 需要post的参数
    local str = "aaa=1&bbb=2%ccc=3"
    -- 请求地址
    local url = "http://wwww.xxxxxxx.com"
    local res, err = httpc:request_uri(url, {
        method = "POST",
        body = str,
        headers = {
            ["Content-Type"] = "application/x-www-form-urlencoded",
            ["Content-Length"] = #str,
        }
    })

	-- 返回值
    ngx.say(res.body)
    ngx.say(res.status)
    ngx.say(res.headers)
    
    return res.body,res.status
end
```

## 参数详解

### 请求参数

| 参数名 | 描述 |
| ------ | -----|
| version | HTTP版本号，目前支持1.0或1.1 |
| method | HTTP方法 |
| path | 路径 | 
| query | 查询字符串，表示为文字字符串或Lua table | 
| headers | 请求 header table |
| body | 请求体作为字符串或迭代器函数（请参阅[get_client_body_reader](https://github.com/pintsized/lua-resty-http#get_client_body_reader)）。|
| ssl_verify | 验证SSL证书是否与主机名匹配 |

### 返回参数

请求成功后，res将包含以下字段：

| 参数名 | 描述 | 
| ------ | ---- |
| status | 状态码 |
| reason | 状态原因短语 |
| headers | headers 类型是table ，相同名称的header会合并成一个table |
| has_body | bool值，指示是否有要读取的正文 |
| body_reader | 迭代器函数，用于以流方式读取正文。 |
| read_body | 一种将整个主体读入字符串的方法。 | 
| read_trailers | 读取正文后，合并标题下trailers的方法。 |

## 参考链接
- [如何引用第三方 resty 库](https://moonbingbing.gitbooks.io/openresty-best-practices/ngx_lua/how_use_third_lib.html)
- [Lua-发送http请求](https://moonbingbing.gitbooks.io/openresty-best-practices/ngx_lua/how_use_third_lib.html)
- [Lua-resty-http上传数据](https://my.oschina.net/u/2539854/blog/848059)
- [Nginx Lua HTTP客户端](https://www.haiyun.me/archives/932.html)
