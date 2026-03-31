---
name: memory-synchronizer
description: 自动检测项目变更，增量同步 CLAUDE.md 项目记忆。支持多层级 CLAUDE.md 体系，各文件仅负责对应目录作用域。当你新增了模块/依赖/命令后想同步项目记忆、项目结构变了需要更新 CLAUDE.md、CLAUDE.md 过时需要刷新、想检查项目记忆是否需要更新时，请使用此 skill。
disable-model-invocation: true
---

# Memory Synchronizer

自动检测项目变更，增量同步多层级 CLAUDE.md 项目记忆体系。

## 流程概述

1. **变更检测与记忆审计** — 发现所有 CLAUDE.md，构建作用域树，检测各自管辖范围内的结构性变更
2. **生成更新提案** — 为每个需要更新的 CLAUDE.md 生成增量修改方案，确保 <= 200 行
3. **执行写入** — 用户确认后执行文件修改

结构规范参考：[references/claudemd-conventions.md](references/claudemd-conventions.md)

---

## 交互原则

**阶段门禁**：Step 2 结束时必须将完整提案展示给用户，经确认后才能进入 Step 3。

**无变更提前终止**：如果 Step 1 未检测到相关变更，报告"项目记忆已是最新"并终止。

---

## Step 1: 变更检测与记忆审计

**目标**：发现项目中所有 CLAUDE.md，确定各自管辖范围，识别需要同步的变更。

### Step 1.1: 记忆体系发现

扫描项目中所有 CLAUDE.md 文件：

```bash
find . -name "CLAUDE.md" -not -path "./.git/*"
```

构建**作用域树**：

- 每个 CLAUDE.md 管辖其所在目录及子目录
- 子目录有自己的 CLAUDE.md 时，从父级作用域中**排除该子树**
- 对每个变更文件，按**最长前缀匹配**路由到对应的 CLAUDE.md

示例：

```
./CLAUDE.md          → 管辖 ./**，排除 backend/** 和 frontend/**
./backend/CLAUDE.md  → 管辖 backend/**
./frontend/CLAUDE.md → 管辖 frontend/**
```

特殊情况：根目录 CLAUDE.md 不存在 → 扫描项目结构，进入"全新创建"模式（生成初始 CLAUDE.md）。子目录 CLAUDE.md 不存在时**不主动创建**，仅更新已有的。

### Step 1.2: Git 变更检测

对每个 CLAUDE.md，检测其管辖范围内的变更：

```bash
# 定位该 CLAUDE.md 最后修改的 commit
git log -1 --format="%H" -- <path>/CLAUDE.md

# 获取自该 commit 以来的已提交变更
git diff <hash>..HEAD --name-status

# 获取工作区和暂存区的未提交变更
git diff --name-status
git diff --cached --name-status
```

合并三部分变更，按作用域路由规则分配到对应的 CLAUDE.md。

### Step 1.3: 变更分类与过滤

> 参考：[references/change-mapping.md](references/change-mapping.md)

对每个 CLAUDE.md 管辖范围内的变更，判断是否为**结构性变更**（影响项目级知识）：

| 检测（高相关） | 忽略 |
|---|---|
| 新增/删除目录结构 | 业务代码修改 |
| 构建/配置文件变更 | 测试用例代码 |
| 依赖文件变更 | 代码格式化变更 |
| CI/CD 配置变更 | lock 文件、资源文件 |

所有 CLAUDE.md 均无相关变更 → 报告"项目记忆已是最新"，**终止流程**。

### Step 1.4: 现状审计

对每个需要更新的 CLAUDE.md：

1. 读取全文内容
2. 按 `##` 标题解析 section 结构（无 `##` 时 fallback 到 `#` 或空行分段）
3. 统计：总行数、各 section 名称和行数
4. 计算剩余行数预算：200 - 当前行数

**输出**：各 CLAUDE.md 的审计摘要（section 列表 + 行数 + 预算余量 + 关联变更列表）

---

## Step 2: 生成更新提案

**目标**：为每个需要更新的 CLAUDE.md 生成独立的增量修改方案。

> 参考：[references/budget-management.md](references/budget-management.md)

### Step 2.1: Section 级修改规划

对每个需要更新的 CLAUDE.md：

1. **语义匹配**变更到现有 section — 按内容特征（表格、代码块、标题关键词）匹配，不硬编码 section 名
2. 确定修改动作：
   - **更新现有行**：如 Skills 表格新增/删除行、命令列表追加条目
   - **新增 section**：如项目首次引入 Docker，需要新增部署 section
   - **删除过时内容**：如被移除的功能、废弃的依赖
3. **增量原则**：只修改受影响的 section，未涉及的 section 保持原样
4. **内容合规**：仅写入项目级知识 — 构建/测试命令、编码规范、架构决策、命名约定、常用工作流。不写个人偏好或业务逻辑

### Step 2.2: 行数预算检查

预估修改后每个 CLAUDE.md 的总行数：

- **<= 200 行**：通过，进入 Step 2.3
- **> 200 行**：执行拆分决策：
  1. 先尝试精简（删除过时内容、合并重复、压缩描述）
  2. 仍超标时，按 section 优先级（P0 项目概览 > P1 命令 > ... > P7 详细说明）选择拆分对象
  3. 将低优先级 + 高行数的 section 提取到同级 `docs/` 目录（如 `docs/ARCHITECTURE.md`）
  4. CLAUDE.md 中替换为：section 标题 + 一句概述 + 引用链接（约 3 行）
  5. 重新计算行数，如仍超标继续拆分下一个 section

外部文件命名使用 **UPPER_SNAKE_CASE**，从 section 标题派生。

### Step 2.3: 提案展示

使用 [assets/checklists/proposal-checklist.md](assets/checklists/proposal-checklist.md) 自检提案完整性后，向用户展示：

**按 CLAUDE.md 文件分组展示**：

对每个文件展示：

1. **变更摘要**：检测到哪些结构性变更，归类为什么
2. **修改 diff 预览**：逐 section 展示具体的行级增删（用 `+`/`-` 标记）
3. **行数变化**：`当前 X 行 → 修改后 Y 行（预算剩余 Z 行）`
4. **拆分计划**（如有）：列出将创建/更新的外部文件及内容概要

**[阶段门禁]**：等待用户确认。用户可要求调整方案后重新展示。

---

## Step 3: 执行写入

**目标**：按用户确认的方案修改文件。

### Step 3.1: 写入变更

逐个 CLAUDE.md 文件执行：

1. 如有外部文件需创建/更新 → 先创建 `docs/` 目录（如不存在），写入外部文件
2. 增量修改 CLAUDE.md → 按 section 定位，精确修改受影响的行，保持其余内容不变
3. 全新创建模式（CLAUDE.md 不存在时）→ 基于项目当前结构生成完整的初始 CLAUDE.md

### Step 3.2: 验证与报告

1. 统计每个修改后的 CLAUDE.md 行数，确认 <= 200 行
2. 输出完成报告：

```
已更新文件：
  - ./CLAUDE.md（58 行 → 65 行）
  - ./backend/CLAUDE.md（42 行 → 48 行）
新建文件：
  - ./docs/ARCHITECTURE.md（从 CLAUDE.md 拆分）
```
