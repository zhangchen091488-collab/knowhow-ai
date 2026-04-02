# zoxide

> 智能目录跳转工具，用 Rust 编写的 `cd` 替代品

---

## :zap: 核心功能

- 记录你经常访问的目录
- 通过简短的名字快速跳转
- 模糊匹配，智能排序
- 支持所有主流 Shell

---

## :gear: 工作原理

> zoxide 会记录每次 `cd` 操作到指定目录的上下文（访问次数、时间、路径深度），建立一个加权评分数据库。当你输入 `z <目录名>` 时，它根据评分算法返回最匹配的目录并自动跳转。

---

## :package: 安装

```bash
# macOS
brew install zoxide

# Linux
curl -Ls https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | bash

# Arch Linux
pacman -S zoxide
```

---

## :wrench: 初始化配置

将以下内容添加到 Shell 配置文件（`.zshrc` / `.bashrc`）：

```bash
# zsh
eval "$(zoxide init zsh)"

# bash
eval "$(zoxide init bash)"
```

> 然后重新加载配置：`source ~/.zshrc`

---

## :keyboard: 使用方式

### 基本跳转

```bash
# 跳转到最常访问的 Documents 目录
z doc

# 完整路径也可以
z /Users/zhangchen/Documents
```

### 交互式选择

```bash
# 显示匹配列表，选择后跳转
zi doc
```

### 仅查询不跳转

```bash
# 查询匹配结果，但不执行跳转
zq doc
```

### 其他命令

| 命令 | 说明 |
|------|------|
| `zoxide query doc` | 显示数据库记录 |
| `zoxide add /path/to/dir` | 添加路径到数据库（不跳转） |
| `zoxide remove /path/to/dir` | 移除路径记录 |

---

## :chart_with_upwards_trend: 评分算法

> zoxide 使用加权评分，综合考虑以下因素：

```mermaid
flowchart LR
    subgraph 评分因素
        F[频率<br/>访问次数] --> SCORE[总分]
        R[近期性<br/>最近访问] --> SCORE
        D[深度<br/>路径深度] --> SCORE
    end

    subgraph 评分规则
        direction TB
        F --> |"次数越多 +分"| F1
        R --> |"越近访问 +分"| R1
        D --> |"越深路径 +分"| D1
    end

    SCORE --> |"z doc"| RESULT{匹配结果}
    RESULT --> |最高分| DOC[/Users/zhangchen/Documents]
    RESULT --> |次高分| DOCS[/Documents]

    style F fill:#e3f2fd,stroke:#1976d2
    style R fill:#e3f2fd,stroke:#1976d2
    style D fill:#e3f2fd,stroke:#1976d2
    style SCORE fill:#fff3e0,stroke:#f57c00
    style DOC fill:#e8f5e9,stroke:#388e3c
```

| 因素 | 说明 |
|------|------|
| 频率 | 访问次数越多，权重越高 |
| 近期性 | 最近访问的目录优先 |
| 深度 | 路径越深权重越高（减少与根目录冲突） |

> 这使得 `z doc` 会跳转到 `/Users/zhangchen/Documents` 而不是 `/Documents`。

---

## :bulb: 适用场景

| 场景 | 说明 |
|------|------|
| 频繁切换深层目录 | 不用再打完整路径 |
| 项目目录跳转 | 在多个项目间切换 |
| 减少重复 cd | 在长路径目录间移动 |
| 替代 autojump | 更快的 Rust 实现，更好的跨平台支持 |

---

## :scales: 与 autojump 对比

| 特性 | zoxide | autojump |
|------|:------:|:--------:|
| 语言 | Rust | Python |
| 速度 | :rocket: 快 | :snail: 较慢 |
| 跨平台 | :white_check_mark: 完整支持 | :warning: 部分支持 |
| 算法 | 综合评分 | 频率统计 |

---

## :star: 技巧

### 配合 fzf 使用

```bash
# 在 .zshrc 中配置
eval "$(zoxide init zsh --cmd j)"
bindkey '^j' autojump
```

### 别名优化

```bash
# 嫌 z 太短？用更多 alias
alias cd='z'
alias cdi='zi'
```

---

## :link: 相关工具

- [[01-tunnelto]] — 本地服务公网暴露
