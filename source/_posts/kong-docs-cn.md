---
title: 微服务 API 网关 Kong 中文文档发布
toc: true
categories: 与代码
tags: 
	- Kong
	- Kong中文文档
	- Kong中文教程
	- OpenResty
	- api网关

date: 2019-06-27
---

由于项目的原因，最近的几个月一直在学习微服务的API网关 Kong ，在这里做一个简单介绍，是一个云原生，高效，可扩展的分布式 API 网关。 自 2015 年在 github 开源后，广泛受到关注，目前已收获 1.68w+ 的 star，其核心价值在于高性能和可扩展性。由于对项目的积极维护，Kong被广泛用于从初创公司到全球5000强以及政府机构的生产中。从技术角度来说，Kong是基于Openresty的一个应用，Openresty是基于Nginx的，使用的语言是Lua。

## 项目地址

- 官方网站：https://konghq.com
- 官方文档：https://docs.konghq.com
- 项目地址：https://github.com/Kong/kong

所以在学习过程中，首要是需要看官方文档，由于项目比较新，所以暂时没有中文文档，我就想着，反正文档总是要全部看一遍的，不如自己翻译一份好了，于是乎就有了这个项目：Kong的文档中文版。欢迎大家star&fork。

**Kong中文文档项目地址：https://github.com/qianyugang/kong-docs-cn**

由于自己不是英语专业，而且主要目的是学习Kong，所以采用的是人工+机翻结合的方式，如果有遇到翻译的不够通顺，或者对于翻译的语句有歧义的地方，麻烦一定点击官网英文文档https://docs.konghq.com/ 查看，并且欢迎提 PR 提修改意见。另，由于kong的文档本身也在不断增加和完善当中，如果有遇到没有即使更新翻译的状况欢迎提issue，我会不断补充的。

todo：

- 目前文档中的超链接都是链接的英文原文，后续会慢慢改成中文内链。
- ~~会在每一页文档里面附上单独的英文原文链接，以便做对照。~~
- ~~会添加kong自带的插件文档。~~

本文档是基于`1.0.x` 版本，目前官网已经更新至 `3.8.x` 版本，如果使用的最新版本，请查看 https://docs.konghq.com/gateway/ 并注意差别。

## 版本说明

- 官网最新版本：

	[![](https://img.shields.io/badge/Kong-3.8.x-green)](https://docs.konghq.com/gateway/3.8.x/)

- 其他版本查看：

	https://github.com/qianyugang/kong-docs-cn/blob/master/README.md#版本说明

