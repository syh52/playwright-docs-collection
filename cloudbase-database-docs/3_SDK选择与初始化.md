# SDK选择与初始化指南

## 目录

- [SDK类型对比](#sdk类型对比)
- [选择建议](#选择建议)
- [SDK初始化配置](#sdk初始化配置)
- [环境配置](#环境配置)
- [多环境管理](#多环境管理)
- [错误处理](#错误处理)
- [性能优化](#性能优化)
- [集合操作指南](#集合操作指南)

## 概述

CloudBase数据库提供了多种SDK，适用于不同的开发场景和平台。选择合适的SDK是高效开发的关键。本指南将详细介绍各种SDK的特点、适用场景以及初始化配置方法。

## SDK类型对比

### 完整对比表格

| SDK类型 | 平台支持 | 身份验证 | 实时监听 | 文件上传 | 云函数调用 | 适用场景 |
|---------|----------|----------|----------|----------|-----------|----------|
| wxCloud实例 | 微信小程序 | 内置 | ✅ | ✅ | ✅ | 微信小程序原生开发 |
| wx-cloud-client-sdk | 微信小程序 | 内置 | ✅ | ✅ | ✅ | 小程序第三方框架 |
| js-sdk | Web/H5 | 自定义 | ✅ | ✅ | ✅ | 前端Web应用 |
| node-sdk | Node.js | 密钥认证 | ❌ | ✅ | ✅ | 服务端开发 |
| HTTP API | 任意平台 | 自定义 | ❌ | ✅ | ✅ | 跨平台集成 |

### 详细特性对比

#### 1. wxCloud实例
```javascript
// 微信小程序内置云开发
wx.cloud.init({
  env: 'your-env-id'
});

// 特点
- 微信小程序原生支持
- 身份验证自动处理
- 最佳性能和兼容性
- 支持所有CloudBase功能
- 无需额外安装
```

**优势：**
- 原生支持，性能最佳
- 身份验证无缝集成
- 完整的功能支持
- 开发体验最佳

**局限：**
- 仅限微信小程序
- 依赖微信生态

#### 2. wx-cloud-client-sdk
```javascript
// 第三方小程序框架使用
import cloud from 'wx-cloud-client-sdk';

cloud.init({
  env: 'your-env-id',
  appid: 'your-appid'
});

// 特点
- 支持第三方小程序框架
- 兼容微信小程序API
- 独立的SDK包
- 完整功能支持
```

**优势：**
- 支持第三方框架（uni-app、Taro等）
- 与原生API兼容
- 完整功能支持
- 易于迁移

**局限：**
- 需要额外安装
- 体积相对较大

#### 3. js-sdk
```javascript
// Web/H5应用使用
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'your-env-id',
  region: 'ap-shanghai'
});

// 特点
- 支持Web和H5应用
- 完整的浏览器兼容性
- 支持多种身份验证
- 实时数据同步
```

**优势：**
- 跨浏览器兼容
- 功能完整
- 灵活的身份验证
- 实时监听支持

**局限：**
- 需要处理跨域问题
- 客户端暴露配置信息

#### 4. node-sdk
```javascript
// 服务端Node.js使用
const cloudbase = require('@cloudbase/node-sdk');

const app = cloudbase.init({
  env: 'your-env-id',
  secretId: 'your-secret-id',
  secretKey: 'your-secret-key'
});

// 特点
- 服务端专用SDK
- 基于密钥认证
- 高性能数据操作
- 适合后端服务
```

**优势：**
- 服务端高性能
- 安全的密钥认证
- 完整的管理权限
- 适合批量操作

**局限：**
- 仅限服务端使用
- 不支持实时监听

#### 5. HTTP API
```javascript
// 直接HTTP请求
const response = await fetch('https://tcb-api.tencentcloudapi.com/web', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer your-token'
  },
  body: JSON.stringify({
    action: 'database.query',
    env: 'your-env-id',
    data: {
      query: 'SELECT * FROM users'
    }
  })
});

// 特点
- 跨平台支持
- 轻量级集成
- 自定义请求
- 灵活的权限控制
```

**优势：**
- 跨平台通用
- 轻量级实现
- 最大灵活性
- 易于集成

**局限：**
- 需要手动处理认证
- 功能相对有限
- 需要自行处理错误

## 选择建议

### 按项目类型选择

#### 微信小程序项目
```javascript
// 推荐：wxCloud实例
wx.cloud.init({
  env: 'your-env-id'
});

// 理由：
- 原生支持，性能最佳
- 身份验证自动处理
- 完整功能支持
- 开发体验最佳
```

#### 第三方小程序框架
```javascript
// 推荐：wx-cloud-client-sdk
import cloud from 'wx-cloud-client-sdk';

cloud.init({
  env: 'your-env-id',
  appid: 'your-appid'
});

// 理由：
- 支持uni-app、Taro等框架
- 与原生API兼容
- 完整功能支持
```

#### Web/H5应用
```javascript
// 推荐：js-sdk
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'your-env-id',
  region: 'ap-shanghai'
});

// 理由：
- 跨浏览器兼容
- 完整功能支持
- 实时监听能力
```

#### 服务端应用
```javascript
// 推荐：node-sdk
const cloudbase = require('@cloudbase/node-sdk');

const app = cloudbase.init({
  env: 'your-env-id',
  secretId: 'your-secret-id',
  secretKey: 'your-secret-key'
});

// 理由：
- 服务端专用优化
- 安全的密钥认证
- 高性能数据操作
```

#### 跨平台集成
```javascript
// 推荐：HTTP API
const apiCall = async (action, data) => {
  const response = await fetch('https://tcb-api.tencentcloudapi.com/web', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`
    },
    body: JSON.stringify({
      action,
      env: 'your-env-id',
      data
    })
  });
  return response.json();
};

// 理由：
- 跨平台通用
- 轻量级实现
- 最大灵活性
```

### 按团队技能选择

#### 新手团队
**推荐：js-sdk或wxCloud实例**
```javascript
// 简单易用，文档完善
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'your-env-id'
});

// 优势：
- 学习成本低
- 示例丰富
- 社区支持好
```

#### 有经验团队
**推荐：根据项目需求选择**
```javascript
// 可以根据性能、功能需求选择
// 服务端：node-sdk
// 前端：js-sdk
// 小程序：wxCloud实例
```

#### 企业级项目
**推荐：node-sdk + HTTP API**
```javascript
// 服务端统一管理
const cloudbase = require('@cloudbase/node-sdk');

// 前端轻量级调用
const apiCall = async (action, data) => {
  // 通过自建API服务调用
  return await fetch('/api/cloudbase', {
    method: 'POST',
    body: JSON.stringify({ action, data })
  });
};

// 优势：
- 集中权限管理
- 更好的安全性
- 统一的错误处理
```

## SDK初始化配置

### wxCloud实例初始化

#### 基础配置
```javascript
// 在app.js中初始化
App({
  onLaunch: function () {
    // 初始化云开发
    wx.cloud.init({
      env: 'your-env-id', // 环境ID
      traceUser: true,    // 用户访问记录
    });
  }
});
```

#### 高级配置
```javascript
wx.cloud.init({
  env: 'your-env-id',
  traceUser: true,
  region: 'ap-shanghai',    // 地域
  timeout: 10000,           // 超时时间
  proxy: {                  // 代理配置
    host: 'proxy.example.com',
    port: 8080
  }
});
```

### wx-cloud-client-sdk初始化

#### 安装SDK
```bash
npm install wx-cloud-client-sdk
```

#### 基础配置
```javascript
import cloud from 'wx-cloud-client-sdk';

cloud.init({
  env: 'your-env-id',
  appid: 'your-appid'
});
```

#### 高级配置
```javascript
cloud.init({
  env: 'your-env-id',
  appid: 'your-appid',
  region: 'ap-shanghai',
  timeout: 10000,
  debug: true,              // 调试模式
  proxy: {
    host: 'proxy.example.com',
    port: 8080
  }
});
```

### js-sdk初始化

#### 安装SDK
```bash
npm install @cloudbase/js-sdk
```

#### 基础配置
```javascript
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'your-env-id'
});
```

#### 完整配置
```javascript
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'your-env-id',
  region: 'ap-shanghai',        // 地域
  persistence: 'local',         // 持久化方式: local/session/none
  timeout: 10000,               // 超时时间
  debug: true,                  // 调试模式
  proxy: {                      // 代理配置
    host: 'proxy.example.com',
    port: 8080
  },
  appSign: 'your-app-sign',     // 应用签名
  appSecret: {                  // 应用密钥
    appAccessKey: 'your-app-access-key',
    appAccessKeyId: 'your-app-access-key-id'
  }
});
```

#### 身份验证配置
```javascript
// 匿名登录
const auth = app.auth();
await auth.signInAnonymously();

// 自定义登录
await auth.signInWithCustomToken('your-custom-token');

// 用户名密码登录
await auth.signInWithUsernameAndPassword('username', 'password');
```

### node-sdk初始化

#### 安装SDK
```bash
npm install @cloudbase/node-sdk
```

#### 基础配置
```javascript
const cloudbase = require('@cloudbase/node-sdk');

const app = cloudbase.init({
  env: 'your-env-id',
  secretId: 'your-secret-id',
  secretKey: 'your-secret-key'
});
```

#### 完整配置
```javascript
const cloudbase = require('@cloudbase/node-sdk');

const app = cloudbase.init({
  env: 'your-env-id',
  secretId: 'your-secret-id',
  secretKey: 'your-secret-key',
  region: 'ap-shanghai',        // 地域
  timeout: 10000,               // 超时时间
  debug: true,                  // 调试模式
  proxy: {                      // 代理配置
    host: 'proxy.example.com',
    port: 8080,
    auth: {
      username: 'proxy-user',
      password: 'proxy-pass'
    }
  },
  headers: {                    // 自定义请求头
    'User-Agent': 'My-App/1.0.0'
  }
});
```

#### 证书配置
```javascript
// 使用临时密钥
const app = cloudbase.init({
  env: 'your-env-id',
  secretId: 'your-secret-id',
  secretKey: 'your-secret-key',
  sessionToken: 'your-session-token' // 临时密钥
});

// 使用服务账号
const app = cloudbase.init({
  env: 'your-env-id',
  serviceAccount: {
    privateKey: 'your-private-key',
    privateKeyId: 'your-private-key-id'
  }
});
```

### HTTP API配置

#### 基础请求封装
```javascript
class CloudBaseAPI {
  constructor(config) {
    this.env = config.env;
    this.region = config.region || 'ap-shanghai';
    this.baseURL = `https://tcb-api.tencentcloudapi.com`;
    this.token = config.token;
  }

  async request(action, data) {
    const response = await fetch(`${this.baseURL}/web`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.token}`,
        'X-CloudBase-Region': this.region
      },
      body: JSON.stringify({
        action,
        env: this.env,
        data
      })
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return await response.json();
  }
}
```

#### 使用示例
```javascript
const api = new CloudBaseAPI({
  env: 'your-env-id',
  region: 'ap-shanghai',
  token: 'your-access-token'
});

// 查询数据
const result = await api.request('database.query', {
  collectionName: 'users',
  query: {
    age: { $gte: 18 }
  }
});
```

## 环境配置

### 开发环境配置

#### 环境变量设置
```javascript
// .env.development
CLOUDBASE_ENV=dev-env-id
CLOUDBASE_REGION=ap-shanghai
CLOUDBASE_SECRET_ID=your-dev-secret-id
CLOUDBASE_SECRET_KEY=your-dev-secret-key
```

#### 配置文件
```javascript
// config/development.js
module.exports = {
  cloudbase: {
    env: process.env.CLOUDBASE_ENV,
    region: process.env.CLOUDBASE_REGION,
    secretId: process.env.CLOUDBASE_SECRET_ID,
    secretKey: process.env.CLOUDBASE_SECRET_KEY,
    timeout: 5000,
    debug: true
  }
};
```

### 生产环境配置

#### 环境变量设置
```javascript
// .env.production
CLOUDBASE_ENV=prod-env-id
CLOUDBASE_REGION=ap-shanghai
CLOUDBASE_SECRET_ID=your-prod-secret-id
CLOUDBASE_SECRET_KEY=your-prod-secret-key
```

#### 配置文件
```javascript
// config/production.js
module.exports = {
  cloudbase: {
    env: process.env.CLOUDBASE_ENV,
    region: process.env.CLOUDBASE_REGION,
    secretId: process.env.CLOUDBASE_SECRET_ID,
    secretKey: process.env.CLOUDBASE_SECRET_KEY,
    timeout: 10000,
    debug: false,
    retry: {
      maxRetries: 3,
      retryDelay: 1000
    }
  }
};
```

## 多环境管理

### 环境切换配置
```javascript
// utils/cloudbase.js
const configs = {
  development: {
    env: 'dev-env-id',
    region: 'ap-shanghai',
    debug: true
  },
  testing: {
    env: 'test-env-id',
    region: 'ap-shanghai',
    debug: true
  },
  production: {
    env: 'prod-env-id',
    region: 'ap-shanghai',
    debug: false
  }
};

const currentEnv = process.env.NODE_ENV || 'development';
const config = configs[currentEnv];

export default config;
```

### 统一初始化封装
```javascript
// utils/init.js
import cloudbase from '@cloudbase/js-sdk';
import config from './cloudbase';

let app = null;

export const initCloudBase = () => {
  if (!app) {
    app = cloudbase.init(config);
  }
  return app;
};

export const getDatabase = () => {
  const app = initCloudBase();
  return app.database();
};

export const getAuth = () => {
  const app = initCloudBase();
  return app.auth();
};

export const getStorage = () => {
  const app = initCloudBase();
  return app.uploadFile();
};
```

## 错误处理

### 通用错误处理
```javascript
// utils/error-handler.js
export const handleCloudBaseError = (error) => {
  console.error('CloudBase Error:', error);
  
  // 根据错误类型进行处理
  switch (error.code) {
    case 'NETWORK_ERROR':
      return '网络连接失败，请检查网络';
    case 'AUTH_FAILED':
      return '身份验证失败，请重新登录';
    case 'PERMISSION_DENIED':
      return '权限不足，请联系管理员';
    case 'QUOTA_EXCEEDED':
      return '请求次数超限，请稍后再试';
    default:
      return error.message || '未知错误';
  }
};
```

### 重试机制
```javascript
// utils/retry.js
export const retryOperation = async (operation, maxRetries = 3, delay = 1000) => {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await operation();
    } catch (error) {
      if (i === maxRetries - 1) {
        throw error;
      }
      
      // 指数退避
      await new Promise(resolve => setTimeout(resolve, delay * Math.pow(2, i)));
    }
  }
};
```

## 性能优化

### 连接池配置
```javascript
// 服务端连接池
const cloudbase = require('@cloudbase/node-sdk');

const app = cloudbase.init({
  env: 'your-env-id',
  secretId: 'your-secret-id',
  secretKey: 'your-secret-key',
  pool: {
    maxConnections: 10,      // 最大连接数
    idleTimeout: 30000,      // 空闲超时
    acquireTimeout: 10000    // 获取连接超时
  }
});
```

### 请求优化
```javascript
// 批量操作
const batchOperations = async (operations) => {
  const db = app.database();
  const batch = db.batch();
  
  operations.forEach(op => {
    switch (op.type) {
      case 'add':
        batch.add(db.collection(op.collection), op.data);
        break;
      case 'update':
        batch.update(db.collection(op.collection).doc(op.id), op.data);
        break;
      case 'delete':
        batch.delete(db.collection(op.collection).doc(op.id));
        break;
    }
  });
  
  return await batch.commit();
};
```

## 集合操作指南

### 1. 增删改查操作

#### 1.1 查询操作

##### 查询操作符

数据库 API 提供了大于、小于等多种动态查询指令，这些指令都暴露在 `db.command` 对象上。

| 操作符 | 说明 | 示例 |
|--------|------|------|
| **eq** | 等于 | `{age: _.eq(18)}` |
| **neq** | 不等于 | `{status: _.neq('deleted')}` |
| **gt** | 大于 | `{score: _.gt(80)}` |
| **gte** | 大于等于 | `{age: _.gte(18)}` |
| **lt** | 小于 | `{price: _.lt(100)}` |
| **lte** | 小于等于 | `{discount: _.lte(0.5)}` |
| **in** | 在数组中 | `{category: _.in(['tech', 'news'])}` |
| **nin** | 不在数组中 | `{status: _.nin(['deleted', 'banned'])}` |

##### 单条查询

```javascript
// 根据文档 ID 查询单条记录
const result = await db.collection('todos')
  .doc('docId')
  .get()

console.log('查询结果:', result.data)
```

##### 多条查询

```javascript
// 查询所有记录
const result = await db.collection('todos').get()

// 条件查询
const result = await db.collection('todos')
  .where({
    completed: false,
    priority: 'high'
  })
  .get()
```

##### 高级查询

```javascript
const _ = db.command

// 复杂条件查询
const result = await db.collection('todos')
  .where({
    // 年龄大于 18
    age: _.gt(18),
    // 标签包含 '技术'
    tags: _.in(['技术', '学习']),
    // 创建时间在最近一周内
    createdAt: _.gte(new Date(Date.now() - 7 * 24 * 60 * 60 * 1000))
  })
  .orderBy('createdAt', 'desc') // 按创建时间倒序
  .limit(10) // 限制 10 条
  .skip(0) // 跳过 0 条（分页）
  .field({ // 只返回指定字段
    title: true,
    completed: true,
    createdAt: true
  })
  .get()
```

##### 分页查询

```javascript
// 分页查询示例
const pageSize = 10
const pageNum = 1

const result = await db.collection('todos')
  .orderBy('createdAt', 'desc')
  .skip((pageNum - 1) * pageSize)
  .limit(pageSize)
  .get()

console.log(`第${pageNum}页数据:`, result.data)
```

#### 1.2 新增数据

##### 单条新增

```javascript
// 添加单条记录
const result = await db.collection('todos').add({
  data: {
    title: '学习 CloudBase',
    content: '完成数据库操作教程',
    completed: false,
    priority: 'high',
    createdAt: new Date(),
    tags: ['学习', '技术']
  }
})

console.log('新增成功，文档 ID:', result._id)
```

##### 批量新增

```javascript
// 批量添加多条记录
const batchResult = await db.collection('todos').add([
  {
    data: {
      title: '任务1',
      priority: 'high',
      completed: false
    }
  },
  {
    data: {
      title: '任务2',
      priority: 'medium',
      completed: false
    }
  }
])

console.log('批量新增成功:', batchResult)
```

#### 1.3 更新数据

##### 单条更新

```javascript
// 更新指定文档
const result = await db.collection('todos')
  .doc('todo-id')
  .update({
    data: {
      title: '学习 CloudBase 数据库',
      completed: true,
      updatedAt: new Date(),
      completedBy: 'user123'
    }
  })

console.log('更新成功，影响记录数:', result.stats.updated)
```

##### 批量更新

```javascript
// 批量更新多条记录
const batchResult = await db.collection('todos')
  .where({
    completed: false,
    priority: 'low'
  })
  .update({
    data: {
      priority: 'normal',
      updatedAt: new Date()
    }
  })

console.log('批量更新结果:', batchResult)
```

#### 1.4 删除数据

##### 单条删除

```javascript
// 删除指定文档
const result = await db.collection('todos')
  .doc('todo-id')
  .remove()

console.log('删除成功，影响记录数:', result.stats.removed)
```

##### 批量删除

```javascript
// 批量删除多条记录
const _ = db.command
const batchResult = await db.collection('todos')
  .where({
    completed: true,
    createdAt: _.lt(new Date(Date.now() - 30 * 24 * 60 * 60 * 1000)) // 30天前
  })
  .remove()

console.log('批量删除结果:', batchResult)
```

### 2. 事务操作

#### 2.1 基础概念

事务操作确保多个数据库操作要么全部成功，要么全部失败，保证数据的一致性。

**注意：目前数据库事务只支持在服务端运行，只有 `node-sdk` 支持事务。**

#### 2.2 使用场景

- **从传统关系型数据库迁移到云开发**：数据模型平滑迁移，业务代码改造成本低
- **涉及多个文档/多个集合的业务流程**：保证一系列读写操作完全成功或者完全失败，防止出现中间态

#### 2.3 支持的方法

##### 事务流程方法

| API | 说明 |
|-----|------|
| **startTransaction** | 发起事务 |
| **commit** | 提交事务 |
| **rollback** | 回滚事务 |
| **runTransaction** | 自动提交事务 |

##### 事务中的数据操作

| API | 说明 |
|-----|------|
| **get** | 查询文档 |
| **add** | 插入文档 |
| **delete** | 删除文档 |
| **update** | 更新文档 |
| **set** | 更新文档，文档不存在时，会自动创建 |

#### 2.4 基础事务示例

```javascript
// 基础事务操作
const result = await db.runTransaction(async transaction => {
  // 在事务中进行多个操作
  const todoDoc = await transaction.collection('todos').doc('todo-id').get()
  
  if (!todoDoc.data) {
    throw new Error('待办事项不存在')
  }

  // 更新待办事项状态
  await transaction.collection('todos').doc('todo-id').update({
    data: {
      completed: true,
      completedAt: new Date()
    }
  })

  // 更新用户统计
  await transaction.collection('users').doc('user-id').update({
    data: {
      completedTodos: db.command.inc(1)
    }
  })

  return { success: true }
})

console.log('事务执行结果:', result)
```

#### 2.5 复杂事务示例：清空购物车

```javascript
// Node.js 环境
const cloudbase = require('@cloudbase/node-sdk')
const app = cloudbase.init({})
const db = app.database()

exports.main = async (event, context) => {
  const userId = 'user1'
  const transaction = await db.startTransaction()

  try {
    const usersCollection = transaction.collection('users')
    const goodsCollection = transaction.collection('goods')

    // 1. 获取用户信息
    const user = await usersCollection.doc(userId).get()
    const { cart, balance } = user.data

    // 2. 获取购物车数据和对应的商品信息
    const goods = []
    for (const { id } of cart) {
      const good = await goodsCollection.doc(id).get()
      goods.push(good.data)
    }

    let totalPrice = 0
    for (let i = 0; i < cart.length; ++i) {
      // 3. 计算购物车中的商品总价
      totalPrice += cart[i].num * goods[i].price

      // 4. 更新商品库存
      await goodsCollection.doc(goods[i]._id).set({
        inventory: goods[i].inventory - cart[i].num
      })
    }

    // 5. 更新用户账户余额和清空购物车
    await usersCollection.doc(userId).set({
      balance: balance - totalPrice,
      cart: []
    })

    await transaction.commit()

    return { success: true, totalPrice }
  } catch (error) {
    await transaction.rollback()
    throw error
  }
}
```

#### 2.6 事务限制

| 限制项 | 说明 | 影响 |
|--------|------|------|
| **操作数量** | 单个事务最多 100 个操作 | 需要合理规划事务范围 |
| **执行时间** | 事务执行时间不能超过 30 秒 | 避免复杂计算和外部调用 |
| **文档操作** | 仅支持 `doc` 方法，不支持 `where` 方法 | 当前事务仅支持单文档操作 |
| **DDL 操作** | 避免在 DDL 操作期间进行事务 | 可能导致事务无法获得锁 |

#### 2.7 最佳实践

##### 事务设计原则

- **避免长时间运行的事务**：事务运行时间过长（超过 30s）可能会被自动终止
- **避免执行过多操作**：当一个事务中的操作过多时，可能会影响数据库性能
- **避免在 DDL 操作时进行事务**：在创建索引、删除数据库期间，事务可能无法获得锁
- **配合 try-catch 捕获异常**：尽早发现和处理写冲突、网络异常等问题

##### 性能优化

- **将事务拆分成更小的事务**：防止长时间运行和操作过多的情况
- **索引优化**：为事务中查询的字段建立索引
- **数据预取**：在事务外预先获取需要的数据

### 3. 聚合搜索

#### 3.1 基础概念

聚合主要用来处理数据（统计平均值，求和等）并返回计算后的数据结果。聚合是一个流水线式的批处理作业，初始文档经过多个阶段的流水线处理后，得到转换后的聚合结果。

#### 3.2 聚合示例

假设已有一个集合 **books**，其中包含以下格式记录：

```javascript
[
  {
    "_id": "xxx",
    "category": "Novel",
    "name": "The Catcher in the Rye",
    "onSale": true,
    "sales": 80
  }
]
```

聚合示例：

```javascript
// 云函数端示例
const cloudbase = require("@cloudbase/node-sdk");
const app = cloudbase.init();
const db = app.database();
const $ = db.command.aggregate;

exports.main = async (event, context) => {
  const res = await db
    .collection("books")
    .aggregate()
    .match({
      onSale: true, // 是否正在出售
    })
    .group({
      // 按 category 字段分组
      _id: "$category",
      // 让输出的每组记录有一个 avgSales 字段，其值是组内所有记录的 sales 字段的平均值
      avgSales: $.avg("$sales"),
    })
    .end();

  return {
    res,
  };
};
```

第一阶段：**match** 阶段过滤出了集合中的文档数据(`onSale:true` 表示找出正在出售的书籍)并传给下一个阶段。

第二阶段：**group** 阶段基于 `category` 字段进行分组，并统计出每组中所有记录的 `sales` 字段平均值。

#### 3.3 优化执行

##### 利用索引

**match** 和 **sort** 如果是在流水线的开头的话是可以利用索引的。**geoNear** 也可以利用地理位置索引，但要注意的是，**geoNear** 必须是流水线的第一个阶段。

##### 尽早缩小数据集

在不需要集合的全集情况下，应该尽早的通过 **match**、**limit** 和 **skip** 缩小要处理的记录数量。

#### 3.4 注意事项

除了 **match** 阶段，在各个聚合阶段中传入的对象中，可使用的操作符都是聚合操作符，需要特别注意的是，**match** 进行的是查询匹配，因此语法同普通查询 **where** 的语法，用的是普通查询操作符。

### 4. 实时推送

#### 4.1 基础概念

云开发数据库支持监听集合中符合查询条件的数据的更新事件。

#### 4.2 建立监听

使用 `watch()` 方法即可建立监听，并且返回 `watcher` 对象，用于关闭监听。

符合条件的文档有任何变化，都会触发 `onChange` 回调。

```javascript
const cloudbase = require("@cloudbase/js-sdk");

const app = cloudbase.init({
  env: "xxxx"
});

// 1. 获取数据库引用
var db = app.database();

const watcher = db
  .collection("todos")
  .where({
    // query...
  })
  .watch({
    onChange: function (snapshot) {
      console.log("snapshot", snapshot);
    },
    onError: function (err) {
      console.error("the watch closed because of error", err);
    }
  });
```

#### 4.3 关闭监听

调用 `watcher.close()` 即可关闭监听。

```javascript
watcher.close();
```

### 5. 最佳实践

#### 5.1 数据结构设计

- **合理设计集合结构**：避免过度嵌套，保持数据结构清晰
- **建立索引**：为常用查询字段建立索引提升性能
- **数据类型**：使用合适的数据类型，如日期使用时间戳

#### 5.2 查询优化

- **限制返回数据量**：使用 `limit()` 控制返回记录数
- **字段投影**：使用 `field()` 只返回需要的字段

#### 5.3 安全考虑

- **安全规则**：配置合适的数据库安全规则
- **输入验证**：验证用户输入数据的合法性
- **敏感信息**：避免在客户端存储敏感信息

#### 5.4 错误处理

```javascript
try {
  const result = await db.collection('todos').get()
  console.log('查询成功:', result.data)
} catch (error) {
  console.error('查询失败:', error)
}
```

## 总结

CloudBase数据库提供了完整的数据操作能力，从基础的增删改查到高级的事务处理、聚合分析和实时监听，满足各种场景的数据处理需求。通过合理选择SDK、正确初始化配置，并遵循最佳实践，可以构建高效、可靠的数据库应用。

---

*最后更新：2024年1月* 