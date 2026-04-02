---
title: "OpenClaw 配置指南"
---

# ⚙️ OpenClaw 配置指南

OpenClaw 的配置文件位于 `~/.openclaw/openclaw.json` (JSON5 格式)。

---

## 📝 配置文件基础

### 最小配置

```json5
// ~/.openclaw/openclaw.json
{
  agents: { 
    defaults: { 
      workspace: "~/.openclaw/workspace" 
    } 
  },
  channels: { 
    whatsapp: { 
      allowFrom: ["+15555550123"] 
    } 
  },
}
```

### 编辑配置的方式

| 方式 | 命令/方法 |
|------|---------|
| **交互式向导** | `openclaw onboard` 或 `openclaw configure` |
| **CLI 单行命令** | `openclaw config set <key> <value>` |
| **控制 UI** | http://127.0.0.1:18789 → Config 标签 |
| **直接编辑** | 编辑 `~/.openclaw/openclaw.json` |

> [!tip]
> 配置文件支持热重载！大多数修改无需重启网关。

---

## 🔑 常用配置任务

### 1. 配置通道 (WhatsApp/Telegram/Discord)

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",  // pairing | allowlist | open | disabled
      allowFrom: ["tg:123"],
    },
  },
}
```

各通道配置文档：
- [[WhatsApp 通道配置]]
- [[Telegram 通道配置]]
- [[Discord 通道配置]]

---

### 2. 配置 AI 模型

```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-5",
        fallbacks: ["openai/gpt-5.2"],
      },
      models: {
        "anthropic/claude-sonnet-4-5": { alias: "Sonnet" },
        "openai/gpt-5.2": { alias: "GPT" },
      },
    },
  },
}
```

---

### 3. 控制访问权限

**DM 访问策略:**
```json5
{
  channels: {
    telegram: {
      dmPolicy: "pairing",  // 默认：未知用户需配对码
      // dmPolicy: "allowlist",  // 仅允许列表用户
      // dmPolicy: "open",       // 开放所有 DM
      // dmPolicy: "disabled",   // 禁用 DM
      allowFrom: ["tg:123"],
    },
  },
}
```

---

### 4. 群聊提及限制

```json5
{
  agents: {
    list: [{
      id: "main",
      groupChat: {
        mentionPatterns: ["@openclaw", "openclaw"],
      },
    }],
  },
  channels: {
    whatsapp: {
      groups: { "*": { requireMention: true } },
    },
  },
}
```

---

### 5. 会话管理

```json5
{
  session: {
    dmScope: "per-channel-peer",  // 推荐：每通道每用户隔离
    threadBindings: {
      enabled: true,
      idleHours: 24,
      maxAgeHours: 0,
    },
    reset: {
      mode: "daily",
      atHour: 4,
      idleMinutes: 120,
    },
  },
}
```

---

### 6. 沙箱隔离

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",  // off | non-main | all
        scope: "agent",    // session | agent | shared
      },
    },
  },
}
```

首先构建沙箱镜像：
```bash
scripts/sandbox-setup.sh
```

---

### 7. 心跳检查

```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last",  // last | whatsapp | telegram | discord | none
      },
    },
  },
}
```

---

### 8. 定时任务 (Cron)

```json5
{
  cron: {
    enabled: true,
    maxConcurrentRuns: 2,
    sessionRetention: "24h",
    runLog: {
      maxBytes: "2mb",
      keepLines: 2000,
    },
  },
}
```

---

### 9. Webhooks

```json5
{
  hooks: {
    enabled: true,
    token: "shared-secret",
    path: "/hooks",
    mappings: [{
      match: { path: "gmail" },
      action: "agent",
      agentId: "main",
      deliver: true,
    }],
  },
}
```

---

### 10. 多代理路由

```json5
{
  agents: {
    list: [
      { id: "home", default: true, workspace: "~/.openclaw/workspace-home" },
      { id: "work", workspace: "~/.openclaw/workspace-work" },
    ],
  },
  bindings: [
    { agentId: "home", match: { channel: "whatsapp", accountId: "personal" } },
    { agentId: "work", match: { channel: "whatsapp", accountId: "biz" } },
  ],
}
```

---

## 🔥 配置热重载

网关会自动监听配置文件变化并应用：

| 模式 | 行为 |
|------|------|
| `hybrid` (默认) | 安全更改即时应用，关键更改自动重启 |
| `hot` | 仅应用安全更改，需要重启时记录警告 |
| `restart` | 任何更改都重启网关 |
| `off` | 禁用热重载 |

```json5
{
  gateway: {
    reload: { mode: "hybrid", debounceMs: 300 },
  },
}
```

### 热重载 vs 需要重启

| 类别 | 字段 | 需要重启？ |
|------|------|-----------|
| 通道 | `channels.*`, `web` | ❌ 否 |
| 代理&模型 | `agent`, `agents`, `models` | ❌ 否 |
| 自动化 | `hooks`, `cron`, `heartbeat` | ❌ 否 |
| 会话 | `session`, `messages` | ❌ 否 |
| 网关服务器 | `gateway.*` (端口、绑定、认证) | ✅ 是 |
| 基础设施 | `discovery`, `plugins` | ✅ 是 |

---

## 📄 拆分配置文件

使用 `$include` 组织大型配置：

```json5
// ~/.openclaw/openclaw.json
{
  gateway: { port: 18789 },
  agents: { $include: "./agents.json5" },
  broadcast: {
    $include: ["./clients/a.json5", "./clients/b.json5"],
  },
}
```

---

## 🔧 CLI 配置命令

```bash
# 获取配置
openclaw config get agents.defaults.workspace

# 设置配置
openclaw config set agents.defaults.heartbeat.every "2h"

# 删除配置
openclaw config unset tools.web.search.apiKey

# 应用完整配置
openclaw gateway call config.apply --params '{...}'
```

---

## 📚 相关文档

- [[01-OpenClaw 快速入门]] - 快速开始
- [[04-OpenClaw CLI 参考]] - CLI 工具详解
- [官方配置参考](https://docs.openclaw.ai/gateway/configuration-reference)

---

*最后更新：{{ lastModified }}*
