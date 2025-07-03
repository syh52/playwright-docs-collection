# CloudBase AI MCP 完整指南

## 什么是 MCP

MCP 是一个开放协议，它标准化了 AI 应用如何向大语言模型（LLMs）提供上下文。可以把 MCP 想象成 AI 应用的 USB-C 接口。就像 USB-C 为设备连接各种外设和配件提供了标准化方式一样，MCP 为 AI 模型连接不同的数据源和工具提供了标准化方式。

MCP 的核心采用 Client-Server 架构，一个应用程序可以连接多个 MCP Server。

只要 AI 应用实现了 MCP，即可接入到任意的 MCP Server，扩展自身的能力。加入 MCP Server 后，工具调用的流程如下：

```
👨‍💻 AI 应用 --> 🔌 MCP Server --> 🧠 大模型
    |                    |              |
    |<---工具列表--------|              |
    |                    |              |
    |-------Prompt + 可用工具列表------->|
    |                    |              |
    |<-------工具调用请求---------------|
    |                    |              |
    |----转发工具调用请求->|              |
    |                    |              |
    |<---工具调用结果-----|              |
    |                    |              |
    |-------工具调用结果---------------->|
    |                    |              |
    |<-------文本响应-------------------|
```

通过这种方式，MCP 实现了：

- **即插即用**：AI 应用只需实现 MCP 协议，即可接入丰富的第三方工具生态
- **标准化**：所有工具遵循统一的描述格式和调用方式
- **解耦工具与应用**：工具提供者可以独立开发和维护工具，不需了解 AI 应用内部实现
- **资源共享**：一次开发的工具可以被多个 AI 应用复用

## 在 Agent 中使用 MCP

1. 请确保已经有一个 Agent，可 [前往云开发平台创建](https://tcb.cloud.tencent.com/dev#/ai?tab=agent)

2. 在 Agent 中添加 MCP Server 选择第一步安装的 MCP 服务，并选择需要的工具，并保存

3. 和 AI 进行对话，例如要求 AI 使用工具完成某个功能，当 Agent 使用了 MCP 执行任务时，会提示正在执行某个工具

## 开发第一个 MCP Server

> 推荐使用 Node.js 18 以上的版本

本文将逐步说明如何在云开发平台上开发一个 MCP Server，包含以下几个步骤：

- 用模板创建出 MCP Server 云托管服务
- 使用云开发 MCP 框架开发 MCP Server
- 使用图形界面本地调试 MCP Server
- 部署至线上，覆盖原有云托管服务
- 在 Agent 中使用 MCP Server

### 创建空白 MCP Server

前往 [云开发平台](https://tcb.cloud.tencent.com/dev#/ai?tab=mcp) 创建一个空白 MCP Server，设置标识为 `first-mcp`。

> 该步骤会自动创建一个云托管服务承载 MCP Server，在后文中我们将会自己开发一个 MCP Server 并覆盖该云托管服务。

### 准备代码

我们提供了一个基本的 MCP Server 模板，包含了一个完整的 MCP Server 项目。可以以此项目为基础进行修改，开发您自己的 MCP Server。

本文将会介绍如何使用此项目代码进行本地开发调试以及部署。

[点击此处下载项目代码](https://tcb.cloud.tencent.com/cloud-run-function-template/cloudrun-mcp-basic.zip?v=2025)

### 安装依赖

下载项目代码，解压后进入项目目录，通过命令行执行如下命令，安装依赖：

```bash
npm i
```

### 为 MCP Server 添加第一个工具

修改 `src/server.ts` 中的实现，实现一个获取用户访问信息的工具，该工具会返回最近 count 条用户访问信息。

也可以使用模板自带的工具，直接进入下一步。

```typescript
import { ContextInjected, TcbExtendedContext } from '@cloudbase/functions-typings';
import { McpServer } from '@modelcontextprotocol/sdk/server/mcp.js';
import tcb from '@cloudbase/node-sdk';
import { z } from 'zod';

export function createServer(context: ContextInjected<TcbExtendedContext>) {
  const server = new McpServer(
    {
      name: 'Basic Server',
      version: '1.0.0',
    },
    { capabilities: { tools: {} } },
  );

  const env = context.extendedContext?.envId || process.env.CLOUDBASE_ENVIRONMENT; // 本地开发从环境变量读
  const secretId = context.extendedContext?.tmpSecret?.secretId;
  const secretKey = context.extendedContext?.tmpSecret?.secretKey;
  const sessionToken = context.extendedContext?.tmpSecret?.token;

  // 创建 Cloudbase Node sdk 实例
  const app = tcb.init({
    env,
    secretId,
    secretKey,
    sessionToken,
  });

  server.registerTool(
    'getUserVisitInfo',
    {
      description: '获取最新的用户访问信息',
      // 工具的入参
      inputSchema: {
        count: z.number().describe('获取最近 {count} 条用户访问信息'),
      },
      // 工具的出参
      outputSchema: {
        total: z.number().describe('数据模型中总共的用户访问信息数量'),
      },
    },
    async ({ count }) => {
      const res = await app.models.sys_user_dau.list({
        pageSize: count,
        getCount: true,
        orderBy: [
          {
            visit_time: 'desc',
          },
        ],
      });

      const structuredContent = {
        total: res.data.total,
        records: res.data.records.map((x) => ({
          device: x.device,
          visitTime: x.visit_time,
        })),
      };

      return {
        content: [
          {
            type: 'text',
            text: JSON.stringify(structuredContent),
          },
        ],
        structuredContent,
      };
    },
  );

  // todo
  // 可以添加更多工具

  return { server };
}
```

### 本地调试 MCP Server

#### 添加环境变量文件

新建一个 `.env.development` 文件，填入以下内容:

```
SKIP_VERIFY_ACCESS=true
```

该文件定义了环境变量：

- `SKIP_VERIFY_ACCESS=true`：设置后可跳过 token 校验。原有 token 校验将只允许来自 API Key 和超管身份的 token 调用

> **提示**
> 设置 `SKIP_VERIFY_ACCESS` 这环境变量有助于我们在本地进行调试，但不建议在线上生产环境设置。

#### 启动本地 MCP Server

```bash
npm run login # 登录云开发
npm run dev
```

启动后，将会在 [http://localhost:3000/messages](http://localhost:3000/messages) 提供服务。

修改代码，即可触发重新编译并重启服务。

#### 启动图形界面调试

开启另一个终端，启动 `@modelcontextprotocol/inspector`:

```bash
npx -y @modelcontextprotocol/inspector
```

根据终端输出，访问调试地址，即可看到调试页面了。

- 在左侧选择 `Streamable HTTP` 类型，并填入 `URL` 为 `http://localhost:3000/messages`
- 在左侧点击 `Connect`
- 在 `Tools` tab 下点击 `List Tools` 展示工具列表
- 选择任一工具进行调用

### 构建

开发调试完成后，在项目根目录运行构建命令:

```bash
npm run build
```

### 部署

构建完成后，运行部署命令:

```bash
npm run login
npm run deploy
```

选择环境后，服务名称输入前面创建 MCP 服务时候指定的服务标识，例如 `first-mcp`。

### 使用线上的 MCP Server

部署后，即可阅读以下文档使用 MCP Server 了:

- [在云开发 Agent 中使用](https://docs.cloudbase.net/ai/mcp/use/agent)
- [在 MCP Host 中使用](https://docs.cloudbase.net/ai/mcp/use/mcp-host)
- [通过 SDK 接入](https://docs.cloudbase.net/ai/mcp/use/sdk)

### 问题排查

#### 本地调试时，MCP Server 无法连接

检查本地环境变量是否正确设置，检查本地端口是否被占用。

可以在 `inspector` 界面中开启 F12 打开浏览器的调试工具查看网络请求

查看类似 `http://localhost:6277/mcp?url=https%3A%2F%2Flowcode-2gp2855c5ce22e35.api.tcloudbasegateway.com%2Fv1%2Fcloudrun%2Fmcp-kuaidi100-qt03oc%2Fmessages&transportType=streamable-http` 这个请求返回的响应，一般如果连接不成功，这里都会有对应的错误信息

## 云开发提供的 MCP 支持

云开发提供了丰富的 MCP 支持：

- [MCP 使用](https://docs.cloudbase.net/ai/mcp/use/agent)
- [MCP Server 开发](https://docs.cloudbase.net/ai/mcp/develop/)
- [MCP Server 托管](https://tcb.cloud.tencent.com/dev?#/ai?tab=mcp)

## 参考链接

- [MCP 概述](https://docs.cloudbase.net/ai/mcp/introduce)
- [在 Agent 中使用 MCP](https://docs.cloudbase.net/ai/mcp/use/agent)
- [在 MCP Host 中使用](https://docs.cloudbase.net/ai/mcp/use/mcp-host)
- [通过 SDK 调用 MCP](https://docs.cloudbase.net/ai/mcp/use/sdk)
- [开发第一个 MCP Server](https://docs.cloudbase.net/ai/mcp/develop/)
- [托管现有的 MCP Server](https://docs.cloudbase.net/ai/mcp/develop/host-mcp) 