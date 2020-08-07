---
title: 初探Api Gateway Tyk
toc: true
categories: 与代码
tags: 
	- api网关
	- Tyk
date: 2018-11-15
---

## 说明
** 项目官网：** https://tyk.io/

** 项目地址：** 

- https://github.com/TykTechnologies/tyk
- https://github.com/PGBI/kong-dashboard

** 项目文档：** https://tyk.io/docs/

## 特点

使用go语言开发，启动之前需要使用redis和mongodb，tyk使用这两个数据做了依赖，Tyk 功能比较全，包括鉴权、监控、负载均衡，用户管理、域名管理、策略等。

**Tyk不足** 

Tyk只能支持HTTP REST API，不支持SOAP或者RPC等其他服务。需要用插件支持   
这些功能不一定能满足我们自己的业务需求，在其基础上修改，比较困难。  
存在商业付费版，安装license收费、免费版。（免费版的也需要一个license，一个节点一年）  
dashboard 需要购买

## 评估指标

**官方性能测试：**  

- 所有这些都在一个2核，2GB， 虚拟服务器上。
- Tyk可以轻松地每秒代理3000个请求，延迟低于65ms，性能曲线相当平坦。
- 在负载下，执行全密钥验证、安全检查、配额管理和分析收集，Tyk每秒可以处理大约2000个请求，延迟低于85ms。

** 并发性能，稳定性：**

较为正常请求情况下

| 请求总数 | 并发数 | 总用时 | 每秒完成请求数 | 用户平均请求等待时间 | 服务器平均请求等待时间 |
| ------ | ------ | ------ | ------ |  ------ |  ------ | 
| 5000 | 500 | 16.650 seconds | 300.31 |  1664.958 ms | 3.330 | 
| 10000 | 500 | 34.037 seconds | 293.80 |  1701.856 | 3.404 | 
| 15000 | 500 | 57.850 seconds | 259.29 |1928.319  | 3.857 | 

sleep 2 sec 请求情况下

| 请求总数 | 并发数 | 总用时 | 每秒完成请求数 | 用户平均请求等待时间 | 服务器平均请求等待时间 |
| ------ | ------ | ------ | ------ |  ------ |  ------ | 
| 500 | 50 | 59.483 seconds |  8.41  |  5948.322 ms | 118.966  | 
| 1000 | 100 | 110.049 seconds | 9.09 | 11004.902 | 110.049 | 
| 3000 | 100 | 311.065 seconds | 9.64 | 10368.826 | 103.688 | 

** 日志功能：**

控制面板有直观查看log以及统计

**插件/二次开发开发**

lua、Python、JS 统统支持，另外 插件里面的 gRPC 也是支持的；

**控制面板**

http://localhost:3000/

## 社区声音

- 客户支持快；
- Kong 的插件只能用 lua 写，Tyk 则不然，lua、Python、JS 统统支持，另外 插件里面的 gRPC 也是支持的；
- Kong 依赖于 Nginx，假如 Nginx 出了 bug，他们很难修复，Tyk 则不然，全部技术栈都是他们可控的；
- 两者的性能差不多，2k qps 完全不是问题，能满足大部分需求；
- Kong 的 UI 是企业版才有的，Tyk 免费用；
- 我们不靠 VC 支持，玩的是长线战略；

## 参考连接
- [Tyk API网关介绍及安装说明](https://www.bbsmax.com/A/amd0MmQ6zg/)
- [深入浅出聊聊企业级API网关](https://sdk.cn/news/7023)
- [开源API网关Tyk与Kong](http://www.damonyi.cc/%E5%BC%80%E6%BA%90api%E7%BD%91%E5%85%B3tyk%E4%B8%8Ekong/)
- [API Gateways: Kong vs. Tyk](https://www.bbva.com/en/api-gateways-kong-vs-tyk/)

