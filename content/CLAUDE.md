# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is an Obsidian knowledge base documenting Cursor AI coding assistant usage patterns, best practices, and methodologies. The repository is organized as a learning resource rather than a code project.

## Structure

```
Cursor-AI-Guide/
├── 00-知识库导航.md       # Knowledge base overview and navigation
├── 01-基础/               # Foundation concepts
│   └── 01-Cursor入门.md
├── 02-进阶/               # Advanced topics
│   ├── Skills深度探索.md
│   ├── Subagent协作模式.md
│   ├── 如何写好Skill-完整指南.md
│   └── 渐进式揭示-Skill设计的艺术与科学.md
├── 04-思考/               # Reflection and methodology
│   └── 公众号文章模板.md
└── .claude/
    └── settings.local.json
```

## Key Concepts

### Skill System Architecture

The knowledge base documents a comprehensive approach to Cursor Skills design:

**Skill Structure:**
```
skill-name/
├── SKILL.md           # Core definition (required) - contains frontmatter with name and description
├── scripts/           # Executable code (optional)
├── references/        # Reference materials (optional) - loaded on-demand
└── assets/            # Resource files (optional)
```

**Three-Layer Loading System (Progressive Disclosure):**
1. **Metadata (always loaded)**: name + description (~100 tokens)
2. **SKILL.md (triggered)**: Core flows and instructions (<5k tokens)
3. **references/ (on-demand)**: Detailed reference materials

### Skill Design Principles

1. **Conciseness**: Keep SKILL.md under 500 lines; move details to references/
2. **Appropriate Freedom**: Adjust constraints based on task complexity
3. **Progressive Disclosure**: Load information at appropriate depth based on need

### Progressive Disclosure Patterns

| Pattern | Use Case | Description |
|---------|----------|-------------|
| Feature Layering | Multiple functional modules | Each feature links to separate reference file |
| Domain Separation | Complex business domains | Each domain has dedicated reference |
| Difficulty Layering | Learning paths | Quick/Standard/Advanced tiers |
| Conditional Triggering | Dynamic requirements | Load based on specific conditions |
| On-Demand Depth | Variable complexity | User or AI determines depth needed |

### Cursor Feature Comparison

| Feature | Purpose | Scope | Persistence |
|---------|---------|--------|-------------|
| MCP | External tool access | Infrastructure | Persistent |
| Rule | Global behavior constraints | Project-level | Persistent |
| Command | Quick shortcuts | Session-level | Temporary |
| Skill | Reusable AI capabilities | Project/User-level | Persistent |
| Subagent | Multi-agent collaboration | Task-level | Temporary |

### When to Use Each Feature

- **MCP**: Need to connect to external APIs, databases, or tools
- **Rule**: Global code style standards, output format constraints
- **Command**: Simple, frequently-used operations
- **Skill**: Complex tasks requiring rich context and domain expertise
- **Subagent**: Complex system development requiring multi-expert collaboration
- **Direct对话**: One-time, simple tasks

## Writing Notes

- Use Obsidian's `[[WikiLink]]` format for internal cross-references
- Notes are written in Chinese but use English for technical terms and code
- Focus on practical examples and actionable guidance
- Content is structured as progressive learning paths (basic → advanced → practical)

## Context for AI Interactions

When assisting with this knowledge base:
- Understand that this documents Cursor-specific patterns, not general AI concepts
- The content focuses on mobile backend development with C/C++ expertise
- Progressive disclosure is a core design philosophy emphasized throughout
- Content is intended for publication (WeChat articles mentioned)
