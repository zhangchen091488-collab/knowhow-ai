---
title: "04-💻OpenClaw CLI 参考"
---

# 💻 OpenClaw CLI 参考

OpenClaw 命令行工具的完整参考。

---

## 📋 命令分类

### 网关管理

| 命令 | 说明 |
|------|------|
| `openclaw gateway status` | 检查网关状态 |
| `openclaw gateway start` | 启动网关服务 |
| `openclaw gateway stop` | 停止网关服务 |
| `openclaw gateway restart` | 重启网关服务 |
| `openclaw gateway --port 18789` | 前台运行网关 |

---

### 安装与配置

| 命令 | 说明 |
|------|------|
| `openclaw onboard` | 交互式配置向导 |
| `openclaw configure` | 配置编辑器 |
| `openclaw config get <key>` | 获取配置值 |
| `openclaw config set <key> <value>` | 设置配置值 |
| `openclaw config unset <key>` | 删除配置项 |
| `openclaw update` | 更新 OpenClaw |
| `openclaw uninstall` | 卸载 OpenClaw |

---

### 通道管理

| 命令 | 说明 |
|------|------|
| `openclaw channels login` | 登录通道 |
| `openclaw channels list` | 列出已配置通道 |
| `openclaw pairing` | 配对管理 |

---

### 消息与会话

| 命令 | 说明 |
|------|------|
| `openclaw message send --target <用户> --message <内容>` | 发送消息 |
| `openclaw sessions list` | 列出会话 |
| `openclaw sessions history <sessionKey>` | 查看会话历史 |

---

### 诊断与调试

| 命令 | 说明 |
|------|------|
| `openclaw doctor` | 诊断配置问题 |
| `openclaw status` | 显示系统状态 |
| `openclaw logs` | 查看日志 |
| `openclaw health` | 健康检查 |

---

### 其他工具

| 命令 | 说明 |
|------|------|
| `openclaw dashboard` | 打开浏览器控制 UI |
| `openclaw agents list` | 列出代理 |
| `openclaw models` | 管理模型 |
| `openclaw skills` | 管理技能 |
| `openclaw cron` | 定时任务管理 |
| `openclaw hooks` | Webhook 管理 |
| `openclaw memory` | 内存管理 |
| `openclaw plugins` | 插件管理 |

---

## 🚀 常用命令示例

### 快速检查
```bash
# 检查网关状态
openclaw gateway status

# 运行诊断
openclaw doctor

# 查看状态
openclaw status
```

### 配置管理
```bash
# 查看工作区配置
openclaw config get agents.defaults.workspace

# 设置心跳间隔
openclaw config set agents.defaults.heartbeat.every "2h"

# 设置主模型
openclaw config set agents.defaults.model.primary "anthropic/claude-sonnet-4-5"
```

### 消息测试
```bash
# 发送测试消息
openclaw message send --target +15555550123 --message "Hello from OpenClaw"
```

### 日志查看
```bash
# 查看实时日志
openclaw logs -f

# 查看最近 100 行
openclaw logs --tail 100
```

---

## 🌐 环境变量

| 变量 | 说明 |
|------|------|
| `OPENCLAW_HOME` | 自定义主目录 |
| `OPENCLAW_STATE_DIR` | 自定义状态目录 |
| `OPENCLAW_CONFIG_PATH` | 自定义配置文件路径 |

---

## 📚 相关文档

- [[OpenClaw 快速入门]] - 快速开始
- [[OpenClaw 配置指南]] - 配置详解
- [官方 CLI 参考](https://docs.openclaw.ai/cli)

---

*最后更新：{{ lastModified }}*
