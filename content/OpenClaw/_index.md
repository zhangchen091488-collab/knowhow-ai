---
title: "OpenClaw 安装配置指南"
---

# 🦞 OpenClaw

> OpenClaw 是一个自托管网关，连接 WhatsApp、Telegram、Discord、iMessage 等聊天应用到 AI 编程助手。

---

## 📚 文档索引

| 文档 | 说明 | 难度 |
|------|------|:----:|
| [[OpenClaw 快速入门]] | 从零开始，5 分钟搭建可用网关 | ⭐ |
| [[OpenClaw 安装方法]] | 详细安装方法和系统要求 | ⭐ |
| [[OpenClaw 配置指南]] | 配置文件详解和常用任务 | ⭐⭐ |
| [[OpenClaw CLI 参考]] | 命令行工具完整参考 | ⭐⭐ |
| [[OpenClaw 架构概念]] | 核心概念和架构说明 | ⭐⭐ |
| [[OpenClaw 飞书插件使用指南]] | 飞书消息收发配置 | ⭐⭐ |
| [[OpenClaw 微信插件使用指南]] | 微信消息收发配置 | ⭐⭐ |
| [[OpenClaw 多 Agent 架构]] | 多 Agent 架构与工作空间隔离 | ⭐⭐⭐ |

---

## 🚀 快速开始

### 安装 OpenClaw

```bash
# macOS / Linux / WSL2
curl -fsSL https://openclaw.ai/install.sh | bash

# Windows (PowerShell)
iwr -useb https://openclaw.ai/install.ps1 | iex
```

### 运行配置向导

```bash
openclaw onboard --install-daemon
```

### 启动网关

```bash
openclaw gateway status
openclaw dashboard  # 打开浏览器控制 UI
```

> 访问 http://127.0.0.1:18789 打开控制台

---

## 🎯 核心能力

| 能力 | 说明 |
|------|------|
| 🌐 **多通道网关** | WhatsApp、Telegram、Discord、iMessage 等 |
| 🤖 **多代理路由** | 隔离的工作空间和会话 |
| 🖼️ **媒体支持** | 图片、音频、文档 |
| 🖥️ **Web 控制 UI** | 浏览器仪表盘 |
| 📱 **移动端节点** | iOS/Android 节点支持 |

---

## 📖 官方文档

| 资源 | 链接 |
|------|------|
| 📚 **官方文档** | https://docs.openclaw.ai |
| 🐙 **GitHub** | https://github.com/openclaw/openclaw |
| 💬 **Discord** | https://discord.com/invite/clawd |

---

## 📂 文档分类

### 🆕 入门系列
- [[OpenClaw 快速入门]] — 5 分钟快速上手
- [[OpenClaw 安装方法]] — 详细安装指南

### ⚙️ 配置系列
- [[OpenClaw 配置指南]] — 配置文件完全解析
- [[OpenClaw CLI 参考]] — CLI 命令大全

### 📖 概念系列
- [[OpenClaw 架构概念]] — 核心概念解读

### 🔌 插件系列
- [[OpenClaw 飞书插件使用指南]] — 飞书接入
- [[OpenClaw 微信插件使用指南]] — 微信接入
- [[OpenClaw 多 Agent 架构]] — 多 Agent 设计

---

*最后更新：{{ lastModified }}*
