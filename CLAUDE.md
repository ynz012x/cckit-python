# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概览

**cckit-python** 是一个 Claude Code 插件，为 Python 开发者提供 AI 驱动的 Skills。插件以 `ccpy` 命名安装到 Claude Code，所有 skill 使用 `ccpy:` 命名空间前缀。这不是一个 Python 包——没有 `pyproject.toml` 或 `setup.py`，核心内容是基于 Markdown 的 Skill 定义。

## 常用命令

### 代码质量
```bash
pre-commit install          # 首次安装 hooks
pre-commit run --all-files  # 运行所有检查与格式化
```

### Skill 同步
```bash
make check-deps   # 检查 pre-commit 和 skillport 是否已安装
make sync-skills  # 从 Anthropic 拉取外部 skill（skill-creator）
```

### 插件本地开发
```bash
claude --plugin-dir .   # 以本地插件模式加载，用于测试
```

## 架构

### 插件入口
`.claude-plugin/plugin.json` 定义插件元数据，`.claude-plugin/marketplace.json` 定义市场列表信息。所有 skill 使用 `ccpy:` 命名空间前缀。

### Skills（`skills/`）
每个子目录是一个独立的 skill，以 `SKILL.md` 作为 agent 提示词。Skill 遵循 Google 5 大设计模式的一种或多种：Tool Wrapper、Generator、Reviewer、Inversion、Pipeline。

| Skill | 模式 | 用途 |
|---|---|---|
| `python-initializr` | Pipeline | 初始化规范的 Python 项目（uv、pre-commit、mypy、pytest、bumpversion） |
| `git-commit-generator` | Generator | 自动生成符合 Conventional Commits v1.0.0 的中文提交信息 |
| `requirement-analysis` | Pipeline | 基于徐峰 SERU 方法论的系统化需求分析 |
| `system-design` | Pipeline | 融合 SERU 的 7 步系统架构设计 |
| `skill-design-advisor` | Reviewer+Generator | 为新建或现有 skill 推荐设计模式 |
| `skill-creator` | Pipeline | 创建、评测和优化 skill（从 Anthropic 同步） |
| `memory-synchronizer` | Pipeline | 自动检测项目变更，增量同步多层级 CLAUDE.md 项目记忆 |

Skill 目录可包含 `references/` 子目录（供 skill agent 执行时读取的参考文档）和 `templates/` 子目录（文件模板）。

`skill-creator` 是外部 skill，通过 `make sync-skills` 使用 `skillport` CLI 同步，不直接编辑。

### 空目录
`agents/`、`commands/`、`hooks/`、`rules/` 为未来扩展预留，当前无内容。

## Skill 开发规范

- Skill 提示词位于各 skill 目录根部的 `SKILL.md`
- Skill 描述（用于触发匹配）必须精确——使用 `ccpy:skill-design-advisor` 设计新 skill，使用 `ccpy:skill-creator` 优化描述
- `skills/skill-creator/scripts/` 中的 Python 脚本支持评测工作流，是工具脚本而非 skill 提示词的一部分
- Pre-commit hooks 对所有 skill 文件强制执行 JSON 格式化（自动修复）、YAML/TOML 校验和空白符一致性

## 依赖

- `pre-commit` — 通过 `uv tool install pre-commit` 安装
- `skillport` — 通过 `uv tool install skillport` 安装（用于 `make sync-skills`）
