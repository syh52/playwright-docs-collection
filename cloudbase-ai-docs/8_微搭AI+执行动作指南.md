# 微搭 AI+ 执行动作指南

## 概述

微搭中集成了具有 AI+ 能力的执行动作，帮助开发者快速搭建具有 AI+ 能力的低代码应用。我们提供了以下执行动作：

- **调用大模型对话**：调用大模型，进行对话，支持流式和非流式调用
- **调用 Agent 对话**：调用 Agent 进行流式对话

支持一键将生成的文本赋值到变量中，并提供处理完整 SSE 事件的能力。

## 使用示例

### 调用大模型对话

#### 1. 配置大模型

首先，进入[云开发 AI+ 配置](https://tcb.cloud.tencent.com/dev#/ai?tab=ai-model)配置我们想用的大模型。这里我们选择配置混元(Open)。

#### 2. 搭建页面

接下来，我们进入到可视化开发页面：

1. **创建变量**：在本页面中新建一个字符串变量 `text`，并新建一个文本组件，将文本组件的内容设置为 `"大模型生成内容：" + $w.page.dataset.state.text`。我们会把大模型的输出结果保存到此变量中。

2. **添加按钮**：在页面中添加一个按钮，点击右侧菜单中的点击事件，即可唤出执行动作面板。

3. **配置执行动作**：点击「调用大模型对话」，进入执行动作的参数配置面板：

   - **模型供应商**：设置为 `hunyuan-open`，对应我们刚刚配置的大模型
   - **模型**：设置为 `hunyaun-lite`
   - **对话消息列表**：添加一条消息，角色为 `user`，意为这是我们对大模型发送的消息
   - **流式调用**：选中「流式」，开启流式调用。后续调用大模型时，每当大模型有新的内容生成，我们就能够接收到响应
   - **temperature 温度参数**：控制大模型生成文本的多样性，建议参考具体使用的大模型文档详细了解后再按需使用
   - **top_p 核采样参数**：同 temperature 温度参数，建议根据需要设置
   - **文本赋值变量**：选中刚刚新建的 `text` 变量，当我们获取到大模型的生成文本时，会将文本保存到此变量中
   - **SSE 回调事件流**：当接收到 SSE 流式调用时，会触发此事件流

#### 3. 进阶：配置 SSE 回调事件流

大模型和 Agent 的流式调用都是基于 SSE 实现的。当大模型生成了新的内容时，就会返回一个 SSE 事件。在 SSE 事件中，包含着这次生成的详细信息，包括生成内容、时间、ID 等等。

**配置步骤**：

1. 在左侧代码区新建一个 `print_SSE` 的事件流，设置为 Javascript 代码事件，并填入这段代码：

```javascript
// 这段代码会打印出接收到的 SSE 事件。
({ event }) => {
  console.log(event.detail);
}
```

2. 在「调用大模型对话」的参数配置面板中，在「SSE 回调事件流」参数中，添加刚刚新建的 `print_SSE` 事件流。

3. 打开浏览器的开发者控制台，再次点击按钮，即可调用大模型，生成的文本显示在应用界面中，同时控制台也打印出了接收到的一系列 SSE 事件。

### 调用 Agent 对话

#### 1. 配置 Agent

首先，进入[云开发 AI+ 配置](https://tcb.cloud.tencent.com/dev#/ai?tab=agent)配置我们想用的 Agent，点击复制 ID 按钮记录下来该 Agent 的 ID。

#### 2. 搭建页面

接下来，我们进入到可视化开发页面：

1. **创建变量**：在本页面中新建一个字符串变量 `text`，并新建一个文本组件，将文本组件的内容设置为 `"Agent 生成内容：" + $w.page.dataset.state.text`。

2. **添加按钮**：在页面中添加一个按钮，点击右侧菜单中的点击事件，即可唤出执行动作面板。

3. **配置执行动作**：点击「调用 Agent 对话」，进入执行动作的参数配置面板：

   - **Agent ID**：填入刚刚在 AI+ 页面中配置好的 Agent ID
   - **消息内容**：填入要发送给 Agent 的对话内容
   - **历史对话记录**：如果有添加上下文的需求，可以在此填入你和 Agent 的历史对话列表
   - **文本赋值变量**：选中刚刚新建的 `text` 变量，当我们获取到 Agent 的生成文本时，会将文本保存到此变量中
   - **SSE 回调事件流**：当接收到 SSE 流式调用时，会触发此事件流。可以参考调用大模型对话中的使用示例进行使用

配置完成后，点击保存。在页面中点击按钮，即可调用 Agent，并看到 Agent 生成的文本在页面上被打印出来了。

## 最佳实践

### 在应用中体现「加载中」的状态

调用大模型、Agent 对话时，需要耗费一定时间才能看到响应。为了让用户感知到应用正在等待调用完成、防止多次调用造成赋值混乱等现象，我们可以在应用中体现出「加载中」的状态。

#### 实现步骤

1. **基础配置**：按照调用大模型对话使用示例，搭建出一个能够调用大模型的应用。页面上应该有：
   - 一个文本组件
   - 一个按钮组件  
   - 一个 `text` 文本变量

2. **创建加载状态变量**：新建一个 `loading` 变量，类型为布尔值，默认值设置为 `false`，这个值会用来表示我们的「加载中」状态。

3. **配置按钮状态**：在按钮组件的高级属性栏下，找到「加载中」和「禁用」的属性，点击切换为表达式，绑定上我们刚刚新建的 `loading` 变量。

4. **设置状态切换**：
   - 在调用大模型之前给 `loading` 变量赋上 `true` 的值
   - 在调用大模型成功时，给 `loading` 变量赋上 `false` 的值

**效果**：在调用大模型时，按钮状态为加载中，且不可用。这样一来既让用户感知到了请求状态，也防止用户多次点击重复触发了。在大模型调用结束后，按钮状态也随之恢复正常。

### 调用混元图生文能力

混元大模型具备图生文能力，在微搭「调用大模型」的执行动作中，也可以传入图片相关的参数，让大模型读取图片。

#### 配置步骤

1. **基础配置**：按照调用大模型对话使用示例，搭建出一个能够调用大模型的应用。

2. **修改对话消息格式**：点击「对话消息列表」表单右边的 `fx` 图标，切换到代码表达式模式。

3. **纯文本消息格式**：
```javascript
[
  {
    "role": "user",
    "content": [
      {
        "type": "text",
        "text": "这个图片内容是什么？"
      }
    ]
  }
]
```

4. **添加图片消息**：在 `content` 字段的数组中追加一个表示图片消息的对象元素：
```javascript
[
  {
    "role": "user",
    "content": [
      {
        "type": "text",
        "text": "这个图片内容是什么？"
      },
      {
        "type": "image_url",
        "image_url": {
          "url": "https://cloudcache.tencent-cloud.com/qcloud/ui/portal-set/build/About/images/bg-product-series_87d.png"
        }
      }
    ]
  }
]
```

5. **选择支持图生文的模型**：选择具有图生文能力的模型，如 `hunyuan-vision`。

**字段说明**：
- `type: "text"`：表示这个元素是个文本类型的消息
- `text`：文本消息的内容
- `type: "image_url"`：表示这个元素是个图片的链接
- `image_url.url`：图片链接地址

## 配置参数详解

### 调用大模型对话参数

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| 模型供应商 | string | 是 | 大模型供应商标识，如 `hunyuan-open` |
| 模型 | string | 是 | 具体的模型名称，如 `hunyuan-lite` |
| 对话消息列表 | array | 是 | 对话消息数组，包含角色和内容 |
| 流式调用 | boolean | 否 | 是否开启流式响应，默认为 false |
| temperature | number | 否 | 温度参数，控制生成文本的随机性，范围 0-2 |
| top_p | number | 否 | 核采样参数，控制生成文本的多样性，范围 0-1 |
| 文本赋值变量 | string | 否 | 用于保存生成文本的变量名 |
| SSE 回调事件流 | string | 否 | 处理 SSE 事件的回调函数名 |

### 调用 Agent 对话参数

| 参数名 | 类型 | 必填 | 说明 |
|--------|------|------|------|
| Agent ID | string | 是 | Agent 的唯一标识符 |
| 消息内容 | string | 是 | 发送给 Agent 的消息内容 |
| 历史对话记录 | array | 否 | 历史对话列表，用于提供上下文 |
| 文本赋值变量 | string | 否 | 用于保存生成文本的变量名 |
| SSE 回调事件流 | string | 否 | 处理 SSE 事件的回调函数名 |

## 错误处理

### 常见错误类型

1. **模型配置错误**：
   - 检查模型供应商和模型名称是否正确
   - 确认模型是否已在 AI+ 配置中启用

2. **Agent ID 错误**：
   - 验证 Agent ID 是否存在
   - 检查 Agent 是否已发布

3. **网络连接问题**：
   - 检查网络连接状态
   - 确认服务端点是否可访问

4. **参数格式错误**：
   - 验证对话消息列表格式
   - 检查历史对话记录结构

### 错误处理示例

```javascript
// SSE 错误处理事件流
({ event }) => {
  if (event.detail.error) {
    console.error('AI 调用错误:', event.detail.error);
    // 显示错误提示
    $w.page.dataset.state.errorMessage = event.detail.error.message;
  }
}
```

## 性能优化建议

### 响应时间优化

1. **使用流式响应**：启用流式调用，让用户能够实时看到生成过程
2. **合理设置参数**：根据实际需求调整 temperature 和 top_p 参数
3. **缓存常用结果**：对于重复的查询，考虑缓存机制

### 用户体验优化

1. **加载状态指示**：使用 loading 状态让用户了解请求进度
2. **错误反馈**：提供清晰的错误信息和重试机制
3. **响应式设计**：确保在不同设备上都有良好的体验

### 资源使用优化

1. **控制调用频率**：避免短时间内大量请求
2. **合理设置超时**：设置适当的请求超时时间
3. **内容长度控制**：控制输入和输出内容的长度

## 集成示例

### 完整的聊天应用示例

以下是一个完整的微搭聊天应用配置示例：

#### 页面变量配置
```javascript
// 页面变量
{
  messages: [], // 消息列表
  inputText: '', // 输入文本
  loading: false, // 加载状态
  errorMessage: '' // 错误信息
}
```

#### 发送消息按钮配置
```javascript
// 点击事件执行动作
[
  {
    action: 'assignVariable',
    variable: 'loading',
    value: true
  },
  {
    action: 'addToArray',
    array: 'messages',
    item: {
      role: 'user',
      content: '$w.page.dataset.state.inputText'
    }
  },
  {
    action: 'callAIModel',
    config: {
      modelProvider: 'hunyuan-open',
      model: 'hunyuan-lite',
      messages: '$w.page.dataset.state.messages',
      streaming: true,
      textVariable: 'currentResponse',
      sseCallback: 'handleSSEResponse'
    }
  }
]
```

#### SSE 处理事件流
```javascript
// handleSSEResponse 事件流
({ event }) => {
  const data = event.detail;
  
  if (data.type === 'content') {
    // 更新当前响应内容
    $w.page.dataset.state.currentResponse = data.content;
  } else if (data.type === 'done') {
    // 响应完成，添加到消息列表
    $w.page.dataset.state.messages.push({
      role: 'assistant',
      content: $w.page.dataset.state.currentResponse
    });
    $w.page.dataset.state.loading = false;
    $w.page.dataset.state.inputText = '';
  } else if (data.type === 'error') {
    // 处理错误
    $w.page.dataset.state.errorMessage = data.error.message;
    $w.page.dataset.state.loading = false;
  }
}
```

## 总结

微搭 AI+ 执行动作为低代码开发提供了强大的 AI 能力集成方案。通过本指南，开发者可以：

1. **快速集成**：使用可视化配置快速添加 AI 功能
2. **灵活配置**：支持多种参数配置满足不同需求
3. **流式响应**：提供实时的用户交互体验
4. **错误处理**：完善的错误处理和状态管理
5. **性能优化**：多种优化策略提升应用性能

关键优势：
- 低代码开发，降低技术门槛
- 丰富的配置选项，满足复杂需求
- 完善的错误处理机制
- 支持流式响应，提升用户体验
- 与微搭平台深度集成

通过合理使用这些功能，开发者可以快速构建出功能强大、用户体验良好的 AI 应用。 