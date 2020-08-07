---
title: 基于OpenResty 的 WEB 框架 Lor 安装初探
toc: true
categories: 与代码
tags: 
	- OpenResty
	- Lua

date: 2019-07-16
---


- **项目介绍：**
    - Lor是一个运行在OpenResty上的基于Lua编写的Web框架.
    - 路由采用[Sinatra](http://www.sinatrarb.com/)风格，Sinatra是Ruby小而精的web框架.
    - API基本采用了[Express](http://expressjs.com/)的思路和设计，Node.js跨界开发者可以很快上手.
    - 支持插件(middleware)，路由可分组，路由匹配支持string/正则模式.
    - lor以后会保持核心足够精简，扩展功能依赖`middleware`来实现. lor本身也是基于`middleware`构建的.
    - 推荐使用lor作为HTTP API Server，lor也已支持session/cookie/html template等功能.
    - 框架简单示例项目[lor-example](https://github.com/lorlabs/lor-example)
    - 框架全站示例项目[openresty-china](https://github.com/sumory/openresty-china)

- **项目地址：** https://github.com/sumory/lor
- **文档地址：** http://lor.sumory.com

## 安装lor框架

```
git clone https://github.com/sumory/lor
cd lor 
make install
```
此时可能会有报错如下：
```
/usr/bin/env: "resty": 没有那个文件或目录
```
原因为lor没有找到resty的执行目录，这个时候只需要找到resty的执行目录并软链过去即可：
```
sudo ln -s /usr/local/openresty/bin/resty /usr/bin/resty
```
框架自带一个示例项目，运行一下代码
```
lord new lor_demo
```

## 启动
然后就能在框架中看到lor_demo文件夹，执行如下命令
```
cd lor_demo
lord start
```

打开浏览器，输入`http://localhost:8888/`正常访问，即正常安装。

