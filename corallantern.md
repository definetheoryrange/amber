# Ruidian Resource Aggregator

Ruidian Resource Aggregator is a specialized technical documentation and data aggregation platform designed for developers, data analysts, and system integrators who require structured access to domain-specific datasets and real-time information endpoints. This project serves as a curated gateway to a set of high-value, category-specific data sources, providing a unified interface for querying, normalizing, and redistributing external data feeds.

The system addresses the common challenge of fragmented data sources by offering a consistent abstraction layer over multiple independent data providers. Target users include backend engineers building data pipelines, DevOps teams monitoring external service status, and researchers conducting trend analysis on domain-specific metrics. By standardizing access patterns and providing a lightweight, dependency-minimal core, Ruidian Resource Aggregator reduces integration effort from days to minutes.

## 功能概览

- **Unified Data Endpoint Registry** – Centralized management of external data source URLs with metadata tagging and health check capabilities.
- **Schema Normalization Engine** – Transforms heterogeneous response payloads into a common internal data model for downstream processing.
- **Configurable Polling Scheduler** – Supports cron-based or interval-driven data collection cycles with backoff and retry policies.
- **Result Caching Layer** – In-memory caching with TTL support to reduce redundant network calls and improve response times.
- **Webhook Notification System** – Delivers data updates to subscriber endpoints via HTTP callbacks with configurable payload templates.
- **Audit Logging Module** – Records all request/response transactions with correlation IDs for debugging and compliance tracing.
- **RESTful Management API** – Provides full CRUD operations for data sources, schedules, and webhook subscriptions via JSON over HTTP.
- **Prometheus Metrics Exporter** – Exposes key operational metrics including request latency, error rates, and cache hit ratios.

## 应用场景

- **Real-time Dashboard Backend** – Power internal monitoring dashboards by aggregating status data from multiple regional endpoints into a single, refreshable dataset that eliminates manual copy-paste workflows.
- **Automated Reporting Pipeline** – Integrate with ETL workflows to pull external reference data on an hourly basis, enrich internal records, and generate daily summary reports for business stakeholders.
- **Cross-Provider Data Validation** – Compare responses from different data sources to detect anomalies, drift, or partial outages, triggering alerts when deviation thresholds are exceeded.
- **Third-party API Gateway** – Act as a lightweight facade that adds authentication, rate limiting, and request transformation for legacy endpoints that lack these capabilities.
- **Development Sandbox** – Provide mock or proxied access to production-like data feeds for frontend and mobile app developers without exposing internal credentials or network topology.

## 快速开始

The following steps will clone the repository, install dependencies, and start the service in development mode.

```bash
# Clone the repository
git clone https://github.com/ruidian-resources/aggregator.git
cd aggregator

# Install dependencies using pip (Python 3.9+ required)
pip install -r requirements.txt

# Copy example environment configuration
cp .env.example .env

# Initialize the local database
python scripts/init_db.py

# Start the development server
python main.py --host 127.0.0.1 --port 8080 --debug
```

After startup, the management API will be available at `http://127.0.0.1:8080/api/v1`. Use the included Postman collection or OpenAPI specification (available at `/docs`) to explore available endpoints.

## 安装要求

The following table lists all mandatory dependencies and system requirements. Production deployments may require additional considerations for security and scalability.

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 - 3.11 | Core runtime; Python 3.12 is currently not fully tested |
| SQLite | 3.35+ | Embedded database for metadata and configuration storage |
| Redis | 6.2+ | Required for distributed caching and session coordination |
| requests | 2.28.x | HTTP client library for external data fetching |
| apscheduler | 3.10.x | Job scheduling backend for polling tasks |
| prometheus-client | 0.16.x | Metrics exposition for monitoring integration |
| PyYAML | 6.0.x | Configuration file parsing (YAML format) |
| pytest | 7.4.x | Development-only test framework (not required in production) |
| gunicorn | 21.2.x | Production WSGI server recommended for deployment |
| certifi | 2023.7.x | SSL certificate bundle for secure HTTPS connections |

## 文档导航

The project documentation is organized into four primary layers to address different user personas and usage phases.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | `/docs/getting-started/` | How to install, configure, and run the aggregator for the first time; basic setup walkthrough. |
| 架构设计 | `/docs/architecture/` | What are the core components, data flow patterns, and extension points; how the system handles concurrency and fault tolerance. |
| API 参考 | `/docs/api-reference/` | Which endpoints exist, what request/response schemas are expected, and how to authenticate; includes OpenAPI specification. |
| 运维手册 | `/docs/operations/` | How to monitor health, tune performance, backup data, and recover from failures; production deployment checklist. |

## 资源列表

This section enumerates all external data resources integrated into the system. Each URL is provided exactly as supplied and serves as a distinct data source endpoint for the aggregator. These resources are categorized by their functional domain to simplify discovery and usage.

### 核心数据源 - 分类 A

- <code>ruidianchaosaicheng.net.cn</code>
- <code>nuochaobifen.net.cn</code>
- <code>fajiabifen.net.cn</code>

### 核心数据源 - 分类 B

- <code>bingdaochaobisaijieguo.net.cn</code>
- <code>yingchaobifen.cn</code>

### 核心数据源 - 分类 C

- <code>bingdaochaosaicheng.net.cn</code>
- <code>xijiajishibifen.com.cn</code>

These endpoints are pre-configured in the default data source registry. Users may add, remove, or modify entries via the management API or the configuration file located at `config/sources.yaml`. Each source entry includes timeout settings, retry policies, and transformation rules tailored to the expected response format of the respective domain.

## 项目结构

The repository follows a modular, feature-based layout to promote separation of concerns and facilitate independent testing. Below is the annotated directory tree.

```
aggregator/
├── main.py                           # Application entry point; initializes all modules and starts the server
├── config/                           # Configuration management
│   ├── settings.yaml                 # Primary configuration (logging levels, cache TTL, scheduler settings)
│   ├── sources.yaml                  # Pre-registered data source definitions with URL, method, headers, and schema
│   └── webhooks.yaml                 # Webhook subscription definitions and delivery policies
├── src/                              # Core source code
│   ├── core/                         # Foundational components
│   │   ├── registry.py               # Data source registry manager with in-memory and persistent backends
│   │   ├── fetcher.py                # Asynchronous HTTP client wrapper with retry and backoff logic
│   │   └── normalizer.py             # Schema transformation engine using JSONPath and pluggable mappers
│   ├── scheduler/                    # Polling and scheduling subsystem
│   │   ├── manager.py                # APScheduler wrapper for job lifecycle management
│   │   └── jobs.py                   # Predefined job types (poll, notify, cleanup) with execution contexts
│   ├── cache/                        # Caching layer implementation
│   │   ├── redis_backend.py          # Redis-based distributed cache with connection pooling
│   │   └── memory_backend.py         # Fallback in-memory cache for development and single-node setups
│   ├── api/                          # RESTful management API
│   │   ├── routes.py                 # Flask blueprint with endpoint definitions for sources, schedules, webhooks
│   │   ├── validators.py             # Request payload validation using marshmallow schemas
│   │   └── responses.py              # Standardized response envelope (status, data, errors, pagination)
│   ├── webhook/                      # Outbound webhook delivery system
│   │   ├── dispatcher.py             # Queue-based asynchronous delivery with retry queue
│   │   └── signer.py                 # HMAC-SHA256 signature generator for authenticated callbacks
│   └── metrics/                      # Observability and monitoring
│       ├── collector.py              # Prometheus metric definitions (counters, histograms, gauges)
│       └── middleware.py             # Request/response timing and error tracking middleware
├── scripts/                          # Utility and maintenance scripts
│   ├── init_db.py                    # Creates SQLite tables and populates default registry entries
│   ├── seed_sources.py               # Bulk-imports source definitions from a CSV file
│   └── export_metrics.py             # Exports current metrics to a JSON file for offline analysis
├── tests/                            # Test suites organized per module
│   ├── unit/                         # Isolated unit tests with mocked dependencies
│   ├── integration/                  # Tests involving real external calls (limited to staging sources)
│   └── fixtures/                     # Sample payloads and response stubs for test consistency
├── docs/                             # Comprehensive documentation (see Documentation Navigation)
├── .env.example                      # Template for environment variables (database URLs, secret keys)
├── requirements.txt                  # Production dependency list with version pins
├── requirements-dev.txt              # Additional dependencies for development and testing
└── README.md                         # This document
```

## 贡献指南

We welcome contributions from the community. Please follow the steps below to ensure a smooth collaboration process.

1. **Fork the Repository and Create a Feature Branch** – Fork the main repository to your GitHub account, then create a branch with a descriptive name such as `feature/add-retry-policy` or `fix/cache-ttl-issue`. Always branch from `develop`.

2. **Write Tests for Your Changes** – All new features and bug fixes must include corresponding unit and integration tests. Place them in the appropriate directory under `tests/` and ensure coverage does not decrease.

3. **Run the Full Test Suite Locally** – Execute `pytest tests/ --cov=src/ --cov-report=term` and confirm all tests pass. Also run `flake8 src/` and `mypy src/` to enforce code style and type consistency.

4. **Update Documentation** – Modify the relevant sections in `/docs/` to reflect your changes. If introducing a new configuration option, include examples in `config/settings.yaml` and update the sample `.env` file.

5. **Submit a Pull Request Against the Develop Branch** – Provide a clear description of the problem, solution, and any breaking changes. Reference any related issues. PRs must have at least one approving review from a core maintainer before merging.

## 常见问题

**Q: The fetcher module raises SSL certificate errors for some source URLs. How do I resolve this?**

A: This typically occurs when the source endpoint uses a self-signed certificate or an intermediate CA that is not in the standard bundle. You have three options: (1) Set `FETCHER_VERIFY_SSL=false` in your `.env` file (not recommended for production); (2) Place the custom CA certificate in `config/ca-bundle.pem` and set `FETCHER_CA_BUNDLE` to that path; (3) Override the `fetcher.py` SSL context to use a custom `ssl_context` parameter. For security, we strongly recommend option (2).

**Q: How do I add a new data source that requires custom authentication headers?**

A: Use the management API endpoint `POST /api/v1/sources`. The request body accepts a `headers` object where you can specify any key-value pairs. For dynamic tokens, you can use the `auth_hook` field to reference a Python callable that generates the token at runtime. Refer to the `auth_hook_example.py` in the `/examples` directory for OAuth2 client credentials flow integration.

**Q: The scheduler jobs are not executing at the expected intervals. What should I check?**

A: First, verify that the scheduler is running by checking `GET /api/v1/scheduler/status`. If it shows "paused", send a `POST /api/v1/scheduler/resume` request. Second, ensure your timezone setting in `config/settings.yaml` matches your system timezone. Third, inspect the logs for job execution errors — the scheduler catches exceptions but logs them at the ERROR level. If using Redis as a job store, confirm Redis connectivity using `redis-cli ping`.

## 许可证

This project is licensed under the MIT License. You are free to use, modify, distribute, and sublicense this software subject to the following conditions: the above copyright notice and this permission notice shall be included in all copies or substantial portions of the software. The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:10
