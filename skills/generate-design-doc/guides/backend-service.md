# 后端服务项目（通用）— 分析指导

> 适用于非特定语言/框架的后端服务项目（REST/GraphQL/RPC），当无法明确归类到 Java/Spring 或前端项目时使用。

## 分析模板

### 第一轮：结构扫描
- 构建与依赖文件 → `pom.xml` / `build.gradle` / `package.json` / `go.mod` / `pyproject.toml` / `requirements.txt` / `Cargo.toml` / `*.csproj`
- 入口与启动方式 → `main` / `server` / `app` / `cmd/*` / `bin/*` / `Dockerfile` / `compose` / 启动脚本
- 配置位置 → `.env*` / `config/*` / `application*` / `settings*` / `values.yaml`
- 目录结构 → `controllers|handlers|routes` / `service` / `domain|model` / `repository|dao` / `infra`
- 文档与约定 → `README.md` / `docs/*` / `CONTRIBUTING.md`

### 第二轮：核心代码扫描
- 路由/协议层 → 路由表、handler/controller、middleware/interceptor
- 业务层 → service/usecase，编排逻辑与事务边界（如存在）
- 数据层 → ORM/SQL client、Repository、迁移脚本、索引/约束
- 接口契约 → OpenAPI/Swagger、GraphQL schema、protobuf/IDL
- 错误处理 → 统一 error type、错误码、异常映射

### 第三轮：特征探测
- 认证授权 → JWT/OAuth/session、RBAC/ABAC
- 异步与任务 → MQ/stream、后台任务/cron、Outbox/重试
- 缓存 → Redis/本地缓存、缓存一致性策略
- 可观测性 → 日志结构、metrics/tracing、health checks
- 部署与运行 → Docker/K8s、配置注入、滚动升级策略
- 外部集成 → 第三方 API/SDK、回调/webhook

## 问题清单

### AI 自答
1. 服务提供的核心能力是什么？（一句话 + 3-5 个要点）
2. 入口在哪里？进程生命周期如何管理？（启动、优雅关闭）
3. 暴露了哪些接口？（端点/方法/协议）有哪些公共约束？（鉴权、幂等、限流）
4. 核心领域模型是什么？主要实体/聚合及关系？
5. 数据存储类型与访问方式？（表/集合/索引/迁移策略）
6. 配置项有哪些？默认值与必填项？如何在不同环境覆盖？
7. 错误模型是什么？错误码/异常如何映射到对外响应？
8. 外部依赖有哪些？（第三方服务、内部服务）如何隔离与重试？
9. 异步/任务/消息系统是否存在？一致性与重试策略？
10. 可观测性如何做？（日志/指标/追踪/健康检查）

### 用户决策点
- 文档受众：服务使用者（对接方）还是服务维护者（开发/运维）？
- 是否需要完整接口清单（OpenAPI/IDL 级别）还是只列核心接口？
- 是否需要包含部署手册（Docker/K8s）与运行参数详解？

## 章节推荐池

### 核心章节
| 章节 | 内容 |
|------|------|
| 概述 | 服务定位、核心能力、边界 |
| 架构与模块结构 | 分层/目录结构、关键模块职责 |
| API/协议 | 端点/方法/消息格式、鉴权、错误模型 |
| 数据模型 | 核心实体、存储结构与约束 |
| 配置与运行 | 配置项清单、启动方式、环境区分 |

### 条件章节
| 章节 | 触发条件 |
|------|---------|
| 认证与授权 | 发现认证/权限逻辑 |
| 异步与一致性 | 发现 MQ/任务/重试/Outbox |
| 缓存策略 | 发现缓存组件或缓存注解 |
| 外部集成 | 发现第三方/内部依赖调用 |
| 可观测性 | 发现 metrics/tracing/health |
| 部署与运维 | 发现 Docker/K8s/helm 或明确部署脚本 |

### 可选章节
| 章节 | 适用场景 |
|------|---------|
| 关键流程详解 | 端到端流程复杂时 |
| 安全与合规 | 有审计/脱敏/安全策略时 |
| 性能考量 | 有高并发/大流量路径时 |
