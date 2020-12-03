---
title: Kong企业版的通用性功能 - 全生命周期服务管控平台
toc: true
comments: true
categories: 与代码
tags: 
	- Kong

date: 2020-10-12
---

我们很自豪地宣布，我们的旗舰企业产品 - Kong Enterprise 2020的最新版本全面上市。在去年的Kong峰会上，我们宣布了服务控制平台作为企业智能信息中介的愿景。今天，我们自豪地宣布，我们已经将[服务控制平台](https://konghq.com/products/service-control-platform/)的愿景向前推进了一步，使全球的组织能够管理和优化整个服务生命周期。 随着Kong Enterprise 2020 (KE 2020)的发布，Kong是唯一一个通过智能自动化和设计治理来优化当今应用现代化需求的解决方案，贯穿API和服务的全生命周期。我们正在为客户提供从生产前到生产后的全面服务管理，跨越所有平台、协议和部署模式。KE 2020完成了我们愿景的第一阶段，即建立一个智能服务控制平台，以管理整个企业的信息流，并将我们的愿景扩展到纳入API或服务的整个生命周期。

KE 2020代表了我们迄今为止最雄心勃勃、功能最丰富的企业版本。我们创建KE 2020是因为今天的组织必须能够在保持治理的同时快速创新。传统的API管理平台由于无法支持分散式架构、跨平台工作和实现自动化而阻碍了这些努力，而有了KE 2020，组织可以通过拥抱分散式架构、自动化工作流和采用CI/CD实践来实现现代化。KE 2020扩展了我们的承诺，即为开发人员提供自由，以最佳方式进行构建，同时也为管理团队提供工具，以确保效率和治理。在KE 2020中，各种规模的组织都可以跨平台、部署模式和传输协议自动实现API和微服务的安全、通信和协调。

![](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2019/10/KE-2020-Header-e1569997958497.png)

请继续阅读，以获得该版本中一些令人兴奋的新功能的预览。我们将详细介绍这些功能如何帮助您超越简单的API管理，智能控制整个服务生命周期。

#  Kong Studio 的集成服务设计和测试

建立在新收购的Insomnia之上，Kong Studio提供了一套强大的工具来简化REST和GraphQL服务的设计和测试。Kong Studio简化了开发工作流程，通过提供多种语言生成代码片段的能力，通过OpenAPI linting自动测试，通过Git Sync集成到CI/CD工作流程中，并能检查JSON和XML以外的响应，从而消除了繁琐的任务。观看我们最近关于[Kong Studio的网络研讨会](Watch our recent webinar on Kong Studio)，或[在此了解更多关于Studio的信息](Learn more about Studio here.)。

# Multi-Protocol 支持 gRPC, GraphQL 和 Kafka

Kong的多协议支持可简化新兴服务平台的采用。通过统一REST、gRPC、GraphQL和数据流服务之间的通信能力，Kong的客户可以解锁新的服务平台、用例和架构范式，而没有孤岛的风险。 Kong对gRPC和REST的原生支持，加上与Apollo GraphQL服务器和Apache Kafka的无缝集成，使用户能够毫不费力地在所有服务中应用一致的策略，以最大限度地减少摩擦并最大限度地控制。点击这里了解更多[关于多协议支持的信息。](Learn more about multi-protocol support here)

# Visual Service Map with Kong Brain

在去年推出的功能基础上进行扩展，最新版本的Kong Brain具有强大的服务地图，通过优雅的可视化界面帮助企业了解其整个服务架构。Kong Brain会自动生成和更新服务地图，以提供整个服务架构的实时视图。对于同时使用Kong Immunity的客户，服务地图还可以通过同一界面暴露实时异常警报和一键问题调查。 [点击这里了解更多关于 "Kong Brain"的信息](Learn more about Kong Brain here)。

![](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2019/10/Service-Map-2.gif)

# 优化的Kong Immunity 警报和用户界面

通过KE 2020，我们改进了Kong Immunity，提供了一个专门的用户界面来调查异常警报和跟踪解决状态。通过直观的过滤和排序功能，用户可以实时解析Immunity警报，将注意力集中在最严重的问题上。使用Kong Brain的客户还可以在服务地图中浮现Immunity警报，并轻松地深入了解显示异常行为的特定端点。[点击这里了解更多关于Kong Immunity的信息](Learn more about Kong Immunity here)。

![](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2019/10/Immunity-10.1.gif)

# 改善与Kong团队的合作和加入

Kong Teams提供了增强的协作、安全和入职功能，使KE用户能够轻松地在整个组织中扩展其部署。有了Kong Teams，KE用户可以轻松地将用户划分为工作小组，在整个环境中实施政策，并在确保合规性的同时，快速加入新用户。基于角色的访问控制和工作空间允许用户分配管理权限，并轻松授予或限制单个用户和消费者、整个团队、合作伙伴公司和整个 Kong 平台环境的访问权限。[点击这里了解更多关于Kong Teams的信息](Learn more about Kong Teams here)。

# Kong Manager和Kong Developer Portal的主要更新。

KE 2020对Kong Manager和开发者门户进行了重大改进，提供了显著升级的用户体验。这两款产品现在都提供了精简的权限、更简单的管理工作流程和更大的灵活性，使其更容易获得Kong的最大效益。最新版本的开发者门户使您更容易定制您的门户，并减少入职摩擦，为内部和外部提供最佳的开发者体验。要了解更多，请查看[管理](https://konghq.com/products/kong-manager)和[开发者门户](https://konghq.com/products/kong-enterprise/dev-portal)页面。

要了解更多关于如何升级到Kong Enterprise 2020的信息，[请参阅我们的升级文档](https://docs.konghq.com/enterprise/)。

# 参考链接

- [gartner full-life-cycle-api-management - kong](https://www.gartner.com/reviews/market/full-life-cycle-api-management/vendor/kong)
- [Reviews for Full Life Cycle API Management Market](https://www.gartner.com/reviews/market/full-life-cycle-api-management)
- [ Full Life Cycle API Management Kong vs MuleSoft](https://www.gartner.com/reviews/market/full-life-cycle-api-management/compare/kong-vs-mulesoft)
- [Kong Inc. Positioned as a Leader in the 2020 Gartner Magic Quadrant for Full Lifecycle API Management Market](https://www.prnewswire.com/news-releases/kong-inc-positioned-as-a-leader-in-the-2020-gartner-magic-quadrant-for-full-lifecycle-api-management-market-301138565.html)
- [Kong Inc. Positioned As A Leader In The 2020 Gartner Magic Quadrant For Full Lifecycle API Management Market](https://aithority.com/it-and-devops/cloud/kong-inc-positioned-as-a-leader-in-the-2020-gartner-magic-quadrant-for-full-lifecycle-api-management-market/)

> 原文地址：[General Availability of Kong Enterprise 2020 – The Full Lifecycle Service Control Platform](https://konghq.com/blog/General-Availability-of-Kong-Enterprise-2020)




