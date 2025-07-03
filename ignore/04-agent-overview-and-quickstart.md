# CloudBase AI Agent 概述与快速开始

## Agent 是什么

Agent，或称为 AI Agent，是以大模型为基础，通过特定指令或指引，能够完成特定任务。Agent 利用 AI 大模型，具有强大的语言理解和生成能力，可以在各种领域执行复杂任务。

## 使用 Agent

开发完成的 Agent，可以通过以下方式进行使用：

- 通过前端组件，在小程序中快速引入并配置对接 Agent，即可在会话组件中和 Agent 沟通；
- 通过 SDK，在小程序、H5、Web 应用的前端逻辑中，或者云函数、服务端等后端逻辑中调用 Agent，实现与 Agent 的交互；
- 通过第三方平台对接能力，将 Agent 与微信小程序客服、微信客服、微信公众号（服务号）、微信公众号（订阅号）完成对接，实现在这些平台中的聊天对话与 Agent 的对接；

## Agent 快速开始

通过这个教程，我们将在云开发平台上创建一个 **Agent AI** 小程序，实现调用 **Agent能力** 完成对话。

如果希望在 **自有小程序源码** 中接入 **Agent** 能力，请参考 [源码接入大模型](https://docs.cloudbase.net/ai/agent-ui/agent-ui-mp)

**Agent** 相关 **API** 请参考 [Agent API](https://docs.cloudbase.net/ai/agent/sdk)

> **提示**
> 在本教程中，我们会使用云开发提供的 **Agent UI** 组件 **0代码配置化** 搭建AI应用

### 新增Agent

进入 [云开发AI+](https://tcb.cloud.tencent.com/dev?#/ai?tab=agent)，选择agent模块，创建agent，按需选择对应的预设

这里可以调整AI名称头像、修改AI人设、添加知识库、开场文案、开场问题、问题建议等功能

完整 **Agent** 配置流程参考 [创建 Agent](https://docs.cloudbase.net/ai/agent/build)

### 创建AI应用

点击 **保存Agent**，然后点击 **确认** 前往AI+首页进行创建应用

此时会自动打开 **AI+首页**，并选择好当前的agent实例

点击安装Agent应用，则会跳转到可视化开发模块，并自动创建好了AI应用

### 发布应用

点击右上角发布，选择 **发布到web端**，选择发布到小程序需要先绑定小程序，可以按提示步骤进行操作并发布

完整 **Agent** 发布流程参考 [发布 Agent 应用](https://docs.cloudbase.net/ai/agent/release)

> **提示**
> 
> 小程序绑定流程：[小程序绑定流程](https://cloud.tencent.com/document/product/1301/86886)
> 
> 小程序发布相关问题：[小程序发布与部署问题](https://docs.cloudbase.net/ai/FAQ#%E5%8F%91%E5%B8%83%E4%B8%8E%E9%83%A8%E7%BD%B2)

发布完成后根据提供的链接或扫码进行访问

### 预览效果

发布PC端效果如下：
- 支持完整的对话界面
- 具备思考过程展示
- 支持文件上传等高级功能

H5及小程序效果如下：
- 移动端适配良好
- 保持一致的用户体验
- 支持微信生态集成

进行测试：
- 可以与Agent进行自然语言对话
- Agent会根据设定的人设和知识库回答问题
- 支持复杂的多轮对话

## 参考链接

- [Agent 概述](https://docs.cloudbase.net/ai/agent/)
- [Agent 快速开始](https://docs.cloudbase.net/ai/agent/quickstart)
- [创建 Agent](https://docs.cloudbase.net/ai/agent/build)
- [发布 Agent 应用](https://docs.cloudbase.net/ai/agent/release)
- [Agent API](https://docs.cloudbase.net/ai/agent/sdk) 