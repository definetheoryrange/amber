# TechLink Navigator

TechLink Navigator 是一个面向技术团队与独立开发者的开源外链资源聚合与导航系统。该项目定位为轻量级的技术资源目录枢纽，用于解决开发者在日常工作中检索高质量外部技术文档、社区讨论、数据看板及赛事信息时效率低下、链接分散的问题。通过结构化的资源分类、版本化的链接追踪以及简洁的本地部署能力，本项目帮助用户构建私有或团队共享的技术资源入口。

项目目标用户包括：需要整理技术学习路径的初级开发者、需要维护团队公共书签库的架构师、以及需要对外输出技术导航页面的开源社区运营者。TechLink Navigator 不存储任何实际内容，仅提供元数据描述与链接跳转，遵循开放互联网的链接原则。

## 功能概览

- 分类资源目录管理：支持按技术领域、赛事阶段、数据统计等维度对链接进行多级标签与分组，便于快速筛选。

- 链接可用性监控：内置定时检测模块，周期性地对已收录链接进行 HTTP 状态检查，自动标记失效或重定向的资源。

- 只读镜像导出：支持将当前资源列表导出为静态 Markdown 或 JSON 格式，便于集成到其他文档系统或静态站点生成器。

- 自定义元数据扩展：每条资源记录允许附加说明文本、维护人、更新频率、关联项目等自定义字段，满足团队协作需求。

- 检索与过滤接口：提供基于关键词、域名、分类标签的简单检索功能，支持命令行与 Web 界面两种交互方式。

- 版本变更日志：记录每次资源新增、删除或 URL 变更的操作历史，支持回滚与审计追踪。

- 单文件部署模式：所有配置与资源数据存储于单一 SQLite 数据库中，无需外部依赖，开箱即用。

## 应用场景

- 技术团队内部知识库聚合：团队可将常用的 API 文档、设计规范、内部工具链入口集中收录，通过 TechLink Navigator 统一呈现，替代浏览器散乱的书签栏。

- 开源项目 README 外链辅助：开源项目维护者可将项目依赖的参考文档、数据源、社区论坛链接通过本系统进行版本化管理，并定期生成链接清单嵌入项目文档。

- 赛事或活动信息追踪：针对周期性技术竞赛或数据预测活动，用户可录入不同阶段的数据网站（如积分榜、赛程表），利用可用性监控功能及时获知页面变更。

- 个人学习路径组织：学习者可按技术栈（前端、后端、运维、算法）建立分类目录，将教程、视频、在线工具等资源统一归档，并定期清理失效链接。

- 技术社区导航站建设：社区运营者可使用本系统快速搭建轻量级导航页面，通过导出静态数据配合 Nginx 等 Web 服务器发布，无需动态后台。

## 快速开始

以下步骤适用于 Linux / macOS / WSL 环境，请确保已安装 Git 与 Python 3.9 及以上版本。

```bash
# 1. 克隆代码仓库
git clone https://github.com/techlink-navigator/navigator-core.git
cd navigator-core

# 2. 创建 Python 虚拟环境并安装依赖
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 3. 初始化数据库并导入默认资源
python manage.py initdb
python manage.py import --source default_resources.yaml

# 4. 启动本地开发服务器
python manage.py runserver --host 127.0.0.1 --port 8080
```

启动成功后，访问 `http://127.0.0.1:8080` 即可查看资源导航首页。如需导入自定义资源，请参考 `docs/import_guide.md` 中的 YAML 格式说明。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 ~ 3.12 | 核心运行环境，3.13 暂未适配 |
| SQLite | 3.35.0 及以上 | 内置数据库，用于存储资源元数据与日志 |
| Git | 2.25.0 及以上 | 仅开发阶段需要，用于版本克隆与提交 |
| requests | 2.31.0 | 用于链接可用性监控的 HTTP 客户端 |
| pyyaml | 6.0.1 | 用于解析资源导入与导出的 YAML 格式 |
| flask | 2.3.3 | Web 交互界面框架（仅当启用 Web 模式时必需） |
| pytest | 7.4.0 | 单元测试框架（仅开发测试需要） |
| black | 23.9.1 | 代码格式化工具（仅贡献代码时使用） |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/user_guide.md` | 如何添加资源、如何分类、如何查看监控结果 |
| 管理员指南 | `docs/admin_guide.md` | 如何迁移数据库、如何备份、如何处理失效链接 |
| 贡献规范 | `CONTRIBUTING.md` | 代码风格要求、PR 流程、测试覆盖率标准 |
| API 参考 | `docs/api_reference.md` | 提供哪些 REST 接口、请求参数与返回格式 |
| 部署方案 | `docs/deployment.md` | 如何部署到生产环境（Nginx + uWSGI / Docker） |
| 设计文档 | `docs/design.md` | 数据库 ER 图、模块划分、监控调度策略 |
| 变更日志 | `CHANGELOG.md` | 每个版本的发布说明与兼容性变更 |

## 资源列表

### 积分与数据统计类

<code>yijiabifen.org.cn</code>

<code>xueyuanyuanzuqiubifenwang.org.cn</code>

<code>xijiazuqiubifenwang.org.cn</code>

<code>xijiazuqiubifen.org.cn</code>

### 赛程与赛事结果类

<code>xueyuanyuanzuqiubisaijieguo.net.cn</code>

<code>xueyuanyuanzuqiubifensaicheng.org.cn</code>

<code>xijiasaicheng.org.cn</code>

## 项目结构

```
navigator-core/
├── app/
│   ├── __init__.py               # 应用工厂模式初始化
│   ├── config.py                 # 环境配置（开发/测试/生产）
│   ├── models/
│   │   ├── __init__.py           # 模型基类与数据库连接
│   │   ├── resource.py           # 资源实体模型（URL、标题、分类、状态）
│   │   ├── category.py           # 分类树模型（支持无限级嵌套）
│   │   └── changelog.py          # 变更日志模型（操作人、时间、字段差异）
│   ├── services/
│   │   ├── __init__.py           # 服务层导出
│   │   ├── monitor.py            # 链接可用性检测服务（异步线程池）
│   │   ├── importer.py           # YAML/JSON 导入解析器
│   │   └── exporter.py           # 数据导出为 Markdown/JSON/HTML
│   ├── web/
│   │   ├── __init__.py           # Flask 蓝图注册
│   │   ├── routes.py             # 首页、分类页、检索页路由
│   │   └── templates/            # Jinja2 模板目录（含基础布局与组件）
│   └── cli/
│       ├── __init__.py           # Click 命令组注册
│       ├── manage.py             # 数据库初始化、迁移、种子数据加载
│       └── monitor_cmd.py        # 手动触发监控与报告生成
├── tests/
│   ├── unit/                     # 单元测试（模型、服务、工具函数）
│   ├── integration/              # 集成测试（数据库、API 端点）
│   └── fixtures/                 # 测试用 YAML 样例与模拟响应数据
├── docs/                         # 完整文档目录（用户、管理、设计、部署）
├── scripts/
│   ├── pre-commit.sh             # Git 钩子：运行 lint 与快速测试
│   └── backup_db.sh              # 定时备份 SQLite 数据库脚本
├── requirements.txt              # 生产环境依赖列表
├── requirements-dev.txt          # 开发额外依赖（测试、lint、文档生成）
├── setup.py                      # 打包配置（用于 pip 安装）
├── pyproject.toml                # 项目元数据与工具配置（black、pytest）
├── README.md                     # 项目主文档（即本文档）
└── LICENSE                       # MIT 许可证文本
```

## 贡献指南

1. 阅读设计文档与 API 参考，了解模块边界与数据流向，建议从 `docs/design.md` 开始。

2. 在 GitHub 仓库中提交 Issue 说明您希望修复的问题或新增的功能，等待维护者确认需求范围。

3. Fork 本仓库，创建以 `feature/` 或 `fix/` 为前缀的分支，遵循 `black` 与 `pylint` 的代码风格要求。

4. 编写或更新单元测试，确保新增代码覆盖率达到 85% 以上，运行 `pytest tests/` 验证全部用例通过。

5. 提交 Pull Request 并填写变更摘要，关联相关 Issue，维护者将在 3 个工作日内进行 Review。

## 常见问题

Q: 监控模块是否会影响被监控网站的访问压力？

A: 监控模块默认采用间隔为 1 小时的 HEAD 请求，且并发数限制为 5，单个链接的单次检测仅产生一个无正文的请求包，对目标服务器几乎无影响。用户可自行调整检测间隔与并发数。

Q: 项目是否支持 MySQL 或 PostgreSQL 替代 SQLite？

A: 当前版本仅适配 SQLite，主要原因在于本项目的轻量级定位以及避免外部数据库依赖。若需使用生产级数据库，可基于现有 ORM 抽象层（SQLAlchemy）自行扩展方言支持，但官方暂不提供额外适配。

Q: 如何批量导入大量已有书签（如浏览器导出的 HTML）？

A: 项目暂不直接支持 HTML 书签解析，但提供 `import --from-csv` 实验性命令，可将 CSV 格式（列：标题,URL,分类,说明）转换为内部 YAML 模板后导入。详细步骤请参考 `docs/admin_guide.md` 中的导入章节。

## 许可证

MIT License。详见项目根目录下的 LICENSE 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:19
