---
description: 打开 billion-context 压缩代理统计面板（实时数据 + 内嵌视图）
---
抓取本机 bili 代理的实时数据并渲染面板。按以下顺序执行，覆盖"从未装过 bili"的全新机器：

1. **探测**：按顺序试 `http://127.0.0.1:18888/__bili/health`、`18787`、`18788`（curl -s -m 3）。有活的端口就用它，跳到第 4 步。
2. **未探测到 → 检查是否安装过**：`bili --version`。
   - 命令不存在 → 告知用户"本机还没装 billion-context（bili），面板依赖它采集数据"，然后直接执行 `npm install -g billion-context`（Windows/macOS/Linux 通用，node ≥20 即可）。装完继续。
   - 命令存在 → 只是代理没在跑，继续第 3 步。
3. **启动代理**：后台执行 `bili start --port 18787`（run_in_background，不等它退出），轮询 `http://127.0.0.1:18787/__bili/health` 直到 200（最多 20 秒）。仍失败则停下来报告启动日志，不要渲染假面板。
4. **抓数据**：用 `curl -s -m 5 http://127.0.0.1:<端口>/__bili/overview`、`/sessions`、`/config`、`/logs`、`/health` 抓全部 JSON 到任务目录。
5. **渲染**：数据内嵌进 HTML 面板（按本插件 skills/bili-panel/SKILL.md 的模板与样式），写入本会话的可视化目录，标题 `bili-dashboard`，用 `::visualize{"path":"<绝对路径>/bili-dashboard.html"}` 引用，总览页默认展开。
6. **收尾**：附一行"实时 Web UI: http://127.0.0.1:<活端口>/__bili/"。如果第 2-3 步做了安装/启动，提醒用户：想让 ZCode 会话流量也走 bili 压缩（面板只是看数，压缩要接入才生效），执行 `bili plugin install zcode` 并重启 ZCode。
