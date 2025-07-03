# CloudBase AI Agent API 参考文档

## 概述

CloudBase SDK 内置了 AI 模块，可以通过该模块调用 Agent，实现与 Agent 的对话。Agent API 提供了完整的智能体交互能力，包括对话、反馈、文件上传等功能。

**在调用 Agent 之前，需要进行 SDK 初始化。请参考[SDK 初始化文档](./sdk-init.md)进行操作。**

## 基本使用

### 1. 查看 Agent 列表

```javascript
const list = await ai.bot.list({
  pageNumber: 1,
  pageSize: 10
});
console.log(list);
```

在返回的 Agent 列表中，会展示 Agent 的 id、名称、设定等信息。在大部分 Agent 接口中，都是通过 Agent id 来指定要与哪个 Agent 进行交互。

### 2. 与 Agent 对话

```javascript
const res = await ai.bot.sendMessage({
  botId: "botId-xxx", // 填入 Agent id
  msg: "你好",
});

// 流式接口，逐步获取响应
for await (let str of res.textStream) {
  console.log(str);
}
```

通过简洁的流式接口，完成和 Agent 的对话交互。

## API 详细说明

### Bot.sendMessage()

与 Agent 进行对话的核心接口。

#### 使用示例

> **提示**：该接口为流式调用。

```javascript
const res = await ai.bot.sendMessage({
  botId: "botId-xxx",
  msg: "给我一个成语。",
});

for await (let str of res.textStream) {
  console.log(str);
}
// 输出示例：
// 当然
// 可以
// ！
// 这里
// 有一个
// 成语
// ：
// 
// 
// **
// 画
// 蛇
// 添
// 足
// **
```

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| botId | string | 是 | Agent ID |
| msg | string | 是 | 要发送的消息内容 |
| history | Array | 否 | 历史对话记录 |
| files | Array | 否 | 上传的文件 ID 数组 |

#### 返回值

| 属性名 | 类型 | 说明 |
|--------|------|------|
| textStream | AsyncIterable | 文本流，可通过 for await 遍历 |
| dataStream | AsyncIterable | 完整数据流，包含更多元数据 |

### Bot.get()

获取某个 Agent 的详细信息。

#### 使用示例

```javascript
const res = await ai.bot.get({ botId: "botId-xxx" });
console.log(res);
```

#### 返回值示例

```javascript
{
  "botId": "bot-809d4ad1",
  "name": "智能体",
  "agentSetting": "这是一个智能体",
  "introduction": "简介",
  "welcomeMessage": "欢迎欢迎",
  "avatar": "http://xxx.avatar.image",
  "background": "http://xxx.avatar.image",
  "isNeedRecommend": false,
  "knowledgeBase": [],
  "initQuestions": ["请问有什么可以帮你的嘛？"],
  "enable": true,
  "type": "text",
  "tags": []
}
```

### Bot.getChatRecords()

查看与 Agent 的聊天记录。

#### 使用示例

```javascript
const res = await ai.bot.getChatRecords({
  botId: "botId-xxx",
  pageNumber: 1,
  pageSize: 10,
  sort: "asc",
});
```

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| botId | string | 是 | Agent ID |
| pageNumber | number | 是 | 页码，从 1 开始 |
| pageSize | number | 是 | 每页数量 |
| sort | string | 否 | 排序方式：'asc' 或 'desc' |

#### 返回值示例

```javascript
{
  "recordList": [
    {
      "botId": "bot-809d4ad1",
      "recordId": "record-96617446",
      "role": "user",
      "content": "你好",
      "conversation": "r3Hjz3qgms3UG7z9lwSYbA",
      "type": "text",
      "triggerSrc": "TCB"
    },
    {
      "botId": "bot-809d4ad1",
      "recordId": "record-632906dd",
      "role": "assistant",
      "content": "您好！看起来您可能遇到了一些技术问题，或者只是想打个招呼。无论哪种情况，我都在这里为您提供帮助。如果您有任何具体的问题或需要协助，请随时告诉我！",
      "conversation": "r3Hjz3qgms3UG7z9lwSYbA",
      "type": "text",
      "triggerSrc": "TCB"
    }
  ],
  "total": 2
}
```

### Bot.sendFeedback()

对某一条聊天记录进行反馈。

#### 使用示例

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

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| userFeedback.botId | string | 是 | Agent ID |
| userFeedback.recordId | string | 是 | 聊天记录 ID |
| userFeedback.comment | string | 否 | 反馈评论 |
| userFeedback.rating | number | 否 | 评分（1-5） |
| userFeedback.tags | Array | 否 | 标签数组 |
| userFeedback.aiAnswer | string | 是 | AI 回答内容 |
| userFeedback.input | string | 是 | 用户输入内容 |
| userFeedback.type | string | 是 | 反馈类型：'upvote' 或 'downvote' |

### Bot.getFeedBack()

查看反馈记录。

#### 使用示例

```javascript
await ai.bot.getFeedBack({
  botId: "botId-xxx",
});
```

#### 返回值示例

```javascript
{
  "feedbackList": [
    {
      "comment": "dawd",
      "botId": "bot-809d4ad1",
      "aiAnswer": "ababa",
      "createTime": "2024-09-10T07:03:45.000Z",
      "input": "你不好",
      "rating": 3,
      "sender": "r3Hjz3qgms3UG7z9lwSYbA",
      "tags": [],
      "type": "downvote"
    }
  ],
  "total": 2
}
```

### Bot.getRecommendQuestions()

获取 Agent 推荐问题。

#### 使用示例

> **提示**：该接口为流式调用。

```javascript
const res = await ai.bot.getRecommendQuestions({
  botId: "botId-xxx",
  history: [{ content: "李白是谁", role: "user" }],
  msg: "你好",
  agentSetting: "",
  introduction: "",
  name: "",
});

for await (let str of res.textStream) {
  console.log(str);
}
// 输出示例：
// 你是
// 做什么
// 的
// 
// 你能
// 提供
// 哪些
// 服务
// 
// 怎么
// 使用
// 你的
// 功能
```

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| botId | string | 是 | Agent ID |
| history | Array | 否 | 历史对话记录 |
| msg | string | 是 | 当前消息 |
| agentSetting | string | 否 | Agent 设定 |
| introduction | string | 否 | Agent 介绍 |
| name | string | 否 | Agent 名称 |

### Bot.list()

查看 Agent 列表。

#### 使用示例

```javascript
const list = await ai.bot.list({
  pageNumber: 1, 
  pageSize: 10
});
console.log(list);
```

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| pageNumber | number | 是 | 页码，从 1 开始 |
| pageSize | number | 是 | 每页数量 |
| name | string | 否 | 按名称筛选 |
| enable | boolean | 否 | 按启用状态筛选 |
| information | string | 否 | 按信息筛选 |
| introduction | string | 否 | 按介绍筛选 |

#### 返回值示例

```javascript
{
  "botList": [
    {
      "botId": "bot-c033e89e",
      "name": "智能体",
      "agentSetting": "这是一个智能体",
      "introduction": "简介",
      "welcomeMessage": "欢迎欢迎",
      "avatar": "http://xxx.avatar.image",
      "background": "http://xxx.avatar.image",
      "isNeedRecommend": false,
      "knowledgeBase": [],
      "initQuestions": ["请问有什么可以帮你的嘛？"],
      "enable": true,
      "type": "text",
      "tags": []
    }
  ],
  "total": 1
}
```

## 文件处理功能

### Bot.uploadFiles()

将云存储中的文件上传至 Agent，用于进行文档聊天。

#### 使用示例

```javascript
// 1. 上传文件
await ai.bot.uploadFiles({
  botId: "botId-xxx",
  fileList: [{
    fileId: "cloud://xxx.docx",
    fileName: "xxx.docx",
    type: "file",
  }],
});

// 2. 进行文档聊天
const res = await ai.bot.sendMessage({
  botId: "your-bot-id",
  msg: "这个文件的内容是什么",
  files: ["xxx.docx"], // 文件 fileId 数组
});

for await (let text of res.textStream) {
  console.log(text);
}
```

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| botId | string | 是 | Agent ID |
| fileList | Array | 是 | 文件列表 |
| fileList[].fileId | string | 是 | 云存储文件 ID |
| fileList[].fileName | string | 是 | 文件名 |
| fileList[].type | string | 是 | 文件类型 |

## 语音功能

### Bot.speechToText()

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

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| botId | string | 是 | Agent ID |
| engSerViceType | string | 是 | 语音识别服务类型 |
| voiceFormat | string | 是 | 语音格式 |
| url | string | 是 | 语音文件 URL |

### Bot.textToSpeech()

文字转语音。

#### 使用示例

```javascript
const res = await ai.bot.textToSpeech({
  botId: "botId-xxx",
  voiceType: 1,
  text: "你好，我是AI助手",
});
```

#### 参数说明

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| botId | string | 是 | Agent ID |
| voiceType | number | 是 | 语音类型 |
| text | string | 是 | 要转换的文本 |

## 错误处理

### 常见错误类型

1. **Agent 不存在**
   ```javascript
   {
     "error": "Agent not found",
     "code": "AGENT_NOT_FOUND"
   }
   ```

2. **权限不足**
   ```javascript
   {
     "error": "Permission denied",
     "code": "PERMISSION_DENIED"
   }
   ```

3. **参数错误**
   ```javascript
   {
     "error": "Invalid parameters",
     "code": "INVALID_PARAMS"
   }
   ```

### 错误处理示例

```javascript
try {
  const res = await ai.bot.sendMessage({
    botId: "botId-xxx",
    msg: "你好",
  });
  
  for await (let str of res.textStream) {
    console.log(str);
  }
} catch (error) {
  console.error('Agent 调用失败:', error);
  
  // 根据错误类型进行处理
  switch (error.code) {
    case 'AGENT_NOT_FOUND':
      console.log('Agent 不存在，请检查 botId');
      break;
    case 'PERMISSION_DENIED':
      console.log('权限不足，请检查账号权限');
      break;
    default:
      console.log('未知错误:', error.message);
  }
}
```

## 最佳实践

### 1. 会话管理

```javascript
// 维护会话历史
let conversationHistory = [];

const sendMessageWithHistory = async (message) => {
  // 添加用户消息到历史
  conversationHistory.push({
    role: 'user',
    content: message
  });

  const res = await ai.bot.sendMessage({
    botId: "botId-xxx",
    msg: message,
    history: conversationHistory
  });

  let assistantMessage = '';
  for await (let str of res.textStream) {
    assistantMessage += str;
    console.log(str);
  }

  // 添加助手回复到历史
  conversationHistory.push({
    role: 'assistant',
    content: assistantMessage
  });
};
```

### 2. 流式响应处理

```javascript
const handleStreamResponse = async (botId, message) => {
  const res = await ai.bot.sendMessage({
    botId,
    msg: message,
  });

  let fullResponse = '';
  const responseElement = document.getElementById('response');

  for await (let chunk of res.textStream) {
    fullResponse += chunk;
    // 实时更新界面
    responseElement.textContent = fullResponse;
  }

  return fullResponse;
};
```

### 3. 文件处理

```javascript
const handleFileChat = async (botId, fileId, question) => {
  // 先上传文件
  await ai.bot.uploadFiles({
    botId,
    fileList: [{
      fileId,
      fileName: "document.pdf",
      type: "file",
    }],
  });

  // 然后基于文件进行对话
  const res = await ai.bot.sendMessage({
    botId,
    msg: question,
    files: [fileId],
  });

  for await (let text of res.textStream) {
    console.log(text);
  }
};
```

### 4. 反馈收集

```javascript
const collectFeedback = async (botId, recordId, isPositive, comment = '') => {
  await ai.bot.sendFeedback({
    userFeedback: {
      botId,
      recordId,
      comment,
      rating: isPositive ? 5 : 1,
      type: isPositive ? 'upvote' : 'downvote',
      // ... 其他必需字段
    },
  });
};
```

## 总结

Agent API 提供了完整的智能体交互能力，支持：

- **基础对话**：文本对话、流式响应
- **会话管理**：历史记录、推荐问题
- **文件处理**：文档上传、文档聊天
- **语音功能**：语音转文字、文字转语音
- **反馈系统**：用户反馈、反馈查询

通过合理使用这些 API，可以构建功能完整的 AI 应用，为用户提供优质的智能对话体验。 