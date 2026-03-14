# 从零开始写好一个Skill：完整指南与实践

> 本文首发于微信公众号，欢迎关注获取更多AI编程实战经验

---

## 前言

在使用Cursor和各类AI工具的过程中，我发现一个有趣的现象：大多数人只停留在"直接问AI"的阶段，很少有人深入思考如何让AI变得更强大、更专业。

Skill是我找到的答案。它让AI从一个"通用助手"变成了可以掌握特定领域知识的"专家"。但如何写好一个Skill，却很少有系统的指导。

今天分享我从零开始构建Skills的完整方法论，包括Skill的设计思想、创建流程和管理实践。

---

## 第一部分：理解Skill的背景与定位

### 1.1 什么是Skill？

**简单定义**：Skill是可复用的AI能力模块，是给AI的"技能包"。

**类比理解**：
- 如果AI是一个新员工
- **对话模式** = 每次手把手教他怎么做
- **Skill模式** = 给他一份详细的工作手册，以后他按手册执行

**核心价值**：
1. **减少重复沟通** - 一次定义，永久复用
2. **提高回答质量** - 包含丰富的上下文和规则
3. **专业化AI** - 让AI成为特定领域的专家
4. **团队协作** - Skills可以分享给团队成员

### 1.2 Skill解决的问题

**痛点1：AI的上下文限制**

```
场景：每次需要让AI审查C++代码
问题：每次都要说明：
  - 关注内存安全
  - 检查异常处理
  - 注意性能优化
  - 遵循项目规范

结果：重复沟通，效率低下
```

**痛点2：AI的专业知识不足**

```
场景：处理公司特定的业务逻辑
问题：通用AI不了解：
  - 公司的数据模型
  - 业务的特殊规则
  - 内部的编码规范

结果：AI的建议不够准确
```

**痛点3：质量一致性难以保证**

```
场景：团队多人使用AI
问题：每个人和AI的对话方式不同
结果：生成的代码风格、质量差异很大
```

Skill通过结构化的知识封装，解决了这三个核心问题。

---

### 1.3 Skill与其他功能的关系

理解Skill之前，需要先搞清楚它与MCP、Rule、Command的关系。这些功能各司其职，共同构建了AI能力的生态系统。

#### MCP（Model Context Protocol）

**定义**：OpenAI提出的标准化协议，用于让模型访问外部工具和数据源。

**核心作用**：
- 连接外部世界（API、数据库、文件系统）
- 定义统一的工具调用接口
- 让模型能够动态获取信息

**MCP vs Skill**：
```
MCP是"高速公路"（基础设施）
Skill是"车辆"（应用）

MCP提供能力：可以访问数据库
Skill包装能力：如何查询数据库、如何处理结果、如何输出格式
```

**示例对比**：
```
MCP提供：访问MySQL数据库的能力
Skill定义：
  - 数据库连接配置
  - 表结构映射
  - 查询最佳实践
  - 结果格式化规范
  - 错误处理流程
```

#### Rule（Cursor规则）

**定义**：全局AI行为约束，定义AI"不能做什么"和"必须怎样做"。

**核心作用**：
- 规范AI的输出格式
- 约束AI的行为边界
- 确保代码风格一致性

**Rule vs Skill**：
```
Rule是"底线规范"（约束层）
Skill是"能力建设"（能力层）

Rule说：不要使用不安全的函数
Skill教：如何正确使用安全的函数
```

**示例对比**：
```
Rule：
  - 禁止使用strcpy，必须用strncpy
  - 每个函数必须有错误处理
  - 变量命名使用snake_case

Skill：
  - 如何实现内存安全的字符串处理
  - C++错误处理的最佳实践
  - 如何设计可维护的变量命名体系
```

#### Command（Cursor命令）

**定义**：快捷指令，用于快速执行常用操作。

**核心作用**：
- 简化重复性工作
- 提高操作效率
- 为复杂操作提供快捷入口

**Command vs Skill**：
```
Command是"快捷键"（交互层）
Skill是"功能模块"（能力层）

Command调用：/optimize （触发优化流程）
Skill定义：优化流程的具体步骤和规则
```

**示例对比**：
```
Command：
  /optimize-performance → 触发性能优化流程

Skill（在Command内部调用）：
  - 分析性能瓶颈
  - 应用优化策略
  - 生成优化报告
```

#### 关系图谱

```
                    AI能力生态系统
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
    基础设施层        约束层           能力层
        │               │               │
        ▼               ▼               ▼
      MCP            Rule            Skill
   (外部访问)      (行为规范)      (专业能力)
        │               │               │
        └───────────────┴───────────────┘
                        │
                        ▼
                     交互层
                        │
                        ▼
                   Command
                 (快捷调用)
```

#### 何时使用哪个功能？

| 场景 | 使用 | 原因 |
|------|------|------|
| 需要连接外部API | **MCP** | 标准化工具调用 |
| 全局代码风格规范 | **Rule** | 统一约束AI行为 |
| 简单的常用操作 | **Command** | 快速执行 |
| 复杂的专业任务 | **Skill** | 需要丰富上下文 |
| 临时性任务 | **直接对话** | 无需复用 |

---

## 第二部分：Skill的基础结构与创建

### 2.1 Skill的目录结构

一个规范的Skill包含以下文件和目录：

```
skill-name/
├── SKILL.md           # 核心：技能定义文件（必需）
├── scripts/           # 可执行脚本（可选）
│   ├── script1.py
│   └── script2.sh
├── references/        # 参考资料（可选）
│   ├── api-docs.md
│   └── schemas.md
└── assets/           # 资源文件（可选）
    ├── templates/
    └── examples/
```

**各部分的作用**：

#### SKILL.md（必需）

Skill的"大脑"，包含：

**1. Frontmatter（元数据）**
```yaml
---
name: cpp-performance-optimizer
description: 优化C++代码性能的专家技能。使用当需要分析C++代码的性能瓶颈并应用优化策略时。涵盖：内存访问优化、CPU缓存友好性、SIMD指令、并行计算等。
---
```

**关键点**：
- `name`：Skill的标识符，用于触发
- `description`：最重要的触发机制
  - 说明Skill做什么
  - 明确何时使用（触发条件）
  - 包含所有"何时使用"信息（不在body中）

**2. Body（指令内容）**
```markdown
## 优化流程

1. 分析代码的性能瓶颈
2. 识别优化机会
3. 应用优化策略
4. 生成优化建议

## 优化策略

### 内存访问优化
...

### CPU缓存优化
...

## 参考资料
- [内存对齐指南](references/memory-alignment.md)
- [SIMD指令集](references/simd.md)
```

**关键原则**：
- 使用祈使语气/不定式
- 只包含AI需要的信息
- 避免冗余解释
- 保持在500行以内

#### scripts/（可选）

可执行代码，用于：
- 需要确定性可靠性的任务
- 重复编写的相同代码

**何时使用**：
```
❌ 不需要：
- AI每次都能写出正确的简单代码

✅ 需要：
- 需要精确执行的复杂脚本
- 同样的代码被反复重写
- 需要外部工具调用
```

**示例**：
```python
# scripts/analyze_performance.py
import subprocess
import json

def analyze_performance(code_file):
    # 使用perf工具分析性能
    result = subprocess.run(['perf', 'stat', code_file], capture_output=True)
    return parse_perf_output(result.stdout)
```

#### references/（可选）

参考资料，用于：
- 需要AI参考但无需执行的文档
- 领域知识、API文档、规范

**何时使用**：
```
❌ 不需要：
- 简单的几句话就能说明的信息

✅ 需要：
- 详细的API文档
- 复杂的业务规则
- 数据库schema
- 编码规范
```

**示例**：
```markdown
# references/memory-alignment.md

## 内存对齐原则

1. 结构体成员按自然边界对齐
2. 避免跨缓存行访问
3. 使用alignas强制对齐

## 对齐要求

| 类型 | 对齐要求 |
|------|---------|
| char | 1字节 |
| short | 2字节 |
| int | 4字节 |
| long long | 8字节 |
```

#### assets/（可选）

资源文件，用于：
- 模板
- 示例代码
- 图片、图标等

**何时使用**：
```
❌ 不需要：
- 需要被AI读取理解的内容

✅ 需要：
- 会直接复制给用户的文件
- AI生成的输出中要包含的资源
```

**示例**：
```
assets/
├── templates/
│   ├── cmake-template.txt
│   └── readme-template.md
└── examples/
    └── optimized-example.cpp
```

---

### 2.2 创建Skill的两种方式

#### 方式一：从零创建（推荐新手）

**Step 1：理解需求**

先想清楚Skill要解决什么问题：
```
问题示例：
"每次需要让AI优化C++代码时，都要反复说明：
  - 关注内存访问模式
  - 考虑CPU缓存友好性
  - 使用SIMD指令
  - 避免分支预测失败
  - 检查锁竞争"

目标：创建一个Skill，让AI自动应用这些优化策略
```

**Step 2：规划资源**

分析需要什么资源：

```
scripts/:
  - 需要吗？暂时不需要，AI可以直接写优化代码

references/:
  - performance-guidelines.md（项目性能规范）
  - simd-instructions.md（SIMD指令集参考）
  - memory-alignment.md（内存对齐指南）

assets/:
  - 需要吗？暂时不需要
```

**Step 3：初始化Skill**

使用skill-creator工具：

```bash
# 基础初始化
python init_skill.py cpp-performance-optimizer \
  --path ~/skills \
  --resources references

# 完整初始化（包含示例）
python init_skill.py cpp-performance-optimizer \
  --path ~/skills \
  --resources scripts,references,assets \
  --examples
```

生成的模板：
```
cpp-performance-optimizer/
├── SKILL.md
└── references/
```

**Step 4：编辑Skill**

**编写SKILL.md**：

```markdown
---
name: cpp-performance-optimizer
description: 优化C++代码性能的专家技能。使用当需要分析C++代码的性能瓶颈并应用优化策略时。涵盖：内存访问优化、CPU缓存友好性、SIMD指令、并行计算等。
---

## 优化流程

分析代码时，按以下步骤进行：

1. **识别热点代码**
   - 查找循环、频繁调用的函数
   - 识别计算密集型操作

2. **分析内存访问**
   - 检查缓存命中率
   - 识别随机访问模式
   - 查找不必要的内存拷贝

3. **应用优化策略**
   - 见下方优化策略章节
   - 按优先级应用（高收益优先）

4. **生成优化报告**
   - 说明优化点
   - 提供修改前后对比
   - 评估性能提升

## 优化策略

### 内存访问优化

**优先级：高**

1. **连续内存访问**
   - 将数据结构设计为连续内存布局
   - 使用数组替代链表（当性能关键时）
   - 使用SoA（Structure of Arrays）替代AoS

2. **避免cache miss**
   - 检查数据是否跨越缓存行
   - 使用`[[likely]]`和`[[unlikely]]`提示分支
   - 循环顺序优化（行优先 vs 列优先）

### SIMD优化

**优先级：中**

1. **识别SIMD机会**
   - 找到可以并行处理的相同操作
   - 寻找向量化模式（数组运算）

2. **使用SIMD指令**
   - 优先使用编译器自动向量化（`-O3 -march=native`）
   - 需要时使用intrinsics（如`<immintrin.h>`）
   - 参见：[SIMD指令集](references/simd-instructions.md)

### 并行化

**优先级：中**

1. **识别并行机会**
   - 独立的数据处理
   - 无依赖的循环迭代

2. **应用并行策略**
   - OpenMP：简单并行（`#pragma omp parallel for`）
   - 线程池：更精细的控制
   - 注意：避免过度并行化导致开销

## 参考资料

详细的规范和指南，参见以下文件：

- [性能规范](references/performance-guidelines.md)
- [SIMD指令集](references/simd-instructions.md)
- [内存对齐](references/memory-alignment.md)
```

**填充references/**：

```markdown
# references/simd-instructions.md

## SIMD指令集快速参考

### SSE/AVX操作

#### 数据类型转换
```cpp
// 将16个char转换为16个int
__m512i a = _mm512_loadu_si512(...);
__m512i b = _mm512_cvtepi8_epi32(a);
```

#### 算术运算
```cpp
// 向量加法
__m512i a = _mm512_loadu_si512(...);
__m512i b = _mm512_loadu_si512(...);
__m512i c = _mm512_add_epi32(a, b);
```

### 常用模式

#### 遍历数组
```cpp
for (int i = 0; i < N; i += 16) {
    __m512i chunk = _mm512_loadu_si512(&data[i]);
    // 处理chunk
    _mm512_storeu_si512(&result[i], chunk);
}
```
```

**Step 5：测试Skill**

```bash
# 测试场景
"帮我优化这段C++代码："

# AI应该：
1. 识别代码中的性能问题
2. 引用references中的规范
3. 应用优化策略
4. 生成优化后的代码和报告
```

**Step 6：迭代优化**

根据测试结果调整：
- AI没识别出某个优化点 → 优化策略中强调
- AI的建议不符合项目规范 → 更新references
- 输出格式不清晰 → 调整报告格式要求

---

#### 方式二：从社区Skill学习（推荐有经验者）

**优点**：
- 站在巨人的肩膀上
- 学习最佳实践
- 节省初始开发时间

**流程**：

**Step 1：搜索相关Skill**

在Skill分享平台搜索：
- ClawHub：https://clawhub.com
- GitHub搜索：`skill cursor`、`cursor-skill`
- 社区论坛、Discord

**搜索关键词**：
```
"cpp performance skill"
"c++ optimization skill"
"code review skill"
"test generation skill"
```

**Step 2：评估Skill质量**

检查清单：
- ✅ description清晰明确吗？
- ✅ 有详细的usage说明吗？
- ✅ 示例丰富吗？
- ✅ 有社区反馈吗？
- ✅ 最近有更新吗？

**Step 3：本地化改造**

根据你的项目调整：

```yaml
原Skill:
  description: "优化C++代码性能"

改造后:
  description: "优化C++代码性能，遵循移动端后端项目规范。
                使用当需要优化C++代码时，自动应用项目性能规范。
                关注：内存限制、CPU效率、功耗优化。"
```

**调整内容**：
1. **修改description** - 加入项目特定触发条件
2. **更新references** - 替换为项目规范
3. **调整优化策略** - 符合项目需求
4. **添加项目特定内容** - 如公司特有的工具链

**Step 4：测试与迭代**

与从零创建相同，进行充分测试。

---

### 2.3 Skill设计核心原则

#### 原则1：简洁至上（Concise is Key）

**上下文窗口是公共资源**

每个Skill占用的上下文都会挤占其他内容的空间：
- 系统提示词
- 对话历史
- 其他Skill的元数据
- 用户的实际请求

**实践建议**：

❌ 不好：
```markdown
## 什么是性能优化？

性能优化是指通过改进代码、算法、数据结构等方式，提高程序运行速度、降低内存占用等。在现代软件开发中，性能优化非常重要，因为...
```

✅ 好：
```markdown
## 优化流程

1. 识别热点代码
2. 分析性能瓶颈
3. 应用优化策略
4. 生成优化报告
```

**默认假设：AI已经很聪明**

不要解释AI已经知道的基础知识：
- ❌ 解释什么是数组、什么是循环
- ✅ 说明如何优化数组访问模式

**用示例代替冗长的解释**：

❌ 不好：
```markdown
你应该尽量避免在循环中进行不必要的内存分配。每次分配内存都会消耗时间，并且可能导致碎片化。你应该...
```

✅ 好：
```markdown
## 性能优化模式

### 循环中避免内存分配

**❌ 不好：**
```cpp
for (int i = 0; i < N; i++) {
    auto buffer = new char[1024];  // 每次分配
    // 使用buffer
    delete[] buffer;
}
```

**✅ 好：**
```cpp
char* buffer = new char[1024];  // 分配一次
for (int i = 0; i < N; i++) {
    // 使用buffer
}
delete[] buffer;
```
```

---

#### 原则2：设置适当的自由度

不同的任务需要不同级别的约束：

**高自由度（文本指令）**
- 适用：多种方法都有效
- 决策依赖上下文
- 启发式方法指导

**示例**：
```markdown
## 代码重构建议

重构代码时，考虑以下方面：
- 可读性
- 可维护性
- 性能影响
- 向后兼容性

根据具体上下文选择合适的重构策略。
```

**中自由度（伪代码或带参数的脚本）**
- 适用：有首选模式
- 一些变体可以接受
- 配置影响行为

**示例**：
```markdown
## 性能分析流程

运行分析工具：
```
perf record -g <binary> <args>
perf report
```

根据报告中的热点函数，应用优化策略。
```

**低自由度（特定脚本，参数很少）**
- 适用：操作易出错
- 一致性很关键
- 必须遵循特定顺序

**示例**：
```python
# scripts/deploy.py
# ⚠️ 此脚本必须按顺序执行，不得修改

def deploy():
    if not preflight_check():
        raise Exception("预检查失败")

    backup_current_version()

    deploy_new_version()

    run_health_checks()

    if not all_checks_passed():
        rollback()
    else:
        cleanup()
```

**类比**：
```
高自由度 = 开阔的田野（很多路径可选）
中自由度 = 有标志的路径（推荐路线）
低自由度 = 独木桥（必须遵循）
```

---

#### 原则3：渐进式揭示（Progressive Disclosure）

**渐进式揭示是一种设计哲学：在合适的时机提供合适的深度信息，而不是一次性倾倒所有细节。**

##### 为什么要使用渐进式揭示？

**问题1：上下文窗口有限，信息过载会降低AI性能**

```
❌ 一次性加载所有信息：
SKILL.md（5000行）+ references/（10个文件，每个1000行）= 15000行
→ 上下文窗口爆满，AI注意力分散，响应质量下降

✅ 渐进式加载：
元数据（100词）→ 触发后加载SKILL.md（500行）→ 需要时加载references/
→ 每个阶段的上下文都很干净，AI专注度高
```

**问题2：无关信息干扰AI判断**

```
场景：用户只想查询销售数据

❌ 所有领域的schema都加载：
Finance Schema + Sales Schema + Product Schema + Marketing Schema
→ AI看到Finance Schema，可能误用，导致错误查询

✅ 只加载Sales Schema：
→ AI专注正确的领域，结果准确
```

**问题3：响应延迟和成本浪费**

```
❌ 每次都加载完整Skill：
用户："帮我优化这个简单的循环"
AI：加载10KB的Skill文档（包含SIMD、并行化等复杂内容）
→ Token浪费，响应慢

✅ 按需加载：
用户："帮我优化这个简单的循环"
AI：加载基础优化策略（2KB）
→ 快速响应，成本低
```

**问题4：Skill难以维护和更新**

```
❌ 所有内容堆在SKILL.md：
- 500行代码，修改一处要全文搜索
- 新增功能可能破坏原有结构
- 多人协作容易冲突

✅ 模块化组织：
- SKILL.md：核心流程（稳定）
- references/：按功能拆分（独立更新）
- 维护成本低，协作效率高
```

##### 如何实现渐进式揭示？

**三层加载系统**：

1. **元数据（name + description）**
   - 始终在上下文中（~100词）
   - 用于判断何时触发Skill

2. **SKILL.md主体**
   - Skill触发时加载（<5k词）
   - 核心流程和指令

3. **捆绑资源（references/）**
   - 按需加载
   - 详细参考资料

**实现模式详解**：

**模式1：高层指南 + 引用（最常用）**

适用场景：Skill有多个功能模块，用户可能只需要其中一个。

```markdown
# PDF处理

## 快速开始

使用pdfplumber提取文本：
```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

## 高级功能

- **表单填充**：参见 [FORMS.md](references/FORMS.md) 完整指南
- **API参考**：参见 [REFERENCE.md](references/REFERENCE.md) 所有方法
- **示例**：参见 [EXAMPLES.md](references/EXAMPLES.md) 常见模式
```

**AI的行为**：
- 用户："帮我填充PDF表单" → AI只加载FORMS.md
- 用户："PDF API有哪些方法？" → AI只加载REFERENCE.md
- 用户："给我一个PDF处理示例" → AI只加载EXAMPLES.md

**模式2：领域特定组织（适用于复杂领域）**

适用场景：Skill覆盖多个独立领域，每个领域有详细的知识库。

```
bigquery-skill/
├── SKILL.md  # 概览和导航（300行）
└── references/
    ├── finance.md  # 收入、账单指标（500行）
    ├── sales.md    # 机会、漏斗（400行）
    ├── product.md  # API使用、功能（600行）
    └── marketing.md # 活动、归因（350行）
```

**SKILL.md的结构**：
```markdown
# BigQuery数据分析

## 支持的领域

此Skill支持以下业务领域的数据查询：

- **财务数据**：收入、成本、利润、账单等 → [finance.md](references/finance.md)
- **销售数据**：机会、转化、管道、客户等 → [sales.md](references/sales.md)
- **产品数据**：API使用、功能采用、用户行为等 → [product.md](references/product.md)
- **市场数据**：活动、转化、归因等 → [marketing.md](references/marketing.md)

## 快速查询

说明你的查询需求，AI会自动加载相应领域的数据模型和schema。
```

**AI的行为**：
- 用户："查询上周的销售机会" → AI加载sales.md
- 用户："分析这个月的收入趋势" → AI加载finance.md
- 用户："哪些活动带来了最多用户？" → AI加载marketing.md

**模式3：条件细节（适用于分层功能）**

适用场景：功能有明显的难度分层，基础功能简单，高级功能复杂。

```markdown
# DOCX处理

## 创建文档

使用docx-js创建新文档。参见 [DOCX-JS.md](references/DOCX-JS.md)。

## 编辑文档

简单编辑直接修改XML。

**追踪修改**：参见 [REDLINING.md](references/REDLINING.md)
**OOXML细节**：参见 [OOXML.md](references/OOXML.md)
```

**AI的行为**：
- 用户："创建一个新文档" → 加载DOCX-JS.md
- 用户："添加追踪修改" → 加载REDLINING.md
- 用户："手动编辑XML结构" → 加载OOXML.md
- 用户："简单修改文字" → 不加载任何reference，直接执行

**模式4：按需深度（适用于可变复杂度）**

适用场景：同一功能有不同的深度级别。

```markdown
# 代码重构

## 快速重构

对代码进行快速优化：
- 移除死代码
- 提取重复代码
- 简化复杂表达式

## 深度重构

如果需要全面重构，会加载详细指南：[REFACTORING-DEEP.md](references/REFACTORING-DEEP.md)

包括：
- 设计模式应用
- 架构重构
- 性能优化
```

**AI的行为**：
- 用户："帮我清理这段代码" → 快速重构，不加载reference
- 用户："这个模块需要全面重构" → 加载REFACTORING-DEEP.md

##### 渐进式揭示的实践技巧

**技巧1：明确"何时加载"**

不要只是链接文件，要说明在什么情况下加载：

❌ 不好：
```markdown
## 更多信息
参见 [ADVANCED.md](references/ADVANCED.md)
```

✅ 好：
```markdown
## 高级优化

当以下情况时，会加载高级优化指南：
- 需要手动控制SIMD指令
- 需要自定义并行策略
- 需要处理内存对齐问题

参见：[ADVANCED.md](references/ADVANCED.md)
```

**技巧2：提供概览信息**

让AI知道reference文件里有什么，避免盲目加载：

```markdown
# API调用

## 核心方法
- `get()`: 获取数据
- `post()`: 提交数据

## 完整API参考

[API.md](references/API.md) 包含：
- 所有方法的详细说明
- 参数和返回值
- 错误码列表
- 速率限制规则

只在需要详细API文档时加载。
```

**技巧3：结构化长reference文件**

超过100行的reference文件，在顶部提供目录：

```markdown
# reference/performance.md

## 目录

1. [内存访问优化](#内存访问优化)
2. [CPU缓存优化](#cpu缓存优化)
3. [SIMD指令](#simd指令)
4. [并行计算](#并行计算)
5. [编译器优化](#编译器优化)

## 内存访问优化

...
```

这样AI preview文件时能快速了解内容结构，决定是否需要完整加载。

**技巧4：避免深层嵌套**

❌ 不好：
```
SKILL.md → references/ADVANCED.md → references/ADVANCED/PART1.md
```

✅ 好：
```
SKILL.md → references/advanced-part1.md
SKILL.md → references/advanced-part2.md
```

所有引用文件应直接从SKILL.md链接，保持扁平结构。

##### 常见误区

**误区1：所有内容都放在SKILL.md中**

```
问题：SKILL.md有2000行，包含了所有细节

后果：
- 每次都加载完整内容
- 上下文拥挤，AI难以聚焦
- 修改困难，容易冲突

解决：将详细内容拆分到references/
```

**误区2：reference文件没有导航**

```
问题：references/api.md有1000行，但没有目录

后果：
- AI不知道文件里有什么
- 可能加载了不需要的部分
- 浪费Token

解决：在文件顶部添加目录或概览
```

**误区3：reference之间互相引用**

```
问题：references/part1.md 引用 references/part2.md

后果：
- 加载一个reference可能触发加载更多
- 难以预测加载量
- 可能产生循环引用

解决：所有引用只从SKILL.md出发
```

---

**📌 深入学习**：渐进式揭示是一个强大的设计模式，我在后续文章中会专门深入讲解其实战应用，包括更多复杂场景的设计技巧和避坑指南。敬请关注！

---

## 第三部分：管理、分享与优化Skill

### 3.1 使用npm管理Skill

虽然Skill不是npm包，但可以利用npm的生态来管理和分发。

#### 初始化npm项目

```bash
cd my-skill
npm init -y
```

生成的package.json：

```json
{
  "name": "cpp-performance-optimizer-skill",
  "version": "1.0.0",
  "description": "C++性能优化专家Skill",
  "main": "SKILL.md",
  "files": [
    "SKILL.md",
    "scripts/",
    "references/",
    "assets/"
  ],
  "keywords": [
    "cursor",
    "skill",
    "cpp",
    "performance",
    "optimization"
  ],
  "author": "Your Name",
  "license": "MIT"
}
```

#### 使用npm发布和安装

**发布到npm**（可选）：

```bash
# 登录npm（首次）
npm login

# 发布
npm publish
```

**他人安装**：

```bash
npm install cpp-performance-optimizer-skill
```

**注意**：
- npm发布是可选的
- 更多用于本地管理，而非实际分发
- 实际分发使用.cursor打包（见下文）

#### 使用npm scripts管理工作流

```json
{
  "scripts": {
    "test": "python test_skill.py",
    "package": "python package_skill.py .",
    "validate": "python validate_skill.py .",
    "format": "prettier --write SKILL.md"
  }
}
```

使用：
```bash
npm run test      # 测试Skill
npm run package   # 打包Skill
npm run validate  # 验证Skill
npm run format    # 格式化文档
```

---

### 3.2 打包与分享Skill

#### 打包为.skill文件

使用cursor的打包工具：

```bash
# 打包当前Skill
python package_skill.py ./my-skill

# 指定输出目录
python package_skill.py ./my-skill ./dist
```

打包后生成：
```
my-skill.skill  # 实际上是一个zip文件
```

**打包过程包含**：
1. 自动验证Skill（检查frontmatter、结构、命名等）
2. 打包所有文件
3. 保持目录结构
4. 生成.skill文件

**手动打包**（如果工具不可用）：

```bash
cd my-skill
zip -r ../my-skill.skill *
```

#### 分享方式

**方式1：直接分享.skill文件**

- 在Slack、Discord、论坛直接上传
- 接收者解压到`~/.cursor/skills/`

**方式2：GitHub仓库**

```bash
# 创建GitHub仓库
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourname/cpp-performance-optimizer-skill.git
git push -u origin main
```

**README.md内容**：

```markdown
# C++ Performance Optimizer Skill

## 安装

1. 下载my-skill.skill
2. 解压到`~/.cursor/skills/`
3. 重启Cursor

## 使用

"帮我优化这段C++代码的性能"

## 功能

- 内存访问优化
- CPU缓存友好性
- SIMD指令
- 并行计算

## 贡献

欢迎提交PR！
```

**方式3：Skill分享平台**

上传到：
- ClawHub（推荐）
- GitHub Gist
- 个人博客

**上传到ClawHub**：
```bash
# 假设clawhub有CLI工具
clawhub publish my-skill.skill
```

---

### 3.3 Skill的最佳实践

#### 实践1：版本管理

使用语义化版本：

```
1.0.0 - 初始版本
1.1.0 - 添加新功能（SIMD优化）
1.2.0 - 添加新功能（并行化）
2.0.0 - 重大更新（重构优化流程）
```

**在package.json中维护**：
```json
{
  "version": "1.2.0"
}
```

**在SKILL.md中记录变更**：
```markdown
---
name: cpp-performance-optimizer
version: "1.2.0"
changelog:
  - "1.2.0: 添加并行化优化策略"
  - "1.1.0: 添加SIMD指令优化"
  - "1.0.0: 初始版本"
---
```

#### 实践2：测试驱动开发

**编写测试用例**：

```python
# test_skill.py
def test_basic_optimization():
    code = """
    for (int i = 0; i < N; i++) {
        sum += data[i];
    }
    """
    result = apply_skill(code)
    assert "SIMD" in result
    assert "cache" in result.lower()
```

**持续测试**：
```bash
# 修改Skill后，运行测试
npm run test
```

#### 实践3：社区反馈循环

1. **发布Skill**
2. **收集反馈**
   - GitHub Issues
   - 社区论坛
   - Discord讨论
3. **迭代优化**
4. **发布新版本**

#### 实践4：文档与示例

**提供清晰的文档**：
- 安装指南
- 使用示例
- 常见问题（FAQ）
- 最佳实践

**丰富的示例**：
```markdown
## 示例

### 示例1：优化循环

输入：
```
...代码...
```

输出：
```
...优化后的代码...
```

### 示例2：优化内存访问

...
```

---

### 3.4 常见问题与解决方案

#### Q1: Skill太长怎么办？

**解决方案**：
- 使用渐进式揭示（references/）
- 将详细内容拆分到reference文件
- SKILL.md保持在500行以内

#### Q2: Skill没有正确触发？

**解决方案**：
1. 检查description是否清晰
2. 包含明确的触发条件
3. description应该在100词以内

#### Q3: 如何减少Token消耗？

**解决方案**：
- 使用简洁的语言
- 避免冗余的解释
- 使用示例代替说明
- 合理设置自由度（低自由度=少解释）

#### Q4: Skill输出质量不稳定？

**解决方案**：
- 添加明确的输出格式要求
- 使用示例引导AI
- 增加验证步骤
- 迭代优化prompt

#### Q5: 如何与其他Skill协作？

**解决方案**：
- 设计清晰的职责边界
- 避免功能重叠
- 使用描述性的name和description
- 可以在一个Skill中调用另一个Skill

---

## 总结

写好一个Skill需要理解其定位、结构、创建流程和管理实践：

**核心要点**：

1. **理解定位**：
   - Skill是可复用的AI能力模块
   - 与MCP、Rule、Command各司其职
   - MCP连接外部，Rule规范行为，Command快捷调用，Skill提供专业能力

2. **掌握结构**：
   - SKILL.md是核心
   - scripts/用于可执行代码
   - references/用于参考资料
   - assets/用于资源文件

3. **创建流程**：
   - 从零创建：理解需求 → 规划资源 → 初始化 → 编辑 → 测试 → 迭代
   - 从社区学习：搜索 → 评估 → 本地化改造 → 测试 → 迭代

4. **设计原则**：
   - 简洁至上
   - 适当的自由度
   - 渐进式揭示

5. **管理分享**：
   - 使用npm管理（可选）
   - 打包为.skill文件
   - 分享到社区平台
   - 版本管理和持续优化

---

## 下一步

现在你已经掌握了写好Skill的完整方法论，接下来可以：

1. **动手实践**：选择一个具体场景，创建你的第一个Skill
2. **深入学习**：研究优秀的开源Skill，学习最佳实践
3. **分享交流**：将你的Skill分享到社区，获取反馈
4. **系列文章**：我将分享更多关于Skills的实战案例

---

**欢迎关注我的公众号，获取更多AI编程实战经验！**

如果你有任何问题或想分享Skill开发心得，欢迎在评论区留言。
