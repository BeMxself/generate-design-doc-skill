# generate-design-doc (Claude Code plugin)

[中文](../../README.md) | [English](../../README.en.md)

Provides one Skill:

- `generate-design-doc`

Invoke it as:

```
/generate-design-doc:generate-design-doc
```

In Codex, invoke it as:

```text
$generate-design-doc
```

This Skill scans an existing codebase/module and drafts a design document (default `DESIGN.md`) using progressive analysis plus a confirmation step to catch wrong assumptions early.

Codex metadata is provided at `skills/generate-design-doc/agents/openai.yaml`.
