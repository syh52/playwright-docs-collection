# CloudBase AI Agent 开发指南

## 📋 概述

Agent，或称为 AI Agent，是以大模型为基础，通过特定指令或指引，能够完成特定任务。Agent 利用 AI 大模型，具有强大的语言理解和生成能力，可以在各种领域执行复杂任务。

## 🚀 Agent 快速开始

### 第一步：新增Agent

进入 [云开发AI+](https://tcb.cloud.tencent.com/dev?#/ai?tab=agent)，选择agent模块，创建agent，按需选择对应的预设。

这里可以调整AI名称头像、修改AI人设、添加知识库、开场文案、开场问题、问题建议等功能。

完整 **Agent** 配置流程参考 [创建 Agent](https://docs.cloudbase.net/ai/agent/build)

### 第二步：创建AI应用

点击 **保存Agent**，然后点击 **确认** 前往AI+首页进行创建应用。

此时会自动打开 **AI+首页**，并选择好当前的agent实例。

点击安装Agent应用，则会跳转到可视化开发模块，并自动创建好了AI应用。

### 第三步：发布应用

点击右上角发布，选择 **发布到web端**，选择发布到小程序需要先绑定小程序，可以按提示步骤进行操作并发布。

完整 **Agent** 发布流程参考 [发布 Agent 应用](https://docs.cloudbase.net/ai/agent/release)

> **提示**
> 
> 小程序绑定流程：[小程序绑定流程](https://cloud.tencent.com/document/product/1301/86886)
> 
> 小程序发布相关问题：[小程序发布与部署问题](https://docs.cloudbase.net/ai/FAQ#发布与部署)

### 第四步：预览效果

发布完成后根据提供的链接或扫码进行访问：

- **PC端效果**：支持完整的对话界面、思考过程展示、文件上传等高级功能
- **H5及小程序效果**：移动端适配良好、保持一致的用户体验、支持微信生态集成

进行测试：
- 可以与Agent进行自然语言对话
- Agent会根据设定的人设和知识库回答问题
- 支持复杂的多轮对话

## 🎨 Agent UI 组件

Agent UI 组件是 CloudBase AI 提供的一套完整的聊天界面组件，可以帮助开发者快速在微信小程序、H5、Web 和微搭低代码平台中搭建 AI 对话界面。

### 核心功能

- **Agent 会话组件**：提供对接 Agent 服务的组件，支持对接 Agent 服务的会话能力
- **用户反馈**：提供用户反馈弹窗，用于提交用户反馈，优化 Agent 服务质量
- **多平台支持**：支持微信小程序、微搭低代码、React 等多种平台

## 🏗️ 微搭可视化组件集成

### 快速开始

1. **创建 Agent**
   - 在 Agent 管理页面，点击创建 Agent 按钮
   - 配置 Agent 的信息
   - 复制 Agent ID，用于 Agent UI 区块配置

2. **添加 Agent UI**
   - 在公共区块中搜索 Agent UI
   - 将 Agent UI 添加到当前页面

3. **配置 Agent ID**
   - 点击 Agent UI 区块
   - 配置 Agent ID（从 AI+ -> Agent -> 具体的 Agent 详情页面复制）

4. **发布应用**
   - 完成配置后，Agent UI 展示完整的聊天界面
   - 可参考[发布 Agent 应用文档](https://docs.cloudbase.net/ai/agent/release)

### 集成到微信小程序

#### 方法一：直接集成

1. **下载代码包**
   - 进入任意微搭应用
   - 在右侧区块中搜索 Agent UI
   - 下载代码包

2. **解压并添加到小程序**
   - 解压 Agent UI 代码包，得到 components 文件夹
   - 重命名为 agent_ui 并添加到小程序 miniprogram 目录中

3. **安装依赖**
   ```bash
   # 在 miniprogram 文件夹中执行
   npm install ./agent_ui
   ```

4. **构建 npm**
   - 在微信开发者工具中选择：工具 -> 构建 npm

5. **初始化 SDK**
   ```javascript
   // app.js
   import { init } from "@cloudbase/weda-client";
   
   init({
     envID: "xxx", // 云开发环境Id
     appConfig: {
       staticResourceDomain: "xxx.tcloudbaseapp.com", // 静态托管域名
     },
     customConfig: {
       async login({ app, auth, loginState, defaultLogin }) {
         await auth.signInWithOpenId();
       },
       loginConfig: { needSignIn: true },
     },
   });
   ```

6. **使用组件**
   - 在页面 JSON 中注册组件
   - 在页面 WXML 中添加组件（需要放在 id="wd-page-root" 的 view 下）
   - 在页面 JS 中配置 agent ID

#### 方法二：分包集成（推荐）

由于 Agent UI 依赖复杂，体积较大，推荐使用小程序子包形式引入。

1. **安装依赖包**
   ```bash
   # 在 agent_ui 目录下执行
   npm install
   ```

2. **配置多目录编译**
   在 `project.config.json` 中配置：
   ```json
   {
     "setting": {
       "packNpmManually": true,
       "packNpmRelationList": [
         {
           "packageJsonPath": "agent_ui/package.json",
           "miniprogramNpmDistDir": "agent_ui"
         }
       ]
     }
   }
   ```

3. **配置分包**
   在 `app.json` 中添加：
   ```json
   {
     "useExtendedLib": {
       "weui": true
     },
     "subpackages": [
       {
         "root": "agent_ui",
         "name": "agent-ui",
         "pages": []
       }
     ]
   }
   ```

## 📱 小程序源码组件

### 快速体验

**方式一：模板创建**
在微信开发者工具中通过模板创建项目，按照模板指引体验 Agent-UI。

**方式二：下载示例**
下载[项目示例源码包](https://gitee.com/TencentCloudBase/cloudbase-agent-ui/tree/main/apps/miniprogram-agent-ui)，导入到微信开发者工具使用。

### 集成到已有小程序

1. **拷贝组件**
   将 `miniprogram/components/agent-ui` 组件拷贝到小程序项目中

2. **注册组件**
   ```json
   // page.json
   {
     "usingComponents": {
       "agent-ui": "/components/agent-ui/index"
     }
   }
   ```

3. **使用组件**
   ```xml
   <!-- page.wxml -->
   <view>
     <agent-ui 
       agentConfig="{{agentConfig}}" 
       showBotAvatar="{{showBotAvatar}}" 
       chatMode="{{chatMode}}" 
       modelConfig="{{modelConfig}}" 
       envShareConfig="{{envShareConfig}}">
     </agent-ui>
   </view>
   ```

4. **配置参数**
   ```javascript
   // page.js
   data: {
     chatMode: "bot", // bot 表示使用agent，model 表示使用大模型
     showBotAvatar: true, // 是否在对话框左侧显示头像
     agentConfig: {
       botId: "bot-e7d1e736", // agent id
       allowWebSearch: true, // 允许客户端选择启用联网搜索
       allowUploadFile: true, // 允许上传文件
       allowPullRefresh: true, // 允许下拉刷新
       allowUploadImage: true, // 允许上传图片
       showToolCallDetail: true, // 是否展示工具调用细节
       allowMultiConversation: true, // 是否展示会话列表，创建会话按钮
       allowVoice: true, // 是否允许客户端界面展示语音按钮
     },
   }
   ```

## 🔧 Agent API 参考

### 基本使用

在调用 Agent 之前，需要进行 SDK 初始化。

```javascript
import cloudbase from "@cloudbase/js-sdk";

const app = cloudbase.init({
  env: "your-env-id"
});

await app.auth().signInAnonymously();
const ai = app.ai();
```

### 核心API

#### 1. 查看 Agent 列表

```javascript
const list = await ai.bot.list({
  pageNumber: 1,
  pageSize: 10
});
console.log(list);
```

#### 2. 与 Agent 对话

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

#### 3. 获取 Agent 信息

```javascript
const res = await ai.bot.get({ botId: "botId-xxx" });
console.log(res);
```

#### 4. 查看聊天记录

```javascript
const res = await ai.bot.getChatRecords({
  botId: "botId-xxx",
  pageNumber: 1,
  pageSize: 10,
  sort: "asc",
});
```

#### 5. 发送反馈

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

#### 6. 推荐问题

```javascript
const res = await ai.bot.getRecommendQuestions({
  botId: "botId-xxx",
  history: [{ content: "李白是谁", role: "user" }],
  msg: "你好",
});

for await (let str of res.textStream) {
  console.log(str);
}
```

#### 7. 文件上传

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

#### 8. 语音功能

```javascript
// 语音转文字
const res = await ai.bot.speechToText({
  botId: "botId-xxx",
  engSerViceType: "16k_zh",
  voiceFormat: "mp3",
  url: "https://example.com/audio.mp3",
});

// 文字转语音
const res = await ai.bot.textToSpeech({
  botId: "botId-xxx",
  voiceType: 1,
  text: "你好，我是AI助手",
});
```

## 📊 组件参数说明

### 通用参数

| 参数名称 | 参数类型 | 说明 |
|---------|---------|------|
| chatMode | 'bot' \| 'model' | 当 chatMode='bot'时，agentConfig.botId 必填；当 chatMode='model'时，modelConfig.modelProvider 和 modelConfig.quickResponseModel 必填 |
| showBotAvatar | boolean | 界面是否展示左侧头像 |

### Agent 配置参数

| 参数名称 | 参数类型 | 说明 |
|---------|---------|------|
| agentConfig.botId | string | agent id，当 chatMode = 'bot' 时，必填 |
| agentConfig.allowUploadFile | boolean | 界面是否展示文件上传 |
| agentConfig.allowWebSearch | boolean | 允许界面呈现联网配置开关 |
| agentConfig.allowPullRefresh | boolean | 允许下拉刷新 |
| agentConfig.allowUploadImage | boolean | 允许上传图片 |
| agentConfig.allowMultiConversation | boolean | 是否展示会话列表，创建会话按钮 |
| agentConfig.showToolCallDetail | boolean | 是否展示工具调用细节 |
| agentConfig.allowVoice | boolean | 是否允许客户端界面展示语音按钮 |

### 模型配置参数

| 参数名称 | 参数类型 | 说明 |
|---------|---------|------|
| modelConfig.modelProvider | 'hunyuan' \| 'deepseek' | 大模型服务商 |
| modelConfig.quickResponseModel | string | 具体的模型版本 |
| modelConfig.logo | string | model 头像 |
| modelConfig.welcomeMsg | string | model 欢迎语 |

## 🎯 最佳实践

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

### 3. 错误处理

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

## 🔍 功能特性

### 文件上传
- **大小限制**：单文件不超过 10M
- **数量限制**：单次最多支持 5 个文件
- **文件类型**：pdf、txt、doc、docx、ppt、pptx、xls、xlsx、csv

### 图片上传
- 每次仅支持上传单张图片，最大不超过 30MB

### 语音功能
- 若未授予小程序使用麦克风权限，组件会进行权限申请

### 大模型版本

#### Hunyuan 模型
| 模型版本 | 说明 |
|---------|------|
| hunyuan-lite | 具备强大的中文创作能力，复杂语境下的逻辑推理能力，以及可靠的任务执行能力 |

#### DeepSeek 模型
| 模型版本 | 说明 |
|---------|------|
| deepseek-v3 | 专注于自然语言处理、知识问答、内容创作等通用任务 |
| deepseek-r1 | 推理模型，专为数学、代码生成和复杂逻辑推理任务设计 |

## 🔧 环境共享

### 开通环境共享
如果 A 小程序想要使用 B 小程序云开发环境下的 AI 能力：
1. 在 B 小程序中开通环境共享
2. 将 B 小程序的云开发环境共享给 A 小程序使用
3. A 小程序和 B 小程序必须是同一个主体才能环境共享

### 配置环境共享参数
```javascript
{
  chatMode: "model",
  showBotAvatar: true,
  modelConfig: {
    modelProvider: "deepseek",
    quickResponseModel: "deepseek-v3",
    logo: "",
    welcomeMsg: "欢迎语",
  },
  envShareConfig: {
    resourceAppid: "wx7a******5f4f", // 资源方 AppID
    resourceEnv: "chr**************46d2d", // 资源方环境 ID
  },
}
```

## 🔗 相关链接

- [Agent 概述](https://docs.cloudbase.net/ai/agent/)
- [Agent 快速开始](https://docs.cloudbase.net/ai/agent/quickstart)
- [创建 Agent](https://docs.cloudbase.net/ai/agent/build)
- [发布 Agent 应用](https://docs.cloudbase.net/ai/agent/release)
- [Agent API](https://docs.cloudbase.net/ai/agent/sdk)
- [工具卡片开发指引](https://docs.cloudbase.net/ai/agent/toolCard)

## 高级功能

### 1. 自定义工具开发

**工具接口定义**：
```javascript
const customTool = {
  name: 'calculate',
  description: '数学计算工具',
  parameters: {
    type: 'object',
    properties: {
      expression: {
        type: 'string',
        description: '数学表达式'
      }
    },
    required: ['expression']
  },
  execute: async (params) => {
    // 工具执行逻辑
    return eval(params.expression);
  }
};
```

### 2. 插件系统

**插件注册**：
```javascript
// 注册自定义插件
ai.bot.registerPlugin({
  name: 'weatherPlugin',
  version: '1.0.0',
  functions: [weatherTool],
  middleware: [authMiddleware]
});
```

### 3. 事件系统

**事件监听**：
```javascript
// 监听Agent事件
ai.bot.on('message', (event) => {
  console.log('收到消息:', event.data);
});

ai.bot.on('error', (error) => {
  console.error('发生错误:', error);
});

ai.bot.on('toolCall', (tool) => {
  console.log('工具调用:', tool.name);
});
```

## 自定义 Artifact 解析

### 什么是 Artifact

通过 Artifact，用户可以轻松创建和管理多种类型的内容，如代码架构图、流程图、网页设计、SVG 图形和交互式组件。Artifact 特别适合开发人员、设计师、产品经理和营销人员，用于将创意快速转化为实际产品。

### 支持的 Artifact 类型

#### 1. Web 应用 Artifact
- 完整的 HTML/CSS/JavaScript 应用
- 交互式组件和界面
- 可视化预览和代码编辑

#### 2. Mermaid 图表 Artifact
- 流程图和架构图
- 时序图和状态图
- 实时渲染和编辑

### 如何实现 Artifact 功能

#### 1. 配置规范 Prompt

Artifact 解析依赖大模型输出规范的 artifact 内容结构，可通过 prompt 约束生成。

```text
你是 CloudBase 的 AI 助手，负责为用户生成各种类型的内容。

<artifact_info>
当需要生成专业内容时，请使用如下格式：

<cloudbaseArtifact id="unique-id" title="Title of the artifact" type="[content-type]">
<!-- Content goes here, format depends on the type -->
</cloudbaseArtifact>

支持的内容类型：

1. 用于 HTML/CSS/JS 应用 (type="code")：
<cloudbaseArtifact id="unique-id" title="Title of the code" type="code">
<!DOCTYPE html>
<html lang="en">
<!-- Complete HTML code here -->
</html>
</cloudbaseArtifact>

2. 用于 Mermaid 流程图 (type="mermaid")：
<cloudbaseArtifact id="unique-id" title="Title of the diagram" type="mermaid">
flowchart TD
    A[Start] --> B{Decision}
    B -->|Yes| C[Action]
    B -->|No| D[Another Action]
</cloudbaseArtifact>

重要说明：
- 必须提供完整且自包含的内容
- 对于 HTML 应用，所有 CSS 和 JavaScript 必须包含在同一个文件中
- 外部库请使用 CDN 链接，不要使用 npm 包
- Mermaid 流程图必须遵循正确语法
- 每个 artifact 必须有唯一的 ID、描述性标题和合适的 type
- 不要使用 Fenced Code Blocks 包裹 artifact 内容
</artifact_info>
```

#### 2. 实现 Artifact 组件

**Code Artifact 组件示例**：
```javascript
export const CodeArtifactComponent = ({ artifact }) => {
  const [isPreviewMode, setIsPreviewMode] = useState(false);

  const handlePreviewToggle = () => {
    setIsPreviewMode(!isPreviewMode);
  };

  return (
    <div className="cloudbase-artifact code-artifact">
      <div className="artifact-header">
        <h3>{artifact.title}</h3>
        <button onClick={handlePreviewToggle}>
          {isPreviewMode ? 'View Code' : 'Preview'}
        </button>
      </div>
      
      {isPreviewMode ? (
        <div className="preview-container">
          <iframe
            title={artifact.title}
            srcDoc={artifact.content}
            width="100%"
            height="400px"
            sandbox="allow-scripts allow-same-origin"
          />
        </div>
      ) : (
        <div className="code-container">
          <pre>
            <code>{artifact.content}</code>
          </pre>
          <button 
            className="copy-button" 
            onClick={() => navigator.clipboard.writeText(artifact.content)}
          >
            Copy
          </button>
        </div>
      )}
    </div>
  );
};
```

**Mermaid Artifact 组件示例**：
```javascript
export const MermaidArtifactComponent = ({ artifact }) => {
  const containerRef = useRef(null);

  useEffect(() => {
    if (containerRef.current) {
      mermaid.initialize({ startOnLoad: false });
      mermaid
        .render(`mermaid-${Date.now()}-${crypto.randomUUID()}`, artifact.content)
        .then(({ svg }) => {
          if (containerRef.current) {
            containerRef.current.innerHTML = svg;
            mermaid.run();
          }
        })
        .catch((error) => {
          console.error('Mermaid rendering error:', error);
          if (containerRef.current) {
            containerRef.current.innerHTML = 
              `<div class="error">Error rendering diagram: ${error.message}</div>`;
          }
        });
    }
  }, [artifact.content]);

  return (
    <div className="cloudbase-artifact mermaid-artifact">
      <div className="artifact-header">
        <h3>{artifact.title}</h3>
      </div>
      <div className="diagram-container" ref={containerRef}></div>
      <div className="code-container">
        <pre>
          <code>{artifact.content}</code>
        </pre>
        <button 
          className="copy-button" 
          onClick={() => navigator.clipboard.writeText(artifact.content || '')}
        >
          Copy
        </button>
      </div>
    </div>
  );
};
```

#### 3. React Agent UI 组件配置

在 extra 属性中传入 artifactMap 对象：

```jsx
<AgentUI
  tcb={tcb}
  chatMode="bot"
  showBotAvatar={true}
  agentConfig={{
    botId: import.meta.env.VITE_BOT_ID,
    allowWebSearch: true,
    allowUploadFile: true,
    allowPullRefresh: true,
    allowUploadImage: true,
    showToolCallDetail: true,
  }}
  modelConfig={{
    modelProvider: 'deepseek',
    quickResponseModel: 'deepseek-v3',
    deepReasoningModel: 'deepseek-r1',
  }}
  extra={{
    artifactMap: {
      code: CodeArtifactComponent,
      mermaid: MermaidArtifactComponent,
    },
  }}
/>
```

## 数据库集成

### 什么时候使用数据库

当开发者已经有小程序/web 应用，用户会通过小程序/web 产生数据，你希望 AI 可以理解这些数据，变成一个千人千面的智能体，同一个问题，每个用户都可以获得与他自身数据相关的答案。

### 数据库和知识库的对比

| 对比项 | 知识库 | 数据库 |
|--------|--------|--------|
| 数据类型 | docx/pdf/ppt/markdown | 表格/json |
| 数据更新频率 | 低频 | 高频 |
| 数据维护者 | 开发者 | 开发者 + C 端用户 |
| 典型应用 | 智能客服、问答系统、专家决策 | 用户行为分析、数据查询 |

### 如何配置数据模型

1. **新建数据模型**：
   - 在[云开发平台](https://tcb.cloud.tencent.com/dev)云数据库模块，新建数据模型
   - 选择云数据库（文档型）

2. **配置数据模型**：
   - 模型名称、字段名称都要使用中文来描述
   - 方便大模型理解用户问题和数据模型的关系

### Agent 如何绑定数据模型

在[云开发平台](https://tcb.cloud.tencent.com/dev)的 AI+ 模块：
1. 找到 Agent 管理页面
2. 选择需要绑定的 Agent
3. 在数据模型配置中绑定相应的数据模型

### 数据库查询对话

使用自然语言和 Agent 对话即可，如果问题里包含与数据库相关的信息，会自动触发查询数据库。

### 图书馆Agent示例

**数据模型设计**：
需要有 3 个数据模型：

1. **用户信息表**：
   - 权限：所有人可读，仅创建者及管理员可读写
   - 字段：ID、用户名称、用户年级

2. **书籍信息表**：
   - 权限：所有人可读，仅管理员可写
   - 字段：ID、书籍名称、书籍作者

3. **借阅记录表**：
   - 权限：仅创建者及管理员可读写
   - 字段：图书ID、借阅人ID、借阅时间

**查询示例**：
- 查询所有书籍：触发查询书籍信息表
- 查询借阅过的书：触发查询书籍信息表和借阅记录表
- 查询大一同学最喜欢的书：触发查询用户信息表、借阅记录表、书籍信息表

## 多会话管理

### 多会话功能概述

Agent 允许用户拥有多个相互独立的会话，不同会话间的上下文互不影响。

### 启用多会话

在云开发平台 AI+ 菜单 -> Agent 详情页 -> 多会话模式，启用多会话模式。

### 多会话集成方式

#### 1. Agent UI 组件

所有 Agent UI 组件都内置多会话能力：
- 可视化低码组件
- 小程序源码组件  
- React 组件

#### 2. SDK 集成

**初始化 SDK**：
```javascript
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'your-env', // 需替换为实际使用环境 id
});

const auth = app.auth();
await auth.signInAnonymously(); // 或者使用其他登录方式

const ai = app.ai();
```

**新建会话**：
```javascript
const res = await ai.bot.createConversation({
  botId: 'botId-xxx',
  title: '对话标题',
});
```

**发送消息时携带会话 ID**：
```javascript
const res = await ai.bot.sendMessage({
  botId: 'botId-xxx',
  history: [{ content: '你好', role: 'user' }],
  msg: '你好',
  conversationId: 'conversation-001',
});
```

**查询消息记录**：
```javascript
await ai.bot.getChatRecords({
  botId: 'botId-xxx',
  pageNumber: 1,
  pageSize: 10,
  sort: 'asc',
  conversationId: 'conversation-001',
});
```

**获取会话列表**：
```javascript
const res = await ai.bot.getConversation({
  botId: 'botId-xxx',
  pageSize: 10,
  pageNumber: 1,
});
```

**删除会话**：
```javascript
await ai.bot.deleteConversation({
  botId: 'botId-xxx',
  conversationId: 'conversation-001',
});
```

#### 3. HTTP API 集成

参考 [HTTP API 文档](https://docs.cloudbase.net/http-api/ai-bot/create-conversation) 进行集成。

## 故障排除

### 常见问题

1. **Agent响应缓慢**：
   - 检查网络连接
   - 优化prompt长度
   - 使用流式响应

2. **文件上传失败**：
   - 检查文件大小限制
   - 验证文件格式支持
   - 确认存储空间充足

3. **工具调用异常**：
   - 验证工具配置正确性
   - 检查API权限设置
   - 查看错误日志详情

### 调试技巧

1. **开启调试模式**：
```javascript
const ai = app.ai({
  debug: true,
  logLevel: 'verbose'
});
```

2. **监控API调用**：
```javascript
ai.bot.on('request', (req) => {
  console.log('API请求:', req);
});

ai.bot.on('response', (res) => {
  console.log('API响应:', res);
});
```

3. **错误日志记录**：
```javascript
ai.bot.on('error', (error) => {
  console.error('Agent错误:', error);
  // 发送错误报告到监控系统
});
```

## 总结

CloudBase AI Agent 提供了完整的AI助手开发解决方案，支持多种集成方式和丰富的功能特性。通过本指南，开发者可以快速上手Agent开发，并构建出高质量的AI应用。

关键要点：
- 合理配置Agent参数以获得最佳性能
- 使用知识库和数据库提供准确的领域知识
- 集成MCP工具扩展Agent能力
- 采用最佳实践确保用户体验
- 实现Artifact功能支持富媒体内容
- 利用多会话管理提升用户体验

通过持续优化和迭代，您的Agent将能够更好地服务用户需求。 