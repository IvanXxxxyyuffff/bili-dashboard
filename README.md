# bili-dashboard — ZCode 插件

billion-context 压缩代理统计面板。在 ZCode 里输入 `/bili-panel`，一键打开实时数据面板：会话总数、token 节省、缓存命中率、会话明细、代理日志。

数据来自本机 [billion-context（bili）](https://github.com/ranxianglei/billion-context)代理的管理 API（loopback-only），不经过任何外部服务。**bili 与任何客户端（DSH/Claude Code/Codex 等）无关，是独立 npm 包，没有 DSH 也能用。**

## 全新机器安装（从零开始）

前提：装了 [ZCode](https://z.ai) 和 Node.js ≥ 20。

```bash
# 1. 添加本市场（仓库即市场）
zcode plugins marketplace add IvanXxxxyyuffff/bili-dashboard

# 2. 安装插件
zcode plugins install bili-dashboard@bili-dashboard-marketplace

# 3.（可选）安装 bili 本体——不装也行，第一次 /bili-panel 会自动装
npm install -g billion-context
```

也可以在 ZCode 桌面端：设置 → 插件 → 插件市场 → 添加 → 输入 `IvanXxxxyyuffff/bili-dashboard`，然后搜索 "bili" 安装。

## 使用

任意 ZCode 会话输入：

```
/bili-panel
```

第一次运行时的行为：

- 本机没装 bili → 自动 `npm install -g billion-context` 并启动代理，然后出面板
- 装了但没跑 → 自动后台 `bili start --port 18787`，然后出面板
- 已在跑 → 直接出面板

想让自己的 ZCode 会话流量也走 bili 压缩（省 token、长会话不丢上下文——面板只是看数，压缩要接入才生效），执行一次：

```bash
bili plugin install zcode
```

然后重启 ZCode。之后每个会话自动接入，`/bili-panel` 能看到会话数和节省量开始增长。

## 面板内容

五个标签页：总览（8 项统计 + 按协议分布 + 最近会话）、会话（全部会话明细）、配置（压缩参数 + provider 模型窗口）、日志（尾部 200 行可过滤）、版本（代理运行状态）。

浏览器直链：`http://127.0.0.1:18787/__bili/`（端口以实际运行实例为准）。

## 卸载

```bash
zcode plugins uninstall bili-dashboard@bili-dashboard-marketplace
zcode plugins marketplace remove bili-dashboard-marketplace
npm uninstall -g billion-context   # 如果不再需要 bili
```

## 作者

IVAN · MIT License
