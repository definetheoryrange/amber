# LeiSu Resource Aggregator

LeiSu Resource Aggregator 是一个轻量级的技术资源导航与外部信息汇总系统，专为技术调研、行业动态追踪和快速原型验证场景设计。项目定位于开发者、数据分析师与技术决策者，通过结构化聚合多个垂直领域的实时数据源，解决信息分散、检索效率低、上下文切换成本高的问题。

系统核心机制为可配置的源地址管理，配合简易的元数据提取与分类存储，使得用户能够在一套统一的命令行界面下完成多源信息的批量拉取、基础过滤与状态标记。本项目不提供数据存储服务，仅作为地址管理与标准化输出层，所有原始数据保留在源地址所在域。

## 功能概览

**多源地址批量导入**：支持从纯文本配置文件或标准输入流中批量载入外部链接，自动进行去重与格式规整。

**源状态健康检查**：对已录入的每个外部地址执行定期可达性探测，输出 HTTP 状态码与响应时间，辅助判断源可用性。

**分类标签管理**：允许用户为每个地址附加自定义标签（如 "预测"、"前瞻"、"实时"、"推荐"），并支持按标签组合过滤。

**元数据快照输出**：针对每个源地址，抓取页面标题、描述及最后修改时间等基础元数据，以 JSON 或 YAML 格式导出，便于下游工具消费。

**定时任务集成接口**：提供 cron 表达式配置入口，可周期触发全量更新与状态检查，适合部署在服务器端无人值守运行。

**命令行交互式过滤**：内置交互式过滤视图，支持按关键字、状态码范围、标签集合动态筛选当前地址列表，并导出子集。

**地址变更日志记录**：自动记录每个外部地址的状态变更历史（新增、失效、恢复），便于审计与异常追溯。

## 应用场景

技术团队内部知识库维护：团队可将本项目作为外部参考链接的统一入口，定期检查订阅的行业分析站点、技术博客或数据面板的可用性，并在内部文档中引用经过健康检查的稳定链接。

数据采集管道前置校验：在 ETL 或数据采集任务执行前，使用本项目的健康检查模块批量验证目标源地址的有效性，提前过滤不可达地址，减少采集任务失败率。

个人开发者信息聚合：开发者可将日常关注的多个预测类、推荐类或实时数据类站点纳入本项目统一管理，通过标签快速切换不同关注领域，避免浏览器书签杂乱。

运维监控告警联动：运维人员可将本项目与现有监控系统集成，当外部源地址状态异常时，通过标准输出或 Webhook 触发告警，实现被动监控向主动探测的补充。

## 快速开始

```bash
# 1. 克隆项目仓库
git clone https://github.com/leisu-resource/aggregator.git
cd aggregator

# 2. 安装依赖（使用 Python 虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 运行初始配置与示例导入
cp config/example_sources.yaml config/sources.yaml
python main.py --import config/sources.yaml
python main.py --check --timeout 5
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 及以上 | 核心运行环境，需支持 asyncio 和 dataclasses |
| aiohttp | 3.9.0 及以上 | 异步 HTTP 客户端，用于并发健康检查与元数据抓取 |
| pyyaml | 6.0 及以上 | YAML 配置文件解析，用于地址列表与定时任务配置 |
| colorama | 0.4.6 及以上 | 命令行输出着色，提升交互式视图可读性 |
| pytest | 8.0 及以上 | 单元测试框架，仅在开发模式或运行测试套件时使用 |
| flake8 | 7.0 及以上 | 代码风格检查工具，仅在提交代码或 CI 流水线中使用 |
| croniter | 2.0 及以上 | 定时任务表达式解析库，用于调度接口的时间计算 |

## 文档导航

| 层面 | 目录/文档 | 回答的问题 |
|------|----------|-----------|
| 用户手册 | docs/user_guide.md | 如何安装、配置源地址、执行健康检查与导出结果？ |
| 配置参考 | docs/config_reference.md | sources.yaml 中每个字段的含义、可选值与默认行为是什么？ |
| API 设计 | docs/api_design.md | 内部模块间的接口契约、事件钩子与扩展点如何定义？ |
| 运维部署 | docs/deployment.md | 如何在 Linux 服务器上以 systemd 服务方式运行并配置自动轮转日志？ |

## 资源列表

本项目作为地址聚合层，当前管理以下外部信息资源。所有地址均按原始格式收录，未做协议或域名规范化处理。

行业前瞻与预测类：

<code>leisuyuce.asia</code>

<code>leisusaishiqianzhan.cn</code>

<code>leisusaishiqianzhan.org.cn</code>

<code>leisusaiguo.asia</code>

实时推荐与即时资讯类：

<code>leisujinrituijian.org.cn</code>

<code>leisujinrituijian.cn</code>

动态数据与分时参考类：

<code>leisujishibifen.asia</code>

## 项目结构

```
aggregator/
├── main.py                     # 程序入口，解析命令行参数并调度各模块
├── config/
│   ├── settings.py             # 全局配置常量（超时、并发数、日志级别）
│   ├── source_schema.py        # 源地址数据模型定义（dataclass 与校验器）
│   └── example_sources.yaml    # 示例源地址配置文件，供初次使用者参考
├── core/
│   ├── loader.py               # 从 YAML/JSON 文件或 stdin 载入地址列表
│   ├── checker.py              # 异步并发健康检查引擎，返回状态码与延迟
│   ├── fetcher.py              # 元数据抓取器（标题、描述、head 信息）
│   └── exporter.py             # 结果导出为 JSON、YAML 或纯文本表格
├── cli/
│   ├── parser.py               # 命令行参数解析与子命令路由
│   ├── interactive.py          # 交互式过滤与筛选视图（基于 prompt_toolkit）
│   └── formatter.py            # 终端输出格式化（表格、颜色、对齐）
├── scheduler/
│   ├── cron_parser.py          # cron 表达式解析与下一次触发时间计算
│   └── job_runner.py           # 定时任务执行器，可独立于主进程运行
├── tests/
│   ├── test_checker.py         # 健康检查模块单元测试（含 mock 网络请求）
│   ├── test_loader.py          # 地址加载与去重逻辑测试
│   └── test_exporter.py        # 导出格式正确性与边界条件测试
├── docs/                       # 完整文档目录，含用户手册、API 设计、部署指南
├── requirements.txt            # 生产环境依赖列表
├── requirements-dev.txt        # 开发环境额外依赖（测试、代码检查）
└── README.md                   # 本文件
```

## 贡献指南

1. 在 GitHub Issues 中搜索现有讨论或创建新议题，描述您希望修复的问题或新增的功能，并等待项目维护者的确认与标签分配。

2. 从项目仓库派生一份副本到您的个人账户，基于 main 分支创建新的功能分支，分支命名遵循 `feature/简述` 或 `fix/简述` 格式。

3. 在本地开发环境中运行 `make lint` 和 `make test`（或对应 flake8 与 pytest 命令），确保所有现有测试通过且代码风格无警告，新增功能需附带相应的单元测试用例。

4. 提交代码时使用常规提交规范（Conventional Commits），提交信息首行概括变更内容，正文说明变更动机与影响范围，并引用关联的 Issue 编号。

5. 发起 Pull Request 到本仓库的 main 分支，PR 描述中填写变更摘要、测试覆盖情况以及是否影响现有配置兼容性，等待至少一位维护者的 Code Review。

## 常见问题

**问：健康检查模块是否会对外部源地址造成额外负载？**

答：默认情况下，健康检查仅发送单次 HTTP HEAD 请求，不拉取完整页面体，超时时间设为 5 秒，并发数限制为 50。对于敏感源地址，用户可在配置文件中单独设置 `check_policy: gentle` 以降低探测频率或禁用自动检查。项目设计原则为最小化对源站的影响，所有探测行为均可由用户完全控制。

**问：如何迁移已有的浏览器书签或 CSV 格式的链接列表？**

答：项目提供 `utils/import_bookmarks.py` 辅助脚本，支持 Netscape 格式书签导出文件（HTML）以及标准 CSV（列标题为 url, tag, note）的转换。运行 `python utils/import_bookmarks.py --input bookmarks.html --output config/sources.yaml` 即可完成迁移。若需支持其他格式，可参照 `core/loader.py` 中的扩展接口自行实现。

**问：定时任务模块是否支持分布式部署或多实例协同？**

答：当前版本定时任务基于本地文件锁机制，仅适用于单机单实例场景。若需分布式调度，建议将 `scheduler/job_runner.py` 中的触发逻辑与外部分布式任务队列（如 Celery 或 RQ）集成，本项目提供 `--webhook` 参数支持将检查结果通过 HTTP POST 转发至外部服务，以实现状态同步。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:13
