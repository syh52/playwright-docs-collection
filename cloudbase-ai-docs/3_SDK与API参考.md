# CloudBase AI SDK 与 API 参考

## 📋 概述

CloudBase AI SDK 提供了完整的 AI 开发能力，包括大模型调用、Agent 交互、工具集成等功能。本文档涵盖了SDK初始化、API调用和小程序集成的完整指引。

## 🚀 SDK 初始化

### Web 端初始化

```javascript
import cloudbase from "@cloudbase/js-sdk";

const app = cloudbase.init({
  env: "your-env-id", // 云开发环境ID
  timeout: 600000, // 设置超时时间为 10 分钟
});

const auth = app.auth();
await auth.signInAnonymously(); // 或者使用其他登录方式

const ai = app.ai(); // 后续就可以按常规方式调用 ai 能力了
```

### 小程序端初始化

```javascript
wx.cloud.init({
  env: "<云开发环境ID>",
});

// 使用微信小程序基础库内置的 AI 能力
const model = wx.cloud.extend.AI.createModel("deepseek");
```

### Node.js 端初始化

```javascript
const cloudbase = require("@cloudbase/node-sdk");

const app = cloudbase.init({
  env: "your-env-id",
  secretId: "your-secret-id",
  secretKey: "your-secret-key"
});

const ai = app.ai();
```

## 📱 小程序接入指南

### 准备工作

- 注册一个微信小程序账号，并且创建本地小程序工程项目
- 小程序基础库需要在 3.7.1 及以上版本，具备 `wx.cloud.extend.AI` 对象
- 小程序需要开通「云开发」，可在小程序开发工具中点击工具栏里的「云开发」按钮进行开通

### 指引一：调用大模型，实现文本生成

以一个"七言绝句"生成器的简单 Demo 为例：

#### 第 1 步：初始化云开发环境

```javascript
wx.cloud.init({
  env: "<云开发环境ID>",
});
```

#### 第 2 步：创建 AI 模型，并调用生成文本

```javascript
// 创建模型实例，这里我们使用 DeepSeek 大模型
const model = wx.cloud.extend.AI.createModel("deepseek");

// 设定 AI 的系统提示词
const systemPrompt = 
  "请严格按照七言绝句或七言律诗的格律要求创作，平仄需符合规则，押韵要和谐自然...";

// 用户的自然语言输入
const userInput = "帮我写一首赞美玉龙雪山的诗";

// 调用大模型
const res = await model.streamText({
  data: {
    model: "deepseek-r1", // 指定具体的模型
    messages: [
      { role: "system", content: systemPrompt },
      { role: "user", content: userInput },
    ],
  },
});

// 接收大模型的响应
for await (let str of res.textStream) {
  console.log(str);
}
```

### 指引二：通过 Agent（智能体）实现智能对话

#### 第 1 步：创建一个 Agent

进入 [云开发平台-AI+](https://tcb.cloud.tencent.com/dev?#/ai?tab=agent)，创建一个新的 Agent。

点击页面中上方的"复制 ID"，获得一个 `bot-id`，即 Agent 的唯一标识。

#### 第 2 步：在小程序中实现与 Agent 的对话

```javascript
// 用户的输入
const userInput = 
  "我的小程序这个报错是什么意思：FunctionName parameter could not be found";

const res = await wx.cloud.extend.AI.bot.sendMessage({
  data: {
    botId: "xxx-bot-id", // Agent唯一标识
    msg: userInput, // 用户的输入
    history: [], // 历史对话的内容
  },
});

for await (let x of res.textStream) {
  console.log(x);
}
```

#### 第 3 步：实现更加丰富的聊天功能

```javascript
// 获取聊天记录
await wx.cloud.extend.AI.bot.getChatRecords({
  botId: "botId-xxx",
  pageNumber: 1,
  pageSize: 10,
  sort: "asc",
});

// 发送用户反馈
const res = await wx.cloud.extend.AI.bot.sendFeedback({
  userFeedback: {
    botId: "botId-xxx",
    recordId: "recordId-xxx",
    comment: "非常棒",
    rating: 5,
    tags: ["优美"],
    aiAnswer: "落英缤纷",
    input: "来个成语",
    type: "upvote",
  },
});

// 获取次轮推荐问题
const res = await wx.cloud.extend.AI.bot.getRecommendQuestions({
  data: {
    botId: "xxx-bot-id",
    msg: "介绍一下 Python 语言",
  },
});

for await (let x of res.textStream) {
  console.log(x);
}
```

### 指引三：使用云开发 AI 对话组件

#### 第 1 步：下载组件包

方式一：直接 [下载组件示例包](https://gitee.com/TencentCloudBase/cloudbase-agent-ui/tree/main/apps/miniprogram-agent-ui)

方式二：通过微信开发者工具创建 agent-ui 组件模板

#### 第 2 步：引入组件到自己的小程序项目

```json
// page.json
{
  "usingComponents": {
    "agent-ui": "/components/agent-ui/index"
  },
}
```

```xml
<!-- page.wxml -->
<view>
  <agent-ui agentConfig="{{agentConfig}}" showBotAvatar="{{showBotAvatar}}" chatMode="{{chatMode}}" modelConfig="{{modelConfig}}"></agent-ui>
</view>
```

```javascript
// page.js
data: {
  chatMode: "bot", // bot 表示使用agent，model 表示使用大模型
  showBotAvatar: true, // 是否在对话框左侧显示头像
  agentConfig: {
    botId: "bot-e7d1e736", // agent id,
    allowWebSearch: true, // 允许客户端选择启用联网搜索
    allowUploadFile: true, // 允许上传文件
    allowPullRefresh: true, // 允许下拉刷新
    allowUploadImage: true, // 允许上传图片
    showToolCallDetail: true, // 是否展示工具调用细节
    allowMultiConversation: true, // 是否展示会话列表，创建会话按钮
  }
}
```

## 🔧 AI SDK API 参考

### app.ai()

初始化后，可以使用挂载至 cloudbase 实例上的 ai 方法创建 AI 实例。

```javascript
const app = cloudbase.init({"env": "your-env"});
const ai = app.ai();
```

### AI 类

#### createModel()

创建指定的 AI 模型。

```javascript
const model = ai.createModel("hunyuan-exp");
```

#### registerFunctionTool()

注册函数工具。在进行大模型调用时，可以告知大模型可用的函数工具。

```javascript
// 1. 定义获取天气的工具
const getWeatherTool = {
  name: "get_weather",
  description: "返回某个城市的天气信息。调用示例：get_weather({city: '北京'})",
  fn: ({city}) => `${city}的天气是：秋高气爽！！！`,
  parameters: {
    type: "object",
    properties: {
      city: {
        type: "string",
        description: "要查询的城市",
      },
    },
    required: ["city"],
  },
};

// 2. 注册工具
ai.registerFunctionTool(getWeatherTool);

// 3. 在给大模型发送消息的同时，告知大模型可用的工具
const model = ai.createModel("hunyuan-exp");
const result = await model.generateText({
  model: "hunyuan-turbo",
  tools: [getWeatherTool], // 传入获取天气工具
  messages: [{
    role: "user",
    content: "请告诉我北京的天气状况",
  }],
});

console.log(result.text);
```

### ChatModel 类

#### generateText()

调用大模型生成文本。

```javascript
const hy = ai.createModel("hunyuan-exp");
const res = await hy.generateText({
  model: "hunyuan-lite",
  messages: [{role: "user", content: "你好，请你介绍一下李白"}],
});
console.log(res.text);
```

**返回值：**

| 属性名 | 类型 | 说明 |
|--------|------|------|
| text | string | 大模型生成的文本 |
| rawResponses | unknown[] | 大模型的完整返回值 |
| messages | ChatModelMessage[] | 本次调用的完整消息列表 |
| usage | Usage | 本次调用消耗的 token |
| error | unknown | 调用过程中产生的错误 |

#### streamText()

以流式调用大模型生成文本。

```javascript
const hy = ai.createModel("hunyuan-exp");
const res = await hy.streamText({
  model: "hunyuan-lite",
  messages: [{role: "user", content: "你好，请你介绍一下李白"}],
});

for await (let str of res.textStream) {
  console.log(str); // 打印生成的文本流
}

for await (let data of res.dataStream) {
  console.log(data); // 打印每次返回的完整数据
}
```

### Bot 类

#### sendMessage()

与 Agent 进行对话的核心接口。

```javascript
const res = await ai.bot.sendMessage({
  botId: "botId-xxx",
  msg: "给我一个成语。",
  history: [], // 历史对话记录
  files: [], // 上传的文件 ID 数组
});

for await (let str of res.textStream) {
  console.log(str);
}
```

#### get()

获取某个 Agent 的详细信息。

```javascript
const res = await ai.bot.get({ botId: "botId-xxx" });
console.log(res);
```

#### list()

查看 Agent 列表。

```javascript
const list = await ai.bot.list({
  pageNumber: 1, 
  pageSize: 10
});
console.log(list);
```

#### getChatRecords()

查看与 Agent 的聊天记录。

```javascript
const res = await ai.bot.getChatRecords({
  botId: "botId-xxx",
  pageNumber: 1,
  pageSize: 10,
  sort: "asc",
});
```

#### sendFeedback()

对某一条聊天记录进行反馈。

```javascript
await ai.bot.sendFeedback({
  userFeedback: {
    botId: "botId-xxx",
    recordId: "recordId-xxx",
    comment: "我觉得你回答的很好",
    rating: 5,
    tags: ["切题"],
    aiAnswer: "李白是伟大的唐朝诗人。",
    input: "一句话介绍李白。",
    type: "upvote",
  },
});
```

#### uploadFiles()

将云存储中的文件上传至 Agent，用于进行文档聊天。

```javascript
// 上传文件
await ai.bot.uploadFiles({
  botId: "botId-xxx",
  fileList: [{
    fileId: "cloud://xxx.docx",
    fileName: "xxx.docx",
    type: "file",
  }],
});

// 进行文档聊天
const res = await ai.bot.sendMessage({
  botId: "your-bot-id",
  msg: "这个文件的内容是什么",
  files: ["xxx.docx"], // 文件 fileId 数组
});

for await (let text of res.textStream) {
  console.log(text);
}
```

#### speechToText()

语音转文字。

```javascript
const res = await ai.bot.speechToText({
  botId: "botId-xxx",
  engSerViceType: "16k_zh",
  voiceFormat: "mp3",
  url: "https://example.com/audio.mp3",
});
```

#### textToSpeech()

文字转语音。

```javascript
const res = await ai.bot.textToSpeech({
  botId: "botId-xxx",
  voiceType: 1,
  text: "你好，我是AI助手",
});
```

## 🌐 HTTP API 调用

### API Key 配置

前往[云开发平台/环境配置/API Key 配置页面](https://tcb.cloud.tencent.com/dev#/env/apikey)即可新建 API Key。

获取到 API Key 后，即可进行 HTTP API 调用，只需要在 HTTP 请求中附带 `"Authorization: Bearer <您的 API Key>"` 请求头。

### 大模型调用

#### cURL 示例

```bash
curl -X POST 'https://your-env-id.api.tcloudbasegateway.com/v1/ai/deepseek/v1/chat/completions' \
  -H 'Authorization: Bearer <您的 API Key>' \
  -H 'Content-Type: application/json' \
  -H 'Accept: text/event-stream' \
  -d '{
    "model": "deepseek-r1",
    "messages": [
      {
        "role": "user",
        "content": "介绍一下你自己"
      }
    ],
    "stream": true
  }'
```

#### OpenAI SDK 示例

```javascript
const OpenAI = require("openai");

const client = new OpenAI({
  apiKey: "您的 API Key",
  baseURL: "https://your-env-id.api.tcloudbasegateway.com/v1/ai/deepseek/v1",
});

async function main() {
  const completion = await client.chat.completions.create({
    model: "deepseek-r1",
    messages: [{role: "user", content: "你好"}],
    temperature: 0.3,
    stream: true,
  });

  for await (const chunk of completion) {
    console.log(chunk);
  }
}

main();
```

### AI Agent 调用

```bash
curl 'https://<your-envId>.api.tcloudbasegateway.com/v1/aibot/bots/<your-botId>/send-message' \
  -H 'Authorization: Bearer <您的 API Key>' \
  -H 'Accept: text/event-stream' \
  -H 'Content-Type: application/json' \
  --data-raw '{"msg":"hi"}'
```

### 知识库调用

```bash
curl 'https://<your-envId>.api.tcloudbasegateway.com/v1/knowlege/send-message' \
  -H 'Authorization: Bearer <您的 API Key>' \
  -H 'Accept: text/event-stream' \
  -H 'Content-Type: application/json' \
  -d '{
    "collectionView": "ykfty_6fWO",
    "search": {
      "content": "云开发 AI+ 最新有哪些新功能？",
      "limit": "5"
    }
  }'
```

## 🔄 类型定义

### BaseChatModelInput

```typescript
interface BaseChatModelInput {
  model: string;
  messages: Array<ChatModelMessage>;
  temperature?: number;
  topP?: number;
  tools?: Array<FunctionTool>;
  toolChoice?: "none" | "auto" | "custom";
  maxSteps?: number;
  onStepFinish?: (prop: IOnStepFinish) => unknown;
}
```

### ChatModelMessage

```typescript
type ChatModelMessage = UserMessage | SystemMessage | AssistantMessage | ToolMessage;

type UserMessage = {
  role: "user";
  content: string;
};

type SystemMessage = {
  role: "system";
  content: string;
};

type AssistantMessage = {
  role: "assistant";
  content?: string;
  tool_calls?: Array<ToolCall>;
};

type ToolMessage = {
  role: "tool";
  tool_call_id: string;
  content: string;
};
```

### FunctionTool

```typescript
type FunctionTool = {
  name: string;
  description: string;
  fn: CallableFunction;
  parameters: object;
};
```

### Usage

```typescript
type Usage = {
  completion_tokens: number;
  prompt_tokens: number;
  total_tokens: number;
};
```

## 🛠️ 跨平台支持

### UniApp/Taro 等框架

如果使用微信 Donut 多端框架开发应用，可以使用 wx.cloud.extend.AI 接口来调用云开发 AI 能力，和微信小程序中保持一致。

UniApp/Taro 可以通过分别调用微信基础库和 JSSDK 来实现多端调用，参考[SDK 初始化](#sdk-初始化)。

### 其他语言或框架

可以通过 HTTP API 来调用，参考[HTTP API 调用](#http-api-调用)。

## 📊 主要特性

- **兼容 OpenAI SDK**：支持使用标准的 OpenAI SDK 进行调用
- **流式输出**：支持实时流式响应
- **多种调用方式**：支持 cURL、各种编程语言的 HTTP 客户端
- **统一认证**：使用 API Key 进行统一身份验证

## 🎯 适用场景

- **第三方系统集成**：将 CloudBase AI 能力集成到现有系统
- **自动化脚本**：在批处理、定时任务中使用 AI 能力
- **多语言支持**：在各种编程语言中调用 AI 服务
- **服务端应用**：在后端服务中直接调用 AI API

## 🚨 最佳实践

### 1. 模型选择
- 根据不同场景选择合适的模型，如 `hunyuan-lite` 适合轻量级对话，`hunyuan-exp` 适合复杂任务
- `deepseek-v3` 专注于通用任务，`deepseek-r1` 适合推理任务

### 2. 工具注册
- 合理设计工具的描述和参数，帮助大模型更好地理解工具用途

### 3. 错误处理
- 注意检查返回值中的 `error` 字段，做好异常处理

### 4. 流式调用
- 对于长文本生成，推荐使用 `streamText()` 提供更好的用户体验

### 5. Agent 交互
- 合理利用历史记录和文件上传功能，提升对话效果

### 6. 超时配置
- 根据实际情况配置合理的超时时间，特别是对于复杂推理任务

## ⚠️ 注意事项

- API Key 需要妥善保管，避免泄露
- 请求需要包含正确的认证头
- 支持流式和非流式两种响应模式
- 不同的 API 具有不同的请求参数和响应格式
- 小程序包体积有限制，引入 AI SDK 会增加一定的包体积
- 小程序需要配置服务器域名，配置完成后 AI SDK 才能进行请求

## 🔗 参考链接

- [初始化 SDK 文档](https://docs.cloudbase.net/ai/sdk-reference/init)
- [小程序接入云开发 AI 能力指引](https://docs.cloudbase.net/ai/miniprogram-using)
- [Agent API 文档](https://docs.cloudbase.net/ai/agent/sdk)
- [HTTP API 文档](https://docs.cloudbase.net/http-api/ai-bot/ai-agent-接入) 