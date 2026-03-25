---
name: skill-design-advisor
description: 基于 Google 5大设计模式与 Anthropic Skill 工程实践的 Skill 结构规划与设计分析工具。分析 skill 的功能类别、输入输出特征、交互复杂度和领域知识需求，推荐最佳设计模式（Tool Wrapper / Generator / Reviewer / Inversion / Pipeline）、目录结构和写作最佳实践。当你在规划一个新 skill 的结构、判断 skill 属于哪种类别、纠结 SKILL.md 该怎么组织、不确定要不要拆 references 目录、想了解 Agent Skill 设计模式、想知道如何写好 skill 或优化现有 skill 时，请使用此 skill。也适用于评审现有 skill 的结构合理性。
---

# Agent Skill 设计顾问

分析 skill 需求，推荐最佳设计模式、目录结构和写作实践。融合 Google 5大设计模式与 Anthropic Skill 工程经验。

## 流程概述

1. **收集 Skill 需求** — 从用户描述中提取目标、类别、输入输出、交互复杂度
2. **设计模式分析** — 类别缩窄 + 信号验证，确定模式组合和写作实践
3. **输出设计建议** — 生成包含类别分析、模式推荐、目录结构、写作实践的建议文档

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
| **类别** | 属于 9 种功能类别中的哪一种 | 缩小模式搜索空间，确定适用的写作实践 |

类别从用户目标描述中自然推断，不单独追问。9 种类别参考：[references/categories.md](references/categories.md)

**收集完成后**，整理需求摘要（含类别）进入 Step 2。

---

## Step 2: 设计模式分析

基于 Step 1 的需求摘要，对 5 种设计模式进行多维度分析。这不是机械套用规则 — 大多数真实 skill 是多模式混合的，分析时解释推荐的原因。

首先确认 Step 1 中识别的 Skill 类别，加载 [references/category-pattern-matrix.md](references/category-pattern-matrix.md) 获取该类别的模式亲和度（★★★ → 主模式假设，★★ → 辅助模式假设），带着假设进入下面的信号分析验证。如果类别不明确，跳过假设直接进入信号分析。

### 2.1 逐模式评估

Step 1 收集的 7 个维度中，**输出特征、交互复杂度、领域知识、流程结构**是模式选择的 4 个核心信号维度（目标和输入特征已融入类别判断，类别走矩阵路径）。对每种模式评估其核心信号是否被触发：

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

### 2.4 选择写作最佳实践

基于确定的类别和模式，加载 [references/best-practices.md](references/best-practices.md)，从 9 条实践中选择 3-5 条最相关的：
- 从 [references/category-pattern-matrix.md](references/category-pattern-matrix.md) 获取类别推荐的 3 条实践
- 补充模式天然关联的实践（如 Tool Wrapper → 渐进披露）
- 去重后保留 3-5 条，每条附带针对该 skill 的具体应用说明

完成分析后直接进入 Step 3 生成建议文档，分析结果（模式、实践）体现在文档的对应章节中。

---

## Step 3: 输出设计建议

基于 Step 2 的模式分析，使用模板 [assets/templates/design-recommendation.md](assets/templates/design-recommendation.md) 生成完整的设计建议文档。

### 建议文档包含：

1. **需求摘要** — Step 1 收集的 7 个维度（含类别），结构化表格
2. **Skill 类别分析** — 识别的类别、类别特征、对设计决策的影响
3. **设计模式推荐** — 主模式 + 辅助模式 + 匹配理由（含类别亲和度参考）+ 未选模式说明
4. **推荐目录结构** — ASCII 树形图 + 各目录/文件的用途说明
5. **SKILL.md 章节结构建议** — Frontmatter（含 description 建议）+ 各章节标题和内容概要
6. **关键设计要点** — 3-5 条针对该模式组合的实操注意事项
7. **写作最佳实践** — Step 2.4 选择的 3-5 条实践 + 针对性应用说明
8. **与 skill-creator 的衔接** — 如何将本建议作为上下文传递给 skill-creator

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

## 2. Skill 类别分析
**代码脚手架与模板** — 脚手架类 skill 的核心在于模板质量和灵活度

## 3. 设计模式推荐
- **主模式：Generator** — 输出是结构化的 API 文档，有明确的模板结构
- **辅助模式：Tool Wrapper** — 需要加载 OpenAPI 规范作为参考知识

## 4. 推荐目录结构
api-doc-generator/
├── SKILL.md
├── assets/templates/
│   └── api-doc-template.md      # API 文档输出模板
└── references/
    └── openapi-spec.md          # OpenAPI 规范参考

## 7. 写作最佳实践
- **不要过度约束** — 模板定义文档骨架，具体内容让 agent 根据代码灵活生成
- **渐进披露** — OpenAPI 规范放 references/ 按需加载，不塞进 SKILL.md
- **Description 用于触发** — 覆盖"生成 API 文档"、"接口文档"等触发表述
```
