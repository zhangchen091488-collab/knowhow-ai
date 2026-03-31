---
title: "🎨 Excalidraw 文档绘图工具 - Showcase"
tags: [Excalidraw, 文档, 工具, 配色, 展示]
---

# 🎨 Excalidraw 文档绘图工具

> 为技术文档而生的专业图表生成器

![封面](../Assets/showcase-cover.png)

---

## ✨ 特性一览

| 特性 | 说明 |
|------|------|
| 🎭 **14 款主题** | 来自 Linear、Vercel、Figma 等知名设计系统 |
| 🎨 **实心填充** | 专业实心颜色，拒绝杂乱斜线 |
| 📱 **Excalidraw** | 导出标准格式，可导入 Excalidraw 继续编辑 |
| 🔄 **自动同步** | 配合 Obsidian + Quartz，文档图表自动上网 |

---

## 🎨 主题展示

![主题对比](../Assets/showcase-themes.png)

### 主题列表

| 主题 | 来源 | 风格 |
|------|------|------|
| **Linear** | Linear App | 紫蓝色，专业简洁 |
| **Vercel** | Vercel | 极简黑色系 |
| **Figma** | Figma | 多彩、创意 |
| **Radiant** | Modern | Indigo/Violet 渐变 |
| **Notion** | Notion | 灰白、极简 |
| **GitHub** | GitHub | 开发者风格 |
| **Spotify** | Spotify | 活力绿色系 |
| **Nord** | Nord Theme | 北欧冷色系 |
| **Dracula** | Dracula | 暗紫、粉色 |
| **Midjourney** | Midjourney | 艺术渐变感 |
| **Sunset** | 自定义 | 日落橙红紫 |
| **Ocean** | 自定义 | 海洋蓝绿 |
| **Forest** | 自定义 | 森林绿 |
| **Candy** | 自定义 | 粉彩色 |

---

## 📋 典型使用场景

![使用场景](../Assets/showcase-usecases.png)

### 1. 系统架构图

清晰展示客户端、服务端、数据库的交互关系。

### 2. 工作流程图

展示任务的开始、过程、结束流程。

### 3. 方案对比

直观对比传统方式与 AI 辅助方式的优劣。

### 4. 时间线

展示项目各阶段的进度和里程碑。

---

## 📊 案例展示

使用 excalidraw-generator 技能生成的实际案例：

### 1. 系统架构图

![系统架构图](../Assets/showcase-architecture.png)

展示 Web App、Mobile、API Gateway、PostgreSQL、Redis、S3 的完整架构。

### 2. 流程图

![流程图](../Assets/showcase-workflow.png)

展示条件判断分支的经典流程图。

### 3. 对比图

![对比图](../Assets/showcase-comparison.png)

直观对比传统开发与 AI 辅助开发的差异。

---

## 🚀 快速开始

### 安装

```bash
# 克隆工具
git clone https://github.com/your-repo/excalidraw-generator.git

# 或直接使用
pip install excalidraw-gen
```

### 基本使用

```python
from excalidraw_gen import Scene, ModernColors

# 选择主题
s = Scene(theme="linear")

# 添加元素
s.rect(100, 100, 200, 80, 
       strokeColor=s.theme["primary"], 
       backgroundColor=s.theme["primary_light"])

s.text(120, 130, "Hello Excalidraw!", fontSize=16)

# 保存
s.save("diagram.excalidraw")
```

### 在文档中使用

```markdown
![我的架构图](../Assets/my-diagram.png)
```

> 💡 **提示**：推荐导出 PNG 格式嵌入文档，网页兼容性更好。

---

## 🎯 最佳实践

### 配色原则

1. **主色统一** — 一个文档使用同一个主题
2. **对比明确** — 重要信息用 Accent 强调
3. **层次分明** — 背景用 Light，主要元素用 Primary

### 元素选择

| 场景 | 推荐元素 |
|------|----------|
| 分组/模块 | Frame（框架） |
| 强调/重点 | Ellipse（椭圆） |
| 判断/决策 | Diamond（菱形） |
| 流程/关系 | Arrow（箭头） |
| 标签/说明 | Text（文本） |

---

## 📦 工具位置

- **生成器**：`~/.openclaw/skills/excalidraw-generator/scripts/excalidraw_gen.py`
- **渲染器**：`~/.agents/skills/excalidraw-gen/scripts/render.js`
- **主题展示**：`knowhow-ai/content/Cursor/03-实战/showcase-*.excalidraw`

---

## 🔗 相关资源

- [Excalidraw 官网](https://excalidraw.com)
- [Excalidraw 图表在文档中的应用](./Excalidraw图表在文档中的应用)
- [CLI-Anything-让软件具有Agent原生能力](./CLI-Anything-让软件具有Agent原生能力)

---

![页脚](../Assets/showcase-footer.png)

*让文档更美观，让知识更易懂* ✨
