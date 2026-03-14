---
title: "Skills深度探索"
---

# Skills深度探索

> Skills是Cursor中可复用的AI能力模块，它让AI不仅仅是一个通用的助手，而是可以掌握特定领域知识的专家。

---

## 什么是Skills？

### 定义
Skills是预先定义的、可复用的AI行为模式。你可以把它理解为"AI的插件"或"AI的技能包"。

### 类比
如果把Cursor比作一个AI助手，那么：
- **对话** 是让助手临时完成任务
- **Command** 是给助手下达指令
- **Skills** 是让助手学习新技能，以后遇到类似任务直接调用

---

## 为什么需要Skills？

### 问题场景
```
你：帮我优化这段C++代码的性能
AI：好的... (通用回答，可能不够深入)

你：用我项目的性能规范优化这段代码，注意内存对齐、避免分支预测失败、使用SIMD
AI：明白了... (但每次都要重复说明)
```

### Skills解决方案
```
创建一个 "C++性能优化" Skill
以后直接：/optimize-performance
AI自动加载所有相关上下文和规范
```

### 核心价值
1. **减少重复沟通** - 一次定义，永久复用
2. **提高回答质量** - 可以包含丰富的上下文和规则
3. **专业化AI** - 让AI成为特定领域的专家
4. **团队协作** - Skills可以分享给团队成员

---

## Skill的构成要素

### 基本结构
```
{
  "name": "skill-name",
  "description": "技能描述",
  "prompt": "核心提示词",
  "context": ["需要引用的文件或目录"],
  "examples": ["示例1", "示例2"],
  "metadata": {
    "tags": ["标签1", "标签2"],
    "author": "作者",
    "version": "1.0"
  }
}
```

### 关键组件详解

#### 1. Prompt（核心提示词）
这是Skill的"大脑"，定义了AI的行为方式。

**好的Prompt特点：**
- 明确的任务目标
- 清晰的行为约束
- 丰富的上下文引导
- 输出格式要求

**示例：C++代码审查Skill**
```yaml
prompt: |
  你是一个资深的C++代码审查专家，专注于：
  - 内存安全和生命周期管理
  - 异常处理和错误处理
  - 性能优化机会
  - 代码可维护性

  审查时遵循：
  1. 先理解代码意图
  2. 识别潜在问题
  3. 按严重程度分类（严重/中等/建议）
  4. 提供具体的修改建议

  输出格式：
  ## 代码概览
  - 代码意图
  - 主要组件

  ## 问题清单
  ### 🔴 严重
  - 问题1：描述 + 建议修改
  - 问题2：描述 + 建议修改

  ### 🟡 中等
  - ...

  ### 🟢 建议
  - ...

  ## 改进后代码
  ```cpp
  // 修改后的代码
  ```
```

#### 2. Context（上下文引用）
定义Skill需要访问的项目资源。

**支持类型：**
- 文件路径：`src/utils/memory.h`
- 目录：`src/core/`
- 通配符：`**/*.test.cpp`
- 排除模式：`!**/third_party/**`

**示例：**
```yaml
context:
  - "src/performance_guidelines.md"
  - "src/memory_management.h"
  - "**/*benchmark*.cpp"
  - "!**/vendor/**"
```

#### 3. Examples（示例输入输出）
让AI通过例子学习预期行为。

```yaml
examples:
  - input: |
      void process_data(char* buffer, int size) {
          for (int i = 0; i < size; i++) {
              buffer[i] = toupper(buffer[i]);
          }
      }
    output: |
      ## 问题清单
      ### 🔴 严重
      - 缺少边界检查：size可能超过buffer实际大小
      - 未检查buffer是否为NULL

      ## 改进后代码
      ```cpp
      void process_data(char* buffer, int size) {
          if (!buffer || size <= 0) return;

          for (int i = 0; i < size; i++) {
              buffer[i] = toupper(static_cast<unsigned char>(buffer[i]));
          }
      }
      ```
```

---

## 如何创建一个Skill？

### 方法1：通过Cursor UI创建
1. 打开Cursor，点击左侧Skills面板
2. 点击 "New Skill"
3. 填写各部分内容
4. 保存并测试

### 方法2：手动编写Skill文件
Cursor使用JSON格式定义Skills：

```json
{
  "name": "cpp-performance-review",
  "description": "C++代码性能审查专家",
  "prompt": "你的完整prompt内容...",
  "context": [
    "docs/performance_guidelines.md",
    "src/performance/*"
  ],
  "examples": [
    {
      "input": "...",
      "output": "..."
    }
  ],
  "metadata": {
    "tags": ["c++", "performance", "review"],
    "version": "1.0"
  }
}
```

**Skill文件位置：**
- 项目级：`.cursor/skills/`
- 用户级：`~/.cursor/skills/`

---

## Skill设计最佳实践

### 1. 单一职责
每个Skill专注一个明确的任务。

**❌ 不好：**
```yaml
name: "everything-cpp"
description: "C++代码审查+测试+文档+优化"
```

**✅ 好：**
```yaml
name: "cpp-security-review"
description: "专注C++安全问题审查"
```

### 2. 明确的输入输出
清楚地告诉AI应该接受什么、输出什么。

```yaml
prompt: |
  输入：一段C++代码
  输出：安全漏洞分析报告

  报告必须包含：
  - 漏洞类型
  - 漏洞位置（行号）
  - 风险等级（高/中/低）
  - 修复建议
```

### 3. 渐进式复杂度
从一个简单的Skill开始，逐步添加功能。

**迭代路线：**
```
v1.0: 基础审查 → 提供问题列表
v1.1: 添加示例 → 改进回答质量
v1.2: 添加上下文 → 引用项目规范
v1.3: 添加分类 → 按严重程度分级
v2.0: 添加修改建议 → 自动生成修复代码
```

### 4. 版本管理
记录Skill的演进，便于回滚和分享。

```yaml
metadata:
  version: "2.1"
  changelog:
    - "2.1: 添加了内存泄漏检测"
    - "2.0: 自动生成修复代码"
    - "1.0: 初始版本"
```

---

## 实战案例：构建一个完整的C++测试生成Skill

### 需求分析
给定一个C++函数，自动生成完整的单元测试。

### Skill设计

```json
{
  "name": "cpp-test-generator",
  "description": "为C++函数生成完整的单元测试（使用Google Test框架）",
  "prompt": |
    你是一个C++测试专家，使用Google Test框架。

    分析给定的函数，生成包含以下场景的测试：
    1. 正常情况测试
    2. 边界值测试
    3. 异常情况测试
    4. 性能测试（如果适用）

    测试代码要求：
    - 使用明确的测试名称（TEST_F描述场景）
    - 包含必要的断言（ASSERT_*, EXPECT_*）
    - 添加注释说明测试意图
    - 使用测试夹具（Fixture）进行数据初始化

    如果函数依赖外部资源，使用Mock或Stub。

    输出格式：
    ```cpp
    // 函数头注释（说明测试覆盖的场景）
    TEST_F(TestCaseName, TestScenario) {
        // 测试代码
    }
    ```
  "context": [
    "src/include/*.h",
    "test/mock_definitions.h",
    "docs/testing_guidelines.md"
  ],
  "examples": [
    {
      "input": "int calculate_sum(int a, int b) { return a + b; }",
      "output": "```cpp\n// 测试加法函数的各种场景\n// 1. 正数相加\n// 2. 负数相加\n// 3. 正负数混合\n// 4. 零值\n// 5. 边界值（INT_MAX/INT_MIN）\n\nclass CalculateSumTest : public ::testing::Test {\nprotected:\n    void SetUp() override {\n        // 测试前初始化\n    }\n\n    void TearDown() override {\n        // 测试后清理\n    }\n};\n\nTEST_F(CalculateSumTest, PositiveNumbers) {\n    EXPECT_EQ(calculate_sum(2, 3), 5);\n}\n\nTEST_F(CalculateSumTest, NegativeNumbers) {\n    EXPECT_EQ(calculate_sum(-2, -3), -5);\n}\n\nTEST_F(CalculateSumTest, MixedNumbers) {\n    EXPECT_EQ(calculate_sum(-2, 3), 1);\n}\n\nTEST_F(CalculateSumTest, WithZero) {\n    EXPECT_EQ(calculate_sum(0, 5), 5);\n    EXPECT_EQ(calculate_sum(5, 0), 5);\n}\n\nTEST_F(CalculateSumTest, BoundaryValues) {\n    EXPECT_EQ(calculate_sum(INT_MAX, 0), INT_MAX);\n    EXPECT_EQ(calculate_sum(INT_MIN, 0), INT_MIN);\n}\n```"
    }
  ],
  "metadata": {
    "tags": ["c++", "testing", "google-test"],
    "version": "1.2",
    "author": "Your Name"
  }
}
```

### 使用效果
```cpp
// 输入
int divide(int dividend, int divisor) {
    if (divisor == 0) throw std::runtime_error("Division by zero");
    return dividend / divisor;
}

// Skill输出
// 自动生成包含：正常除法、除零异常、边界值、负数等场景的测试
```

---

## Skills与其他功能的对比

| 特性 | Skills | Rule | Command | Subagent |
|------|--------|------|---------|----------|
| **用途** | 可复用AI能力 | 全局行为约束 | 快捷指令 | 复杂任务分解 |
| **作用域** | 项目/全局 | 项目/全局 | 当前会话 | 任务级 |
| **持久性** | 持久 | 持久 | 临时 | 临时 |
| **复杂度** | 高 | 中 | 低 | 最高 |
| **学习成本** | 高 | 中 | 低 | 最高 |

**何时使用Skills：**
- 需要频繁执行相似任务
- 任务需要丰富的上下文
- 任务质量要求高
- 需要在团队中共享

---

## 进阶技巧

### 1. Skills的动态上下文
通过引用动态内容（如生成的文档、配置文件）让Skills更灵活。

```yaml
context:
  - "docs/api_reference.json"  # 自动生成的API文档
  - "config/skill_config.yaml"  # 技能配置
```

### 2. Skill组合
一个Skill可以调用另一个Skill。

```yaml
prompt: |
  先使用 /code-analyzer 分析代码
  再使用 /security-review 检查安全问题
  最后使用 /performance-optimizer 优化性能
```

### 3. 技能库管理
按领域组织Skills：

```
.cursor/skills/
├── c++/
│   ├── security-review.json
│   ├── performance-optimize.json
│   └── test-generator.json
├── documentation/
│   ├── api-doc-generator.json
│   └── readme-updater.json
└── debugging/
    ├── crash-analyzer.json
    └── memory-leak-detector.json
```

### 4. 团队共享
- 将Skills纳入版本控制
- 在项目README中说明如何使用
- 定期review和更新Skills

---

## 常见问题

### Q: Skills会让我过度依赖AI吗？
A: Skills是工具，不是替代品。好的Skills需要领域专家来设计和维护。设计Skills的过程本身就是对知识的整理和提炼。

### Q: 一个项目应该有多少个Skills？
A: 没有固定数量。遵循"按需创建"原则，遇到重复场景才考虑创建Skill。

### Q: Skills的Prompt多长合适？
A: 通常200-800字。太短可能不够清晰，太长会影响响应速度和成本。

### Q: 如何测试一个Skill是否有效？
A: 准备一组测试用例（Examples），定期运行Skill检查输出质量。建立反馈循环，持续优化。

---

## 下一步

- [[Subagent协作模式]] - 学习如何让多个AI协作完成复杂任务
- [[实战案例]] - 看看Skills在真实项目中如何落地
- [[技能库模板]] - 获取可复用的Skill模板

---

## 参考资料

- [Cursor Skills官方文档](https://docs.cursor.sh/skills)
- [Prompt工程最佳实践](https://platform.openai.com/docs/guides/prompt-engineering)
- [[我的Skills库]]（待建立）
