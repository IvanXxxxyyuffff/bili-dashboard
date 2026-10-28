---
name: bili-panel
description: "渲染 billion-context 压缩代理统计面板。当用户要求查看 bili 面板、压缩统计、token 节省、缓存命中率、会话列表时使用。数据源为本机 bili 代理的 __bili 管理 API。"
---

# bili-panel 面板渲染规范

## 数据抓取

bili 管理端点（loopback-only，仅本机可访问）：

- `/__bili/health` — 版本/commit/pid/端口/启动时间
- `/__bili/overview` — 全局统计：sessions、requests、inputTokens、cachedTokens、outputTokens、grossSavedTotal、netSavedTotal、repayTotal、summaryCostTotal、hitPct、missNewTotal、missCompTotal、missTtlTotal、byProtocol[]、recent[]
- `/__bili/sessions` — 全部会话明细（title、clientHint、live、requests、foldCount、contextTokens、contextWindow、netSaved、cacheHitPct、lastSeen）
- `/__bili/config` — 配置文件路径、providers、压缩参数
- `/__bili/logs` — 日志尾部（path、total、lines[]）

端口探测顺序：18888 → 18787 → 18788。用 curl 抓到任务目录后内嵌进 HTML。

## 视图约束（ZCode 内嵌视图硬性规则）

- 只写 HTML fragment：无 `<!doctype>`/`<html>`/`<head>`/`<body>`，根元素用 `id="bili-dash"`
- **禁止 fetch/XHR**（内嵌视图沙箱不允许）——所有数据在生成面板时用 curl 抓好，以 `<script>window.BILI_DATA={...}</script>` 内嵌
- 根元素必须用 `document.getElementById('bili-dash')` 定位
- 数据 < 1 MB；日志只内嵌尾部 200 行
- 通过 `::visualize{"path":"<绝对路径>"}` 引用；宽面板加 `"mode":"wide"`

## 样式（对齐 DSH 面板）

深色：背景 #161618、卡片 #222226、边框 #3a3a40、文字 #e8e8ea、次级 #9a9aa2、节省数字绿 #4ade80、协议 pill 蓝描边。标签页：总览/会话/配置/日志/版本。

### 总览页结构

副标题"全局统计：覆盖内存与磁盘上的所有会话"，8 卡片三列网格（会话总数[含 N 运行中 · N 历史]、请求总数、压缩累计节省[绿]、净节省[绿]、全局缓存命中率[带说明]、输入/缓存命中/输出 token），下方"按协议分布"表（协议列为蓝色 pill）与"最近会话"表。

### 数字格式

≥1e9 → `x.xxB`；≥1e6 → `x.xM`；≥1e3 → `x.xK`；其余整数。

### 标签页交互

纯本地 JS 切换（tab onclick 改显示区块），会话表按 lastSeen 倒序，日志可按 warn/acp/session 过滤。

## 完成动作

面板写好后引用渲染，另起一行给出浏览器直链 `http://127.0.0.1:<活端口>/__bili/`。
