# cckit-python

Claude Code Python 开发工具集

## 安装

添加 marketplace 并安装插件：

```
/plugin marketplace add https://github.com/ynz012x/cckit-python.git
/plugin install ccpy@cckit-python
/reload-plugins
```

本地开发加载：

```bash
claude --plugin-dir .
```

> **参考文档**：[Claude Code Plugins Reference](https://code.claude.com/docs/en/plugins-reference)

## Rules 同步

由于 Claude Code Plugin 不支持 Rules 分发，需要手动从 Git 仓库拉取到项目的 `.claude` 目录：

```bash
# 在项目根目录下执行
mkdir -p ./.claude
git clone --depth=1 --filter=blob:none --sparse https://github.com/ynz012x/cckit-python.git /tmp/cckit-python
cd /tmp/cckit-python
git sparse-checkout set rules
cp -r rules/* ../.claude/
rm -rf /tmp/cckit-python
```

## Skills

安装后所有 skill 以 `ccpy:` 为命名空间前缀。

| Skill | 说明 |
|-------|------|
| `ccpy:python-initializr` | 初始化规范的 Python 项目，集成 uv、black、flake8、ruff、mypy、pytest 等工具链 |
| `ccpy:git-commit-generator` | 分析暂存区变更，生成 Conventional Commits 规范的中文提交消息 |
| `ccpy:requirement-analysis` | 基于徐峰《有效需求分析》方法论，采用 SERU 方法和三阶段分析流程 |
| `ccpy:system-design` | 基于系统设计 7 步法融合 SERU 方法论，支持增量设计和接口签名输出 |
| `ccpy:skill-design-advisor` | 基于 Google 5大设计模式，分析 Skill 需求并推荐最佳结构。建议在 skill-creator 之前运行 |
| `ccpy:skill-creator` | 创建新 skill、修改和优化现有 skill，支持性能测试和描述优化 |

## 目录结构

```
cckit-python/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   ├── python-initializr/
│   ├── git-commit-generator/
│   ├── requirement-analysis/
│   ├── system-design/
│   ├── skill-design-advisor/
│   └── skill-creator/
├── LICENSE
└── README.md
```

## 开发

项目使用 pre-commit 进行代码质量检查：

```bash
pre-commit install
pre-commit run --all-files
```

### Makefile 命令

```bash
make help          # 显示所有可用命令
make check-deps    # 检查依赖工具是否已安装
make sync-skills   # 同步外部 skills（覆盖更新 + validate）
```

## License

[MIT](LICENSE)
