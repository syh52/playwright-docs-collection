# CloudBase AI ToolKit MCP 工具规格

## 概述

CloudBase AI ToolKit 提供了一系列 MCP（Model Context Protocol）工具，用于在 AI 开发环境中集成云开发能力。这些工具使 AI 助手能够直接操作云开发资源，实现从代码生成到部署的完整流程。

## MCP 工具列表

### 1. 身份验证工具

#### `cloudbase_auth`
**功能**: 腾讯云身份验证和授权
**描述**: 用于登录腾讯云账号，获取操作权限

**输入参数**:
```json
{
  "action": "login" | "logout" | "status",
  "credentials": {
    "secretId": "string",
    "secretKey": "string"
  }
}
```

**输出**:
```json
{
  "success": boolean,
  "user": {
    "uid": "string",
    "nickname": "string",
    "avatar": "string"
  },
  "message": "string"
}
```

### 2. 环境管理工具

#### `cloudbase_env`
**功能**: 云开发环境管理
**描述**: 创建、选择、配置云开发环境

**输入参数**:
```json
{
  "action": "list" | "create" | "select" | "info",
  "envId": "string",
  "envName": "string",
  "payMode": "prepay" | "postpay"
}
```

**输出**:
```json
{
  "success": boolean,
  "envs": [
    {
      "envId": "string",
      "envName": "string",
      "status": "string",
      "createTime": "string"
    }
  ],
  "currentEnv": "string"
}
```

### 3. 静态网站托管工具

#### `cloudbase_hosting`
**功能**: 静态网站托管管理
**描述**: 部署前端代码到静态网站托管

**输入参数**:
```json
{
  "action": "deploy" | "delete" | "list",
  "localPath": "string",
  "remotePath": "string",
  "files": [
    {
      "path": "string",
      "content": "string"
    }
  ]
}
```

**输出**:
```json
{
  "success": boolean,
  "deployId": "string",
  "url": "string",
  "files": [
    {
      "path": "string",
      "size": number,
      "lastModified": "string"
    }
  ]
}
```

### 4. 云函数工具

#### `cloudbase_function`
**功能**: 云函数管理
**描述**: 创建、部署、调用云函数

**输入参数**:
```json
{
  "action": "create" | "deploy" | "invoke" | "list" | "delete",
  "functionName": "string",
  "code": "string",
  "runtime": "nodejs14" | "nodejs16" | "python3.7" | "python3.9",
  "handler": "string",
  "environment": {
    "key": "value"
  },
  "invokeData": {}
}
```

**输出**:
```json
{
  "success": boolean,
  "functionName": "string",
  "result": {},
  "logs": "string",
  "duration": number,
  "memory": number
}
```

### 5. 数据库工具

#### `cloudbase_database`
**功能**: 云数据库管理
**描述**: 创建集合、插入数据、查询数据

**输入参数**:
```json
{
  "action": "createCollection" | "insertData" | "queryData" | "updateData" | "deleteData",
  "collectionName": "string",
  "data": {},
  "query": {},
  "update": {}
}
```

**输出**:
```json
{
  "success": boolean,
  "data": [],
  "total": number,
  "requestId": "string"
}
```

### 6. 云存储工具

#### `cloudbase_storage`
**功能**: 云存储管理
**描述**: 上传、下载、删除文件

**输入参数**:
```json
{
  "action": "upload" | "download" | "delete" | "list",
  "cloudPath": "string",
  "localPath": "string",
  "fileContent": "string",
  "contentType": "string"
}
```

**输出**:
```json
{
  "success": boolean,
  "fileID": "string",
  "downloadURL": "string",
  "files": [
    {
      "fileID": "string",
      "size": number,
      "lastModified": "string"
    }
  ]
}
```

### 7. 日志查看工具

#### `cloudbase_logs`
**功能**: 查看云函数日志
**描述**: 获取云函数执行日志和错误信息

**输入参数**:
```json
{
  "action": "query",
  "functionName": "string",
  "startTime": "string",
  "endTime": "string",
  "limit": number
}
```

**输出**:
```json
{
  "success": boolean,
  "logs": [
    {
      "timestamp": "string",
      "level": "INFO" | "ERROR" | "WARN",
      "message": "string",
      "requestId": "string"
    }
  ]
}
```

## 使用示例

### 完整开发流程示例

```javascript
// 1. 身份验证
const authResult = await mcp.call('cloudbase_auth', {
  action: 'login',
  credentials: {
    secretId: 'your-secret-id',
    secretKey: 'your-secret-key'
  }
});

// 2. 选择环境
const envResult = await mcp.call('cloudbase_env', {
  action: 'select',
  envId: 'your-env-id'
});

// 3. 部署前端代码
const hostingResult = await mcp.call('cloudbase_hosting', {
  action: 'deploy',
  files: [
    {
      path: 'index.html',
      content: '<html><body>Hello World</body></html>'
    }
  ]
});

// 4. 创建云函数
const functionResult = await mcp.call('cloudbase_function', {
  action: 'create',
  functionName: 'hello',
  code: 'exports.main = async (event, context) => { return "Hello"; }',
  runtime: 'nodejs16',
  handler: 'index.main'
});

// 5. 创建数据库集合
const dbResult = await mcp.call('cloudbase_database', {
  action: 'createCollection',
  collectionName: 'users'
});
```

### 错误处理示例

```javascript
try {
  const result = await mcp.call('cloudbase_function', {
    action: 'invoke',
    functionName: 'hello',
    invokeData: { name: 'World' }
  });
  
  if (!result.success) {
    console.error('Function invocation failed:', result.message);
    // 查看日志
    const logs = await mcp.call('cloudbase_logs', {
      action: 'query',
      functionName: 'hello',
      limit: 10
    });
    console.log('Function logs:', logs.logs);
  }
} catch (error) {
  console.error('MCP call failed:', error);
}
```

## 工具集成指南

### 在 AI IDE 中集成

1. **安装 MCP 客户端**
   ```bash
   npm install @cloudbase/mcp-client
   ```

2. **配置 MCP 服务器**
   ```json
   {
     "mcpServers": {
       "cloudbase": {
         "command": "node",
         "args": ["cloudbase-mcp-server.js"],
         "env": {
           "CLOUDBASE_SECRET_ID": "your-secret-id",
           "CLOUDBASE_SECRET_KEY": "your-secret-key"
         }
       }
     }
   }
   ```

3. **使用 MCP 工具**
   ```javascript
   // 在 AI 提示中使用
   const tools = [
     {
       name: 'cloudbase_auth',
       description: '腾讯云身份验证'
     },
     {
       name: 'cloudbase_hosting',
       description: '静态网站托管'
     }
   ];
   ```

### 自定义工具开发

#### 创建自定义 MCP 工具

```javascript
// custom-tool.js
const { Tool } = require('@cloudbase/mcp-sdk');

class CustomTool extends Tool {
  constructor() {
    super({
      name: 'custom_tool',
      description: '自定义工具',
      inputSchema: {
        type: 'object',
        properties: {
          action: { type: 'string' },
          data: { type: 'object' }
        },
        required: ['action']
      }
    });
  }

  async execute(input) {
    const { action, data } = input;
    
    switch (action) {
      case 'process':
        return this.processData(data);
      default:
        throw new Error(`Unknown action: ${action}`);
    }
  }

  async processData(data) {
    // 自定义处理逻辑
    return {
      success: true,
      result: data
    };
  }
}

module.exports = CustomTool;
```

## 最佳实践

### 1. 错误处理
- 始终检查 `success` 字段
- 记录错误信息到日志
- 提供用户友好的错误消息

### 2. 性能优化
- 批量操作减少 API 调用
- 使用缓存避免重复查询
- 异步处理提高响应速度

### 3. 安全考虑
- 不在代码中硬编码密钥
- 使用环境变量管理敏感信息
- 定期轮换访问密钥

### 4. 调试技巧
- 使用日志工具查看执行过程
- 启用详细模式获取更多信息
- 使用测试环境验证功能

## 故障排除

### 常见问题

1. **身份验证失败**
   - 检查 SecretId 和 SecretKey 是否正确
   - 验证账号是否有足够权限
   - 确认网络连接正常

2. **环境访问失败**
   - 确认环境 ID 是否正确
   - 检查环境状态是否正常
   - 验证账号是否有环境访问权限

3. **部署失败**
   - 检查文件路径和内容
   - 验证文件大小是否超限
   - 确认网络连接稳定

4. **函数调用失败**
   - 检查函数名称是否正确
   - 验证函数代码是否有语法错误
   - 查看函数执行日志

### 调试命令

```bash
# 查看 MCP 服务器状态
mcp status

# 测试工具连接
mcp test cloudbase_auth

# 查看工具帮助
mcp help cloudbase_function

# 启用调试模式
export MCP_DEBUG=1
```

## 总结

CloudBase AI ToolKit 的 MCP 工具为 AI 开发提供了强大的云开发能力集成。通过这些工具，AI 助手可以直接操作云开发资源，实现从需求到部署的完整自动化流程。

**核心优势**:
- 🔧 **工具丰富**: 覆盖云开发全部核心功能
- 🚀 **高效集成**: 简单易用的 API 接口
- 🛡️ **安全可靠**: 完善的身份验证和权限管理
- 📊 **可观测性**: 详细的日志和监控能力 