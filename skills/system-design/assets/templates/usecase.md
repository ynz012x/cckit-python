# 用例描述

## 基本信息

| 项目 | 内容 |
|------|------|
| 用例ID | UC-{{usecase_id}} |
| 用例名称 | {{usecase_name}} |
| 主题域 | {{subject_area}} |
| 类型 | 业务支持 / 管理支持 / 维护支持 |
| 优先级 | 高 / 中 / 低 |
| 复杂度 | 高 / 中 / 低 |
| 状态 | 草稿 / 评审中 / 已确认 |

---

## 概述

{{usecase_description}}

---

## 参与者

| 类型 | 参与者 | 说明 |
|------|--------|------|
| 主要参与者 | {{primary_actor}} | {{primary_actor_desc}} |
| 次要参与者 | {{secondary_actor}} | {{secondary_actor_desc}} |

---

## 前置条件

- {{precondition_1}}
- {{precondition_2}}
- {{precondition_3}}

---

## 后置条件

**成功时**：
- {{postcondition_success_1}}
- {{postcondition_success_2}}

**失败时**：
- {{postcondition_failure_1}}

---

## 主成功场景

| 步骤 | 参与者 | 系统 |
|------|--------|------|
| 1 | {{step_1_actor}} | {{step_1_system}} |
| 2 | {{step_2_actor}} | {{step_2_system}} |
| 3 | {{step_3_actor}} | {{step_3_system}} |
| 4 | {{step_4_actor}} | {{step_4_system}} |
| 5 | {{step_5_actor}} | {{step_5_system}} |

---

## 扩展场景

### {{extension_step}}a. {{extension_condition_1}}

**触发条件**：{{extension_trigger_1}}

| 步骤 | 操作 |
|------|------|
| 1 | {{ext_action_1_1}} |
| 2 | {{ext_action_1_2}} |
| 3 | 返回主场景步骤{{return_step_1}} / 用例结束 |

---

### {{extension_step_2}}a. {{extension_condition_2}}

**触发条件**：{{extension_trigger_2}}

| 步骤 | 操作 |
|------|------|
| 1 | {{ext_action_2_1}} |
| 2 | {{ext_action_2_2}} |

---

## 特殊需求

### 性能要求
- {{performance_req_1}}

### 安全要求
- {{security_req_1}}

### 其他要求
- {{other_req_1}}

---

## 业务规则

| 规则ID | 规则描述 |
|--------|----------|
| BR-{{rule_id_1}} | {{rule_desc_1}} |
| BR-{{rule_id_2}} | {{rule_desc_2}} |

---

## 数据要求

### 输入数据

| 数据项 | 数据类型 | 必填 | 校验规则 |
|--------|----------|------|----------|
| {{input_1}} | {{input_type_1}} | {{input_required_1}} | {{input_validation_1}} |
| {{input_2}} | {{input_type_2}} | {{input_required_2}} | {{input_validation_2}} |

### 输出数据

| 数据项 | 数据类型 | 说明 |
|--------|----------|------|
| {{output_1}} | {{output_type_1}} | {{output_desc_1}} |

---

## 相关用例

| 关系 | 用例ID | 用例名称 | 说明 |
|------|--------|----------|------|
| 包含 (include) | UC-{{include_id}} | {{include_name}} | {{include_desc}} |
| 扩展 (extend) | UC-{{extend_id}} | {{extend_name}} | {{extend_desc}} |
| 前序用例 | UC-{{pre_id}} | {{pre_name}} | {{pre_desc}} |
| 后续用例 | UC-{{post_id}} | {{post_name}} | {{post_desc}} |

---

## UI要求

### 界面原型

{{ui_prototype_link}}

### 界面要求

- {{ui_req_1}}
- {{ui_req_2}}

### 交互说明

- {{interaction_1}}
- {{interaction_2}}

---

## 测试要点

| 测试场景 | 测试步骤 | 预期结果 |
|----------|----------|----------|
| 正常流程 | {{test_step_1}} | {{expected_1}} |
| 异常流程1 | {{test_step_2}} | {{expected_2}} |
| 边界条件 | {{test_step_3}} | {{expected_3}} |

---

## 备注

{{notes}}

---

## 变更历史

| 版本 | 日期 | 修订人 | 修订内容 |
|------|------|--------|----------|
| 1.0 | {{date}} | {{author}} | 初始版本 |
