---
title: "写了 84 个任务、7,308 条轨迹后，我们发现了写好 Skills 的 7 个原则"
date: 2026-02-19 20:47:37
tags:
  - Agent
  - Skills
  - SkillsBench
  - AI
categories:
  - AI
---

## 背景

Anthropic 提出 Agent Skills 概念后，社区迅速积累了成千上万的 Skills。但有个问题没人系统回答过：这些 Skills 真的有用吗？

SkillsBench 团队做了件扎实的事：他们策划了 **84 个任务**，覆盖 **11 个领域**，跑了 **7,308 条轨迹**，测试了 **7 种模型配置**。

结果发现——Skills 的效果差异巨大：

- 好的 Skills 能让成功率提升 **+51.9%**
- 差的 Skills 会让成功率下降 **-39.3%**
- 甚至，让模型自己写 Skills 是负效果

> "Models cannot reliably author the procedural knowledge they benefit from consuming."

这意味着什么？**写好 Skills 是一门手艺**，不是把文档丢给 Agent 就行。

<!-- more -->

## 7 个核心原则

### 原则 1：人工编写 > 模型生成

| Skills 来源 | 平均效果 |
|------------|---------|
| 人工编写的 Skills | +16.2% |
| 模型自生成的 Skills | -1.3% |

**为什么？** 模型可以识别"需要领域知识"，但生成的步骤往往：
- 太笼统（"用 pandas 处理数据"）
- 缺细节（没有具体 API 调用）
- 错重点（列出工具但不讲 workflow）

**启示：** Skills 需要人工提炼领域经验。不能指望 Agent 自己总结怎么做事。

---

### 原则 2：少即是多（数量）

| 每个任务的 Skills 数量 | 提升效果 |
|----------------------|---------|
| 1 个 | +17.8% |
| 2-3 个 | +18.6% ✅ |
| 4+ 个 | +5.9% ❌ |

**反直觉：** 给 Agent 更多 Skills，效果反而更差。

**为什么？** 认知过载。Agent 面临选择困难，或者把不相关的 Skill 硬套到当前任务。

**启示：** 每个任务配套 **2-3 个 Skills** 最优。

---

### 原则 3：详细 > 全面（长度）

| Skills 文档复杂度 | 效果 |
|------------------|-----|
| Detailed（详细） | +18.8% |
| Compact（简洁） | +17.1% |
| Standard（标准） | +10.1% |
| Comprehensive（全面） | -2.9% ❌ |

**又一个反直觉发现：** 过长的文档反而有害。

"Comprehensive" 看上去美好——覆盖所有 edge cases、详尽解释原理。但实际上：
- 占用宝贵的 context budget
- Agent 难以从长篇中定位关键步骤
- 重要信息淹没在噪声里

**启示：** 控制 Skills 长度在 **800-1500 tokens**。聚焦核心步骤，细节可以放在代码示例里。

---

### 原则 4：程序性 > 陈述性

| 类型 | 是 Skills 吗？ | 为什么 |
|-----|--------------|-------|
| 系统 Prompt | ❌ | 缺乏结构化组件 |
| Few-shot 示例 | ❌ | 陈述性，不是程序性 |
| RAG 检索结果 | ❌ | 事实性，不是流程性 |
| 工具文档 | ❌ | 描述能力，不是指导使用 |
| 程序性指导 | ✅ | 描述"怎么做" |

**关键区别：**
- ❌ "Python 是编程语言"（是什么）
- ✅ "用 pandas 读取 CSV 的三步流程"（怎么做）

**启示：** Skills 必须包含可执行的步骤、代码示例、验证检查点。

---

### 原则 5：领域差异巨大

| 领域 | Skills 提升效果 | 原因 |
|-----|---------------|------|
| Healthcare | +51.9% | 专业流程知识模型缺乏 |
| Manufacturing | +41.9% | 特定领域 heuristics |
| Software Engineering | +4.5% | 模型已有足够预训练知识 |
| Mathematics | +6.0% | 逻辑推理而非流程 |

**启示：**
- 领域知识越专、模型预训练越少，Skills 价值越大
- 通用编程任务写 Skills 收益有限
- **医疗、制造、金融等垂直领域**最需要投入 Skills

---

### 原则 6：模块化与组合

SkillsBench 的 Skills 设计强调：
- **Task-class applicability**：适用于一类任务，不是单一实例
- **Portability**：基于文件系统，易于编辑、版本、共享
- **Composability**：多个 Skills 可以组合使用

**启示：** 设计 Skills 时考虑复用性。一个 Skill 解决一类问题，不是一次性的硬编码。

---

### 原则 7：验证与迭代

SkillsBench 的每个任务都有确定性验证器（deterministic verifier）。这不是用 LLM 评判，而是程序化断言。

**启示：**
- 写 Skill 时必须配套验证方法
- 在测试任务上跑通再投入使用
- 成功率低于 70% 的 Skill 需要重写

---

## 避坑清单

| 常见错误 | ❌ | ✅ |
|---------|-----|-----|
| 百科全书式文档 | "这个框架的所有 API 参考..." | "完成 X 任务最常用的 3 个 API" |
| 硬编码任务细节 | "处理 user_data_2024.csv" | "处理 CSV 格式的用户数据" |
| 纯文本无代码 | 长篇描述算法原理 | 提供可复制的代码片段 |
| 过度工程 | 覆盖 20 个 edge cases | 聚焦 80% 场景的核心路径 |
| 让模型自己写 | "Agent 请根据经验总结 Skill..." | 人工提炼 + 验证 + 迭代 |

---

## 结论

SkillsBench 的核心启示：**Skills 的有效性不是必然的，而是设计出来的。**

同样一个 Agent，同样的模型：
- 好的 Skills 能让成功率提升 **50%+**
- 差的 Skills 会让成功率下降 **40%+**

差距来自 7 个简单的原则：

1. 人工编写，不要模型生成
2. 2-3 个 Skills 最优
3. 详细但不过长（800-1500 tokens）
4. 程序性指导，不是陈述性描述
5. 垂直领域收益最大
6. 模块化设计便于复用
7. 配套验证方法

> "The gap between effective and ineffective Skills is not model capability—it's empirical engineering at the skill boundary."

**这或许是 Agent 时代的一门新手艺：不是写代码，而是写"怎么做"。**

---

**参考来源：**
- [SkillsBench: Benchmarking How Well Agent Skills Work Across Diverse Tasks](https://arxiv.org/abs/2602.12670)（Xiangyi Li et al.）

<!-- 信息图待生成 -->