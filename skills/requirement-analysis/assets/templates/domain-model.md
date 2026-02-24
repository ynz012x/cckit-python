# 领域模型文档

**项目名称**：{{project_name}}
**版本**：{{version}}
**日期**：{{date}}

---

## 实体清单

| 编号 | 实体名称 | 类型 | 所属主题域 | 说明 |
|------|----------|------|------------|------|
| ENT-001 | {{entity_1}} | 实体 | {{domain_1}} | {{desc_1}} |
| ENT-002 | {{entity_2}} | 实体 | {{domain_2}} | {{desc_2}} |
| ENT-003 | {{entity_3}} | 值对象 | {{domain_3}} | {{desc_3}} |

---

## 实体详情

### ENT-001: {{entity_1}}

**基本信息**：
- 实体名称：{{entity_1}}
- 所属主题域：{{domain_1}}
- 类型：实体 / 值对象

**定义**：
{{entity_definition_1}}

**关键属性**：

| 属性名 | 数据类型 | 必填 | 说明 |
|--------|----------|------|------|
| id | String | 是 | 唯一标识 |
| {{attr_1_1}} | {{type_1_1}} | {{required_1_1}} | {{attr_desc_1_1}} |
| {{attr_1_2}} | {{type_1_2}} | {{required_1_2}} | {{attr_desc_1_2}} |
| {{attr_1_3}} | {{type_1_3}} | {{required_1_3}} | {{attr_desc_1_3}} |
| created_at | DateTime | 是 | 创建时间 |
| updated_at | DateTime | 是 | 更新时间 |

**关系**：

| 关系类型 | 目标实体 | 基数 | 说明 |
|----------|----------|------|------|
| {{rel_type_1_1}} | {{rel_target_1_1}} | {{cardinality_1_1}} | {{rel_desc_1_1}} |
| {{rel_type_1_2}} | {{rel_target_1_2}} | {{cardinality_1_2}} | {{rel_desc_1_2}} |

**业务规则**：
- {{rule_1_1}}
- {{rule_1_2}}

---

### ENT-002: {{entity_2}}

**基本信息**：
- 实体名称：{{entity_2}}
- 所属主题域：{{domain_2}}
- 类型：实体 / 值对象

**定义**：
{{entity_definition_2}}

**关键属性**：

| 属性名 | 数据类型 | 必填 | 说明 |
|--------|----------|------|------|
| id | String | 是 | 唯一标识 |
| {{attr_2_1}} | {{type_2_1}} | {{required_2_1}} | {{attr_desc_2_1}} |
| {{attr_2_2}} | {{type_2_2}} | {{required_2_2}} | {{attr_desc_2_2}} |

**关系**：

| 关系类型 | 目标实体 | 基数 | 说明 |
|----------|----------|------|------|
| {{rel_type_2_1}} | {{rel_target_2_1}} | {{cardinality_2_1}} | {{rel_desc_2_1}} |

---

## 实体关系汇总

| 源实体 | 关系 | 目标实体 | 基数 | 说明 |
|--------|------|----------|------|------|
| {{source_1}} | {{relation_1}} | {{target_1}} | {{card_1}} | {{rel_sum_desc_1}} |
| {{source_2}} | {{relation_2}} | {{target_2}} | {{card_2}} | {{rel_sum_desc_2}} |

---

## 领域模型图

```mermaid
erDiagram
    {{entity_1}} ||--o{ {{entity_2}} : contains
    {{entity_2}} }o--|| {{entity_3}} : belongs_to

    {{entity_1}} {
        string id PK
        {{type_1_1}} {{attr_1_1}}
        {{type_1_2}} {{attr_1_2}}
        datetime created_at
        datetime updated_at
    }

    {{entity_2}} {
        string id PK
        string {{entity_1}}_id FK
        {{type_2_1}} {{attr_2_1}}
        {{type_2_2}} {{attr_2_2}}
    }
```

---

## 枚举类型

### {{enum_1}}

{{enum_definition_1}}

| 值 | 显示名 | 说明 |
|----|--------|------|
| {{enum_value_1_1}} | {{enum_label_1_1}} | {{enum_desc_1_1}} |
| {{enum_value_1_2}} | {{enum_label_1_2}} | {{enum_desc_1_2}} |

---

## 主数据与参考数据

### 主数据

| 实体 | 说明 | 数据治理要求 |
|------|------|--------------|
| {{master_data_1}} | {{master_desc_1}} | {{master_gov_1}} |

### 参考数据

| 数据项 | 说明 | 来源 |
|--------|------|------|
| {{ref_data_1}} | {{ref_desc_1}} | {{ref_source_1}} |

---

## 备注

{{notes}}
