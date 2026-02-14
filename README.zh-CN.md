[English](README.md) | 中文

# generate-design-doc-skill

这是一个 Claude Code 的插件市场（marketplace）仓库，目前只包含一个插件：`generate-design-doc`。

它提供了一个 Skill：`generate-design-doc`。该 Skill 会扫描你指定的现有代码库/模块，并生成一份设计文档（默认文件名为 `DESIGN.md`）。生成过程采用“渐进式分析 + 关键假设确认”的工作流，避免一开始就把模块边界、职责或技术栈判断错。

另见：`plugins/generate-design-doc/README.md`

## 安装（Claude Code）

添加该 marketplace：

```
/plugin marketplace add BeMxself/generate-design-doc-skill
```

安装插件：

```
/plugin install generate-design-doc@generate-design-doc-skill
```

安装后重启 Claude Code。

## 使用

该 Skill 按插件名进行命名空间隔离：

```
/generate-design-doc:generate-design-doc
```

## 安装（Codex）

Codex 支持通过 `~/.agents/skills/` 和仓库内 `.agents/skills/` 发现 skills。

推荐方式：在 Codex 中使用 OpenAI 官方 skill installer：

```text
$skill-installer install the generate-design-doc skill from https://github.com/BeMxself/generate-design-doc-skill/tree/master/plugins/generate-design-doc/skills/generate-design-doc
```

手动兜底方式：

```bash
# 1. 克隆本仓库（位置随意）
git clone https://github.com/BeMxself/generate-design-doc-skill.git ~/.codex/third_party/generate-design-doc-skill

# 2. 将 skill 目录软链接到 Codex 全局发现目录
mkdir -p ~/.agents/skills
ln -s ~/.codex/third_party/generate-design-doc-skill/plugins/generate-design-doc/skills/generate-design-doc ~/.agents/skills/generate-design-doc
```

如果你在本仓库内运行 Codex，也可以直接使用仓库内已提交的 `.agents/skills/generate-design-doc`。

## 安装（Kiro CLI）

Kiro CLI 可以通过 agent 的 `resources` 加载项目内 `.kiro/skills/` 下的 skills（例如 `skill://.kiro/skills/**/SKILL.md`）。

```bash
# 1. 克隆本仓库（位置随意）
git clone https://github.com/BeMxself/generate-design-doc-skill.git ~/.kiro/third_party/generate-design-doc-skill

# 2. 在当前项目中注册该 skill
mkdir -p .kiro/skills
ln -s ~/.kiro/third_party/generate-design-doc-skill/plugins/generate-design-doc/skills/generate-design-doc .kiro/skills/generate-design-doc
```

如果你使用的 agent 尚未在 `resources` 中包含 `skill://.kiro/skills/**/SKILL.md`，可以创建一个 agent 并补上该资源：

```bash
mkdir -p .kiro/agents
kiro-cli agent create --name generate-design-doc --directory .kiro/agents
kiro-cli agent edit --name generate-design-doc --path .kiro/agents/generate-design-doc.json
```

在 `.kiro/agents/generate-design-doc.json` 中，确认 `resources` 包含：

```json
[
  "skill://.kiro/skills/**/SKILL.md"
]
```

然后用该 agent 启动聊天：

```
kiro-cli --agent generate-design-doc chat
```

## 它能做什么

- 阅读目标代码库/模块，梳理整体架构、模块职责与关键流程
- 产出结构化的 `DESIGN.md`（或你指定的文件名），便于评审与迭代
- 通过渐进式分析与确认步骤，让你尽早纠正错误假设，减少“写偏了”的风险

## 在 Codex 中使用

在 Codex 中可这样调用：

```text
$generate-design-doc
```

## 仓库结构

- `.claude-plugin/marketplace.json`：marketplace 目录
- `plugins/generate-design-doc/.claude-plugin/plugin.json`：插件清单（manifest）
- `plugins/generate-design-doc/skills/generate-design-doc/`：Skill 本体与配套指南
- `plugins/generate-design-doc/skills/generate-design-doc/agents/openai.yaml`：Codex 元数据
- `.agents/skills/generate-design-doc`：仓库内 Codex skill 入口（软链接）
