# RizhiLink Hub

RizhiLink Hub 是一个面向数据分析师、运维工程师与开源技术爱好者的高质量外链与资源导航系统。项目定位为“技术日志与数据链路的中转站”，旨在解决技术从业者在日常工作中难以快速定位可靠、专业、垂直领域资源的问题。通过人工筛选与社区共建机制，RizhiLink Hub 将分散于全球网络中的优质日志分析平台、数据看板工具、趋势预测站点与实时比分接口统一收录，并提供结构化的访问入口与状态监控。

目标用户包括但不限于数据可视化工程师、系统监控运维人员、体育数据爱好者、量化分析研究者以及开源软件贡献者。项目本身不托管任何数据内容，仅提供合规的公开资源链接与元信息描述，帮助用户节省检索时间，降低信息噪音。

## 功能概览

- **垂直领域资源聚合**：按“日志分析”“趋势预测”“实时数据”“排行榜单”等维度对收录链接进行分类，每个类别下提供站点名称、简要描述与访问地址。

- **链接可用性健康检查**：系统每天定时对收录的所有外链发起 HEAD 请求，检测响应状态码与响应时间，并在仪表盘中标记异常链接，便于维护者及时更新。

- **标签化检索与过滤**：每个资源可被打上多个标签，如“#体育数据”“#日线走势”“#积分系统”，用户可通过组合标签快速缩小范围。

- **用户提交与投票机制**：注册用户可提交新的资源链接，经社区投票达到阈值后由管理员审核并入收录库，形成去中心化的资源增长模式。

- **访问统计与热度排行**：记录每个外链的点击次数与最近 7 天访问趋势，生成热度榜单，帮助新用户发现当前最受关注的资源。

- **开源 API 接口**：提供 RESTful API 供第三方开发者批量获取资源列表、分类信息与健康状态，方便集成到其他导航页或监控系统中。

- **暗色主题与响应式布局**：前端页面支持明暗主题切换，并在桌面端、平板与手机设备上自适应显示，保证移动端使用体验。

## 应用场景

- **运维值班期间的快速查证**：当系统产生异常日志时，运维人员可通过 RizhiLink Hub 快速跳转至多个日志分析平台，对照不同源的数据进行交叉排查，缩短故障定位时间。

- **体育数据爱好者的每日看板**：用户每天早上打开 RizhiLink Hub 的“体育数据”分类，查看积分榜更新、赛程前瞻以及实时比分接口，获取当日赛事动态，无需分别记忆多个网址。

- **技术博客与教程的外链引用**：技术作者在撰写数据分析教程时，可将 RizhiLink Hub 中的资源链接作为参考文献或工具推荐，提高教程的实用性与可复现性，同时为项目带来自然流量。

- **量化策略研究的数据源发现**：量化研究人员通过项目的“趋势预测”与“历史分时”类别，发现新的数据源站点，用于回测模型或补充特征工程中的数据维度。

## 快速开始

以下命令演示如何从 GitHub 克隆项目源码，安装依赖并启动开发服务器。请确保系统已安装 Git 与 Node.js 18.x 及以上版本。

```bash
# 克隆代码仓库
git clone https://github.com/rizhilink/rizhilink-hub.git

# 进入项目目录
cd rizhilink-hub

# 安装 npm 依赖
npm install

# 启动本地开发服务器（默认监听 3000 端口）
npm run dev
```

启动成功后，在浏览器中访问 `http://localhost:3000` 即可预览首页。生产环境部署请参考 `docs/deployment.md` 文档。

## 安装要求

生产环境部署所需的最小依赖组件如下表所示。开发环境下可省略 PostgreSQL 与 Redis，使用 SQLite 与内存缓存替代。

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x 或 20.x LTS | 项目运行时环境，建议使用 nvm 管理版本 |
| npm | 9.x 或 10.x | 包管理器，用于安装前端与后端依赖 |
| PostgreSQL | 14.x 及以上 | 主数据库，存储资源信息、用户数据、访问日志 |
| Redis | 6.x 及以上 | 缓存会话与热点数据，提升 API 响应速度 |
| Nginx | 1.22.x 及以上 | 反向代理服务器，用于静态资源缓存与负载均衡 |
| PM2 | 5.x 及以上 | Node.js 进程守护工具，生产环境推荐使用 |
| Git | 2.30.x 及以上 | 版本控制，用于拉取代码与自动部署脚本 |

## 文档导航

项目文档按层面划分为用户手册、运维指南、开发文档与 API 参考，下表列出各目录对应的核心内容与解决的实际问题。

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | `/docs/user/quick-start.md` | 如何注册账号、提交资源链接、设置标签偏好？ |
| 用户手册 | `/docs/user/faq.md` | 链接失效如何处理、投票规则是什么、账号被锁定怎么办？ |
| 运维指南 | `/docs/ops/deployment.md` | 如何使用 Docker Compose 一键部署生产环境？ |
| 运维指南 | `/docs/ops/monitoring.md` | 如何配置链接健康检查的阈值与告警规则？ |
| 开发文档 | `/docs/dev/architecture.md` | 项目整体架构图、模块划分与数据流向是怎样的？ |
| 开发文档 | `/docs/dev/contribution.md` | 代码风格规范、commit message 格式、PR 流程细则 |
| API 参考 | `/docs/api/endpoints.md` | 所有 RESTful 接口的请求参数、返回值示例与错误码说明 |
| API 参考 | `/docs/api/rate-limit.md` | 接口调用频率限制策略与白名单申请方式 |

## 资源列表

本项目的核心价值在于收录高质量的外部资源。以下链接由社区成员提交并经管理员审核通过，按类别分组展示。每个 URL 均保持用户提交时的原始格式，未做任何协议补全或域名改写。

### 日志与趋势分析类

- <code>rizhiliantuijian.asia</code>
- <code>rizhilianqianzhan.asia</code>
- <code>rizhilianfenxi.asia</code>

### 实时数据与比分类

- <code>rizhiliansheshoubang.asia</code>
- <code>rizhiliansaicheng.asia</code>

### 积分与排行榜类

- <code>rizhilianjishibifen.asia</code>
- <code>rizhilianjifenbang.asia</code>

## 项目结构

项目采用前后端分离的 Monorepo 结构，后端基于 NestJS，前端基于 Next.js。以下为源码目录树的简化示意，包含主要子目录与功能注释。

```
rizhilink-hub/
├── apps/                                 # 应用程序入口
│   ├── backend/                          # NestJS 后端服务
│   │   ├── src/
│   │   │   ├── modules/                  # 功能模块（资源、用户、健康检查、统计）
│   │   │   ├── common/                   # 公共守卫、拦截器、管道
│   │   │   └── migrations/               # 数据库迁移脚本
│   │   └── package.json
│   └── frontend/                         # Next.js 前端页面
│       ├── src/
│       │   ├── pages/                    # 路由页面（首页、分类、提交、仪表盘）
│       │   ├── components/               # 可复用 UI 组件（卡片、筛选栏、图表）
│       │   └── hooks/                    # 自定义 React Hooks（数据请求、主题切换）
│       └── next.config.js
├── packages/                             # 共享库与工具
│   ├── types/                            # 全局 TypeScript 类型定义（资源实体、API 响应）
│   ├── utils/                            # 通用工具函数（日期格式化、URL 校验）
│   └── config/                           # 环境变量解析与默认配置
├── docs/                                 # 文档目录（见上文“文档导航”）
├── scripts/                              # 运维脚本（备份、健康检查、数据导入导出）
│   ├── health-check.sh                   # 批量检查收录链接可用性
│   └── seed-db.js                        # 初始化种子数据
├── docker-compose.yml                    # 本地开发与生产部署的容器编排文件
├── Dockerfile.backend                    # 后端服务的镜像构建文件
├── Dockerfile.frontend                   # 前端服务的镜像构建文件
└── README.md                             # 当前文件
```

## 贡献指南

我们欢迎所有形式的贡献，包括但不限于提交新资源链接、报告链接失效、改进文档、修复 Bug 以及新增功能特性。请遵循以下步骤参与项目：

1. **Fork 仓库并创建功能分支**：从主仓库 Fork 代码到个人账号，然后基于 `main` 分支创建以 `feat/` 或 `fix/` 为前缀的新分支，例如 `feat/add-log-trend-resource`。

2. **本地开发与自测**：按照“快速开始”章节启动开发环境，确保代码风格符合 ESLint 与 Prettier 配置，并编写必要的单元测试（Jest）覆盖新增逻辑。

3. **提交 Pull Request**：推送分支到个人 Fork 仓库后，在 GitHub 上向主仓库的 `main` 分支发起 PR。PR 描述需清晰说明改动目的、关联 Issue 编号以及测试结论。

4. **等待 Code Review**：至少一位项目维护者会审核 PR，可能会提出修改意见。请及时响应并更新代码。通过审核后，将由维护者合并进主分支。

5. **更新文档与示例**：如果新增了 API 接口或修改了配置项，请同步更新 `/docs` 下的对应文档，并在 PR 中包含文档改动。

## 常见问题

**问：收录的链接访问失败或内容不符，我该如何报告？**

答：您可以在项目首页点击“报告链接”按钮，填写链接地址与异常描述。系统会自动生成一个 Issue 提交到 GitHub 仓库的 `issues` 板块，管理员会在 48 小时内核实并处理。您也可以直接在 GitHub 仓库中提交 Issue，选择“Broken Link”模板。

**问：我可以提交商业性质的网站或带推广参数的链接吗？**

答：原则上我们不排斥商业站点，但前提是站点内容与“日志分析”“数据趋势”“体育数据”“积分排行”等主题高度相关，且不包含恶意代码或诱导点击行为。带有明显 UTM 推广参数的链接会被系统自动清洗掉参数后再收录。如果发现推广痕迹过重，社区投票会将该链接标记为“待复审”，最终由管理员裁决是否移出收录库。

**问：项目会存储用户的浏览历史或外链点击记录吗？**

答：项目仅存储外链的累计点击次数与最近 7 天的日访问量（用于生成热度排行），这些数据是聚合统计信息，无法关联到具体用户。我们不使用任何第三方跟踪脚本，也不设置跨站 Cookie。详细的隐私保护策略请参阅 `/docs/user/privacy.md`。

## 许可证

MIT License

Copyright (c) 2026 RizhiLink Hub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
