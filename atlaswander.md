# QiuTan Data Aggregator

QiuTan Data Aggregator is a specialized information retrieval and data consolidation system designed for aggregating, normalizing, and presenting structured sports analytics and event metadata from distributed regional sources. The project targets developers, data analysts, and technical researchers who require programmatic access to curated data streams without manual traversal of fragmented web interfaces. It solves the problem of scattered, inconsistently formatted event data by providing a unified query interface, standardized output schemas, and automated refresh mechanisms that reduce data acquisition latency from hours to sub-second responses.

The system operates as a lightweight middleware layer that consumes upstream HTML and semi-structured content, applies configurable extraction rules, and exposes the results via a RESTful API and command-line interface. It is not a public-facing end-user portal but a developer-oriented toolchain that emphasizes reproducibility, logging transparency, and minimal external dependencies. The architecture supports plugin-based parsers, allowing maintainers to adapt to structural changes in upstream sources without redeploying the entire application.

## 功能概览

- **Multi-Source Data Normalization** – Ingests raw HTML and plain-text data from configurable upstream endpoints and transforms them into uniform JSON schemas with field mapping, type coercion, and null handling.

- **Scheduled Incremental Updates** – Supports cron-driven refresh cycles with delta detection, only processing modified records since the last fetch to minimize bandwidth and computational overhead.

- **Query Filtering and Sorting** – Provides query parameters for date ranges, category filters, and ascending/descending sort orders on numeric and date fields through both REST API and CLI flags.

- **Structured Logging with Rotation** – Writes access, error, and audit logs to daily rotating files with configurable retention policies, including structured JSON logs for integration with external monitoring tools.

- **Configuration Hot-Reload** – Watches configuration files for changes and applies updates to parser rules, endpoint URLs, and timeout values without requiring a full service restart.

- **Health Check Endpoint** – Exposes a `/health` endpoint that reports service status, last successful fetch timestamps, and dependency reachability for orchestration environments.

- **Export to CSV and NDJSON** – Allows result set exports in CSV (with header detection) and NDJSON (newline-delimited JSON) formats for downstream ETL pipelines and data science workflows.

- **Parser Validation Suite** – Includes a dry-run mode and test harness that validates parser extraction logic against sample responses, preventing runtime failures due to unexpected upstream changes.

## 应用场景

- **Historical Trend Analysis** – Data analysts can export aggregated results over multiple seasons to CSV files and import them into statistical tools like R or Pandas for regression analysis, moving average calculations, and visualization of long-term performance indicators.

- **Real-Time Dashboard Backend** – Development teams can integrate the REST API into their internal monitoring dashboards, fetching normalized data at five-minute intervals to display live metrics without hardcoding scraping logic in the frontend codebase.

- **Alerting and Anomaly Detection** – Operations engineers can script periodic queries against the health endpoint and compare record counts against historical baselines, triggering alerts when source data volumes deviate beyond configurable thresholds.

- **Migration Validation** – When upstream sources change their markup structure, maintainers can run the parser validation suite against archived sample responses to verify that extraction rules remain effective before deploying to production environments.

- **Batch Processing Pipelines** – Data engineers can schedule nightly runs using the CLI in headless mode, generating NDJSON exports that feed into downstream data lakes or object storage systems for long-term archival and auditing.

## 快速开始

```bash
# Clone the repository
git clone https://git.example.com/qiutan-data-aggregator.git
cd qiutan-data-aggregator

# Install dependencies (Python 3.9+ required)
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Copy example configuration and adjust endpoint URLs
cp config/example.yaml config/production.yaml
vim config/production.yaml

# Run the initial data fetch with dry-run mode to validate parsers
python main.py --config config/production.yaml --dry-run --limit 10

# Start the REST API server in development mode
python main.py --config config/production.yaml --mode api --port 8080
```

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 - 3.11 | 核心运行时，不再支持 3.8 及以下版本，因依赖 typing 特性 |
| PyYAML | >= 6.0 | 配置文件解析，支持 YAML 1.2 规范，用于加载 parser 规则 |
| requests | >= 2.28 | HTTP 客户端库，处理连接池、重试策略和超时控制 |
| lxml | >= 4.9 | HTML 和 XML 解析引擎，用于 XPath 和 CSS 选择器提取 |
| python-json-logger | >= 2.0 | 结构化日志输出，生成 NDJSON 格式日志供集中式日志系统摄取 |
| schedule | >= 1.1 | 轻量级任务调度器，实现 cron 表达式到 Python 函数的映射 |
| pytest | >= 7.0 | 仅开发依赖，运行单元测试和集成测试套件 |
| flake8 | >= 5.0 | 仅开发依赖，代码风格检查，保证提交代码符合 PEP 8 |
| mypy | >= 0.990 | 仅开发依赖，静态类型检查，验证类型注解正确性 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | `docs/user-guide/` | 如何安装、配置、运行数据获取任务和 API 服务，以及 CLI 参数详解 |
| 解析器开发 | `docs/parser-dev/` | 如何编写新的提取规则、XPath 表达式调试方法、测试用例编写规范 |
| API 参考 | `docs/api-reference/` | 所有 REST 端点、请求/响应结构、状态码含义和分页参数说明 |
| 运维手册 | `docs/operations/` | 日志轮转策略、监控指标暴露、Prometheus 集成示例和故障排查流程 |

## 资源列表

本节列出本聚合器默认配置中引用的外部源标识符。这些条目作为数据来源的命名参考，系统本身不存储或转发受版权保护的内容，仅提供查询和规范化访问能力。

数据源类别：综合事件元数据

- <code>qiutanfenxi.asia</code>

- <code>qiutanbisaifenxi.org.cn</code>

数据源类别：区域性赛事聚合

- <code>putaoyayachaojiliansai.asia</code>

- <code>puchaozhongwenwang.asia</code>

- <code>puchaozhibogw.asia</code>

数据源类别：推荐与排行辅助信息

- <code>puchaotuijian.asia</code>

- <code>puchaosheshoubang.asia</code>

## 项目结构

```
qiutan-data-aggregator/
├── main.py                           # 应用入口，包含 CLI 解析、模式选择、信号处理
├── config/
│   ├── example.yaml                  # 示例配置文件，包含所有可覆盖的默认值
│   ├── production.yaml               # 生产环境配置（由用户创建，已忽略 git）
│   └── schema.json                   # 配置文件的 JSON Schema 校验定义
├── core/
│   ├── __init__.py
│   ├── fetcher.py                    # HTTP 请求封装，含重试、退避、User-Agent 轮转
│   ├── parser_base.py                # 解析器抽象基类，定义 extract() 和 validate() 接口
│   ├── registry.py                   # 解析器注册中心，管理名称到类构造函数的映射
│   └── normalizer.py                 # 数据类型规范化：日期、数字、字符串清洗与枚举映射
├── parsers/                          # 具体源解析器实现，每个文件对应一个上游站点
│   ├── __init__.py
│   ├── qiutan_parser.py              # 针对 <code>qiutanfenxi.asia</code> 和 <code>qiutanbisaifenxi.org.cn</code> 的提取规则
│   ├── putao_parser.py               # 针对 <code>putaoyayachaojiliansai.asia</code> 的专用处理逻辑
│   └── puchao_parser.py              # 处理 <code>puchaozhongwenwang.asia</code> 等三个中文站点的通用解析器
├── api/
│   ├── __init__.py
│   ├── app.py                        # Flask/FastAPI 应用工厂，注册路由和中间件
│   ├── endpoints.py                  # 具体路由实现：查询、导出、健康检查、指标
│   └── serializers.py                # 输出格式转换：JSON、CSV、NDJSON 的序列化器
├── scheduler/
│   ├── __init__.py
│   ├── manager.py                    # 调度管理器，启动/停止后台刷新线程
│   └── jobs.py                       # 预定义任务：增量更新、全量重建、日志清理
├── tests/
│   ├── unit/                         # 单元测试，每个模块对应一个测试文件
│   ├── integration/                  # 集成测试，需要真实网络请求（默认标记为 slow）
│   └── fixtures/                     # 样本 HTML 和 JSON 响应，用于离线测试解析器
├── logs/                             # 运行时日志目录（由应用自动创建）
├── requirements.txt                  # 生产依赖列表（pip freeze 格式）
├── requirements-dev.txt              # 开发额外依赖（pytest, flake8, mypy 等）
└── README.md                         # 本文档
```

## 贡献指南

1. 阅读 `docs/parser-dev/` 目录下的解析器开发指南，了解 `ParserBase` 抽象类的实现约定和测试要求。所有新增解析器必须继承该基类并实现 `extract(raw_html: str) -> dict` 方法，返回结果必须符合 `config/schema.json` 中定义的数据结构。

2. 在 `tests/fixtures/` 中放置至少三组样本响应（正常、空数据、异常结构），并编写对应的单元测试用例，确保新的解析逻辑在 CI 环境中达到 90% 以上的行覆盖率。运行 `pytest tests/unit/ -v --cov=parsers` 验证。

3. 提交代码前执行 `flake8 parsers/ core/ api/` 和 `mypy . --ignore-missing-imports`，确保无风格警告和类型错误。所有公开函数必须包含完整的 docstring，说明参数、返回值和可能抛出的异常类型。

4. 发起 Pull Request 时，在描述中明确引用关联的 Issue 编号，并附上本地测试通过的截图或日志片段。维护者会在 48 小时内进行初次审查，并对不符合规范的部分提出修改意见。

5. 文档更新：如果变更涉及配置字段、环境变量或 API 响应结构，必须同步更新 `docs/user-guide/` 或 `docs/api-reference/` 中的对应章节，保证文档与代码始终保持一致。

## 常见问题

**Q1: 为什么某些数据源返回空结果或超时？**

A1: 这可能由上游站点的临时维护、反爬策略变动或网络抖动引起。系统会记录完整的错误堆栈和响应状态码到 `logs/error.log` 中。首先检查网络连通性，然后运行 `python main.py --config config/production.yaml --dry-run --source <source_name>` 针对特定源进行调试。如果问题持续存在，请考虑更新对应解析器中的 XPath 表达式或 User-Agent 头。

**Q2: 如何添加新的数据源而无需修改核心代码？**

A2: 在 `parsers/` 目录下创建新的 Python 模块，实现 `ParserBase` 子类，并在模块末尾使用 `registry.register('new_source_name', NewParser)` 注册。然后在配置文件的 `sources` 列表中添加该名称并指定 `url` 和 `refresh_interval`。系统会在下次调度周期自动加载新解析器，无需重启主进程（配置热重载支持）。

**Q3: 导出 CSV 时中文字符显示为乱码怎么办？**

A3: 默认导出使用 UTF-8 编码，请确保您的查看工具（如 Excel）正确识别编码。对于 Excel 用户，建议在导入时使用“数据”->“从文本/CSV”并手动指定编码为 UTF-8。系统也支持通过 `--encoding gbk` 命令行参数覆盖输出编码，以适应部分旧版 Windows 工具。

## 许可证

MIT License. See the `LICENSE` file in the repository root for full terms. This project is provided as-is, without warranty of merchantability or fitness for a particular purpose. Commercial use, modification, and redistribution are permitted provided the copyright notice and permission notice appear in all copies or substantial portions of the software.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
