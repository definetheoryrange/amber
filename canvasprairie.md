# ResourceLink Navigator

ResourceLink Navigator 是一个面向技术开发者与开源项目维护者的外链资源聚合与导航系统。该项目定位于解决技术文档编写、项目调研、外部参考链接收集整理过程中出现的链接分散、版本追溯困难、引用格式不统一等问题。目标用户包括开源项目维护者、技术文档编写者、技术调研人员以及需要系统化管理外部资源链接的研发团队。

该项目通过提供结构化的资源收录框架、可定制的分类体系以及标准化的链接输出格式，帮助用户高效建立外部资源库，并在多个文档或项目之间复用同一套资源索引。ResourceLink Navigator 本身不存储或缓存外部资源内容，仅提供链接组织与展示能力，确保资源引用的原始性与实时性。

## 功能概览

- **结构化链接收录**：按照自定义类别对海量外部链接进行分门别类的整理，支持多级分类嵌套。
- **标准化格式输出**：强制使用 code 标签包裹所有 URL，确保链接在不同渲染环境下的可识别性与一致性。
- **链接状态标记**：支持对每个链接添加可用性、更新频率、重要程度等元数据标记，便于后期维护。
- **多文档协同索引**：一份资源索引可同时被多个下游文档引用，确保链接信息的单一事实源。
- **版本变更追踪**：记录每次链接的新增、删除与修改操作，支持回退至任意历史版本的资源清单。
- **批量导入导出**：支持从 CSV、JSON 及 Markdown 表格等常见格式批量导入链接数据，也可反向导出。
- **导航目录自动生成**：基于链接分类与标签自动生成多级导航目录，支持静态站点生成器直接集成。

## 应用场景

- **开源项目 README 外部参考整理**：当项目 README 需要引用大量第三方文档、官方网站或技术博客时，可使用 ResourceLink Navigator 统一管理这些外链，保证 README 正文部分专注于项目本身，而外部资源以结构化方式附录在后。
- **技术调研报告的资源附录编写**：技术调研过程中会积累数十甚至上百个参考链接，通过该导航系统可按照技术领域、厂商、优先级等维度分类，最终自动生成调研报告末尾的资源附录章节。
- **团队内部知识库的外部引用标准化**：企业内部 Wiki 或知识库中频繁引用外部技术文档，不同成员引用的链接格式不一致，使用本系统可强制统一输出格式，并集中维护链接的有效性。
- **静态技术博客的友情链接页管理**：技术博客作者可将友链、工具推荐、常用文档等资源纳入导航系统，每次博客站点重新构建时自动更新链接页面，避免手工维护的遗漏。

## 快速开始

以下步骤将在本地环境完成 ResourceLink Navigator 的克隆、依赖安装与开发服务器启动。

```bash
# 克隆项目仓库
git clone https://github.com/resourcelink-navigator/rln-core.git

# 进入项目目录
cd rln-core

# 安装 npm 依赖（项目使用 Node.js 18+ 与 pnpm）
pnpm install

# 复制环境变量示例文件
cp .env.example .env.local

# 启动开发服务器，默认监听端口 3000
pnpm dev
```

启动成功后，访问控制台输出的本地地址即可进入资源管理界面，初始管理员账号与密码见 .env.local 文件中的默认配置。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Node.js | 18.x 或更高 LTS 版本 | 项目运行时环境，用于执行构建脚本与开发服务器 |
| pnpm | 8.x 或更高版本 | 包管理器，用于依赖安装与 monorepo 工作区管理 |
| PostgreSQL | 15.x 或更高版本 | 主数据库，用于存储链接元数据、分类树与变更历史 |
| Redis | 7.x 或更高版本 | 缓存中间件，用于加速导航目录生成与链接状态查询 |
| TypeScript | 5.x 或更高版本 | 开发时类型检查与编译所需，项目代码全量采用 TypeScript 编写 |
| Docker | 20.x 或更高版本（可选） | 用于生产环境容器化部署，开发环境非必需 |
| Nginx | 1.22.x 或更高版本（可选） | 生产环境反向代理与静态资源缓存推荐使用 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | docs/getting-started/installation.md | 如何在不同操作系统上完成安装与初始配置 |
| 核心概念 | docs/core-concepts/link-schema.md | 链接数据模型包含哪些字段，分类与标签如何设计 |
| 使用教程 | docs/usage/bulk-import.md | 如何从现有 CSV 表格批量导入链接数据 |
| 运维手册 | docs/operations/backup-and-restore.md | 如何备份数据库与文件存储，故障时如何恢复 |
| 开发者指南 | docs/development/api-reference.md | 后端 RESTful API 的完整端点列表与调用示例 |
| 版本发布 | docs/releases/changelog.md | 每个版本的更新内容、破坏性变更与升级注意事项 |

## 资源列表

### 体育数据分析类资源

<code>qiutanzuqiufenxi.asia</code>

<code>qiutanwanchangbifen.asia</code>

<code>qiutansaishiqianzhan.org.cn</code>

<code>qiutansaiguo.asia</code>

<code>qiutanjinrituijian.org.cn</code>

<code>qiutanjinrituijian.asia</code>

<code>qiutanjishibifenw.org.cn</code>

## 项目结构

```
rln-core/
├── apps/
│   ├── web/                           # 主 Web 应用（Next.js 13+）
│   │   ├── src/app/                   # App Router 页面路由
│   │   ├── src/components/            # UI 组件库（shadcn/ui 风格）
│   │   └── src/lib/                   # 前端工具函数与 API 客户端
│   └── admin/                         # 后台管理面板（独立微前端应用）
│       ├── src/pages/                 # 管理页面（链接管理、分类编辑、历史记录）
│       └── src/hooks/                 # 管理面板专用 React Hooks
├── packages/
│   ├── core/                          # 核心数据模型与验证逻辑（跨应用共享）
│   │   ├── src/schemas/               # Zod 链接数据模型与分类树 Schema
│   │   └── src/validators/            # 自定义校验规则（URL 格式、域名黑名单等）
│   ├── db/                            # 数据库迁移与 ORM 模型（Prisma 5.x）
│   │   ├── prisma/schema.prisma       # 主数据模型定义（Link、Category、History）
│   │   └── migrations/                # 按时间顺序的迁移文件
│   └── utils/                         # 通用工具集（日志、缓存、加密、格式化）
│       ├── src/logger/                # 结构化日志封装（Pino 驱动）
│       └── src/cache/                 # Redis 缓存装饰器与过期策略
├── configs/
│   ├── eslint/                        # ESLint 共享配置（扁平配置格式）
│   ├── typescript/                    # TypeScript 编译选项（严格模式启用）
│   └── prettier/                      # Prettier 代码格式化规则
├── scripts/
│   ├── seed/                          # 初始化种子数据（默认分类与示例链接）
│   └── export/                        # 批量导出脚本（支持 Markdown / JSON / CSV）
├── docker-compose.yml                 # 开发环境 PostgreSQL + Redis 容器编排
├── .env.example                       # 环境变量模板（含数据库连接串与 JWT 密钥）
├── package.json                       # 根 package.json（pnpm workspace 配置）
├── pnpm-workspace.yaml                # pnpm 工作区声明（apps 与 packages）
└── README.md                          # 项目说明文档（当前文档）
```

## 贡献指南

1. 在 GitHub Issues 中搜索现有议题或创建新议题，描述你希望修复的问题或新增的功能，等待维护者确认需求合理性后再着手开发，避免重复劳动或方向偏离。
2. 从项目仓库 Fork 个人副本，并在本地新建功能分支，分支命名遵循 `feat/功能简述` 或 `fix/问题简述` 格式，提交信息遵循 Conventional Commits 规范。
3. 开发完成后，确保所有现有单元测试与集成测试通过，并为新增代码补充对应的测试用例。测试覆盖率不得低于原有水平。
4. 提交 Pull Request 至主仓库的 `main` 分支，PR 描述中需关联对应的 Issue 编号，并简要说明实现方案与测试结果。PR 将由两名维护者进行 Code Review，通过后合并。
5. 若涉及数据库迁移或配置变更，必须在 PR 中附带详细的迁移说明与回滚方案，并在合并后及时更新 `docs/operations/` 下的运维文档。

## 常见问题

**Q：项目是否支持完全离线运行，不访问任何外部网络？**

A：ResourceLink Navigator 本身不主动发起对外部资源的网络请求，所有链接仅作为静态字符串存储与展示。在首次安装时需要通过 npm/pnpm 下载依赖包，这部分需要网络连接。安装完成后，系统可在完全内网环境中运行，数据库与缓存服务均部署在本地或私有网络内。

**Q：如果收录的某个外部链接失效或域名过期，系统能否自动检测并告警？**

A：当前版本不提供自动探测链接可用性的功能，以避免对外部站点造成不必要的流量压力。但系统支持手动为每个链接标记“待验证”状态，并提供了链接状态筛选视图，方便维护者定期人工复查。后续版本计划引入可选的主动探测模块，由用户按需启用。

**Q：如何将现有资源列表从旧版本迁移到新版本，迁移过程中数据是否安全？**

A：项目提供了一套完整的迁移工具链，位于 `scripts/export/` 目录下。在执行主版本升级前，建议先使用导出脚本将当前全部链接数据导出为 JSON 文件。升级完成后，再使用批量导入功能将 JSON 文件重新导入。所有数据操作均记录在历史变更表中，即便迁移过程中出现异常，也可以从历史记录中追溯数据变更轨迹。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
