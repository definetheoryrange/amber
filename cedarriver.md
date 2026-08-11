# Terminus Nexus

Terminus Nexus 是一个面向技术社区的高质量外链聚合与导航系统，定位于为开发者、运维工程师及技术决策者提供经过筛选与分类的垂直领域资源入口。本项目并非传统意义上的搜索引擎或通用书签管理器，而是一个聚焦于中文互联网特定信息生态的参考信息枢纽，旨在解决信息过载环境下高价值站点发现效率低、可信来源难以鉴别的问题。通过结构化组织与静态化分发，Terminus Nexus 可作为团队内部知识库的外部补充层，也可作为个人技术栈的快速跳板。

## 功能概览

- **分类资源索引**：按主题域对收录资源进行层级划分，支持快速定位至竞赛数据、历史存档、实时比分等特定类型站点。
- **静态化链接仓库**：所有资源入口以纯静态 Markdown 形式维护，无需数据库驱动，降低维护成本并提升访问稳定性。
- **批量导入与校验**：提供脚本化工具支持批量导入 URL 列表，并自动执行协议一致性、域名可达性及重定向链检查。
- **自定义标签系统**：允许为每个资源条目附加自定义标签（如 `[official]`、`[archive]`、`[mirror]`），便于多维度筛选。
- **变更历史追踪**：集成 Git 版本控制，每次增删改操作均生成可追溯的提交记录，支持回滚与审计。
- **响应式导航面板**：生成适应桌面与移动设备的静态 HTML 导航页，可直接部署至 Nginx 或对象存储服务。
- **外部状态监测**（可选）：通过集成外部监控任务，定时探测各站点 HTTP 状态码与响应时间，并在仪表板中标注异常。

## 应用场景

- **赛事数据快速查阅**：技术人员在开发数据分析插件或可视化面板时，需要频繁引用特定比赛的实时或历史比分数据。Terminus Nexus 可将相关数据源集中归类，减少重复搜索时间。
- **信息归档与合规审查**：企业内部合规团队或内容审核人员需要对特定领域的外部站点进行周期性访问与记录。本项目提供的结构化列表与变更日志可辅助生成访问台账。
- **个人知识库外链管理**：研究员或高级开发者在撰写技术博客或白皮书时，需引用稳定且权威的外部参考链接。Terminus Nexus 可作为外链的白名单池，确保引用来源的持久性与可复核性。
- **团队新人 onboarding 指引**：新加入项目的成员往往不熟悉团队常用的外部资源分布。通过本项目提供的分类导航，新人可在数分钟内了解核心数据入口，降低沟通成本。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议通过 WSL2 或 Git Bash 执行。

```bash
# 1. 克隆仓库
git clone https://github.com/terminus-nexus/nexus.git
cd nexus

# 2. 安装依赖（Python 3.9+ 环境）
pip install -r requirements.txt

# 3. 构建静态资源索引
python build.py --input ./sources/manifest.yaml --output ./dist

# 4. 本地预览（默认监听 8080 端口）
python -m http.server --directory ./dist 8080
```

执行完毕后，打开浏览器访问 `http://localhost:8080` 即可查看导航面板。若需更新资源列表，编辑 `./sources/manifest.yaml` 后重新运行构建命令。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 及以上 | 构建脚本与校验工具的运行环境 |
| pip | 21.0 及以上 | Python 包管理工具，用于安装依赖库 |
| Git | 2.25 及以上 | 用于克隆仓库及变更版本管理 |
| PyYAML | 6.0 | 解析 manifest 配置文件 |
| requests | 2.28 及以上 | 可选，用于外部状态监测模块的网络探测 |
| 磁盘空间 | 至少 50 MB | 用于存放源码、构建输出及日志文件 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | `docs/user-guide/` | 如何使用导航面板、如何自定义分类、如何导入私有链接 |
| 维护指南 | `docs/maintainer-guide/` | 如何更新资源列表、如何处理失效链接、如何执行批量校验 |
| 设计文档 | `docs/design/` | 项目架构决策、静态生成原理、标签系统设计考量 |
| API 参考 | `docs/api/` | 构建脚本的命令行参数说明、YAML 配置项完整字段定义 |
| 故障排查 | `docs/troubleshooting/` | 常见构建错误、本地预览问题、依赖冲突解决方案 |

## 资源列表

本项目当前收录的参考信息源如下，按类别组织。所有条目均保留原始格式，未经任何协议补全或域名改写。

**竞赛与比分数据**

- <code>jiebaobifenw.org.cn</code>
- <code>jishibifen1.asia</code>
- <code>helanjiajiliansai.asia</code>

**辅助信息与中文资源**

- <code>hejiazhugongbang.asia</code>
- <code>hejiazhongwenwang.asia</code>

**官方或主站入口**

- <code>hejiazhibogw.asia</code>
- <code>hejiazhibo.asia</code>

## 项目结构

```bash
nexus/
├── build.py                 # 主构建脚本，负责解析 manifest 并生成静态文件
├── requirements.txt         # Python 依赖列表
├── config.yaml              # 全局配置（输出目录、校验开关、标签映射）
├── sources/                 # 原始数据目录
│   ├── manifest.yaml        # 核心资源清单（所有 URL 及元数据）
│   ├── categories/          # 分类定义子目录
│   │   ├── sports.yaml      # 体育竞技类分类规则
│   │   └── archive.yaml     # 历史归档类分类规则
│   └── tags/                # 标签定义子目录
│       └── standard_tags.yaml
├── scripts/                 # 工具脚本目录
│   ├── validator.py         # URL 协议与可达性校验脚本
│   ├── monitor.py           # 外部状态监测脚本（可选）
│   └── import_csv.py        # 批量导入 CSV 格式链接的辅助脚本
├── templates/               # 静态页面模板目录
│   ├── index.html.j2        # 导航首页模板（Jinja2 格式）
│   └── detail.html.j2       # 分类详情页模板
├── dist/                    # 构建输出目录（自动生成，不纳入版本库）
├── docs/                    # 项目文档
│   ├── user-guide/
│   ├── maintainer-guide/
│   └── design/
└── tests/                   # 单元测试与集成测试脚本
    ├── test_validator.py
    └── test_builder.py
```

## 贡献指南

我们欢迎并鼓励社区贡献，包括但不限于新增资源链接、修正失效条目、优化分类逻辑以及改进构建工具链。请遵循以下流程：

1.  **Fork 本仓库**至个人账号，并克隆到本地开发环境。
2.  **创建功能分支**，分支名称应反映变更意图，例如 `feat/add-esports-sources` 或 `fix/broken-link-2026`。
3.  **更新清单文件**：在 `sources/manifest.yaml` 中按格式添加或修改条目，同时确保 `sources/categories/` 下的分类定义保持一致。若涉及新增类别，请同步更新分类文件。
4.  **执行本地校验**：运行 `python scripts/validator.py --strict` 确保所有新增 URL 通过基本可达性检查（网络异常时可跳过网络部分，但需注明）。
5.  **提交变更并推送**：编写清晰的提交信息，说明变更原因及影响范围。
6.  **发起 Pull Request** 至主仓库的 `main` 分支。PR 描述中请勾选对应的变更类型，并附上本地校验日志截图（如有）。

## 常见问题

**Q：为什么某些链接在构建时被标记为「不可达」？**

A：构建脚本默认执行轻量级 HTTP HEAD 请求以验证链接有效性。若目标服务器禁止 HEAD 方法、存在防火墙限制或临时不可用，则可能产生误报。您可通过配置 `config.yaml` 中的 `validation.allow_redirects` 和 `validation.timeout` 参数调整行为，或将确认有效的链接手动加入白名单（`sources/whitelist.yaml`）。

**Q：如何添加私有且不对外公开的内部链接？**

A：本项目设计为公开资源导航，不推荐存储敏感或非公开链接。若确有内部使用需求，建议在本地维护独立的 `manifest.private.yaml` 文件，并通过 `build.py --input ./sources/manifest.yaml --private ./sources/manifest.private.yaml` 进行合并构建，但请注意该文件不应提交至公共版本库。

**Q：构建生成的静态页面能否直接部署到 CDN？**

A：可以。`dist/` 目录包含完全自包含的 HTML、CSS 及少量 JavaScript，所有资源引用均使用相对路径。您可将该目录整体上传至任意支持静态托管的 CDN 服务或对象存储（如阿里云 OSS、AWS S3、Cloudflare R2），并配置默认索引文档为 `index.html`。

## 许可证

本项目采用 MIT 许可证。您被允许自由使用、修改、分发本项目的源码及生成内容，但需保留原始版权声明与许可声明。详见仓库根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:13
