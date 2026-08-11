# Puchao Resource Navigator

Puchao Resource Navigator is a specialized technical resource aggregation and external link management system designed for developers, researchers, and technical analysts who need to organize, categorize, and rapidly access domain-specific information sources. The project addresses the critical challenge of link rot, resource fragmentation, and inefficient bookmark management by providing a structured, version-controlled, and queryable repository of curated external URLs.

The platform targets technical teams that maintain large collections of external references, API endpoints, documentation portals, and data sources. Rather than relying on browser bookmarks or unstructured text files, Puchao Resource Navigator offers a markdown-based, Git-friendly approach to resource lifecycle management, complete with validation hooks, metadata tagging, and dependency mapping between linked resources.

## 功能概览

- **Structured Resource Indexing** – Organize external URLs under hierarchical categories with custom metadata fields including status codes, last-verified timestamps, and content-type hints.

- **Automated Link Health Checking** – Integrate with CI pipelines to periodically test all registered URLs for HTTP availability and response time, flagging broken or redirected links.

- **Tag-Based Retrieval System** – Assign multiple tags per resource entry and perform Boolean queries across the entire dataset to quickly locate relevant references.

- **Markdown-Centric Data Storage** – Store all resource definitions in human-readable markdown files, enabling seamless version control diffs, offline viewing, and collaborative editing.

- **External API Proxy Layer** – Provide a lightweight proxy service that rewrites external URLs to bypass CORS restrictions during development and testing phases.

- **Resource Dependency Visualization** – Generate ASCII or Mermaid diagrams that illustrate relationships between linked resources, such as API dependencies, data flow chains, or documentation hierarchies.

- **Batch Import/Export Utilities** – Support CSV, JSON, and plain-text list imports for bulk addition of resources, along with scheduled export to static HTML for public sharing.

- **Watchdog Notification Module** – Send alerts via email or webhook when critical resources become unavailable or when SSL certificates are about to expire.

## 应用场景

- **Microservices Documentation Hub** – A team maintaining twenty-plus microservices can use Puchao Resource Navigator to centralize links to each service's Swagger UI, health check endpoints, and deployment dashboards, reducing the time spent searching internal wikis.

- **Academic Research Reference Management** – Researchers aggregating datasets, pre-print servers, and statistical computing environments can organize thousands of external links with verification timestamps, ensuring that cited sources remain accessible throughout a multi-year study.

- **DevOps Monitoring Aggregation** – Site reliability engineers can collect URLs for Prometheus exporters, Grafana instances, and cloud provider status pages into a single navigable structure, with automated alerts when monitoring endpoints become unreachable.

- **Open-Source Project Documentation** – Large open-source projects with extensive external dependencies can maintain a verified list of upstream project URLs, mirror sites, and migration guides, helping contributors quickly find authoritative sources without sifting through outdated forum posts.

- **Compliance and Audit Trail Management** – Regulated industries can use the system to track external regulatory portals, standard body websites, and compliance checklist sources, with audit logs showing when each link was last reviewed and by whom.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/puchao/resource-navigator.git
cd resource-navigator

# Install dependencies (Python 3.9+ required)
pip install -r requirements.txt

# Initialize the local resource database
python navigator.py init --seed data/default_resources.md

# Run the validation service on port 8080
python navigator.py serve --port 8080 --watch

# Verify all registered URLs (offline mode)
python navigator.py verify --timeout 5 --retries 2
```

The `serve` command starts the web-based navigation interface at `http://localhost:8080`. The `verify` command performs a one-time health check on all resources and outputs a CSV report to `reports/verification_<timestamp>.csv`. For background monitoring, use `python navigator.py daemon --interval 3600` to run continuous verification every hour.

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 - 3.12 | Core runtime environment; 3.11 recommended for performance |
| aiohttp | >=3.9.0 | Asynchronous HTTP client for concurrent link verification |
| jinja2 | >=3.1.0 | Template engine for rendering the web navigation interface |
| pyyaml | >=6.0 | YAML parser for advanced configuration files and metadata |
| click | >=8.1.0 | Command-line interface framework for subcommand routing |
| watchdog | >=3.0.0 | Filesystem event monitor for auto-reloading resource indexes |
| pytest | >=7.4.0 (development only) | Unit test framework for running the validation test suite |
| black | >=23.0.0 (development only) | Code formatter to maintain consistent style across contributions |
| mypy | >=1.5.0 (optional) | Static type checker for catching type-related errors early |

All production dependencies are listed in `requirements.txt`. For development, use `requirements-dev.txt` which includes testing and linting tools. The system is platform-agnostic and runs on Linux, macOS, and Windows Subsystem for Linux (WSL2).

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | `docs/getting-started/` | How to install, configure, and launch the navigator for the first time? What are the minimal setup steps? |
| 资源管理 | `docs/resource-management/` | How to add, edit, delete, and tag external URLs? How to perform batch imports and exports? |
| 运维手册 | `docs/operations/` | How to set up health-check cron jobs, configure alert webhooks, and tune performance for large datasets? |
| 架构设计 | `docs/architecture/` | What is the internal data model? How does the proxy layer work? How to extend with custom verifiers? |
| API 参考 | `docs/api-reference/` | Which RESTful endpoints are available for programmatic access? What are request/response schemas? |
| 故障排查 | `docs/troubleshooting/` | How to resolve common issues like SSL errors, rate-limiting, or corrupted index files? |

Each document in these directories is written in markdown and includes executable code snippets where applicable. The `docs/architecture/` folder contains sequence diagrams and entity-relationship models to aid contributors and maintainers.

## 资源列表

### 核心赛事数据源

<code>puchaosheshoubang.asia</code>

<code>puchaosaicheng.asia</code>

<code>puchaoqianzhan.asia</code>

### 实时信息聚合

<code>puchaoliansai.asia</code>

<code>puchaoguanwang.asia</code>

### 数据分析与结果

<code>puchaofenxi.asia</code>

<code>puchaobisaijieguo.asia</code>

These resources form the initial seed set for the navigator's default index. Users are encouraged to add supplementary URLs relevant to their specific domains. Each listed URL is treated as a first-class entry with auto-generated UUID, timestamp of addition, and a default tag set derived from the domain name.

## 项目结构

```
puchao-resource-navigator/
├── navigator.py                 # Main CLI entry point with subcommand routing
├── requirements.txt             # Production dependency list
├── requirements-dev.txt         # Development and testing dependencies
├── config/
│   ├── default.yaml             # Base configuration (port, timeout, retry)
│   ├── logging.conf             # Logging format and level definitions
│   └── schema.json              # JSON Schema for resource metadata validation
├── src/
│   ├── __init__.py
│   ├── core/
│   │   ├── indexer.py           # Resource index building and query engine
│   │   ├── validator.py         # HTTP/S validation logic with retry policies
│   │   ├── proxy.py             # CORS-avoiding proxy handler
│   │   └── watcher.py           # Filesystem watch and auto-reload routines
│   ├── web/
│   │   ├── app.py               # aiohttp web application factory
│   │   ├── routes.py            # URL route definitions and handlers
│   │   ├── templates/           # Jinja2 HTML templates for UI rendering
│   │   └── static/              # CSS, JavaScript, and favicon assets
│   ├── cli/
│   │   ├── commands.py          # Click command implementations
│   │   └── helpers.py           # Shared CLI utilities and formatters
│   └── utils/
│       ├── parsers.py           # CSV/JSON/plain-text import parsers
│       ├── exporters.py         # HTML/JSON/Markdown export generators
│       └── notifiers.py         # Email and webhook alert senders
├── data/
│   ├── default_resources.md     # Seed resource list in markdown format
│   ├── tags.yaml                # Predefined tag taxonomy with descriptions
│   └── user_resources/          # User-added resources (one file per entry)
├── tests/
│   ├── unit/                    # Unit tests for core modules
│   ├── integration/             # Integration tests with live HTTP mocks
│   └── fixtures/                # Sample data and mock responses
├── docs/                        # Full documentation (see Documentation Navigation)
├── scripts/
│   ├── pre-commit-hook.sh       # Git pre-commit hook for link validation
│   └── migration-v1-to-v2.py    # Data migration script for schema upgrades
└── LICENSE                      # MIT License file
```

The directory tree includes over five subdirectories at the root level, each with specialized roles. The `src/core/` module handles all business logic, while `src/web/` manages the presentation layer. User data is stored separately from system data to simplify backups and upgrades.

## 贡献指南

1. **Fork and Clone the Repository** – Create a personal fork of the main repository on GitHub, then clone your fork locally. Set up the upstream remote to track changes from the original project.

2. **Create a Feature Branch** – Use a descriptive branch name that reflects the nature of your contribution, such as `feat/add-http3-verifier` or `fix/issue-42-timeout-handling`. Branch from the latest `main` or `develop` branch.

3. **Implement Changes with Tests** – Write clean, commented code that adheres to the project's PEP 8 style guidelines. Add unit tests for new functionality and update existing tests if your changes affect core behavior. Run `pytest tests/` to ensure all tests pass before committing.

4. **Update Documentation** – If your changes introduce new configuration options, CLI commands, or API endpoints, update the relevant markdown files in the `docs/` folder. Include code examples where appropriate.

5. **Submit a Pull Request** – Push your branch to your fork and open a pull request against the main repository's `develop` branch. Fill out the pull request template completely, referencing any related issues. Wait for the CI pipeline to run verification and for maintainers to review your submission.

All contributions are subject to the Developer Certificate of Origin (DCO) process. By submitting a pull request, you agree that your contribution can be included in the project under the MIT License.

## 常见问题

**Q: How does the navigator handle resources that require authentication or have session-based access?**

A: The validator module supports custom request headers and cookie injection via per-resource metadata fields. You can store static authentication tokens or API keys in a separate encrypted vault file (not committed to Git) and reference them in the resource configuration. For session-based resources, we recommend using the proxy layer with a session persistence plugin, which is documented in `docs/operations/auth-integration.md`. The system does not store user credentials in plain text within the index files.

**Q: Can the navigator detect changes in resource content, not just availability?**

A: Yes, the `validator.py` module includes an optional content-hashing feature. When enabled, the system computes a SHA-256 hash of the response body (up to a configurable size limit) and stores it alongside the verification timestamp. Subsequent verifications compare the new hash against the stored value, and a mismatch triggers a content-change alert. This is particularly useful for monitoring API response schemas or regulatory document updates. Enable this feature by setting `content_hashing: true` in `config/default.yaml` and adjusting the `hash_max_bytes` parameter.

**Q: How to migrate existing bookmark collections or CSV lists into the navigator?**

A: Use the batch import command: `python navigator.py import --format csv --path bookmarks.csv --tag-prefix imported`. The parser automatically maps common CSV columns (URL, title, description, tags) to the internal resource schema. For plain-text lists (one URL per line), use `--format txt`. After import, run the verification command to immediately check the health of all newly added resources. Detailed mapping rules and template files are available in `docs/resource-management/batch-import.md`.

## 许可证

This project is licensed under the MIT License. You are free to use, copy, modify, merge, publish, distribute, sublicense, and sell copies of the software, subject to the following conditions: The above copyright notice and this permission notice shall be included in all copies or substantial portions of the software. The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and noninfringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability, whether in an action of contract, tort, or otherwise, arising from, out of, or in connection with the software or the use or other dealings in the software.

For the full license text, refer to the `LICENSE` file in the project root.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:18
