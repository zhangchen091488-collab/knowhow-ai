---
title: "OpenClaw 安装配置指南"
---

# 🦞 OpenClaw 安装配置指南

> OpenClaw 是一个自托管网关，连接 WhatsApp、Telegram、Discord、iMessage 等聊天应用到 AI 编程助手。

---

## 📚 文档索引

- [[01-OpenClaw 快速入门]] - 从零开始安装和配置
- [[02-OpenClaw 安装方法]] - 详细安装方法和系统要求
- [[03-OpenClaw 配置指南]] - 配置文件详解和常用任务
- [[04-OpenClaw CLI 参考]] - 命令行工具使用
- [[05-OpenClaw 架构概念]] - 核心概念和架构说明

---

## 🚀 快速开始

### 1. 安装 OpenClaw

**macOS / Linux / WSL2:**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 2. 运行向导
```bash
openclaw onboard --install-daemon
```

### 3. 检查状态
```bash
openclaw gateway status
openclaw dashboard  # 打开浏览器控制 UI
```

---

## 🎯 核心能力

| 能力 | 说明 |
|------|------|
| **多通道网关** | WhatsApp、Telegram、Discord、iMessage 等 |
| **多代理路由** | 隔离的工作空间和会话 |
| **媒体支持** | 图片、音频、文档 |
| **Web 控制 UI** | 浏览器仪表盘 |
| **移动端节点** | iOS/Android 节点支持 |

---

## 📖 官方文档

- **官方文档:** https://docs.openclaw.ai
- **GitHub:** https://github.com/openclaw/openclaw
- **Discord:** https://discord.com/invite/clawd

---

*最后更新：{{ lastModified }}*
