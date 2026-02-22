---
title: 🛠️ 工程师的 Claude Code 使用指南：六步工作流
date: 2026-02-22 21:37:32
tags:
  - AI
  - Claude Code
  - 编程
  - 工作流
categories: 工具教程
---

![核心要点](/images/2026-02-22/213732-summary.png)

## 一、原文概括

本文作者 @vikingmute 分享了一位 Cloudflare 工程师使用 Claude Code 的高效工作流程：

**核心理念：**
- 永远不要让 Claude 写代码，直到你审核并批准了书面计划
- 计划与执行的分离是最重要的事
- 这避免了浪费精力，保持对架构决策的控制

**六步工作流：**

```
Research → Plan → Annotation Cycle → Todo List → Implement → Feedback & Iterate
```

### 1. Research（研究）

- 深入阅读指定代码目录/模块
- 要求输出详细的 research.md 报告
- 关键：使用"deeply"、"in great details"、"intricacies"等词汇
- 否则 Claude 会 skim（略读）

### 2. Planning（规划）

- 基于 research 生成详细的 plan.md 文件
- 包含方法解释、代码片段、文件路径、权衡考虑
- 作者不用内置的 plan mode，更喜欢自己的 markdown 文件

### 3. Annotation Cycle（注释循环）

- 在 plan.md 上直接写评论
- 让 Claude 更新 plan 直到满意
- 重复 1-6 次

### 4. Todo List

- 根据 plan 生成待办事项列表

### 5. Implement（执行）

- 完整执行计划

### 6. Feedback & Iterate（反馈与迭代）

- 用简单指令反馈

---

## 二、关键洞察

### 为什么这个工作流有效？

1. **研究阶段的重要性**
   - 防止"在孤立环境中可行但破坏周围系统"的实现
   - 忽略现有缓存层、不符合 ORM 约定的迁移、重复 API 逻辑
   - research.md 是审查面，可以验证 Claude 是否真正理解系统

2. **计划阶段的控制**
   - 自己写 plan.md 而非用内置模式
   - 可以在编辑器中编辑、添加注释
   - 作为项目中的真实工件持久化

3. **注释循环的创新**
   - 直接在 plan 上评论，让 Claude 更新
   - 迭代直到满意

---

## 三、辩证思考

### 3.1 独立观点

**这个工作流确实值得学习：**

1. **计划与执行分离是对的**
   - 避免直接跳到代码的诱惑
   - 保持对架构决策的控制
   - 减少 token 消耗

2. **研究阶段被低估了**
   - 大多数人直接让 AI 写代码
   - 没有先理解现有代码库
   - 这是 AI 辅助编程最昂贵的失败模式

3. **这个流程适用于其他 Agent**
   - 不只是 Claude Code，很多 Agent 都可用

### 3.2 适用性

- 适合有经验的工程师
- 适合复杂项目
- 适合需要控制架构决策的场景

---

## 四、总结

**一句话结论：**
Claude Code 高效使用的关键是"计划先行"，通过 Research → Plan → Annotation 循环确保代码质量。

**行动建议：**
- 📝 永远先研究，再计划，最后才写代码
- 📄 用 markdown 文件记录 research 和 plan
- 🔄 在 plan 上迭代注释直到满意
- 🧠 保持对架构决策的控制
- 💡 这个流程适用于各种 AI 编程工具

**核心原则：**
- 不要让 AI 直接写代码
- 先理解，再计划，后执行
- 用书面计划作为审查面
