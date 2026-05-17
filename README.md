轻量化可落地实战训练项目
🔥 项目核心亮点
全链路手写：拒绝黑盒调用，从数据采集到监控告警全程手写核心逻辑
一站式部署：Docker Compose 一键启动所有服务，零环境配置成本
混沌工程：内置故障注入工具，支持一键模拟生产故障，实战化演练
自动化运维：配置热重载、CI/CD 流水线、自动告警，贴近企业真实场景
体系化学习：配套架构文档、技术笔记、故障复盘案例，边做边学
🎯 适用人群
想要从零搭建企业级监控链路的开发者
希望掌握 Prometheus+Grafana 监控体系的运维工程师
想学习混沌工程、故障复盘的后端 / 运维从业者
寻找轻量化实战项目的学生与技术爱好者
📌 项目概述
本项目是一套面向实战学习的完整落地项目，旨在帮助开发者从零搭建一套完整的业务运行链路。
覆盖数据采集 → 服务部署 → 指标监控 → 告警通知 → 故障注入 → 故障复盘全流程，全程手写核心逻辑，拒绝第三方黑盒依赖，让你真正吃透底层原理，掌握企业级生产环境必备技能。
🚀 项目进度
阶段 1 - 基础链路实现 ✅
已完成全量基础功能交付：
核心业务逻辑解析器开发，支持多源数据采集、清洗、处理与标准化输出
统一标准接口服务开发，暴露 HTTP 调用入口，兼容 Prometheus 指标采集
集成 Grafana 可视化监控面板，实现 CPU、内存、负载、队列等核心指标实时展示
集成 Alertmanager 告警模块，支持邮件自动通知，异常情况实时推送
完成.env 环境配置文件，支持服务端口、告警阈值、推送参数自定义
阶段 2 - 故障实验与复盘 ✅
已完成混沌工程与故障复盘体系搭建：
自研混沌工程工具，支持 CPU 满载、内存溢出、网络延迟、队列阻塞等故障一键注入
自动故障快照机制，故障触发时自动保存指标、日志、现场数据，便于回溯
标准化 Postmortem 故障复盘报告模板，规范根因分析、改进措施流程
输出多套真实故障复盘案例，沉淀可复用的故障定位与分析方法论
阶段 3 - 高级特性与学习文档 ✅
已完成工程化优化与配套学习资料：
核心技术深度学习笔记，覆盖监控原理、Exporter 开发、PromQL 语法全知识点
配置热重载机制，修改.env 配置无需重启服务，自动加载最新参数
完整 GitHub Actions CI/CD 流水线，实现自动测试→构建→推送镜像全流程
全链路指标验证方案，确保采集数据与系统原生数据一致
⚡ 快速开始
前置依赖
Git
Docker & Docker Compose
部署步骤
# 1. 克隆项目到本地
git clone https://github.com/你的用户名/你的项目名.git
cd 你的项目名

# 2. 复制环境变量模板并修改配置
cp .env.example .env
vim .env
# 需修改：告警阈值、服务端口、邮箱通知配置、服务器IP等参数

# 3. 一键启动所有服务
docker compose up -d

# 4. 验证指标暴露是否正常
curl http://localhost:9100/metrics

# 5. 访问Grafana监控大盘
# 地址：http://你的服务器IP:3000
# 默认账号：admin  默认密码：admin
🐳 Docker Compose 服务一览
服务名称	镜像版本	核心功能	暴露端口
collector	你的项目 /collector:latest	核心数据采集、业务逻辑处理	内部通信
exporter	你的项目 /exporter:latest	Prometheus 指标格式暴露	9100
prometheus	prom/prometheus:v2.47.0	指标存储、告警规则计算、数据拉取	9090
grafana	grafana/grafana:10.1.0	监控数据可视化、大盘展示	3000
alertmanager	prom/alertmanager:v0.26.0	告警聚合、分发、邮件通知	9093
📂 Project Layout / 项目结构
./
├── collector/               # 核心业务逻辑实现、数据采集处理器
├── exporter/                # Prometheus Exporter开发、指标暴露
├── config/                  # Prometheus/Alertmanager配置文件模板
├── grafana/                 # Grafana Dashboard JSON导入文件
├── chaos/                   # 混沌工程故障注入脚本、工具集
├── docs/                    # 架构文档、技术笔记、故障复盘案例
├── tests/                   # 单元测试、集成测试、指标验证用例
├── main.py                  # 项目独立CLI命令行入口
├── docker-compose.yml       # 多服务编排、一键部署配置
├── .env.example             # 环境变量配置示例文件
├── .github/workflows/       # GitHub Actions CI/CD流水线配置
└── README.md                # 项目说明文档
⚙️ 配置说明
所有核心配置统一存放于 .env 文件，支持配置热重载，修改后无需重启服务自动生效：
ini
# ====================== 核心告警阈值 ======================
# 核心资源使用率阈值(%)
CORE_HIGH_THRESHOLD=80
# 缓存使用率阈值(%)
CACHE_HIGH_THRESHOLD=85
# 系统负载阈值
LOAD_HIGH_THRESHOLD=4
# 消息队列堆积阈值
QUEUE_HIGH_THRESHOLD=1000

# ====================== 告警通知配置 ======================
# 接收告警邮箱
ALERT_EMAIL_TO=your-email@example.com
# SMTP服务器地址
SMTP_HOST=smtp.example.com
# SMTP端口
SMTP_PORT=587
# SMTP账号
SMTP_USER=your-user
# SMTP密码/授权码
SMTP_PASS=your-pass
📊 Metric Validation / 指标验证
为保证采集数据准确可靠，可使用系统原生命令进行交叉验证：
核心处理能力：对比 top / mpstat 输出
缓存使用情况：对比 free -h 输出
系统负载情况：对比 cat /proc/loadavg 输出
连接状态统计：对比 ss -ant | awk '{print $1}' | sort | uniq -c 输出
🔁 CI/CD Pipeline / 自动化流水线
项目内置 GitHub Actions 全自动化流水线，无需人工干预：
流水线阶段	触发条件	执行内容
CI 单元测试	向 main 分支 Push、创建 PR	代码检出、Python 环境配置、依赖安装、单元测试执行
CD 构建推送	main 分支 Push、打版本 Tag (CI 通过后)	Docker Hub 登录、多架构镜像构建、镜像自动推送
🛣️ Roadmap / 版本历史
v1.0.0 (2024-05-01)：首个稳定版，完成全部核心监控链路
v1.1.0 (2024-05-15)：完成混沌工程、故障快照、复盘模板
v1.2.0 (2024-06-01)：完成热重载、CI/CD、全链路验证
v1.3.0 (2024-06-15)：规划支持 eBPF 高级追踪能力
v1.4.0 (2024-07-01)：规划适配 Kubernetes 云原生环境
📚 学习文档
docs/architecture.md：项目整体架构设计详解
docs/tech_notes.md：Prometheus/Grafana 核心技术笔记
docs/postmortem-case1.md：故障复盘案例 1
docs/postmortem-case2.md：故障复盘案例 2
🤝 贡献指南
Fork 本项目
创建特性分支 (git checkout -b feature/AmazingFeature)
提交修改 (git commit -m 'Add some AmazingFeature')
推送到分支 (git push origin feature/AmazingFeature)
提交 Pull Request
❓ 常见问题 FAQ
Q：启动后 Grafana 看不到数据？
A：检查 Prometheus 是否正常拉取 exporter 指标，确认服务器 IP 配置正确。
Q：告警邮件收不到？
A：检查 SMTP 配置、授权码、邮箱安全设置，确认 Alertmanager 日志无报错。
Q：如何注入故障进行测试？
A：进入 chaos 目录，执行对应故障脚本，即可一键模拟异常场景。
📄 License
本项目基于 MIT License 开源，可自由使用、修改、分发。
⭐ 支持项目
如果这个项目对你有帮助，欢迎点个 Star 支持一下！谢谢。
