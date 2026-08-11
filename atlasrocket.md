# BeiLian Resource Gateway

BeiLian Resource Gateway is a specialized technical aggregation and navigation system designed for sports data researchers, journalists, and analytics professionals who require structured access to regional competition results and statistical repositories. The project addresses the fragmentation of Chinese domestic sports result data by providing a unified, machine-readable indexing layer over multiple authoritative result sources. It is not a data scraping tool nor a database itself; rather, it operates as a curated gateway that normalizes access patterns, validates source availability, and provides standardized output formats for downstream integration into analytics pipelines, reporting dashboards, and historical trend analysis systems. The target users include data engineers in sports media, academic researchers studying regional athletic performance, and hobbyist developers building community-driven statistics platforms.

## 功能概览

- **Source Availability Probing** - Continuous health checks against all registered result endpoints, with configurable timeout and retry policies, outputting JSON-formatted status reports.

- **Canonical URL Normalization** - Automatically rewrites and validates result source URLs according to a predefined rule set, ensuring that all downstream requests target the correct authoritative base without protocol or path ambiguities.

- **Result Schema Standardization** - Parses HTML and plain-text result pages from multiple sources and transforms them into a unified tabular schema comprising event time, participant identifiers, score lines, and venue metadata.

- **Historical Snapshot Archiving** - Maintains daily immutable snapshots of all reachable result pages, stored as compressed plain-text files with cryptographic hash verification, enabling point-in-time reconstruction.

- **Query Filtering and Sorting** - Provides a lightweight command-line interface for filtering results by competition category, date range, and participant name, with sorting by date, score, or source reliability score.

- **Notification Dispatch** - Sends alerts via webhook or local syslog when a source becomes unreachable or when schema parsing fails, supporting operational monitoring without external dependencies.

- **Export Generators** - Produces CSV, JSON Lines, and Markdown table exports from the internal normalized dataset, suitable for manual inspection or import into spreadsheet software.

- **Local Cache with TTL** - Caches parsed results in a local SQLite database with configurable time-to-live per source, reducing repeated network fetches and improving response time for frequent queries.

## 应用场景

- **Post-Match Reporting Automation** - Journalists and content management systems can query the gateway to retrieve standardized result snippets for specific competition IDs, automatically populating match reports without manual copy-pasting from multiple websites.

- **Academic Performance Analysis** - Researchers studying regional athletic development over multiple seasons can use the historical snapshot feature to collect consistent datasets across years, bypassing the variability of source page redesigns and URL changes.

- **Community Statistics Dashboard** - Independent hobbyist developers building community websites can integrate the gateway's JSON export endpoint to display live-updating result tables on their forums, with minimal backend coding required.

- **Data Quality Validation** - Quality assurance teams can run the source probing module on a scheduled basis to detect stale or malformed result pages, generating alert tickets before users encounter broken links in production applications.

- **Competition Archives Migration** - Archive engineers can use the export generators to produce structured data dumps for long-term storage in institutional repositories, ensuring that historical results remain accessible even when original sources are decommissioned.

## 快速开始

```bash
# Clone the repository from the official mirror
git clone https://github.com/example/beilian-gateway.git
cd beilian-gateway

# Install required Python packages and system dependencies
pip install -r requirements.txt
python setup.py develop

# Initialize the local cache database and perform an initial probe
python -m beilian_gateway init --sources config/sources.yaml
python -m beilian_gateway probe --all --output status.json

# Run a sample query for today's results across all registered sources
python -m beilian_gateway query --date 2026-08-11 --format table
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 或更高 | 核心解释器，用于运行网关引擎和命令行工具 |
| SQLite | 3.31 或更高 | 本地缓存和快照存储的嵌入式数据库 |
| libxml2 | 2.9.10 或更高 | HTML 解析加速库，用于提高结果页面提取性能 |
| curl | 7.68.0 或更高 | 用于底层 HTTP 请求的健康检查和内容获取 |
| git | 2.25.0 或更高 | 版本控制工具，用于克隆和更新项目代码 |
| make | 3.81 或更高 | 构建辅助工具，用于运行自动化测试和文档生成 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | docs/user/query-syntax.md | 如何构造日期过滤、来源筛选和排序参数？ |
| 管理员手册 | docs/admin/source-configuration.md | 如何添加、修改或禁用新的结果源端点？ |
| 开发参考 | docs/dev/parsing-architecture.md | 解析器如何适配不同来源的 HTML 结构差异？ |
| 运维手册 | docs/ops/monitoring-alerts.md | 如何配置 webhook 告警和日志轮转策略？ |
| 设计说明 | docs/design/caching-strategy.md | 缓存失效策略如何权衡数据新鲜度和系统负载？ |
| 贡献者指南 | CONTRIBUTING.md | 提交代码或文档变更需要遵循哪些规范和流程？ |

## 资源列表

本系统注册并维护以下结果源端点，所有 URL 均按用户提供原样收录，不作任何协议或域名改写：

官方结果主站点

<code>beimailiansaibeibisaijieguo.org.cn</code>

<code>beimailiansaibeibifen.org.cn</code>

杯赛分组与积分站点

<code>bajiazuqiubifenwang.org.cn</code>

<code>bajiazuqiubifen.org.cn</code>

杯赛赛程与成绩站点

<code>bajiasaichengjieguo.org.cn</code>

<code>bajiasaicheng.org.cn</code>

杯赛即时比分站点

<code>bajiajishibifen.net.cn</code>

## 项目结构

```
beilian-gateway/
├── config/                           # 配置文件目录
│   ├── sources.yaml                  # 注册的所有结果源端点及元数据
│   ├── parsing_rules/                # 每个源对应的解析规则子目录
│   │   ├── beimailiansaibeibisaijieguo.yaml
│   │   ├── beimailiansaibeibifen.yaml
│   │   ├── bajiazuqiubifenwang.yaml
│   │   ├── bajiazuqiubifen.yaml
│   │   ├── bajiasaichengjieguo.yaml
│   │   ├── bajiasaicheng.yaml
│   │   └── bajiajishibifen.yaml
│   └── logging.yaml                  # 日志级别和输出目标配置
├── beilian_gateway/                  # 主要 Python 源码包
│   ├── __init__.py                   # 包初始化，暴露核心 API
│   ├── probe/                        # 健康检查和可用性探测模块
│   │   ├── checker.py                # 并发 HTTP 探测及响应分析
│   │   └── reporter.py               # 生成 JSON/HTML 状态报告
│   ├── parser/                       # 结果页面解析引擎
│   │   ├── base.py                   # 抽象解析器基类
│   │   ├── html_parser.py            # 基于 libxml2 的 HTML 提取器
│   │   └── registry.py               # 源到解析器的动态映射注册
│   ├── cache/                        # 本地缓存和快照管理
│   │   ├── db.py                     # SQLite 连接池和表结构初始化
│   │   ├── ttl.py                    # 基于来源的 TTL 计算策略
│   │   └── snapshot.py               # 每日快照创建和压缩
│   ├── export/                       # 导出格式生成器
│   │   ├── csv_generator.py          # 逗号分隔值输出
│   │   ├── jsonl_generator.py        # JSON Lines 流式导出
│   │   └── markdown_generator.py     # 人类可读的 Markdown 表格
│   ├── cli/                          # 命令行接口子命令
│   │   ├── init.py                   # 初始化和数据库迁移
│   │   ├── probe.py                  # 探测子命令
│   │   ├── query.py                  # 查询和过滤子命令
│   │   └── export.py                 # 导出子命令
│   └── utils/                        # 通用工具函数
│       ├── network.py                # 带重试的 HTTP 请求封装
│       ├── hash.py                   # SHA-256 校验和计算
│       └── datetime.py               # 时区感知的日期解析工具
├── tests/                            # 单元测试和集成测试
│   ├── test_probe.py                 # 探测模块的模拟测试
│   ├── test_parser.py                # 各源解析器的样本数据测试
│   └── fixtures/                     # 测试用的样本 HTML 文件
│       ├── beimailiansaibeibisaijieguo_sample.html
│       └── bajiazuqiubifen_sample.html
├── docs/                             # 文档源码
│   ├── user/                         # 面向终端用户的指南
│   ├── admin/                        # 面向系统管理员的配置文档
│   ├── dev/                          # 面向开发者的内部设计文档
│   └── ops/                          # 面向运维的部署和监控文档
├── scripts/                          # 辅助运维脚本
│   ├── daily_probe.sh                # crontab 调用的每日探测脚本
│   └── archive_cleaner.py            # 清理超过 90 天的旧快照
├── requirements.txt                  # Python 依赖列表
├── setup.py                          # 安装和分发包配置
├── Makefile                          # 常用开发任务快捷命令
└── README.md                         # 本文件
```

## 贡献指南

1. 在 GitHub 上 fork 本仓库并创建功能分支，分支命名遵循 `feature/<简短描述>` 或 `fix/<问题编号>` 格式，确保分支历史干净且每个提交聚焦于单一逻辑变更。

2. 修改或新增解析器时，必须附带对应的样本 HTML 文件放入 `tests/fixtures/` 目录，并编写至少三个单元测试用例覆盖正常解析、缺失字段处理和异常页面三种情形。

3. 更新 `config/sources.yaml` 添加新源端点时，需要同步填写 `parsing_rules/` 下对应的规则文件，包括字段映射、选择器路径和数据类型转换说明，并执行 `make validate-config` 校验语法正确性。

4. 所有提交的 Python 代码必须通过 `black` 和 `flake8` 格式检查，并运行 `make test` 确保本地所有测试通过后，再发起 Pull Request 到主分支。

5. 文档更新需同步修改 `docs/` 下对应章节，并在 `CHANGELOG.md` 中记录变更摘要，说明该变更对用户或运维人员的影响，便于版本发布时汇总。

## 常见问题

**问：某个源端点返回 403 禁止访问错误，网关是否会自动重试？**

答：网关的探测模块默认启用指数退避重试机制，最多重试 3 次，间隔分别为 1 秒、2 秒和 4 秒。如果所有重试均失败，该源会被标记为 `unreachable` 状态，并触发配置的 webhook 告警。管理员可手动运行 `python -m beilian_gateway probe --source <域名> --force` 强制重新探测。

**问：本地 SQLite 缓存文件会无限增长吗？**

答：缓存数据库包含自动清理策略。每个源最多保留最近 30 天的每日快照，超过 30 天的旧记录会在每日凌晨的维护任务中被移除。同时，导出生成器产生的临时文件存放在 `tmp/` 目录，超过 24 小时未被访问的文件会被清理脚本删除。管理员可通过修改 `config/logging.yaml` 中的 `cache_retention_days` 参数调整保留时长。

**问：如何在不修改主配置文件的情况下，临时禁用某个源？**

答：您可以使用环境变量 `BEILIAN_SOURCE_BLACKLIST` 指定逗号分隔的禁用域名列表，例如 `export BEILIAN_SOURCE_BLACKLIST=<code>bajiasaicheng.org.cn</code>,<code>bajiasaichengjieguo.org.cn</code>`。网关初始化时会读取该变量，跳过黑名单中的源，该方式不影响配置文件的完整性，适合临时调试或应对源站维护窗口。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
