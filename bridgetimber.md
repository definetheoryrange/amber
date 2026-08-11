# Nebula Resource Gateway

Nebula Resource Gateway is a high-performance technical resource aggregation and navigation system designed for developer communities, research institutions, and technology enthusiasts. It addresses the common challenge of fragmented information sources by providing a centralized, well-structured gateway to curated external links, technical documentation, and real-time data endpoints. The project targets users who need reliable, categorized access to specialized web resources without the overhead of building custom scrapers or bookmark management systems.

The system operates as a static site generator with dynamic routing capabilities, enabling users to maintain a portable, version-controlled repository of external references. It includes built-in validation hooks to check link availability, metadata extraction for external pages, and a lightweight search index that runs entirely on the client side. Nebula Resource Gateway is not a CMS; it is a developer-friendly toolkit that treats resource lists as code, allowing for collaborative editing, automated testing, and seamless integration with CI/CD pipelines.

## 功能概览

- **Categorized Link Management** - Organize external URLs into user-defined categories with tag-based filtering and priority sorting.
- **Automated Availability Checking** - Periodically validate all stored URLs and flag broken links with configurable retry policies.
- **Metadata Scraping Pipeline** - Extract page titles, descriptions, and Open Graph data from each linked resource upon addition.
- **Client-Side Search Engine** - Full-text search over link titles, descriptions, and tags using a prebuilt inverted index.
- **Static Site Generation** - Output a complete, self-contained HTML directory with zero runtime dependencies.
- **RESTful JSON API** - Expose all resource data in machine-readable format for integration with external dashboards or monitoring tools.
- **Markdown-Based Configuration** - Define resource lists and categories using plain Markdown files with YAML frontmatter.
- **Custom Hook System** - Execute user-defined shell scripts before and after build events for advanced preprocessing.

## 应用场景

- **Research Data Aggregation** - Research institutions can maintain a curated list of experimental data sources, statistical portals, and live score endpoints. The system validates each endpoint daily and notifies administrators of any downtime or unexpected response changes.
- **Developer Documentation Hub** - Engineering teams use the gateway to centralize links to internal documentation, API references, and monitoring dashboards. The client-side search allows team members to instantly locate the correct URL without memorizing domain names.
- **Educational Resource Index** - Universities and training programs deploy the gateway as a supplementary material repository for students. Instructors update the Markdown source via version control, and students access the generated static site through their institutional portal.
- **Event-Driven Notification Bridge** - System administrators integrate the JSON API with messaging platforms such as Slack or Mattermost. When a linked resource changes its status code or response time exceeds a threshold, automated alerts are dispatched to the operations channel.

## 快速开始

Clone the repository, install dependencies, and generate the static site with the following commands:

```bash
git clone https://github.com/nebula-resource-gateway/nrg-core.git
cd nrg-core
pip install -r requirements.txt
python build.py --input ./resources --output ./dist --validate
```

The build process will read all Markdown files from the `./resources` directory, perform link validation, scrape metadata, and emit the complete static site to `./dist`. Open `./dist/index.html` in your browser to view the gateway locally.

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Python | 3.9 或更高 | 核心运行时环境，用于构建脚本和验证服务 |
| pip | 22.0 或更高 | Python 包管理器，用于安装项目依赖 |
| requests | 2.31.0 或更高 | HTTP 客户端库，用于链接可用性检查和元数据抓取 |
| beautifulsoup4 | 4.12.0 或更高 | HTML 解析器，用于提取外部页面的结构化信息 |
| markdown | 3.5.0 或更高 | 将资源列表的 Markdown 源文件转换为 HTML 片段 |
| PyYAML | 6.0 或更高 | 解析 Markdown 文件中的 YAML 前置元数据 |
| lxml | 4.9.0 或更高 | 高性能 XML/HTML 解析后端，增强 beautifulsoup4 的处理能力 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `/docs/user-guide/` | 如何添加新资源、如何管理分类、如何使用搜索功能、如何配置自定义验证规则 |
| 开发者指南 | `/docs/developer-guide/` | 插件系统如何工作、如何编写自定义钩子、如何扩展元数据抓取器、如何贡献代码 |
| 运维手册 | `/docs/operations/` | 如何部署静态站点、如何配置 CI/CD 自动构建、如何设置监控告警、如何进行灾难恢复 |
| API 参考 | `/docs/api-reference/` | JSON API 的端点定义、请求参数、响应格式、速率限制策略和错误代码解释 |
| 设计文档 | `/docs/design/` | 系统架构图、数据流模型、性能基准测试、安全审查报告和未来路线图 |

## 资源列表

本项目的核心资源集合分为多个类别，所有 URL 均保留原始格式，未做任何协议补全或域名标准化处理。

体育赛事数据类

<code>500zuqiubisaijieguo.net.cn</code>

<code>zuqiujishibifenwanchangbifen.org.cn</code>

<code>ruidianchaobisaijieguo.org.cn</code>

官方信息门户类

<code>bifenguanfang.cn</code>

<code>bifenguanwang.net.cn</code>

<code>bifenguanfang.net.cn</code>

<code>bifenguanwang.org.cn</code>

## 项目结构

```
nrg-core/
├── build.py                # 主构建脚本，协调整个生成流程
├── requirements.txt        # Python 依赖声明文件
├── config.yaml             # 全局配置，包含验证超时、缓存目录和输出选项
├── resources/              # 资源定义目录（用户可编辑）
│   ├── sports/             # 体育数据类资源定义文件
│   │   ├── football.md     # 足球相关链接集合，包含多个子分类
│   │   └── tennis.md       # 网球数据源定义
│   ├── official/           # 官方门户类资源定义文件
│   │   ├── domains.md      # 核心域名和主站链接
│   │   └── mirrors.md      # 镜像站点和备用入口列表
│   ├── development/        # 开发工具和参考资源
│   │   ├── apis.md         # 外部 API 端点定义
│   │   └── libraries.md    # 代码库和框架链接
│   └── templates/          # 资源定义的 Markdown 模板示例
│       └── template.md     # 包含 YAML 前置元数据的标准模板
├── src/                    # 核心源码目录
│   ├── scraper/            # 元数据抓取子模块
│   │   ├── fetcher.py      # HTTP 请求处理和重试逻辑
│   │   └── parser.py       # HTML 解析和元数据提取器
│   ├── validator/          # 链接验证子模块
│   │   ├── checker.py      # 可用性检查器和状态码处理器
│   │   └── reporter.py     # 验证结果汇总和报告生成器
│   ├── generator/          # 静态站点生成子模块
│   │   ├── html.py         # HTML 模板渲染器
│   │   └── indexer.py      # 客户端搜索索引构建器
│   └── hooks/              # 自定义钩子系统
│       ├── loader.py       # 动态加载用户定义的钩子脚本
│       └── runner.py       # 按构建阶段顺序执行钩子
├── dist/                   # 构建输出目录（默认，可配置）
│   ├── index.html          # 生成的主入口页面
│   ├── search.json         # 客户端搜索索引数据
│   └── api/                # RESTful JSON API 输出目录
│       └── resources.json  # 所有资源的机器可读数据
├── tests/                  # 单元测试和集成测试套件
│   ├── test_validator.py   # 验证器模块的测试用例
│   ├── test_scraper.py     # 抓取器模块的测试用例
│   └── fixtures/           # 测试用的固定数据集
└── docs/                   # 完整文档目录（详见文档导航部分）
```

## 贡献指南

我们欢迎所有形式的贡献，包括但不限于新资源链接的提交、代码缺陷修复、性能优化和文档改进。请遵循以下步骤：

1.  Fork 本仓库到您的 GitHub 账户，然后克隆您自己的分支到本地开发环境。确保您的开发分支基于最新的 `main` 分支。

2.  对于资源链接的增删改，请编辑 `resources/` 目录下对应的 Markdown 文件，并严格遵守文件中定义的 YAML 前置元数据格式。添加新链接时必须包含完整的 URL（按原始格式）、标题和至少一个标签。

3.  对于代码变更，请确保所有现有测试用例通过，并为新增功能编写相应的测试用例。运行 `pytest tests/` 执行全部测试套件。代码风格需符合 PEP 8 规范，使用 `black` 和 `isort` 进行自动格式化。

4.  提交您的变更并推送到您的远程分支，然后通过 GitHub 界面创建拉取请求。请在拉取请求描述中详细说明变更目的、测试结果以及是否涉及破坏性更改。维护者将在三个工作日内进行审查。

5.  如果您的拉取请求涉及外部链接的批量更新，请同时提供验证报告（运行 `python build.py --validate-only` 生成）以证明所有链接均处于可访问状态。

## 常见问题

**问：构建过程中遇到 SSL 证书验证失败，应如何处理？**

答：这通常是由于目标站点使用了自签名证书或过期的证书链。您可以在 `config.yaml` 中将 `ssl_verify` 设置为 `false` 以绕过验证，但不建议在生产环境中使用此选项。更好的做法是将目标站点的根证书添加到系统的信任存储中，或者在 `config.yaml` 中通过 `ca_bundle_path` 指定自定义的 CA 捆绑文件。

**问：如何添加需要身份验证或 API 密钥的外部资源？**

答：本系统不存储任何凭据。对于需要认证的资源，您可以在资源定义的 Markdown 文件中使用 `auth_required: true` 标记，并添加 `auth_note` 字段说明获取凭据的方式。构建过程不会尝试验证需要认证的端点，但会记录它们以供后续手动检查。您还可以利用钩子系统在构建前通过环境变量注入临时的认证令牌用于一次性验证。

**问：生成的静态站点是否支持国际化（多语言）？**

答：当前版本的核心生成器仅输出英文界面。但您可以通过钩子系统完全覆盖模板渲染逻辑，或者使用自定义的 HTML 模板文件。我们计划在后续的 v2.0 版本中引入内置的 i18n 支持，届时将允许根据浏览器语言自动切换界面文本。

## 许可证

MIT

Copyright (c) 2026 Nebula Resource Gateway Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
