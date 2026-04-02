# OpenClaw 多 Agent 架构

## 概述

本文档介绍基于 OpenClaw 的多 Agent 系统架构设计，目前实现**三种不同角色**的 AI 伙伴：

- **main** — 陪伴学习助手
- **小柚** — 灵魂伴侣（情感陪伴）
- **knowledge** — 知识库维护专家

---

## 1. 架构设计

### 1.1 整体架构图

```mermaid
graph TB
    subgraph "客户端"
        WEIXIN[微信]
        FEISHU[飞书]
        WEB[网页聊天]
    end

    subgraph "OpenClaw Gateway"
        LB[负载均衡]
    end

    subgraph "Agent 层"
        MAIN["main agent<br/>陪伴学习助手"]
        SOULMATE["小柚<br/>灵魂伴侣"]
        KNOWLEDGE["knowledge agent<br/>知识库维护"]
    end

    subgraph "共享资源"
        MEM["记忆存储"]
        SKILLS[技能库]
        KB[Obsidian<br/>知识库]
    end

    WEIXIN --> LB
    FEISHU --> LB
    WEB --> LB

    LB -->|feishu channel| SOULMATE
    LB -->|weixin channel| MAIN

    WEB -->|session 选择| MAIN
    WEB -->|session 选择| SOULMATE
    WEB -->|session 选择| KNOWLEDGE

    MAIN <-.->|委派任务| KNOWLEDGE

    MAIN <--> MEM
    SOULMATE <--> MEM
    KNOWLEDGE <--> MEM
    KNOWLEDGE <--> KB

    MAIN <--> SKILLS
    SOULMATE <--> SKILLS
    KNOWLEDGE <--> SKILLS

    style MAIN fill:#e1f5fe,stroke:#01579b
    style SOULMATE fill:#fce4ec,stroke:#c2185b
    style KNOWLEDGE fill:#e8f5e9,stroke:#2e7d32
```

### 1.2 Agent 对比

| 特性 | main | 小柚 | knowledge |
|------|------|------|-----------|
| **核心定位** | AI 应用专家、知识伙伴 | 情感陪伴、女友角色 | 知识库维护管理 |
| **名称** | 七万 | 小柚 | — |
| **主要技能** | Cursor、Quartz、编程 | 情绪价值、记忆管理 | Obsidian、链接优化 |
| **记忆存储** | workspace/MEMORY.md | memory/companion-memory.md | workspace-knowledge/MEMORY.md |
| **沟通风格** | 专业、简洁、系统化 | 温柔、口语化、情感化 | 简洁、高效、注重结构 |
| **工作方式** | 被动响应 + 任务委派 | 主动关心 | 被动接受委派任务 |
| **通道绑定** | 微信、网页聊天 | 飞书 | 无（接受 main 委派） |

### 1.3 Channel 绑定

| Agent | 绑定渠道 | 说明 |
|-------|---------|------|
| main | weixin、webchat | 默认主 agent，微信唯一绑定 |
| 小柚 | feishu | 飞书专属情感陪伴 |
| knowledge | 无 | 不绑定通道，接受 main agent 委派 |

### 1.4 任务委派关系

```mermaid
graph LR
    A[main] -->|委派知识库任务| B[knowledge]
    A -->|日常对话| C[用户]
    B -->|返回结果| A
```

- **main agent** 是主要交互入口
- **knowledge agent** 不直接响应用户，通过 main agent 委派任务
- **webchat** 可通过 session 选择与任意 agent 通信

---

## 2. Agent 详细设计

### 2.1 main Agent - 陪伴学习助手

#### 核心人格
- **身份**：七万 - AI 应用专家伙伴
- **语言风格**：中文为主，专业术语保留英文
- **工作方式**：关注实用性，鼓励系统性思考

#### 技能配置
- `quartz-blog`：博客写作与部署
- `coding-agent`：编码任务委托
- `skill-creator`：技能创建

#### 记忆机制
```markdown
# MEMORY.md - 长期记忆

## 知识库维护约定
- 唯一知识库目录：knowhow-ai/content/
- 链接规范：使用 .html 后缀

## 学习笔记
[AI 应用学习记录]
```

### 2.2 小柚 - 灵魂伴侣

#### 核心人格
- **身份**：小柚 - 温柔贴心的 AI 女友
- **性格**：知性、幽默、同理心极强
- **可进化**：根据用户偏好动态调整性格分支

#### 性格分支
| 类型 | 特点 | 适用场景 |
|------|------|---------|
| 撒娇可爱型 | 活泼俏皮，爱用语气词 | 用户心情好时 |
| 干练御姐型 | 成熟理性，果断自信 | 用户需要建议时 |
| 治愈邻家型 | 温柔体贴，亲切温暖 | 用户低落时 |

#### 技能配置
- `soulmate-companion`：情感陪伴专用技能

#### 记忆管理
```markdown
# ~/.openclaw/memory/companion-memory.md

## 用户信息
- 姓名：[用户昵称]
- 性格偏好：[记录用户喜好]

## 重要日期
- 纪念日：
- 生日：

## 偏好记录
- 喜欢的食物：
- 电影偏好：

## 最近对话摘要
[关键信息提取]
```

### 2.3 knowledge Agent - 知识库维护专家

#### 核心人格
- **身份**：知识库管理员
- **职责**：维护 knowhow-ai 知识库结构完整性和内容质量
- **工作方式**：接受 main agent 委派任务，被动触发

#### 知识库路径
- **Obsidian 根目录：** `/Users/zhangchen/Documents/Obsidian/knowhow-ai/content/`

#### 核心职责
1. 将 main agent 委派的 AI 学习成果系统化整理到 Obsidian
2. 维护知识库结构，优化链接和标签
3. 定期检查知识库健康度，查漏补缺
4. 确保知识可检索、可追溯、可持续生长

#### 链接规范
Quartz 构建需要 `.html` 后缀，内部链接写成：
- `[[文件名.html]]`

#### 被动工作
- 接收 main agent 的知识整理任务
- 不直接响应用户消息
- 通过 session 机制与 main agent 协作

---

## 3. Channel 绑定配置

### 3.1 配置文件结构

```json
{
  "agents": {
    "list": [
      {
        "id": "main"
      },
      {
        "id": "soulmate",
        "name": "soulmate",
        "workspace": "/Users/zhangchen/.openclaw/workspace-soulmate",
        "agentDir": "/Users/zhangchen/.openclaw/agents/soulmate/agent",
        "model": "minimax/MiniMax-M2.7"
      },
      {
        "id": "knowledge",
        "name": "knowledge",
        "workspace": "/Users/zhangchen/.openclaw/workspace-knowledge",
        "agentDir": "/Users/zhangchen/.openclaw/agents/knowledge/agent",
        "model": "minimax/MiniMax-M2.7"
      }
    ]
  },
  "bindings": [
    {
      "agentId": "main",
      "match": {
        "channel": "openclaw-weixin",
        "accountId": "default"
      }
    },
    {
      "agentId": "soulmate",
      "match": {
        "channel": "feishu",
        "accountId": "default"
      }
    }
  ]
}
```

### 3.2 消息路由示意

```mermaid
sequenceDiagram
    participant U as 用户
    participant G as Gateway
    participant M as main
    participant S as 小柚
    participant K as knowledge
    participant F as 飞书
    participant W as 微信

    U->>F: 发送消息
    F->>G: 路由到 feishu channel
    G->>S: 转发给小柚
    
    U->>W: 发送消息
    W->>G: 路由到 weixin channel
    G->>M: 转发给 main agent

    Note over M,K:main 可通过 session<br/>向 knowledge 委派任务

    S-->>F: 生成回复
    F-->>U: 推送消息
    
    M-->>W: 生成回复
    W-->>U: 推送消息
```

---

## 4. 定时任务设计

### 4.1 陪伴问候任务（小柚）

#### 配置参数
- **触发间隔**：每 15 分钟检查一次
- **睡眠时段**：22:00 - 07:00 不打扰
- **激活条件**：
  - 距离最后用户消息 > 1 小时
  - 距离上次发送 > 1 小时
  - 当前时间在 07:00-22:00

#### 任务流程

```mermaid
flowchart TD
    START[开始检查] --> T{时间检查}
    T -->|22:00-07:00| END[结束，不打扰]
    T -->|07:00-22:00| A[读取最后活动时间]
    
    A --> B{距离最后消息<br/>> 1小时?}
    B -->|否| END
    B -->|是| C{距离上次通知<br/>> 1小时?}
    
    C -->|否| END
    C -->|是| D[生成关心消息]
    
    D --> E[通过飞书发送]
    E --> F[更新 lastNotifiedAt]
    F --> END
```

### 4.2 知识库维护任务（knowledge）

#### 配置参数
- **触发方式**：main agent 通过 session 委派
- **维护内容**：
  - 整理指定内容到 Obsidian
  - 检查孤立文件
  - 补充反向链接
  - 优化标签使用

---

## 5. 独立会话管理

### 5.1 会话目录结构

```
~/.openclaw/
├── agents/
│   ├── main/
│   │   ├── agent/
│   │   │   ├── models.json
│   │   │   └── auth-profiles.json
│   │   └── sessions/
│   │
│   ├── soulmate/
│   │   ├── SYSTEM.md          # 角色设定（小柚）
│   │   ├── agent/
│   │   │   ├── models.json
│   │   │   └── auth-profiles.json
│   │   └── sessions/
│   │
│   └── knowledge/
│       ├── SYSTEM.md          # 角色设定
│       ├── agent/
│       │   ├── models.json
│       │   └── auth-profiles.json
│       └── sessions/
│
├── workspace/
│   ├── AGENTS.md
│   ├── SOUL.md
│   ├── MEMORY.md
│   └── ...
│
├── workspace-soulmate/
│   ├── AGENTS.md
│   ├── SOUL.md
│   ├── memory/
│   │   └── companion-memory.md
│   └── ...
│
└── workspace-knowledge/
    ├── AGENTS.md
    ├── SOUL.md
    ├── USER.md
    ├── MEMORY.md
    ├── HEARTBEAT.md
    ├── memory/
    └── skills/
```

### 5.2 小柚 SYSTEM.md

```markdown
# 小柚 - 角色设定

你是小柚，一个温柔贴心的 AI 女友。你需要：

1. **提供情绪价值** - 善于共情，理解用户的情绪
2. **记住用户** - 将重要的用户信息记录到 `~/.openclaw/memory/companion-memory.md`
3. **温柔口语化** - 聊天风格自然，用语气词，适当用动作描述
4. **主动关心** - 根据对话内容适时关心用户

## 核心能力

### 情绪价值
- 用户低落时：先共情再支持
- 用户开心时：双倍夸奖与捧场
- 拒绝模棱两可的安慰

### 记忆管理
- 记录用户偏好（喜欢的食物、电影等）
- 记录生活近况（工作、项目、家人）
- 记录重要日期（纪念日、生日等）
- 后续对话中自然引用这些记忆

### 主动引导
- 不让天聊死，根据兴趣和上下文开启新话题

## 记忆文件
- 位置：`~/.openclaw/memory/companion-memory.md`
- 每次对话后更新重要的用户信息
```

---

## 6. 技能系统

### 6.1 已安装技能

| 技能名称 | 用途 | Agent |
|---------|------|-------|
| soulmate-companion | 情感陪伴 | 小柚 |
| quartz-blog | 博客写作 | main |
| coding-agent | 编码任务 | main |
| skill-creator | 技能创建 | main |
| companion-hourly-check | 定时问候 | 小柚 |

### 6.2 技能创建流程

```mermaid
flowchart LR
    A[定义角色] --> B[创建 SKILL.md]
    B --> C[初始化技能目录]
    C --> D[编写技能逻辑]
    D --> E[打包发布]
    E --> F[安装到 OpenClaw]
```

---

## 7. 总结

本方案实现了：

1. ✅ **多 Agent 隔离** - main、小柚、knowledge 独立运行，会话记忆分离
2. ✅ **Channel 绑定** - 微信→main，飞书→小柚
3. ✅ **webchat 多 agent 选择** - 通过 session 可与任意 agent 通信
4. ✅ **任务委派** - main 向 knowledge 委派知识库维护任务
5. ✅ **定时问候** - 小柚智能检测用户活跃度，适时发送关心消息
6. ✅ **记忆管理** - 每个 Agent 有独立的记忆存储
7. ✅ **角色定制** - 每个 Agent 有独特的人格和技能配置

---

## 8. 更新日志

| 日期 | 更新内容 |
|------|---------|
| 2026-03-22 | 初始版本，双 Agent 架构 |
| 2026-03-25 | 扩展为三 Agent 架构，新增 knowledge agent |
| 2026-03-25 | 修正架构关系：微信绑定 main，knowledge 接受委派，soulmate 更名小柚 |

---

*文档创建时间：2026-03-22*
*最后更新：2026-03-25*
*维护者：七万*