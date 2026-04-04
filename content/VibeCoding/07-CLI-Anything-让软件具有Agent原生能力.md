---
title: 07-CLI-Anything-让软件具有Agent原生能力
---

# CLI-Anything：让任何软件具有 Agent 原生能力

> 一条命令，让任何软件都可以被 AI 代理控制

## 什么是 CLI-Anything

CLI-Anything 是由 HKUDS 开发的开源工具，它的核心理念是：**今天的软件服务于人类，明天的用户将是 AI 代理**。

当前 AI 代理虽然擅长推理，但在使用真实专业软件时却表现糟糕。现有的解决方案都有明显缺陷：
- UI 自动化脆弱不稳定
- API 有限，功能残缺
- 重新实现的功能往往缺失 90% 的原软件能力

CLI-Anything 的解决方案是：将任何专业软件转化为可被 AI 代理控制的命令行工具，只需一条命令。

## 核心特性

### 为什么选择 CLI？

- **结构化 & 可组合** - 文本命令符合 LLM 格式，可链式调用完成复杂工作流
- **轻量 & 通用** - 最小依赖，跨系统通用
- **自描述** - `--help` 自动提供文档，代理可自行发现
- **已被验证** - Claude Code 每天通过 CLI 运行数千个真实工作流
- **Agent 优先设计** - 结构化 JSON 输出消除解析复杂度
- **确定性 & 可靠** - 一致的结果实现可预测的代理行为

### CLI-Anything 能做什么？

一条命令生成完整的 CLI，包含：
- 🔍 **分析** - 扫描源代码，将 GUI 操作映射到 API
- 📐 **设计** - 设计命令组、状态模型、输出格式
- 🔨 **实现** - 构建 Click CLI，支持 REPL、JSON 输出、撤销/重做
- 📋 **计划测试** - 创建 TEST.md，包含单元和端到端测试计划
- 🧪 **编写测试** - 实现完整测试套件
- 📝 **文档** - 更新 TEST.md 包含测试结果
- 📦 **发布** - 创建 setup.py，安装到 PATH

## 支持的平台

CLI-Anything 已支持多种主流 AI 编码代理：

| 平台 | 状态 | 安装方式 |
|------|------|----------|
| Claude Code | ✅ 已支持 | 插件市场 |
| OpenClaw | ✅ 已支持 | Skill 安装 |
| OpenCode | ✅ 已支持 | 命令复制 |
| Codex | ✅ 已支持 | Skill 脚本 |
| Qodercli | ✅ 已支持 | 插件注册 |
| Cursor | 🔜 即将支持 | - |
| Windsurf | 🔜 即将支持 | - |

## 安装指南

### 在 Claude Code 中使用

```bash
# 添加插件市场
/plugin marketplace add HKUDS/CLI-Anything

# 安装插件
/plugin install cli-anything

# 生成 CLI
/cli-anything:cli-anything ./gimp
```

### 在 OpenClaw 中使用

```bash
# 克隆仓库
git clone https://github.com/HKUDS/CLI-Anything.git

# 安装 skill
mkdir -p ~/.openclaw/skills/cli-anything
cp CLI-Anything/openclaw-skill/SKILL.md ~/.openclaw/skills/cli-anything/SKILL.md

# 使用
@cli-anything build a CLI for ./gimp
```

### 手动安装

```bash
git clone https://github.com/HKUDS/CLI-Anything.git
cp -r CLI-Anything/cli-anything-plugin ~/.claude/plugins/cli-anything
/reload-plugins
```

## 使用示例

### 生成 CLI

```bash
# 为本地软件生成
/cli-anything:cli-anything ./gimp

# 从 GitHub 仓库生成
/cli-anything:cli-anything https://github.com/blender/blender
```

### 迭代优化

```bash
# 广泛优化 - 代理分析所有功能的差距
/cli-anything:refine ./gimp

# 针对性优化 - 聚焦特定功能领域
/cli-anything:refine ./gimp "batch processing and filters"
```

### 使用生成的 CLI

```bash
# 安装到 PATH
cd gimp/agent-harness && pip install -e .

# 查看帮助
cli-anything-gimp --help

# 结构化输出（便于 AI 解析）
cli-anything-gimp --json layer add -n "Background" --type solid --color "#1a1a2e"

# 交互式 REPL
cli-anything-gimp
```

## 应用场景

CLI-Anything 支持多种软件类型：

| 类别 | 示例 |
|------|------|
| 🖼️ 创意媒体 | Blender, GIMP, OBS Studio, Audacity, Krita, Kdenlive |
| 🤖 AI/ML 平台 | Stable Diffusion WebUI, ComfyUI, InvokeAI, Fooocus |
| 📊 数据分析 | JupyterLab, Apache Superset, DBeaver, KNIME |
| 🔬 科学计算 | ImageJ, FreeCAD, QGIS, ParaView |
| 💻 开发工具 | Jenkins, Gitea, Hoppscotch, Portainer |
| 📂 GitHub 项目 | VSCodium, WordPress, Calibre, Zotero, Joplin |
| 🏢 企业办公 | LibreOffice, NextCloud, GitLab, Grafana |
| 📞 视频会议 | Zoom, Jitsi Meet, BigBlueButton |

## 进阶特性

### SKILL.md 自动生成

从 2026-03-16 开始，生成的 CLI 现在会自动包含 AI 可发现的 skill 定义。每个 CLI 包都包含 `cli_anything/<software>/skills/SKILL.md`，REPL 启动时会显示该文件的绝对路径，AI 代理可以自动发现并读取。

###  refinement 迭代优化

`refine` 命令执行功能差距分析，对比软件的完整能力与当前 CLI 的覆盖范围，然后为识别的差距实现新命令、测试和文档。可以多次运行以稳步扩展覆盖范围——每次运行都是增量且非破坏性的。

## 相关资源

- GitHub: https://github.com/HKUDS/CLI-Anything
- 中文文档: [README_CN.md](https://github.com/HKUDS/CLI-Anything/blob/main/README_CN.md)

---

*本文档最后更新于 2026-03-17*
