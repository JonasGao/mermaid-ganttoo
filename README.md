# Mermaid 甘特图工具

一个基于 Svelte 的交互式甘特图编辑工具，使用本地打包的 Mermaid.js 实现。

## 功能特性

1. **左右分栏布局**
   - 左侧：任务配置面板（表格编辑）
   - 右侧：甘特图实时预览和 Mermaid 代码

2. **Section 管理**
   - 支持添加和删除多个 Section（章节）
   - 每个 Section 可自定义名称
   - 每个 Section 包含一个独立的任务表格

3. **任务管理**
   - 支持在每个 Section 中添加、删除任务行
   - 支持修改任务的各个字段

4. **任务字段**
   - **任务名称**：任务的显示名称
   - **任务 ID**：任务的唯一标识符
   - **起始日期**：任务的开始日期（格式：YYYY-MM-DD）
   - **前置任务**：依赖的任务 ID
   - **任务时长**：任务持续天数

5. **实时预览**
   - 自动生成 Mermaid 格式的甘特图代码
   - 实时渲染甘特图
   - 支持复制生成的 Mermaid 代码

## 使用方法

### 开发模式

```bash
npm install
npm run dev
```

然后在浏览器中打开 `http://localhost:5173`

### 生产构建

```bash
npm run build
```

构建后的文件在 `dist` 目录下，可以部署到任何静态文件服务器。

### 预览生产构建

```bash
npm run preview
```

## 示例

工具自带示例数据，展示了如何使用前置任务依赖关系创建项目计划。

## 技术栈

- **Svelte 5** - 现代化的响应式框架
- **Vite** - 快速的构建工具
- **Mermaid.js 11** - 本地打包，无需 CDN
- **JavaScript (ES6+)**

## 项目结构

```
├── src/
│   ├── App.svelte          # 主应用组件
│   ├── lib/
│   │   ├── Section.svelte  # Section 组件
│   │   └── Notification.svelte  # 通知组件
│   └── main.js             # 入口文件
├── index.html              # HTML 模板
├── package.json            # 依赖配置
└── vite.config.js          # Vite 配置
```

## 特性

- ✅ 无需 CDN - Mermaid.js 已本地打包
- ✅ 组件化架构 - 使用 Svelte 组件
- ✅ 热模块替换 - 开发时实时更新
- ✅ 生产优化 - Vite 自动优化和压缩
- ✅ 现代浏览器支持 - 使用 Clipboard API
- ✅ 优雅的通知系统 - Toast 提示

## 许可证

MIT
