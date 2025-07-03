# CloudBase AI Agent UI 组件完整指南

## 概述

Agent UI 组件是 CloudBase AI 提供的一套完整的聊天界面组件，可以帮助开发者快速在微信小程序、H5、Web 和微搭低代码平台中搭建 AI 对话界面。

Agent UI 提供以下核心功能：
- **Agent 会话组件**：提供对接 Agent 服务的组件，支持对接 Agent 服务的会话能力
- **用户反馈**：提供用户反馈弹窗，用于提交用户反馈，优化 Agent 服务质量
- **多平台支持**：支持微信小程序、微搭低代码、React 等多种平台

## 微搭可视化组件

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

4. **初始化 SDK**
   有两种选择：
   - **分包中初始化**：在 agent_ui 目录下创建 index.js，导入并初始化 SDK
   - **主包中初始化**：在 app.js 的 onLaunch 生命周期内异步初始化

5. **使用组件**
   ```json
   // page.json
   {
     "usingComponents": {
       "chat": "/agent-ui/dist/Agent-UI/index"
     },
     "componentPlaceholder": {
       "chat": "block"
     }
   }
   ```

   ```xml
   <!-- page.wxml -->
   <view id="wd-page-root">
     <chat bot="{{xxx}}" />
   </view>
   ```

### 小程序合法域名配置

使用 Agent UI 时，需要配置小程序合法域名：
- 将相关域名添加到小程序管理后台的 request 合法域名列表
- 如果是授权给微搭的小程序，需要先解绑、配置域名后再重新绑定

## 小程序源码组件

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
     chatMode: "model", // bot 表示使用agent，model 表示使用大模型
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
     modelConfig: {
       modelProvider: "hunyuan-open", // 大模型服务厂商
       quickResponseModel: "hunyuan-lite", // 大模型名称
       logo: "", // model 头像
       welcomeMsg: "欢迎语", // model 欢迎语
     },
   }
   ```

5. **初始化 SDK**
   ```javascript
   // app.js
   App({
     onLaunch: function () {
       wx.cloud.init({
         env: "<云开发环境ID>",
       });
     },
   })
   ```

## 组件参数说明

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

### 环境共享配置

| 参数名称 | 参数类型 | 说明 |
|---------|---------|------|
| envShareConfig.resourceAppid | string | 环境共享的资源方 AppID |
| envShareConfig.resourceEnv | string | 环境共享的资源方环境 ID |

## 功能特性说明

### 文件上传
- **大小限制**：单文件不超过 10M
- **数量限制**：单次最多支持 5 个文件
- **文件类型**：pdf、txt、doc、docx、ppt、pptx、xls、xlsx、csv
- **域名配置**：需要添加文件上传接口到 request 合法域名列表

### 图片上传
- 每次仅支持上传单张图片，最大不超过 30MB

### 语音功能
- 若未授予小程序使用麦克风权限，组件会进行权限申请

## 使用示例

### 使用 deepseek-v3 模型
```javascript
{
  "chatMode": "model",
  "showBotAvatar": true,
  "modelConfig": {
    "modelProvider": "deepseek",
    "quickResponseModel": "deepseek-v3",
    "logo": "",
    "welcomeMsg": "欢迎语"
  }
}
```

### 使用 deepseek-r1 模型
```javascript
{
  chatMode: "model",
  showBotAvatar: true,
  modelConfig: {
    modelProvider: "deepseek",
    quickResponseModel: "deepseek-r1",
    logo: "",
    welcomeMsg: "欢迎语",
  },
}
```

### 使用 Agent
```javascript
{
  chatMode: "bot",
  showBotAvatar: true,
  agentConfig: {
    botId: "bot-e7d1e736",
    allowWebSearch: true,
    allowUploadFile: true,
    allowPullRefresh: true,
    allowUploadImage: true,
    showToolCallDetail: true,
    allowMultiConversation: true,
    allowVoice: true,
  },
}
```

## 大模型版本

### Hunyuan 模型
| 模型版本 | 说明 |
|---------|------|
| hunyuan-lite | 具备强大的中文创作能力，复杂语境下的逻辑推理能力，以及可靠的任务执行能力 |

### DeepSeek 模型
| 模型版本 | 说明 |
|---------|------|
| deepseek-v3 | 专注于自然语言处理、知识问答、内容创作等通用任务 |
| deepseek-r1 | 推理模型，专为数学、代码生成和复杂逻辑推理任务设计 |

## 环境共享

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

## 自定义 MCP 工具卡片

可以参考[工具卡片开发指引](https://docs.cloudbase.net/ai/agent/toolCard)来开发自定义的 MCP 工具卡片。

## 最佳实践

1. **合理选择组件类型**
   - 简单场景使用微搭可视化组件
   - 需要深度定制使用小程序源码组件
   - 大型项目推荐分包集成

2. **性能优化**
   - 使用分包降低主包体积
   - 合理配置组件参数，避免不必要的功能
   - 及时处理合法域名配置

3. **用户体验**
   - 根据业务场景配置合适的欢迎语
   - 开启推荐问题功能引导用户
   - 合理使用语音和文件上传功能

4. **安全考虑**
   - 正确配置环境共享权限
   - 定期检查合法域名配置
   - 及时更新组件版本 