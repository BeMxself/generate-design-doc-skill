# generate-design-doc-skill

[中文](README.md) | English

A Claude Code plugin marketplace containing a single plugin: `generate-design-doc`.

It provides the `generate-design-doc` Skill, which scans an existing codebase/module and produces a design document (default `DESIGN.md`) with a progressive analysis workflow and a confirmation step to avoid misclassification.

See also: `plugins/generate-design-doc/README.md`

## Install (Claude Code)

Add this marketplace:

```
/plugin marketplace add BeMxself/generate-design-doc-skill
```

Install the plugin:

```
/plugin install generate-design-doc@generate-design-doc-skill
```

Restart Claude Code after installation.

## Use

The skill is namespaced under the plugin name:

```
/generate-design-doc:generate-design-doc
```

## Install (Codex)

Codex supports skills via `~/.agents/skills/`.

Recommended: use OpenAI's skill installer in Codex:

```text
$skill-installer install the generate-design-doc skill from https://github.com/BeMxself/generate-design-doc-skill/tree/master/plugins/generate-design-doc/skills/generate-design-doc
```

Manual fallback:

```bash
# 1. Clone this repo (anywhere you like)
git clone https://github.com/BeMxself/generate-design-doc-skill.git ~/.codex/third_party/generate-design-doc-skill

# 2. Symlink the skill folder into Codex's global discovery directory
mkdir -p ~/.agents/skills
ln -s ~/.codex/third_party/generate-design-doc-skill/plugins/generate-design-doc/skills/generate-design-doc ~/.agents/skills/generate-design-doc
```

You can optionally use a repo-local `.agents/skills/` layout for team sharing, but it is not required for this repository.

## Install (Kiro CLI)

Kiro CLI can load skills from your project’s `.kiro/skills/` via agent resources (e.g. `skill://.kiro/skills/**/SKILL.md`).

```bash
# 1. Clone this repo (anywhere you like)
git clone https://github.com/BeMxself/generate-design-doc-skill.git ~/.kiro/third_party/generate-design-doc-skill

# 2. Register the skill in the current project
mkdir -p .kiro/skills
ln -s ~/.kiro/third_party/generate-design-doc-skill/plugins/generate-design-doc/skills/generate-design-doc .kiro/skills/generate-design-doc
```

If your agent does not already include `skill://.kiro/skills/**/SKILL.md` in `resources`, create one and add it:

```bash
mkdir -p .kiro/agents
kiro-cli agent create --name generate-design-doc --directory .kiro/agents
kiro-cli agent edit --name generate-design-doc --path .kiro/agents/generate-design-doc.json
```

In `.kiro/agents/generate-design-doc.json`, ensure `resources` includes:

```json
[
  "skill://.kiro/skills/**/SKILL.md"
]
```

Then start chat with the agent:

```
kiro-cli --agent generate-design-doc chat
```

## What it does

- Reads a target codebase/module and summarizes architecture, responsibilities, and key flows
- Proposes a structured `DESIGN.md` (or your chosen filename) for review
- Uses progressive analysis + a confirmation step so you can correct assumptions early

## Use in Codex

Invoke the skill in Codex with:

```text
$generate-design-doc
```

## Repo Layout

- `.claude-plugin/marketplace.json`: marketplace catalog
- `plugins/generate-design-doc/.claude-plugin/plugin.json`: plugin manifest
- `plugins/generate-design-doc/skills/generate-design-doc/`: the skill and guides
- `plugins/generate-design-doc/skills/generate-design-doc/agents/openai.yaml`: Codex metadata
