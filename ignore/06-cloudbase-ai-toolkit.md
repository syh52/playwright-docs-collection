# CloudBase AI ToolKit 完整指南

## 项目概述

**🪐 用 AI IDE 一键生成、部署和托管你的全栈 Web 应用与小程序、数据库和后端服务，无需运维，极速上线你的创意 💫**

[![GitHub Stars](https://img.shields.io/github/stars/TencentCloudBase/CloudBase-AI-ToolKit?style=social)](https://github.com/TencentCloudBase/CloudBase-AI-ToolKit)
[![CNB Community](https://img.shields.io/badge/CNB-Community-blue)](https://cnb.cool/tencent/cloud/cloudbase/CloudBase-AI-ToolKit)

当你在 **Cursor/ VSCode GitHub Copilot/WinSurf/CodeBuddy/Augment Code/Claude Code** 等AI编程工具里写代码时，它能自动帮你生成可直接部署的前后端应用+小程序，并一键发布到腾讯云开发 CloudBase。

### 🌟 开源项目

- **GitHub 仓库**: [TencentCloudBase/CloudBase-AI-ToolKit](https://github.com/TencentCloudBase/CloudBase-AI-ToolKit) - 欢迎 Star 和贡献代码
- **CNB 社区**: [CloudBase-AI-ToolKit](https://cnb.cool/tencent/cloud/cloudbase/CloudBase-AI-ToolKit) - 中国开发者社区

**📹 完整视频演示** ⬇️

[视频演示](https://www.bilibili.com/video/BV1hpjvzGESg/)

## ✨ 核心特性

- **🤖 AI 原生** - 专为 AI 编程工具设计的规则库，生成代码符合云开发最佳实践
- **🚀 一键部署** - MCP 自动化部署到腾讯云开发 CloudBase 平台，Serverless 架构无需购买服务器
- **📱 全栈应用** - Web + 小程序 + 数据库 + 后端一体化，支持多种应用形式和后端托管
- **🔧 智能修复** - AI 自动查看日志并修复问题，降低运维成本
- **⚡ 极速体验** - 国内 CDN 加速，比海外平台访问速度更快
- **📚 知识检索** - 内置云开发、微信小程序等专业知识库的智能向量检索

## 💻 支持的 AI 开发工具

| 工具 | 支持平台 |
|------|----------|
| [Cursor](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/cursor) | 独立 IDE |
| [WindSurf](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/windsurf) | 独立 IDE, VSCode、JetBrains 插件 |
| [CodeBuddy](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/codebuddy) | VS Code、JetBrains、微信开发者工具插件 |
| [CLINE](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/cline) | VS Code 插件 |
| [GitHub Copilot](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/github-copilot) | VS Code 插件 |
| [Trae](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/trae) | 独立 IDE |
| [通义灵码](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/tongyi-lingma) | 独立 IDE，VS Code、 JetBrains插件 |
| [RooCode](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/roocode) | VS Code插件 |
| [文心快码](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/baidu-comate) | VS Code、JetBrains插件 |
| [Augment Code](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/augment-code) | VS Code、JetBrains 插件 |
| [Claude Code](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/claude-code) | 命令行工具 |
| [Gemini CLI](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/ide-setup/gemini-cli) | 命令行工具 |

## 快速开始

### 🌟 核心能力

- **🤖 AI 智能开发**：自动生成代码和架构设计
- **☁️ 云开发集成**：一键接入数据库、云函数、静态托管
- **🚀 快速部署**：几分钟内完成全栈应用上线
- **📱 全栈支持**：Web + 小程序 + 后端一体化
- **🔧 智能修复**：AI 自动查看日志并修复问题

#### 🎯 适用场景
快速原型、学习实践、业务开发、小程序开发

---

### 🚀 第一步：选择你的 AI 开发工具

选择你正在使用或计划使用的 AI 开发工具，点击查看详细配置指南：

#### 快速配置（通用）

如果你的工具暂时没有专门的指南，可以使用以下通用 MCP 配置：

```json
{
  "mcpServers": {
    "cloudbase": {
      "command": "npx",
      "args": ["@cloudbase/cloudbase-mcp@latest"]
    }
  }
}
```

配置完成后，在 AI 对话中输入以下内容验证：

```
检查 CloudBase 工具是否可用
```

### ✅ 第二步：验证环境

#### 前置条件检查

- ✅ Node.js 环境
- ✅ 网络连接正常
- ✅ AI 开发工具已配置

### 🎯 第三步：选择项目模板

#### 📦 使用现成模板（推荐新手）

我们为你准备了多种技术栈的项目模板，内置云开发最佳实践和 AI 规则：

- **📱 微信小程序** + 云开发
- **⚛️ React Web** + 云开发
- **💚 Vue Web** + 云开发
- **🦄 UniApp** 跨端应用 + 云开发
- **🛠️ 通用模板**（适用于任意技术栈）

**👉 [查看所有项目模板](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/templates)**

选择合适的模板后，只需对 AI 说：

```
下载 [模板名称] 到当前目录
```

#### 🔧 增强现有项目

如果你已经有项目，只需对 AI 说：

```
在当前项目中下载云开发 AI 规则
```

AI 会自动下载并配置云开发规则到你的项目中。

### 🔐 第四步：登录云开发

在 AI 对话中输入：

```
登录云开发
```

AI 会自动：

- 弹出腾讯云登录界面
- 展示可用的云开发环境
- 完成环境选择和配置

#### 验证登录状态

```
查询当前云开发环境信息
```

### 🚀 第五步：开始开发

现在你可以通过自然语言描述需求，让 AI 帮你开发应用：

#### 示例需求

```
帮我创建一个待办事项应用，包含：
- 添加、删除、编辑待办事项
- 数据存储到云数据库
- 支持标记完成状态
- 部署到云端
```

```
创建一个简单的博客系统，需要：
- 文章列表页面
- 文章详情页面
- 文章发布功能
- 数据存储到云数据库
```

#### 💡 开发技巧

- **详细描述**：越详细的需求描述，AI 生成的代码越准确
- **增量开发**：可以逐步添加功能，比如先实现基础功能，再添加高级特性
- **错误处理**：遇到问题时，把完整的错误信息提供给 AI
- **日志调试**：可以要求 AI 查看云函数日志进行问题诊断

### 🎯 典型开发流程

```
描述需求 → AI 生成代码 → 创建云资源 → 本地测试 → 测试通过? 
    ↓                                                      ↑
部署上线 ← 获得访问链接                        AI 查看日志修复（如果测试不通过）
```

### 🔧 调试和问题修复

#### 遇到错误时

```
报错了，错误信息是：[粘贴完整错误信息]
```

#### 云函数调试

```
云函数运行异常，需求是 [描述原本的需求]，请查看日志进行调试和修复
```

#### 环境切换

如需切换云开发环境：

```
退出云开发
```

然后重新登录选择其他环境。

## 项目模板详解

### 🚀 新项目推荐

#### 微信小程序 + 云开发模板

云开发小程序解决方案模板，包含小程序基础配置。

- **下载地址**：[代码包下载](https://static.cloudbase.net/cloudbase-examples/miniprogram-cloudbase-miniprogram-template.zip?v=2025053001)
- **开源代码**：[GitHub](https://github.com/TencentCloudBase/awesome-cloudbase-examples/tree/master/miniprogram/cloudbase-miniprogram-template)
- **适用场景**：微信小程序开发

#### React Web 应用 + 云开发模板

现代化的 React 全栈应用模板，包含云开发集成配置。

- **下载地址**：[代码包下载](https://static.cloudbase.net/cloudbase-examples/web-cloudbase-react-template.zip?v=2025053001)
- **开源代码**：[GitHub](https://github.com/TencentCloudBase/awesome-cloudbase-examples/tree/master/web/cloudbase-react-template)
- **适用场景**：Web 应用开发

#### Vue Web 应用 + 云开发模板

现代化的 Vue 全栈应用模板，包含云开发集成配置。

- **下载地址**：[代码包下载](https://static.cloudbase.net/cloudbase-examples/web-cloudbase-vue-template.zip?v=2025053001)
- **开源代码**：[GitHub](https://github.com/TencentCloudBase/awesome-cloudbase-examples/tree/master/web/cloudbase-vue-template)
- **适用场景**：Web 应用开发

#### UniApp 跨端应用 + 云开发模板

基于 UniApp 的云开发跨端应用模板，可编译到 H5 和微信小程序。

- **下载地址**：[代码包下载](https://static.cloudbase.net/cloudbase-examples/universal-cloudbase-uniapp-template.zip?v=2025053001)
- **开源代码**：[GitHub](https://github.com/TencentCloudBase/awesome-cloudbase-examples/tree/master/universal/cloudbase-uniapp-template)
- **适用场景**：跨端应用开发（H5、微信小程序）

#### AI 规则通用云开发模板

不限定语言和框架，内置 CloudBase AI 规则和MCP，适用于任意云开发项目。

- **下载地址**：[代码包下载](https://static.cloudbase.net/cloudbase-examples/web-cloudbase-project.zip)
- **开源代码**：[GitHub](https://github.com/TencentCloudBase/awesome-cloudbase-examples/tree/master/web/cloudbase-project)
- **适用场景**：任意云开发项目

### 🛠️ 已有项目增强

如果你已经有自己的项目，只需在配置好 MCP 后，对 AI 说：

```
在当前项目中下载云开发 AI 规则
```

即可一键下载并补全 AI 编辑器规则配置到当前项目目录，无需手动操作。

### 📁 模板包含内容

所有模板都包含以下配置：

- ✅ AI IDE 规则配置（针对不同编辑器）
- ✅ CloudBase MCP 配置
- ✅ 云开发最佳实践代码结构
- ✅ 项目说明文档

## 🎉 完成！你已准备就绪

✅ AI 开发工具已配置
✅ 云开发环境已连接
✅ 项目模板已准备
✅ 开发流程已了解

现在开始用自然语言描述你的应用需求，让 AI 帮你快速构建全栈应用吧！

## 📚 进阶学习

- 📖 [项目模板详解](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/templates) - 深入了解各种项目模板
- 💻 [开发最佳实践](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/development) - 掌握高级开发技巧
- 🎯 [实际案例](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/examples) - 学习真实项目经验
- 🛠️ [MCP 工具参考](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/mcp-tools) - 了解所有可用工具
- ❓ [常见问题](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/faq) - 解决开发中的问题

---

💫 如果这个项目对你有帮助，欢迎给我们一个 ⭐ Star！

[![GitHub](https://img.shields.io/github/stars/TencentCloudBase/CloudBase-AI-ToolKit?style=social)](https://github.com/TencentCloudBase/CloudBase-AI-ToolKit)
[![CNB](https://img.shields.io/badge/CNB-Community-blue)](https://cnb.cool/tencent/cloud/cloudbase/CloudBase-AI-ToolKit)

## 🌟 为什么选择 CloudBase？

- **⚡ 极速部署**：国内节点,访问速度比海外更快
- **🛡️ 稳定可靠**：330 万开发者选择的 Serverless 平台
- **🔧 开发友好**：专为AI时代设计的全栈平台，支持自动环境配置
- **💰 成本优化**：Serverless 架构更具弹性，新用户开发期间可以免费体验

如有迁移、集成等常见疑问，请查阅 [FAQ 常见问题](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/faq)。

## 💬 技术交流群

遇到问题或想要交流经验？加入我们的技术社区！

### 🔥 微信交流群

扫码加入微信技术交流群

## 参考链接

- [CloudBase AI ToolKit 概述](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/)
- [快速开始](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/getting-started)
- [项目模板](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/templates)
- [开发指南](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/development)
- [教程](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/tutorials)
- [常见问题 FAQ](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/faq)
- [使用案例](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/examples)
- [MCP 工具](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/mcp-tools) 