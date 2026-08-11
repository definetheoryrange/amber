# NovaLink 技术资源聚合导航

NovaLink 是一个面向全球开发者的高性能技术资源聚合与导航系统，定位于为技术团队、独立开发者及研究机构提供结构化、可检索的外部工具链与数据源索引服务。本项目不托管任何第三方内容，仅作为互联网公开技术资源的逻辑映射层，通过严格的链接审核与分类机制，帮助用户快速定位高价值技术站点，减少信息检索损耗。

目标用户包括：需要持续跟踪前沿技术动态的架构师、从事多语言环境开发的运维工程师、以及需要批量接入外部数据接口的自动化脚本开发者。NovaLink 通过统一的导航入口、标签体系与可用性检测，解决技术收藏夹混乱、链接失效、类别交叉等长期痛点，每日自动执行链接可达性校验并生成健康报告。

## 功能概览

- 多维度资源分类体系：按技术领域、应用层级、数据格式等维度对每个外部链接打标，支持组合筛选与快速定位。
- 实时可用性拨测引擎：对收录的每一个外部资源自动执行周期性 HTTP/HTTPS 探活，超时或状态码异常时触发告警并记录历史可用率。
- 自定义标签与收藏集：用户可基于项目需求为资源添加私有标签，构建个人化的技术栈集合，并支持导入/导出为 JSON 格式。
- 全文与元数据检索：支持对资源标题、描述、标签、来源国别、更新频率等字段进行组合检索，检索结果按相关度与可用性加权排序。
- 访问统计与热度排序：记录每个外部链接的点击次数与最近访问时间，提供按周、月、季度的热度排行榜，辅助发现社区关注趋势。
- 健康状态可视化仪表板：以图表形式展示全部资源的总体可用率、响应时间分布、异常类型分布，支持一键导出报告用于团队同步。
- 开放 API 与 Webhook 集成：提供 RESTful API 允许第三方系统查询资源列表与状态，并支持配置异常告警的 Webhook 回调地址。

## 应用场景

场景一：技术团队内部知识库建设。团队负责人可使用 NovaLink 聚合日常开发依赖的文档站点、API 参考、公共镜像源等外部链接，统一分发给新成员，并利用可用性拨测功能提前发现不可用的关键资源，减少开发环境配置过程中的无效等待时间。

场景二：数据采集管道的外部源管理。数据工程师可将频繁调用的公共数据接口、地理编码服务、汇率转换 API 等纳入 NovaLink 的监控范围，当任意外部源响应时间超过设定阈值时，系统自动记录异常快照，辅助判断是上游故障还是网络抖动。

场景三：开源项目 README 与文档站的动态链接库。开源维护者可将 NovaLink 作为项目文档中“相关资源”章节的后端数据源，通过 API 拉取最新的推荐链接列表，避免因单个链接变更而反复修改文档静态内容。

场景四：技术资讯聚合与每日简报生成。技术爱好者可订阅 NovaLink 中“更新频率高”类别的资源，系统每日汇总这些站点的最新动态摘要（仅依赖各站点提供的 RSS/Atom 输出），生成轻量级技术早报。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL2 环境，建议使用 Python 3.10 及以上版本。

```bash
# 步骤1：克隆项目仓库
git clone https://github.com/novalink-dev/novalink-core.git
cd novalink-core

# 步骤2：创建并激活虚拟环境（推荐）
python -m venv venv
source venv/bin/activate      # Linux/macOS
# venv\Scripts\activate       # Windows

# 步骤3：安装项目依赖
pip install -r requirements.txt

# 步骤4：初始化本地配置文件
cp .env.example .env
# 编辑 .env 文件，至少设置 SECRET_KEY 和 DATABASE_URL

# 步骤5：执行数据库迁移
python manage.py migrate

# 步骤6：启动开发服务器
python manage.py runserver --host 0.0.0.0 --port 8000
```

访问 `http://localhost:8000` 即可进入 NovaLink 导航主页。首次启动将自动创建默认管理员账户，用户名 `admin`，初始密码打印在控制台日志中，请及时修改。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.10 ~ 3.12 | 核心解释器，低于 3.10 将无法使用 match 语句与类型注解新特性 |
| PostgreSQL | 14.x 及以上 | 生产环境推荐，用于存储资源元数据、标签、访问日志与拨测历史 |
| Redis | 6.2.x 及以上 | 用于缓存高频查询结果、存储会话数据及作为 Celery 消息代理 |
| Node.js | 18.x 及以上 | 仅用于前端静态资源构建（Vite + React），运行时无需 Node |
| Nginx | 1.22.x 及以上 | 生产环境反向代理与静态文件服务，可选但强烈推荐 |
| supervisor | 4.2.x 及以上 | 用于守护 Celery Worker 与 Beat 进程，生产环境必备 |
| git | 2.30.x 及以上 | 用于版本管理与后续热更新拉取 |
| openssl | 3.x 及以上 | 用于生成密钥与 HTTPS 本地测试证书 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户入门 | /docs/quick-start.md | 如何安装、配置并第一次运行 NovaLink？默认账户密码是什么？ |
| 管理员指南 | /docs/admin-guide.md | 如何批量导入外部链接？如何配置拨测频率与告警阈值？ |
| 开发者手册 | /docs/developer-api.md | API 鉴权方式是什么？分页参数如何传递？如何自定义拨测器？ |
| 部署运维 | /docs/deployment/production.md | 使用 Docker Compose 还是 Kubernetes？日志如何轮转？ |
| 架构设计 | /docs/architecture.md | 系统模块如何划分？拨测引擎与主应用如何解耦？ |
| 故障排查 | /docs/troubleshooting.md | 拨测超时怎么办？Redis 连接池耗尽如何调整？ |

## 资源列表

以下为 NovaLink 项目当前收录的全部外部资源链接，按类别分组呈现。每个链接均经过首次可达性验证，但建议用户结合自身网络环境进行二次确认。

体育赛事数据类

<code>aodaliyazuqiuchaojiliansai.asia</code>

<code>aochaozhibogw.asia</code>

<code>aochaotuijian.asia</code>

<code>aochaosheshoubang.asia</code>

<code>aochaosaicheng.asia</code>

<code>aochaoqianzhan.asia</code>

<code>aochaojishibifen.asia</code>

## 项目结构

```
novalink-core/
├── src/                                  # 核心源代码目录
│   ├── core/                             # 项目配置与全局工具
│   │   ├── settings.py                   # 多环境配置（开发/测试/生产）
│   │   ├── celery_app.py                 # Celery 应用实例与任务调度配置
│   │   └── utils/                        # 通用辅助函数（时间处理、加密、验证码）
│   ├── resources/                        # 资源管理子模块
│   │   ├── models.py                     # Resource、Tag、Category 等 ORM 模型
│   │   ├── services/                     # 业务逻辑层（资源增删改查、标签合并）
│   │   └── validators.py                 # URL 规范化、域名黑名单校验
│   ├── probes/                           # 拨测引擎子模块
│   │   ├── checker.py                    # 多协议探测（HTTP/HTTPS/TCP）
│   │   ├── scheduler.py                  # 基于 Celery Beat 的周期任务编排
│   │   └── reporters.py                  # 可用率统计与报告生成器
│   ├── api/                              # RESTful API 路由与视图
│   │   ├── v1/                           # 版本化接口（资源列表、状态查询、标签树）
│   │   └── middlewares/                  # 限流、鉴权、请求日志中间件
│   └── web/                              # 前端渲染与静态资源服务
│       ├── templates/                    # Jinja2 模板（主页、详情页、仪表板）
│       └── static/                       # 编译后的 CSS/JS 资产（由 Vite 构建）
├── tests/                                # 单元测试与集成测试
│   ├── unit/                             # 模型、工具函数、序列化器单测
│   └── integration/                      # API 端到端测试、拨测流程模拟
├── scripts/                              # 运维与辅助脚本
│   ├── init_db.py                        # 初始化数据库表与默认标签
│   ├── import_links.py                   # 从 CSV/JSON 批量导入外部资源
│   └── health_check.py                   # 手动触发全量拨测并输出控制台报告
├── deployments/                          # 部署编排文件
│   ├── docker-compose.yml                # 全栈容器编排（PostgreSQL/Redis/Worker/Web）
│   └── kubernetes/                       # K8s 部署清单（Deployment/Service/Ingress）
├── docs/                                 # 项目文档（见“文档导航”章节）
├── .env.example                          # 环境变量模板（含数据库连接、密钥、拨测参数）
├── requirements.txt                      # Python 生产依赖清单
├── requirements-dev.txt                  # 额外开发依赖（pytest、black、mypy）
├── pyproject.toml                        # 项目元数据与构建配置（setuptools 后端）
└── README.md                             # 本文档
```

## 贡献指南

我们欢迎社区贡献，包括但不限于新增资源链接、改进拨测逻辑、优化前端界面及完善文档。请遵循以下步骤：

1. 在 GitHub 上 Fork 本仓库至个人账号，并克隆到本地开发环境。确保本地已安装所有必需的依赖组件并可通过自检测试套件。

2. 创建以 `feature/` 或 `fix/` 为前缀的功能分支，例如 `feature/add-http3-probe`。请保持分支粒度单一，避免一个分支包含多个不相关的改动。

3. 编写或修改代码时，严格遵循项目根目录下的 `.pre-commit-config.yaml` 中定义的格式化与 Lint 规则（包含 black、isort、flake8）。提交前必须执行 `pre-commit run --all-files` 确保无风格警告。

4. 为新增功能或修复缺陷编写对应的单元测试，确保测试覆盖率达到 80% 以上。所有测试用例必须通过 `pytest tests/` 命令执行，且不得依赖外部网络环境（需使用 mock）。

5. 提交 Pull Request 至主仓库的 `main` 分支，并在 PR 描述中清晰说明改动动机、实现方案以及测试结果。核心维护者将在 3 个工作日内进行 Code Review，并就具体修改点提出反馈。

## 常见问题

Q: NovaLink 是否存储或缓存外部资源的内容？

A: 否。NovaLink 仅存储外部资源的元数据（标题、描述、URL、标签、最后验证时间）以及拨测产生的状态记录（响应码、响应耗时、异常信息）。系统不会以任何形式抓取、缓存或代理外部资源的内容本体，所有链接的访问均直接重定向至原始站点。用户需自行遵守各外部站点的 robots.txt 及服务条款。

Q: 拨测引擎对目标站点的请求频率是多少？是否可能被误判为攻击？

A: 默认配置下，每个外部链接每日执行 3 次拨测（间隔 8 小时），每次拨测仅发送一个轻量级 HEAD 请求（若 HEAD 不被支持则回退为 GET 且仅读取前 2048 字节）。整体请求速率控制在每秒不超过 2 个目标，远低于普通浏览器行为。用户可在环境变量中调整 `PROBE_INTERVAL_HOURS` 与 `PROBE_TIMEOUT_SECONDS` 以进一步降低频率，或配置 `PROBE_WHITELIST_USER_AGENT` 携带自定义标识供目标站点识别。

Q: 如何升级已收录链接的元数据（如描述变更或 URL 迁移）？

A: 方式一：通过管理后台逐条编辑，修改后系统自动更新 `updated_at` 时间戳并重新触发一次即时拨测验证新 URL 的可达性。方式二：通过 API 的批量更新接口提交 JSON 格式的差异数据，系统将按 `resource_id` 进行幂等合并。对于 URL 迁移，旧地址将自动加入 `redirect_history` 字段，保留可追溯性。

## 许可证

MIT License

Copyright (c) 2026 NovaLink Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
