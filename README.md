# 校园轻集市

基于 Vue 3 + TypeScript + Pinia 的校园信息服务平台，支持二手交易、失物招领、拼单搭子、跑腿委托四大业务模块。

## 快速开始

```bash
# 1. 安装依赖
pnpm install

# 2. 启动 Mock 服务（终端 1）
pnpm mock

# 3. 启动前端项目（终端 2）
pnpm dev

# 4. 浏览器访问
http://localhost:5173
```

## 技术栈

| 技术 | 用途 |
|------|------|
| Vue 3 + TypeScript | 前端框架 |
| Vite | 构建工具 |
| Vue Router | 路由导航 |
| Pinia | 状态管理 |
| Axios | HTTP 请求 |
| JSON Server | Mock 数据服务 |

## 项目结构

```
src/
├── api/            # 接口封装层（http.ts + 4 个业务模块）
├── components/     # 通用组件（ItemCard、FormField、EmptyState 等 9 个）
├── router/         # 路由配置（13 条路由）
├── stores/         # Pinia Store（user + favorite）
├── views/          # 页面组件（12 个页面）
├── assets/         # 全局样式
└── types/          # 类型定义
```

## 功能清单

- **二手交易**：商品列表、分类筛选、价格展示
- **失物招领**：寻物/招领区分、联系方式展示
- **拼单搭子**：拼单进度、截止时间倒计时
- **跑腿委托**：任务酬劳、起止地点展示
- **信息发布**：四种业务类型动态表单、输入校验、Mock 数据写入
- **收藏系统**：收藏/取消收藏、跨页面状态同步
- **个人中心**：用户资料展示、收藏列表管理
- **交互优化**：加载状态、空状态、错误提示、搜索过滤

## 7 天实训推进

| Day | 主题 | 核心成果 |
|-----|------|---------|
| Day1 | 需求分析与项目初始化 | 环境搭建、Git 配置、项目启动 |
| Day2 | 页面骨架与路由导航 | 12 个页面、路由配置、布局组件 |
| Day3 | Mock 数据与列表渲染 | JSON Server、Axios、ItemCard |
| Day4 | 表单设计与数据提交 | 发布表单、校验、POST 写入 |
| Day5 | 状态管理与用户中心 | Pinia Store、收藏、个人中心 |
| Day6 | 交互优化与体验完善 | 加载/错误/搜索组件、状态打磨 |
| Day7 | 综合验收与项目展示 | 构建检查、文档归档、总结复盘 |

## 证据归档

实训过程证据文档详见 `docs/evidence/` 目录，包含 7 天的完整开发记录、AI 协作日志、问题排查和反思总结。

## 启动命令速查

```bash
pnpm dev        # 启动前端开发服务器
pnpm mock       # 启动 JSON Server Mock 服务（端口 3001）
pnpm build      # 生产构建
pnpm preview    # 预览生产构建
```
