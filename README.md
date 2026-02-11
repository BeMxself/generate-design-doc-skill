# generate-design-doc-skill

A Claude Code plugin marketplace containing a single plugin: `generate-design-doc`.

It provides the `generate-design-doc` Skill, which scans an existing codebase/module and produces a design document (default `DESIGN.md`) using progressive analysis and a confirmation step to avoid misclassification.

## Install (Claude Code)

Add this marketplace:

  /plugin marketplace add BeMxself/generate-design-doc-skill

Install the plugin:

  /plugin install generate-design-doc@generate-design-doc-skill

Restart Claude Code after installation.

## Use

The skill is namespaced under the plugin name:

  /generate-design-doc:generate-design-doc

## Repo Layout

- `.claude-plugin/marketplace.json`: marketplace catalog
- `plugins/generate-design-doc/.claude-plugin/plugin.json`: plugin manifest
- `plugins/generate-design-doc/skills/generate-design-doc/`: the skill and guides
