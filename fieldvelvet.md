# WebLink Navigator

WebLink Navigator 是一个面向开发者与技术研究人员的轻量级外链资源导航与状态监控系统。该项目定位于解决个人或小团队在维护多个外部数据源、第三方接口文档、运营活动页面时出现的链接分散、可用性未知、访问协议混乱等问题。通过统一收录、标签化分类、定时可用性探测以及基础访问统计，帮助用户构建一个可追溯、可观测的外部资源访问锚点平台。

目标用户包括开源项目维护者、数据运营人员、个人站长以及需要频繁查阅特定领域信息聚合页的研究者。项目不依赖复杂后端架构，采用静态生成结合边缘函数探测的方式，兼顾部署简便性与状态反馈的实时性。

## 功能概览

- 链接分类标签系统：支持对收录的每个 URL 赋予多个自定义标签，并可依据标签进行筛选与聚合视图展示，便于按业务域或数据来源快速定位资源。

- 可用性主动探测：利用定时任务或边缘请求对收录链接进行 HTTP 状态码与响应时长探测，并在管理面板中以颜色标记和趋势图呈现可用性变化，辅助判断外部服务稳定性。

- 访问跳转计数与来源分析：提供带重定向计数的短链式跳转入口，自动记录点击时间、来源 Referrer 与大致地理位置（基于 IP 库），为运营决策提供基础数据支撑。

- 多协议兼容与规范提示：录入时自动识别并提示 URL 协议缺失或大小写异常，保留原始输入格式的同时生成标准化访问链接，减少人工校对成本。

- 全文检索与快速过滤：基于标题、标签、描述字段构建简单倒排索引，支持模糊搜索与按状态（可用/不可用/未检测）快速过滤，提升日常查阅效率。

- 数据导入导出：支持 JSON 与 CSV 格式的批量链接导入，以及按筛选结果导出为 Markdown 表格或纯文本列表，便于与其他文档工具或监控系统对接。

- 自定义分组与排序：允许用户为不同项目或批次创建独立分组（如“第 51/567 批”），并在分组内手动拖拽排序或按可用率、添加时间排序。

## 应用场景

1. 活动运营资源聚合：运营人员需要集中管理多个外部比分、赛程、成绩查询页面，例如体育赛事期间不同数据服务商提供的实时接口。WebLink Navigator 可将这些链接统一收录并定时检测可用性，确保活动页面嵌入的外链始终有效，避免用户访问失败。

2. 技术文档外链维护：开源项目文档中常引用大量第三方依赖、规范标准、示例代码仓库等外部链接。随着时间推移部分链接可能失效或迁移。项目维护者可使用本系统定期扫描文档中的外链状态，及时更新或标记异常链接，提升文档质量。

3. 个人知识库资源索引：研究员或开发者会收集大量行业报告、数据集、在线工具等网址。使用 WebLink Navigator 按主题标签整理，配合搜索和过滤功能，可显著提高个人知识检索效率，避免浏览器收藏夹杂乱无章。

4. 数据源灾备切换辅助：在金融或实时数据展示类系统中，通常需要备用数据源。系统可监控多个同类型数据源链接的响应时间和状态码，当主源异常时迅速定位可用备用源，辅助运维决策。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 1. 克隆项目仓库
git clone https://github.com/weblink-navigator/weblink-navigator.git
cd weblink-navigator

# 2. 安装依赖（使用 pnpm，也可使用 npm 或 yarn）
pnpm install

# 3. 复制环境变量模板并配置
cp .env.example .env.local

# 4. 初始化本地数据库（SQLite）
pnpm run db:init

# 5. 启动开发服务
pnpm run dev
```

访问 <code>http://localhost:5173</code> 即可进入本地管理界面。生产环境部署请参考 `docs/deployment.md` 中的 Docker 或静态导出方案。

## 安装要求

| 依赖 | 必需版本 | 说明 |
|------|----------|------|
| Node.js | >= 18.17.0 | 运行时环境，需支持原生 fetch 和 ES Module |
| pnpm | >= 8.0.0 | 包管理器，也可使用 npm 7+ 或 yarn 1.22+ |
| SQLite3 | >= 3.35.0 | 嵌入式数据库，用于存储链接元数据、探测记录和计数 |
| Git | >= 2.30.0 | 版本控制，用于克隆仓库和后续更新 |
| 浏览器 | 现代浏览器（Chrome 110+ / Firefox 110+ / Safari 16+） | 管理界面与探测面板前端运行环境 |
| 可选：Docker | >= 20.10.0 | 若使用容器化部署方式，则需要 Docker Engine |
| 可选：Redis | >= 6.2.0 | 若启用分布式探测队列或缓存加速，可配置 Redis |
| 可选：Nginx / Caddy | 任意稳定版本 | 生产环境反向代理，用于静态资源服务和负载均衡 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/user-guide/` | 如何添加链接、配置探测频率、查看统计图表、导入导出数据 |
| 管理员指南 | `docs/admin-guide/` | 如何配置环境变量、调整探测并发数、设置告警阈值、备份数据库 |
| 开发参考 | `docs/developer-guide/` | 项目目录结构说明、核心数据模型、API 路由设计、如何编写自定义探测插件 |
| 部署方案 | `docs/deployment/` | 支持 Vercel / Netlify 静态导出、Docker Compose 完整部署、Kubernetes Helm 包等场景 |

## 资源列表

以下收录了当前批次（第 51/567 批）的全部外部资源链接，按类别分组展示。

### 体育赛事数据（比分/成绩）

<code>wangyitiyusaichengjieguo.org.cn</code>

<code>wangyitiyubifenwang.org.cn</code>

<code>wangyitiyubifensaicheng.org.cn</code>

<code>wangyitiyubifen.org.cn</code>

### 足球实时数据

<code>shoujiqiutanzuqiujishibifenwang.net.cn</code>

### 瑞典超级联赛数据

<code>ruidianchaojishibifen.org.cn</code>

<code>ruidianchaojifenbang.org.cn</code>

## 项目结构

```
weblink-navigator/
├── apps/
│   ├── web/                           # 主前端应用 (Vite + React + Tailwind)
│   │   ├── src/
│   │   │   ├── pages/                 # 路由页面：仪表盘、链接列表、详情、探测日志
│   │   │   ├── components/            # 通用 UI 组件：标签选择器、状态徽章、图表卡片
│   │   │   ├── hooks/                 # 自定义 React Hooks：useProbe, useFilter, usePagination
│   │   │   └── store/                 # Zustand 状态管理：链接数据、筛选条件、主题
│   │   └── index.html
│   └── api/                           # 边缘 API 服务 (Cloudflare Workers / Vercel Functions)
│       ├── routes/
│       │   ├── links/                 # 链接 CRUD 与查询接口
│       │   ├── probe/                 # 探测触发与结果查询接口
│       │   └── stats/                 # 访问计数与趋势汇总接口
│       └── utils/
│           ├── fetcher.ts             # 带超时与重试的 HTTP 探测工具
│           └── parser.ts              # 协议补全与域名规范化辅助
├── packages/
│   ├── db/                            # 数据库模型与迁移 (Prisma + SQLite)
│   │   ├── schema.prisma              # 表定义：Link, ProbeRecord, ClickLog, Tag
│   │   └── migrations/                # 增量迁移脚本
│   └── shared/                        # 类型定义与常量 (TypeScript)
│       ├── types/                     # LinkStatus, ProbeResult, FilterOptions
│       └── constants/                 # 默认标签列表、探测超时默认值
├── scripts/
│   ├── cron/                          # 定时任务脚本 (node-cron)
│   │   └── daily-probe.ts             # 每日全量可用性探测
│   └── import/                        # 批量导入工具 (支持 JSON / CSV)
├── docs/                              # 详细文档（见上方文档导航）
├── tests/                             # 单元测试与集成测试 (Vitest)
├── .env.example                       # 环境变量模板
├── docker-compose.yml                 # 容器化完整部署编排
├── Dockerfile                         # 生产镜像构建文件
└── README.md                          # 当前文件
```

## 贡献指南

1. 复刻项目仓库并创建新分支：从主仓库复刻代码，然后基于 `main` 分支创建以 `feat/` 或 `fix/` 为前缀的功能分支，例如 `feat/add-batch-import`。

2. 遵循代码规范与提交格式：项目使用 ESLint + Prettier 统一代码风格，提交信息需遵循 Conventional Commits 规范（如 `feat: 增加探测重试机制`）。提交前会自动运行 lint-staged 进行格式化。

3. 编写或更新测试用例：针对新增功能或修复的缺陷，请在 `tests/` 目录下补充对应的单元测试或集成测试，确保覆盖率不低于现有水平。

4. 更新相关文档与示例：若改动涉及用户操作流程、配置项或 API 行为，请同步更新 `docs/` 下对应的用户手册或开发指南，并在 PR 描述中说明文档变更。

5. 提交 Pull Request 并等待审阅：推送分支至远程仓库后，向主仓库提交 PR。请详细填写 PR 模板中的改动概述、测试情况和影响范围。审阅通过后由维护者合并。

## 常见问题

**问：探测功能是否会因为外部网站限制导致频繁失败？**

答：系统默认使用 `HEAD` 请求进行探测，超时时间设为 5 秒，并支持自定义 User-Agent 和重试次数（默认 2 次）。若目标网站有严格的防爬策略，可在环境变量中配置 `PROBE_METHOD=GET` 或增加 `PROBE_TIMEOUT` 值。同时，系统会记录每次探测的响应头摘要，便于判断是否被拒绝访问。

**问：如何迁移已有的浏览器收藏夹或书签到本系统？**

答：大多数浏览器支持导出书签为 HTML 文件。项目提供 `scripts/import/bookmark-html.ts` 脚本，可将 Netscape 格式的书签 HTML 解析为 CSV 中间格式，再通过导入界面批量录入。若书签数量较大（超过 500 条），建议分批次导入并利用标签分组功能进行分类。

**问：静态导出模式下，可用性探测如何工作？**

答：若部署为完全静态站点（如使用 Vite 的 `build` 输出），系统会保留一个轻量级边缘函数（部署于 Vercel / Cloudflare Pages）用于探测。当用户访问仪表盘时，前端会异步请求该边缘函数返回最新的探测缓存结果（缓存 TTL 默认 10 分钟）。也可配置完全客户端探测，但会受到浏览器跨域限制，不建议用于生产环境。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
