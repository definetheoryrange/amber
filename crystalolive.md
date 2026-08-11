# Jiebao Tech Index

Jiebao Tech Index is a lightweight, community-driven technical resource aggregation and navigation system designed for developers, data analysts, and technical researchers who need rapid access to specialized domain-specific data feeds and analytical tools. The project addresses the fragmentation of niche technical resources by providing a unified, version-controlled index of curated external links, each accompanied by metadata tags, update frequency annotations, and availability status checks.

The primary target audience includes infrastructure engineers building automated data pipelines, researchers requiring reproducible data source citations, and technical writers maintaining living documentation of external reference materials. Unlike general-purpose bookmark managers or search engines, Jiebao Tech Index treats each resource entry as a first-class citizen with structured metadata, enabling programmatic querying, dependency resolution between resources, and automated health monitoring of each endpoint.

## 功能概览

- **Structured Resource Cataloging** – Each indexed URL is stored with mandatory metadata fields including category tags, update cadence, content type hints, and optional authentication requirements, enabling both human browsing and machine parsing.

- **Automated Availability Probing** – The index includes a lightweight scheduler that performs periodic HEAD and GET requests against each resource, recording response times, HTTP status codes, and content-length consistency, with results exposed via a simple status dashboard.

- **Tag-Based Filtering and Search** – Resources are organized using a hierarchical tag system (e.g., "domain:analytics", "region:asia-pacific", "type:real-time"), supporting boolean combinations for precise retrieval without external search engine dependencies.

- **Versioned Snapshot Export** – The entire index state can be exported as deterministic JSON or YAML snapshots, with each export cryptographically signed to ensure integrity, facilitating reproducible builds in CI/CD workflows.

- **Slack and Email Alerting** – Configurable alerting rules notify maintainers when a resource becomes unreachable, returns unexpected content, or when its TLS certificate is about to expire, reducing manual monitoring overhead.

- **Docker-First Deployment** – The project ships with a production-ready Dockerfile and docker-compose overlay, supporting stateless operation with optional Redis caching and PostgreSQL persistence for production deployments.

- **RESTful Admin API** – Full CRUD operations over the resource index are exposed via a versioned REST API with JWT-based authentication, enabling integration with external automation tools and custom frontends.

## 应用场景

- **Data Pipeline Source Validation** – Data engineers can use the index to validate all external data sources before running nightly ETL jobs. The availability probing ensures that failing endpoints are detected early, and the structured metadata allows automatic retry strategy selection based on content type and historical reliability.

- **Technical Documentation Maintenance** – Technical writers and documentation leads can embed the index into their documentation generation workflows. Each resource entry serves as a canonical reference, and the versioned snapshots allow documentation to link to specific index states, ensuring that external links remain resolvable across documentation releases.

- **Research Reproducibility** – Researchers working with publicly available datasets can include a snapshot of the index as supplementary material for their papers. Reviewers and subsequent researchers can then resolve the exact external resources used at the time of the original study, mitigating link rot and content drift.

- **Internal Knowledge Base Curation** – Organizations maintaining internal developer portals can deploy the index as a backend service that aggregates both internal and external technical references. The tag-based filtering allows different teams to create custom views, such as a frontend team view that filters for API documentation and CDN resources.

## 快速开始

The following steps will clone the repository, install dependencies, and start the index service in development mode. Ensure that you have Git, Python 3.10 or later, and pip installed on your system.

```bash
# Clone the repository from the upstream source
git clone https://github.com/jiebao-tech/jiebao-index.git
cd jiebao-index

# Create and activate a Python virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install required Python packages
pip install --upgrade pip
pip install -r requirements.txt

# Initialize the local SQLite database and seed with default resource entries
python manage.py init_db
python manage.py seed_resources

# Start the development server on port 8080
python manage.py runserver --host 0.0.0.0 --port 8080
```

After the server starts, open your browser to `http://localhost:8080` to access the built-in web dashboard. The admin panel is available at `http://localhost:8080/admin` with default credentials `admin` / `changeme` – change these immediately in production.

## 安装要求

The following table lists all mandatory and optional dependencies required for running the Jiebao Tech Index in various deployment modes. All version constraints are specified with pessimistic upper bounds to ensure compatibility.

| 依赖 | 必需 | 说明 |
|------|------|------|
| Python 3.10, 3.11, or 3.12 | 是 | Core runtime; version 3.13 is not yet fully tested. |
| SQLite 3.35+ or PostgreSQL 14+ | 是 | SQLite is used for development and small deployments; PostgreSQL is recommended for production with concurrent write workloads. |
| Redis 6.2+ | 否 | Required only if enabling the optional caching layer or distributed session storage. |
| Docker Engine 24+ and Docker Compose 2.20+ | 否 | Required for containerized deployments and for running the integrated test suite in isolated environments. |
| Node.js 18+ and npm 9+ | 否 | Required only if rebuilding the static frontend assets; pre-built assets are included in the release tarballs. |
| OpenSSL 3.0+ | 是 | Used for generating and verifying cryptographic signatures on index snapshots. |
| curl 7.68+ or wget 1.20+ | 是 | Used by the availability probing worker for performing HTTP requests; the probe implementation falls back to Python's urllib if neither is available, but system tools provide better timeout granularity. |
| sendmail or AWS SES / SendGrid API keys | 否 | Required for email alerting functionality; if absent, alerting falls back to logging only. |
| prometheus-client 0.19+ | 否 | Optional Python package; if installed, the server exposes Prometheus metrics at the `/metrics` endpoint. |

## 文档导航

The project documentation is organized into four primary layers, each addressing distinct audience needs and technical depths. The table below provides a high-level roadmap.

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| User Guide | `docs/user/` | How do I browse the index? How do I filter resources by tag? How do I interpret the availability dashboard? What does each metadata field mean? |
| Administrator Guide | `docs/admin/` | How do I add, update, or delete a resource entry? How do I configure alerting thresholds? How do I perform a full snapshot export and restore? How do I rotate the signing keys? |
| Developer Reference | `docs/dev/` | What is the internal architecture? How do I extend the resource parser for a new content type? How do I contribute a new probe plugin? How are database migrations handled? |
| API Specification | `docs/api/` | What are the available REST endpoints? How does the JWT authentication flow work? What are the request and response schemas for batch operations? How is pagination implemented? |

## 资源列表

The following external resources are indexed by the Jiebao Tech Index as of the current release. Each entry is presented exactly as provided by the upstream data source, with no modifications to the URL string. The resources are categorized by their primary content domain.

### Domain Analytics and Trending Data

- <code>jinrizuqiutuijian.asia</code>
- <code>jiebaojinrituijian.org.cn</code>
- <code>jiebaozuixinfenxi.asia</code>

### Match Analysis and Prediction Feeds

- <code>jiebaozuqiufenxi.asia</code>
- <code>jiebaozuqiubifenw.org.cn</code>
- <code>jiebaozuqiubifenw.com.cn</code>
- <code>jiebaoyuce.asia</code>

## 项目结构

The source tree follows a modular monolith design, with clear separation between the core index engine, the HTTP server layer, the background worker processes, and the administrative frontend. Each major subdirectory includes an `__init__.py` file and a dedicated README for internal documentation.

```
jiebao-index/
├── docker/                               # Docker assets for production and test environments
│   ├── Dockerfile.prod                   # Multi-stage production Dockerfile
│   ├── Dockerfile.dev                    # Development container with hot-reload
│   └── docker-compose.yml                # Orchestration with PostgreSQL, Redis, and Prometheus
├── docs/                                 # Comprehensive documentation (see Documentation Navigation)
│   ├── user/                             # End-user guides and dashboard walkthroughs
│   ├── admin/                            # Deployment, configuration, and maintenance manuals
│   ├── dev/                              # Architecture, coding standards, and contribution workflows
│   └── api/                              # OpenAPI specification and usage examples
├── src/                                  # Main Python application source code
│   ├── core/                             # Core domain models: Resource, Tag, ProbeResult
│   ├── storage/                          # Database adapters (SQLite, PostgreSQL) and migration scripts
│   ├── probe/                            # Availability probing engines with pluggable checkers
│   ├── api/                              # RESTful route handlers, middlewares, and DTO schemas
│   ├── worker/                           # Background task queue (Celery-based) for periodic probes
│   ├── alerts/                           # Alerting backends: email, Slack, and logging
│   └── cli/                              # Command-line management commands (init_db, seed, export)
├── static/                               # Compiled frontend assets (JavaScript, CSS, images)
│   ├── dist/                             # Production-minified bundles
│   └── src/                              # Source Vue.js components and SCSS stylesheets
├── tests/                                # Automated test suites
│   ├── unit/                             # Isolated unit tests with mocked external dependencies
│   ├── integration/                      # Integration tests with real database and probe targets
│   └── e2e/                              # End-to-end tests using Playwright against a live server
├── scripts/                              # Utility scripts for development and CI/CD automation
│   ├── bump-version.sh                   # Semantic version incrementing script
│   ├── generate-snapshot.sh              # Creates a signed snapshot of the current index state
│   └── validate-resources.py             # Offline validator for resource URL syntax and metadata
├── config/                               # Environment-specific configuration files
│   ├── development.yaml                  # Debug mode, SQLite, and verbose logging
│   ├── staging.yaml                      # Pre-production settings with PostgreSQL and reduced concurrency
│   └── production.yaml                   # Optimized settings with Redis caching and alerting enabled
├── requirements.txt                      # Production Python package dependencies
├── requirements-dev.txt                  # Additional dependencies for testing and documentation builds
├── manage.py                             # Primary CLI entry point for all administrative commands
├── README.md                             # This document
├── LICENSE                               # MIT License text
└── .gitignore                            # Ignored paths for version control
```

## 贡献指南

Contributions to Jiebao Tech Index are welcome from all skill levels. The project maintains a code of conduct and a commitment to respectful, inclusive collaboration. All contributions must be accompanied by a signed Developer Certificate of Origin.

1. **Fork and Clone** – Fork the repository on GitHub, then clone your fork locally. Create a new branch with a descriptive name that follows the pattern `feature/` or `fix/` followed by the issue number, for example `feature/192-add-http2-probe`.

2. **Set Up Development Environment** – Follow the Quick Start instructions to install dependencies. Additionally, install the development extras using `pip install -r requirements-dev.txt`. Pre-commit hooks are configured to enforce code style using Black, isort, and flake8 – run `pre-commit install` to enable them automatically.

3. **Implement Changes with Tests** – Write your code changes, ensuring that all new functionality is covered by unit tests and, where applicable, integration tests. Update the relevant documentation in the `docs/` directory. Ensure that `pytest` passes with 100% test coverage for modified code.

4. **Run the Full Validation Suite** – Before submitting, execute `./scripts/validate-resources.py` to ensure that no indexed resources have been inadvertently broken. Run `python manage.py check` to validate all configuration files and database schema consistency.

5. **Submit a Pull Request** – Push your branch to your fork and open a pull request against the main repository's `develop` branch. The pull request description must include a summary of changes, a reference to any related issues, and a checklist confirming that tests pass and documentation has been updated. The maintainers will review the submission within five business days.

## 常见问题

**Q: The availability probe shows a resource as "unreachable" but I can access it from my browser. Why is there a discrepancy?**

A: The probe engine performs requests from a server-side environment that may have different network routing, DNS resolution, or firewall rules compared to your local machine. Common causes include: the target server blocking requests that lack a specific User-Agent header (the probe uses `JiebaoProbe/1.0` by default); the target requiring a specific IP allowlist that does not include your server's outbound IP; or the target responding with a non-200 status code for HEAD requests but accepting GET requests. You can configure the probe to use GET instead of HEAD, and you can customize the User-Agent and additional request headers via the admin panel on a per-resource basis.

**Q: How do I migrate from SQLite to PostgreSQL without losing existing resource entries and probe history?**

A: The project includes a dedicated migration command `python manage.py migrate_db --target postgresql --connection "postgresql://user:pass@host:5432/dbname"`. This command performs a schema introspection on the target database, creates the required tables, and then streams all data from the SQLite source using batched INSERT operations. The migration preserves all foreign key relationships and timestamps. After migration, update the `DB_URL` environment variable in your configuration file to point to the new PostgreSQL endpoint and restart the service. The original SQLite file is not modified, so you can revert by simply switching back to the old configuration.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
