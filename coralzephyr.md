# AISP - Ajia Intelligence Score Platform

AISP is a comprehensive technical resource aggregation and intelligence scoring platform designed for data analysts, quantitative researchers, and technical decision-makers. The platform serves as a centralized external link repository that systematically organizes, categorizes, and evaluates domain-specific information sources across multiple technical and analytical disciplines. Target users include technical leads, research engineers, product managers conducting competitive analysis, and developers building data-driven applications who require structured access to high-signal external resources without the overhead of manual curation. AISP solves the problem of fragmented information discovery by providing a maintainable, version-controlled, and queryable index of curated external references, each accompanied by contextual metadata, relevance scoring, and usage annotations that enable rapid assessment of resource applicability to specific technical challenges.

## 功能概览

- **智能资源索引与分类** - 基于动态标签系统与多维度属性标注，自动将外部链接归类至对应的技术领域、数据主题或分析框架，支持按学科、时效性、可信度进行过滤检索。

- **外链可用性监控** - 内置周期性健康检查管道，自动探测每个已收录资源的响应状态、重定向链变化与SSL证书有效期，标记异常状态并生成告警通知。

- **使用频次统计与热度分析** - 记录每个资源在团队内部的点击次数、引用频次以及外部反向链接数量，输出趋势图表辅助判断信息源的长期价值。

- **自定义评分规则引擎** - 允许用户根据自身业务需求配置评分维度权重，包括信息密度、更新频率、访问延迟、内容深度等指标，生成个性化资源推荐排序。

- **标记与备注协作系统** - 支持团队成员为每个资源添加私有或公开的技术备注、使用心得、代码示例片段以及版本兼容性说明，形成团队内部的集体知识沉淀。

- **批量导入与导出接口** - 提供RESTful API与命令行工具，支持从JSON、CSV、OPML等格式批量导入现有书签集合，并支持按筛选条件导出为结构化数据集供下游分析工具使用。

- **变更历史审计日志** - 记录所有资源条目的增删改操作、字段变更前后对比以及操作人信息，满足企业级合规审计要求，支持时间点回溯。

## 应用场景

- **技术选型调研** - 技术负责人需要对比多个中间件或数据库方案时，通过AISP快速检索已收录的官方文档站、性能对比报告、第三方评测博客以及社区讨论串，一站式获取多视角信息，大幅缩短调研周期。

- **竞品动态追踪** - 产品与策略团队设定关注的一组竞品官方公告栏、版本发布日志、社交媒体技术账号以及用户反馈论坛，AISP定期检测这些资源的更新状态并推送变更摘要，使团队能够及时捕捉市场动向。

- **数据源质量评估** - 数据工程团队在接入第三方数据API或公开数据集之前，利用AISP中收录的历史服务可用性记录、响应时间百分位数以及社区反馈评分，对候选数据源进行量化评估，降低生产环境集成风险。

- **新人入职知识导览** - 新加入的开发人员通过AISP的体系化资源分类树快速了解团队常用的技术栈文档、内部工具链地址、云服务控制台入口以及关键业务指标看板，缩短上手适应时间并减少反复询问。

- **技术文章撰写参考** - 技术博主或开源项目维护者撰写技术方案对比文章时，使用AISP的标签筛选与评分排序功能高效定位权威参考资料，确保引用来源的准确性与时效性。

## 快速开始

以下步骤指导您在本地环境快速启动AISP开发实例，完成源码获取、依赖安装与服务运行。

```bash
# 步骤1: 克隆代码仓库
git clone https://github.com/aisp/aisp-core.git
cd aisp-core

# 步骤2: 安装项目依赖（使用pipenv管理Python依赖）
pip install --upgrade pip
pip install pipenv
pipenv install --dev

# 步骤3: 初始化配置文件与环境变量
cp .env.example .env
cp config/application.yml.example config/application.yml

# 步骤4: 执行数据库迁移与初始数据加载
pipenv run flask db upgrade
pipenv run flask seed load --type=resources

# 步骤5: 启动开发服务器
pipenv run flask run --host=0.0.0.0 --port=8080
```

服务启动后，访问 `http://localhost:8080` 进入AISP仪表板。默认管理员账户为 `admin@aisp.local`，初始密码在首次启动时输出至控制台日志。

## 安装要求

AISP基于Python 3.10+构建，依赖PostgreSQL作为主数据存储，Redis用于缓存与任务队列。生产环境部署建议额外配置Nginx反向代理与系统级进程守护。以下为完整依赖清单：

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10.x 或 3.11.x | 核心运行时，低于3.10将导致类型注解解析错误 |
| PostgreSQL | 14.x 或 15.x | 主数据库，存储资源条目、评分记录、审计日志与用户信息 |
| Redis | 6.2.x 或 7.0.x | 缓存会话、速率限制计数器及Celery任务代理 |
| Node.js | 18.x LTS | 仅前端资源构建时必需，后端运行不依赖 |
| npm | 9.x 或 10.x | 配合Node.js用于打包静态资产 |
| Docker | 24.x 或更高 | 可选，用于容器化部署与依赖服务快速启动 |
| OpenSSL | 3.0.x | 用于生成API密钥与签名验证，系统一般预装 |

数据库连接字符串、缓存端点、密钥种子等敏感配置需通过环境变量或密钥管理服务注入，禁止硬编码于代码或配置文件中。

## 文档导航

AISP项目文档体系按照读者角色与使用目的划分为四个层面，涵盖从入门操作到二次开发的完整指引。下表展示了各文档模块的定位与覆盖问题范围：

| 层面 | 目录位置 | 回答的问题 |
|------|---------|-----------|
| 用户手册 | `/docs/user-guide/` | 如何注册账号、创建资源条目、配置评分规则、查看统计仪表板、管理团队协作权限 |
| 运维手册 | `/docs/ops-guide/` | 如何部署生产环境、配置SSL证书、设置自动备份策略、扩展工作节点、故障排查清单 |
| 开发指南 | `/docs/dev-guide/` | 如何扩展自定义评分器、添加新的资源源适配器、调试异步任务、编写单元测试、提交代码变更 |
| API参考 | `/docs/api-reference/` | 如何通过REST API增删改查资源、批量提交更新、获取健康状态、管理API密钥与速率限制 |

除上述核心文档外，`/docs/architecture/` 目录存放系统设计决策记录、数据模型ER图以及外部依赖风险评估报告，供架构评审参考。

## 资源列表

本节按照功能类别对AISP项目收录的外部资源进行分组整理。所有条目均来源于用户提供的原始数据，内容未经修改且按原样呈现。

### 综合推荐类

- <code>ajiatuijian.asia</code>

### 数据统计与分析类

- <code>ajiasheshoubang.asia</code>
- <code>ajiafenxi.asia</code>
- <code>ajiajishibifen.asia</code>
- <code>ajiajifenbang.asia</code>

### 赛事与排名信息类

- <code>ajiaqianzhan.asia</code>
- <code>ajialiansai.asia</code>

## 项目结构

AISP遵循分层架构设计，源代码按功能域组织为独立的子模块。以下为项目根目录的完整树形结构，附带每个目录的职责说明：

```
aisp-core/
├── aisp/                                   # 主应用包
│   ├── api/                                # RESTful API端点实现（路由、请求验证、响应序列化）
│   │   ├── v1/                             # API版本1命名空间
│   │   │   ├── resources.py                # 资源CRUD与搜索端点
│   │   │   ├── scores.py                   # 评分计算与排行端点
│   │   │   └── health.py                   # 存活探针与就绪探针
│   │   └── middleware/                     # 认证、日志、速率限制中间件
│   ├── core/                               # 核心业务逻辑层
│   │   ├── models/                         # SQLAlchemy数据模型定义（Resource, Tag, AuditLog等）
│   │   ├── services/                       # 业务服务类（评分引擎、资源索引器、监控调度器）
│   │   ├── scoring/                        # 评分策略实现（贝叶斯平均、时间衰减、威尔逊区间）
│   │   └── validators/                     # 自定义校验器（URL规范化、SSL证书预检）
│   ├── tasks/                              # Celery异步任务定义
│   │   ├── health_check.py                 # 周期性资源可用性探测任务
│   │   ├── stats_aggregator.py             # 每日统计汇总任务
│   │   └── notification.py                 # 告警与报告邮件发送任务
│   ├── extensions/                         # Flask扩展初始化与配置
│   │   ├── database.py                     # SQLAlchemy实例
│   │   ├── cache.py                        # Redis缓存客户端
│   │   └── celery.py                       # Celery应用实例
│   └── utils/                              # 通用工具函数
│       ├── network.py                      # 网络请求重试、超时控制、代理配置
│       ├── crypto.py                       # 签名生成、哈希计算、令牌编解码
│       └── logging.py                      # 结构化日志配置与上下文注入
├── frontend/                               # 前端单页应用源码（React + TypeScript）
│   ├── src/
│   │   ├── components/                     # 可复用UI组件（表格、图表、筛选器面板）
│   │   ├── pages/                          # 路由页面（仪表板、资源列表、详情、设置）
│   │   ├── hooks/                          # 自定义React钩子（数据获取、分页、表单状态）
│   │   └── services/                       # API客户端封装与类型定义
│   └── public/                             # 静态资源入口文件与favicon
├── config/                                 # 环境配置文件目录
│   ├── application.yml                     # 主配置文件（含默认参数与feature flags）
│   ├── application.dev.yml                 # 开发环境覆盖配置
│   └── application.prod.yml                # 生产环境覆盖配置（敏感信息从环境变量读取）
├── migrations/                             # 数据库迁移脚本（Alembic自动生成）
│   ├── versions/                           # 各迁移版本文件
│   └── alembic.ini                         # 迁移工具配置文件
├── tests/                                  # 单元测试与集成测试
│   ├── unit/                               # 纯逻辑单元测试（无外部依赖）
│   ├── integration/                        # 数据库与API集成测试
│   └── fixtures/                           # 测试数据夹具与mock对象
├── scripts/                                # 运维与开发辅助脚本
│   ├── seed_data.py                        # 初始资源种子数据加载脚本
│   ├── backup_db.sh                        # 数据库备份脚本（配合cron使用）
│   └── generate_api_key.py                 # API密钥生成命令行工具
├── docs/                                   # 文档源码（Markdown + Sphinx扩展）
├── docker-compose.yml                      # 本地开发环境服务编排定义
├── Dockerfile                              # 生产镜像构建定义（多阶段构建）
├── Makefile                                # 常用任务快捷命令（install, test, lint, run）
├── pyproject.toml                          # 项目元数据与依赖声明（PEP 621）
└── README.md                               # 项目入口文档（本文件）
```

## 贡献指南

AISP项目欢迎各类形式的贡献，包括但不限于新增资源条目、改进评分算法、修复缺陷、完善文档以及提出功能建议。请遵循以下流程确保协作顺畅：

1. 查阅问题追踪器与路线图 - 访问GitHub Issues页面查看现有任务列表，优先认领标注为`help wanted`或`good first issue`的工作项，避免重复劳动。新功能提议应先创建讨论议题，经维护者初步确认后再着手实现。

2. 分叉仓库并创建特性分支 - 将主仓库分叉至个人账户下，基于`develop`分支创建新分支，命名格式为`feature/简要描述`或`fix/问题编号`。分支名称使用连字符分隔的英文小写单词，例如`feature/resource-tag-autocomplete`。

3. 编写代码与测试用例 - 所有新增功能必须附带对应的单元测试或集成测试，覆盖率不低于现有基线。代码风格遵循PEP 8与项目配置的Black格式化规范，提交前运行`make lint`与`make test`确保本地检查全部通过。

4. 提交变更说明与签署开发者原产地证书 - 提交信息采用约定式提交格式（`type: subject`），正文详细描述修改动机与实现方案。所有外部贡献者需在提交信息末尾添加`Signed-off-by`行，表明您同意开发者原产地证书条款。

5. 发起合并请求至开发分支 - 推送特性分支至分叉仓库后，向主仓库的`develop`分支发起合并请求。请求描述中需关联相关议题编号，并勾选自测清单。维护者将在三个工作日内进行代码审查，可能提出修改意见或进一步讨论。

## 常见问题

**问：AISP是否支持私有化部署，以及是否需要联网才能使用已收录的资源链接？**

答：AISP完全支持私有化部署，所有源代码、配置模板与依赖服务均可运行于隔离的内部网络环境。平台本身不强制要求互联网访问，但已收录的外部链接资源在被访问时自然需要从您的内网环境能够路由至对应域名。若内网存在出口代理或防火墙限制，您可以在配置文件中设置全局HTTP/HTTPS代理地址，使健康检查与资源抓取任务通过代理访问外部站点。平台不会将任何内部数据或使用记录回传至外部服务器。

**问：如何导入大量已有的书签或收藏夹数据，是否有格式限制？**

答：AISP提供了三种批量导入途径。第一，通过Web界面的批量添加功能，您可粘贴每行一个URL的纯文本列表，系统自动探测元数据并填充默认属性。第二，通过REST API的`/api/v1/resources/batch`端点提交JSON数组，每个元素包含`url`、`tags`、`category`等字段。第三，使用命令行工具`./scripts/bulk_import.py`支持读取CSV文件（列头映射可配置）以及Netscape格式的HTML书签导出文件。导入前建议先执行`--dry-run`模式预览解析结果，确认无误后再执行实际写入操作。

**问：资源评分规则是如何计算的，是否可以完全自定义？**

答：系统默认使用基于威尔逊置信区间与时间衰减因子相结合的复合评分算法，兼顾用户投票热度、资源更新活跃度以及历史点击转化率。默认规则封装在`aisp/core/scoring/default_scorer.py`中，完全开源可查看。若默认算法不符合您的业务逻辑，您可以通过实现`ScorerInterface`接口编写自定义评分器，并在`application.yml`中通过`scoring.custom_class`路径替换默认实现。自定义评分器支持引入任意数据字段和外部查询，系统在每次评分计算周期（每日凌晨）自动加载并执行。

## 许可证

MIT License

Copyright (c) 2026 AISP Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:18
