# Terminus Resource Aggregator

Terminus Resource Aggregator is a specialized technical metadata aggregation and navigation system designed for researchers, data analysts, and information professionals who require structured access to domain-specific statistical resources. The project addresses the critical challenge of discovering, validating, and monitoring distributed data sources across multiple top-level domains and naming conventions, providing a unified query interface and availability monitoring layer over heterogeneous external references.

The system operates as a lightweight gateway that consumes external data endpoints, normalizes their response structures, and exposes a consistent API surface for downstream consumption. It does not store or cache payload data but maintains a real-time routing table with health status, response latency, and schema compatibility flags. Target users include DevOps engineers building observability pipelines, data scientists sourcing time-series indicators, and system architects designing federated information retrieval layers.

## 功能概览

- **多源并行探测** 同时对所有配置的外部资源执行 HTTP HEAD 和 GET 请求，收集状态码、响应头及首字节时间，用于可用性评估。
- **响应结构指纹分析** 对返回的 HTML 或 JSON 内容提取 DOM 层级深度、关键选择器命中率及数值字段密度，辅助判断数据完整性。
- **健康状态聚合面板** 以表格和趋势图形式展示每个资源的近 24 小时在线率、平均响应时间及异常计数，支持按域分类筛选。
- **变更监控与告警** 定期比对资源响应中的版本号、时间戳或内容哈希，检测到变动时通过 Webhook 或邮件发送通知。
- **查询路由与代理转发** 提供统一的 RESTful 查询入口，将客户端请求参数映射到对应的外部资源 URL，并可选择透传或改写部分查询字符串。
- **元数据标注系统** 允许用户为每个资源添加标签、描述、所属类别及数据更新频率，形成可检索的资产目录。
- **可扩展资源解析器** 基于 YAML 配置模板定义新资源的抓取规则、解析脚本和输出字段映射，无需修改核心代码即可接入新数据源。

## 应用场景

- **赛事数据实时监控** 数据分析团队使用本系统同时监控多个赛事预测、结果和分析类资源站点的可用性，当某个站点响应超时或返回异常状态码时，系统立即触发告警，便于团队快速切换备用数据通道，保障下游报表的连续性。
- **域名迁移跟踪与验证** 运维人员在批量域名迁移或 TLS 证书更新后，通过系统对一批相关域名执行全量探测，比对响应体中的版本标识和关键字段，确认所有实例均已生效且返回内容一致，避免因部分节点未同步导致的查询分裂问题。
- **第三方数据源合规巡检** 法务或合规部门定期通过系统扫描外部资源列表，检查各站点是否仍符合数据来源声明要求，同时记录响应头中的 Cookie 策略和 P3P 头信息，生成合规报告供审计存档。
- **教学实验中的网络测量** 计算机网络课程使用该系统作为实验工具，让学生观察不同顶级域名（.asia, .org.cn, .com.cn）下同类资源的 DNS 解析时间、首包延迟及内容压缩率差异，理解域名体系和地理位置对 Web 性能的影响。

## 快速开始

以下步骤指导您在 Linux 或 macOS 环境中完成项目克隆、依赖安装和服务启动。

```bash
# 克隆项目仓库
git clone https://github.com/terminus-agg/terminus-resource-agg.git
cd terminus-resource-agg

# 安装 Python 依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 初始化配置文件（复制示例配置并编辑资源列表）
cp config/endpoints.example.yaml config/endpoints.yaml
vim config/endpoints.yaml

# 运行健康检查（单次探测模式）
python cli.py probe --config config/endpoints.yaml --output table

# 启动 Web 服务（默认监听 8080 端口）
python server.py --host 0.0.0.0 --port 8080
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 核心运行环境，低于 3.9 版本不支持类型注解部分语法 |
| pip | 21.0 及以上 | 包管理工具，用于安装 requirements.txt 中列出的依赖 |
| aiohttp | 3.8.4 及以上 | 异步 HTTP 客户端，用于并发探测外部资源，需支持 SSL 上下文 |
| pyyaml | 6.0 及以上 | YAML 配置文件解析，用于加载端点定义和解析规则模板 |
| jinja2 | 3.1.2 及以上 | 模板引擎，用于生成动态查询路径和参数插值 |
| prometheus-client | 0.16.0 及以上 | 指标暴露库，用于向 Prometheus 推送健康检查计数和延迟直方图 |
| uvicorn | 0.23.0 及以上 | ASGI 服务器，用于生产环境运行 Web 接口（如使用 FastAPI 模式） |
| watchdog | 2.3.0 及以上 | 文件系统监控，用于配置热重载（可选依赖，推荐安装） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户手册 | docs/user-guide/probe-usage.md | 如何配置探测间隔、超时阈值和重试策略；如何解读健康报告中的各项指标 |
| 运维指南 | docs/ops/deployment.md | 生产环境如何通过 systemd 或 docker-compose 部署；日志轮转和指标采集配置方法 |
| 开发参考 | docs/dev/endpoint-schema.md | 自定义资源端点时 YAML 文件内各字段（name, url, method, parse_rule）的含义及合法取值 |
| 架构设计 | docs/arch/overview.md | 系统的模块划分（probe_engine, router, notifier）、异步事件循环模型及扩展点设计 |
| API 参考 | docs/api/rest-api.md | 对外提供的 REST 接口路径、请求参数格式、响应结构示例及错误码说明 |
| 测试指南 | docs/dev/testing.md | 如何运行单元测试和集成测试；模拟外部资源不可用场景的 mock 策略 |

## 资源列表

以下为系统默认配置中纳入监控和路由管理的全部外部资源地址，按内容主题分组。所有地址均以用户提供的原始格式原样收录，未做任何协议补全或域名规范化处理。

赛事预测类

- <code>jiebaojishibifen.asia</code>
- <code>jiebaojishibifenw.org.cn</code>
- <code>jiebaojishibifenw.com.cn</code>
- <code>jiebaobisaiyuce.org.cn</code>

赛事结果类

- <code>jiebaobisaijieguo.asia</code>

赛事分析类

- <code>jiebaobisaifenxi.org.cn</code>
- <code>jiebaobisaifenxi.com.cn</code>

## 项目结构

```
terminus-resource-agg/
├── cli.py                         # 命令行入口，支持 probe、list、validate 子命令
├── server.py                      # ASGI 应用启动脚本，加载路由和中间件
├── requirements.txt               # 生产环境依赖列表（含版本锁定）
├── requirements-dev.txt           # 开发测试额外依赖（pytest, black, mypy）
├── docker-compose.yml             # 本地开发环境编排（含 Prometheus + Grafana）
├── Dockerfile                     # 多阶段构建镜像定义（基于 python:3.11-slim）
├── config/
│   ├── endpoints.yaml             # 实际生效的资源端点配置（gitignore 保护）
│   ├── endpoints.example.yaml     # 示例配置，含全部 7 个资源及注释说明
│   ├── alert-rules.yaml           # 告警阈值定义（错误率 > 5% 或延迟 > 3s）
│   └── logging.conf               # 日志格式、输出路径及按日期轮转策略
├── src/
│   ├── __init__.py
│   ├── probe/                     # 探测引擎模块
│   │   ├── client.py              # aiohttp 会话封装，支持超时和重试
│   │   ├── runner.py              # 并发任务调度器，管理 worker 池
│   │   ├── parser.py              # 响应内容解析（HTML/JSON 指纹提取）
│   │   └── metrics.py             # 指标收集与 Prometheus 注册
│   ├── router/                    # 查询路由与代理转发
│   │   ├── matcher.py             # URL 模式匹配与参数映射
│   │   ├── proxy.py               # 透传请求处理，支持流式响应
│   │   └── cache.py               # 可选的内存缓存层（TTL 控制）
│   ├── notifier/                  # 告警通知模块
│   │   ├── webhook.py             # 通用 Webhook 发送（支持 Slack, DingTalk）
│   │   ├── smtp.py                # 邮件通知，支持 TLS 和认证
│   │   └── manager.py             # 告警事件聚合与去重
│   ├── schema/                    # 数据模型与校验
│   │   ├── endpoint.py            # EndpointConfig 数据类（Pydantic 模型）
│   │   ├── result.py              # ProbeResult 结构体（状态、耗时、摘要）
│   │   └── validator.py           # YAML 配置的 JSON Schema 校验器
│   └── utils/
│       ├── time-utils.py          # 时间戳格式化与时区转换
│       ├── hash-utils.py          # 内容哈希计算（用于变更检测）
│       └── network.py             # IP 类型判断、端口连通性预检
├── tests/
│   ├── unit/                      # 单元测试（mock 外部请求）
│   ├── integration/               # 集成测试（需要本地起 stub 服务）
│   └── fixtures/                  # 测试用样本响应数据（JSON/HTML）
├── docs/                          # 完整文档源文件（Markdown + Mermaid 图）
│   ├── user-guide/
│   ├── ops/
│   ├── dev/
│   └── api/
├── scripts/
│   ├── init-db.sql                # 初始化 SQLite 元数据库（可选）
│   └── migrate-v1-to-v2.py        # 配置格式升级迁移脚本
└── .github/
    └── workflows/
        ├── ci.yml                 # GitHub Actions 持续集成（测试 + 镜像构建）
        └── nightly-probe.yml      # 定时任务：每日凌晨全量探测并生成报告
```

## 贡献指南

我们欢迎并鼓励社区贡献，无论是新增资源解析模板、改进探测引擎效率，还是完善文档和示例。请遵循以下步骤提交您的贡献：

1. 在 GitHub Issues 中查找或创建与您改动相关的问题，简要描述您要解决的问题或新增的功能，避免重复劳动。对于较大改动，建议先通过 Issue 讨论设计方案。

2. 派生（Fork）本仓库到您的个人账户，并在派生的仓库中创建一个功能分支（feature/your-feature-name 或 fix/issue-number）。请保持分支名称简洁且具有描述性。

3. 编写代码或文档改动时，请遵守项目现有的编码风格（使用 black 进行 Python 代码格式化，行宽 88 字符），并为新增的函数或类添加 docstring 和类型注解。如果是新增资源端点配置，请在 endpoints.example.yaml 中同步添加示例并标注注释。

4. 在提交 Pull Request 之前，请确保所有单元测试和集成测试通过（执行 pytest tests/），并且新代码的测试覆盖率不低于 80%。若您改动涉及 API 行为，请更新 docs/api/rest-api.md 中的对应示例。

5. 提交 Pull Request 到本仓库的 main 分支，在 PR 描述中关联对应的 Issue 编号，并简要列出改动点、测试结果和任何破坏性变更说明。项目维护者会在 5 个工作日内进行 Review，必要时会请求补充修改。

## 常见问题

**Q: 系统是否会自动重试失败的外部资源请求？重试策略如何配置？**

A: 是的。探测引擎内置了指数退避重试机制，默认最多重试 3 次，初始间隔 1 秒，后续间隔翻倍并增加随机抖动（jitter）以避免同时重试造成的拥塞。您可以在 config/endpoints.yaml 中为每个资源单独设置 retry_count 和 retry_backoff_base 字段覆盖全局默认值。此外，对于返回 5xx 状态码或连接超时的请求，系统会将其计入错误率并触发告警，但不会无限重试以免阻塞探测队列。

**Q: 如何添加一个新的外部资源地址并自定义解析规则？**

A: 不需要编写 Python 代码。您只需在 config/endpoints.yaml 中新增一个条目，包含 name（唯一标识）、url（原始地址）、method（GET/POST）、interval_seconds（探测间隔）以及 parse_rule 字段。parse_rule 支持 xpath、jsonpath 或 regex 三种提取方式，用于从响应中抓取您关心的版本号或数据指纹。系统每次探测后会根据 parse_rule 计算指纹值，并与上一次记录比对，若不一致则生成变更事件。具体语法示例请参考 docs/dev/endpoint-schema.md 中的完整说明。

**Q: Web 服务启动后，查询接口的访问路径是什么？如何限制访问频率？**

A: Web 服务默认提供 /api/v1/query 端点，接受 GET 请求，参数包括 target（资源名称）和 params（JSON 格式的查询参数）。该接口会将请求转发到对应的外部资源并返回原始响应内容（透传模式）。频率限制未在核心模块中内置，但您可以在反向代理层（如 Nginx 或 Traefik）配置 rate limiting 策略，或使用 src/router/middleware.py 中的限流装饰器（需自行启用 Redis 后端）。生产部署建议开启 API Key 认证，通过环境变量 API_KEYS 配置合法密钥列表。

## 许可证

MIT License

Copyright (c) 2026 Terminus Resource Aggregator Contributors

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

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:18
