# TechNav Resource Aggregator

TechNav is a lightweight, developer-oriented technical resource aggregation and navigation system designed to streamline the discovery, classification, and retrieval of domain-specific online references. It targets technical leads, documentation engineers, and infrastructure architects who need to maintain curated lists of external URLs across multiple environments and projects.

The project solves the problem of scattered bookmarks, inconsistent URL formats, and untracked external dependencies by providing a structured, version-controlled repository of categorized resource links. Each entry is stored with metadata, status annotations, and usage context, enabling teams to share and audit their external reference pool efficiently. TechNav does not host content; it organizes pointers to content, ensuring that link rot and domain drift are actively monitored through integrated health checks.

## 功能概览

- **Categorized Link Storage** – Organize external URLs under user-defined tags, projects, or domains with support for hierarchical nesting and alias resolution.

- **Format Compliance Enforcement** – Automatically validate and store URLs in their original casing, protocol, and path structure without adding or stripping prefixes such as www, http, or trailing slashes.

- **Batch Import and Export** – Support for CSV, JSON, and plain-text bulk operations to synchronize resource lists with external documentation systems or migration scripts.

- **Status Monitoring Pipeline** – Periodic HEAD/GET requests to detect HTTP status changes, certificate expiry, and DNS resolution failures for all tracked resources.

- **Search and Filter DSL** – A domain-specific query language for filtering resources by status code, domain suffix, last-seen timestamp, or custom metadata tags.

- **Audit History Logging** – Every addition, removal, or modification of a resource entry is recorded with timestamp and operator identity for compliance traceability.

- **RESTful API Gateway** – Expose resource lists as JSON endpoints with pagination, sorting, and field selection for integration with CI/CD pipelines or monitoring dashboards.

## 应用场景

- **Documentation Dependency Tracking** – Technical writing teams can embed TechNav resource IDs into their markdown docs, enabling automated link validation during the build process. When a referenced domain changes status, the build system fetches the current state from TechNav API and warns about broken links.

- **Multi-Environment Configuration Management** – DevOps engineers maintain separate resource lists for development, staging, and production environments. TechNav allows environment-specific overrides while keeping the base URL definitions shared across all contexts.

- **Compliance Audit Preparation** – Security and compliance officers export the full resource inventory alongside status history to demonstrate that all external data sources remain accessible and properly versioned over time.

- **Knowledge Base Curation** – Community managers and open-source maintainers use TechNav to publish curated lists of official documentation, community forums, and reference implementations, ensuring that new contributors start from verified sources rather than outdated search results.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/technav/technav-core.git
cd technav-core

# Install dependencies using pip (Python 3.9+ required)
pip install -r requirements.txt

# Initialize the local SQLite database and load seed categories
python manage.py init-db --seed data/default_categories.json

# Start the development server on port 8080
python manage.py runserver --port 8080 --host 0.0.0.0

# After startup, access the web interface at http://localhost:8080/dashboard
# To import a list of URLs from a plain text file:
python manage.py import --file resources.txt --category external --strict-mode
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9, 3.10, 3.11 | 核心运行时，类型注解依赖 PEP 585 特性 |
| SQLite | 3.35.0 或更高 | 内置数据库，支持 JSON 扩展和窗口函数 |
| requests | 2.28.0 或更高 | 用于状态监控中的 HTTP 探测 |
| click | 8.1.0 或更高 | 命令行接口解析框架 |
| pyyaml | 6.0 或更高 | 用于配置文件和自定义元数据序列化 |
| pytest | 7.2.0 或更高 | 仅开发环境需要，用于运行单元测试套件 |
| gunicorn | 20.1.0 或更高 | 生产环境推荐 WSGI 服务器，非开发强制 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | 如何安装、配置首个资源列表、启动监控任务 |
| API 参考 | docs/api/endpoints.md | 每个 REST 端点的请求/响应格式、认证方式、错误码 |
| 监控配置 | docs/monitoring/check-intervals.md | 如何调整探测频率、超时阈值、重试策略 |
| 数据模型 | docs/schema/entity-relationship.md | 资源表、标签表、审计日志表的字段定义与关联关系 |
| 部署手册 | docs/deployment/production-checklist.md | 反向代理配置、数据库迁移、日志轮转、备份策略 |
| 贡献规范 | docs/contributing/coding-standards.md | 代码风格、提交信息格式、PR 评审流程 |

## 资源列表

### 官方参考站点

<code>dszuqiujishibifen.org.cn</code>

<code>dszuqiujishibifen.cn</code>

<code>dszuqiujishibifengw.com.cn</code>

### 扩展数据索引

<code>dszuqiujifenbang.cn</code>

<code>dszuqiujifenbang.org.cn</code>

<code>dszuqiujifenbang.net.cn</code>

<code>dszuqiujifenbang.com.cn</code>

## 项目结构

```
technav-core/
├── docs/                           # 完整文档套件，包含 API 示例和部署指南
│   ├── api/                        # OpenAPI 规范与交互式文档生成源
│   ├── deployment/                 # Docker、Kubernetes、systemd 示例文件
│   └── contributing/               # 开发者指引、PR 模板、issue 分类说明
├── src/
│   ├── technav/                    # 主包目录
│   │   ├── core/                   # 核心数据模型、验证器、URL 规范化工具
│   │   ├── monitor/                # 健康检查调度器、结果存储、告警触发器
│   │   ├── web/                    # Flask/WSGI 应用、路由、模板上下文处理器
│   │   ├── cli/                    # Click 命令组，包含 import/export/check 子命令
│   │   └── utils/                  # 日志封装、配置加载器、异常处理基类
│   ├── tests/                      # 单元测试与集成测试，按模块分层组织
│   │   ├── unit/                   # 针对每个核心函数的隔离测试
│   │   └── integration/            # 数据库与 API 端点的端到端测试
│   ├── scripts/                    # 运维辅助脚本，如数据库备份、迁移生成
│   └── data/                       # 预置种子数据，包括默认类别和示例资源
├── config/                         # 环境配置文件，支持 development/staging/production 三套
├── requirements.txt                # 生产依赖列表，带哈希锁定
├── requirements-dev.txt            # 开发额外依赖，包括 linter、formatter、profiler
├── pyproject.toml                  # 项目元数据、构建系统配置、工具设置
├── Makefile                        # 常用任务快捷方式，如 make test, make lint, make run
└── LICENSE                         # MIT 许可全文，包含免责声明与版权归属
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从主分支 checkout 一个新的分支，命名遵循 `feature/` 或 `fix/` 前缀加简短描述，例如 `feature/url-normalizer-optimization`。

2.  **编写测试并确保通过** – 所有新增或修改的代码必须附带对应的单元测试。运行 `make test` 确保覆盖率不低于 85%，且原有测试未出现回归失败。

3.  **更新文档和变更日志** – 如果修改影响了用户可见的行为或配置格式，需同步更新 `docs/` 下相关章节，并在 `CHANGELOG.md` 的 Unreleased 段落中添加条目，说明变动类型（新增、修复、废弃）。

4.  **提交前进行本地检查** – 执行 `make lint` 和 `make format` 统一代码风格（基于 Black 和 isort），并运行 `make typecheck`（基于 mypy）静态检查类型注解的一致性。

5.  **发起 Pull Request 并等待评审** – 描述中应清晰说明解决的问题、实现方案以及手动测试的步骤。至少需要一名核心维护者批准后方可合并，合并前需解决所有对话评论。

## 常见问题

**Q: 当监控任务检测到某个 URL 返回 404 或超时时，系统会自动移除该资源吗？**

A: 不会。系统默认只记录状态变化并触发告警（输出到日志和可选的 webhook），不会自动删除或修改任何资源条目。用户可以通过管理界面或 CLI 命令 `check --report --filter status=failed` 查看异常列表，然后手动决定是否更新或移除。如果需要自动隔离，可以在配置中启用 `auto_disable_after_failures` 参数，但该功能默认关闭以避免误操作。

**Q: 如何迁移现有的收藏夹或书签文件到 TechNav？**

A: TechNav 提供了 `import` 命令，支持从 Firefox/Chrome 导出的 HTML 书签文件、纯文本逐行列表以及 CSV 格式文件导入。对于标准格式，直接使用 `python manage.py import --browser-html bookmarks.html` 即可自动解析。如果格式特殊，可以先用 `--dry-run` 预览解析结果，再通过 `--map-fields` 自定义列映射关系。

**Q: 项目支持高可用部署吗？多个实例之间如何同步状态？**

A: 是的。生产部署推荐使用外部 PostgreSQL 替代默认的 SQLite，并配置 Redis 作为缓存和会话存储。多个实例共享同一个数据库后端时，监控调度器通过数据库行锁（SELECT ... FOR UPDATE SKIP LOCKED）来避免重复探测同一资源。内部队列使用 Redis 的发布/订阅机制协调任务分发，确保每个资源在同一时刻只被一个工作节点处理。

## 许可证

MIT License

Copyright (c) 2026 TechNav Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:20
