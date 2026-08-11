# CloudStream Resource Catalog

CloudStream Resource Catalog 是一个面向流媒体技术研究者与内容聚合平台开发者的轻量级外链资源索引系统。该项目定位于构建高可用、可扩展的媒体资源导航中间层，通过结构化元数据管理与标准化输出协议，帮助开发者快速集成第三方影视、动画、综艺及纪录片等类型的播放源与信息页。项目本身不存储、不分发、不转码任何媒体文件，仅提供公开互联网资源的超链接整理与分类展示，适用于个人学习、内部工具链搭建或非商业用途的实验性项目。

目标用户包括流媒体技术爱好者、自建媒体聚合服务的后端工程师、内容推荐算法研究人员以及需要批量管理播放源链接的数据运维人员。通过本项目的索引机制，用户可在合规前提下高效获取分散于不同域名的公开视频资源入口，降低手工收集成本，提升资源调度效率。

## 功能概览

- **多源资源聚合索引** 提供覆盖电影、电视剧、动漫、综艺及综合娱乐等品类的公开播放页链接，按站点来源与内容类型双重分类。

- **结构化元数据导出** 所有收录链接附带域名特征、内容语言、更新频率标签，支持 JSON、YAML 及纯文本列表三种导出格式。

- **定时健康检查接口** 内置简易 HTTP 探活模块，可周期性检测每个外链域名的可达性，并生成不可用站点报告。

- **标签过滤与搜索** 支持按站点归属地、内容题材、视频清晰度标识（如 1080p、4K）等自定义标签进行过滤查询。

- **黑白名单管理** 提供动态域名过滤机制，允许运维人员通过配置文件实时增删允许或禁止收录的域名模式。

- **增量更新日志** 每次资源新增、删除或状态变更均记录时间戳与操作类型，便于追溯与回滚。

- **对外 RESTful API** 暴露标准 HTTP 接口，支持分页获取全量资源、根据 ID 查询单个详情以及按标签批量拉取。

## 应用场景

1. **个人媒体导航站后端** 开发者可利用本项目的索引数据与 API，快速搭建属于自己的视频资源导航页面，避免重复手工整理数百个外链。

2. **内容聚合实验平台** 在研究推荐算法或用户偏好分析时，可将本项目的资源列表作为初始种子数据，结合播放量模拟或用户反馈进行排序实验。

3. **运维监控辅助工具** 通过内置健康检查功能，定期扫描外链可用性，自动生成失效站点清单，辅助运维人员及时清理或替换无效入口。

4. **内部团队资源共享** 小型制作团队或兴趣小组可将本项目部署在内网，作为成员间共享常用审片、素材参考链接的统一管理工具。

5. **教学演示案例** 适用于计算机相关课程中关于网络爬虫、数据清洗或 RESTful 接口设计的实践教学，提供真实可用的外链数据集。

## 快速开始

以下指令适用于 Linux / macOS 及 Windows WSL 环境，请确保已安装 Git 与 Node.js 18.x 及以上版本。

```bash
# 1. 克隆项目仓库
git clone https://github.com/cloudstream-resource-catalog/csrc.git
cd csrc

# 2. 安装项目依赖
npm install

# 3. 启动开发服务器（默认监听 3000 端口）
npm run start
```

启动成功后，访问 `http://localhost:3000/api/v1/resources` 可获取首批加载的资源列表。若需修改端口或数据源路径，请编辑 `config/default.yaml` 文件。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，用于执行核心索引服务与 API 网关 |
| npm | 9.x 或更高 | 包管理器，用于安装所有第三方库与工具链 |
| SQLite3 | 系统内置或自动编译 | 轻量级嵌入式数据库，存储资源元数据与健康记录 |
| git | 2.30+ | 版本控制工具，用于克隆仓库与拉取更新 |
| curl / wget | 任意稳定版 | 可选，用于手动测试 API 接口或健康检查脚本 |
| 操作系统 | Linux (glibc 2.28+), macOS 11+, Windows 10+ (WSL2) | 支持主流开发环境 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | `docs/getting-started.md` | 如何快速部署并获取第一条资源数据；初始配置项有哪些必填字段 |
| API 参考 | `docs/api-reference.md` | 每个接口的请求方法、路径参数、响应结构与状态码含义 |
| 运维手册 | `docs/operations.md` | 如何调整健康检查频率、如何备份数据库、如何迁移至生产环境 |
| 数据格式 | `docs/data-format.md` | 资源条目的完整字段定义、标签规范与扩展字段使用方法 |

## 资源列表

以下为当前版本已收录的全部外链资源，按内容类别分组展示。所有链接均取自公开互联网，仅供技术验证与学习参考。

### 综合影视资源

- <code>guochanjiatingyingyuan.org.cn</code>
- <code>guochanyirenwang.org.cn</code>

### 动漫与动画资源

- <code>mianfeidongmandaquan.org.cn</code>

### 字幕与语言辅助资源

- <code>zhongwenzimuzaixianguankan.org.cn</code>

### 在线播放与点播资源

- <code>zaixiannidongde.org.cn</code>
- <code>dianyingtiantangzaixianbofang.org.cn</code>

### 电视剧与综艺资源

- <code>zuixindianshijuzaixianguankan.org.cn</code>

## 项目结构

```
csrc/
├── src/
│   ├── core/                    # 核心索引引擎与元数据管理
│   │   ├── indexer.js           # 资源条目解析与规范化
│   │   └── validator.js         # URL 格式与域名合法性校验
│   ├── api/                     # RESTful 接口路由与控制器
│   │   ├── routes.js            # 路由注册与版本前缀
│   │   └── resources.controller.js  # 资源查询、过滤与分页逻辑
│   ├── health/                  # 健康检查定时任务与报告生成
│   │   ├── checker.js           # HTTP 探活与超时重试策略
│   │   └── reporter.js          # 可用性统计与告警触发器
│   ├── db/                      # 数据库初始化与查询适配层
│   │   ├── schema.sql           # SQLite 表结构定义
│   │   └── repository.js        # CRUD 操作封装与事务管理
│   └── utils/                   # 日志、配置读取与通用工具函数
│       ├── logger.js            # 分级日志输出与文件轮转
│       └── configLoader.js      # YAML/JSON 配置合并与环境变量覆盖
├── config/
│   ├── default.yaml             # 默认端口、数据库路径、健康检查间隔
│   └── production.yaml.example  # 生产环境覆盖配置示例
├── data/
│   └── seed.json                # 初始资源种子数据（首次启动时导入）
├── tests/                       # 单元测试与集成测试用例
│   ├── unit/
│   └── integration/
├── docs/                        # 完整文档 Markdown 源文件
├── package.json                 # npm 项目清单与脚本定义
├── README.md                    # 项目概览与快速入门（本文件）
└── LICENSE                      # MIT 许可证文本
```

## 贡献指南

我们欢迎任何形式的贡献，包括但不限于新增资源链接、改进健康检查策略、优化 API 响应速度以及完善文档。请遵循以下步骤参与协作：

1. **提交 Issue 讨论** 在发起拉取请求之前，请先在 GitHub Issues 中描述您希望修复的问题或新增的功能，避免重复劳动与方向偏离。建议附上初步实现思路或参考资料。

2. **拉取开发分支并本地验证** 从 `develop` 分支创建您的特性分支（如 `feature/add-japanese-anime-sites`），在本地完成代码编写与自测。确保运行 `npm run test` 通过所有已有测试用例，并为新逻辑补充对应测试。

3. **更新资源列表文件** 若贡献涉及新增外链，请同步修改 `data/seed.json` 或对应的增量更新脚本，并按照 `docs/data-format.md` 中定义的字段规范填写类别、标签与备注信息。

4. **撰写清晰的变更日志** 在拉取请求描述中逐条列出变更点，并说明测试覆盖情况。若影响现有 API 行为或配置格式，务必在文档中同步标注破坏性变更。

5. **等待代码审查与合并** 项目维护者会在 3 个工作日内审核您的提交，可能会提出修改建议。通过后您的更改将被合并至 `develop` 分支，并在下一个版本发布时进入 `main` 分支。

## 常见问题

**问：项目是否会存储或缓存任何第三方视频文件？**

答：不会。本项目的全部功能仅限于收集和展示公开可访问的超链接，不涉及任何文件下载、转码、代理或缓存行为。所有播放或访问操作均发生在用户浏览器与目标源站之间。

**问：如果某个收录的链接失效或内容变更，我应该如何处理？**

答：您可以通过提交 Issue 或直接发送邮件至维护团队报告失效链接。同时，项目内置的健康检查会每日自动扫描所有站点，并在日志中标记异常条目。运维人员也可手动调用 `POST /api/v1/health/check` 接口触发即时检测。

**问：我可以将本项目用于商业产品吗？**

答：本项目采用 MIT 许可证，允许自由使用、修改、分发和再许可，包括用于商业目的。但请注意，本项目中索引的第三方链接均由其各自所有者运营，您在使用这些外部资源时应遵守其对应的服务条款与法律法规。

## 许可证

MIT License

Copyright (c) 2026 CloudStream Resource Catalog Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:11
