# 🌍 AI 智能旅游规划系统

基于 Spring Boot + Vue3 + LLM 的全栈 AI 旅游规划平台。用户输入目的地、天数、预算和偏好，AI 自动生成详细的个性化旅行计划。

## 🔗 在线访问

前端：https://travel-planner-frontend-five.vercel.app

## ✨ 功能特性

- 🤖 AI 智能生成旅行计划（接入 NVIDIA Build DeepSeek-V4-Pro）
- 📝 支持目的地、天数、预算、偏好自定义
- 💾 行程历史保存与查询
- 📱 响应式前端界面，Markdown 渲染 AI 输出
- 🔍 按目的地搜索历史行程

## 🛠 技术栈

### 后端
- Spring Boot 3.5 + Java 21
- Spring Data JPA + H2 内存数据库
- NVIDIA Build API（DeepSeek-V4-Pro）

### 前端
- Vue 3 + TypeScript + Vite
- Axios + Marked.js

## 🚀 本地运行

### 后端
```bash
cd personal-project-lsy270
mvn spring-boot:run
```

### 前端
```bash
npm install
npm run dev
```

## 📡 接口列表

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | /api/travel/plans | 创建行程 |
| GET | /api/travel/plans | 获取所有行程 |
| GET | /api/travel/plans/{id} | 获取单个行程 |
| GET | /api/travel/plans/search | 搜索行程 |
| DELETE | /api/travel/plans/{id} | 删除行程 |

## 🤖 AI 辅助开发说明

本项目使用 Claude AI 辅助开发，比例约 70%。
