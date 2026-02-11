[English](README.md) | 中文

# generate-design-doc-skill

这是一个 Claude Code 的插件市场（marketplace）仓库，目前只包含一个插件：`generate-design-doc`。

它提供了一个 Skill：`generate-design-doc`。该 Skill 会扫描你指定的现有代码库/模块，并生成一份设计文档（默认文件名为 `DESIGN.md`）。生成过程采用“渐进式分析 + 关键假设确认”的工作流，避免一开始就把模块边界、职责或技术栈判断错。

另见：`plugins/generate-design-doc/README.md`

## 安装（Claude Code）

添加该 marketplace：

  /plugin marketplace add BeMxself/generate-design-doc-skill

安装插件：

  /plugin install generate-design-doc@generate-design-doc-skill

安装后重启 Claude Code。

## 使用

该 Skill 按插件名进行命名空间隔离：

  /generate-design-doc:generate-design-doc

## 它能做什么

- 阅读目标代码库/模块，梳理整体架构、模块职责与关键流程
- 产出结构化的 `DESIGN.md`（或你指定的文件名），便于评审与迭代
- 通过渐进式分析与确认步骤，让你尽早纠正错误假设，减少“写偏了”的风险

## 仓库结构

- `.claude-plugin/marketplace.json`：marketplace 目录
- `plugins/generate-design-doc/.claude-plugin/plugin.json`：插件清单（manifest）
- `plugins/generate-design-doc/skills/generate-design-doc/`：Skill 本体与配套指南
