# CloudBase AI SDK API 参考文档

## 概述

CloudBase AI SDK 提供了完整的 AI 开发能力，包括大模型调用、Agent 交互、工具集成等功能。

## 初始化 SDK

SDK 初始化方法请参考 [初始化 SDK 文档](./初始化%20SDK.md)。

## app.ai

初始化后，可以使用挂载至 cloudbase 实例上的 ai 方法创建 AI 实例，用于后续模型创建。

### 使用示例

```javascript
app = cloudbase.init({"env": "your-env"});
const ai = app.ai();
```

### 类型声明

```typescript
function ai(): AI;
```

### 返回值

返回新创建的 AI 实例。

## AI 类

用于创建 AI 模型的类。

### createModel()

创建指定的 AI 模型。

#### 使用示例

```javascript
const model = ai.createModel("hunyuan-exp");
```

#### 类型声明

```typescript
function createModel(model: string): ChatModel;
```

返回一个实现了 ChatModel 抽象类的模型实例，该实例提供 AI 生成文本相关能力。

### bot

挂载了 Bot 类的实例，上面集合了一系列与 Agent 交互的方法。

#### 使用示例

```javascript
const agentList = await ai.bot.list({pageNumber: 1, pageSize: 10});
```

### registerFunctionTool()

注册函数工具。在进行大模型调用时，可以告知大模型可用的函数工具，当大模型的响应被解析为工具调用时，会自动调用对应的函数工具。

#### 使用示例

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

#### 类型声明

```typescript
function registerFunctionTool(functionTool: FunctionTool);
```

#### 参数

| 参数名 | 必填 | 类型 | 说明 |
|--------|------|------|------|
| functionTool | 是 | FunctionTool | 详见 FunctionTool 类型定义 |

## ChatModel 类

这个抽象类描述了 AI 生文模型类提供的接口。

### generateText()

调用大模型生成文本。

#### 使用示例

```javascript
const hy = ai.createModel("hunyuan-exp");
const res = await hy.generateText({
  model: "hunyuan-lite",
  messages: [{role: "user", content: "你好，请你介绍一下李白"}],
});
console.log(res.text);
```

#### 类型声明

```typescript
function generateText(data: BaseChatModelInput): Promise<{
  rawResponses: Array<unknown>;
  text: string;
  messages: Array<ChatModelMessage>;
  usage: Usage;
  error?: unknown;
}>;
```

#### 参数

| 参数名 | 必填 | 类型 | 示例 | 说明 |
|--------|------|------|------|------|
| data | 是 | BaseChatModelInput | `{model: "hunyuan-lite", messages: [{ role: "user", content: "你好" }]}` | 参数类型定义为 BaseChatModelInput，作为基础的入参定义 |

#### 返回值

| 属性名 | 类型 | 示例 | 说明 |
|--------|------|------|------|
| text | string | "李白是一位唐朝诗人。" | 大模型生成的文本 |
| rawResponses | unknown[] | 大模型返回值数组 | 大模型的完整返回值 |
| messages | ChatModelMessage[] | 完整消息列表 | 本次调用的完整消息列表 |
| usage | Usage | `{"completion_tokens":33,"prompt_tokens":3,"total_tokens":36}` | 本次调用消耗的 token |
| error | unknown | - | 调用过程中产生的错误 |

### streamText()

以流式调用大模型生成文本。

#### 使用示例

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

## Bot 类

用于与 Agent 交互的类。

### get()

获取某个 Agent 的信息。

#### 使用示例

```javascript
const res = await ai.bot.get({botId: "botId-xxx"});
console.log(res);
```

### list()

批量获取多个 Agent 的信息。

#### 使用示例

```javascript
await ai.bot.list({
  pageNumber: 1,
  pageSize: 10,
  name: "",
  enable: true,
  information: "",
  introduction: "",
});
```

### sendMessage()

与 Agent 进行对话。

#### 使用示例

```javascript
const res = await ai.bot.sendMessage({
  botId: "botId-xxx",
  history: [{content: "你是李白。", role: "user"}],
  msg: "你好",
});

for await (let str of res.dataStream) {
  console.log(str);
}
```

### getChatRecords()

获取聊天记录。

#### 使用示例

```javascript
await ai.bot.getChatRecords({
  botId: "botId-xxx",
  pageNumber: 1,
  pageSize: 10,
  sort: "asc",
});
```

### sendFeedback()

发送对某条聊天记录的反馈信息。

#### 使用示例

```javascript
const res = await ai.bot.sendFeedback({
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
  botId: "botId-xxx",
});
```

### uploadFiles()

将云存储中的文件上传至 Agent，用于进行文档聊天。

#### 使用示例

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

### speechToText()

语音转文字。

#### 使用示例

```javascript
const res = await ai.bot.speechToText({
  botId: "botId-xxx",
  engSerViceType: "16k_zh",
  voiceFormat: "mp3",
  url: "https://example.com/audio.mp3",
});
```

### textToSpeech()

文字转语音。

#### 使用示例

```javascript
const res = await ai.bot.textToSpeech({
  botId: "botId-xxx",
  voiceType: 1,
  text: "你好，我是AI助手",
});
```

## 类型定义

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

## 最佳实践

1. **模型选择**: 根据不同场景选择合适的模型，如 `hunyuan-lite` 适合轻量级对话，`hunyuan-exp` 适合复杂任务
2. **工具注册**: 合理设计工具的描述和参数，帮助大模型更好地理解工具用途
3. **错误处理**: 注意检查返回值中的 `error` 字段，做好异常处理
4. **流式调用**: 对于长文本生成，推荐使用 `streamText()` 提供更好的用户体验
5. **Agent 交互**: 合理利用历史记录和文件上传功能，提升对话效果 