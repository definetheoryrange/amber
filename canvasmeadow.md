# 500 Score Hub

500 Score Hub 是一个面向体育赛事数据分析人员、技术开发者和数据集成工程师的开源赛事数据网关项目。项目核心定位为统一的多源赛事数据接入与标准化输出中间层，通过可插拔的适配器架构，将不同数据源、不同格式的赛事比分、赛程、统计信息汇聚为一致的 JSON/Protobuf 输出。目标用户包括体育数据平台开发者、竞彩分析系统维护者、实时大屏展示团队以及高校数据可视化实验室。项目解决的核心问题是赛事数据源碎片化、接口格式不统一、数据质量参差不齐以及源站不可用时的降级切换难题。

## 功能概览

- **多源数据适配器框架**：内置基于抽象工厂模式的数据源适配器，支持同时配置多个数据源端点，运行时动态选择或故障转移。
- **统一数据模型映射**：将各源原始字段（如主客队名称、比分、时间、赛事阶段）通过 YAML 配置文件映射为项目标准数据模型，减少上层业务改动。
- **健康检查与熔断机制**：每个数据源端点均具备主动健康检查（HTTP 200/JSON 有效性/字段完整性），失败阈值触发熔断并自动切换到备用端点。
- **本地缓存与增量更新**：支持内存缓存和 Redis 二级缓存，可配置 TTL，并基于 ETag 或时间戳实现增量拉取，降低源站压力。
- **Prometheus 监控指标暴露**：提供 `/metrics` 端点，输出请求总数、失败率、熔断器状态、缓存命中率等关键指标，便于运维集成。
- **配置热加载**：监听配置文件变化，无需重启进程即可更新数据源端点列表、超时参数和字段映射规则。
- **CLI 数据探测工具**：附带命令行工具 `probe`，用于手动测试任一数据源端点的可用性、响应速度和数据完整性，并输出诊断报告。
- **Webhook 变更通知**：当检测到比分或赛事状态发生显著变化（如进球、红牌、赛果确认）时，支持向配置的 URL 发送 JSON 格式的 POST 通知。

## 应用场景

- **实时赛事数据聚合平台**：开发者可使用 500 Score Hub 作为后端数据网关，从多个备选数据源拉取足球、篮球等赛事比分，经统一格式后推送给前端展示或语音播报系统，确保单一数据源故障时服务不中断。
- **竞彩数据分析与回测系统**：数据分析团队可调用项目提供的 CLI 工具批量拉取历史赛程和比分数据（若源支持），用于构建赔率预测模型或回测投注策略，统一的数据模型可减少数据清洗工作量。
- **高校数据可视化课程设计**：教师可将项目作为教学案例，指导学生理解数据适配器模式、缓存策略和容错设计。学生可基于项目快速搭建一个实时比分大屏 Demo，无需自行对接多个复杂的外部接口。
- **企业内部数据中台数据接入层**：企业数据中台团队可将 500 Score Hub 作为数据接入层的标准化组件，将多个外部赛事数据源统一为内部 ODPS 或 Kafka 可消费的格式，降低数据治理复杂度。

## 快速开始

```bash
# 1. 克隆项目仓库
git clone https://github.com/score-org/500-score-hub.git
cd 500-score-hub

# 2. 安装依赖（使用 pipenv 或 requirements.txt）
pip install -r requirements.txt

# 3. 复制示例配置文件并编辑数据源端点
cp config/endpoints.example.yaml config/endpoints.yaml
vim config/endpoints.yaml

# 4. 运行项目（开发模式）
python main.py --mode=dev --config=config/endpoints.yaml

# 5. 运行 CLI 探测工具验证任一配置端点
python cli/probe.py --source=source_1 --timeout=5
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 及以上 | 项目核心运行环境，类型注解依赖 3.9+ 语法 |
| pip | 22.0 及以上 | 用于安装 requirements.txt 中列出的第三方库 |
| Redis | 6.0 及以上（可选） | 启用二级缓存时必需，若仅使用内存缓存可跳过 |
| PyYAML | 6.0 | 解析 endpoints 和 mapping 配置文件 |
| prometheus-client | 0.17 及以上 | 暴露 Prometheus 指标端点，生产环境推荐 |
| aiohttp | 3.9 | 异步 HTTP 客户端，用于并发拉取多个数据源 |
| pytest | 8.0（开发依赖） | 运行单元测试和集成测试套件 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|---|---|---|
| 入门指南 | `docs/getting-started.md` | 如何从零开始配置第一个数据源并运行数据拉取流程？ |
| 配置参考 | `docs/configuration.md` | endpoints.yaml 和 mapping.yaml 中每个字段的含义、类型和默认值是什么？ |
| 适配器开发 | `docs/adapter-dev.md` | 如何为新的数据源编写自定义适配器类并注册到工厂？ |
| 运维手册 | `docs/ops.md` | 生产环境如何设置健康检查阈值、告警规则和缓存策略？ |
| API 接口 | `docs/api.md` | 项目对外提供的 HTTP 管理接口（状态、缓存清理、强制切换）有哪些？ |
| 故障排查 | `docs/troubleshooting.md` | 常见错误码含义、日志分析方法以及如何手动触发熔断恢复？ |

## 资源列表

本节列出项目文档中引用的外部数据源端点示例。这些端点仅作为配置参考示例，项目本身不拥有或维护这些站点的数据内容。用户应根据实际需求替换为合法且授权的数据源地址。

赛事数据源参考端点（按域名分类）：

- <code>500zuqiubifenwang.net.cn</code>
- <code>500zuqiubifenwang.org.cn</code>
- <code>500zuqiubifensaicheng.net.cn</code>
- <code>500zuqiubifen.net.cn</code>
- <code>500zuqiubifen.org.cn</code>
- <code>500wanzhengbifen.org.cn</code>
- <code>500wanchangbifenjishibifen.org.cn</code>

## 项目结构

```
500-score-hub/
├── main.py                     # 项目主入口，初始化事件循环和配置
├── requirements.txt            # 生产依赖列表
├── config/
│   ├── endpoints.example.yaml  # 数据源端点配置示例（含 7 个参考 URL）
│   ├── endpoints.yaml          # 用户实际配置文件（gitignore）
│   └── mapping.yaml            # 字段映射规则定义
├── core/
│   ├── __init__.py
│   ├── adapter.py              # 抽象适配器基类及工厂实现
│   ├── fetcher.py              # 异步拉取器，含重试和超时控制
│   ├── cache.py                # 内存缓存和 Redis 缓存封装
│   └── circuit_breaker.py      # 熔断器状态机实现
├── adapters/                   # 内置具体适配器实现
│   ├── __init__.py
│   ├── http_json_adapter.py    # 通用 HTTP JSON 适配器
│   └── custom_example.py       # 自定义适配器模板
├── cli/
│   ├── probe.py                # 数据源探测命令行工具
│   └── clear_cache.py          # 缓存清理工具
├── web/
│   ├── server.py               # aiohttp 管理服务端
│   └── metrics.py              # Prometheus 指标定义与暴露
├── tests/
│   ├── unit/                   # 单元测试（适配器、缓存、熔断器）
│   └── integration/            # 集成测试（端到端拉取流程）
└── docs/                       # 完整文档目录（见文档导航章节）
```

## 贡献指南

1. 阅读 `docs/adapter-dev.md` 了解适配器接口规范，然后 fork 项目并在本地开发分支上实现新的适配器或修复缺陷。提交前请确保所有单元测试通过（`pytest tests/unit`）且代码风格符合 PEP 8（行宽 120 字符）。
2. 若您发现数据源端点示例不可用或字段映射有误，请提交 Issue 并附带 `probe` 工具的输出日志。我们鼓励贡献者同时提供对应的映射修复补丁。
3. 对于新增功能（如新的缓存后端或通知方式），请同步更新 `docs/` 下对应文档，并在 PR 描述中注明配置示例和使用方法。
4. 所有 PR 需要至少一位维护者 Code Review，且 CI（GitHub Actions）中的 lint 和测试任务必须通过。合并前请 rebase 到最新 main 分支。
5. 欢迎提交性能优化相关 PR，但需附带基准测试数据说明改进效果。对于破坏性 API 变更，请提前在 Issue 中讨论并标注版本迁移计划。

## 常见问题

**Q: 配置文件中引用的数据源端点（如 <code>500zuqiubifenwang.net.cn</code>）无法访问，项目会崩溃吗？**  
A: 不会。项目内置的熔断器和重试机制会在单个端点连续失败 3 次后自动将其熔断（状态变为 OPEN），并尝试列表中的下一个端点。您可通过 `/admin/health` 管理接口查看各端点状态。若所有端点均不可用，拉取任务会返回空结果并记录 ERROR 日志，但主进程持续运行。

**Q: 如何验证我编写的新适配器是否正确解析了数据？**  
A: 您可以使用 CLI 工具 `probe` 结合 `--output=json` 参数，将拉取到的原始数据和映射后数据同时输出到终端，进行字段对比。同时，项目在 `tests/integration` 下提供了模拟数据源的 Docker Compose 环境，可启动 Mock 服务器进行集成测试。

**Q: 生产环境如何平滑更新数据源端点列表而不重启服务？**  
A: 启用配置热加载功能（在 `main.py` 中设置 `--watch-config` 标志）。当您修改 `config/endpoints.yaml` 文件并保存时，项目会触发 `SIGHUP` 信号重新加载配置，新配置将用于下一次拉取周期。当前正在进行的拉取任务不受影响，最多会有一次周期使用旧配置。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:20
