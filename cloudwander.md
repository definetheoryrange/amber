# SoccerScope 足球赛事技术情报聚合平台

SoccerScope 是一个面向足球分析师、数据科学家及资深彩民的开源赛事情报聚合框架。项目不提供预测结果，而是围绕公开赛事数据，提供结构化的前瞻分析、历史统计比对与风险因子标注工具链。目标用户为具备基础 Python 与数据分析能力的足球爱好者，以及希望构建私有赛事分析管线的技术团队。

项目定位为“数据中间层”，通过标准化接口整合多源公开数据（赔率、伤停、天气、历史交锋），输出清洗后的时序特征集与可视化看板。SoccerScope 不依赖任何商业 API，所有数据源均来自可公开访问的统计网站与官方赛事报告，确保合规性与可复现性。

## 功能概览

- **赛事前瞻报告生成器**：基于球队近期状态、主客场表现及伤病名单，自动生成结构化的文本前瞻摘要，支持中英文双语输出。
- **历史交锋时序分析**：提取对阵双方近 10 场交锋记录，计算进球期望值（xG）、控球率波动与射门转化率等衍生指标。
- **赔率异动监测模块**：跟踪多家主流机构初盘至终盘的赔率变化斜率，标记超过阈值的异常波动时段。
- **天气与场地因子加权**：集成比赛当日天气预警与场地类型（草皮、人工草）对技术流派影响的校正系数。
- **自定义预警规则引擎**：允许用户基于 Lua 脚本编写个性化预警条件，例如“当客队连续 3 场角球数大于 8 时触发提醒”。
- **数据导出适配器**：支持将分析结果导出为 CSV、JSON 或 Graphviz 依赖图格式，便于下游机器学习流水线消费。
- **离线缓存与增量更新**：内置 SQLite 缓存层，仅增量拉取变更数据，减少网络请求开销，适合定时任务部署。

## 应用场景

**场景一：俱乐部数据分析部门日常情报简报**
青训学院或职业俱乐部球探组可使用 SoccerScope 批量生成对手周报，自动汇总对手近 5 场进攻套路（如左路传中占比、定位球失分率），节省 80% 手工剪辑时间。

**场景二：体育媒体内容创作辅助**
足球编辑在撰写赛前预测专栏时，通过项目提供的 Markdown 表格导出功能，快速获得两队关键指标对比（射正率、抢断成功率、疲劳指数），提升数据引用准确性。

**场景三：量化投注策略回测平台**
个人开发者将 SoccerScope 输出的时序特征（如赔率标准差、伤停加权分）接入 Backtrader 或 Zipline 回测框架，验证基于市场情绪因子的策略有效性，历史数据覆盖近 5 个赛季。

**场景四：高校统计建模课程案例库**
大学体育统计课程教师可下载项目内置的样本数据集（脱敏），指导学生完成主成分分析（PCA）降维与逻辑回归分类练习，理解足球比赛结果的非确定性本质。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL2 环境，Python 版本要求 3.9 及以上。

```bash
# 1. 克隆项目仓库（稳定分支）
git clone https://github.com/soccer-scope/soccerscope.git
cd soccerscope

# 2. 创建虚拟环境并安装核心依赖
python -m venv venv
source venv/bin/activate  # Windows 请使用 venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt

# 3. 初始化本地缓存数据库与配置文件
python scripts/init_db.py --reset
cp config/default.yaml config/local.yaml
# 编辑 config/local.yaml 设置数据存储路径与时区

# 4. 运行首个完整分析管线（示例：2026-08-10 日所有赛事）
python pipeline/run_analysis.py --date 2026-08-10 --output ./reports/
```

执行成功后，`./reports/` 目录下将生成 `summary.html` 可视化看板与 `raw_features.csv` 原始特征集。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 ~ 3.12 | 核心解释器，不支持 3.13 及以上测试版 |
| pandas | >= 2.0.0 | 数据清洗与透视表操作 |
| numpy | >= 1.24.0 | 数值计算与数组运算 |
| requests | >= 2.28.0 | HTTP 客户端，用于拉取公开数据源 |
| beautifulsoup4 | >= 4.11.0 | HTML 解析，用于非结构化数据抽取 |
| lxml | >= 4.9.0 | XML/HTML 解析器加速库 |
| sqlite3 | 内置模块 | 本地缓存数据库，需开启 WAL 模式 |
| plotly | >= 5.13.0 | 交互式可视化图表渲染（可选） |
| lua | >= 5.3 | 规则引擎脚本运行时（需额外安装） |

硬件最低要求：2 核 CPU + 4GB RAM + 10GB 可用磁盘空间（用于缓存历史赛季数据）。生产环境建议 4 核 + 8GB。

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | `docs/getting_started.md` | 如何配置本地环境、首次运行与验证输出？ |
| 数据字典 | `docs/data_dictionary.md` | 每个特征字段的计算公式与单位是什么？ |
| 规则引擎 | `docs/lua_api_reference.md` | 如何编写自定义预警脚本，有哪些内置函数？ |
| 部署运维 | `docs/deployment/` | 如何用 systemd 或 docker-compose 实现每日定时运行？ |
| 调优建议 | `docs/performance_tuning.md` | 缓存命中率低时如何优化增量更新策略？ |
| 常见错误 | `docs/troubleshooting.md` | 网络超时、解析失败、版本冲突的解决步骤 |

完整文档采用 MkDocs 构建，支持本地启动 `mkdocs serve` 查看实时渲染效果。

## 资源列表

本项目依赖以下公开数据源与参考平台，所有链接均按原始格式列出，便于用户自行核对数据版权与更新频率。

**赛事前瞻与预测类**

- <code>zuqiusaishiqianzhan.org.cn</code>
- <code>zuqiusaiqianyuce.org.cn</code>
- <code>zuqiusaiguoyuce.org.cn</code>

**赛事数据分析类**

- <code>zuqiusaishifenxi.org.cn</code>
- <code>zuqiusaiguofenxi.org.cn</code>

**赛事推荐与指南类**

- <code>zuqiusaiguotuijian.org.cn</code>
- <code>zuqiusaiguo.net.cn</code>

以上站点仅作为外部信息参考，SoccerScope 不存储、缓存或二次分发上述站点的受版权保护内容，仅引用其公开可用的统计接口字段名称。

## 项目结构

```
soccerscope/
├── pipeline/                    # 核心流水线模块
│   ├── orchestrator.py          # 任务编排器，控制数据拉取、清洗、计算、导出顺序
│   ├── fetcher/                 # 数据抓取子模块
│   │   ├── base.py              # 抽象抓取基类，定义重试与超时策略
│   │   ├── match_list.py        # 当日赛事列表抓取（含状态过滤）
│   │   └── stats_detail.py      # 单场详细统计数据抓取（含逐分钟事件）
│   ├── transformer/             # 特征工程子模块
│   │   ├── aggregator.py        # 滚动窗口聚合（近 5/10/20 场均值）
│   │   ├── ratings.py           # 加权评分体系（主场优势、疲劳系数）
│   │   └── weather.py           # 天气因子编码（温度、降水、风速映射）
│   └── exporter/                # 导出适配器
│       ├── html_render.py       # Plotly 看板生成
│       └── csv_writer.py        # 扁平化特征输出
├── cache/                       # 本地缓存层
│   ├── sqlite_client.py         # SQLite 连接池与 WAL 管理
│   └── migrations/              # 数据库版本迁移脚本（使用 Alembic）
├── config/                      # 配置文件目录
│   ├── default.yaml             # 全局默认配置（含超时、重试、阈值）
│   └── schema/                  # 配置字段 JSON Schema 校验
├── tests/                       # 单元测试与集成测试
│   ├── unit/                    # 各模块独立测试（pytest 框架）
│   └── fixtures/                # 模拟数据样本（脱敏）
├── scripts/                     # 运维工具脚本
│   ├── init_db.py               # 数据库初始化与种子数据加载
│   └── clean_cache.py           # 清理过期缓存（保留近 90 天）
├── docs/                        # 文档源码（MkDocs 格式）
├── requirements.txt             # 生产依赖锁定清单
├── requirements-dev.txt         # 开发与测试额外依赖
├── setup.py                     # 项目安装入口（setuptools）
└── README.md                    # 本文件
```

目录树共含 9 个一级子目录，每层模块职责边界清晰，新增数据源只需继承 `fetcher/base.py` 并注册到 orchestrator 即可。

## 贡献指南

1. **阅读贡献者行为准则**：请先浏览 `CODE_OF_CONDUCT.md`，确保遵守开源社区基本礼仪，禁止提交含有商业预测结论的代码或文档。

2. **查找或创建议题**：在 GitHub Issues 中搜索现有任务，若无重叠则新建议题并打上 `enhancement` 或 `bug` 标签，简要描述改进目标与预期效果。

3. **派生仓库并本地开发**：Fork 项目至个人账户，克隆后创建新分支 `feature/你的功能名`，建议分支名与议题编号关联，例如 `issue-42-add-odds-smoothing`。

4. **编写测试与文档**：所有新功能必须附带单元测试（覆盖率不低于 80%）及对应文档更新，运行 `pytest tests/` 确保全部用例通过。

5. **提交合并请求**：推送分支后发起 Pull Request，PR 描述中需包含“Closes #议题编号”字样，并附上本地运行日志截图或命令行输出。等待至少一位维护者审核，CI 流水线（含 lint、type-check、test）全部绿方可合并。

## 常见问题

**Q：项目是否提供即时的比赛结果预测或投注建议？**
A：不提供。SoccerScope 严格限定于历史数据统计与特征提取，所有输出均为客观数值或描述性文本，不包含任何“胜平负”概率推荐。用户自行承担基于本工具做决策的所有责任。

**Q：遇到数据源网站结构变更导致抓取失败，该如何处理？**
A：请先检查 `config/local.yaml` 中的 `user_agent` 和 `headers` 是否被目标站点拦截，尝试更新为最新浏览器 UA。若仍失败，可在 GitHub Issue 中附上目标页面片段（脱敏），维护者会尽快适配新结构。项目内置了 `fetcher/base.py` 中的 `retry` 装饰器，默认重试 3 次，指数退避。

**Q：缓存数据库占用空间增长过快，如何清理？**
A：执行 `python scripts/clean_cache.py --days 30` 即可删除 30 天前的比赛详情缓存，保留基础球队元数据。建议配合 crontab 每月自动清理一次。若需彻底重置，请使用 `--reset` 参数（会清空全部表）。

## 许可证

MIT License

Copyright (c) 2026 SoccerScope Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
