# C4模型说明

## 概述

C4模型是一种用于可视化软件架构的方法，由Simon Brown提出。C4代表四个抽象层次：Context、Container、Component、Code。

```mermaid
flowchart TB
    subgraph Level1[Level 1: System Context]
        SC[系统上下文图]
    end

    subgraph Level2[Level 2: Container]
        CT[容器图]
    end

    subgraph Level3[Level 3: Component]
        CP[组件图]
    end

    subgraph Level4[Level 4: Code]
        CD[代码图]
    end

    Level1 --> Level2 --> Level3 --> Level4
```

## Level 1: System Context（系统上下文）

### 目的

展示系统与外部实体（用户、外部系统）的关系，回答"系统与什么交互？"

### 元素

| 元素 | 说明 |
|------|------|
| Person | 使用系统的人员 |
| Software System | 被设计的系统 |
| External System | 外部依赖系统 |

### 示例

```mermaid
C4Context
    title 系统上下文图

    Person(user, "用户", "使用系统的终端用户")
    System(system, "目标系统", "我们正在设计的系统")
    System_Ext(external, "外部系统", "第三方支付系统")

    Rel(user, system, "使用")
    Rel(system, external, "调用支付接口")
```

---

## Level 2: Container（容器）

### 目的

展示系统内部的主要技术组件，回答"系统由什么构成？"

### 元素

| 元素 | 说明 |
|------|------|
| Container | 可独立部署的单元（应用、服务、数据库） |
| Person | 使用者 |
| External System | 外部系统 |

### 示例

```mermaid
C4Container
    title 容器图

    Person(user, "用户", "终端用户")

    System_Boundary(system, "系统边界") {
        Container(web, "Web应用", "React", "用户界面")
        Container(api, "API服务", "Go/Gin", "业务逻辑")
        ContainerDb(db, "数据库", "PostgreSQL", "数据存储")
        ContainerQueue(mq, "消息队列", "Kafka", "异步通信")
    }

    System_Ext(payment, "支付系统", "第三方")

    Rel(user, web, "HTTPS")
    Rel(web, api, "REST/JSON")
    Rel(api, db, "SQL")
    Rel(api, mq, "发布消息")
    Rel(api, payment, "HTTPS")
```

---

## Level 3: Component（组件）

### 目的

展示容器内部的组件结构，回答"容器内部如何组织？"

### 元素

| 元素 | 说明 |
|------|------|
| Component | 容器内的逻辑组件（模块、类组） |

### 示例

```mermaid
C4Component
    title API服务组件图

    Container_Boundary(api, "API服务") {
        Component(handler, "Handler", "处理HTTP请求")
        Component(service, "Service", "业务逻辑")
        Component(repo, "Repository", "数据访问")
    }

    ContainerDb(db, "数据库", "PostgreSQL")

    Rel(handler, service, "调用")
    Rel(service, repo, "调用")
    Rel(repo, db, "SQL")
```

---

## Level 4: Code（代码）

### 目的

展示组件的代码级结构，回答"代码如何组织？"

### 元素

使用UML类图或其他代码级图表。

### 示例

```mermaid
classDiagram
    class OrderService {
        <<interface>>
        +CreateOrder(req) Order
        +GetOrder(id) Order
        +CancelOrder(id)
    }

    class OrderServiceImpl {
        -repo OrderRepository
        +CreateOrder(req) Order
        +GetOrder(id) Order
        +CancelOrder(id)
    }

    class OrderRepository {
        <<interface>>
        +Save(order)
        +FindByID(id) Order
    }

    OrderService <|.. OrderServiceImpl
    OrderServiceImpl --> OrderRepository
```

---

## 在本技能中的应用

| C4层次 | 对应步骤 | 使用场景 |
|--------|----------|----------|
| Context | Step 1 | 展示系统边界和外部交互 |
| Container | Step 3 | 展示组件划分和通信 |
| Component | Step 5 | 展示服务内部结构 |
| Code | Step 5 | 展示类和接口关系 |

---

## Mermaid语法

### C4Context

```
C4Context
    Person(alias, "label", "description")
    System(alias, "label", "description")
    System_Ext(alias, "label", "description")
    Rel(from, to, "label")
```

### C4Container

```
C4Container
    Container(alias, "label", "technology", "description")
    ContainerDb(alias, "label", "technology", "description")
    ContainerQueue(alias, "label", "technology", "description")
    System_Boundary(alias, "label") { ... }
```

### C4Component

```
C4Component
    Component(alias, "label", "description")
    Container_Boundary(alias, "label") { ... }
```

---

## 最佳实践

1. **由粗到细**：从Context开始，逐层深入
2. **保持简洁**：每张图的元素不超过20个
3. **统一风格**：使用一致的命名和描述格式
4. **关注受众**：不同层次面向不同读者
5. **及时更新**：架构变更时同步更新图表
