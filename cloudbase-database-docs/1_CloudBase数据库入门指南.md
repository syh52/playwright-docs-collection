# CloudBase数据库入门指南

## 概述

CloudBase数据库是腾讯云提供的一站式数据库解决方案，支持多种数据库类型，包括集合管理、文档型数据库、MySQL型数据库等。

## 核心价值

### 零运维
- 无需关注底层数据库运维
- 自动化备份、监控、扩缩容
- 专注业务逻辑开发

### 多选择
- 支持多种数据库类型
- 集合管理：NoSQL文档存储
- 文档型数据库：MongoDB兼容
- MySQL型数据库：关系型数据库
- 自有MySQL：已有数据库整合

### 智能化
- 数据模型：智能数据抽象层
- 自动校验和关联关系
- AI分析和数据洞察
- 智能推荐和优化

### 可视化
- 控制台可视化管理
- 数据实时监控
- 性能指标展示
- 操作日志记录

## 功能特性对比

| 特性 | 集合管理 | 文档型 | MySQL型 | 自有MySQL |
|------|---------|--------|---------|-----------|
| 数据存储 | JSON文档 | BSON文档 | 关系型表 | 关系型表 |
| 查询语言 | 简化API | MongoDB语法 | SQL | SQL |
| 事务支持 | 基础 | 完整 | 完整 | 完整 |
| 索引优化 | 自动 | 手动 | 手动 | 手动 |
| 扩展性 | 高 | 高 | 中 | 低 |
| 学习成本 | 低 | 中 | 高 | 高 |

## 选择决策指南

### 推荐场景

**集合管理**
- 快速原型开发
- 小程序/H5应用
- 内容管理系统
- 用户数据存储

**文档型数据库**
- 复杂数据结构
- 大数据量应用
- 灵活schema需求
- MongoDB迁移

**MySQL型数据库**
- 传统Web应用
- 复杂关系查询
- 数据一致性要求高
- 现有SQL技能

**自有MySQL**
- 现有数据库整合
- 数据迁移需求
- 自定义配置
- 成本控制

### 团队能力评估

**新手团队**：推荐集合管理
- 学习成本低
- 开发效率高
- 运维简单

**有经验团队**：根据项目需求选择
- 文档型：处理复杂数据
- MySQL型：关系型需求
- 自有MySQL：成本优化

## 快速开始

### 前置条件

1. **开通CloudBase服务**
   - 注册腾讯云账号
   - 创建CloudBase环境
   - 开通数据库服务

2. **环境准备**
   - Node.js 8.0+
   - npm 或 yarn
   - 开发IDE

### 创建数据模型

#### 1. 控制台创建
```bash
# 访问CloudBase控制台
https://console.cloud.tencent.com/tcb

# 导航：数据库 > 数据模型 > 新建
```

#### 2. 配置数据模型
```javascript
// 示例：用户信息模型
{
  "modelName": "users",
  "displayName": "用户信息",
  "fields": {
    "username": {
      "type": "string",
      "displayName": "用户名",
      "required": true
    },
    "email": {
      "type": "string",
      "displayName": "邮箱",
      "required": true
    },
    "age": {
      "type": "number",
      "displayName": "年龄"
    },
    "profile": {
      "type": "object",
      "displayName": "个人资料"
    }
  }
}
```

### SDK集成

#### 1. 安装SDK
```bash
# 小程序/H5
npm install @cloudbase/js-sdk

# Node.js
npm install @cloudbase/node-sdk

# 微信小程序云开发
# 无需安装，直接使用wx.cloud
```

#### 2. 初始化配置
```javascript
// 小程序/H5
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'your-env-id',
  region: 'ap-shanghai' // 可选
});

// Node.js
const cloudbase = require('@cloudbase/node-sdk');

const app = cloudbase.init({
  env: 'your-env-id',
  secretId: 'your-secret-id',
  secretKey: 'your-secret-key'
});
```

### 基础CRUD操作

#### 1. 创建数据
```javascript
// 添加单条记录
const db = app.database();
const result = await db.collection('users').add({
  username: 'john',
  email: 'john@example.com',
  age: 25,
  profile: {
    nickname: 'Johnny',
    avatar: 'avatar.jpg'
  }
});

console.log('添加成功:', result);
```

#### 2. 查询数据
```javascript
// 查询所有数据
const allUsers = await db.collection('users').get();

// 条件查询
const adults = await db.collection('users')
  .where({
    age: db.command.gte(18)
  })
  .get();

// 复杂查询
const query = await db.collection('users')
  .where({
    age: db.command.gte(18),
    username: db.command.eq('john')
  })
  .orderBy('age', 'desc')
  .limit(10)
  .get();
```

#### 3. 更新数据
```javascript
// 更新单条记录
const updateResult = await db.collection('users')
  .doc('record-id')
  .update({
    age: 26,
    'profile.nickname': 'Johnny Updated'
  });

// 批量更新
const batchUpdate = await db.collection('users')
  .where({
    age: db.command.lt(18)
  })
  .update({
    status: 'minor'
  });
```

#### 4. 删除数据
```javascript
// 删除单条记录
const deleteResult = await db.collection('users')
  .doc('record-id')
  .remove();

// 批量删除
const batchDelete = await db.collection('users')
  .where({
    status: 'inactive'
  })
  .remove();
```

### 系统默认字段

CloudBase数据库会自动为每条记录添加以下字段：

| 字段名 | 类型 | 说明 |
|--------|------|------|
| _id | String | 记录唯一标识 |
| _createTime | Date | 创建时间 |
| _updateTime | Date | 更新时间 |

```javascript
// 查询结果示例
{
  "_id": "5f7b1a2c3d4e5f6g7h8i9j0k",
  "_createTime": "2021-01-01T00:00:00.000Z",
  "_updateTime": "2021-01-02T00:00:00.000Z",
  "username": "john",
  "email": "john@example.com",
  "age": 25
}
```

## 最佳实践

### 数据模型设计

1. **合理命名**
   - 使用有意义的集合名
   - 字段名使用驼峰命名
   - 避免保留字

2. **字段类型选择**
   - 根据数据特征选择合适类型
   - 合理使用嵌套对象
   - 避免过深嵌套

3. **索引优化**
   - 为常用查询字段创建索引
   - 避免过多索引影响写入性能
   - 定期检查索引使用情况

### 查询优化

1. **条件查询**
   - 使用精确匹配优于模糊匹配
   - 利用索引提高查询效率
   - 避免全表扫描

2. **分页查询**
   - 使用limit限制结果数量
   - 结合orderBy进行排序
   - 考虑使用游标分页

3. **数据获取**
   - 只获取必要字段
   - 使用field指定返回字段
   - 避免获取大量数据

### 安全考虑

1. **权限控制**
   - 配置合适的安全规则
   - 限制数据访问范围
   - 定期检查权限设置

2. **数据验证**
   - 在客户端和服务端都进行验证
   - 使用数据模型约束
   - 防止恶意数据注入

3. **敏感信息**
   - 敏感数据加密存储
   - 避免在客户端存储敏感信息
   - 使用HTTPS传输

## 常见问题

### Q: 如何选择合适的数据库类型？
A: 根据项目需求、团队技能、数据特征等因素综合考虑。新手推荐集合管理，复杂应用考虑文档型或MySQL型。

### Q: 数据库性能如何优化？
A: 合理设计索引、优化查询条件、使用分页查询、避免深度嵌套等。

### Q: 如何处理数据库迁移？
A: 使用CloudBase提供的迁移工具，或编写脚本进行数据导入导出。

### Q: 是否支持事务操作？
A: 集合管理支持基础事务，文档型和MySQL型支持完整事务特性。

## 相关资源

- [CloudBase官方文档](https://docs.cloudbase.net/)
- [SDK API参考](https://docs.cloudbase.net/api-reference/)
- [示例代码](https://github.com/TencentCloudBase/cloudbase-examples)
- [社区论坛](https://cloud.tencent.com/developer/tag/10369)

---

*最后更新：2024年1月* 