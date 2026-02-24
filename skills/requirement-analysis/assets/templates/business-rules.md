# 业务规则文档

**项目名称**：{{project_name}}
**版本**：{{version}}
**日期**：{{date}}

---

## 规则概览

| 统计项 | 数量 |
|--------|------|
| 计算规则 | {{calc_rules_count}} |
| 验证规则 | {{validation_rules_count}} |
| 推理规则 | {{inference_rules_count}} |
| 约束规则 | {{constraint_rules_count}} |
| 触发规则 | {{trigger_rules_count}} |
| **总计** | {{total_rules_count}} |

---

## 规则清单

### 计算规则

| ID | 规则名称 | 优先级 | 适用范围 |
|----|----------|--------|----------|
| BR-CALC-001 | {{calc_rule_1_name}} | {{calc_rule_1_priority}} | {{calc_rule_1_scope}} |
| BR-CALC-002 | {{calc_rule_2_name}} | {{calc_rule_2_priority}} | {{calc_rule_2_scope}} |

### 验证规则

| ID | 规则名称 | 优先级 | 适用范围 |
|----|----------|--------|----------|
| BR-VAL-001 | {{val_rule_1_name}} | {{val_rule_1_priority}} | {{val_rule_1_scope}} |
| BR-VAL-002 | {{val_rule_2_name}} | {{val_rule_2_priority}} | {{val_rule_2_scope}} |

### 约束规则

| ID | 规则名称 | 优先级 | 适用范围 |
|----|----------|--------|----------|
| BR-CON-001 | {{con_rule_1_name}} | {{con_rule_1_priority}} | {{con_rule_1_scope}} |

---

## 规则详情

### BR-CALC-001: {{calc_rule_1_name}}

**基本信息**：

| 项目 | 内容 |
|------|------|
| 规则ID | BR-CALC-001 |
| 规则名称 | {{calc_rule_1_name}} |
| 类型 | 计算规则 |
| 优先级 | {{calc_rule_1_priority}} |
| 来源 | {{calc_rule_1_source}} |
| 生效日期 | {{calc_rule_1_effective}} |

**描述**：
{{calc_rule_1_description}}

**触发条件**：
{{calc_rule_1_trigger}}

**规则逻辑**：

```
{{calc_rule_1_logic}}
```

**示例**：

| 输入 | 计算过程 | 输出 |
|------|----------|------|
| {{calc_example_1_input}} | {{calc_example_1_process}} | {{calc_example_1_output}} |

**异常处理**：
{{calc_rule_1_exception}}

**关联用例**：
- UC-{{calc_rule_1_uc}}

---

### BR-VAL-001: {{val_rule_1_name}}

**基本信息**：

| 项目 | 内容 |
|------|------|
| 规则ID | BR-VAL-001 |
| 规则名称 | {{val_rule_1_name}} |
| 类型 | 验证规则 |
| 优先级 | {{val_rule_1_priority}} |
| 来源 | {{val_rule_1_source}} |

**描述**：
{{val_rule_1_description}}

**触发条件**：
{{val_rule_1_trigger}}

**验证逻辑**：

| 条件 | 有效值 | 错误提示 |
|------|--------|----------|
| {{val_condition_1}} | {{val_valid_1}} | {{val_error_1}} |
| {{val_condition_2}} | {{val_valid_2}} | {{val_error_2}} |

**关联用例**：
- UC-{{val_rule_1_uc}}

---

### BR-CON-001: {{con_rule_1_name}}

**基本信息**：

| 项目 | 内容 |
|------|------|
| 规则ID | BR-CON-001 |
| 规则名称 | {{con_rule_1_name}} |
| 类型 | 约束规则 |
| 优先级 | {{con_rule_1_priority}} |
| 来源 | {{con_rule_1_source}} |

**描述**：
{{con_rule_1_description}}

**约束条件**：
{{con_rule_1_condition}}

**违反时处理**：
{{con_rule_1_violation}}

**关联用例**：
- UC-{{con_rule_1_uc}}

---

## 决策表

### {{decision_table_name}}

**说明**：{{decision_table_desc}}

| 条件/动作 | 规则1 | 规则2 | 规则3 | 规则4 |
|-----------|-------|-------|-------|-------|
| **条件** | | | | |
| {{condition_1}} | {{c1_r1}} | {{c1_r2}} | {{c1_r3}} | {{c1_r4}} |
| {{condition_2}} | {{c2_r1}} | {{c2_r2}} | {{c2_r3}} | {{c2_r4}} |
| **动作** | | | | |
| {{action_1}} | {{a1_r1}} | {{a1_r2}} | {{a1_r3}} | {{a1_r4}} |

---

## 规则依赖关系

```mermaid
graph TD
    BR1[BR-CALC-001] --> BR2[BR-VAL-001]
    BR2 --> BR3[BR-CON-001]

    {{rule_dependency_diagram}}
```

---

## 规则变更历史

| 规则ID | 版本 | 日期 | 变更内容 | 变更人 |
|--------|------|------|----------|--------|
| {{change_rule_id}} | {{change_version}} | {{change_date}} | {{change_content}} | {{change_author}} |

---

## 待确认规则

| 编号 | 规则描述 | 待确认内容 | 相关方 | 状态 |
|------|----------|------------|--------|------|
| TBD-001 | {{tbd_rule_1}} | {{tbd_content_1}} | {{tbd_party_1}} | {{tbd_status_1}} |

---

## 备注

{{notes}}
