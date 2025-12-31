# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 语言规范
- 所有对话和文档都使用中文 Chinese
- 文档使用 markdown 格式
- 代码与注释使用英文 English

## Repository Overview

Personal Skills marketplace - 一个模块化的指令包集合，用于扩展 Claude 的专业任务能力。包含 Skills（技能）、Commands（命令）和 Rules（规则）三种类型的扩展。

## Architecture

```
skills/                    # Skill 实现
├── <skill-name>/
│   ├── SKILL.md          # Required: YAML frontmatter + instructions
│   ├── scripts/          # Optional: Python/Bash 脚本
│   ├── references/       # Optional: 按需加载的文档
│   └── assets/           # Optional: 模板、图片、字体等资源
commands/                  # Slash commands（/xxx 触发）
├── <command-name>.md     # Command prompt 文件
rules/                     # 规则文件（自动/按需应用）
├── <rule-name>.mdc       # 带 frontmatter 的规则文件
spec/                      # Agent Skills 规范
template/                  # Skill 模板
.claude-plugin/marketplace.json  # Plugin marketplace 配置
```

### Current Skills

- `changelog-generator` - 从 git commits 自动生成用户友好的 changelog
- `code-reviewer` - 代码审查工具包，支持 PR 分析、代码质量检查和审查报告生成
- `error-resolver` - 基于第一性原理的错误诊断和解决方案，支持 replay 功能
- `frontend-design` - 创建高质量、生产级的前端界面
- `skill-creator` - 创建有效 skills 的指南

### Commands

| Command | Description |
|---------|-------------|
| `/add-command` | 添加新的自定义命令到工作区 |
| `/create-doc` | 基于 Diátaxis 框架创建文档（含子命令） |
| `/git-commit` | Git commit 工作流（conventional-changelog 风格） |
| `/git-push` | 完整 Git 工作流（含分支管理和 PR 创建） |
| `/issue` | 使用 GitHub CLI 创建标准化 issue |
| `/no-spec` | 跳过 spec 流程，直接执行简单任务 |
| `/spec` | 强制使用完整 spec 工作流 |
| `/translate` | 将文档翻译为中文 |

**`/create-doc` 子命令**（内部使用，不直接调用）：
- `doc/tutorial` - 创建教程型文档
- `doc/howto` - 创建任务导向的 How-to 指南
- `doc/reference` - 创建技术参考文档
- `doc/explanation` - 创建概念解释型文档

### Rules

| Rule | Description | Auto Apply |
|------|-------------|------------|
| `documentation-rules` | Diátaxis 框架文档标准 | No |
| `font-awesome-icons` | FontAwesome 图标参考 | No |
| `interactive-dev` | 交互式开发工作流（研究→构思→计划→执行→评审） | No |
| `project-rules` | 项目编码规范和 Git 提交标准 | Yes |

## Skill Structure

Every skill requires a `SKILL.md` with:

```markdown
---
name: skill-name
description: What the skill does AND when to trigger it
---

# Instructions here
```

Key design principles:
- **Progressive disclosure**: Metadata always loaded (~100 words) → SKILL.md body when triggered (<5k words) → bundled resources on-demand
- **Concise instructions**: Only add context Claude doesn't already have
- **Appropriate freedom**: High (text) for flexible tasks, Low (scripts) for fragile operations

## Claude Code Plugin Installation

```bash
# Add this marketplace
/plugin marketplace add RealAlexandreAI/personal-skills-marketplace

# Install all skills
/plugin install personal-skills@alexandre-owned
```

## Key Files

### Skills
- `skills/changelog-generator/SKILL.md` - Changelog 生成指南
- `skills/code-reviewer/SKILL.md` - 代码审查工具和流程
- `skills/error-resolver/SKILL.md` - 错误诊断框架和 replay 系统
- `skills/frontend-design/SKILL.md` - 前端设计规范
- `skills/skill-creator/SKILL.md` - Skills 创建指南

### Commands
- `commands/*.md` - 8 个 slash commands
- `commands/doc/*.md` - 4 个子命令（由 `/create-doc` 内部调用）

### Rules
- `rules/project-rules.mdc` - 项目规范（自动应用）
- `rules/documentation-rules.mdc` - 文档标准
- `rules/interactive-dev.mdc` - 交互式开发流程
- `rules/font-awesome-icons.mdc` - 图标参考
