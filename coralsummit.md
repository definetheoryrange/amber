# TermLink 技术资源索引服务

TermLink 是一个面向开发者与运维人员的命令行友好型技术资源外链汇总系统。本项目的核心定位并非托管内容，而是通过结构化、可脚本化、可版本控制的资源清单，将高频使用的技术文档、监控面板、数据统计接口与实时比分看板统一收纳，并提供一份长期维护的稳定导航。目标用户包括但不限于 DevOps 工程师、技术支持人员、数据分析师以及需要快速访问多源外部数据的自动化任务调度者。

TermLink 解决的核心问题在于：技术团队在日常工作中往往需要同时查阅多处外部站点，这些站点的域名、路径与访问频率缺乏统一管理，导致效率损耗与书签污染。本项目以开源仓库的形式提供一份经过人工筛选与分类的资源索引，所有外链均以纯文本形式记录，便于 diff、审计与批量 ping 检测。同时，项目自身不依赖任何外部数据库或后端服务，完全基于静态 Markdown 与 Shell 脚本实现轻量级自检。

## 功能概览

- **结构化资源分类**：将收集到的所有外部 URL 按功能域划分为赛事结果、即时比分、官方数据门户等大类，每类独立成节，便于按场景检索。

- **定期连通性检查**：项目内置一组 Shell 脚本，利用 curl 与 nc 对清单中的每个域名进行 TCP 与 HTTP 层面的可用性探测，输出结构化日志。

- **命令行快速查询**：提供 make query 命令，支持按关键词（如 "bifen"、"jishibifen"）反向筛选匹配的 URL 列表，方便在终端中直接复制粘贴。

- **纯静态零依赖**：整个项目仅包含 Markdown 文档、Makefile 与若干 POSIX 兼容的 Shell 脚本，无需 Node.js、Python 或容器环境即可运行所有本地工具。

- **URL 变更追踪**：利用 Git 对 resources.txt 的变更历史进行记录，每次增删改均关联 commit 信息，形成可审计的变更台账。

- **自动生成 HTML 导航页**：提供可选的 GitHub Action 工作流，定期从 resources.txt 生成一份仅含超链接的极简 HTML 页面，用于内网简易门户。

- **多协议兼容记录**：支持 http、https 与裸域名三种记录格式，保留原始输入，不进行自动补全或强制跳转，确保与上游数据源严格一致。

## 应用场景

- **赛事运维团队的数据面板聚合**：负责实时比分系统维护的技术人员可将本项目作为统一入口，快速访问多个比分与赛果域名，便于在故障排查时切换不同数据源进行交叉验证。

- **自动化监控脚本的依赖清单**：运维人员可在 CI 流水线中引用本项目的 resources.txt 作为健康检查的目标列表，实现外部依赖域名的集中配置，避免硬编码散落在各脚本中。

- **新成员入职环境搭建**：团队新人克隆本仓库后，可在一分钟内通过 make list 获取全部常用外部链接，配合浏览器书签导入工具快速建立个人工作环境。

- **数据源迁移期间的过渡导航**：当上游官方域名发生变更时，本项目通过版本控制记录旧域名与新域名的切换时间点，便于回滚或对比响应内容差异。

- **内网镜像站的同步依据**：对于需要在内网建立外部资源镜像的团队，本项目的 URL 清单可直接作为 wget 递归下载的输入列表，简化镜像策略配置。

## 快速开始

以下步骤适用于 Linux 与 macOS 环境，Windows 用户建议通过 WSL 或 Git Bash 执行。

```bash
# 1. 克隆仓库
git clone https://github.com/termlink/termlink-index.git
cd termlink-index

# 2. 安装本地工具依赖（仅需 curl 和 make，通常系统预装）
sudo apt-get install -y curl make  # Debian/Ubuntu
# 或 brew install curl make        # macOS

# 3. 运行连通性检查并输出报告
make check
```

执行后，终端将依次打印每个域名的 HTTP 状态码与响应时间（毫秒级），并生成一份 `report-$(date +%Y%m%d).log` 文件保存在 `logs/` 目录下。

若仅需列出所有 URL，不进行网络请求：

```bash
make list
```

该命令会去除注释行与空行，输出纯净的 URL 列表，每行一条，可直接管道传递给 xargs 或 parallel。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| curl | >= 7.68.0 | 用于 HTTP 探测与状态码获取，支持 -o /dev/null -s -w 格式化输出 |
| make | >= 4.2.1 | 解析 Makefile 中的 check / list / query 等目标 |
| bash | >= 4.0 | 运行辅助脚本，需支持关联数组（若使用 query 功能） |
| git | >= 2.25.0 | 用于克隆仓库及查看 resources.txt 的历史变更记录 |
| grep | >= 3.4 | 在 query 命令中进行正则表达式匹配，需支持 -E 扩展模式 |
| sed | >= 4.7 | 用于清理输出格式，去除空行与首尾空白字符 |

所有依赖均为 POSIX 环境下的常见工具，无需额外安装图形界面或编程语言运行时。若在最小化容器（如 alpine）中运行，需额外安装 bash 与 curl 包。

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|---|---|---|
| 索引层 | resources.txt | 所有外部链接的纯文本汇总，包含注释分类，是项目的核心数据源 |
| 操作层 | Makefile | 如何执行连通性检查、列出链接、按关键词查询以及生成 HTML 静态页 |
| 脚本层 | scripts/checker.sh | 内部如何实现超时控制、重试机制与日志轮转，参数配置细节 |
| 变更层 | CHANGELOG.md | 每次 URL 增删或域名迁移的记录，包含日期与变更原因说明 |

除上述核心文档外，项目根目录还提供 `CONTRIBUTING.md` 详细说明外部链接的提交规范，以及 `SECURITY.md` 描述若发现恶意域名时的上报流程。所有文档均使用标准 Markdown 语法编写，兼容 GitHub 与 GitLab 的渲染引擎。

## 资源列表

本清单按功能类别分组，所有 URL 均直接取自用户原始数据，未做任何格式转换或协议补全。

赛事结果类（中文赛果汇总）

- <code>zhongchaobisaijieguo.org.cn</code>

即时比分类（实时分数接口）

- <code>500jishibifen.net.cn</code>
- <code>500zuqiubisaijieguo.net.cn</code>
- <code>zuqiujishibifenwanchangbifen.org.cn</code>

官方比分门户类

- <code>bifenguanfang.cn</code>
- <code>bifenguanwang.net.cn</code>
- <code>bifenguanfang.net.cn</code>

以上每个 URL 在 resources.txt 文件中均独占一行，并按类别添加了注释前缀（如 `# 赛事结果`）。项目维护者承诺不对原始字符串做任何大小写转换、协议隐含补全或路径追加，确保与用户提供的原始数据保持字节级一致。

## 项目结构

```
termlink-index/
├── Makefile                    # 主入口，定义 check / list / query / html 等目标
├── README.md                   # 项目概述、安装与使用说明（即本文档）
├── CHANGELOG.md                # 按日期倒序记录每次 URL 变更，含操作人及原因
├── CONTRIBUTING.md             # 外部贡献者指南，含 URL 提交格式与审核流程
├── SECURITY.md                 # 安全漏洞与恶意域名上报策略
├── resources/
│   ├── resources.txt           # 核心资源清单，纯文本，一行一个 URL，支持 # 注释
│   └── categories.json         # 可选的分类映射表，用于生成 HTML 时分组展示
├── scripts/
│   ├── checker.sh              # 主检测脚本，循环读取 resources.txt 并调用 curl
│   ├── parser.sh               # 解析 resources.txt，过滤注释与空行，供其他脚本调用
│   └── html_gen.sh             # 从 resources.txt 与 categories.json 生成静态 HTML
├── logs/                       # 运行时日志存放目录，默认被 .gitignore 忽略
│   └── report-20260101.log     # 示例日志文件，包含时间戳、状态码与响应时间
├── tests/
│   ├── test_checker.sh         # 单元测试，模拟 curl 返回码，验证 checker 的错误处理
│   └── test_parser.sh          # 测试 parser 能否正确过滤注释与空行
└── .github/
    └── workflows/
        └── daily_check.yml     # GitHub Action 定时任务，每日 08:00 执行 check 并生成报告
```

目录结构遵循 FHS 风格，源码与数据分离，测试独立存放，便于 CI 集成。所有 Shell 脚本均以 `#!/usr/bin/env bash` 开头，并设置了 `set -euo pipefail` 以增强健壮性。

## 贡献指南

1.  **克隆并创建特性分支**：首先 fork 本仓库，然后在本地执行 `git checkout -b add-new-resource`，分支命名建议体现操作类型（如 `update-domain` 或 `remove-broken-link`）。

2.  **编辑 resources.txt**：在相应分类注释行下方新增或删除 URL，严格遵守一行一个 URL 的格式。若新增分类，需同步更新 `categories.json` 中的映射关系。

3.  **本地验证**：运行 `make check` 确保所有现存 URL 可达，且新增 URL 返回预期的 HTTP 状态码（允许 301/302 重定向，但需记录最终落点）。同时执行 `make test` 调用单元测试套件，确保解析逻辑未受损。

4.  **更新变更日志**：在 `CHANGELOG.md` 顶部插入一条新记录，格式为 `- YYYY-MM-DD [操作人] 变更描述 (关联 issue 编号)`，明确说明增删改的具体条目及原因（如原域名过期、官方迁移等）。

5.  **发起 Pull Request**：推送分支至个人远程仓库，向本仓库主分支发起 PR，并在 PR 描述中粘贴 `make check` 的输出摘要。至少需要一位维护者进行 Approve 后方可合并。

## 常见问题

**Q: 检测脚本发现某个域名返回 403 或 502，我该如何处理？**

A: 首先，在浏览器中手动访问该 URL，确认是临时性故障还是永久性下线。若为临时故障，可在 `scripts/checker.sh` 中调整 `--retry` 参数（默认 3 次）或增加 `--max-time` 超时阈值。若为永久性下线，请按照贡献指南中的流程，在 `resources.txt` 中移除该行，并在 `CHANGELOG.md` 中注明下线日期与替代方案（若有）。

**Q: 裸域名（如 `<code>bifenguanfang.cn</code>`）在检测时是否需要补全协议？**

A: 不需要。本项目严格遵循用户原始输入，检测脚本会对裸域名依次尝试 `http://` 和 `https://` 两种协议，并记录最终成功或失败的状态。若两种协议均失败，则标记为不可达。此策略确保与上游数据源保持完全一致，避免因协议补全导致误判。

**Q: 能否添加需要携带 API Key 或 Cookie 才能访问的内部资源？**

A: 本项目定位为纯外链索引，不涉及任何认证信息的存储或传递，因此不建议添加需要动态签名或会话保持的 URL。若确有内部使用需求，请考虑将此类地址放入独立的私有配置文件中，并确保该文件不被提交至公共仓库。

## 许可证

MIT License

Copyright (c) 2026 TermLink Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
