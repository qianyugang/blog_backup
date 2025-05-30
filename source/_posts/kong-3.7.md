---
title: Kong 3.7重磅上线！Kong AI Gateway 正式 GA！🦍
toc: true
comments: true
categories: 与代码
tags: 
	- Kong
	- Kong中文文档

date: 2024-08-27
---


Kong Gateway 3.7 版本已经重磅上线，我们给 AI Gateway 带来了一系列升级，下面是 AI Gateway 的更新亮点一览。
文转载自 Kong Inc 公众号，原文查看 https://mp.weixin.qq.com/s/zMZpuEA1tI0UNlD7X7PP8A 

# AI Gateway 正式 GA

在 Kong Gateway 的最新版本 3.7 中，我们正式宣布 Kong AI Gateway 达到了通用可用性（GA）阶段。

现在，AI 开发者们可以专注于开发 AI 定制应用，比如利用大型语言模型（LLM）和检索增强生成（RAG）技术打造的聊天机器人，或者其他 AI 集成方案。他们无需再从零开始搭建底层架构，去构建保证 AI 应用在生产环境中安全、可监控的基础设施。Kong Konnect 和 Kong Gateway Enterprise 平台将提供所需的扩展性支持。

此外，Kong AI Gateway 现在也可以作为一个软件即服务（SaaS）解决方案完全部署在云端。同时，Kong 还推出了新的 Konnect Dedicated Cloud Gateways 选项，供用户进行云端部署。

Kong AI Gateway 可以用于广泛的场景，帮助加速新的人工智能应用程序在生产环境中的落地。

# 对现有的 OpenAI SDK 提供支持

Kong AI Gateway 允许让用户通过 OpenAI API 规范作为统一标准，访问其支持的所有 LLM。

使用开发人员熟悉的 OpenAI API 规范将大大简化大家上手的难度。

并且， Kong AI Gateway 原生支持了 OpenAI SDK 客户端库，进一步简化了构建 AI 代理和应用程序的过程。您只需将请求重定向到指向 AI Gateway 路由的 URL，即可通过 AI Gateway 使用LLM。

如果您已经使用 OpenAI SDK 编写了现有的业务逻辑，则可以重用它来使用 Kong AI Gateway 支持的每个 LLM，无需修改代码，因为它是100%兼容的。

# 引入流式 AI 消息支持

Kong AI Gateway 已在”ai-proxy” 插件中，对所有LLM引擎加入了对AI的流式交互能力的原生支持。这将解锁更多实时体验，而不用等待 LLM 完成处理后再发送回客户端。

在流式模式下，响应将以词元（token）为单位通过 HTTP 响应块（SSE）逐个发送。用户可以通过设置“ ai-proxy” 的以下属性来启用该功能：

```
config:
  model:
    options:
      response_streaming: "allow"
```

功能启用后，客户端便可在请求体中显式地进行流式请求，例如：

```
{
  "prompt": "What is 1 + 1?",
  "stream": true
}
```
凭借这项新功能，Kong AI Gateway 的用户将能够打造更具吸引力和互动性的人工智能体验。

# 基于 Token 的高级限流能力 (企业版)

我们正在引入一项基于 token 请求量进行限流的企业级功能。通过启用新的“ai-rate-limiting-advanced” 插件，客户可以更好地管理组织中不同团队的 token 消耗水平，从而更好地控制整体 AI 开销。对于自托管 LLM 提供商，当应用程序中的 AI 流量增加时，客户将能够更好地调整其在 AI 基础设施上的流量。

Kong 已经提供了基于发送到 API 的请求数量进行速率限制的 API 速率限制功能。而新版“ai-rate-limiting-advanced” 插件则专注于所请求 AI token 的数量，并不考虑发送给它们原始 HTTP 请求的数量。如果客户希望同时对原始请求和特定AI Token进行速率限制，则“ai-rate-limiting-advanced” 插件可以与标准 Kong 速率限制插件结合使用。

ai-rate-limiting-advanced 插件是目前市面上唯一可以用于 AI 的速率限制插件。

# 基于 Azure 的内容安全能力 (企业版)

新的企业插件“ai-azure-content-safety”允许客户与包括“Azure AI”在内的多个内容安全服务无缝集成，以验证每个通过AI网关的prompt请求。这项功能也被所有ai-proxy插件的所有LLM引擎所支持。

例如：凭借该功能，客户可以使用 Azure 的原生安全服务策略，在 Kong AI Gateway 中检测和过滤所有不和谐的内容，并将该策略应用于所有 LLM 提供商的prompt请求，以实现内容安全的统一管理。

# 基于 URL 动态选择 LLM

该特性使用户可以通过客户端请求的 URL 路径动态调用所需的模型。同时，用户可以通过在插件配置中硬编码其名称来使用模型。通过启用此功能，Kong AI Gateway 便可以更容易地扩展到希望尝试各种模型的团队，而无需预先在 “ai-proxy” 插件中进行配置。

该功能可以通过 “ai-proxy” 的新 “config.route_source” 配置参数进行配置。并且，用户只需配置一次，便可使所有模型均通过识别URL路径的方式来动态地、灵活地调用。

# 支持 Anthropic Claude 2.1 Messages API

Kong AI Gateway 提供一个 API 接口，使用户可以随意调用部署在云端的或自托管提供商提供的模型。该接口在新版本中得到了扩展，以支持 Anthropic Claude 2.1 Messages 这样的通常用于创建聊天机器人或虚拟助手应用程序的API，用于管理用户与 Anthropic Claude 模型（助手）之间的对话交流。

基于用户需求， Kong AI Gateway 将持续增加对更多 LLM 的支持。


# 更新 AI 用量统计的格式

随着 Kong AI Gateway 进入 GA 阶段，我们已经更新了所有由 Kong 处理的 AI 请求的分析日志格式。

通过这种新的日志格式，用户可以测量 “ai-proxy”，“ai-request-transformer”和“ai-response-transformer” 所请求的每个模型的消耗情况。

```
"ai": {
    "ai-proxy": {
      "meta": {
        "request_model": "gpt-35-turbo",
        "provider_name": "azure",
        "response_model": "gpt-35-turbo",
        "plugin_id": "5df193be-47a3-4f1b-8c37-37e31af0568b"
      },
      "payload": {},
      "usage": {
        "prompt_token": 89,
        "completion_token": 56,
        "total_tokens": 145
      }
    },
    … more AI Plugins
```

这种新的分析日志格式取代了旧的格式，以便企业用户实现更精细化的用量管理。







