---
title: 初探Api Gateway Kong
toc: true
categories: 与代码
tags: 
	- api网关
	- Kong
	- Kong中文教程
date: 2018-11-14
---

## 说明
** 项目官网：** https://konghq.com/

** 项目地址：** 

- https://github.com/Kong/kong/
- https://github.com/PGBI/kong-dashboard

** 项目文档：** https://docs.konghq.com/

## 特点

Kong是一款基于Nginx_Lua模块写的高可用，易扩展由Mashape公司开源的API Gateway项目。由于Kong是基于Nginx的，所以可以水平扩展多个Kong服务器，通过前置的负载均衡配置把请求均匀地分发到各个Server，来应对大批量的网络请求。

实际上 Kong 出于性能上的考虑，并不会将每次请求都去查询数据库，而是将数据库中的实体缓存在了自身的多级缓存中。

**不足**

- 目前无法进行负载均衡，需要开发插件
- 后期根据业务需求，进行扩展，不好掌握，例如增加服务发现、负载均衡等

**Kong主要有三个组件：**  

- Kong Server ：基于nginx的服务器，用来接收API请求。
- Apache Cassandra/PostgreSQL ：用来存储操作数据。
- Kong dashboard：官方推荐UI管理工具，当然，也可以使用 restfull 方式 管理admin api。

## 评估指标

**官方性能测试** 

AWS EC2虚拟机，超过两分钟的117,185个请求，平均延迟为10ms，每秒976个请求，或每天大约84,373,200个请求通过Kong，然后返回，只有一个超时。  
阿里做的测试：在没有进行任何优化的条件下，峰值约16000 QPS

** 并发性能，稳定性：**

较为正常请求情况下

| 请求总数 | 并发数 | 总用时 | 每秒完成请求数 | 用户平均请求等待时间 | 服服务器平均请求等待时间 |
| ------ | ------ | ------ | ------ |  ------ |  ------ |
| 5000 | 500 | 17.131 seconds | 291.87 | 1713.088 ms | 3.426 |
| 10000 | 500 | 31.789 seconds | 314.57 | 1589.471 ms | 3.179 |
| 15000 | 500 | 55.677 seconds | 269.41 | 1855.900 |3.712 |

sleep 2 sec 请求情况下

| 请求总数 | 并发数 | 总用时 | 每秒完成请求数 | 用户平均请求等待时间 | 服务器平均请求等待时间 |
| ------ | ------ | ------ | ------ |  ------ |  ------ |
| 500 | 50 | 58.620 seconds | 8.53  |  5862.036 ms | 117.241 |
| 1000 | 100 | 108.861 seconds | 9.19 |  10886.115  ms | 108.861 |
| 3000 | 100 | 310.854 seconds | 9.65 |  10361.786  ms | 103.618 |

** 日志功能：**

** 插件/二次开发的便利性：**

Kong采用插件机制进行功能定制，插件集（可以是0或n个）在API请求响应循环的生命周期中被执行。插件使用Lua编写，目前已有几个基础功能：HTTP基本认证、密钥认证、CORS（ Cross-origin Resource Sharing，跨域资源共享）、TCP、UDP、文件日志、API请求限流、请求转发以及nginx监控。

插件使用比较方便，开发是基于nginx的lua开发

**控制面板**

http://localhost:8080
## 社区声音

- Kong 的功能丰富、插件齐全；
- 进可当做 Ingress 接管入口中央集权制，退可当 Mesh 去中心化分封制；
- Nginx + lua 性能杠杠的，扛起了全球 10% 的流量，连 CloudFlare 跟我们用的都是一样的技术栈，而且 lua 也很容易学习；
- 市场上独领风骚；
- 基准测试中是最快的；
- 心动了没，赶紧找我们销售；

## 参考链接

- [KONG API Gateway-用户指南](https://github.com/cloudframeworks-apigateway/user-guide-apigateway)  
- [【kong系列一】之 API网关 & kong 概述](https://blog.csdn.net/li396864285/article/details/77371385)  
- [Kong：Nginx支持的API管理解决方案](https://sdk.cn/news/1596)
- [apigateway-kong(二)admin-api(结合实例比官网还详细)]( https://www.cnblogs.com/zhoujie/p/kong2.html)
- [KONG插件开发示例：accesslimiting](https://github.com/cloudframeworks-apigateway/user-guide-apigateway/blob/master/READMORE/KONG%E6%8F%92%E4%BB%B6%E5%BC%80%E5%8F%91%E7%A4%BA%E4%BE%8B%EF%BC%9Aaccesslimiting.md)
- [Kong 插件开发指南](https://ms2008.github.io/2018/06/19/kong-plugin-development/)
- [微服务Kong（六）——配置参考](https://www.cnblogs.com/SummerinShire/p/6624548.html)

