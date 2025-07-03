# 通过 API 调用

前往[云开发平台/环境配置/API Key 配置页面](https://tcb.cloud.tencent.com/dev#/env/apikey)即可新建 API Key。

获取到 API Key 后，即可进行 HTTP API 调用，只需要在 HTTP 请求中附带 `"Authorization: Bearer <您的 API Key>"` 请求头。

可用的 HTTP API 包括：

- [大模型 HTTP API](/http-api/ai-model/call-llm)
- [AI Agent HTTP API](/http-api/ai-bot/send-message)
- [知识库 HTTP API](/http-api/knowledge/search)

## 大模型调用

### cURL 示例

以下是一个使用 cURL 调用大模型 HTTP API 的示例：

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

### OpenAI SDK 示例

获取到 API Key 后，也可以使用 OpenAI SDK 访问大模型服务，只需要替换 `baseURL` 及 `apiKey` 即可，以下是一个示例：

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

## AI Agent 调用

### cURL 示例

以下是一个使用 cURL 调用 AI Agent HTTP API 的示例：

```bash
curl 'https://<your-envId>.api.tcloudbasegateway.com/v1/aibot/bots/<your-botId>/send-message' \
  -H 'Authorization: Bearer <您的 API Key>' \
  -H 'Accept: text/event-stream' \
  -H 'Content-Type: application/json' \
  --data-raw '{"msg":"hi"}'
```

## 知识库调用

### cURL 示例

以下是一个使用 cURL 调用 知识库 HTTP API 的示例：

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

## 主要特性

- **兼容 OpenAI SDK**：支持使用标准的 OpenAI SDK 进行调用
- **流式输出**：支持实时流式响应
- **多种调用方式**：支持 cURL、各种编程语言的 HTTP 客户端
- **统一认证**：使用 API Key 进行统一身份验证

## 适用场景

- **第三方系统集成**：将 CloudBase AI 能力集成到现有系统
- **自动化脚本**：在批处理、定时任务中使用 AI 能力
- **多语言支持**：在各种编程语言中调用 AI 服务
- **服务端应用**：在后端服务中直接调用 AI API

## 注意事项

- API Key 需要妥善保管，避免泄露
- 请求需要包含正确的认证头
- 支持流式和非流式两种响应模式
- 不同的 API 具有不同的请求参数和响应格式 