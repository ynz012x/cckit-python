# cckit-python

Claude Code Python 开发工具集

## 安装

添加 marketplace 并安装插件：

```
/plugin marketplace add github:ynz012x/cckit-python
/plugin install cckit-python@cckit-python
```

本地开发加载：

```bash
claude --plugin-dir .
```

## Skills

安装后所有 skill 以 `cckit-python:` 为命名空间前缀。

| Skill | 说明 |
|-------|------|
| `cckit-python:python-initializr` | 初始化规范的 Python 项目，集成 uv、black、flake8、ruff、mypy、pytest 等工具链 |
| `cckit-python:git-commit-generator` | 分析暂存区变更，生成 Conventional Commits 规范的中文提交消息 |
| `cckit-python:requirement-analysis` | 基于徐峰《有效需求分析》方法论，采用 SERU 方法和三阶段分析流程 |
| `cckit-python:system-design` | 基于系统设计 7 步法融合 SERU 方法论，支持增量设计和接口签名输出 |

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
│   └── system-design/
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
