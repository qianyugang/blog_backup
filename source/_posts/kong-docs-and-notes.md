---
title: Kong 开发笔记&相关文章
toc: true
categories: 与代码
tags: 
	- Kong
	-	Kong教程
	- api网关
date: 2018-12-19
---

在学习和开发 Nginx & OpenResty & Lua % Kong 的途中，遇到的一些教程或者文章，还有一些注意事项，在这里集中分享一下，此文章会不定期更新，如果有好的有关的教程或者文章，欢迎留言分享。
如果有遇到问题，不妨可以先查看一下下面的文章，或许会能找到一些答案。

## 官方必看

- [Kong 官网开发文档](https://docs.konghq.com/)
- [Kong 官方插件开发文档](https://docs.konghq.com/0.14.x/plugin-development/)
- [OpenResty Lua Nginx Module](https://www.nginx.com/resources/wiki/modules/lua/)
- [OpenResty Lua Nginx Module github](https://github.com/openresty/lua-nginx-module)

## 源码分析

- [kong源码导读](https://jinfei21.github.io/2018/04/16/20180416/)
- [Kong源码分析: 启动](http://cyukang.com/2017/07/07/kong-intro-2.html)
- [Kong 插件加载机制源码解析](https://ms2008.github.io/2018/05/18/kong-plugin-load-source/)
- [kong流程学习](https://www.cnblogs.com/mentalidade/p/6863677.html)

## 插件使用和开发相关

- [使用JWT(Json Web Token)实现登录认证](https://segmentfault.com/a/1190000013841289#articleHeader2)
- [API网关Kong（四）：功能梳理和插件使用-认证插件使用](https://my.oschina.net/u/3895037/blog/2878475)
- [kong使用jwt 一个简单的例子](https://zhuanlan.zhihu.com/p/35913698)
- [Kong 插件开发指南](https://ms2008.github.io/2018/06/19/kong-plugin-development/)
- [Kong插件开发](https://www.jianshu.com/p/fa0fd22e0ca8)
- [lua-发送http请求](https://anbolihua.iteye.com/blog/2316423)

## Nginx & OpenResty & Lua 

- [Nginx、OpenResty、Lua与Kong](https://www.lijiaocn.com/nginx/)
- [agentzh 的 Nginx 教程（版本 2016.07.21）](https://openresty.org/download/agentzh-nginx-tutorials-zhcn.html)
- [OpenResty 最佳实践](https://moonbingbing.gitbooks.io/openresty-best-practices/openresty/response.html)
- [Lua程序设计 编译：中国lua开发者](http://book.luaer.cn/)
- [Lua顺序 执行顺序](https://www.cnblogs.com/archoncap/p/4960221.html)
- [Nginx与Lua的执行顺序和步骤说明](http://www.10tiao.com/html/357/201605/2247483796/1.html)
- [跟我学 Nginx+Lua 开发 极客学院](http://wiki.jikexueyuan.com/project/nginx-lua/)
- [使用Nginx+Lua(OpenResty)开发高性能Web应用](https://www.jianshu.com/p/4a13272e6171)
- [分析openresty redis的长连接问题](http://xiaorui.cc/2017/08/26/%E5%88%86%E6%9E%90openresty-redis%E7%9A%84%E9%95%BF%E8%BF%9E%E6%8E%A5%E9%97%AE%E9%A2%98/)
- [给API接口增加Nginx+lua签名认证](https://www.04007.cn/article/309.html)
- [lua中实现签名验证的脚本,lua中利用ngx实现md5加密](https://www.04007.cn/article/428.html)
- [Openresty 学习笔记 系列](https://www.cnblogs.com/tinywan/category/888115.html)
- [基于 lua-resty-redis 的 nginx 中 lua-redis 的连接模块封装](https://juejin.im/entry/5909821661ff4b0066f067ac)
- [nginx lua api解读](https://segmentfault.com/a/1190000012734809)
- [Lua 的模块安装和部署工具 - LuaRocks](https://blog.csdn.net/xu_ya_fei/article/details/46276609)

## 其他
- [使用API网关Kong配置反向代理和负载均衡](https://zacksleo.github.io/2018/07/06/%E4%BD%BF%E7%94%A8API%E7%BD%91%E5%85%B3Kong%E9%85%8D%E7%BD%AE%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86%E5%92%8C%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1/)
- [浅谈网关 kong 中的服务(service)与路由(route)](https://zhuanlan.zhihu.com/p/35521440)
- [** API网关Kong学习笔记 系列** ](https://www.lijiaocn.com/tags/class.html)
- [微服务网关Kong实践](http://ljchen.net/2018/08/11/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E7%BD%91%E5%85%B3Kong%E5%AE%9E%E8%B7%B5/)
- [拍拍贷基础框架团队博客](http://techblog.ppdai.com/2018/04/16/20180416/)
- [Apigateway-kong 系列](https://www.cnblogs.com/zhoujie/tag/apigateway/)

## 手札

- 修改了配置之后需要重启才生效
- 修改了配置之后,已经加入的需要删除重新加入才生效
- 在Lua中将`false`与`nil`视为假，其他都为真。

