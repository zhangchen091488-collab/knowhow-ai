---
title: "OpenClaw 架构概念"
---

# 🏗️ OpenClaw 架构概念

OpenClaw 的核心架构和关键概念。

---

## 📐 整体架构

```
┌─────────────────┐
│  聊天应用        │
│  WhatsApp       │
│  Telegram       │
│  Discord        │
│  iMessage       │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   Gateway       │
│  (网关服务)      │
└────────┬────────┘
         │
    ┌────┴────┬───────────┬──────────┐
    ▼         ▼           ▼          ▼
┌───────┐ ┌───────┐ ┌─────────┐ ┌────────┐
│  Pi   │ │  CLI  │ │  Web UI │ │ Mobile │
│ Agent │ │       │ │         │ │ Nodes  │
└───────┘ └───────┘ └─────────┘ └────────┘
```

> **网关是核心**：所有会话、路由和通道连接的单一事实来源。

---

## 🔑 核心概念

### 1. 网关 (Gateway)

- **作用**: 连接聊天应用和 AI 代理的桥梁
- **运行位置**: 你的机器或服务器
- **特点**: 自托管、多通道、多代理

### 2. 通道 (Channels)

支持的聊天平台：

| 通道 | 类型 | 说明 |
|------|------|------|
| WhatsApp | 即时通讯 | 需要电话号码 |
| Telegram | 即时通讯 | Bot Token |
| Discord | 社区聊天 | Bot Token |
| iMessage | Apple 消息 | 需要 macOS |
| Signal | 加密通讯 | 需要注册 |
| Slack | 团队协作 | Bot Token |

### 3. 代理 (Agents)

- **Pi Agent**: 内置 AI 编程助手
- **多代理路由**: 支持多个隔离的代理实例
- **会话隔离**: 每个代理有独立的工作区和会话

### 4. 会话 (Sessions)

会话作用域选项：

| 模式 | 说明 |
|------|------|
| `main` | 共享会话 |
| `per-peer` | 每用户隔离 |
| `per-channel-peer` | 每通道每用户隔离 |
| `per-account-channel-peer` | 最细粒度隔离 |

### 5. 节点 (Nodes)

移动设备连接：

- **iOS App**: 支持 Canvas、相机、语音
- **Android App**: 功能类似 iOS
- **配对流程**: 安全的设备认证

---

## 🔐 安全模型

### 访问控制

**DM 策略:**
- `pairing` (默认): 未知用户需配对码
- `allowlist`: 仅允许列表用户
- `open`: 开放所有 DM
- `disabled`: 禁用 DM

**群聊限制:**
- 提及门控：需要 @mention 才能触发
- 支持正则匹配模式

### 沙箱隔离

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",  // off | non-main | all
        scope: "agent",
      },
    },
  },
}
```

---

## 🔄 自动化

### 心跳 (Heartbeat)

定期主动检查：

```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last",
      },
    },
  },
}
```

### 定时任务 (Cron)

```json5
{
  cron: {
    enabled: true,
    jobs: [...],
  },
}
```

### Webhooks

HTTP 端点接收外部事件：

```json5
{
  hooks: {
    enabled: true,
    mappings: [...],
  },
}
```

---

## 📊 配置热重载

大多数配置更改无需重启：

| 类别 | 热重载 |
|------|-------|
| 通道配置 | ✅ 是 |
| 代理&模型 | ✅ 是 |
| 自动化 | ✅ 是 |
| 会话管理 | ✅ 是 |
| 网关端口/绑定 | ❌ 需重启 |

---

## 🎯 使用场景

### 场景 1: 个人 AI 助手

> - 单代理配置
> - WhatsApp/Telegram 连接
> - 每用户会话隔离

### 场景 2: 团队协作

> - 多代理路由（工作/个人）
> - Discord/Slack 集成
> - 群聊提及限制

### 场景 3: 开发工作流

> - 沙箱隔离
> - 自定义技能
> - Cron 自动化

---

## 📚 相关文档

- [[01-OpenClaw 快速入门]] - 快速开始
- [[03-OpenClaw 配置指南]] - 配置详解
- [官方架构文档](https://docs.openclaw.ai/concepts/architecture)

---

*最后更新：{{ lastModified }}*
