# {{project_name}} 需求规格说明书

**版本**：1.0
**日期**：{{date}}
**状态**：{{status}}

---

## 文档信息

| 项目 | 内容 |
|------|------|
| 项目名称 | {{project_name}} |
| 文档版本 | 1.0 |
| 创建日期 | {{date}} |
| 分析深度 | {{analysis_depth}} |

### 修订历史

| 版本 | 日期 | 修订人 | 修订内容 |
|------|------|--------|----------|
| 1.0 | {{date}} | {{author}} | 初始版本 |

---

## 1. 概述

### 1.1 项目背景

{{project_background}}

### 1.2 项目目标

{{project_goals}}

### 1.3 项目范围

**范围内**：
{{in_scope}}

**范围外**：
{{out_of_scope}}

### 1.4 术语定义

| 术语 | 定义 |
|------|------|
| {{term_1}} | {{definition_1}} |
| {{term_2}} | {{definition_2}} |

---

## 2. 干系人分析

### 2.1 干系人清单

| 干系人 | 角色 | 期望 | 影响力 | 利益关系 |
|--------|------|------|--------|----------|
| {{stakeholder_1}} | {{role_1}} | {{expectation_1}} | {{influence_1}} | {{interest_1}} |
| {{stakeholder_2}} | {{role_2}} | {{expectation_2}} | {{influence_2}} | {{interest_2}} |

### 2.2 干系人管理策略

{{stakeholder_strategy}}

---

## 3. 系统分解

### 3.1 主题域划分

| 主题域 | 描述 | 核心职责 |
|--------|------|----------|
| {{subject_area_1}} | {{sa_desc_1}} | {{sa_resp_1}} |
| {{subject_area_2}} | {{sa_desc_2}} | {{sa_resp_2}} |

### 3.2 主题域关系图

```mermaid
graph TB
    subgraph 系统边界
        {{subject_area_diagram}}
    end
```

---

## 4. 业务事件与报表

### 4.1 业务事件清单

| ID | 事件名称 | 类型 | 触发者 | 主题域 | 优先级 |
|----|----------|------|--------|--------|--------|
| E-001 | {{event_1}} | {{type_1}} | {{trigger_1}} | {{sa_1}} | {{priority_1}} |
| E-002 | {{event_2}} | {{type_2}} | {{trigger_2}} | {{sa_2}} | {{priority_2}} |

### 4.2 报表清单

| ID | 报表名称 | 用途 | 使用者 | 更新频率 |
|----|----------|------|--------|----------|
| R-001 | {{report_1}} | {{purpose_1}} | {{user_1}} | {{frequency_1}} |

### 4.3 管控点清单

| ID | 管控点 | 类型 | 触发条件 | 处理方式 |
|----|--------|------|----------|----------|
| CP-001 | {{control_point_1}} | {{cp_type_1}} | {{cp_condition_1}} | {{cp_action_1}} |

---

## 5. 领域模型

### 5.1 核心实体

| 实体 | 定义 | 关键属性 |
|------|------|----------|
| {{entity_1}} | {{entity_def_1}} | {{entity_attrs_1}} |
| {{entity_2}} | {{entity_def_2}} | {{entity_attrs_2}} |

### 5.2 实体关系

| 源实体 | 关系 | 目标实体 | 说明 |
|--------|------|----------|------|
| {{source_1}} | {{relation_1}} | {{target_1}} | {{rel_desc_1}} |

### 5.3 领域模型图

```mermaid
erDiagram
    {{domain_model_diagram}}
```

---

## 6. 用例模型

### 6.1 参与者

| 参与者 | 类型 | 说明 |
|--------|------|------|
| {{actor_1}} | {{actor_type_1}} | {{actor_desc_1}} |

### 6.2 用例清单

#### 业务支持功能

| ID | 用例名称 | 参与者 | 优先级 | 状态 |
|----|----------|--------|--------|------|
| UC-B001 | {{usecase_b1}} | {{uc_actor_b1}} | {{uc_priority_b1}} | {{uc_status_b1}} |

#### 管理支持功能

| ID | 用例名称 | 参与者 | 优先级 | 状态 |
|----|----------|--------|--------|------|
| UC-M001 | {{usecase_m1}} | {{uc_actor_m1}} | {{uc_priority_m1}} | {{uc_status_m1}} |

#### 维护支持功能

| ID | 用例名称 | 参与者 | 优先级 | 状态 |
|----|----------|--------|--------|------|
| UC-S001 | {{usecase_s1}} | {{uc_actor_s1}} | {{uc_priority_s1}} | {{uc_status_s1}} |

### 6.3 用例图

```mermaid
graph LR
    {{usecase_diagram}}
```

---

## 7. 详细功能需求

### 7.1 用例详情

{{detailed_usecases}}

---

## 8. 数据需求

### 8.1 实体属性详情

{{entity_details}}

### 8.2 数据字典

| 字段名 | 数据类型 | 长度 | 必填 | 说明 |
|--------|----------|------|------|------|
| {{field_1}} | {{type_1}} | {{length_1}} | {{required_1}} | {{desc_1}} |

---

## 9. 质量需求

### 9.1 性能需求

| 指标 | 目标值 | 测试条件 |
|------|--------|----------|
| 响应时间 | {{response_time}} | {{test_condition_1}} |
| 吞吐量 | {{throughput}} | {{test_condition_2}} |
| 并发用户 | {{concurrent_users}} | {{test_condition_3}} |

### 9.2 可靠性需求

| 指标 | 目标值 |
|------|--------|
| 可用性 | {{availability}} |
| MTBF | {{mtbf}} |
| MTTR | {{mttr}} |

### 9.3 安全需求

{{security_requirements}}

### 9.4 其他质量需求

{{other_quality_requirements}}

---

## 10. 业务规则

| ID | 规则名称 | 类型 | 描述 | 优先级 |
|----|----------|------|------|--------|
| BR-001 | {{rule_1}} | {{rule_type_1}} | {{rule_desc_1}} | {{rule_priority_1}} |

---

## 11. 约束条件

### 11.1 技术约束

{{technical_constraints}}

### 11.2 组织约束

{{organizational_constraints}}

### 11.3 法规约束

{{regulatory_constraints}}

---

## 12. 附录

### 12.1 参考资料

{{references}}

### 12.2 待确认事项

| 编号 | 事项 | 相关方 | 状态 |
|------|------|--------|------|
| TBD-001 | {{tbd_1}} | {{tbd_party_1}} | {{tbd_status_1}} |
