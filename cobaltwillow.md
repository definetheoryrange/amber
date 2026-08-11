# BifenNav

BifenNav 是一个面向中文互联网技术从业者的导航型开源项目，专注于聚合与整理高频使用的技术文档、社区入口、数据查询工具及运营资源。项目定位为“技术外链的标准化索引库”，解决开发者在日常工作中频繁查找官方入口、分散收藏、链接失效等问题。BifenNav 不存储任何实质内容，仅提供结构化、可审计的外部资源引用，适用于个人开发者、技术团队及企业内网门户的快速搭建与定制。

BifenNav 以纯静态 Markdown 和 JSON 数据驱动，支持一键导出为浏览器书签、HTML 导航页或 JSON API 数据源。项目本身不依赖数据库，所有资源链接以明文清单形式维护，便于版本控制、协作审阅和自动化检测。通过定期健康检查脚本，BifenNav 能够标记失效链接，确保索引质量。

## 功能概览

- **结构化资源索引**：按照技术栈、服务类型、使用频率等维度对 URL 进行分类，提供多级目录视图。
- **一键书签导出**：通过内置脚本将资源列表转换为浏览器书签 HTML 文件，支持 Chrome、Firefox 和 Edge 导入。
- **链接健康检测**：集成基于 curl 的自动化检测工具，每日检查资源可达性，输出异常报告。
- **只读数据模型**：所有资源以 YAML 或 JSON 文件存储，不涉及用户输入，避免 XSS 与注入风险。
- **快速模糊搜索**：前端静态页面提供基于 Fuse.js 的本地搜索，支持按域名、关键词和分类查找。
- **多端适配视图**：响应式导航面板，适配桌面平板与手机，支持明暗主题切换。
- **变更审计日志**：每次资源更新均记录提交哈希、时间和变更说明，便于回溯。
- **离线备份支持**：提供资源列表的 CSV 和 JSON 导出，便于本地归档或二次开发。

## 应用场景

1. **团队内部导航门户**  
   技术团队可将 BifenNav 部署为内部开发门户，统一存放 CI/CD 系统、监控面板、日志平台、代码仓库等日常入口，避免每个成员自行收藏造成混乱。

2. **个人开发环境起始页**  
   开发者可克隆本项目，删减或增补个人常用的 API 文档、技术社区、在线工具等链接，搭配浏览器新标签页插件使用，提升每日工作效率。

3. **开源文档站的外链附录**  
   技术书籍、博客或开源项目文档站可使用 BifenNav 作为“相关资源”附录，以可维护的清单形式提供外部引用，降低文档中的外链维护成本。

4. **运维监控仪表盘补充**  
   运维人员可将 BifenNav 与监控系统集成，将业务后台、告警平台、数据库管理界面等链接集中展示，配合健康检测及时感知管理入口的可用性。

5. **培训与 onboarding 物料**  
   新员工入职时，BifenNav 可作为技术资源地图，帮助新人在短时间内了解公司常用的技术栈文档、内部工具和协作平台入口。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，需提前安装 Git 和 Node.js 16+。

```bash
# 1. 克隆仓库
git clone https://github.com/your-org/bifennav.git
cd bifennav

# 2. 安装依赖（用于本地预览和检测工具）
npm install

# 3. 生成静态导航页面并启动预览服务
npm run build
npm run serve
```

执行完成后，访问 <code>http://localhost:3000</code> 即可查看导航页。资源列表位于 `data/resources.json`，可直接编辑该文件增删链接。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | 16.x 或更高 | 用于运行构建脚本、链接检测和预览服务 |
| npm | 8.x 或更高 | 包管理器，用于安装项目依赖 |
| Git | 2.25+ | 用于克隆仓库和版本管理 |
| curl | 7.68+ | 链接健康检测脚本依赖的命令行工具 |
| bash | 4.0+ | 运行自动化检测脚本的 shell 环境 |
| 浏览器 | 现代版本 | 仅用于预览导航页面，非运行必需 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `docs/user-guide.md` | 如何使用导航页、搜索、导出书签、切换主题？ |
| 维护指南 | `docs/maintainer-guide.md` | 如何增删链接、运行健康检测、处理失效资源？ |
| 数据格式 | `docs/data-format.md` | 资源 JSON 的字段定义、分类标签规范和示例。 |
| API 参考 | `docs/api-reference.md` | 静态 JSON API 端点、查询参数和返回结构。 |
| 部署指南 | `docs/deployment.md` | 如何将导航页部署到 Nginx、Vercel 或 Docker 容器。 |
| 贡献规范 | `CONTRIBUTING.md` | 提交资源更新的流程、Commit Message 格式和 PR 要求。 |

## 资源列表

### 官方与主站

<code>bifenwangjiebao.org.cn</code>

<code>bifenwang500gw.org.cn</code>

<code>bifenguanwang.cn</code>

<code>bifenguanfang.org.cn</code>

<code>bifenguanwang.com.cn</code>

<code>bifenqiutangw.org.cn</code>

<code>bifenleisu.org.cn</code>

## 项目结构

```
bifennav/
├── data/                         # 核心数据目录
│   ├── resources.json            # 主资源列表（分类、URL、标签）
│   ├── categories.json           # 分类元数据（图标、显示顺序）
│   └── health-report.json        # 链接健康检测结果（自动生成）
├── scripts/                      # 工具脚本
│   ├── check-links.sh            # curl 批量检测可达性
│   ├── export-bookmarks.js       # 生成浏览器书签 HTML
│   └── validate-schema.js        # 校验 resources.json 格式
├── src/                          # 前端源码
│   ├── index.html                # 导航页主模板
│   ├── assets/                   # CSS、JS、字体文件
│   ├── components/               # 可复用 UI 组件
│   └── search/                   # 本地搜索索引生成脚本
├── docs/                         # 项目文档
│   ├── user-guide.md             # 用户操作手册
│   ├── maintainer-guide.md       # 维护者操作流程
│   ├── data-format.md            # 资源数据格式详解
│   ├── api-reference.md          # 静态 API 接口说明
│   └── deployment.md             # 生产环境部署方案
├── tests/                        # 单元测试与集成测试
│   ├── schema.test.js            # 数据格式校验测试
│   └── link-format.test.js       # URL 格式合规测试
├── .github/                      # GitHub 工作流
│   ├── workflows/                # CI 流水线（自动检测、构建）
│   └── ISSUE_TEMPLATE/           # 资源增删改的问题模板
├── package.json                  # npm 依赖与脚本入口
├── README.md                     # 项目总览（本文档）
└── LICENSE                       # MIT 许可证
```

## 贡献指南

1. **提交资源增删改请求**  
   请在 GitHub Issues 中使用“资源变更”模板，填写完整 URL、分类建议和变更理由。若为失效链接，请附上检测时间或错误截图。

2. **本地验证数据格式**  
   Fork 仓库后，在 `data/resources.json` 中按格式修改，然后运行 `npm run validate` 确保 JSON 结构合法。新增的 URL 必须包含 `https://` 或 `http://` 协议头。

3. **运行链接健康检测**  
   提交 PR 前，请执行 `bash scripts/check-links.sh` 检测所有外链可达性。若新增链接返回非 2xx/3xx 状态码，需要先确认其可用性。

4. **编写清晰的 Commit Message**  
   使用 `<type>(<scope>): <subject>` 格式，例如 `feat(resources): add <code>bifenguanwang.cn</code> to gateway category` 或 `fix(links): update expired <code>bifenwangjiebao.org.cn</code> entry`。

5. **提交 Pull Request**  
   PR 描述中请注明关联 Issue 编号，并附上本地验证结果截图或日志。PR 需要至少一名维护者审阅，通过所有 CI 检查后方可合并。

## 常见问题

**Q：我可以将 BifenNav 用于商业项目吗？**  
A：可以。BifenNav 采用 MIT 许可证，允许自由使用、修改、分发和商业化，仅需保留原版权声明。详细条款请参阅 LICENSE 文件。

**Q：资源列表中的链接失效了怎么办？**  
A：您可以通过 Issue 报告失效链接，或自行 Fork 仓库后删除/更新相应条目并提交 PR。项目本身也提供 `check-links.sh` 脚本，您可定期运行以主动发现异常。

**Q：如何自定义分类或添加新分类？**  
A：编辑 `data/categories.json` 增加分类定义（id、名称、图标），然后在 `data/resources.json` 中为资源指定对应的 `categoryId` 即可。前端构建时会自动聚合新分类。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:16
