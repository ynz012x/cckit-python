# 变更检测与 Section 映射规则

## 作用域路由

对每个变更文件，确定它归属哪个 CLAUDE.md：

1. 列出项目中所有 CLAUDE.md 的所在目录路径
2. 对变更文件路径，找到**最长前缀匹配**的 CLAUDE.md 目录
3. 将该变更归属到匹配的 CLAUDE.md

示例：变更文件 `backend/api/routes.py`，存在 `./CLAUDE.md` 和 `./backend/CLAUDE.md`，匹配 `./backend/CLAUDE.md`（更长前缀）。

## 结构性变更检测

CLAUDE.md 记录的是项目级知识，只有**结构性变更**（影响构建、架构、规范、依赖等）才需要同步。

### 高相关变更（应检测）

| 变更信号 | 检测方法 | 映射目标 Section |
|----------|----------|-----------------|
| 新增/删除顶层目录 | 新路径的第一级目录不在已知列表中 | 架构 |
| 新增/删除模块或子包 | 出现新的 `__init__.py` 或包目录 | 架构 |
| 新增/删除 skill | `skills/` 下新增或删除含 SKILL.md 的目录 | 架构（Skills 表格） |
| 构建配置变更 | Makefile、CMakeLists.txt、build.gradle 等 | 常用命令 |
| 包管理配置变更 | pyproject.toml、package.json、go.mod、Cargo.toml | 依赖 |
| CI/CD 配置变更 | .github/workflows/、.gitlab-ci.yml、Jenkinsfile | 工作流 |
| 测试配置变更 | pytest.ini、jest.config、vitest.config 等 | 测试 |
| 部署配置变更 | Dockerfile、docker-compose.yml、k8s/ 等 | 部署 |
| 代码规范配置变更 | .eslintrc、ruff.toml、.pre-commit-config.yaml | 开发规范 |
| 插件/工具配置变更 | .claude-plugin/、.vscode/ 等 | 架构 或 开发规范 |
| Rules/约定文件变更 | rules/、.cursor/ 等 | 开发规范 |

### 应忽略的变更

| 变更类型 | 原因 |
|----------|------|
| 业务代码文件（src/*.py、lib/*.ts 等） | 不影响项目级知识 |
| 测试用例代码（tests/*.py、__tests__/*.ts） | 除非测试框架本身变更 |
| .gitignore、.editorconfig | 影响过小 |
| 文档内容更新（docs/ 下已有文件的内容修改） | CLAUDE.md 不是文档索引 |
| 纯代码格式化（style 变更） | 无结构影响 |
| 资源文件（图片、字体等） | 不影响开发知识 |
| lock 文件（package-lock.json、uv.lock） | 自动生成，不需记录 |

### 模糊变更处理

某些变更需要读取 diff 内容才能判断：

- **Makefile 变更**：可能影响"常用命令"也可能影响"依赖"——读取 diff 判断修改的是 target 还是依赖声明
- **pyproject.toml 变更**：可能影响"依赖"也可能影响"构建命令"或"开发规范"——按修改的 section（[project.dependencies] vs [tool.ruff]）分别映射
- **README.md 重大重写**：如果项目定位发生变化，可能需要同步"项目概览"——仅当 diff 涉及项目描述段落时触发

## Section 语义匹配

变更需要映射到 CLAUDE.md 的具体 section。不硬编码 section 名，而是按**内容特征**匹配：

| 目标 Section | 匹配特征 |
|-------------|----------|
| 架构 | 包含目录树（ASCII art）或模块/组件列表表格 |
| 常用命令 | 包含 bash/shell 代码块 |
| 依赖 | 标题含"依赖"/"Dependencies"/"环境"或列出包名+版本 |
| 开发规范 | 标题含"规范"/"Standards"/"Guidelines"/"约定" |
| 测试 | 标题含"测试"/"Testing"/"Test" |
| 部署 | 标题含"部署"/"Deploy"/"发布" |
| 项目概览 | 文件的第一个 `##` section，或包含技术栈描述 |
| 工作流 | 标题含"工作流"/"Workflow"/"CI"/"流程" |

无匹配时：建议在合适位置（通常在文件末尾、最后一个 section 之后）新增 section。
