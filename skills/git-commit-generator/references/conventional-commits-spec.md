# Conventional Commits v1.0.0 规范参考

## 提交消息格式

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Type 定义

| Type | 说明 | SemVer 对应 |
|------|------|------------|
| `feat` | 新功能 | MINOR |
| `fix` | 修复缺陷 | PATCH |
| `docs` | 仅文档变更 | - |
| `style` | 代码格式（不影响逻辑） | - |
| `refactor` | 重构（既非新功能也非修复） | - |
| `perf` | 性能优化 | - |
| `test` | 添加或修改测试 | - |
| `build` | 构建系统或外部依赖变更 | - |
| `ci` | CI 配置或脚本变更 | - |
| `chore` | 其他不影响源码或测试的变更 | - |

## Breaking Change

表示方式（二选一或同时使用）：

1. type/scope 后添加 `!`：`feat!: 移除废弃的 API`
2. footer 中声明：`BREAKING CHANGE: 移除了 v1 版本的接口`

Breaking Change 对应 SemVer MAJOR 版本升级。

## 规范要点

1. 提交消息必须以 type 为前缀
2. scope 可选，用括号包裹：`feat(auth): ...`
3. description 紧跟冒号和空格之后
4. body 与 description 之间用空行分隔
5. footer 与 body 之间用空行分隔
6. footer 格式为 `token: value` 或 `token #value`
7. BREAKING CHANGE 作为 footer 时必须大写

## 中文 Commit Message 示例

### 简单提交（仅标题）

```
feat(auth): 添加用户登录功能
```

```
fix(api): 修复空指针异常导致的接口崩溃
```

```
docs: 更新 README 安装说明
```

```
refactor(database): 重构数据库连接池逻辑
```

```
test(user): 补充用户模块单元测试
```

```
chore(deps): 升级 Spring Boot 至 3.2.0
```

```
style: 统一代码缩进格式
```

```
perf(query): 优化首页查询性能
```

```
build: 调整 Docker 镜像构建配置
```

```
ci: 添加 GitHub Actions 自动化测试流水线
```

### 包含 body 的提交

```
feat(order): 添加订单导出功能

- 支持导出为 CSV 和 Excel 格式
- 添加日期范围筛选
- 限制单次导出最大 10000 条记录
```

### Breaking Change 提交

```
feat(api)!: 重新设计用户认证接口

- 使用 JWT 替代 Session 认证
- 移除 /api/v1/login 接口
- 新增 /api/v2/auth/token 接口

BREAKING CHANGE: 移除了 v1 版本的认证接口，客户端需要迁移到 v2 接口
```

### 关联 Issue 的提交

```
fix(payment): 修复支付回调重复处理的问题

添加幂等性校验，防止同一笔支付回调被重复处理

Closes #128
```
