# Google ADK 模式 → Claude Code SKILL.md 映射

Google 的 5 种 Agent Skill 设计模式源自 ADK（Agent Development Kit），有代码级编排能力。Claude Code 的 skill 通过 SKILL.md 纯文本指令控制，没有运行时状态管理。本文档建立两者之间的映射桥梁。

## 核心差异

| 能力 | Google ADK | Claude Code |
|------|-----------|-------------|
| 指令定义 | Python 代码 + SkillToolset | SKILL.md 纯文本 |
| 资源加载 | `LoadSkillResourceTool` 框架级支持 | SKILL.md 中用 Markdown 链接引用 |
| 阶段控制 | `SequentialAgent` + `before_tool_callback` | 自然语言"阶段门禁"（无框架保障） |
| 输出校验 | 可接 Pydantic 校验 | 无框架级校验，依赖模型遵循指令 |
| 状态管理 | `tool_context.state` 持久状态 | 无持久状态，依赖上下文窗口 |

## 映射表

| ADK 模式 | Claude Code 对应范式 | 实现机制 | 典型场景 |
|----------|---------------------|---------|---------|
| **Tool Wrapper** | `references/` 渐进加载 | SKILL.md 用链接指向 references/ 文件，模型按需读取 | 加载编码规范、API 文档、协议规格 |
| **Generator** | `assets/templates/` + 步骤化生成 | SKILL.md 指示读取模板文件，按步骤填充变量 | 项目脚手架、报告生成、配置文件模板 |
| **Reviewer** | `assets/checklists/` + 检查清单 | SKILL.md 指示读取 checklist 文件，逐项评估 | 代码审查、合规检查、阶段验证 |
| **Inversion** | `前置准备` + `交互原则` | SKILL.md 设置参数收集表 + 歧义澄清规则 + 阶段门禁 | 需求分析、方案咨询、个性化配置 |
| **Pipeline** | 多 Step 流程 + checklists 门禁 | SKILL.md 按 Step 1/2/3... 组织，每步末尾设门禁确认 | 系统设计、迁移计划、多阶段审批 |

## Claude Code 渐进加载机制

Claude Code 的 skill 有三层加载：

```
L1: Metadata（name + description）  → 始终在上下文中（~100词）
L2: SKILL.md body                   → skill 触发时加载（建议 <500行）
L3: references/ + assets/            → 按需读取（无上限）
```

这对应了 Tool Wrapper 模式的核心理念：不要一次性加载所有知识，而是让模型按需获取。

**在 SKILL.md 中引用资源的写法**：
```markdown
> 详细方法论：[references/some-guide.md](references/some-guide.md)
```
模型会在需要时主动读取链接指向的文件。

## ADK 能做但 Claude Code 不能做的事

| ADK 能力 | Claude Code 替代方案 | 风险 |
|----------|---------------------|------|
| `before_tool_callback` 阶段拦截 | SKILL.md 中写"每步结束需用户确认后才能进入下一步" | 5-10轮后模型可能跳过确认 |
| `require_confirmation=True` 暂停 | SKILL.md 中写"执行操作前必须展示给用户确认" | 依赖模型遵循度 |
| `SequentialAgent` 顺序编排 | SKILL.md 中按 Step 1→2→3 线性组织 | 无框架保障执行顺序 |
| `tool_context.state` 状态持久化 | 依赖上下文窗口保持状态 | 长对话可能丢失早期信息 |
| Pydantic 输出校验 | SKILL.md 中定义输出模板 + 完整性检查步骤 | 无自动校验 |

## 缓解策略

针对 Claude Code 的局限性，推荐以下实践：

1. **阶段门禁弱化风险**：在每步开头加"回顾上一步确认结果"，强化上下文连续性
2. **长流程注意力稀释**：将超过 5 步的 Pipeline 考虑拆分为多个 skill
3. **输出质量保障**：添加独立的"完整性检查"步骤作为 Reviewer 子流程
4. **状态丢失防护**：在关键步骤输出结构化摘要，作为后续步骤的输入锚点
