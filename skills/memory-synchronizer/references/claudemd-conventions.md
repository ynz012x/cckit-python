# CLAUDE.md 结构规范

## 定位

CLAUDE.md 是项目的**常驻记忆**，为 AI 编码助手提供项目级上下文。它区别于：
- **README.md**：面向人类读者的项目介绍
- **CONTRIBUTING.md**：面向贡献者的协作流程
- **CLAUDE.md**：面向 AI 助手的项目级工作知识

## 内容标准

CLAUDE.md 应包含**适用于任何参与项目开发者的项目级标准**，而非个人偏好：

| 应包含 | 不应包含 |
|--------|----------|
| 构建和测试命令 | 个人 IDE 配置 |
| 编码规范和约定 | 临时调试笔记 |
| 架构决策和设计原则 | 具体业务逻辑说明 |
| 命名约定 | 个人工作流偏好 |
| 常用工作流 | 会议记录或待办事项 |

## 多层级体系

项目可在不同目录层级放置独立的 CLAUDE.md，形成**作用域树**。

### 作用域规则

- 每个 CLAUDE.md 管辖其**所在目录及所有子目录**
- 子目录有自己的 CLAUDE.md 时，**从父级作用域中排除该子树**
- 根目录 CLAUDE.md 为最顶层，管辖所有未被子级覆盖的路径

### 作用域路由算法

对每个文件路径，找到**路径最长匹配**的 CLAUDE.md 所在目录：

```
项目/
├── CLAUDE.md              ← 管辖: ./**，排除 backend/** 和 frontend/**
├── backend/
│   ├── CLAUDE.md          ← 管辖: backend/**
│   └── api/handler.py     ← 归属 backend/CLAUDE.md
├── frontend/
│   ├── CLAUDE.md          ← 管辖: frontend/**
│   └── src/App.tsx        ← 归属 frontend/CLAUDE.md
├── Makefile               ← 归属 ./CLAUDE.md
└── scripts/deploy.sh      ← 归属 ./CLAUDE.md
```

### 内容隔离

每个 CLAUDE.md 只记录**其管辖范围**内的项目级知识：
- `./CLAUDE.md`：全局构建命令、顶层架构、跨模块约定
- `./backend/CLAUDE.md`：后端构建/测试命令、后端架构、后端编码规范
- `./frontend/CLAUDE.md`：前端构建/测试命令、前端框架约定、组件规范

## 标准 Section 类型

以下为常见的 section 类型及其变体标题（用于语义匹配）：

| Section 类型 | 常见标题变体 | 内容 |
|-------------|-------------|------|
| 项目概览 | 项目概览、Overview、简介、About | 一句话定位 + 技术栈 |
| 常用命令 | 常用命令、Commands、开发命令 | 构建/测试/部署命令 |
| 架构 | 架构、Architecture、项目结构、目录结构 | 目录布局 + 模块关系 |
| 开发规范 | 开发规范、规范、Coding Standards、Guidelines | 编码约定、代码风格 |
| 依赖 | 依赖、Dependencies、环境要求 | 关键依赖及安装方式 |
| 测试 | 测试、Testing、测试策略 | 测试框架和运行方式 |
| 部署 | 部署、Deployment、发布 | 部署流程和环境 |
| 注意事项 | 注意事项、Gotchas、Known Issues、坑 | 容易犯错的地方 |
| 工作流 | 工作流、Workflows、开发流程 | 分支策略、CI/CD 等 |

## 格式约定

- 使用 `##` 作为顶层 section 分隔符
- 表格用于结构化信息（如模块列表、命令列表）
- 代码块用于命令和配置示例
- 行数上限 200 行，超出时拆分到同级 `docs/` 目录
- 文件通过版本控制与团队共享
