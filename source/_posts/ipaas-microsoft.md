---
title: 微软 iPaaS 产品介绍
toc: true
categories: 与代码
tags: 
	- iPaaS

date: 2020-04-02
---

## API全管理产品

微软Azure API Management 最开始是 Apiphany 开发的, 2013年被微软收购,然后集成到Azure平台。Azure API Management全面上市于2014年9月。

Azure API Management 是一个纯云平台的产品。它可以让组织能够安全，可靠且大规模地发布API。它旨在增加内部团队，合作伙伴和开发人员的API使用量。管理员门户使客户能够设置用户角色，创建使用计划和配额，应用策略来转换有效负载和限制以及使用分析，监视和警报。Azure API Management分为四个主要层，分别是Developer，Basic，Standard和Premium（允许进行多区域部署），为此，根据分配的资源和功能集，向用户收取固定的小时费用。附加的消费层将支持基于API调用的定价，但在撰写本文时仍处于有限的地理范围内。

Azure API Management在美洲，欧洲，亚洲，非洲和澳大利亚的40个公共云区域以及六个美国政府和国防部区域中可用。它的大多数客户位于美国和欧洲。

### 优势

- Azure API Management是低成本的--您为使用的API量付费，如果不使用，则无需付费。它还在全球范围内广泛使用，并支持9种语言。对于使用Azure云服务的任何组织来说，这都是立即选择，并且与Azure平台很好地集成在一起。
- Azure API Management易于使用且易于启动API程序。其管理功能是Azure门户的一部分。该产品背后的设计概念贯穿到使用它的所有服务中，并基于广泛的设计，可用性和可访问性测试。
- 作为Azure云服务系列的一部分，Azure API管理可从立即接触广泛的用户群中受益。它与一系列Azure服务集成在一起，包括Azure应用服务，Azure功能，Azure IoT中心，Azure Logic应用，Azure事件中心，Azure应用程序见解，Azure Monitor，Azure Active Directory和Azure服务总线。尽管Microsoft在其国际市场营销和销售方面投入的资金很少，但API管理用户社区还是众所周知的。


### 不足

- Azure API Management包括43常用操作政策和开发人员门户,但缺乏能力为以后API——例如生命周期阶段,许多先进的部署和运行特性和API退休/弃用支持。随着提供一般可用早在2014年,Gartner担心微软承诺的资源来管理市场在快速发展的API。
- Azure API Management支持用于访问本地资源的虚拟网络连接，但是缺少可以在本地或Azure外部托管的客户管理的部署选项。微软最近宣布，该选项将在2020年中期普遍可用。
- Azure API Management是一项Azure云服务，将继续关注开发人员和IT专业人员通过Microsoft的全球云构建，部署和管理应用程序的优先级。与该市场上的大多数其他产品相比，它不可能响应数字化转型的主要业务驱动因素而发展。

## 集成平台产品

微软（Microsoft）成立于1975年，总部位于美国华盛顿州雷德蒙德（Redmond），随着Azure Logic Apps的推出，于2016年7月进入EiPaaS 市场。当前，其主要的EiPaaS产品是Azure Integration Services（AIS），它将Azure Logic Apps 与Azure API Management，Azure Service Bus（消息队列和发布-订阅服务）和Azure Event Grid（海量事件提取服务）结合在一起。其他与集成相关的服务包括数据集成工具Azure Data Factory和基于Azure Logic Apps构建的持续集成工具Microsoft Flow。

### 优势

- 快速增长的是用来：尽管在市场上投放了不到三年的时间，Azure Logic Apps（AIS的主要组件）已经是为数不多的，拥有10,000多个客户的EiPaaS产品之一。这项卓越的成就源于Microsoft Azure的市场势头，Microsoft客户的忠诚度以及Microsoft作为可靠，有远见和创新的IT提供者的声誉，可以被视为战略业务合作伙伴。
- 全球：AIS在全球范围内可用，得到了全球Microsoft组织和约1,000个启用AIS的合作伙伴的支持。分布在世界每个主要地区的26个Azure数据中心（计划在2019年中期之前计划30个）中提供大多数AIS组件。加上Azure对一系列国际，地区和行业标准的支持，AIS成为了为数不多的真正全球EiPaaS产品之一。
- 技术愿景：AIS，Azure Data Factory,，Microsoft Flow和丰富的Azure服务使客户能够解决各种主流用例。 AIS的计划投资包括与Microsoft的AI / ML平台，区块链，IoT，边缘计算，函数即服务的集成，以及Azure Stack在本地部署上的可用性（预计在未来12到18个月内）。这些增强功能将把Microsoft EiPaaS的适用性扩展到几个关键的新兴集成方案中。

### 不足

- 功能成熟度：尽管Microsoft一直在努力改进其EiPaaS产品，但一些参考客户认为仍有改进AIS功能的余地。他们确定需要更大成熟度的领域包括数据转换和映射，EDI支持，集成工件的生命周期管理，自动测试和元数据发现。一些参考客户还对AIS的可管理性，可伸缩性和吞吐量感到担忧。
- 定价的复杂性和成本：尽管AIS的定价具有高度的灵活性和透明度，但参考客户也发现它的处理相当复杂，因为它需要详细了解其方案以得出成本。微软正在开发一个更简单，更可预测的定价模型，但是参考客户也对服务的实际成本以及微软在谈判中缺乏灵活性表示担忧。
- 以微软为中心的销售和营销策略：可以理解，微软的销售和营销主要利用Azure云平台的普及，并在较小程度上利用其SaaS应用程序（Office 365和Dynamics 365）的日益增长的成功来促进AIS的采用。因此，许多潜在客户认为AIS仅适用于“会员”，而对于寻求具备多云功能的EiPaaS的组织则没有特别的吸引力。


> 来源：

> - [Magic Quadrant for Full Life Cycle API Management](https://www.gartner.com/doc/reprints?id=1-1S7GFML6&ct=191011&st=sb)
> - [Magic Quadrant for Enterprise Integration Platform as a Service](https://www.gartner.com/doc/reprints?id=1-6DKO9VM&ct=190315&st=sb?)
> - [Magic Quadrant for Data Integration Tools](https://www.gartner.com/doc/reprints?id=1-1OCUQYNJ&ct=190805&st=sb?)


