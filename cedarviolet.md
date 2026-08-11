# Jishibi Resource Navigator

Jishibi Resource Navigator is a specialized technical resource aggregation and navigation system designed for developers, researchers, and technical writers who need rapid access to domain-specific knowledge bases and documentation sets. The project serves as a curated entry point for technical reference materials, offering structured access to multiple specialized resource nodes.

The system addresses the common challenge of fragmented technical documentation by providing a unified navigation interface that organizes resources by topic, usage frequency, and reference relevance. It is particularly suited for teams managing large-scale technical documentation repositories, API reference libraries, and versioned specification collections.

## 功能概览

- **Multi-Source Resource Aggregation**: Consolidates references from multiple domain-specific resource nodes into a single searchable index with automatic metadata extraction.

- **Topic-Based Categorization Engine**: Automatically classifies resources into predefined technical domains using keyword matching and reference graph analysis.

- **Version-Aware Reference Linking**: Maintains version associations across resource nodes to ensure documentation consistency and deprecation tracking.

- **Rapid Lookup Interface**: Provides command-line and web-based query interfaces with fuzzy search support and auto-suggestion capabilities.

- **Resource Health Monitoring**: Periodically checks availability and response times of all registered resource nodes with alerting on degradation.

- **Export and Syndication**: Supports export of resource lists in JSON, YAML, and Markdown table formats for integration with other documentation pipelines.

- **Access Control Integration**: Offers basic IP-based and token-based access filtering for internal deployment scenarios.

## 应用场景

- **Technical Documentation Maintenance**: Documentation engineers can use the navigator to verify that all external reference links remain valid and correctly categorized across multiple product versions, reducing broken link incidents.

- **Onboarding and Training**: New team members can quickly discover the authoritative resource nodes for each technical domain through the categorized index, shortening the learning curve for large codebases.

- **API Reference Consolidation**: API maintainers can aggregate endpoint specifications from distributed service repositories into a unified reference view, simplifying cross-service integration testing.

- **Compliance and Auditing**: Compliance officers can generate periodic reports of all referenced external resources, including change history and availability metrics, for regulatory review purposes.

- **Open Source Project Documentation**: Open source maintainers can expose their resource dependency tree to contributors, clarifying which external references are required for building and testing.

## 快速开始

Clone the repository and run the setup script to start the navigator service locally.

```bash
git clone https://github.com/jishibi/navigator.git
cd navigator
pip install -r requirements.txt
python setup.py build
python -m navigator serve --port 8080 --config config/default.yaml
```

The service listens on port 8080 by default. Access the web interface at http://localhost:8080 or use the CLI tool with `navigator query --term "keyword"`.

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 或更高 | 核心运行时环境，用于执行资源抓取和索引逻辑 |
| Pip | 21.0 或更高 | Python 包管理器，用于安装依赖库 |
| SQLite | 3.35 或更高 | 本地元数据缓存和查询索引存储引擎 |
| Redis | 6.2 或更高 | 可选，用于生产环境下的分布式缓存和会话管理 |
| Node.js | 16.0 或更高 | 仅用于构建前端静态资源，运行时不需要 |
| YAML | PyYAML 6.0 | 配置文件解析库，用于读取资源配置 |
| Requests | 2.28 或更高 | HTTP 客户端库，用于资源节点健康检查 |
| Click | 8.1 或更高 | CLI 命令框架，用于构建命令行交互工具 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user-guide/ | 如何安装、配置、启动导航服务，以及如何进行基础查询操作 |
| 管理员指南 | docs/admin-guide/ | 如何添加新资源节点、调整分类规则、监控系统健康状态 |
| 开发者文档 | docs/developer-guide/ | 如何扩展分类引擎、实现自定义导出格式、参与核心代码贡献 |
| API 参考 | docs/api-reference/ | 所有 RESTful API 端点的请求格式、响应结构、错误码定义 |
| 架构设计 | docs/architecture/ | 系统模块划分、数据流图、缓存策略、扩展性设计说明 |
| 变更日志 | CHANGELOG.md | 每个版本的特性新增、修复问题、已知限制和迁移注意事项 |

## 资源列表

本导航项目聚合了以下技术参考资源节点，所有资源节点均按原始地址原样收录，未做任何协议或域名改写。

技术资源主站系列：

<code>jishibifenwanzhengban.org.cn</code>

<code>jishibifenwanzhengban.net.cn</code>

专题技术资源节点：

<code>jishibifenqiutan.org.cn</code>

<code>jishibifenleisugw.org.cn</code>

<code>jishibifenjiebaogw.org.cn</code>

<code>jishibifen500gw.org.cn</code>

综合参考门户：

<code>hupuzuqiujishibifen.org.cn</code>

## 项目结构

```
navigator/
├── src/                           # 核心源代码目录
│   ├── core/                      # 核心模块：资源索引、查询解析、缓存管理
│   │   ├── indexer.py             # 资源索引构建和增量更新逻辑
│   │   ├── query.py               # 查询解析、分词、模糊匹配算法
│   │   └── cache.py               # 本地和远程缓存策略实现
│   ├── collectors/                # 资源收集器：从各节点拉取元数据
│   │   ├── http.py                # HTTP 资源抓取和重试机制
│   │   ├── parser.py              # HTML/JSON/XML 元数据解析器
│   │   └── scheduler.py           # 周期性收集任务调度器
│   ├── web/                       # Web 界面模块
│   │   ├── app.py                 # Flask/FastAPI 应用入口和路由
│   │   ├── templates/             # Jinja2 模板文件
│   │   └── static/                # CSS、JavaScript、图标等静态资源
│   ├── cli/                       # 命令行交互模块
│   │   ├── main.py                # CLI 入口命令组定义
│   │   ├── query_cmd.py           # 查询子命令实现
│   │   └── admin_cmd.py           # 管理子命令（添加/删除资源节点）
│   └── utils/                     # 通用工具函数
│       ├── validators.py          # URL 格式验证、版本号比较工具
│       └── logger.py              # 结构化日志配置和输出工具
├── tests/                         # 单元测试和集成测试目录
│   ├── test_indexer.py            # 索引器单元测试用例
│   ├── test_query.py              # 查询引擎测试用例
│   └── fixtures/                  # 测试用的模拟数据和响应样本
├── config/                        # 配置文件目录
│   ├── default.yaml               # 默认配置：端口、缓存时长、收集间隔
│   ├── production.yaml            # 生产环境覆盖配置
│   └── resources.yaml             # 预置资源节点列表（初始导入）
├── docs/                          # 完整文档目录（参见文档导航章节）
│   ├── user-guide/                # 用户手册章节
│   ├── admin-guide/               # 管理员手册章节
│   ├── developer-guide/           # 开发者手册章节
│   └── api-reference/             # API 参考文档
├── scripts/                       # 运维辅助脚本
│   ├── init_db.py                 # 初始化 SQLite 数据库表结构
│   ├── seed_resources.py          # 从配置文件导入初始资源数据
│   └── health_check.py            # 独立健康检查脚本，可被 cron 调用
├── requirements.txt               # Python 运行时依赖清单
├── setup.py                       # 项目安装构建脚本
└── README.md                      # 本文件
```

## 贡献指南

1.  Fork 本仓库并克隆到本地开发环境。确保您的 Python 版本满足 3.9 或更高，并安装所有开发依赖（使用 `pip install -r requirements-dev.txt`）。

2.  创建新的特性分支进行开发。分支命名建议遵循 `feature/描述` 或 `fix/描述` 格式，例如 `feature/add-json-export`。

3.  编写代码时请遵循 PEP 8 编码规范，并为新增的函数和类添加清晰的 docstring 注释。所有公共 API 变更需同步更新对应的文档章节。

4.  在提交 Pull Request 之前，请确保所有现有测试用例通过，并为新增功能编写不少于 3 个单元测试用例。运行 `pytest tests/` 验证测试覆盖。

5.  提交 Pull Request 时请填写完整的变更描述模板，包括变更动机、实现方式、测试结果以及相关文档链接。等待至少一位核心维护者进行代码审阅。

## 常见问题

Q: 导航服务启动时提示 "Resource node unreachable"，但浏览器可以正常访问该地址。

A: 该错误通常由网络代理或 SSL 证书验证引起。请检查配置文件中的 `proxy` 和 `ssl_verify` 选项。如果资源节点为裸域名（无 HTTPS），请确认配置中未强制启用 SSL。您也可以尝试运行 `navigator health --node <url>` 进行独立诊断。

Q: 如何添加自定义资源节点而不修改默认配置文件？

A: 您可以在 `config/resources.yaml` 中按照 `name: url` 格式添加新节点，然后执行 `python scripts/seed_resources.py --append` 将其导入数据库而不覆盖已有数据。或者使用 CLI 命令 `navigator admin add --url <code>example.com</code> --tag custom` 动态添加。

Q: 索引缓存占用磁盘空间过大，如何清理或限制？

A: 缓存文件默认存储在 `./cache/` 目录下。您可以在配置文件中调整 `cache.max_size_mb` 和 `cache.ttl_hours` 参数。执行 `navigator admin cache --clear` 可手动清理过期缓存条目。对于生产环境，建议启用 Redis 缓存并设置 `maxmemory` 策略。

## 许可证

MIT License

Copyright (c) 2025 Jishibi Project Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:16
