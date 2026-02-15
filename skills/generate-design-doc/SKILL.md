---
name: generate-design-doc
description: Use when scanning existing code to generate a design document for a module or component. Triggers on reverse-engineering tasks, module documentation requests, or when onboarding to unfamiliar codebases.
---

# Generate Design Document from Code

## Overview

Scan existing code and generate a design document through progressive analysis and user collaboration. AI drives the analysis; user makes structural decisions.

## Workflow

```
Phase 0: SCOPE
  Confirm module root → (optional) confirm doc audience → choose output path
  ↓
Phase 1: IDENTIFY
  Scan project structure → detect tech stack → predict branch (General vs Ouroboros) → predict module type
  ↓
Phase 2: CONFIRM
  Show prediction + evidence → ask user to confirm/override → collect missing context
  ↓
Phase 3: LOAD GUIDE
  Select matching guide file from guides/ directory
  ↓
Phase 4: ANALYZE (AI autonomous)
  Follow guide's analysis template:
    Round 1: Structure scan (build files, directory layout)
    Round 2: Core code scan (interfaces, models, entry points)
    Round 3: Feature detection (extensions, concurrency, config, etc.)
  AI answers all factual questions from the guide
  ↓
Phase 5: NEGOTIATE (with user)
  Present to user:
    - Module analysis summary (what AI discovered)
    - Recommended chapter outline + one-line summary per chapter
    - Mark chapters as "detected" vs "optional deep-dive"
  User confirms/adjusts chapter structure
  ↓
Phase 6: GENERATE
  Write full design document per confirmed outline
```

### Phase 2: Confirmation (Anti-Misclassification)

After auto-detecting, present the predicted module type and ask the user to confirm or override.

Ask the user to provide missing context in 3-6 lines:
- What problem it solves / key responsibilities (3-5 bullets is fine)
- Expected audience (maintainers vs integrators vs operators) (if not decided in Phase 0)
- Confirmed type (or a correction) (e.g. frontend app / backend service / infra library / ouroboros core/ability/dev/app/platform/security)
- Any "must include" chapters (API list, config, SPI, deployment, UI pages, etc.)

## Module Type Detection

First choose a branch, then pick a guide.

### Precedence Rules

Default to auto-detection, but always let the user override after seeing the prediction.

If user hints conflict with repo signals:
- If confidence is high: show the conflict and ask user to confirm the chosen type.
- If confidence is low: present 2-3 candidate types and let the user choose.

### Branch Selection

| Signal | Branch |
|--------|--------|
| Has `core/` + `ability/` + `libs/` (or `security/` + `platform/`) | Ouroboros |
| Maven multi-module with many `ouroboros-*` artifacts | Ouroboros |
| Otherwise | General |

### General Branch (Non-Ouroboros)

Use these signals to classify the project/module:

| Signal | Inferred Type | Guide File |
|--------|--------------|------------|
| Has `package.json` + pages/routes | Web Frontend App | `web-application.md` |
| Has `package.json` + exports + reusable components | Web Component Library | `web-component-library.md` |
| Has `pom.xml` or `build.gradle` + Controller/Service/Repository | Java Backend Service | `java-business.md` |
| Has `pom.xml` + SPI/AutoConfiguration/Registry patterns | Java Infrastructure/Framework | `java-infrastructure.md` |
| Has `libs/` or `util` style APIs, small surface | Java Utility | `java-utility.md` |
| Has `common` module, widely imported, cross-cutting | Java Common | `java-common.md` |
| Not Java/Web-specific but is a server (routes + config + persistence) | Backend Service (Generic) | `backend-service.md` |

**If uncertain**: present options to user and let them choose.

### Ouroboros Branch (Specialized)

Prefer path-based classification first, then confirm via `pom.xml` and `META-INF/*`.

| Signal | Inferred Type | Guide File |
|--------|--------------|------------|
| Path starts with `core/` | Ouroboros Core Module | `ouroboros-core.md` |
| Path starts with `ability/` | Ouroboros Ability Module | `ouroboros-ability.md` |
| Path starts with `dev-module/` | Ouroboros Dev Module | `ouroboros-dev-module.md` |
| Path starts with `platform/` | Ouroboros Platform App | `ouroboros-platform.md` |
| Path starts with `security/` | Ouroboros Security Module | `ouroboros-security.md` |
| Path starts with `app-module/` | Ouroboros App Module | `ouroboros-app-module.md` |
| Path starts with `libs/common/` | Java Common (Ouroboros) | `java-common.md` |
| Path starts with `libs/util/` | Java Utility (Ouroboros) | `java-utility.md` |

If the folder-based guess conflicts with the module's actual role (e.g., a core-like module living under `security/` for historical reasons), call it out in Phase 2 and let the user choose the most appropriate guide (often `ouroboros-core.md` vs `ouroboros-security.md`).

Ouroboros-specific scan targets (often load-bearing in docs):
- `META-INF/services/` (SPI)
- `META-INF/spring.factories` (auto-config registration)
- `META-INF/ouroboros/**` (field repository, templates, built-in definitions)
- `*AutoConfiguration`, `*Properties`, `@ConfigurationProperties`
- `dev-module/DEV-MODULE-PATTERN*.md` (dev module conventions)

## Guide File Usage

Each guide in `guides/` contains:

1. **Analysis Template** — what to scan and how (AI executes autonomously)
2. **Question Checklist** — factual questions AI self-answers + decision points for user
3. **Chapter Pool** — core/conditional/optional chapters with trigger conditions

Read the selected guide, then follow its analysis template before presenting findings to user.

## Writing Rules

Apply these rules when generating the document:

- **Interface-first**: Show public API signatures, not internal implementation
- **Diagrams**: ASCII art for architecture, step lists or pseudocode for flows
- **Tables**: Use for module lists, config items, API inventories, enumerations
- **Code blocks**: Method signatures + key comments only, no full implementations
- **Exclude**: TODOs, roadmaps, detailed JavaDoc content, internal implementation details

### Branch Notes

General projects:
- Prefer "how to use/build/run" + API/contracts + deployment/config over deep internals.
- For frontends, prioritize routing/pages, state/data, component boundaries, and build/deploy.
- For backends, prioritize API surface, data model, configuration, integrations, and operational concerns.

Ouroboros projects:
- Always document module position in the layer graph (libs/common/util → core → security → ability → app/dev-module → platform).
- Treat SPI and Spring auto-configuration as first-class public contracts (inventory + extension guidance).
- If module touches low-code concepts (dynamic model, UI schema, workflow), add a "Runtime Model" chapter even if optional.

## Negotiation Format

Present analysis results to user like this:

```markdown
## 模块分析摘要

**模块**: [name] | **类型**: [detected type] | **技术栈**: [stack]

### 发现
- 公开接口: 12 个（3 个核心 + 9 个辅助）
- SPI 扩展点: 4 个
- 配置项: 23 个
- Spring 自动配置: 2 个
- 并发组件: 无
- 事务管理: 有（Service 层）

### 推荐章节

| # | 章节 | 来源 | 一句话摘要 |
|---|------|------|-----------|
| 1 | 概述 | 核心 | 模块职责和核心特性 |
| 2 | 模块结构 | 核心 | 3 个子模块的职责划分 |
| 3 | 核心 API | 核心 | 12 个公开接口的签名和用途 |
| 4 | SPI 扩展点 | 探测到 | 4 个扩展接口及内置实现 |
| 5 | 配置项 | 探测到 | 23 个配置属性及默认值 |
| 6 | 事务管理 | 探测到 | Service 层事务边界 |
| 7 | 关键流程 | 可选 | 核心操作的执行路径 |
| 8 | 设计模式 | 可选 | 使用的模式及理由 |

请确认或调整章节结构。
```

## Output

Design document is written to the module's directory or a user-specified location.

Default filename: `DESIGN.md` (in module root) or `docs/design-[module-name].md`.
