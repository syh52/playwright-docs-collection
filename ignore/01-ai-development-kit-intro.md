# 云开发 AI 开发套件

## 简介

> **提示**  
> 无缝接入 DeepSeek + 混元双模型开发智能体，支持小程序、h5、公众号、微信客服等场景  
> 新用户首月免费体验，并赠送 100 万 token  
> [了解更多](https://docs.cloudbase.net/ai/FAQ)

腾讯云开发提供了云开发 AI 开发套件，帮助开发者快速构建自己的 AI 应用。

在云开发 [AI+](https://tcb.cloud.tencent.com/dev?#/ai) 中，我们提供一系列与 AI 相关的功能，如大模型接入、Agent 等，帮助开发者为自己的小程序、web 或者应用快速接入 AI 能力，同时也提供了云开发 Copilot，来加速用户的开发，帮助用户更快构建自己的应用。

## Agent 搭建

Agent，或称为 AI Agent，是以大模型为基础，具有强大的语言理解和生成能力，可以在各种领域执行复杂任务。

| 核心场景 | 能力介绍 | 操作指引 |
|---------|----------|----------|
| **智能问答 Agent** | 零代码：🧠 支持对接垂直知识库 ✅ 支持多轮对话/意图识别/多语言场景 | [搭建指南](https://docs.cloudbase.net/ai/agent/quickstart) |
| **自动化智能 Agent** | 零代码+MCP 扩展：🧠 通过 MCP 为 AI 提供调用外部世界的能力，AI 自动拆解任务，调用 MCP 连接万物并执行 | [搭建指南](https://docs.cloudbase.net/ai/agent/mcp) |
| **函数型 Agent** | 面向专业开发者：🧠 复杂场景 Agent，通过编码来调用 AI 大模型、MCP 插件、知识库、对接业务数据和业务接口，实现自定义代码逻辑 | [搭建指南](https://docs.cloudbase.net/ai/cbrf-agent/intro) |

Agent 搭建功能入口：[Agent 搭建](https://tcb.cloud.tencent.com/dev?#/ai?tab=agent)

[查看详细文档](https://docs.cloudbase.net/ai/agent/)

> **注意**  
> 云开发 Copilot 就是基于 AI+ 的 Agent 构建能力搭建的

## Agent 集成

开发完成的 Agent，可以通过以下方式进行使用：

| 核心场景 | 能力介绍 | 操作指引 |
|---------|----------|----------|
| **小程序** | ⚡ 即时响应/智能应答/多场景话术支持 | [立即接入](https://docs.cloudbase.net/ai/agent/release-mp) |
| **web 应用** | ⚡ 即时响应/智能应答/多场景话术支持 | [立即接入](https://docs.cloudbase.net/ai/agent/release) |
| **公众号客服** | ⚡ 即时响应/智能应答/多场景话术支持 | [立即接入](https://docs.cloudbase.net/ai/agent/integration-pa) |
| **小程序客服** | ⚡ 即时响应/智能应答/多场景话术支持 | [立即接入](https://docs.cloudbase.net/ai/agent/integration-mp) |
| **企微客服** | ⚡ 即时响应/智能应答/知识库联动/多场景话术支持 | [立即接入](https://docs.cloudbase.net/ai/agent/integration-workwx) |
| **服务号客服** | ⚡ 即时响应/智能应答/知识库联动/多场景话术支持 | [立即接入](https://docs.cloudbase.net/ai/agent/integration-server) |
| **现有应用中集成** | 🔧 现有**小程序/PC/H5**无缝接入方案 | [代码对接方案](https://docs.cloudbase.net/ai/agent/agent-ui) |

## Agent UI

我们还提供了 [Agent UI](https://docs.cloudbase.net/ai/agent/agent-ui)，可以无需开发聊天界面，直接在小程序、web 或者应用中使用。

| 组件名称 | 操作指引 |
|---------|----------|
| **小程序源码组件** 集成 | [快速接入](https://docs.cloudbase.net/ai/agent-ui/agent-ui-mp) |
| **微搭可视化组件** 集成 | [快速接入](https://docs.cloudbase.net/ai/agent-ui/) |
| **React 组件** 集成 | [快速接入](https://docs.cloudbase.net/ai/agent-ui/agent-ui-react) |

## AI SDK

我们提供了多端的 SDK，方便开发者快速集成 Agent，实现与 Agent 的交互。

小程序的完整指南可以参考 [小程序接入云开发 AI 能力指引](https://docs.cloudbase.net/ai/miniprogram-using)

| 核心场景 | 操作指引 |
|---------|----------|
| **微信小程序** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#%E5%B0%8F%E7%A8%8B%E5%BA%8F%E7%AB%AF%E5%88%9D%E5%A7%8B%E5%8C%96) |
| **微信小游戏** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#%E5%B0%8F%E7%A8%8B%E5%BA%8F%E7%AB%AF%E5%88%9D%E5%A7%8B%E5%8C%96) |
| **微信跨端框架** 集成 | [快速接入](https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/miniapp/new-capability/cloud/cloud.html#_3-5-wx-cloud-extend-AI-%E6%8E%A5%E5%8F%A3) |
| **web 端** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#web-%E7%AB%AF%E5%88%9D%E5%A7%8B%E5%8C%96) |
| **Node 端** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#%E4%BA%91%E5%87%BD%E6%95%B0-nodejs-%E7%AB%AF%E5%88%9D%E5%A7%8B%E5%8C%96) |
| **微搭中** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#%E5%9C%A8%E5%BE%AE%E6%90%AD%E4%B8%AD%E5%88%9D%E5%A7%8B%E5%8C%96) |
| **App 端** 集成 | [快速接入](https://docs.cloudbase.net/ai/FAQ#uniapptaro-%E7%AD%89%E6%A1%86%E6%9E%B6%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8%E4%BA%91%E5%BC%80%E5%8F%91-ai-%E8%83%BD%E5%8A%9B) |
| **其他语言和框架** 集成 | [快速接入](https://docs.cloudbase.net/ai/api-key) |

## 知识库

知识库是针对一类数据、信息、知识内容的整合。通过知识库，可以将现有数据、信息、内容进行整合、向量化，并通过知识库的查询、检索、分析等操作，快速找到需要的信息。通过 Agent 和知识库的整合，Agent 可以获取到特定领域的信息或知识，在问题解答、知识问答、数据检索等场景下，可以快速、准确地基于知识库内信息进行回复。

Agent 基于 AI 大模型结合知识库的智能化解决方案，通过 Agent 可以实现智能问答、智能检索、智能客服等功能。

查看 [知识库入门](https://docs.cloudbase.net/ai/agent/knowledgebase)

## MCP

MCP 是一个开放协议，它标准化了 AI 应用如何向大语言模型（LLMs）提供上下文。可以把 MCP 想象成 AI 应用的 USB-C 接口。就像 USB-C 为设备连接各种外设和配件提供了标准化方式一样，MCP 为 AI 模型连接不同的数据源和工具提供了标准化方式。

MCP 的核心采用 Client-Server 架构，一个应用程序可以连接多个 MCP Server。

只要 AI 应用实现了 MCP，即可接入到任意的 MCP Server，扩展自身的能力。加入 MCP Server 后，工具调用的流程如下：

通过这种方式，MCP 实现了：

1. **即插即用**：AI 应用只需实现 MCP 协议，即可接入丰富的第三方工具生态
2. **标准化**：所有工具遵循统一的描述格式和调用方式
3. **解耦工具与应用**：工具提供者可以独立开发和维护工具，不需了解 AI 应用内部实现
4. **资源共享**：一次开发的工具可以被多个 AI 应用复用

查看 [MCP 入门](https://docs.cloudbase.net/ai/agent/mcp)

## 内测反馈交流群

AI+ 内测反馈和交流群，欢迎开发者加入 