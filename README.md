# bili-dashboard — ZCode 插件

billion-context 压缩代理统计面板。在 ZCode 里输入 `/bili-panel`，一键打开实时数据面板：会话总数、token 节省、缓存命中率、会话明细、代理日志。

数据来自本机 [billion-context（bili）](https://github.com/ranxianglei/billion-context)代理的管理 API（loopback-only），不经过任何外部服务。

## 前置条件

装了本插件 + 本机运行着 bili 代理（ZCode 通过 `bili plugin install zcode` 接入后每次会话自动拉起；或手动 `bili start`）。

## 安装

在 ZCode 的终端（或任何终端）执行两步：

```bash
# 1. 添加本市场（仓库即市场）
zcode plugins marketplace add IvanXxxxyyuffff/bili-dashboard

# 2. 安装插件
zcode plugins install bili-dashboard@bili-dashboard-marketplace
```

也可以在 ZCode 桌面端：设置 → 插件 → 插件市场 → 添加 → 输入 `IvanXxxxyyuffff/bili-dashboard`，然后搜索 "bili" 安装。

## 使用

任意 ZCode 会话输入：

```
/bili-panel
```

或直接说"打开 bili 面板 / 看下压缩统计"。面板内五个标签页：总览（8 项统计 + 按协议分布 + 最近会话）、会话（全部会话明细）、配置（压缩参数 + provider 模型窗口）、日志（尾部 200 行可过滤）、版本（代理运行状态）。

浏览器直链：`http://127.0.0.1:18787/__bili/`（端口以实际运行实例为准）。

## 卸载

```bash
zcode plugins uninstall bili-dashboard@bili-dashboard-marketplace
zcode plugins marketplace remove bili-dashboard-marketplace
```

## 作者

IVAN · MIT License
