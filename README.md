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

## Skills

安装后所有 skill 以 `ccpy:` 为命名空间前缀。

| Skill | 说明 |
|-------|------|
| `ccpy:python-initializr` | 初始化规范的 Python 项目，集成 uv、black、flake8、ruff、mypy、pytest 等工具链 |
| `ccpy:git-commit-generator` | 分析暂存区变更，生成 Conventional Commits 规范的中文提交消息 |
| `ccpy:requirement-analysis` | 基于徐峰《有效需求分析》方法论，采用 SERU 方法和三阶段分析流程 |
| `ccpy:system-design` | 基于系统设计 7 步法融合 SERU 方法论，支持增量设计和接口签名输出 |
| `ccpy:skill-design-advisor` | 基于 Google 5大设计模式，分析 Skill 需求并推荐最佳结构。建议在 skill-creator 之前运行 |

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
│   └── skill-design-advisor/
├── LICENSE
└── README.md
```

## 开发

项目使用 pre-commit 进行代码质量检查：

```bash
pre-commit install
pre-commit run --all-files
```

## License

[MIT](LICENSE)
