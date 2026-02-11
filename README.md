# generate-design-doc-skill

[English](README.md) | [中文](README.zh-CN.md)

A Claude Code plugin marketplace containing a single plugin: `generate-design-doc`.

It provides the `generate-design-doc` Skill, which scans an existing codebase/module and produces a design document (default `DESIGN.md`) with a progressive analysis workflow and a confirmation step to avoid misclassification.

See also: `plugins/generate-design-doc/README.md`

## Install (Claude Code)

Add this marketplace:

  /plugin marketplace add BeMxself/generate-design-doc-skill

Install the plugin:

  /plugin install generate-design-doc@generate-design-doc-skill

Restart Claude Code after installation.

## Use

The skill is namespaced under the plugin name:

  /generate-design-doc:generate-design-doc

## What it does

- Reads a target codebase/module and summarizes architecture, responsibilities, and key flows
- Proposes a structured `DESIGN.md` (or your chosen filename) for review
- Uses progressive analysis + a confirmation step so you can correct assumptions early

## Repo Layout

- `.claude-plugin/marketplace.json`: marketplace catalog
- `plugins/generate-design-doc/.claude-plugin/plugin.json`: plugin manifest
- `plugins/generate-design-doc/skills/generate-design-doc/`: the skill and guides
