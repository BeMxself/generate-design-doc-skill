# Ouroboros 开发模块（Dev Module）— 分析指导

> 适用于 `dev-module/` 下的开发期管理模块（管理低代码模型、脚手架、VCS/环境/数据源等），通常提供“管理端 API + UI Schema/MicroWeb + 元数据/Inspector/SPI”组合能力。

## 分析模板

### 第一轮：结构扫描
- `pom.xml` → 子模块划分（manage/runtime/starter/web 等）与依赖（core/ability/common-dev）
- 模式指南 → `dev-module/DEV-MODULE-PATTERN.md` 与 `dev-module/DEV-MODULE-PATTERN-CLAUDE.md`
- 目录结构 → 是否包含前端资源、UI schema 模板、元数据提供者
- `README.md` → 模块定位、使用方式、入口页面/菜单
- 全局搜索 → 被哪个 platform 应用装配？（通常是 `platform/ouroboros-develop`）

### 第二轮：核心代码扫描
- Dev 服务层 → 管理能力接口（CRUD/导入导出/校验/发布）
- Inspector/Provider → 对工作区/模型文件的扫描与解析（例如 VCSFileInspector 类似模式）
- 模型定义 → Dev 模型、DTO、查询条件、分页结构
- AutoConfiguration → Spring Boot 装配点、Bean 注册、条件装配
- Web API → 管理端 Controller、鉴权/权限注解、异常映射

### 第三轮：特征探测（dev-module 特有）
- UI Schema / MicroWeb → amis schema、页面模板、微前端注册与加载方式
- 菜单与导航 → dev 菜单配置/权限点与页面路由的对应关系
- 工作区与版本控制 → workspace 路径、VCS 操作、差异/提交/回滚策略
- 事件与发布流程 → “保存/校验/生成/发布/回滚”等 dev 流程编排
- 与 runtime 的边界 → dev 产生的产物如何被 runtime 消费（配置、模型、schema）

## 问题清单

### AI 自答
1. 该 dev-module 管理的“对象”是什么？（模型/资源/数据源/环境/容器…）核心生命周期是什么？
2. 依赖了哪些 core/ability/common-dev 能力？分别怎么用？
3. 提供了哪些管理端 Web API？（端点清单 + 权限点）核心接口与数据结构？
4. 是否有 Inspector/Provider/SPI？扩展点有哪些？默认实现有哪些？
5. UI Schema/MicroWeb 是否存在？页面有哪些？页面与 API 如何映射？
6. 是否有“校验/生成/发布”流程？产物是什么？产物落点在哪里？
7. 工作区/文件结构假设是什么？（路径约定、命名约定）
8. 并发与一致性：是否有锁/版本号/幂等？是否可能出现并发编辑冲突？
9. 异常体系与错误码：如何向前端呈现校验失败与系统错误？

### 用户决策点
- 文档侧重：开发者扩展（SPI/Inspector）还是产品操作（管理功能与页面）？
- 是否需要列出完整 UI 页面（amis schema）与字段表？
- 是否需要写清“dev → runtime”交付链路（发布/装配/生效/回滚）？

## 章节推荐池

### 核心章节
| 章节 | 内容 |
|------|------|
| 概述 | 模块定位、管理对象、典型使用路径 |
| 模块结构 | 子模块表格、依赖关系（core/ability/platform） |
| 管理能力与 API | 管理端 API 清单、权限点、核心 DTO |
| Dev 运行时与工作区 | workspace 约定、文件结构、VCS/环境依赖 |

### 条件章节
| 章节 | 触发条件 |
|------|---------|
| Inspector/SPI 扩展点 | 发现 `META-INF/services/` 或 Provider/Inspector 接口 |
| UI Schema/MicroWeb | 发现 amis schema、MicroWeb 注册或前端资源 |
| 校验与发布流程 | 发现 validate/generate/publish/rollback 流程 |
| 事件机制 | 发现 dev 事件发布/监听 |
| 安全与权限 | 发现细粒度权限定义或 RBAC 配置 |

### 可选章节
| 章节 | 适用场景 |
|------|---------|
| 关键流程详解 | 发布链路复杂、跨模块协作多时 |
| 数据一致性与并发 | 存在锁、版本、冲突解决策略时 |
| 使用示例 | 面向 dev-module 二次开发者时 |
