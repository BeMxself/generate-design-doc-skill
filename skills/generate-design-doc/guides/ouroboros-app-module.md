# Ouroboros 应用模块（App Module）— 分析指导

> 适用于 `app-module/` 下的应用模块（面向最终用户或平台内置应用，如 dynamic-form、ui-schema-management、document-management）。
> 重点是：它提供的“应用能力/页面/模块入口”，如何与 ability/core 协作，以及前后端契约与运行时装配方式。

## 分析模板

### 第一轮：结构扫描
- `pom.xml` → 依赖哪些 ability/core/security？是否有 starter/runtime/web 拆分？
- 目录结构 → Controller/Service/Repository 分层，是否包含前端资源或 UI schema 模板
- `README.md` → 应用定位、入口、菜单/路由信息（如有）
- 被哪个平台装配 → 通常被 `platform/ouroboros-develop` 或 `platform/ouroboros-mothership` 聚合依赖

### 第二轮：核心代码扫描
- 对外入口 → Controller/API、WebSocket、任务调度、消息消费（如有）
- 业务编排 → Service 如何组合多个 ability（文件、通知、组织、数据、工作流等）
- 领域模型 → 本模块定义的实体/DTO，以及与其他模块模型的边界
- 权限与菜单 → 权限点定义、菜单/导航注册方式（如存在）
- 配置与开关 → `*Properties`、profile、功能开关、默认值

### 第三轮：特征探测（app-module 常见）
- UI schema / 页面模型 → amis schema、页面定义、组件库依赖、MicroWeb/微前端（如存在）
- 低代码运行时 → 是否涉及动态模型、UI schema runtime、工作流/审批等
- 批量能力 → 导入/导出、批处理、异步任务与重试
- 集成契约 → OpenAPI/接口约定、统一错误码、分页/筛选协议

## 问题清单

### AI 自答
1. 应用模块的目标用户是谁？提供什么场景能力？（功能清单）
2. 依赖了哪些 ability/core/security？分别承担什么职责？（依赖矩阵）
3. 暴露了哪些 API/入口？端点清单与核心请求/响应模型？
4. 关键业务流程是什么？（端到端：页面/操作 → API → service 编排 → 数据/外部集成）
5. 权限模型是什么？权限点如何声明与校验？菜单/路由如何关联？
6. 配置项有哪些？默认值与必填项？不同环境如何覆盖？
7. 是否有 UI schema/MicroWeb？页面列表是什么？与 API 如何映射？
8. 异常与错误码如何对外呈现？（校验失败 vs 系统错误）
9. 是否有异步处理/批量操作？一致性与重试策略？

### 用户决策点
- 文档侧重：用户功能（产品视角）还是技术集成（开发视角）？
- 是否需要完整 API 清单（全端点）还是仅列核心接口？
- 是否需要把 UI 页面（schema）也纳入文档目录（页面清单 + 字段表）？

## 章节推荐池

### 核心章节
| 章节 | 内容 |
|------|------|
| 概述 | 应用定位、目标用户、核心能力清单 |
| 模块结构与依赖 | 子模块/包结构、依赖矩阵（app-module ↔ ability/core/security） |
| 对外入口（API/页面） | API 清单、页面/路由入口（如有） |
| 关键流程 | 2-3 条端到端核心流程（ASCII 流程/时序） |

### 条件章节
| 章节 | 触发条件 |
|------|---------|
| UI schema/MicroWeb | 发现 amis schema、MicroWeb、前端资源或页面注册 |
| 权限与菜单 | 发现权限点/菜单/导航注册 |
| 配置与开关 | 发现 `*Properties` 或大量配置 |
| 异步与批量 | 发现 MQ/任务/导入导出/重试 |
| 低代码运行时模型 | 发现动态模型/UI schema runtime/工作流 |

### 可选章节
| 章节 | 适用场景 |
|------|---------|
| 使用示例 | 面向集成开发者或二开时 |
| 部署与运维 | 对外依赖多、运行参数复杂时 |
