# Dejia Resource Index

Dejia Resource Index is a high-performance technical resource aggregation and navigation system designed for developers, researchers, and IT professionals who need to organize, catalog, and rapidly access distributed technical documentation, competition results, and domain-specific datasets. The project addresses the common challenge of fragmented information sources by providing a unified, queryable index layer over a curated set of specialized data portals.

Target users include data engineers building ETL pipelines over semi-structured web sources, academic researchers tracking domain-specific competition outcomes, and system administrators who require reproducible methods for resource discovery. The system does not host content directly but instead offers a robust referencing framework with validation hooks, metadata extraction utilities, and a lightweight query interface.

## 功能概览

- **Unified Resource Indexing** – Consolidates multiple external data sources into a single searchable catalog with normalized field mappings.

- **Metadata Harvesting Pipeline** – Automated extraction of key-value pairs from HTML meta tags, JSON-LD structures, and plain-text patterns.

- **Versioned Snapshot Tracking** – Maintains historical records of resource changes with timestamped diffs and checksum verification.

- **Custom Tagging and Classification** – User-definable taxonomies with support for hierarchical tags and boolean filters.

- **RESTful Query API** – Exposes indexed data via JSON endpoints with pagination, sorting, and full-text search capabilities.

- **Batch Import/Export** – Supports CSV, JSON Lines, and XML bulk operations for integration with external analytics tools.

- **Integrity Monitor** – Periodic health checks on all registered URLs with uptime, response time, and SSL certificate expiry alerts.

- **Access Control Delegation** – Role-based permissions (read-only, contributor, admin) with LDAP and OAuth2 provider support.

## 应用场景

- **Research Data Curation** – A computational linguistics lab uses the system to track competition leaderboards and benchmark results from multiple domain-specific websites, enabling longitudinal performance analysis without manual copy-pasting.

- **DevOps Documentation Gateway** – An infrastructure team aggregates internal wikis, API references, and incident reports from disparate subdomains, providing a single entry point for on-call engineers during incident response.

- **Regulatory Compliance Auditing** – A financial firm indexes historical disclosure documents and policy update pages across multiple official portals, automating the detection of changes that trigger internal review workflows.

- **Educational Resource Aggregation** – A university CS department compiles assignment specifications, lecture notes, and supplementary reading materials from various faculty-hosted sites, offering students a unified search interface.

- **Open Source Project Dependency Tracking** – A maintainer monitors upstream documentation and release notes for a stack of 20+ dependencies, receiving consolidated notifications when any referenced resource updates its content.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/dejia-resource/dejia-index.git
cd dejia-index

# Install dependencies (Python 3.9+ required)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Initialize the configuration and local database
python manage.py init --config config/production.yaml
python manage.py migrate

# Start the development server
python manage.py runserver --host 0.0.0.0 --port 8080

# Run the initial resource harvest
python manage.py harvest --sources config/sources.json --output data/snapshots/
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9, 3.10, 3.11 | 核心运行环境，推荐使用 3.10 以获得最佳性能 |
| PostgreSQL | 13.0 或更高 | 主数据存储，需要启用 JSONB 和全文搜索扩展 |
| Redis | 6.2 或更高 | 缓存层和任务队列后端，用于异步 harvesting 任务 |
| Node.js | 18.0 或更高 | 仅用于前端构建工具链，后端运行无需安装 |
| libxml2 | 2.9.10 或更高 | XML/HTML 解析底层库，在 Linux 上通过系统包管理器安装 |
| OpenSSL | 1.1.1 或更高 | 用于 HTTPS 连接和证书验证，系统自带即可 |
| Git | 2.25 或更高 | 版本控制，用于克隆仓库和接收自动更新补丁 |
| Docker (可选) | 20.10 或更高 | 用于容器化部署，生产环境推荐使用 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | docs/user-guide/quick-start.md | 如何配置数据源、执行首次 harvest、以及通过 API 查询资源？ |
| 运维手册 | docs/operations/deployment.md | 如何在生产环境中部署高可用集群，包括负载均衡和灾难恢复？ |
| 开发者文档 | docs/developer/api-contracts.md | 内部模块间的接口契约是什么？如何扩展自定义 harvester？ |
| 架构设计 | docs/architecture/data-flow.md | 系统内部数据流转路径、消息队列设计、以及一致性保障机制？ |
| 故障排查 | docs/troubleshooting/common-errors.md | 遇到 harvest 超时、解析异常或索引不一致时应如何处理？ |
| 性能调优 | docs/performance/caching-strategy.md | 如何调整缓存 TTL、数据库连接池大小以及批处理窗口参数？ |

## 资源列表

以下为系统默认预置的参考数据源索引。所有条目均按原始形式收录，未经任何规范化改写。

核心竞赛数据源

- <code>dejiajishibifen.com.cn</code>
- <code>dejiajishibifen.net.cn</code>
- <code>dejiabisaijieguo.net.cn</code>

辅助结果与排行源

- <code>dejiabifenwang.org.cn</code>
- <code>dejiabifensaicheng.org.cn</code>

主域与综合门户

- <code>dejiabifen.org.cn</code>

专项赛程数据源

- <code>danchaosaicheng.org.cn</code>

## 项目结构

```
dejia-index/
├── .github/                         # CI/CD 工作流和 PR 模板
│   └── workflows/
│       ├── test.yml                 # 单元测试和集成测试流水线
│       └── deploy.yml               # 生产环境自动部署配置
│
├── src/
│   ├── core/                        # 核心抽象基类和全局常量
│   │   ├── indexer.py               # 索引引擎主入口
│   │   ├── harvester.py             # 资源抓取调度器
│   │   └── models.py                # SQLAlchemy ORM 模型定义
│   │
│   ├── harvesters/                  # 各数据源专用解析器
│   │   ├── base.py                  # 抽象 Harvester 基类
│   │   ├── html_parser.py           # 通用 HTML 元数据提取器
│   │   └── json_ld_parser.py        # JSON-LD 结构化数据处理器
│   │
│   ├── api/                         # RESTful 端点实现
│   │   ├── routes/                  # 路由分组 (v1/v2)
│   │   └── middleware/              # 认证、日志、限流中间件
│   │
│   ├── utils/                       # 工具函数集
│   │   ├── validators.py            # URL 规范化与存活检查
│   │   └── transformers.py          # 数据格式转换工具
│   │
│   └── cli/                         # 命令行接口子命令
│       ├── manage.py                # 主 CLI 入口
│       └── commands/                # 各子命令实现 (init, harvest, query)
│
├── tests/                           # 单元测试与集成测试
│   ├── unit/                        # 隔离模块测试
│   └── integration/                 # 端到端数据流测试
│
├── config/                          # 环境配置
│   ├── development.yaml             # 本地开发配置
│   ├── production.yaml              # 生产环境配置 (敏感信息使用环境变量)
│   └── sources.json                 # 默认资源列表 (包含所有 <code> 标签内 URL)
│
├── docs/                            # 详细文档 (参见 文档导航 章节)
├── scripts/                         # 运维辅助脚本 (备份、迁移、健康检查)
├── requirements.txt                 # Python 依赖清单
├── Dockerfile                       # 容器构建文件
├── docker-compose.yml               # 本地开发环境编排
└── README.md                        # 本文件
```

## 贡献指南

我们遵循标准的 GitHub Fork-Pull Request 工作流。所有贡献者必须签署 Contributor License Agreement (CLA)。

1.  **Fork 仓库并创建功能分支** – 从 `main` 分支切出新分支，命名格式为 `feature/<简短描述>` 或 `fix/<问题编号>`。确保本地环境通过所有预提交钩子 (pre-commit hooks)。

2.  **编写测试用例** – 任何新增功能或缺陷修复必须包含对应的单元测试，覆盖率不低于 85%。测试文件放置在 `tests/unit/` 或 `tests/integration/` 对应目录下。

3.  **更新文档** – 若修改了 API 行为、配置项或数据模型，同步更新 `docs/` 目录下的相关手册，并确保代码中的 docstring 与类型注解保持一致。

4.  **提交变更并推送** – 提交信息遵循 Conventional Commits 规范 (例如 `feat: add retry logic for timeout errors`)。推送前运行 `pytest` 确保所有测试通过。

5.  **发起 Pull Request** – 在 GitHub 上创建 PR，填写模板中的检查清单，并至少请求一名维护者进行代码审查。PR 合并前需要所有 CI 检查 (包括静态分析、安全扫描、测试矩阵) 全部通过。

## 常见问题

**Q: 系统如何处理目标网站反爬机制，例如验证码或频率限制？**

A: 默认 harvester 实现了指数退避重试策略，并支持配置自定义请求头 (User-Agent, Referer)。对于严格限制的站点，用户可以启用代理池模块或手动设置 `rate_limit` 参数 (请求/秒)。系统不会自动破解验证码，建议在 `sources.json` 中将此类站点标记为 `requires_manual_review`，并通过 Webhook 通知管理员。

**Q: 索引数据存储在本地的什么位置？如何迁移到另一台服务器？**

A: 所有结构化数据存储在 PostgreSQL 数据库中，文件快照 (如原始 HTML 归档) 存放在 `data/snapshots/` 目录。迁移时，使用 `pg_dump` 导出数据库，并压缩快照目录。在新服务器上恢复数据库后，运行 `python manage.py rebuild --snapshot-path <路径>` 重新关联文件。对于生产环境，我们强烈建议将快照存储在 S3 或 NFS 共享存储上。

**Q: 如何自定义新增一个数据源，而不修改核心代码？**

A: 在 `config/sources.json` 中添加新条目，指定 `url`、`parser_type` (html, json, xml) 以及 `field_mapping` (将目标页面的 XPath/JSONPath 映射到内部字段)。如果目标结构复杂，可以编写一个独立的 harvester 类放在 `src/harvesters/custom/` 目录，并在 `__init__.py` 中注册。系统会在下次 `harvest --incremental` 运行时自动发现并处理新增源。

## 许可证

MIT License

Copyright (c) 2026 Dejia Resource Index Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
