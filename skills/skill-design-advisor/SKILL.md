---
name: skill-design-advisor
description: 基于 Google 5大 Agent Skill 设计模式的 Skill 结构规划与设计模式分析工具。分析 skill 的输入输出特征、交互复杂度和领域知识需求，推荐最佳设计模式（Tool Wrapper / Generator / Reviewer / Inversion / Pipeline）及目录结构。当你在规划一个新 skill 的结构、纠结 SKILL.md 该怎么组织、不确定要不要拆 references 目录、或者想了解 Agent Skill 设计模式时，请使用此 skill。也适用于评审现有 skill 的结构合理性。
---

# Agent Skill 设计顾问

基于 Google 5大 Agent Skill 设计模式，分析 skill 需求并推荐最佳设计模式和目录结构。

## 流程概述

1. **收集 Skill 需求** — 了解 skill 的目标、输入输出、交互复杂度
2. **设计模式分析** — 多维度评估 5 种模式适配度，确定最佳组合
3. **输出设计建议** — 生成包含模式推荐、目录结构、章节规划的建议文档

设计模式全景：[references/overview.md](references/overview.md)

---

## Step 1: 收集 Skill 需求

通过对话了解用户计划创建（或评审）的 skill。从用户描述中提取以下维度信息，缺失的再追问。不必逐条提问，自然对话即可，目标是一轮（最多两轮）完成信息收集。

| 维度 | 关注点 | 影响模式选择的原因 |
|------|--------|-------------------|
| **目标** | skill 要解决什么问题，核心价值 | 决定 skill 的整体复杂度 |
| **输入特征** | 用户口述需求 / 代码文件 / 配置 / 多轮问答 | 区分 Inversion（多轮问答）vs 其他模式 |
| **输出特征** | 生成文件 / 检查报告 / 分析建议 / 执行操作 | 区分 Generator（文件）vs Reviewer（报告） |
| **交互复杂度** | 全自动 / 1-2轮 / 3-5轮 / 5+轮 | 轮次越多越倾向 Inversion/Pipeline |
| **领域知识** | 是否需要加载规范、文档、检查清单 | 有领域知识需求指向 Tool Wrapper |
| **流程结构** | 线性执行 / 分支 / 阶段门禁 / 先收集再执行 | 区分 Pipeline（门禁）vs Inversion（先收集） |

**收集完成后**，整理成需求摘要表展示给用户确认，再进入 Step 2。

---

## Step 2: 设计模式分析

基于 Step 1 的需求摘要，对 5 种设计模式进行多维度分析。这不是机械套用规则 — 大多数真实 skill 是多模式混合的，分析时解释推荐的原因。

### 2.1 逐模式评估

对每种模式评估其核心信号是否被触发：

| 模式 | 强信号（高度匹配） | 弱信号（部分匹配） |
|------|-------------------|-------------------|
| **Tool Wrapper** | 需要加载规范/API文档；skill 逻辑简单，核心价值在知识 | 有少量参考资料；输入明确无需交互 |
| **Generator** | 输出有固定模板结构；需要变量替换；格式统一 | 输出格式相对稳定；有可复用的输出骨架 |
| **Reviewer** | 输出是评估/打分/检查报告；有明确检查标准 | 流程中有验证步骤；需要逐项判定 |
| **Inversion** | 需求不明确需多轮挖掘；"先收集再执行"模式 | 有前置信息收集；需理解用户上下文 |
| **Pipeline** | 严格多步骤顺序；步间需用户确认；有阶段门禁 | 流程超过 3 步；涉及方法论框架 |

### 2.2 确定主模式和辅助模式

从信号最强的模式中选出**主模式**。如果有第二种模式也有较强信号，作为**辅助模式**。

常见组合搭配：
- **Pipeline + Reviewer** → 添加 `assets/checklists/` 做阶段门禁
- **Inversion + Generator** → 添加 `assets/templates/` 做收集后生成
- **Generator + Reviewer** → 添加完整性检查步骤
- **Tool Wrapper + 任意** → 添加 `references/` 提供领域知识

当多种模式信号强度接近时，优先推荐更简单的模式。复杂度排序：Tool Wrapper < Generator < Reviewer < Inversion < Pipeline。

### 2.3 加载详细参考

确定模式后，读取对应的详细参考文档获取 Claude Code 中的具体实现指导：

- Tool Wrapper → [references/patterns/tool-wrapper.md](references/patterns/tool-wrapper.md)
- Generator → [references/patterns/generator.md](references/patterns/generator.md)
- Reviewer → [references/patterns/reviewer.md](references/patterns/reviewer.md)
- Inversion → [references/patterns/inversion.md](references/patterns/inversion.md)
- Pipeline → [references/patterns/pipeline.md](references/patterns/pipeline.md)

同时参考 ADK → Claude Code 的映射关系：[references/pattern-mapping.md](references/pattern-mapping.md)

如果对模式选择有不确定性，参考决策指南中的边界案例：[references/decision-guide.md](references/decision-guide.md)

### 2.4 输出模式分析

向用户展示分析结果：主模式 + 辅助模式 + 匹配理由 + 未选模式的说明。用户确认后进入 Step 3。

---

## Step 3: 输出设计建议

基于 Step 2 的模式分析，使用模板 [assets/templates/design-recommendation.md](assets/templates/design-recommendation.md) 生成完整的设计建议文档。

### 建议文档包含：

1. **需求摘要** — Step 1 收集的 6 个维度，结构化表格
2. **设计模式推荐** — 主模式 + 辅助模式 + 匹配理由 + 未选模式说明
3. **推荐目录结构** — ASCII 树形图 + 各目录/文件的用途说明
4. **SKILL.md 章节结构建议** — Frontmatter（含 description 建议）+ 各章节标题和内容概要
5. **关键设计要点** — 3-5 条针对该模式组合的实操注意事项
6. **与 skill-creator 的衔接** — 如何将本建议作为上下文传递给 skill-creator

### 输出规则：

- 建议文档输出为 Markdown，直接展示给用户
- 目录结构根据推荐模式自动确定：
  - Tool Wrapper → `SKILL.md` + `references/`
  - Generator → `SKILL.md` + `assets/templates/`
  - Reviewer → `SKILL.md` + `assets/checklists/`
  - Inversion → `SKILL.md` + `assets/templates/` + 可选 `references/`
  - Pipeline → `SKILL.md` + `assets/checklists/` + `assets/templates/` + `references/`
- description 建议要"pushy" — 主动列出触发场景，覆盖用户可能的各种表述方式
- 章节结构建议只给标题和概要，不写具体内容（那是 skill-creator 的职责）

### 示例输出（节选）：

```markdown
# api-doc-generator — Skill 设计建议

## 设计模式推荐
- **主模式：Generator** — 输出是结构化的 API 文档，有明确的模板结构
- **辅助模式：Tool Wrapper** — 需要加载 OpenAPI 规范作为参考知识

## 推荐目录结构
api-doc-generator/
├── SKILL.md
├── assets/templates/
│   └── api-doc-template.md      # API 文档输出模板
└── references/
    └── openapi-spec.md          # OpenAPI 规范参考

## SKILL.md 章节建议
1. 流程概述（3步：解析代码 → 提取接口 → 生成文档）
2. Step 1: 代码解析 — 识别函数/类/路由定义
3. Step 2: 接口提取 — 整理参数、返回值、异常
4. Step 3: 文档生成 — 按模板填充，输出 Markdown

## 关键设计要点
- 模板文件放在 assets/templates/ 中，SKILL.md 用链接引用
- 不要把 OpenAPI 规范全文塞进 SKILL.md，放在 references/ 按需加载
- Step 之间不需要阶段门禁（线性流程），保持简洁
```
