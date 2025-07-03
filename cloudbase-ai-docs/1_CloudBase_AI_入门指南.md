# CloudBase AI 入门指南

> **提示**  
> 云开发 AI+ 无缝接入 DeepSeek + 混元双模型开发智能体，支持小程序、h5、公众号、微信客服等场景  
> 新用户首月免费体验，并赠送 100 万 token  
> [了解更多](https://docs.cloudbase.net/ai/FAQ)

## 🌟 产品简介

腾讯云开发提供了云开发 AI 开发套件，帮助开发者快速构建自己的 AI 应用。

在云开发 [AI+](https://tcb.cloud.tencent.com/dev?#/ai) 中，我们提供一系列与 AI 相关的功能，如大模型接入、Agent 等，帮助开发者为自己的小程序、web 或者应用快速接入 AI 能力，同时也提供了云开发 Copilot，来加速用户的开发，帮助用户更快构建自己的应用。

### 🎯 核心优势

- **⚡ 极速开发**：零代码智能问答 Agent，5分钟完成部署
- **🧠 AI 能力强大**：DeepSeek + 混元双模型支持
- **🔌 集成方式丰富**：多端覆盖，多场景支持
- **🛠️ 开发友好**：MCP 协议，函数型 Agent，完整文档

## 🚀 快速开始

### 第一步：环境准备

1. **拥有腾讯云账号并开通云开发环境**
   - [立即开通云开发环境](https://docs.cloudbase.net/quick-start/create-weda)
   - 推荐 [开通微搭免费体验版](https://console.cloud.tencent.com/lowcode)

2. **登录云开发平台**
   - 访问 [云开发平台](https://tcb.cloud.tencent.com/dev)
   - 或访问 [云开发 Copilot 游客版](https://tcb.cloud.tencent.com/copilot) (无需登录)

### 第二步：选择开发方式

#### 🧠 Agent 类型选择

| 核心场景 | 能力介绍 | 操作指引 |
|---------|----------|----------|
| **智能问答 Agent** | 零代码：🧠 支持对接垂直知识库 ✅ 支持多轮对话/意图识别/多语言场景 | [搭建指南](https://docs.cloudbase.net/ai/agent/quickstart) |
| **自动化智能 Agent** | 零代码+MCP 扩展：🧠 通过 MCP 为 AI 提供调用外部世界的能力，AI 自动拆解任务，调用 MCP 连接万物并执行 | [搭建指南](https://docs.cloudbase.net/ai/agent/mcp) |
| **函数型 Agent** | 面向专业开发者：🧠 复杂场景 Agent，通过编码来调用 AI 大模型、MCP 插件、知识库、对接业务数据和业务接口，实现自定义代码逻辑 | [搭建指南](https://docs.cloudbase.net/ai/cbrf-agent/intro) |

### 第三步：集成应用

#### 💻 Agent 集成方式

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

#### 🧩 AI 组件接入

| 组件名称 | 操作指引 |
|---------|----------|
| **小程序源码组件** 集成 | [快速接入](https://docs.cloudbase.net/ai/agent-ui/agent-ui-mp) |
| **微搭可视化组件** 集成 | [快速接入](https://docs.cloudbase.net/ai/agent-ui/) |
| **React 组件** 集成 | [快速接入](https://docs.cloudbase.net/ai/agent-ui/agent-ui-react) |

#### ⚙️ AI SDK 接入

| 核心场景 | 操作指引 |
|---------|----------|
| **微信小程序** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#小程序端初始化) |
| **微信小游戏** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#小程序端初始化) |
| **微信跨端框架** 集成 | [快速接入](https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/miniapp/new-capability/cloud/cloud.html#_3-5-wx-cloud-extend-AI-接口) |
| **web 端** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#web-端初始化) |
| **Node 端** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#云函数-nodejs-端初始化) |
| **微搭中** 集成 | [快速接入](https://docs.cloudbase.net/ai/sdk-reference/init#在微搭中初始化) |
| **App 端** 集成 | [快速接入](https://docs.cloudbase.net/ai/FAQ#uniapptaro-等框架如何使用云开发-ai-能力) |
| **其他语言和框架** 集成 | [快速接入](https://docs.cloudbase.net/ai/api-key) |

## 📊 核心功能概览

### Agent 智能体

Agent，或称为 AI Agent，是以大模型为基础，具有强大的语言理解和生成能力，可以在各种领域执行复杂任务。

**核心特性**：
- 多种AI能力：可直接调用大模型API，或自定义Agent接入知识库系统
- 全渠道部署：支持小程序、Web网页、微信公众号/服务号及微信客服系统
- 模型支持：同时兼容DeepSeek和混元双模型

### 知识库

知识库是针对一类数据、信息、知识内容的整合。通过知识库，可以将现有数据、信息、内容进行整合、向量化，并通过知识库的查询、检索、分析等操作，快速找到需要的信息。

**支持格式**：
- 文件类型：.md, .pdf, .docx, .pptx
- 单个文件最大100M，md文件最大支持10M
- 一个知识库支持多个文件

### MCP协议

MCP是一个开放协议，它标准化了AI应用如何向大语言模型（LLMs）提供上下文。可以把MCP想象成AI应用的USB-C接口。

**核心优势**：
- **即插即用**：AI应用只需实现MCP协议，即可接入丰富的第三方工具生态
- **标准化**：所有工具遵循统一的描述格式和调用方式
- **解耦工具与应用**：工具提供者可以独立开发和维护工具
- **资源共享**：一次开发的工具可以被多个AI应用复用

## 🔄 开发流程

### 1. 环境准备
```bash
# 开通云开发环境
# 获取环境ID和密钥
```

### 2. 选择开发方式
- **零代码**：智能问答 Agent
- **低代码**：自动化智能 Agent + MCP
- **全代码**：函数型 Agent

### 3. 配置和开发
- 配置模型参数
- 设计对话流程
- 集成知识库
- 测试和优化

### 4. 部署和集成
- 选择部署方式
- 配置访问权限
- 集成到现有应用

## 📋 应用场景

### 客服场景
- **智能客服**：24/7 自动回复
- **工单处理**：自动分类和路由
- **FAQ 问答**：快速响应常见问题

### 营销场景
- **个性化推荐**：基于用户画像推荐
- **内容生成**：自动生成营销文案
- **客户洞察**：分析客户需求和偏好

### 办公场景
- **智能助手**：日程管理、文档处理
- **知识管理**：企业知识库问答
- **流程自动化**：审批流程智能化

### 教育场景
- **智能答疑**：学习问题实时解答
- **个性化学习**：根据进度调整内容
- **作业批改**：自动批改和反馈

## 🎯 下一步

现在您已经了解了CloudBase AI的基本概念和能力，可以：

1. **体验Demo**：通过小程序体验云开发AI能力
2. **创建第一个Agent**：跟随快速指引创建智能问答Agent
3. **探索高级功能**：了解MCP协议和函数型Agent
4. **查看案例**：学习实际项目经验和最佳实践

开始您的AI应用开发之旅吧！🚀 