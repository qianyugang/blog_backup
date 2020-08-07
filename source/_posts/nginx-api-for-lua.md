---
title: Nginx API for Lua 汇总
toc: true
categories: 与代码
tags: 
	- Lua
	- Nginx
date: 2018-12-11
---

- 官方文档[Lua Ngx API](https://openresty-reference.readthedocs.io/en/latest/Lua_Nginx_API/#ngxexec)
- Git hub [lua-nginx-module](https://github.com/openresty/lua-nginx-module#ngxreqfinish_body)

### 常量列表
| 常量  | 说明 | 值 |
| ----| ---- |
| Core constants | 核心常量 |ngx.OK (0) <br> ngx.ERROR (-1)<br>ngx.AGAIN (-2)<br>ngx.DONE (-4)<br>ngx.DECLINED (-5)<br>ngx.nil |
| HTTP method constants | HTTP方法常量 | ngx.HTTP_GET<br />ngx.HTTP_HEAD<br />ngx.HTTP_PUT<br />ngx.HTTP_POST<br />ngx.HTTP_DELETE<br />ngx.HTTP_OPTIONS<br />ngx.HTTP_MKCOL&nbsp;&nbsp;&nbsp;&nbsp;<br />ngx.HTTP_COPY<br />ngx.HTTP_MOVE&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br />ngx.HTTP_PROPFIND<br />ngx.HTTP_PROPPATCH&nbsp;<br />ngx.HTTP_LOCK<br />ngx.HTTP_UNLOCK&nbsp;&nbsp;&nbsp;&nbsp;<br />ngx.HTTP_PATCH<br />ngx.HTTP_TRACE|
| HTTP status constants | HTTP 状态常量 | ngx.HTTP_OK (200)<br />ngx.HTTP_CREATED (201)<br />ngx.HTTP_SPECIAL_RESPONSE (300)<br />ngx.HTTP_MOVED_PERMANENTLY (301)<br />ngx.HTTP_MOVED_TEMPORARILY (302)<br />ngx.HTTP_SEE_OTHER (303)<br />ngx.HTTP_NOT_MODIFIED (304)<br />ngx.HTTP_BAD_REQUEST (400)<br />ngx.HTTP_UNAUTHORIZED (401)<br />ngx.HTTP_FORBIDDEN (403)<br />ngx.HTTP_NOT_FOUND (404)<br />ngx.HTTP_NOT_ALLOWED (405)<br />ngx.HTTP_GONE (410)<br />ngx.HTTP_INTERNAL_SERVER_ERROR (500)<br />ngx.HTTP_METHOD_NOT_IMPLEMENTED (501)<br />ngx.HTTP_SERVICE_UNAVAILABLE (503)<br />ngx.HTTP_GATEWAY_TIMEOUT (504) |
| Nginx log level constants | nginx日志级别常量 | ngx.STDERR<br />ngx.EMERG<br />ngx.ALERT<br />ngx.CRIT<br />ngx.ERR<br />ngx.WARN<br />ngx.NOTICE<br />ngx.INFO<br />ngx.DEBUG<br /> |

### Api一览
| Nginx Api name | 描述 |
| -------------- | ---- |
| ngx.arg | 指令参数，如跟在content_by_lua_file后面的参数 |
| ngx.var.VARIABLE | 变量，ngx.var.VARIABLE引用某个变量 |
| print | 等价于 ngx.log(ngx.NOTICE, ...) |
| ngx.ctx | 这个 Lua 表可以用来存储基于请求的 Lua 环境数据，其生存周期与当前请求相同 (类似 Nginx 变量)。 |
| ngx.location.capture | 发布一个子请求 |
| ngx.location.capture_multi | 发布多个子请求 |
| ngx.status | 响应码 |
| ngx.header.HEADER | 响应头，ngx.header.HEADER引用某个头 |
| ngx.resp.get_headers | 获取响应头 |
| ngx.req.is_internal | 返回一个布尔值，指示当前请求是否是“内部请求”，即从当前nginx服务器内部而不是从客户端侧发起的请求 |
| ngx.req.start_time | 请求的开始时间 |
| ngx.req.http_version | 请求的HTTP版本号 |
| ngx.req.raw_header | 请求头（包括请求行） |
| ngx.req.get_method | 请求方法 |
| ngx.req.set_method | 请求方法重载 |
| ngx.req.set_uri | 请求URL重写 |
| ngx.req.set_uri_args | 通过args参数重写当前请求的URI查询参数。 |
| ngx.req.get_uri_args | 获取请求参数 |
| ngx.req.get_post_args | 获取请求表单 |
| ngx.req.get_headers | 获取请求头 |
| ngx.req.set_header | 将当前请求的名为header_name的请求标头设置为值header_value，覆盖任何现有的 |
| ngx.req.clear_header | 清除当前请求的名为header_name的请求标头。当前请求的现有子请求都不会受到影响，但随后发起的子请求将默认继承该更改。 |
| ngx.req.read_body | 读取请求体 |
| ngx.req.discard_body | 扔掉请求体 |
| ngx.req.get_body_data | 检索内存中的请求正文数据。它返回一个Lua字符串，而不是一个包含所有已解析查询参数的Lua表。如果需要Lua表，请使用ngx.req.get_post_args函数。 |
| ngx.req.get_body_file | 检索文件内请求正文数据的文件名。如果请求正文尚未被读取或已被读入内存，则返回nil。 |
| ngx.req.set_body_data | 使用data参数指定的内存数据设置当前请求的请求主体。 |
| ngx.req.set_body_file | 使用file_name参数指定的文件内数据设置当前请求的请求正文。 |
| ngx.req.init_body | 为当前请求创建一个新的空白请求主体，并通过ngx.req.append_body和ngx.req.finish_body API将缓冲区用于以后的请求正文数据写入。 |
| ngx.req.append_body | 将data_chunk参数指定的新数据块附加到由ngx.req.init_body调用创建的现有请求主体上。|
| ngx.req.finish_body | 完成由ngx.req.init_body和ngx.req.append_body调用创建的新请求正文的构造过程。 |
| ngx.req.socket | 返回包装下游连接的只读cosocket对象。此对象仅支持receive和receiveuntil方法。 |
| ngx.exec | 是否使用args将内部重定向到uri，并且类似于echo-nginx-module的echo_exec指令。 |
| ngx.redirect | 向uri发出HTTP 301或302重定向 |
| ngx.send_headers | 发送响应头 |
| ngx.headers_sent | 响应头是否已发送 |
| ngx.print | 输出响应 |
| ngx.say | 输出响应，自动添加'\n' |
| ngx.log | 输出到error.log |
| ngx.flush | 刷新响应 |
| ngx.exit | 结束请求 |
| ngx.eof | 明确指定响应输出流的结束。在HTTP 1.1分块编码输出的情况下，它只会触发Nginx核心发送“last chunk”。 |
| ngx.sleep | 无阻塞的休眠（使用定时器实现） |
| ngx.escape_uri | 字符串的url编码 |
| ngx.unescape_uri | 字符串url解码 |
| ngx.encode_args | 将table编码为一个参数字符串 |
| ngx.decode_args | 将参数字符串编码为一个table |
| ngx.encode_base64 | 字符串的base64编码 |
| ngx.decode_base64 | 字符串的base64解码 |
| ngx.crc32_short | 字符串的crs32_short哈希 |
| ngx.crc32_long | 	字符串的crs32_long哈希 |
| ngx.hmac_sha1 | 字符串的hmac_sha1哈希 |
| ngx.md5 | 返回16进制MD5 |
| ngx.md5_bin | 返回2进制MD5 |
| ngx.sha1_bin | 返回2进制sha1哈希值 |
| ngx.quote_sql_str | SQL语句转义 |
| ngx.today | 返回当前日期 |
| ngx.time | 返回UNIX时间戳 |
| ngx.now | 返回当前时间 |
| ngx.update_time | 刷新时间后再返回 |
| ngx.localtime | 返回nginx缓存时间的当前时间戳（格式为yyyy-mm-dd hh：mm：ss）（与Lua的os.date函数不同，不涉及系统调用）|
| ngx.utctime | 返回nginx缓存时间的当前时间戳（格式为yyyy-mm-dd hh：mm：ss）（与Lua的os.date函数不同，不涉及系统调用） |
| ngx.cookie_time | 返回的时间可用于cookie值 |
| ngx.http_time | 返回的时间可用于HTTP头 |
| ngx.parse_http_time | 解析HTTP头的时间 |
| ngx.is_subrequest | 当前请求是否是子请求 |
| ngx.re.match | 使用Perl兼容的正则表达式正则表达式与可选选项匹配主题字符串。 |
| ngx.re.find | |
| ngx.re.gmatch | |
| ngx.re.sub | |
| ngx.re.gsub | |
| ngx.shared.DICT | |
| ngx.shared.DICT.get | |
| ngx.shared.DICT.get_stale | |
| ngx.shared.DICT.set | |
| ngx.shared.DICT.safe_set | |
| ngx.shared.DICT.add | |
| ngx.shared.DICT.safe_add | |
| ngx.shared.DICT.replace | |
| ngx.shared.DICT.delete | |
| ngx.shared.DICT.incr | |
| ngx.shared.DICT.lpush | |
| ngx.shared.DICT.rpush | |
| ngx.shared.DICT.lpop | |
| ngx.shared.DICT.rpop | |
| ngx.shared.DICT.llen | |
| ngx.shared.DICT.ttl | |
| ngx.shared.DICT.expire | |
| ngx.shared.DICT.flush_all | |
| ngx.shared.DICT.flush_expired | |
| ngx.shared.DICT.get_keys | |
| ngx.shared.DICT.capacity | |
| ngx.shared.DICT.free_space | |
| ngx.socket.udp | |
| udpsock:setpeername | |
| udpsock:send | |
| udpsock:receive | |
| udpsock:close | |
| udpsock:settimeout | |
| ngx.socket.stream | |
| ngx.socket.tcp | |
| tcpsock:connect | |
| tcpsock:sslhandshake | |
| tcpsock:send | |
| tcpsock:receive | |
| tcpsock:receiveany | |
| tcpsock:receiveuntil | |
| tcpsock:close | |
| tcpsock:settimeout | |
| tcpsock:settimeouts | |
| tcpsock:setoption | |
| tcpsock:setkeepalive | |
| tcpsock:getreusedtimes | |
| ngx.socket.connect | |
| ngx.get_phase | |
| ngx.thread.spawn | |
| ngx.thread.wait | |
| ngx.thread.kill | |
| ngx.on_abort | 注册client断开请求时的回调函数 |
| ngx.timer.at | 注册定时器事件 |
| ngx.timer.every | |
| ngx.timer.running_count | |
| ngx.timer.pending_count | |
| ngx.config.subsystem | |
| ngx.config.debug | 编译时是否有 --with-debug选项 |
| ngx.config.prefix | 编译时的 - -prefix选项 |
| ngx.config.nginx_version | 返回nginx版本号 |
| ngx.config.nginx_configure | 返回编译时 ./configure的命令行选项 |
| ngx.config.ngx_lua_version | 返回ngx_lua模块版本号 |
| ngx.worker.exiting | 当前worker进程是否正在关闭（如reload、shutdown期间） |
| ngx.worker.pid | 返回当前worker进程的pid |
| ngx.worker.count | |
| ngx.worker.id | |
| ngx.semaphore | |
| ngx.balancer | 提供了一个Lua API，允许在纯Lua中定义完全动态的负载均衡器。 |
| ngx.ssl | |
| ngx.ocsp | |
| ndk.set_var.DIRECTIVE | |
| coroutine.create | |
| coroutine.resume | |
| coroutine.yield | |
| coroutine.wrap | |
| coroutine.running | |
| coroutine.status | . |

