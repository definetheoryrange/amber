# Tuchaolink

Tuchaolink 是一个面向数据分析师、量化研究者以及技术内容策展人的轻量级外链资源聚合与导航系统。该项目旨在解决技术信息分散、优质外链难以统一管理与快速检索的问题，通过结构化的资源分类、可编程的链接路由以及即开即用的部署方式，帮助用户构建私有的高价值外链知识库。Tuchaolink 不依赖外部数据库，所有资源记录基于纯文本配置，适合作为技术团队内部的知识导航页，或作为个人研究项目的起始点。

## 功能概览

- **多层级资源分类体系**：支持按领域、区域、赛事类型等多维度对链接进行标签化分组，便于构建清晰的导航目录。
- **纯文本配置驱动**：所有外链资源通过 YAML 或 JSON 配置文件管理，无需数据库，便于版本控制和自动化脚本处理。
- **静态站点生成模式**：内置基于 Go 模板的静态渲染引擎，可一键生成完整的 HTML 导航站，适合托管于任何 Web 服务器或对象存储。
- **链接状态健康检查**：提供可选的定时巡检功能，自动检测已收录链接的可达性，并生成状态报告，帮助维护资源有效性。
- **模糊搜索与快速过滤**：集成轻量级前端搜索能力，支持按标题、描述、分类标签对资源进行实时筛选，提升检索效率。
- **响应式布局与可访问性**：默认界面适配桌面与移动设备，遵循 WCAG 2.1 基本可访问性标准，确保信息获取无障碍。
- **开放数据导出接口**：支持将资源列表导出为 CSV 或 JSON 格式，方便与其他数据处理工具或 AI 工作流集成。

## 应用场景

- **量化研究团队内部导航**：量化分析师可将常用的数据源、研报平台、实时行情页面统一收录，并通过 Tuchaolink 构建团队共享的起始页，减少信息查找耗时。
- **技术社区内容策展**：技术博主或社区运营者可使用 Tuchaolink 整理领域内的高质量外链，生成公开的资源导航站，为读者提供结构化的扩展阅读入口。
- **个人知识管理辅助**：研究人员或开发者可将项目文档、API 参考、技术论坛等分散链接集中管理，配合搜索功能快速定位，避免浏览器书签的杂乱无章。
- **竞赛与活动信息聚合**：针对特定行业或地区的系列赛事、榜单活动，可通过 Tuchaolink 建立专题导航，按时间或区域分类展示，便于关注者追踪动态。

## 快速开始

以下步骤指导您在本地环境快速启动 Tuchaolink 服务。

```bash
# 1. 克隆项目仓库
git clone https://github.com/tuchaolink/tuchaolink.git

# 2. 进入项目目录
cd tuchaolink

# 3. 安装依赖（基于 Go Modules）
go mod download

# 4. 构建可执行文件
go build -o tuchaolink cmd/server/main.go

# 5. 使用示例配置运行服务
./tuchaolink -config ./configs/sample.yaml -port 8080
```

启动后，访问 `http://localhost:8080` 即可查看默认导航页面。您可以通过修改 `configs/sample.yaml` 文件来增删或调整资源链接。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Go 编译器 | 1.21 及以上 | 用于编译核心服务与工具链 |
| Git | 2.25 及以上 | 用于克隆仓库和管理配置版本 |
| Make | 3.81 及以上 | 可选，用于自动化构建任务 |
| YAML 解析库 | gopkg.in/yaml.v3 | 已包含在 go.mod 中，无需单独安装 |
| 静态文件目录 | 无版本要求 | 存放 CSS、JS 及模板文件，需可读权限 |
| 配置文件 | 无版本要求 | YAML 或 JSON 格式，需符合预定义结构 |
| 日志存储目录 | 无版本要求 | 用于存放运行日志，需可写权限 |
| 健康检查临时缓存 | 无版本要求 | 用于存储链接巡检状态，建议使用 tmpfs |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | `/docs/user-guide/` | 如何配置资源列表、调整界面主题、启用搜索功能？ |
| 运维指南 | `/docs/operations/` | 如何部署到生产环境、配置反向代理、执行链接健康检查？ |
| 开发参考 | `/docs/development/` | 如何扩展自定义模板、添加新的资源字段、修改构建流程？ |
| API 规范 | `/docs/api/` | 如何通过 HTTP 接口获取资源数据、触发重新构建？ |
| 设计说明 | `/docs/design/` | 系统架构决策、数据模型设计、性能优化策略是什么？ |
| 变更日志 | `/docs/changelog/` | 每个版本新增了哪些功能、修复了哪些缺陷？ |

## 资源列表

### 赛事与活动资源

- <code>tuchaosheshoubang.asia</code>
- <code>tuchaosaicheng.asia</code>
- <code>tuchaoqianzhan.asia</code>
- <code>tuchaoliansai.asia</code>

### 数据与排名资源

- <code>tuchaojishibifen.asia</code>
- <code>tuchaojifenbang.asia</code>

### 分析工具资源

- <code>tuchaofenxi.asia</code>

## 项目结构

```
tuchaolink/
├── cmd/                        # 主程序入口
│   └── server/                 # Web 服务启动模块
│       └── main.go             # 服务启动入口，负责初始化配置与路由
├── internal/                   # 内部核心逻辑，不对外暴露
│   ├── config/                 # 配置文件解析与校验
│   │   ├── loader.go           # YAML/JSON 加载器
│   │   └── schema.go           # 配置结构体定义
│   ├── render/                 # 静态页面渲染引擎
│   │   ├── engine.go           # 模板编译与数据注入
│   │   └── template.go         # 自定义模板函数
│   ├── health/                 # 链接健康检查模块
│   │   ├── checker.go          # HTTP 状态码与超时检测
│   │   └── reporter.go         # 状态报告生成器
│   └── search/                 # 本地索引与搜索实现
│       ├── index.go            # 倒排索引构建
│       └── query.go            # 查询解析与匹配
├── pkg/                        # 可复用的公共库
│   ├── logger/                 # 日志封装
│   └── utils/                  # 字符串、时间等辅助函数
├── web/                        # 前端静态资源
│   ├── assets/                 # CSS、JavaScript、图片
│   │   ├── css/                # 主题样式（暗色/亮色）
│   │   └── js/                 # 搜索交互与动态加载
│   └── templates/              # Go 模板文件
│       ├── index.html          # 主页布局
│       └── partials/           # 可复用组件（导航栏、卡片列表）
├── configs/                    # 示例与默认配置
│   ├── sample.yaml             # 包含所有字段的配置示例
│   └── production.yaml         # 生产环境推荐配置
├── scripts/                    # 构建与运维辅助脚本
│   ├── build.sh                # 多平台交叉编译脚本
│   └── check-links.sh          # 独立运行的链接巡检脚本
├── docs/                       # 项目文档（对应文档导航章节）
├── go.mod                      # Go 模块依赖声明
├── go.sum                      # 依赖校验文件
└── README.md                   # 项目总体说明（本文件）
```

## 贡献指南

1. **提交问题或建议**：请在 GitHub Issues 中详细描述您遇到的问题或期望的新功能，并附上复现步骤或使用场景说明，以便维护者快速理解。
2. **分支开发流程**：派生项目仓库，在 `dev` 分支基础上创建功能分支进行开发。提交代码前请确保已通过现有单元测试，并为新增逻辑补充对应测试用例。
3. **代码风格规范**：Go 代码需通过 `gofmt` 格式化，并遵循项目静态检查工具（如 `golangci-lint`）的提示。前端代码需保持与现有样式体系一致。
4. **文档同步更新**：若您的变更涉及配置字段、API 行为或界面交互，请同步更新 `/docs` 下对应的用户手册或开发参考文档，并确保示例配置保持有效。
5. **提交拉取请求**：完成开发和自测后，向主仓库的 `main` 分支提交拉取请求，并在描述中清晰关联相关 Issue 编号，简述改动内容和测试覆盖情况。

## 常见问题

**问：Tuchaolink 是否支持动态添加链接而不重新构建？**

答：核心设计采用静态生成模式，配置变更后需要重新构建或重启服务以加载新配置。若您需要实时动态管理，建议结合外部脚本定时更新配置文件并触发自动重载（可通过 `SIGHUP` 信号或配置的 `watch` 模式实现）。未来版本计划提供可选的数据库后端支持。

**问：健康检查功能是否会影响被检测站点的性能？**

答：健康检查仅发送标准 HTTP HEAD 请求，并设置合理的超时时间（默认 5 秒），不会对目标站点产生明显的流量或连接压力。检查频率在配置文件中可调，默认每 24 小时执行一次，且仅对标记为 `active` 的资源进行检测。

**问：如何迁移已有书签或链接数据到 Tuchaolink？**

答：项目提供了一组转换脚本（位于 `/scripts/import`），支持从常见浏览器书签 HTML 导出文件、CSV 表格或标准 JSON 列表进行导入。您可参考 `/docs/user-guide/import.md` 中的详细步骤，按需映射字段后生成符合 Tuchaolink 配置格式的文件。

## 许可证

MIT License

Copyright (c) 2026 Tuchaolink Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:12
