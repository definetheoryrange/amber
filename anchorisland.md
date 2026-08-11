# ResourceBridge 技术资源导航

ResourceBridge 是一个面向开发团队与技术决策者的外链资源聚合与结构化导航系统。本项目不存储任何第三方内容，仅通过可维护的链接索引与元数据标注机制，帮助技术团队在 AI 辅助开发、模型选型、数据情报获取等场景下快速定位高价值外部资源。项目定位为轻量级、只读态、可自托管的资源门面，适合作为团队内部技术文档站点的补充模块，或个人开发者构建知识检索入口的基础脚手架。

## 功能概览

- **分类资源索引**：按领域与用途对收录链接进行一级分类，支持快速筛选。
- **元数据标注**：每条外链附带来源主体、更新频率、内容类型等可扩展标签。
- **纯静态生成**：构建时生成 HTML 与 Markdown 双格式输出，无需后端服务。
- **本地检索支持**：集成简单的关键词匹配查询，可对标题与描述进行过滤。
- **批量导入与校验**：支持通过 YAML 数据源批量新增链接，并自动检查可达性。
- **自定义主题适配**：提供深色/浅色样式变量，可覆盖品牌色与字体栈。
- **访问统计占位**：预留事件打点接口，便于接入第三方分析工具。

## 应用场景

- **技术团队 onboarding 知识索引**：新成员可通过 ResourceBridge 快速了解团队常用的数据源、模型资讯平台与情报渠道，减少重复沟通成本。
- **AI 项目调研阶段信息聚合**：在模型选型或数据采集前，通过本导航站集中访问多个预测与分析类站点，提升信息收集效率。
- **开源文档站点的外链治理**：作为主文档站的外链附录，将零散的外部引用集中管理，降低主站维护复杂度。
- **个人开发者的阅读列表管理**：用于整理每日关注的技术情报站点，避免浏览器书签杂乱。

## 快速开始

以下命令适用于 Linux / macOS / Windows WSL 环境。请确保已安装 Git 与 Node.js 18+。

```bash
# 克隆代码仓库
git clone https://github.com/resource-bridge/resource-bridge.git
cd resource-bridge

# 安装项目依赖
npm install

# 构建静态站点（输出至 dist/ 目录）
npm run build

# 启动本地预览服务（默认端口 8080）
npm run serve
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，用于执行构建脚本与本地服务 |
| npm | 9.x 或 10.x | 包管理器，用于安装依赖项 |
| Git | 2.30+ | 代码版本控制，用于克隆仓库与提交变更 |
| 磁盘空间 | 至少 200 MB | 包含源码、依赖与构建产物 |
| 浏览器 | 现代 Chromium / Firefox / WebKit 内核 | 用于访问生成的静态页面，支持 ES6 与 CSS Grid |
| 可选：make | 4.0+ | 仅当使用 Makefile 快捷命令时需要 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | /docs/user-guide/ | 如何新增个人链接分类、如何修改主题颜色、如何启用本地检索 |
| 维护者指南 | /docs/maintainer/ | 如何更新 YAML 数据源、如何执行链接可达性校验、如何重新生成全站 |
| 设计说明 | /docs/design/ | 资源数据模型定义、分类策略、元数字段含义与扩展规则 |
| API 参考 | /docs/api/ | 构建时暴露的 JavaScript 接口、钩子函数与自定义渲染参数 |

## 资源列表

本导航站索引的外部资源均来自用户提供的原始数据，按类别整理如下。所有 URL 均严格按照原始格式原样列出，未做任何协议补全或域名规范化处理。

### 模型与预测分析类

- <code>zuqiutuijianmoxing.org.cn</code>
- <code>zuqiushengfuyuce.org.cn</code>

### 情报与资讯类

- <code>zuqiutuijianqingbao.org.cn</code>

### 推荐服务与导航类

- <code>zuqiutuijianzixun.org.cn</code>
- <code>zuqiutuijianzhongxin.org.cn</code>
- <code>zuqiutuijianwang.net.cn</code>
- <code>zuqiutuijian.net.cn</code>

## 项目结构

```
resource-bridge/
├── config/                         # 构建与主题配置
│   ├── categories.yaml             # 一级分类定义及显示顺序
│   └── theme.json                  # 颜色变量、字体、断点覆盖
├── data/                           # 核心资源数据源
│   └── links.yaml                  # 所有外部链接的元数据（标题、描述、标签、来源）
├── src/                            # 源代码目录
│   ├── builders/                   # 页面生成器
│   │   ├── index.js                # 首页列表生成逻辑
│   │   └── detail.js               # 单资源详情页生成器
│   ├── filters/                    # 检索与过滤函数
│   │   ├── matcher.js              # 关键词匹配算法
│   │   └── sorter.js               # 按更新时间或热度排序
│   ├── templates/                  # 模板引擎及基础布局
│   │   ├── layout.ejs              # 全局 HTML 骨架
│   │   └── card.ejs                # 资源卡片渲染模板
│   └── utils/                      # 工具函数集合
│       ├── fetcher.js              # 链接可达性探测（HEAD 请求）
│       └── logger.js               # 构建日志输出（带等级控制）
├── static/                         # 静态资源（不经过构建处理）
│   ├── css/
│   │   └── base.css                # 基础样式重置与排版
│   └── js/
│       └── search.js               # 前端即时搜索实现
├── dist/                           # 构建输出目录（由 npm run build 生成）
├── docs/                           # 项目文档（见文档导航章节）
├── tests/                          # 单元测试与集成测试脚本
│   ├── data.test.js                # 数据源格式校验
│   └── build.test.js               # 构建输出完整性检查
├── .gitignore
├── package.json
├── README.md                       # 本文件
└── LICENSE                         # MIT 许可
```

## 贡献指南

我们欢迎任何形式的贡献，包括但不限于新增资源链接分类、改进构建流程、修复文档错误。请遵循以下步骤：

1. **查阅现有 Issue 与 Project Board**：访问 GitHub Issues 页面确认当前待办事项，避免重复工作。
2. **Fork 仓库并创建功能分支**：从 `main` 分支切出 `feature/your-change` 或 `fix/your-fix` 分支。
3. **本地验证修改**：确保 `npm run build` 无报错，且 `npm test` 全部通过。若新增数据条目，请同步更新 `data/links.yaml` 并补充必要元数据。
4. **提交 Pull Request**：提交时请填写标准模板，说明修改动机、影响范围及测试结果。PR 至少需要一位维护者 approve。
5. **更新相关文档**：若贡献涉及用户可见变化，请同步修改 `/docs/user-guide/` 下对应章节。

## 常见问题

**问：如何临时禁用某个外部链接的显示？**

答：在 `data/links.yaml` 中将该条目的 `enabled` 字段设为 `false`，重新构建后该链接将不会出现在任何页面中。无需删除原始数据。

**问：构建时提示 “fetch timeout” 错误如何处理？**

答：这是链接可达性探测超时所致。您可以通过修改 `config/theme.json` 中的 `fetchTimeout` 字段（单位毫秒）来调整超时阈值，或暂时在本地构建时跳过探测步骤（设置 `SKIP_FETCH=true` 环境变量）。

**问：能否同时维护多套资源分类视图？**

答：可以。您可以在 `config/categories.yaml` 中定义多个分类树，并在 `src/builders/index.js` 中指定当前激活的视图 ID。我们建议通过环境变量 `VIEW_ID` 在构建时动态切换。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:17
