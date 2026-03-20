# Pipeline 模式

## 定义

Pipeline 是严格的多步骤工作流，包含关键步骤的检查点（Checkpoints）。每一步有明确的输入、操作和输出，步骤之间需要用户确认才能继续。适用于复杂、高风险或需要人工审核的流程。

## 在 Claude Code 中的实现方式

### SKILL.md 结构建议

```markdown
---
name: <skill-name>
description: <描述 + 触发场景>
---

# <标题>

<方法论概述>

## 流程概述

```
Step1: <名称> → Step2: <名称> → ... → StepN: <名称>
```

**方法论参考**：[references/overview.md](references/overview.md)

---

## 前置准备

与用户确认以下信息：

| 参数 | 说明 | 默认值 |
|------|------|--------|
| ... | ... | ... |

---

## 交互原则

**歧义澄清**：<歧义类型和处理方式>

**阶段门禁**：每个 Step 结束时必须将产出展示给用户，经确认后才能进入下一 Step。

---

## Step 1: <步骤名称>

> 方法论：<参考> | 参考：[references/<detail>.md](references/<detail>.md)

**目标**：<本步目标>

### Step 1.1: <子步骤>
<操作流程>

### Step 1.2: <子步骤>
<操作流程>

**输出**：<本步产出>

**检查清单**：[assets/checklists/step1-checklist.md](assets/checklists/step1-checklist.md)

---

## Step 2: <步骤名称>
...
```

### 推荐目录布局

```
<skill-name>/
├── SKILL.md                        # 流程编排（步骤序列 + 交互规则）
├── assets/
│   ├── checklists/                 # 各步骤检查清单
│   │   ├── step1-checklist.md
│   │   ├── step2-checklist.md
│   │   └── completeness-checklist.md
│   └── templates/                  # 输出模板
│       └── <output-template>.md
└── references/
    ├── overview.md                 # 方法论概述
    └── <topic>/                    # 按主题分组的参考文档
        ├── <detail-1>.md
        └── <detail-2>.md
```

这是最复杂的目录结构，通常三层资源齐备：checklists + templates + references。

### 关键设计要点

1. **每步有明确的"目标 → 操作 → 输出 → 检查"结构**
2. **方法论分离到 references/**：SKILL.md 写流程编排，不写方法论细节
3. **阶段门禁是核心**：每步结束必须用户确认。这是 Pipeline 区别于其他模式的标志
4. **检查清单驱动质量**：每步末尾引用对应的 checklist 做完整性验证
5. **控制步骤数量**：建议 3-7 步。超过 7 步考虑拆分为多个 skill

## 典型示例

**技术方案评审流程**：
- 5 步流程（明确目标 → 梳理现状 → 设计方案 → 风险评估 → 输出文档）
- 每步有子步骤（Step 1.1, 1.2...），粒度可控
- `references/` 按主题分组存放评审标准和方法论
- `assets/checklists/` 每步一个检查清单 + 最终完整性检查
- `assets/templates/` 各步骤的输出文档模板
- 阶段门禁：每个 Step 结束须用户确认

关键结构特征：
```
前置准备 → Step1（子步骤 + 门禁）→ Step2（子步骤 + 门禁）→ ... → StepN（总结输出）
```

**数据库迁移流程**：
- 3 步流程：分析现有 schema → 生成迁移脚本 → 验证兼容性
- 每步需人工确认（避免破坏性操作）
- checklists 确保每步不遗漏关键检查项

## 适用场景

- 复杂流程，步骤间有严格的先后顺序
- 每步产出需要用户审核确认
- 涉及方法论框架（如评审流程、设计流程、分析流程等）
- 高风险操作需要人工把关
- 典型场景：系统设计、需求分析、项目规划、迁移计划

## 不适用场景

- 只有 1-2 个步骤（过度工程化）
- 步骤间没有依赖关系（不需要顺序执行）
- 全自动执行，不需要人工确认（用 Generator 或 Tool Wrapper）

## 常见陷阱

1. **在单个 SKILL.md 中定义过长的 Pipeline**：超过 500 行时，将方法论细节拆到 references/
2. **阶段门禁失效**：Claude Code 无框架保障，长流程后模型可能跳过确认
   - 缓解：每步开头加"确认上一步已获用户批准"
   - 缓解：在每步输出 checklist 验证结果，增强执行纪律
3. **步骤过多**：超过 7 步时注意力稀释风险显著增加，考虑拆分
4. **checklist 缺失**：Pipeline 没有 checklist 就失去了质量保障的核心机制
5. **方法论和流程混在一起**：SKILL.md 应是"做什么"，references 是"为什么这么做"

## 与其他模式的组合

- **Pipeline + Reviewer**：最自然的搭配。每步末尾用 checklist 做 Reviewer 子流程
- **Pipeline + Generator**：某些步骤的产出用模板驱动生成
- **Pipeline + Inversion**：Step 1 作为信息收集阶段，采用 Inversion 模式
- **Pipeline + Tool Wrapper**：步骤中按需加载领域知识/规范文档
