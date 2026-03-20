# Tool Wrapper 模式

## 定义

Tool Wrapper 让 Agent 按需加载特定领域的知识和规范文档。它是最基础的模式 — skill 的核心价值在于"知道去哪里找信息"，而非复杂的流程编排。

## 在 Claude Code 中的实现方式

### SKILL.md 结构建议

```markdown
---
name: <skill-name>
description: <描述 + 触发场景>
---

# <标题>

<一句话说明 skill 做什么>

## 流程概述

1. <步骤1> — 简要描述
2. <步骤2> — 简要描述

完整规范参考：[references/<spec-file>.md](references/<spec-file>.md)

---

## Step 1: <步骤名称>

<具体操作指令>

## Step 2: <步骤名称>

<具体操作指令，需要时引用 references/ 中的文档>
```

### 推荐目录布局

```
<skill-name>/
├── SKILL.md              # 简洁的操作指令（<200行）
└── references/
    └── <spec>.md          # 规范/API文档/领域知识
```

特点：目录结构最简单，通常只有 SKILL.md + references/。

### 关键设计要点

1. **SKILL.md 保持精简**：操作指令放 SKILL.md，领域知识放 references/
2. **链接引用**：在需要参考知识的地方用 Markdown 链接指向 references 文件
3. **渐进加载**：模型只在需要时才读取 references 文件，节省上下文
4. **references 文件可以很长**：不受 500 行限制，但建议超过 300 行时加目录索引

## 典型示例

**Terraform 资源命名检查器**：
- SKILL.md（~80行）定义 2 步流程：扫描 .tf 文件 → 对照命名规范检查
- `references/naming-convention.md` 存放团队的云资源命名规范
- SKILL.md 中用链接引用：`命名规范参考：[references/naming-convention.md](...)`
- 模型在检查资源名时按需查阅规范，而非每次触发都加载全文

**REST API 客户端生成器**：
- SKILL.md 定义调用流程和参数校验规则
- `references/api-reference.md` 存放目标服务的 API 文档（endpoint、参数、返回值）
- 模型在构造请求时按需查阅对应 endpoint 的文档

## 适用场景

- skill 的核心价值是"按某个规范/文档来做事"
- 操作步骤本身不复杂（1-3步），但需要领域知识支撑
- 输入明确，不需要多轮交互
- 典型场景：按编码规范格式化、按 API 文档调用、按模板生成配置

## 不适用场景

- 需要多轮交互挖掘需求（用 Inversion）
- 输出是结构化评估报告（用 Reviewer）
- 有严格的多步骤流程需要阶段确认（用 Pipeline）

## 常见陷阱

1. **把全部规范塞进 SKILL.md**：导致 L2 层过大，每次触发都加载全部内容
2. **references 文件不加索引**：超过 300 行的参考文档应加目录，方便模型定位
3. **操作指令和领域知识混在一起**：应清晰分离"做什么"（SKILL.md）和"参考什么"（references/）

## 与其他模式的组合

- **Tool Wrapper + Generator**：最常见组合。用 references/ 提供知识，用 assets/templates/ 提供输出模板
- **Tool Wrapper + Reviewer**：用 references/ 提供检查标准，配合 checklist 做评审
- **作为其他模式的基底**：几乎任何模式都可以叠加 Tool Wrapper 来补充领域知识
