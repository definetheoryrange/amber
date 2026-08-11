# DSZQ 技术资源导航站

DSZQ 是一个面向开发者和技术研究人员的开源外链资源聚合与导航系统，专注于收录、分类与验证各类技术社区、数据查询接口、文档镜像及工具链入口。项目定位为「技术资源的中转枢纽」，解决开发者在多个垂直领域（如体育数据查询、网络诊断、合规性检查）中快速定位可用服务端点的痛点。

本项目不提供具体数据内容，而是构建一个可审计、可扩展的链接资源清单，配合自动化健康检查脚本，确保每个收录的域名在部署环境中可达且响应正常。适用于内部研发团队的基础设施建设、开源项目的依赖镜像配置，以及个人技术博客的快捷导航页搭建。

## 功能概览

- 资源清单结构化存储：所有外链以 YAML 格式维护，支持版本控制与差异对比。
- 自动可达性探测：集成 Python 脚本，定时向每个域名发送 HTTP HEAD 请求并记录状态码。
- 分组标签过滤：按地域、服务商、内容类型等维度为链接打标，便于动态生成视图。
- 静态页面生成：内置 Jinja2 模板引擎，可将资源列表渲染为纯 HTML 导航页，适配 Nginx 部署。
- 健康报告输出：每次探测生成 CSV 格式报告，含响应时间、SSL 证书有效期、重定向链。
- 命令行交互工具：提供 CLI 子命令，支持添加、移除、禁用单个资源条目。
- 外部监控集成：可对接 Prometheus 网关，将可用性指标暴露为监控数据源。

## 应用场景

- 内部技术中台导航：企业基础架构团队可用本项目搭建内部 DNS 别名与外部服务端点的对照表，减少开发人员查找公网文档或 API 入口的耗时。
- 开源项目依赖镜像配置：当开源项目需要引用多个外部数据源或 CDN 时，可将本项目收录的链接作为候选列表，在构建流水线中自动测试并选用响应最快的端点。
- 个人技术博客侧边栏：独立开发者可将生成的静态页面嵌入个人网站，作为「常用工具」或「友链」板块，利用健康检查脚本自动剔除失效链接，降低手动维护成本。
- 网络诊断前置工具：运维人员可结合本项目的探测功能，快速判断特定区域或运营商环境下多个备选域名的可达性差异，辅助路由决策。

## 快速开始

以下命令适用于 Linux/macOS 环境，Windows 用户可使用 WSL2 或 Git Bash。

```bash
# 克隆仓库
git clone https://github.com/dszq-tech/navigation-hub.git
cd navigation-hub

# 安装 Python 依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# 初始化本地配置文件
cp config.example.yaml config.yaml

# 执行一次全量健康检查并生成静态页面
python cli.py check --all --output ./dist/index.html

# 启动本地开发服务器预览
python -m http.server 8000 --directory ./dist
```

访问 `http://localhost:8000` 即可查看生成的导航页面。如需定时运行，可将 `cli.py check --all` 加入 crontab。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 及以上 | 核心运行环境，用于探测脚本与模板渲染 |
| pip | 21.0 及以上 | Python 包管理工具，用于安装依赖库 |
| requests | 2.28.0 及以上 | HTTP 请求库，用于发送探测请求 |
| pyyaml | 6.0 及以上 | YAML 配置文件解析 |
| jinja2 | 3.1.0 及以上 | 模板引擎，用于生成静态 HTML |
| pytest | 7.0.0 及以上 | 仅开发测试时需要，用于单元测试 |
| curl | 7.68.0 及以上 | 可选，用于外部调用探测，非 Python 实现时备用 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户指南 | docs/user-guide/quick-start.md | 如何首次运行、配置个性化分组、自定义输出模板 |
| 运维手册 | docs/ops/deployment.md | 如何部署到生产环境（Nginx、Docker、Systemd） |
| 开发参考 | docs/dev/api.md | 各模块函数签名、配置项字段含义及扩展钩子 |
| 故障排查 | docs/troubleshooting/common-issues.md | 探测超时、SSL 错误、页面空白等问题的处理步骤 |

完整文档位于 `docs/` 目录，建议从 `docs/index.md` 开始阅读。

## 资源列表

本导航站当前收录并维护以下外部链接。所有链接均按原始输入格式逐条列出，未做任何协议或主机名字段修改。

体育数据类查询入口

<code>dszuqiubifen1.net.cn</code>

<code>dszuqiubifengw.com.cn</code>

赛事版权或合规类参考站点

<code>dszuqiubanquanchang.net.cn</code>

<code>dszuqiubanquanchang.com.cn</code>

<code>dszuqiubanquanchang.cn</code>

<code>dszuqiubanquanchang.org.cn</code>

综合网关或聚合入口

<code>dszuqiugw.org.cn</code>

## 项目结构

```
navigation-hub/
├── cli.py                         # 命令行入口，解析子命令并调用核心逻辑
├── config.example.yaml            # 示例配置文件，含探测超时、重试次数、分组定义
├── requirements.txt               # Python 依赖列表
├── src/                           # 核心源代码目录
│   ├── checker/                   # 健康检查模块
│   │   ├── __init__.py            # 模块初始化，导出公开类
│   │   ├── http_probe.py          # 实现 HTTP/HTTPS 探测，处理重定向与证书
│   │   └── reporter.py            # 生成 CSV 及 JSON 格式报告
│   ├── parser/                    # 配置解析模块
│   │   ├── __init__.py
│   │   └── yaml_loader.py         # 读取 YAML 资源列表，校验必填字段
│   ├── renderer/                  # 静态页面渲染模块
│   │   ├── __init__.py
│   │   ├── template_engine.py     # 封装 Jinja2 环境，加载模板文件
│   │   └── filters.py             # 自定义模板过滤器（例如格式化时间戳）
│   └── utils/                     # 通用工具函数
│       ├── __init__.py
│       ├── network.py             # 获取本机 IP、DNS 解析辅助
│       └── logger.py              # 统一日志配置，支持文件与控制台输出
├── templates/                     # Jinja2 模板目录
│   ├── base.html                  # 基础布局，含 CSS 框架引用
│   └── index.html                 # 资源列表主页面模板，循环渲染分组
├── tests/                         # 单元测试目录
│   ├── test_probe.py              # 模拟 HTTP 响应的探测测试
│   └── test_loader.py             # 配置文件加载与异常处理测试
├── docs/                          # 完整项目文档
│   ├── index.md                   # 文档首页
│   ├── user-guide/                # 用户指南子目录
│   └── ops/                       # 运维相关手册
└── dist/                          # 构建输出目录（默认生成静态页面的位置）
    └── index.html                 # 由 cli.py 渲染生成的最终导航页
```

## 贡献指南

1. 查阅 `docs/dev/api.md` 了解模块划分与编码规范，确保新增代码符合 PEP 8 风格，函数及类需包含 docstring。
2. 在 `src/parser/yaml_loader.py` 中增加自定义校验逻辑后，需在 `tests/test_loader.py` 同步添加对应的正向与异常用例，并执行 `pytest` 确保全部测试通过。
3. 若需添加新的输出格式（例如 JSON API 响应），请在 `src/checker/reporter.py` 中新增方法，并在 `cli.py` 中暴露对应的命令行选项。
4. 修改模板文件时，请确认 `templates/base.html` 的 CSS 类名与 `src/renderer/template_engine.py` 中的渲染上下文变量严格一致。
5. 提交前执行 `python cli.py check --all` 进行端到端验证，并确认生成的 `dist/index.html` 在本地浏览器中无控制台报错。

## 常见问题

Q: 探测时出现 `SSL: CERTIFICATE_VERIFY_FAILED` 错误，如何解决？
A: 默认启用了证书验证。若目标站点使用自签名证书或处于内网环境，可在 `config.yaml` 中设置 `verify_ssl: false`。注意该选项会降低安全性，仅建议在受信任网络中使用。

Q: 如何添加一个新的分组并让其在生成的页面中独立显示？
A: 在 `config.yaml` 的 `groups` 字段下新增一个条目，指定 `name` 和 `description`，然后在 `resources` 列表中为每个链接指定 `group` 值匹配该名称。重新运行 `cli.py check --all` 即可。

Q: 定时任务运行探测时报告 `ConnectionTimeout`，但手动 curl 可以访问？
A: 检查定时任务的环境变量（例如 `http_proxy`、`https_proxy`）是否与交互式 Shell 一致。另可调整 `config.yaml` 中的 `timeout` 值（单位秒），默认为 5 秒，可适当增大至 10 秒。

## 许可证

MIT License

Copyright (c) 2026 DSZQ Technology Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:20
