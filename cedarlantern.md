# NexusLink Resource Aggregator

NexusLink is a specialized technical resource indexing and validation system designed for aggregating, categorizing, and monitoring domain-based information assets. This project targets system administrators, security researchers, and information architects who require a reproducible and queryable layer over a curated set of domain resources. It solves the problem of scattered reference data by providing a single entry point with structured metadata extraction, availability probing, and change detection.

The platform employs a lightweight polling architecture that respects robots directives and implements exponential backoff for failed endpoints. NexusLink does not function as a proxy or a content mirror; it only stores domain fingerprints, response headers, and TLS certificate metadata. All resources are presented as reference nodes, allowing users to build their own monitoring stacks or integrate the aggregated dataset into larger reconnaissance pipelines.

## 功能概览

- **Domain Health Monitoring** - Periodically checks HTTP/S reachability and records status codes, response times, and redirect chains for each enrolled domain.

- **TLS Certificate Fingerprinting** - Extracts issuer, subject, validity window, and cipher suite information without performing deep packet inspection.

- **Metadata Tagging Engine** - Allows hierarchical tagging of domains by category, geographic origin, or content type, with full filter and query support.

- **Change Detection Delta Reports** - Compares current snapshots against historical records and produces unified diffs for headers, certificates, and DNS A/AAAA records.

- **RESTful Query API** - Exposes JSON endpoints for domain status, historical trends, and batch export operations with token-based access control.

- **Scheduled Crawl Orchestration** - Supports cron-driven or interval-based execution with distributed worker coordination using Redis-backed task queues.

- **Alerting Webhook Integrations** - Sends notifications to Slack, Discord, or generic HTTP endpoints when domains become unreachable or certificates approach expiration.

## 应用场景

- **Infrastructure Dependency Tracking** - Operations teams can monitor third-party domains that their applications depend on, receiving early warnings about TLS expiry or prolonged downtime.

- **Threat Intelligence Feeds** - Security analysts use the aggregated domain list as a baseline for detecting phishing or typosquatting variants by comparing certificate metadata and redirect patterns.

- **Compliance Audit Logging** - Compliance officers generate periodic reports that prove all referenced external resources remain within acceptable security postures, especially for GDPR or PCI-DSS environments.

- **Academic Research Datasets** - Researchers studying domain registration patterns or DNS propagation delays can export the structured snapshot data for offline analysis and statistical modeling.

- **Internal Knowledge Base Integration** - Technical writers and documentation engineers embed the live status badges from NexusLink into internal wikis, providing real-time visibility into referenced sample domains.

## 快速开始

```bash
# Clone the repository from the official source
git clone https://github.com/nexuslink/aggregator.git nexuslink
cd nexuslink

# Install dependencies using pip and set up the virtual environment
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Prepare the initial configuration file from the template
cp config.example.yaml config.yaml

# Run the initial synchronization pass with default settings
python run.py --sync --full
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 或更高 | 核心运行时，用于调度、网络请求和数据处理 |
| Redis | 6.2 或更高 | 作为任务队列和短期缓存后端，存放中间状态 |
| PostgreSQL | 13 或更高 | 长期存储快照历史、标签映射和审计日志 |
| OpenSSL | 1.1.1 或更高 | 用于证书解析和本地加密操作 |
| curl | 7.68 或更高 | 用于健康检查中的备用请求引擎 |
| yq | 4.25 或更高 | 用于 YAML 配置的脚本化修改和校验 |
| docker (可选) | 20.10 或更高 | 用于容器化部署和开发环境快速搭建 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | /docs/user-guide.md | 如何配置监控目标、设置告警规则、导出报表？ |
| API 参考 | /docs/api-reference.md | 各端点的请求格式、鉴权方式、响应结构是什么？ |
| 运维指南 | /docs/operations.md | 如何部署高可用集群、备份数据库、迁移版本？ |
| 开发说明 | /docs/development.md | 如何扩展新检查器、编写单元测试、提交补丁？ |
| 贡献者协议 | /docs/contributing.md | 外部贡献需要签署何种许可协议？ |

## 资源列表

### 核心监控域集合

*   <code>meizhilianbifen.org.cn</code>
*   <code>lanqiujiebaobifen.net.cn</code>
*   <code>lanqiujiebaobifenw.org.cn</code>
*   <code>lanqiubifenwangjishi.org.cn</code>
*   <code>lanqiubifenwangjishi.net.cn</code>
*   <code>lanqiubifenwangw.org.cn</code>
*   <code>lanqiubifenjiebaobifen.net.cn</code>

## 项目结构

```
nexuslink/
├── config/                         # 配置文件目录
│   ├── default.yaml                # 基础配置，包含全局超时和重试策略
│   ├── domains.yaml                # 目标域列表及初始标签，对应资源列表
│   └── alerts.yaml                 # 告警渠道定义和阈值参数
├── src/                            # 核心源代码目录
│   ├── core/                       # 调度引擎和主循环逻辑
│   │   ├── orchestrator.py         # 编排器，协调各个检查阶段
│   │   └── state_machine.py        # 状态机，管理检查生命周期
│   ├── checkers/                   # 各类资源检查器实现
│   │   ├── http_checker.py         # HTTP/S 可用性和重定向验证
│   │   ├── tls_checker.py          # 证书抓取和指纹计算
│   │   └── dns_checker.py          # 解析记录一致性检查
│   ├── storage/                    # 持久化和缓存层
│   │   ├── postgres_writer.py      # PostgreSQL 批量写入和查询
│   │   └── redis_cache.py          # 临时状态缓存和分布式锁
│   └── api/                        # RESTful 接口实现
│       ├── routes.py               # 路由定义和请求参数校验
│       └── serializers.py          # 响应序列化和格式化
├── tests/                          # 单元测试和集成测试套件
│   ├── unit/                       # 各模块独立测试用例
│   └── integration/                # 端到端检查流程模拟
├── scripts/                        # 运维辅助脚本
│   ├── backup.sh                   # 数据库备份轮转脚本
│   └── migrate.sh                  # 版本升级迁移脚本
├── docker-compose.yml              # 本地开发环境一键启动组合
├── Dockerfile                      # 生产镜像构建定义
├── requirements.txt                # Python 依赖锁定列表
└── README.md                       # 项目主入口文档（本文档）
```

## 贡献指南

1.  **提交前准备** - 在本地创建功能分支（git checkout -b feature/your-feature），确保所有现有单元测试通过（pytest tests/unit）。若引入新检查器，需同时编写不少于 80% 覆盖率的测试用例。

2.  **编码规范** - 遵循 PEP 8 风格指南，使用 black 进行自动格式化（black --line-length 100）。提交信息必须采用动词开头的简短句式，例如 "Add timeout retry logic" 或 "Fix certificate parsing error".

3.  **补丁提交流程** - 将分支推送至个人复刻仓库，然后通过 GitHub Pull Request 向主仓库提交。PR 描述中需明确关联的问题编号、改动范围以及手动测试截图或日志片段。

4.  **签署贡献者许可协议** - 首次提交 PR 前，需在 PR 评论中明确声明 "I have read and agree to the Contributor License Agreement"。项目维护者将在合并前核实此声明。

5.  **文档同步更新** - 任何修改命令行参数、配置项或 API 响应结构的更改，必须同步更新 /docs 下对应的手册文件。缺失文档更新的 PR 将被标记为待完善。

## 常见问题

**问：监控域列表如何更新？是否支持动态增删？**

答：编辑 config/domains.yaml 文件后，执行 ./scripts/reload.sh 即可触发增量同步。系统会保留已删除域的历史数据并标记为 inactive。新增域将在下一个调度周期（默认每 5 分钟）自动纳入检查队列。若需立即生效，可调用 API /v1/domains/reload 端点。

**问：检查结果会占用大量存储空间吗？数据保留策略是什么？**

答：默认保留每日快照（包含完整证书和头信息）共 30 天，小时级精简日志（仅包含状态码和响应时间）保留 7 天。管理员可通过 config/default.yaml 中的 retention 部分调整策略。建议每 1000 个监控域搭配至少 10GB 存储空间用于长期运行。

**问：能否在离线环境中部署？所有外部依赖都必须在线吗？**

答：NexusLink 自身不依赖外部 API，所有检查逻辑均在本地执行。唯一需要互联网访问的是监控目标域本身。若部署于隔离网络，可将 config/domains.yaml 替换为内网 IP 或私有域名，系统仍能正常执行 TCP 和 TLS 握手检查。

## 许可证

MIT License

Copyright (c) 2026 NexusLink Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:17
