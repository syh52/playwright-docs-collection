# CloudBase AI 知识库

本知识库包含腾讯云开发 CloudBase AI 开发套件的完整文档，涵盖从入门到高级应用的所有内容。

## 📖 文档目录

### 1. [CloudBase AI 入门指南](./1_CloudBase_AI_入门指南.md)
- CloudBase AI 平台概述
- 快速开始和环境配置
- 基础概念和核心功能
- 平台架构和服务组件
- 开发环境搭建指南

### 2. [Agent 开发指南](./2_Agent开发指南.md) ⭐ **已更新**
- Agent 基础功能和配置
- 知识库集成和MCP工具
- Agent UI组件集成（React、小程序）
- SDK开发和API调用
- **🆕 自定义 Artifact 解析** - 支持代码和图表渲染
- **🆕 数据库集成** - 与云数据库深度整合
- **🆕 多会话管理** - 独立会话上下文管理
- 性能优化和最佳实践

### 3. [SDK 与 API 参考](./3_SDK与API参考.md)
- CloudBase AI SDK 完整API参考
- JavaScript SDK详细使用方法
- 微信小程序扩展API
- HTTP API接口规范
- 认证和权限管理
- 错误处理和调试指南

### 4. [MCP 与 ToolKit 指南](./4_MCP与ToolKit指南.md)
- MCP (Model Context Protocol) 协议详解
- CloudBase AI ToolKit 使用指南
- 自定义MCP工具开发
- 工具集成和部署
- IDE配置和开发环境

### 5. [Copilot 与开发指南](./5_Copilot与开发指南.md)
- 云开发 Copilot 功能介绍
- AI辅助开发最佳实践
- 代码生成和优化
- 智能调试和错误修复
- 开发效率提升技巧

### 6. [FAQ 与案例教程](./6_FAQ与案例教程.md)
- 常见问题解答
- 典型使用案例
- 故障排除指南
- 性能优化建议
- 社区资源和支持

### 7. [项目模板与开发工具](./7_项目模板与开发工具.md)
- 官方项目模板库
- 快速开发脚手架
- 开发工具推荐
- 部署和运维指南
- 持续集成配置

### 8. [微搭 AI+ 执行动作指南](./8_微搭AI+执行动作指南.md) 🆕 **新增**
- 微搭低代码平台AI+集成
- 调用大模型对话功能
- 调用Agent对话功能
- SSE流式响应处理
- 加载状态和错误处理
- 图生文能力应用
- 完整聊天应用示例

## 🎯 快速导航

### 新手入门
1. 从 [入门指南](./1_CloudBase_AI_入门指南.md) 开始了解平台
2. 阅读 [Agent开发指南](./2_Agent开发指南.md) 掌握核心功能
3. 参考 [SDK与API参考](./3_SDK与API参考.md) 进行开发

### 高级功能
- **Artifact 解析**：实现代码预览和图表渲染
- **数据库集成**：构建千人千面的智能助手
- **多会话管理**：支持独立的对话上下文
- **微搭集成**：低代码开发AI应用

### 开发工具
- **MCP 工具开发**：扩展Agent能力
- **Copilot 辅助**：提升开发效率
- **项目模板**：快速启动项目

## 🚀 新增功能亮点

### Artifact 自定义解析
```javascript
// 支持代码和图表的交互式渲染
<cloudbaseArtifact id="demo" title="示例应用" type="code">
<!DOCTYPE html>
<html>
  <!-- 完整的Web应用代码 -->
</html>
</cloudbaseArtifact>
```

### 数据库集成
```javascript
// 与云数据库深度集成，实现个性化AI助手
const response = await ai.bot.sendMessage({
  botId: 'your-bot-id',
  msg: '查询我借阅过的书籍',
  // 自动触发数据库查询
});
```

### 多会话管理
```javascript
// 独立的会话上下文管理
const conversation = await ai.bot.createConversation({
  botId: 'your-bot-id',
  title: '新的对话'
});
```

### 微搭AI+集成
- 可视化配置AI功能
- 支持流式响应显示
- 完善的状态管理
- 图生文能力支持

## 📊 文档更新记录

| 日期 | 更新内容 | 文档 |
|------|----------|------|
| 2024-01 | 🆕 新增微搭AI+执行动作完整指南 | 8_微搭AI+执行动作指南.md |
| 2024-01 | ✨ 新增Artifact自定义解析功能 | 2_Agent开发指南.md |
| 2024-01 | ✨ 新增数据库集成详细教程 | 2_Agent开发指南.md |
| 2024-01 | ✨ 新增多会话管理功能说明 | 2_Agent开发指南.md |
| 2024-01 | 📝 完善错误处理和调试指南 | 2_Agent开发指南.md |

## 🛠️ 技术栈支持

- **前端框架**：React、Vue、微信小程序
- **后端服务**：Node.js、Python、云函数
- **数据库**：Cloud Firestore、关系型数据库
- **AI模型**：混元、GPT、Claude等主流模型
- **开发工具**：VS Code、WebStorm、微信开发者工具

## 📞 获取帮助

- **官方文档**：[https://docs.cloudbase.net/ai/](https://docs.cloudbase.net/ai/)
- **开发平台**：[https://tcb.cloud.tencent.com/dev](https://tcb.cloud.tencent.com/dev)
- **技术支持**：通过开发平台获取技术支持
- **社区交流**：参与开发者社区讨论

## 📈 使用建议

1. **新手开发者**：按照文档顺序学习，重点关注入门指南和基础功能
2. **有经验开发者**：直接查看感兴趣的功能模块和API参考
3. **企业用户**：重点关注数据库集成、多会话管理等高级功能
4. **低代码用户**：优先了解微搭AI+执行动作指南

## ⚡ 快速链接

- [云开发控制台](https://tcb.cloud.tencent.com/dev)
- [AI+配置页面](https://tcb.cloud.tencent.com/dev#/ai)
- [Agent管理](https://tcb.cloud.tencent.com/dev#/ai?tab=agent)
- [模型配置](https://tcb.cloud.tencent.com/dev#/ai?tab=ai-model)

---

*本知识库持续更新中，如有问题或建议，请通过官方渠道反馈。* 