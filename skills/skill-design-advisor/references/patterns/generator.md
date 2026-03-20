# Generator 模式

## 定义

Generator 用模板驱动结构化内容的生成。skill 的核心价值在于"按固定格式产出高质量内容"，模板定义了输出的骨架，模型负责填充细节。

## 在 Claude Code 中的实现方式

### SKILL.md 结构建议

```markdown
---
name: <skill-name>
description: <描述 + 触发场景>
---

# <标题>

## 流程概述

1. 确认信息 — 与用户确认变量值
2. <核心步骤> — 按模板生成内容
3. 输出结果 — 写入文件

---

## Step 1: 确认信息

与用户确认以下变量：

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `variable_1` | ... | ... |
| `variable_2` | ... | ... |

## Step 2: <核心步骤>

读取模板 [assets/templates/<template>.md](assets/templates/<template>.md)，替换变量并生成内容。

<具体的生成指令和规则>

## Step 3: 输出

将生成内容写入 `<output_path>`。
```

### 推荐目录布局

```
<skill-name>/
├── SKILL.md                   # 生成流程和规则
└── assets/
    └── templates/
        └── <template>.md      # 输出模板（Markdown 或 .j2）
```

如果需要领域知识，叠加 Tool Wrapper：
```
<skill-name>/
├── SKILL.md
├── assets/templates/
│   └── <template>.md
└── references/
    └── <spec>.md              # 领域知识参考
```

### 关键设计要点

1. **模板是给模型看的参考**：Claude Code 无法执行模板引擎，"变量替换"由模型理解模板后生成
2. **模板可以用 `{{ variable }}` 占位**：帮助模型识别需要替换的位置
3. **Jinja2 (.j2) 或纯 Markdown 均可**：选哪种取决于团队习惯
4. **前置信息收集要简洁**：Generator 不是 Inversion，参数确认控制在 1-2 轮

## 典型示例

**Changelog 生成器**：
- SKILL.md 定义 3 步流程：收集版本号和变更列表 → 按模板生成 changelog → 写入文件
- `assets/templates/changelog.md` 定义固定格式，用 `{{ version }}`、`{{ date }}` 占位
- SKILL.md 中引用：`读取模板 [assets/templates/changelog.md](...) 并替换变量`

**周报生成器**：
- SKILL.md 定义 2 步流程：收集本周工作内容 → 按模板生成周报
- `assets/templates/weekly-report.md` 定义固定格式：本周完成、下周计划、风险项
- 模型读取模板后，根据用户口述填充各 section

## 适用场景

- 输出有固定的结构和格式
- 核心工作是"填充模板" — 变量替换 + 内容补充
- 不同用户/项目使用同一格式，只是内容不同
- 典型场景：项目脚手架、报告生成、配置文件生成、文档模板填充

## 不适用场景

- 输出格式不固定，每次差异很大（模板价值有限）
- 核心工作是"评审/检查"而非"生成"（用 Reviewer）
- 需要复杂的多轮需求挖掘才能确定输出内容（用 Inversion）

## 常见陷阱

1. **模板过于僵化**：模板应定义骨架，不是逐字逐句规定。给模型留有发挥空间
2. **无输出校验**：Claude Code 无框架级输出校验 — 如果输出质量很重要，叠加 Reviewer 模式添加完整性检查步骤
3. **前置参数收集过重**：如果需要 3+ 轮交互来确认参数，说明这不是纯 Generator，考虑叠加 Inversion

## 与其他模式的组合

- **Generator + Tool Wrapper**：最常见。模板定义输出格式，references 提供填充内容的领域知识
- **Generator + Reviewer**：生成后验证。在最终输出步骤后添加完整性检查
- **Inversion + Generator**：先采访收集信息，再按模板生成。Inversion 负责前半段，Generator 负责后半段
