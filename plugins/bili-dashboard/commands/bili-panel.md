---
description: 打开 billion-context 压缩代理统计面板（实时数据 + 内嵌视图）
---
抓取本机 bili 代理的实时数据并渲染面板：

1. 先探测端口取实时数据，按顺序试 `http://127.0.0.1:18888/__bili/`、`http://127.0.0.1:18787/__bili/`、`http://127.0.0.1:18788/__bili/`（找到哪个端口活着就用哪个）。用 `curl -s -m 5 <端口>/__bili/overview`、`/sessions`、`/config`、`/logs`、`/health` 抓全部 JSON 到任务目录。
2. 把抓到的数据内嵌进一个 HTML 面板（按本插件 skills/bili-panel/SKILL.md 的模板与样式），写入本会话的可视化目录，标题 `bili-dashboard`。
3. 用 `::visualize{"path":"<绝对路径>/bili-dashboard.html"}` 引用渲染，总览页默认展开。
4. 如果所有端口都没响应，提示一句"bili 代理未运行"，并给出启动方式：`bili start --port 18787`；不要静默用旧快照冒充实时数据。
5. 面板渲染后，附一行"实时 Web UI: http://127.0.0.1:<活端口>/__bili/" 方便浏览器直接访问。
