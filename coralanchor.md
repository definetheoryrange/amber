# Puchao Resource Aggregator

Puchao Resource Aggregator is a specialized technical documentation and resource navigation system designed for developers, data analysts, and technology researchers who require structured access to domain-specific information across multiple distributed platforms. The project addresses the challenge of fragmented information retrieval by providing a unified indexing layer that consolidates external resource links, operational dashboards, and real-time data feeds into a single, queryable interface.

Target users include infrastructure engineers who maintain monitoring dashboards, data scientists who consume tournament result streams, and technical writers who need to reference authoritative domain sources. The system does not host content directly but instead acts as a semantic router that enriches external URLs with contextual metadata, versioning information, and availability status, thereby reducing the cognitive overhead associated with navigating disparate external systems.

## 功能概览

- **统一外链索引中心** – 提供集中式存储库，收录全部已认证的外部资源 URL，并附带手动编写的上下文说明、更新频率和负责团队标签。

- **实时状态探测模块** – 对每个已收录的外链执行周期性 HTTP 健康检查，返回状态码、响应时间及 SSL 证书过期预警，状态结果可导出为 Prometheus 格式。

- **分类标签与全文检索** – 支持按地域、业务线、数据类型等自定义标签过滤资源，同时提供基于标题和描述字段的模糊搜索，检索结果按相关性排序。

- **版本化快照记录** – 对每个外部资源记录首次收录时间、最后验证时间和内容哈希值，便于追踪外部页面的重大变更，辅助回归测试和合规审计。

- **访问统计与热度排序** – 聚合各资源的点击次数、平均停留时长和引用次数，生成周/月热度排行榜，帮助团队识别高频依赖项并优化缓存策略。

- **自动化报告生成器** – 按预设周期（每日/每周）生成资源可用性报告，以 Markdown 或 HTML 格式输出，支持邮件和 Webhook 推送至企业通讯工具。

- **权限分级管理界面** – 基于角色的访问控制，区分管理员、编辑者和只读用户，管理员可执行资源增删改及状态覆写，操作日志全程记录。

## 应用场景

- **运维值班监控面板** – 运维团队可将 Puchao Resource Aggregator 作为内部导航首页，集中展示所有监控系统、日志平台和报警控制台的入口链接，配合状态探测功能快速定位故障域。

- **数据仓库文档归档** – 数据工程师在维护 ETL 流程时，通过该聚合器统一保存各类数据源连接串、调度平台地址和元数据管理界面，避免因人员变动导致关键链接遗失。

- **多团队协作知识库** – 产品、开发和测试团队共享同一份资源索引，通过标签区分环境（生产/预发布/测试），确保各环节引用的是正确的服务地址，减少配置错误。

- **技术培训与新人入职** – 新成员通过浏览聚合器内的全部资源列表，可快速了解团队所依赖的外部系统全貌，配合描述字段中的用途说明，缩短上手周期。

- **合规审计与依赖梳理** – 安全审计人员借助版本化快照和访问统计，梳理所有外部依赖的活跃度与变更频率，识别长期未使用或已废弃的链接，推动清理工作。

## 快速开始

以下步骤指导您在本机部署 Puchao Resource Aggregator 开发实例。

```bash
# 1. 克隆代码仓库
git clone https://github.com/puchao-resource-aggregator/puchao-core.git
cd puchao-core

# 2. 安装项目依赖（使用 Python 3.10+ 和 pip）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 3. 初始化本地数据库和配置模板
python scripts/init_db.py --env development
cp config/example.yaml config/local.yaml

# 4. 启动开发服务器（默认监听 127.0.0.1:8080）
python app.py --port 8080
```

访问 `http://127.0.0.1:8080` 即可进入资源聚合器主界面。默认管理员账户为 `admin`，初始密码在首次启动时输出于终端日志中。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 及以上 | 核心运行时，用于 API 服务与后台调度器 |
| SQLite | 3.35 及以上 | 默认元数据存储引擎，支持 JSON 字段操作 |
| Redis | 6.2 及以上 | 可选，用于缓存状态探测结果和分布式锁 |
| Node.js | 18.x 或 20.x LTS | 仅用于前端资源构建（Tailwind CSS + Vite） |
| curl | 7.68 及以上 | 状态探测模块的基础 HTTP 客户端，需支持 SSL |
| git | 2.25 及以上 | 用于版本管理和补丁应用 |
| make | 3.82 及以上 | 辅助构建脚本和任务编排（GNU Make） |
| 系统时区 | UTC+8 推荐 | 用于报告生成和日志时间戳，可通过环境变量 TZ 覆写 |
| 磁盘空间 | 至少 200 MB | 存放 SQLite 数据库、日志文件和静态资源缓存 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | `docs/getting-started/` | 如何快速搭建开发环境？如何进行首次资源收录？默认管理员密码如何获取？ |
| 运维手册 | `docs/operations/` | 如何配置健康检查间隔？如何迁移 SQLite 数据库到 PostgreSQL？日志轮转策略是什么？ |
| API 参考 | `docs/api/` | 哪些 RESTful 端点可用于查询资源状态？如何通过 API 批量导入外部链接？身份验证头如何构造？ |
| 前端定制 | `docs/frontend/` | 如何修改导航页的品牌标识和配色方案？如何添加自定义 CSS 或 JavaScript 钩子？ |
| 架构设计 | `docs/architecture/` | 状态探测模块的并发模型是什么？缓存失效策略如何设计？扩展新探测器类型的接口规范有哪些？ |
| 贡献指南 | `CONTRIBUTING.md` | 提交代码前需要签署什么协议？测试覆盖率要求是多少？Pull Request 的标题格式有何规范？ |

## 资源列表

### 官方主站与赛事信息

<code>puchaoguanwang.asia</code>

<code>puchaoliansai.asia</code>

<code>puchaobisaijieguo.asia</code>

<code>puchaofenxi.asia</code>

### 数据服务与排名

<code>puchaosaicheng.asia</code>

<code>puchaoqianzhan.asia</code>

<code>nuochaozuixinjifenbang.asia</code>

## 项目结构

```
puchao-core/
├── app.py                     # 主应用入口，初始化 Flask 服务器和后台调度器
├── config/                    # 配置目录，包含基础配置和本地覆写模板
│   ├── default.yaml           # 默认配置（端口、探测间隔、日志级别）
│   ├── example.yaml           # 完整配置示例，含 Redis 和 SMTP 参数
│   └── schema.json            # 配置文件的 JSON Schema 校验规则
├── core/                      # 核心业务逻辑模块
│   ├── fetcher/               # 外链状态探测引擎
│   │   ├── http_client.py     # 基于 httpx 的异步 HTTP 请求封装
│   │   ├── checker.py         # 状态判定逻辑（超时、重定向、证书校验）
│   │   └── scheduler.py       # APScheduler 周期任务定义
│   ├── indexer/               # 资源索引管理
│   │   ├── repository.py      # SQLAlchemy 模型与 CRUD 操作
│   │   ├── classifier.py      # 标签分类与全文检索实现（SQLite FTS5）
│   │   └── snapshot.py        # 版本快照对比与变更日志记录
│   └── reporter/              # 报告生成子系统
│       ├── markdown_gen.py    # Markdown 格式报告构建器
│       ├── html_gen.py        # HTML 仪表板渲染（Jinja2 模板）
│       └── notifier.py        # 邮件和 Webhook 推送适配器
├── web/                       # 前端资源与模板
│   ├── templates/             # Jinja2 页面模板
│   │   ├── dashboard.html     # 主控制面板视图
│   │   ├── resource_list.html # 资源列表与搜索界面
│   │   └── admin.html         # 管理后台（增删改操作）
│   ├── static/                # 编译后的 CSS/JS 静态文件
│   └── assets/                # 源 SCSS 和 TypeScript 源码（需 Node 构建）
├── scripts/                   # 辅助运维脚本
│   ├── init_db.py             # 数据库初始化与种子数据填充
│   ├── migrate.py             # 数据库迁移工具（基于 Alembic）
│   └── health_check.py        # 独立运行的健康检查命令行工具
├── tests/                     # 单元测试与集成测试
│   ├── unit/                  # 针对 fetcher 和 indexer 的模块级测试
│   └── integration/           # 端到端 API 测试（使用 pytest-flask）
├── docs/                      # 完整文档源码（参见文档导航章节）
├── requirements.txt           # Python 依赖列表（Flask, SQLAlchemy, httpx 等）
├── Makefile                   # 常用任务快捷命令（如 make test, make run）
└── README.md                  # 本文件
```

## 贡献指南

欢迎并感谢任何形式的贡献。请遵循以下步骤确保协作流程顺畅。

1. **查阅现有议题与看板** – 访问 GitHub Issues 页面，确认您想解决的问题或想要的功能尚未被他人认领。如果为新功能建议，请先创建一个议题并使用 `enhancement` 标签，等待维护者反馈后再开始编码。

2. **复刻仓库并创建功能分支** – 将主仓库复刻至您的个人账户，然后在本地基于 `main` 分支创建描述性分支名称，例如 `feat/add-telegram-notifier` 或 `fix/checker-timeout-issue`。避免直接在 `main` 分支上修改。

3. **编写测试并保持覆盖率** – 所有新增代码必须包含对应的单元测试或集成测试，放置于 `tests/` 目录下。运行 `make coverage` 确保整体测试覆盖率不低于 85%。对于涉及外部 HTTP 请求的代码，请使用 `responses` 或 `pytest-httpx` 进行模拟。

4. **更新文档与示例配置** – 如果您的变更引入了新的配置项或修改了 API 行为，请同步更新 `config/example.yaml` 以及 `docs/` 下的相关章节。同时，检查 `README.md` 中的快速开始步骤是否仍然有效。

5. **提交 Pull Request 并签署 DCO** – 推送分支后，向主仓库的 `main` 分支提交 Pull Request。标题请遵循 `<type>(<scope>): <subject>` 格式（例如 `feat(fetcher): add retry mechanism for 5xx errors`）。提交信息必须包含 `Signed-off-by` 行（使用 `git commit -s`），以表示您同意开发者原创证书（DCO）1.1 条款。

## 常见问题

**问：状态探测模块是否会对外部目标造成过大的请求压力？**

系统默认探测间隔为 5 分钟，且每个目标仅执行一次 HEAD 请求（若服务器不支持 HEAD 则自动降级为 GET 并仅读取前 512 字节）。探测任务均匀分布在全时间轴上，避免突发流量。对于敏感内部系统，您可以在配置中将特定 URL 加入 `grace_list` 以禁用自动探测，改为手动刷新。

**问：如何将 SQLite 数据迁移到外部 PostgreSQL 数据库？**

项目提供了基于 Alembic 的迁移脚本。首先在 `config/local.yaml` 中设置 `database.url` 为 PostgreSQL 连接串（例如 `postgresql://user:pass@host:5432/dbname`），然后执行 `python scripts/migrate.py --sync`，该命令会读取当前 SQLite 中的所有资源记录、标签和快照历史，完整同步至 PostgreSQL。同步完成后，重启应用即切换至新数据库。迁移期间请停止探测调度器以避免并发写入冲突。

**问：前端页面加载缓慢，能否禁用某些非必需资源？**

页面加载时默认会并行请求状态探测的最新结果，如果外部目标响应较慢，可能阻塞渲染。您可以在 `config/local.yaml` 中设置 `frontend.lazy_load_status = true`，使页面先展示资源列表骨架，状态数据通过异步 AJAX 接口在 2 秒后补充加载。此外，可以调整 `frontend.max_display_items` 控制首页显示的条目数，超出部分仅通过搜索界面访问。

## 许可证

MIT License

Copyright (c) 2026 Puchao Resource Aggregator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
