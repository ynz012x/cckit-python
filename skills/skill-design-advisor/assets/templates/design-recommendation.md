# {{ skill_name }} — Skill 设计建议

## 1. 需求摘要

| 维度 | 分析结果 |
|------|---------|
| **目标** | {{ skill_purpose }} |
| **输入特征** | {{ input_type }} |
| **输出特征** | {{ output_type }} |
| **交互复杂度** | {{ interaction_complexity }} |
| **领域知识** | {{ domain_knowledge }} |
| **流程结构** | {{ process_structure }} |

## 2. 设计模式推荐

### 主模式：{{ primary_pattern }}

**匹配理由**：{{ primary_reason }}

**在 Claude Code 中的实现方式**：{{ primary_implementation }}

### 辅助模式：{{ secondary_pattern }}

**组合理由**：{{ secondary_reason }}

### 未选模式说明

| 模式 | 不推荐理由 |
|------|-----------|
| {{ unused_pattern_1 }} | {{ unused_reason_1 }} |
| {{ unused_pattern_2 }} | {{ unused_reason_2 }} |
| {{ unused_pattern_3 }} | {{ unused_reason_3 }} |

## 3. 推荐目录结构

```
{{ skill_name }}/
├── SKILL.md                          # {{ skill_md_description }}
{{ directory_tree }}
```

各目录/文件用途：
{{ directory_explanations }}

## 4. SKILL.md 章节结构建议

### Frontmatter

```yaml
---
name: {{ skill_name }}
description: {{ suggested_description }}
---
```

### 章节规划

{{ chapter_plan }}

## 5. 关键设计要点

{{ design_points }}

## 6. 与 skill-creator 的衔接

将本设计建议作为上下文传递给 `skill-creator`：

1. 将本文档的"目录结构"和"章节规划"作为 skill-creator 的输入参考
2. 在 skill-creator 的 Interview 阶段，重点关注上述设计要点
3. 编写 SKILL.md 时按照推荐的章节结构组织内容
4. 测试时验证是否遵循了推荐模式的核心特征

---

*本建议由 skill-design-advisor 基于 Google 5大 Agent Skill 设计模式生成*
