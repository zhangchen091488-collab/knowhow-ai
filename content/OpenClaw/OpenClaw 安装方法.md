---
title: "02-📦OpenClaw 安装方法"
---

# 📦 OpenClaw 安装方法

详细的安装方法、系统要求和故障排除。

---

## 💻 系统要求

| 要求 | 说明 |
|------|------|
| **Node.js** | 24 (推荐) 或 22 LTS (`22.16+`) |
| **操作系统** | macOS、Linux、Windows (推荐 WSL2) |
| **包管理器** | `pnpm` (仅源码构建需要) |

> [!warning]
> Windows 用户强烈建议使用 [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install)

---

## 🚀 安装方法

### 方法 1: 安装脚本 (推荐) ⭐

自动处理 Node 检测、安装和配置。

**macOS / Linux / WSL2:**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

**跳过向导（仅安装 CLI）:**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

---

### 方法 2: npm / pnpm

适合自行管理 Node 环境的用户。

**npm:**
```bash
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

**遇到 sharp 构建错误？**
```bash
# macOS 有全局 libvips 时
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

**pnpm:**
```bash
pnpm add -g openclaw@latest
pnpm approve-builds -g  # 批准构建脚本
openclaw onboard --install-daemon
```

---

### 方法 3: 源码安装

适合贡献者或需要自定义的用户。

```bash
# 1. 克隆仓库
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 2. 构建
pnpm install
pnpm ui:build
pnpm build

# 3. 全局链接
pnpm link --global

# 4. 运行向导
openclaw onboard --install-daemon
```

---

### 其他安装方式

| 方式 | 适用场景 |
|------|---------|
| [Docker](https://docs.openclaw.ai/install/docker) | 容器化部署 |
| [Nix](https://docs.openclaw.ai/install/nix) | 声明式安装 |
| [Ansible](https://docs.openclaw.ai/install/ansible) | 批量部署 |
| [Bun](https://docs.openclaw.ai/install/bun) | 仅 CLI 使用 |

---

## ✅ 安装后验证

```bash
# 检查配置问题
openclaw doctor

# 查看网关状态
openclaw status

# 打开浏览器 UI
openclaw dashboard
```

---

## 🔧 故障排除

### `openclaw` 命令找不到

**诊断:**
```bash
node -v
npm -v
npm prefix -g
echo "$PATH"
```

**解决:**
```bash
# 添加全局 npm 目录到 PATH
export PATH="$(npm prefix -g)/bin:$PATH"

# 添加到 ~/.zshrc 或 ~/.bashrc
echo 'export PATH="$(npm prefix -g)/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

---

## 🔄 更新/卸载

| 操作 | 命令 |
|------|------|
| **更新** | `openclaw update` |
| **卸载** | `openclaw uninstall` |
| **迁移** | 参考 [[OpenClaw 迁移指南]] |

---

## 📚 相关文档

- [[OpenClaw 快速入门]] - 快速开始
- [[OpenClaw 配置指南]] - 配置详解
- [官方安装文档](https://docs.openclaw.ai/install)

---

*最后更新：{{ lastModified }}*
