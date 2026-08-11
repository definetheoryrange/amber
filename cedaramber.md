# Bifrost Gateway Index

Bifrost Gateway Index 是一个面向技术调研、域名资产识别与网络信息聚合的开源导航聚合系统。该项目定位于网络安全研究员、威胁情报分析师、基础设施运维工程师以及开源情报（OSINT）采集人员，用于快速归类、校验和访问各类垂直领域的信息资源入口。Bifrost Gateway Index 本身不提供任何代理、翻墙或内容转发服务，仅作为公开可访问的 URL 元数据索引引擎，帮助用户从分散的公开数据源中建立结构化的访问链路。

本项目解决的核心问题在于：大量有价值的技术文档、社区论坛、漏洞库与数据统计页面散布于不同域名与子站点下，缺乏统一的命名空间与健康检测机制。Bifrost Gateway Index 通过静态索引表、定期连通性探测与标签化分类，将分散的资源聚合并提供一致的访问引导。项目完全基于纯静态 Markdown 与 Shell 脚本构建，可部署于任何支持 HTTP 静态托管的平台，无需数据库或后端运行时。

## 功能概览

**多源域名聚合索引**：支持将原始域名列表导入系统，自动生成带分类标签的导航目录，支持手动调整分组。

**HTTP 存活健康检查**：内置基于 curl 的轻量级探测脚本，可定期检测每个域名的可达性，并输出状态码与响应时间。

**元数据标签系统**：允许为每个域名添加自定义元数据标记，如区域归属、服务商识别、内容语言、协议偏好等。

**静态目录生成**：每次更新索引后，自动生成完整的 Markdown 导航文档，支持嵌套分类与锚点跳转。

**批量导入导出**：支持 CSV 与纯文本行格式的批量域名导入，支持将当前索引完整导出为 JSON 格式以供外部工具消费。

**自定义重定向规则模板**：提供 nginx 与 Apache 的 rewrite 规则生成器，便于将索引条目快速映射为短链接或路径转发。

**访问统计快照**：基于访问日志的简单统计聚合，展示热门条目与失效链接排行。

## 应用场景

安全研究团队内部知识库构建：团队可将日常累积的漏洞披露站点、厂商安全公告页、CVE 查询镜像等资源统一录入 Bifrost Gateway Index，形成团队共用的起始页，避免重复查找。

开源情报（OSINT）采集工作流前置：分析师在进行目标信息收集时，需要频繁访问多个公开数据聚合器。本索引可作为采集管道的入口清单，配合健康检查自动剔除失效源。

运维监控仪表盘辅助：运维人员可将内部监控面板、日志查询界面、告警历史页等内部工具链接纳入索引，通过分组标签快速定位不同环境（生产、预发布、测试）。

技术文档归档整理：技术写作人员或文档工程师可将多个版本的项目文档、API 参考、设计提案的托管地址统一编排，并通过重定向模板实现语义化访问路径。

教育训练环境资源分发：培训机构或实验室管理员可将实验手册、镜像下载站、示例代码仓库等资源集中在一个入口页面，学员通过索引即可获得全部所需外部链接。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/bifrost-index/bifrost-gateway-index.git
cd bifrost-gateway-index

# 安装依赖（基于 Ubuntu/Debian）
sudo apt update
sudo apt install -y curl jq coreutils

# 执行初始化安装脚本
./scripts/install.sh

# 构建索引并启动本地预览
./scripts/build-index.sh
./scripts/serve.sh --port 8080

# 浏览器打开 http://localhost:8080 查看生成的主索引页面
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Bash | 4.0 及以上 | 所有核心脚本的运行环境，需支持数组与关联数组 |
| curl | 7.68.0 及以上 | 用于执行 HTTP 健康探测，支持 -o /dev/null -s -w 格式输出 |
| jq | 1.6 及以上 | 解析 JSON 格式的索引文件，用于生成动态统计 |
| coreutils | 8.30 及以上 | 提供 date、sort、uniq 等基础工具，用于日志处理 |
| nginx 或 Apache（可选） | 1.18.0 或 2.4.0 及以上 | 用于生产环境部署，支持 rewrite 模块以启用重定向规则 |
| Git | 2.25.0 及以上 | 用于克隆仓库和版本管理，非运行时强制依赖 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | docs/user-guide/indexing-workflow.md | 如何新增、修改或删除索引条目，如何理解健康检查结果颜色标识 |
| 运维手册 | docs/ops/deployment-options.md | 如何将索引部署到生产服务器，如何配置 HTTPS 以及自定义域名 |
| 脚本参考 | docs/developer/script-architecture.md | 各个 Shell 脚本的调用关系，如何扩展新的探测协议（如 TCP 半开检测） |
| 配置说明 | docs/config/schema.md | 索引配置文件 schema 定义，标签取值规范，重定向模板的占位符语法 |
| 故障排查 | docs/troubleshooting/common-issues.md | 针对探测超时、JSON 解析失败、权限拒绝等典型问题的处理步骤 |

## 资源列表

以下为 Bifrost Gateway Index 当前收录的全部原始域名资源，按功能意向分组。每个条目均保持用户提供的原始格式，未做任何协议补全或域名改写。

公开镜像站与数据聚合域

<code>bifenguanwang.cn</code>

<code>bifenguanfang.org.cn</code>

<code>bifenguanwang.com.cn</code>

统计信息与趋势页面域

<code>bifenqiutangw.org.cn</code>

<code>bifenleisu.org.cn</code>

快速参考与转化工具域

<code>bifenjiebaogw.org.cn</code>

<code>bifen500gw.org.cn</code>

## 项目结构

```
bifrost-gateway-index/
├── index/                           # 主索引工作目录
│   ├── raw/                         # 原始域名清单（按分类存放）
│   │   ├── mirror.domains.txt       # 镜像站列表，每行一个裸域名
│   │   ├── stats.domains.txt        # 统计类资源列表
│   │   └── tools.domains.txt        # 工具类资源列表
│   ├── parsed/                      # 解析后的结构化索引 JSON
│   │   ├── index-snapshot.json      # 完整索引快照，包含标签与探测结果
│   │   └── last-probe.json          # 最近一次健康检查的原始记录
│   └── output/                      # 生成的可浏览文档
│       ├── homepage.md              # 主入口 Markdown 页面
│       └── categories/              # 按标签拆分的子页面
│           ├── mirror.md
│           ├── statistics.md
│           └── utility.md
├── scripts/                         # 可执行脚本集合
│   ├── build-index.sh               # 从 raw 读取并生成 parsed 与 output
│   ├── probe.sh                     # 并发探测所有域名存活状态
│   ├── serve.sh                     # 使用 Python 或 busybox 启动临时 HTTP 服务
│   └── import-csv.sh                # 从外部 CSV 导入增量条目
├── config/                          # 配置与模板
│   ├── schema.json                  # 索引字段类型定义
│   ├── rewrite-template.conf        # nginx rewrite 规则生成模板
│   └── labels.yaml                  # 标签别名与颜色映射配置
├── tests/                           # 单元测试与集成测试
│   ├── test-probe.sh                # 模拟探测行为，验证超时处理
│   └── test-index-format.sh         # 检查生成的 JSON 是否合规
├── docs/                            # 项目文档（详见文档导航）
│   ├── user-guide/
│   ├── ops/
│   ├── developer/
│   └── troubleshooting/
├── .github/                         # 社区协作配置
│   └── workflows/
│       └── ci-daily-probe.yml       # 每日自动探测并提交状态报告
├── LICENSE                          # MIT 许可证全文
└── README.md                        # 本文件
```

## 贡献指南

1. 提交索引新增或变更请求前，请先 fork 本仓库，并在本地执行 `./scripts/probe.sh` 验证所有新增域名的基本可达性。若新增域名在三次探测中均超时，需在提交说明中备注原因。

2. 所有新增域名必须归类到 `index/raw/` 下正确的分类文件中，并同步更新 `config/labels.yaml` 中对应的标签定义。分类命名采用小写英文，多个词以连字符连接。

3. 提交 Pull Request 时，请确保 `docs/` 目录下与变更相关的用户文档已同步更新，特别是 `indexing-workflow.md` 中关于条目格式的示例部分。

4. 若您发现某个已收录域名长期失效或已被劫持，请先通过 Issue 报告，并附带三次不同时间点的探测结果（可附 `probe.sh` 的 JSON 输出片段）。维护者确认后将移除或替换该条目。

5. 对于自动化脚本的改进（如增加新的探测参数、优化并发逻辑），请提供对应 `tests/` 目录下的测试用例，并确保现有测试全部通过后方可合并。

## 常见问题

**问：健康检查显示超时，但浏览器可以正常打开该网站，是什么原因？**

答：部分网站会对非浏览器的 User-Agent 或缺少 Cookie 的请求返回 403 或延迟响应。请检查 `scripts/probe.sh` 中 `CURL_OPTS` 变量是否设置了 `-A "Mozilla/5.0"` 以及是否添加了 `-L` 跟随重定向。若仍无法解决，可在 `config/override.conf` 中为该域名单独配置更长的超时时间（单位秒）。

**问：生成的首页 Markdown 文件如何部署到公网服务器？**

答：本项目的所有输出均为纯静态文件。您可以将 `index/output/` 目录下的全部内容通过 rsync 或 scp 上传到任意 HTTP 服务器的根目录下，例如 `/var/www/html/`。若需要使用重定向规则，请将 `config/rewrite-template.conf` 根据您的服务器类型转换为对应的 `.htaccess` 或 nginx `location` 块后包含进主配置。

**问：如何批量更新所有域名的标签分类而不丢失健康检查历史记录？**

答：标签分类变更不会影响 `index/parsed/last-probe.json` 的探测数据。您可以直接编辑 `config/labels.yaml` 调整标签映射关系，然后重新运行 `./scripts/build-index.sh`。该脚本会保留最近七次的探测快照，仅重新生成静态页面。若需完全重置历史，请删除 `index/parsed/` 下所有 JSON 文件后再执行构建。

## 许可证

MIT License

Copyright (c) 2026 Bifrost Gateway Index Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
