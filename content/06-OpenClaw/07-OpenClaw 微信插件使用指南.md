# OpenClaw 微信插件使用指南

> 让小龙虾（OpenClaw）通过微信与你沟通

[[06-OpenClaw/00-OpenClaw 文档索引|返回文档索引]]

## 概述

`@tencent-weixin/openclaw-weixin` 是 OpenClaw 的微信通道插件，支持通过二维码扫码授权登录，让你的微信账号可以直接与 AI 助手对话。

## 版本信息

| 插件版本 | OpenClaw 版本要求 | npm dist-tag |
|---------|------------------|--------------|
| **2.x** | >= 2026.3.22 | `latest`（推荐）|
| 1.x | >= 2026.1.0 < 2026.3.22 | `legacy` |

> ⚠️ 插件启动时会检查宿主版本，如果 OpenClaw 版本超出支持范围，插件将拒绝加载。

**当前最新版本：2.1.1**（发布于 2026-03-27）

## 前置要求

- 已安装 OpenClaw（`openclaw` CLI 可用）
- Node.js >= 22
- 一个微信账号

## 安装步骤

### 方式一：快速安装（推荐）

```bash
npx -y @tencent-weixin/openclaw-weixin-cli install
```

### 方式二：手动安装

如果快速安装不生效，按以下步骤操作：

#### 1. 安装插件

```bash
openclaw plugins install "@tencent-weixin/openclaw-weixin"
```

#### 2. 启用插件

```bash
openclaw config set plugins.entries.openclaw-weixin.enabled true
```

#### 3. 二维码登录

```bash
openclaw channels login --channel openclaw-weixin
```

终端会显示一个二维码，用手机微信扫描并确认授权。授权成功后，登录凭证会自动保存在本地，无需其他操作。

#### 4. 重启 Gateway

```bash
openclaw gateway restart
```

## 多账号支持

重复执行登录命令可以添加更多微信账号：

```bash
openclaw channels login --channel openclaw-weixin
```

每次二维码扫码登录会创建一个新的账号条目，支持**多个微信账号同时在线**。

## 上下文隔离配置

默认情况下，所有通道共享同一个 AI 对话上下文。如果你希望每个微信账号有独立的 AI 记忆（避免账号间上下文串扰），可以执行：

```bash
openclaw config set agents.mode per-channel-per-peer
```

这会让每个「微信账号 + 消息发送者」组合拥有独立的 AI 内存。

## 常见问题

### Q: 扫码后提示登录失败？

确保二维码扫描后点击了"确认授权"按钮，部分设备需要多次确认。

### Q: 如何查看当前登录的账号？

可以使用 `openclaw channels list` 查看已连接的通道和账号。

### Q: 如何退出登录？

删除插件目录下的认证文件即可（具体路径取决于 OpenClaw 数据目录）。

### Q: 提示 "requires OpenClaw >=2026.3.22" 报错？

你的 OpenClaw 版本太旧，不兼容当前插件版本。检查版本：

```bash
openclaw --version
```

如果版本低于 2026.3.22，需要升级 OpenClaw 或安装旧版插件：

```bash
openclaw plugins install @tencent-weixin/openclaw-weixin@legacy
```

### Q: Channel 显示 "OK" 但未连接？

确保 `~/.openclaw/openclaw.json` 中 `plugins.entries.openclaw-weixin.enabled` 为 `true`：

```bash
openclaw config set plugins.entries.openclaw-weixin.enabled true
openclaw gateway restart
```

## 卸载插件

```bash
openclaw openclaw-weixin uninstall
```

---

*文档更新时间：2026-03-29*
*插件版本：2.1.1*
