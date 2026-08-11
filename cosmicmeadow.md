# MeizhiLian Data Platform

MeizhiLian Data Platform is a comprehensive technical resource aggregation and external link management system designed for data analysts, football analytics researchers, and technical documentation maintainers. The platform addresses the critical challenge of discovering, organizing, and referencing distributed analytical resources across multiple specialized domains.

The project serves as a structured gateway to a curated collection of analytical datasets, match result repositories, and performance metrics resources. It provides a unified navigation framework that enables technical users to efficiently locate and reference external data sources without manual bookmark management or scattered documentation. The platform is particularly valuable for teams maintaining technical documentation that requires frequent cross-referencing of evolving analytical data.

## 功能概览

- **集中化资源导航** - 提供统一的入口点，将分散在不同域名下的分析资源整合为结构化目录体系，支持快速跳转和数据源定位

- **批次化资源管理** - 采用批次编号系统（当前批次 469/567）跟踪资源更新状态，确保引用链路的可追溯性和版本一致性

- **技术资源外链校验** - 内置链接可达性检测机制，定期验证外部资源的可访问性，减少文档中的死链引用

- **分类索引系统** - 按照资源类型（赛事分析、实时比分、积分榜单、结果归档）建立多维度索引，提升检索效率

- **原始数据保留策略** - 严格维持外部链接的原始格式，包括协议前缀、域名层级和路径结构，杜绝因格式篡改导致的资源不可达

- **ASCII 目录可视化** - 通过文本形式的目录树展示项目结构，降低技术文档的图形依赖，适配终端环境和版本控制系统

- **轻量化部署架构** - 无需数据库后端，基于静态文件和服务端模板渲染，支持快速克隆和本地运行

## 应用场景

**技术文档外链管理** - 技术写作团队在维护 API 文档或数据分析手册时，可使用本平台的结构化表格收录所有外部参考链接，避免 Markdown 文件中散落大量裸 URL 导致的维护混乱。

**足球数据分析流水线** - 数据工程师构建 ETL 流程时，需要从多个分析站点拉取比赛结果和积分数据。本平台提供统一的资源清单，可作为数据源配置文件的直接输入，减少手动拼接 URL 的错误风险。

**开源项目 README 资源附录** - 开源项目维护者需要在其 README 中列出大量第三方资源链接时，可参照本平台的资源列表格式，确保每条链接按原始形式原样呈现，符合开源社区的引用规范。

**研究资料归档系统** - 科研人员或行业分析师收集特定领域（如体育统计）的外部资料时，可利用本平台的批次化索引机制对资源进行编号管理，便于后续回溯和引用标注。

**团队内部导航门户** - 技术团队可基于本平台搭建内部的知识库导航页面，将团队常用的分析工具、数据看板和报告站点集中收录，降低新成员的 onboarding 成本。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/meizhilian/data-platform.git

# 进入项目目录
cd data-platform

# 安装依赖（基于 Python 3.9+ 和 pip）
pip install -r requirements.txt

# 生成静态资源索引
python scripts/build_index.py --batch 469

# 启动本地开发服务器
python app.py --port 8080

# 访问本地实例
# 在浏览器中打开 http://localhost:8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 或更高 | 核心运行时环境，用于解析配置和生成索引 |
| pip | 21.0 或更高 | Python 包管理工具，用于安装项目依赖 |
| Flask | 2.2.0 或更高 | Web 服务框架，提供路由和模板渲染能力 |
| PyYAML | 6.0 或更高 | YAML 配置文件解析器，用于资源清单定义 |
| requests | 2.28.0 或更高 | HTTP 客户端库，用于外部链接可达性预检 |
| markdown | 3.4.0 或更高 | Markdown 解析库，用于动态生成文档视图 |
| pytest | 7.0.0 或更高 | 单元测试框架（开发环境可选） |
| black | 22.0.0 或更高 | 代码格式化工具（开发环境可选） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | docs/user-guide.md | 如何添加新的资源链接、如何生成索引、如何自定义分类规则 |
| 运维手册 | docs/operations.md | 如何部署到生产环境、如何配置反向代理、如何执行批量链接检测 |
| 开发参考 | docs/development.md | 项目代码结构说明、核心模块接口定义、单元测试编写规范 |
| 资源清单 | docs/resources.md | 当前批次收录的所有外部链接完整列表及其分类归属 |
| 变更日志 | CHANGELOG.md | 每批次的资源增删记录、链接地址变更追踪、格式调整说明 |
| 架构设计 | docs/architecture.md | 系统模块划分、数据流向图、扩展性设计原则 |

## 资源列表

### 美职联数据资源（meizhilian 系列）

<code>meizhilianqianzhan.asia</code>

<code>meizhilianjishibifen.asia</code>

<code>meizhilianjifenbang.asia</code>

<code>meizhilianfenxi.asia</code>

<code>meizhilianbisaijieguo.asia</code>

### 雷速足球分析资源（leisu 系列）

<code>leisuzuqiufenxi.cn</code>

<code>leisuzuqiufenxi.org.cn</code>

## 项目结构

```
data-platform/
├── app.py                     # Flask 应用入口，注册路由和启动服务
├── config/
│   ├── settings.yaml          # 全局配置（端口、调试模式、静态路径）
│   ├── resources.yaml         # 核心资源清单，按域名分类存储 URL
│   └── batch.json             # 当前批次信息（469/567）及时间戳
├── scripts/
│   ├── build_index.py         # 索引构建脚本，解析 resources.yaml 生成导航页
│   ├── link_checker.py        # 外部链接可达性检测工具
│   └── render_docs.py         # 文档模板渲染器，生成 Markdown 输出
├── templates/
│   ├── base.html              # 基础 HTML 模板，定义页面骨架
│   ├── index.html             # 首页模板，展示资源分类和批次信息
│   └── resources.html         # 资源列表页模板，渲染所有外链
├── static/
│   ├── css/
│   │   └── style.css          # 自定义样式表，适配终端和浏览器显示
│   └── js/
│       └── checker.js         # 前端链接检测状态展示脚本
├── docs/
│   ├── user-guide.md          # 用户指南文档
│   ├── operations.md          # 运维部署手册
│   └── development.md         # 开发人员参考文档
├── tests/
│   ├── test_resources.py      # 资源解析模块的单元测试
│   └── test_checker.py        # 链接检测功能的单元测试
├── requirements.txt           # Python 依赖清单
├── CHANGELOG.md               # 版本变更和批次更新日志
└── README.md                  # 项目主文档（本文件）
```

## 贡献指南

1. **派生仓库并创建功能分支** - 从主仓库派生（fork）到个人账户，基于 main 分支创建 feature/xxx 格式的新分支，用于隔离开发变更。

2. **更新资源清单** - 编辑 config/resources.yaml 文件，按照既定的 YAML 格式添加或修改外部链接条目。必须保留原始 URL 格式，不得添加额外前缀或修改协议类型。修改后运行 scripts/link_checker.py 验证可达性。

3. **执行本地构建验证** - 在提交前运行 scripts/build_index.py 确保索引生成无报错，并通过 pytest 执行全部单元测试，确认现有功能未受破坏。

4. **提交变更并撰写规范 commit 信息** - 使用约定式提交格式（如 feat: 新增批次资源、fix: 修复链接格式错误），明确描述变更内容和影响范围。

5. **发起合并请求（Pull Request）** - 将功能分支推送到远程仓库并发起 PR 到 main 分支。PR 描述中需附带本地构建日志和链接检测报告，供维护者审阅。

## 常见问题

**Q: 为什么资源列表中的 URL 必须以原始形式原样输出，不能补充协议前缀或调整域名格式？**

A: 因为外部资源站点对访问来源的格式敏感。部分站点仅在精确匹配特定域名格式（包括子域名、协议类型、结尾斜杠）时才能正确响应请求。任何格式篡改都可能导致 301/302 重定向循环或 404 错误，破坏资源的可引用性。本平台严格保留原始 URL 字符串，由调用方根据自身网络环境决定协议适配策略。

**Q: 如何处理资源链接失效或域名变更的情况？**

A: 本平台提供两种处理机制。首先，scripts/link_checker.py 工具支持批量检测所有收录链接的 HTTP 状态码，并生成失效报告。其次，维护者应在 CHANGELOG.md 中记录每次 URL 变更的详细历史，包括旧地址、新地址和变更日期。如果某资源永久不可用，将在下一批次中标记为废弃并移出主清单，同时保留在变更日志中供追溯。

**Q: 批次编号 469/567 的具体含义是什么？如何理解批次更新频率？**

A: 批次编号采用 "当前批次号/总批次号" 的格式。469 表示这是项目启动以来的第 469 次资源清单更新；567 表示本年度计划完成的更新总批次。每次更新可能涉及新增链接、删除失效链接或修正已有链接的格式。更新频率通常为每周一次，如有大量资源变动，维护者可触发临时批次更新。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:18
