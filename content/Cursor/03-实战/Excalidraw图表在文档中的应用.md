---
title: "🎨 Excalidraw 图表在文档中的应用"
tags: [Excalidraw, 文档, 技巧, 可视化]
---

# 🎨 Excalidraw 图表在文档中的应用

> 让文档不再是单调的文字和代码块

---

## 🎯 为什么需要 Excalidraw 图表？

传统的技术文档通常包含：
- 📝 文字描述
- 💻 代码块
- 🔀 Mermaid 流程图
- 🖼️ 静态图片

但这些形式有时难以传达复杂概念。Excalidraw 图表可以：

- ✏️ 手绘风格，更有人情味
- 🎯 精确控制每个元素的位置和样式
- 📱 导入到 Excalidraw 进一步编辑
- 🔄 每次生成都是确定性的

---

## 📁 文件位置

已生成的示例图表：

| 文件 | 说明 |
|------|------|
| `Cursor/03-实战/mobile-architecture.excalidraw` | 移动端架构图 |
| `Cursor/03-实战/comparison.excalidraw` | 方案对比图 |
| `Cursor/03-实战/ai-workflow.excalidraw` | AI 工作流程图 |

---

## 🛠️ 使用方法

### 在 Python 中生成图表

```python
from excalidraw_gen import Scene, Rectangle, Frame, Text, Arrow

s = Scene()

# 标题
s.rect(50, 20, 600, 40, strokeColor="#1e3a5f", backgroundColor="#1e3a5f")
s.text(200, 32, "📱 我的架构图", fontSize=18, strokeColor="#ffffff")

# 添加组件
s.frame(100, 100, 150, 60, strokeColor="#3b82f6", backgroundColor="#dbeafe")
s.text(120, 120, "🎯 前端", fontSize=14, strokeColor="#1d4ed8")

s.frame(300, 100, 150, 60, strokeColor="#10b981", backgroundColor="#d1fae5")
s.text(320, 120, "⚡ 后端", fontSize=14, strokeColor="#047857")

# 添加连线
s.arrow(250, 130, 300, 130, strokeColor="#94a3b8")

# 保存
s.save("path/to/diagram.excalidraw")
```

---

## 🎨 颜色参考

| 用途 | 颜色 | Hex |
|------|------|-----|
| 标题背景 | 深蓝 | `#1e3a5f` |
| 蓝色强调 | 蓝色 | `#3b82f6` |
| 成功/绿色 | 绿色 | `#10b981` |
| 警告/橙色 | 橙色 | `#f59e0b` |
| 错误/红色 | 红色 | `#ef4444` |
| 紫色/创新 | 紫色 | `#8b5cf6` |
| 浅色背景 | 浅蓝 | `#dbeafe` |
| 浅绿背景 | 浅绿 | `#d1fae5` |

---

## 📋 可用的图形元素

| 元素 | 方法 | 用途 |
|------|------|------|
| 矩形 | `s.rect(x, y, w, h)` | 容器、卡片 |
| 框架 | `s.frame(x, y, w, h)` | 分组、模块 |
| 椭圆 | `s.ellipse(x, y, w, h)` | 开始/结束、节点 |
| 菱形 | `s.diamond(x, y, w, h)` | 判断、决策 |
| 文本 | `s.text(x, y, content)` | 标签、说明 |
| 箭头 | `s.arrow(x1, y1, x2, y2)` | 流程、关系 |
| 线条 | `s.line(x1, y1, x2, y2)` | 分隔线 |

---

## 🔗 在文档中引用

在 Obsidian 笔记中，可以直接链接：

```markdown
![架构图](../实战/mobile-architecture.excalidraw)
```

或者使用 Quartz 的语法：

```markdown
![[mobile-architecture.excalidraw]]
```

---

## 📝 配合 AI 使用

现在可以在文档创作时让我生成图表：

1. 告诉我你想要什么类型的图
2. 我使用 `excalidraw_gen` 生成代码
3. 保存到 Obsidian 目录
4. 自动同步到网站

支持的图表类型：
- 📊 架构图
- 🔄 流程图
- ⚖️ 对比图
- 📅 时间线
- 🗺️ 思维导图
- 📱 UI 原型

---

## 💡 后续计划

- 📦 封装成独立的 pip 包
- 🎭 添加更多预设模板
- 🔌 支持直接从自然语言生成图表

---

*本文档配合：[[如何写好Skill-完整指南]]*

*相关工具：[[CLI-Anything-让软件具有Agent原生能力]]*
