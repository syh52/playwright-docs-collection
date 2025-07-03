# CloudBase 项目模板使用指南

基于 CloudBase AI ToolKit 开发的现代化 Web 应用模板，包含 React 和 Vue 两个版本，为开发者提供快速构建全栈应用的能力。

![Powered by CloudBase](https://github.com/TencentCloudBase/CloudBase-AI-ToolKit)

> 本指南基于 [CloudBase AI ToolKit](https://github.com/TencentCloudBase/CloudBase-AI-ToolKit) 开发，通过AI提示词和 MCP 协议+云开发，让开发更智能、更高效，支持AI生成全栈代码、一键部署至腾讯云开发（免服务器）、智能日志修复。

## 🚀 模板特点

### 通用特点
- 🚀 基于 Vite 构建，提供极速的开发体验
- 🎨 集成 Tailwind CSS 和 DaisyUI 组件库，快速构建漂亮的界面
- 🎁 深度集成腾讯云开发 CloudBase，提供一站式后端云服务
- 🛡️ 多种AI工具配置，支持 Cursor、WindSurf、Comate 等主流AI编辑器

### React 模板特点
- ⚛️ 使用 React 18 和 React Router 6 构建现代化 UI
- 🔄 使用 Framer Motion 实现流畅的动画效果
- 🛣️ HashRouter 路由管理，兼容静态网站托管

### Vue 模板特点
- ⚡ 使用 Vue 3 Composition API 构建现代化 UI
- 🛣️ 集成 Vue Router 4 实现前端路由
- 💡 Vue 3 Reactivity API 状态管理

## 📂 项目架构

### React 模板架构

#### 前端架构
- **框架**：React 18
- **构建工具**：Vite
- **路由**：React Router 6（使用 HashRouter）
- **样式**：Tailwind CSS + DaisyUI
- **动画**：Framer Motion

#### 目录结构
```
├── public/              # 静态资源
├── src/
│   ├── components/      # 可复用组件
│   ├── pages/          # 页面组件
│   ├── utils/          # 工具函数和云开发初始化
│   ├── App.jsx         # 应用入口
│   ├── main.jsx        # 渲染入口
│   └── index.css       # 全局样式
├── index.html          # HTML 模板
├── tailwind.config.js  # Tailwind 配置
├── postcss.config.js   # PostCSS 配置
├── vite.config.js      # Vite 配置
└── package.json        # 项目依赖
```

### Vue 模板架构

#### 前端架构
- **框架**：Vue 3 (Composition API)
- **构建工具**：Vite
- **路由**：Vue Router 4（使用 Hash Router）
- **样式**：Tailwind CSS + DaisyUI
- **状态管理**：Vue 3 Reactivity API

#### 目录结构
```
├── public/             # 静态资源
│   └── vite.svg       # Vite logo
├── src/
│   ├── components/     # 可复用组件
│   │   └── Footer.vue # 页脚组件
│   ├── pages/         # 页面组件
│   │   └── HomePage.vue # 首页
│   ├── utils/         # 工具函数和云开发初始化
│   │   └── cloudbase.js # 云开发配置
│   ├── App.vue        # 应用入口组件
│   ├── main.js        # 应用入口文件
│   └── style.css      # 全局样式
├── index.html         # HTML 模板
├── tailwind.config.js # Tailwind 配置
├── postcss.config.js  # PostCSS 配置
├── vite.config.js     # Vite 配置
├── cloudbaserc.json   # CloudBase CLI 配置
└── package.json       # 项目依赖
```

## 🏗️ 云开发资源

两个模板都使用了以下腾讯云开发（CloudBase）资源：

- **身份认证**：用于用户登录和身份验证
- **云数据库**：可用于存储应用数据
- **云函数**：可用于实现业务逻辑
- **云存储**：可用于存储文件
- **静态网站托管**：用于部署前端应用

## 🚀 快速开始

### 前提条件
- 安装 Node.js (版本 14 或更高，Vue模板需要16+)
- 腾讯云开发账号 (可在 [腾讯云开发官网](https://tcb.cloud.tencent.com/) 注册)

### 1. 获取模板

```bash
# 克隆仓库
git clone https://github.com/TencentCloudBase/awesome-cloudbase-examples.git

# 进入React模板目录
cd awesome-cloudbase-examples/web/cloudbase-react-template

# 或者进入Vue模板目录
cd awesome-cloudbase-examples/web/cloudbase-vue-template
```

### 2. 安装依赖

```bash
npm install
```

### 3. 配置云开发环境

1. 打开 `src/utils/cloudbase.js` 文件
2. 将 `ENV_ID` 变量的值修改为您的云开发环境 ID
3. 将 `vite.config.js` 中的 `https://envId-appid.tcloudbaseapp.com/` 替换为你的云开发环境静态托管默认域名，可以使用 MCP 来查询云开发环境静态托管默认域名

### 4. 本地开发

```bash
npm run dev
```

### 5. 构建生产版本

```bash
npm run build
```

### 6. 预览构建结果（Vue模板）

```bash
npm run preview
```

## 🛣️ 路由系统

### React 模板路由

使用 React Router 6 的 HashRouter：

```jsx
<Router>
  <div className="flex flex-col min-h-screen">
    <main className="flex-grow">
      <Routes>
        <Route path="/" element={<HomePage />} />
        {/* 可以在这里添加新的路由 */}
        <Route path="*" element={<HomePage />} />
      </Routes>
    </main>
    <Footer />
  </div>
</Router>
```

#### 添加新页面和路由

1. 在 `src/pages` 目录下创建新页面组件：

```jsx
import React from 'react';

const ProductPage = () => {
  return (
    <div className="container mx-auto py-8">
      <h1 className="text-3xl font-bold mb-4">产品页面</h1>
      <p>这是产品页面的内容</p>
    </div>
  );
};

export default ProductPage;
```

2. 在 `App.jsx` 中导入新页面并添加路由：

```jsx
import ProductPage from './pages/ProductPage';

// 在 Routes 中添加新路由
<Routes>
  <Route path="/" element={<HomePage />} />
  <Route path="/products" element={<ProductPage />} />
  <Route path="*" element={<HomePage />} />
</Routes>
```

3. 使用 Link 组件添加导航：

```jsx
import { Link } from 'react-router-dom';

<Link to="/products" className="btn btn-primary">
  前往产品页面
</Link>
```

#### 路由参数使用

```jsx
// 定义带参数的路由
<Route path="/product/:id" element={<ProductDetailPage />} />

// 在组件中获取参数
import { useParams } from 'react-router-dom';

const ProductDetailPage = () => {
  const { id } = useParams();
  return (
    <div className="container mx-auto py-8">
      <h1 className="text-3xl font-bold mb-4">产品详情</h1>
      <p>产品ID: {id}</p>
    </div>
  );
};
```

#### 编程式导航

```jsx
import { useNavigate } from 'react-router-dom';

const ComponentWithNavigation = () => {
  const navigate = useNavigate();
  
  const handleClick = () => {
    navigate('/products');
    // 或者带参数: navigate('/product/123');
    // 或者返回上一页: navigate(-1);
  };
  
  return (
    <button onClick={handleClick} className="btn btn-primary">
      前往产品页面
    </button>
  );
};
```

### Vue 模板路由

使用 Vue Router 4 的 Hash Router：

```javascript
const routes = [
  { path: '/', component: HomePage },
  { path: '/:pathMatch(.*)*', redirect: '/' } // 404重定向到首页
]
```

#### 添加新页面和路由

1. 在 `src/pages` 目录下创建新页面组件
2. 在 `src/main.js` 中导入新页面并添加到路由配置
3. 使用 `<router-link>` 或 `$router.push()` 进行页面导航

## 🎨 Vue 3 特性使用

### Composition API

```vue
<script setup>
import { ref, onMounted } from 'vue'

const count = ref(0)
const increment = () => count.value++

onMounted(() => {
  console.log('组件已挂载')
})
</script>
```

### 响应式系统

```javascript
import { ref, reactive, computed } from 'vue'

const state = reactive({
  user: null,
  isLoggedIn: false
})

const userDisplayName = computed(() => 
  state.user ? state.user.name : '游客'
)
```

### 组件开发

```vue
<template>
  <div class="my-component">
    <h2>{{ title }}</h2>
    <button @click="handleClick">点击我</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

defineProps(['title'])
const emit = defineEmits(['click'])

const handleClick = () => {
  emit('click', '按钮被点击了')
}
</script>
```

## ☁️ 云开发功能使用

### 初始化云开发

两个模板都在 `src/utils/cloudbase.js` 中集中管理云开发的初始化和匿名登录功能。

### 使用云开发服务

```javascript
import { app, ensureLogin } from '../utils/cloudbase';

// 数据库操作
await ensureLogin();
const db = app.database();
const result = await db.collection('users').get();  // 查询数据
await db.collection('users').add({ name: 'test' }); // 添加数据

// 调用云函数
const funcResult = await app.callFunction({
  name: 'getEnvInfo'  // React模板
  // name: 'hello'    // Vue模板
});

// 文件上传
const uploadResult = await app.uploadFile({
  cloudPath: 'test.jpg',
  filePath: file
});

// 数据模型（React模板）
const models = app.models;
```

### 重要说明

- 在使用前请先将 `ENV_ID` 变量修改为您的云开发环境 ID
- 模板默认使用匿名登录，适合快速开发和测试
- 所有云开发功能都通过初始化的应用实例直接调用
- `ensureLogin` 方法会检查登录状态，未登录则进行匿名登录
- 匿名登录状态无法使用 `logout` 方法退出
- 使用云开发功能前，请确保在控制台中已创建相应资源

## 🎨 样式开发

两个模板都使用 Tailwind CSS，推荐使用原子化的 class：

```html
<!-- React 版本 -->
<div className="bg-white shadow-lg rounded-lg p-6">
  <h2 className="text-2xl font-bold text-gray-800 mb-4">标题</h2>
  <p className="text-gray-600">内容文本</p>
</div>

<!-- Vue 版本 -->
<template>
  <div class="bg-white shadow-lg rounded-lg p-6">
    <h2 class="text-2xl font-bold text-gray-800 mb-4">标题</h2>
    <p class="text-gray-600">内容文本</p>
  </div>
</template>
```

## 🚀 部署指南

### 部署到云开发静态网站托管

1. 构建项目：`npm run build`
2. 登录腾讯云开发控制台
3. 进入您的环境 -> 静态网站托管
4. 上传 `dist` 目录中的文件

### 使用 CloudBase CLI 部署（Vue模板）

```bash
# 安装 CloudBase CLI
npm install -g @cloudbase/cli

# 登录
tcb login

# 设置环境变量
export ENV_ID=your-env-id

# 部署
tcb framework deploy
```

## 🛠️ AI 工具集成

两个模板都包含了多种AI开发工具的配置：

- **.cursor** - Cursor AI 编辑器配置
- **.windsurf** - WindSurf AI 编辑器配置
- **.comate** - 腾讯云智能编程助手配置
- **.gemini** - Google Gemini 配置
- **.lingma** - 通义灵码配置
- **.roo** - RooCode 配置
- **.trae** - Trae AI 配置
- **.rules** - AI 规则配置
- **.mcp.json** - MCP 协议配置

## 📚 技术栈对比

| 特性 | React 模板 | Vue 模板 |
|------|------------|----------|
| 前端框架 | React 18 | Vue 3 |
| 路由 | React Router 6 | Vue Router 4 |
| 状态管理 | Context/Hooks | Composition API |
| 动画 | Framer Motion | 内置 Transition |
| 构建工具 | Vite | Vite |
| 样式 | Tailwind CSS + DaisyUI | Tailwind CSS + DaisyUI |
| 云服务 | CloudBase JS SDK | CloudBase JS SDK |

## 🤝 贡献指南

欢迎贡献代码、报告问题或提出改进建议！

## 📄 许可证

MIT License

---

## 🔗 相关链接

- [CloudBase AI ToolKit](https://github.com/TencentCloudBase/CloudBase-AI-ToolKit)
- [腾讯云开发控制台](https://tcb.cloud.tencent.com/)
- [React 模板仓库](https://github.com/TencentCloudBase/awesome-cloudbase-examples/tree/master/web/cloudbase-react-template)
- [Vue 模板仓库](https://github.com/TencentCloudBase/awesome-cloudbase-examples/tree/master/web/cloudbase-vue-template) 