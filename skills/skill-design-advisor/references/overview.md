# Agent Skill 设计模式全景

Google 提出的 5 种 Agent Skill 设计模式，覆盖了从简单工具封装到复杂多步骤流程的全部场景。

## 5 种模式速览

| 模式 | 一句话定义 | 最佳适用场景 | 核心特征 |
|------|-----------|-------------|---------|
| **Tool Wrapper** | 让 Agent 按需加载领域知识和规范文档 | 需要引用外部文档/规范，skill 本身逻辑简单 | 渐进加载、按需读取 |
| **Generator** | 用模板驱动结构化文档或代码的生成 | 输出有固定格式，需要模板填充 | 模板驱动、变量替换 |
| **Reviewer** | 用检查清单对内容进行结构化评审 | 代码审查、合规检查、质量评估 | 检查清单、评分输出 |
| **Inversion** | 先采访用户收集完整信息，再执行任务 | 需求不明确，需要多轮交互挖掘 | 采访先行、阶段门控 |
| **Pipeline** | 严格的多步骤工作流，含阶段检查点 | 复杂顺序流程，需要人工确认 | 多步骤、阶段门禁 |

## 决策流程图

```mermaid
flowchart TD
    Start([分析 Skill 需求]) --> Q1{输出形态是什么?}

    Q1 -->|检查报告/评估结果| R[Reviewer]
    Q1 -->|生成文件/文档| Q2{需要多轮交互?}
    Q1 -->|执行操作/分析建议| Q3{流程复杂度?}

    Q2 -->|≥3轮，需挖掘需求| Q4{步骤有严格顺序?}
    Q2 -->|<3轮| Q5{有模板结构?}

    Q4 -->|是，且需阶段确认| P[Pipeline]
    Q4 -->|否，收集完再执行| I[Inversion]

    Q5 -->|是| G[Generator]
    Q5 -->|否| TW[Tool Wrapper]

    Q3 -->|多步骤+阶段门禁| P
    Q3 -->|简单线性| Q6{需要领域知识?}

    Q6 -->|是| TW
    Q6 -->|否| G

    R --> Mix{评估辅助模式}
    G --> Mix
    TW --> Mix
    I --> Mix
    P --> Mix

    Mix --> Done([输出设计建议])
```

> 此流程图为参考指引，实际分析应基于多维度信号综合判断，而非机械沿图走。大多数真实 skill 是多模式混合的。

## 模式组合矩阵

| 主模式 \ 辅助 | Tool Wrapper | Generator | Reviewer | Inversion | Pipeline |
|-------------|-------------|-----------|----------|-----------|----------|
| **Tool Wrapper** | — | 常见 | 偶尔 | 少见 | 少见 |
| **Generator** | 常见 | — | 常见 | 偶尔 | 少见 |
| **Reviewer** | 常见 | 偶尔 | — | 少见 | 偶尔 |
| **Inversion** | 偶尔 | 常见 | 偶尔 | — | 少见 |
| **Pipeline** | 常见 | 常见 | 常见 | 偶尔 | — |

各模式详细参考：
- [Tool Wrapper](patterns/tool-wrapper.md)
- [Generator](patterns/generator.md)
- [Reviewer](patterns/reviewer.md)
- [Inversion](patterns/inversion.md)
- [Pipeline](patterns/pipeline.md)

模式选择详细指南：[decision-guide.md](decision-guide.md)
ADK → Claude Code 映射详解：[pattern-mapping.md](pattern-mapping.md)
