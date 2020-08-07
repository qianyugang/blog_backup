---
title: 如何在 Kong 和 OpenResty 中使用环境变量 os.getenv()
toc: true
categories: 与代码
tags: 
	- Kong
	- Kong教程
date: 2019-09-02
---

在项目中有时会遇到使用系统环境变量的问题，但是直接使用 `os.getenv()` 是不可行的，不仅是在 Kong 中，在 OpenResty 也都是不可以的，原因是 Kong 是基于 OpenResty ，OpenResty 是基于 Nginx 的，而Nginx在启动的时候，会把环境中所有的环境变量都清除掉，我们可以从Nginx的官方文档中看到这段描述：http://nginx.org/en/docs/ngx_core_module.html#env 。

> By default, nginx removes all environment variables inherited from its parent process except the TZ variable. 

具体可以参考春哥在这个issue下的回复：https://github.com/openresty/lua-nginx-module/issues/601

所以需要在你的nginx.conf文件中把你需要的环境变量声明一下：

```
events{
  ...
}

env PATH;
env USERNAME;
```
然后重启一下Nginx，就可以直接在你的 Lua 代码中使用 `os.getenv("USERNAME")` 获取环境变量。

## 在 Kong 中使用 os.getenv()

那如果我想在kong的插件中是使用`os.getenv()`要怎么操作呢？

在 Kong 里面，我们看到Kong生成的 Nginx配置文件是不可修改的，即使你修改了，Kong 还是会生成恢复成修改前，那么这个时候就要用到 Kong 的一个[自定义 Nginx 配置文件](https://getkong.org/docs/0.13.x/configuration/#custom-nginx-configuration-embedding-kong)功能。详情可以看这俩issue：
- https://github.com/Kong/kong/issues/3473
- https://github.com/Kong/kong/issues/3424

那么实际操作起来就很简单了，新建一个`custom-nginx.conf`文件，里面和上文一样，把你需要使用到的环境变量写到这个配置文件中。
```
events{
  ...
}

env PATH;
env USERNAME;
```
接着重启 Kong 即可。不过这里要注意的是，重启的时候，需要带上一个参数`--nginx-conf`。不过不要搞错的是`-c`参数是kong的配置文件参数，`--nginx-conf` 是自定义Nginx模板配置参数

```
 kong restart -c kong.conf --nginx-conf custom-nginx.conf
```

重启成功之后，就可以在 Kong 的插件中使用代码中使用 `os.getenv("USERNAME")` 来获取环境变量了。







