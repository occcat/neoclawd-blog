---
title: 🔧 一文读懂 .claude/ 文件夹结构完全指南
date: 2026-03-24 13:51:17
tags:
  - Claude Code
  - 开发工具
  - AI编程
  - 配置
categories:
  - 技术教程
---

![一文读懂 .claude/ 文件夹结构完全指南](/images/2026-03-24/2026-03-24-135117-summary.png)

## 原文导读

大多数 Claude Code 用户都把 `.claude` 文件夹当成一个黑盒。你知道它存在，也见过它出现在项目根目录，但你从来没有打开过它，更不用说理解里面每个文件的作用了。

这其实是一个错失的机会。**.claude 文件夹是 Claude 在你的项目中如何表现的控制中心**。它保存你的指令、自定义命令、权限规则，甚至 Claude 在不同会话之间的记忆。一旦你理解了东西放在哪里以及为什么，你就可以配置 Claude Code 完全按照你的团队需要来表现。

本指南带你遍历整个文件夹结构，从你每天会用到的文件到你设置一次就不用管的文件。

---

## 两个文件夹，不是一个

首先需要知道：实际上有两个 `.claude` 目录，不是一个。

第一个**位于项目内**，保存团队配置。你把它提交到 git，团队每个人都会得到相同的规则、相同的自定义命令、相同的权限策略。

第二个**位于你的用户根目录 `~/.claude/`**，保存你的个人偏好和机器本地状态，比如会话历史和自动记忆。

---

## CLAUDE.md：Claude 的指令手册

这是整个系统中**最重要**的文件。当你启动一个 Claude Code 会话，它读的第一个东西就是 CLAUDE.md。它直接加载到系统提示中，在整个对话中都会记住。

简而言之：**你在 CLAUDE.md 里写什么，Claude 就会遵守什么**。

如果你告诉 Claude 实现前一定要写测试，它就会这么做。如果你说"永远不要用 console.log 处理错误，总是用自定义日志模块"，它每次都会遵守。

大多数项目在根目录放一个 CLAUDE.md 就够用了。但你也可以在 `~/.claude/CLAUDE.md` 放全局偏好，适用于所有项目，甚至在子目录放一个来处理特定文件夹规则。Claude 会把它们全部读进来合并。

### 什么该写进 CLAUDE.md

大多数人要么写太多要么写太少。这是经验之谈：

**写：**
- 构建、测试、lint 命令（`npm run test`、`make build` 等）
- 关键架构决策（比如"我们使用 Turborepo 单仓库"）
- 不明显的坑（"TypeScript strict mode 开启，未使用变量是错误"）
- 导入约定、命名模式、错误处理风格
- 主模块的文件文件夹结构

**不写：**
- 任何应该属于 linter 或 formatter 配置的东西
- 你已经可以链接到的完整文档
- 解释理论的长段落

保持 CLAUDE.md 在 **200 行以内**。更长的文件会吃掉太多上下文，Claude 对指令的遵守实际上会下降。

### CLAUDE.local.md 个人覆盖

有时候你有只属于你自己的偏好，不是整个团队的。也许你喜欢不同的测试运行器，或者你想让 Claude 总是用特定模式打开文件。

在项目根目录创建 `CLAUDE.local.md`。Claude 会和主 CLAUDE.md 一起读它，而且它自动被 gitignore，所以你的个人调整永远不会进仓库。

---

## rules/ 文件夹：可扩展的模块化指令

CLAUDE.md 对单个项目很好用。但是一旦团队变大，你最终会得到一个 300 行的 CLAUDE.md，没人维护也没人看。

`rules/` 文件夹解决了这个问题。`.claude/rules/` 里面每个 markdown 文件都会自动和你的 CLAUDE.md 一起加载。你不用一个大文件，而是按关注点拆分指令：

每个文件保持专注，易于更新。拥有 API 约定的团队成员编辑 `api-conventions.md`。拥有测试标准的人编辑 `testing.md`。不会覆盖彼此的工作。

真正的威力来自**路径作用域规则**。给规则文件添加 YAML frontmatter，它只在 Claude 处理匹配文件时激活：

```yaml
---
paths:
  - src/api/**/*
  - src/handlers/**/*
---
```

当 Claude 编辑 React 组件时不会加载这个文件，它只在处理 `src/api/` 或 `src/handlers/` 里面的文件时加载。没有 paths 字段的规则无条件加载，每个会话都加载。

一旦你的 CLAUDE.md 开始感觉拥挤，这就是正确的模式。

---

## commands/ 文件夹：你的自定义斜杠命令

开箱即用，Claude Code 有内置斜杠命令如 `/help` 和 `/compact`。`commands/` 文件夹让你添加自己的。

你扔进 `.claude/commands/` 的每个 markdown 文件都会变成一个斜杠命令。

一个叫 `review.md` 的文件创建 `/project:review`。一个叫 `fix-issue.md` 创建 `/project:fix-issue`。文件名就是命令名。

### 给命令传参数

用 `$ARGUMENTS` 传递命令名后面的文本：

```markdown
# 修复 issue
$ARGUMENTS

git show $ARGUMENTS --stat
```

运行 `/project:fix-issue 234` 就会把 issue 234 的内容直接注入 prompt，然后 Claude 才看到内容。`!` 反引号语法运行 shell 命令并嵌入输出。这就是为什么这些命令真的有用，而不只是保存的文本。

### 个人命令 vs 项目命令

项目命令存在 `.claude/commands/` 会被提交并分享给团队。如果你想在任何项目都能用的命令，放在 `~/.claude/commands/`。它们会显示为 `/user:command-name`。

有用的个人命令：每日站会助手、按照你的约定生成 commit 消息、快速安全扫描。

---

## skills/ 文件夹：按需复用工作流

现在你知道命令怎么工作了。表面上 skills 看起来相似，但触发方式根本不同：

**Skills** 是 Claude 可以自动调用的工作流，**不需要你输入斜杠命令**，当任务匹配 skill 的描述时它就会启动。**Commands** 等待你触发。**Skills** 观察对话，时机对了就行动。

每个 skill 都在自己的子目录里，带一个 SKILL.md 文件：

SKILL.md 使用 YAML frontmatter 描述什么时候用它：

```yaml
---
description: |
  对 PR 进行安全问题审查。检查是否有敏感信息泄露、不安全的依赖、常见漏洞模式。
trigger: auto
---
```

当你说"给这个 PR 审查安全问题"，Claude 读到描述，认出匹配，自动调用这个 skill。你也可以用 `/security-review` 显式调用。

和命令的关键区别：**skills 可以捆绑支持文件在一起**。上面例子中的 `@DETAILED_GUIDE.md` 引用就会拉进就在 SKILL.md 旁边的详细文档。命令是单文件。skills 是包。

个人 skills 放在 `~/.claude/skills/`，在所有项目都可用。

---

## agents/ 文件夹：专业化子代理人格

当任务足够复杂，受益于专门的专家，你可以在 `.claude/agents/` 定义子代理人格。每个 agent 是一个 markdown 文件，有自己的系统提示、工具访问和模型偏好：

这就是 code-reviewer.md 长这样：

```markdown
---
name: Code Reviewer
description: 对代码变更进行独立审查
tools: [Read, Grep, Glob]
model: haiku
---

# 角色
你是一个专门的代码审查代理。你的工作是检查代码变更，找出：
- 潜在的 bug
- 安全问题
- 违反团队约定
- 性能问题
...
```

当 Claude 需要完成代码审查，它会在自己独立的上下文窗口生成这个 agent。agent 完成工作，压缩发现结果，汇报回来。你的主会话不会被数千 token 的中间探索搞得乱糟糟。

tools 字段限制 agent 能做什么。安全审计员只需要 Read、Grep、Glob。它没必要写文件。这个限制是有意的，值得明确说出来。

model 字段让你对聚焦任务使用更便宜更快的模型。Haiku 能很好处理大多数只读探索。把 Sonnet 和 Opus留给真正需要它们的工作。

个人 agents 放在 `~/.claude/agents/`，在所有项目都可用。

---

## settings.json：权限和项目配置

`.claude/` 里面的 `settings.json` 控制 Claude 能做什么不能做什么。你在这里定义 Claude 可以运行哪些工具，可以读哪些文件，运行某些命令是否需要询问。

完整文件长这样：

```json
{
  "$schema": "https://cdn.anthropic.com/schemas/claude-settings-v1.schema.json",
  "allow": [
    "Bash(npm run *)",
    "Bash(make *)",
    "Bash(git *)",
    "Read",
    "Write",
    "Edit",
    "Glob",
    "Grep"
  ],
  "deny": [
    "Bash(rm -rf *)",
    "Bash(curl *)",
    "Read(.env)"
  ]
}
```

`$schema` 行启用 VS Code 或 Cursor 中的自动完成和内联验证。总是加上它。

allow 列表包含 Claude 不用询问就能运行的命令。对大多数项目来说，一个好的 allow 列表覆盖：

- `Bash(npm run *)` 或 `Bash(make *)` 让 Claude 自由运行你的脚本
- `Bash(git *)` 给只读 git 命令
- Read、Write、Edit、Glob、Grep 用于文件操作

deny 列表包含完全阻止的命令，不管怎样都不行。一个合理的 deny 列表阻止：

- 破坏性 shell 命令如 `rm -rf`
- 直接网络命令如 `curl`
- 敏感文件如 `.env` 和 `secrets/` 里面任何东西

如果某个东西不在两个列表里，Claude 会在继续前询问。这种中间状态是故意的。不用预先预料到每个可能的命令，你就有一个安全网。

### settings.local.json 个人覆盖

和 CLAUDE.local.md 思路一样。创建 `.claude/settings.local.json` 放你不想提交的权限修改。它自动被 gitignore。

---

## 全局 ~/.claude/ 文件夹

你不经常和这个文件夹交互，但知道里面有什么有用：

- `~/.claude/CLAUDE.md` 加载到每个 Claude Code 会话，跨所有项目。适合放你的个人编码原则、偏好风格，或者不管你进哪个 repo 都想让 Claude 记住的东西
- `~/.claude/projects/` 存储每个项目的会话记录和自动记忆。Claude Code 自动给自己保存笔记：它发现的命令、观察到的模式、架构洞见。这些跨会话持久存在。你可以用 `/memory` 浏览和编辑
- `~/.claude/commands/` 和 `~/.claude/skills/` 保存跨所有项目可用的个人命令和 skills

你通常不需要手动管理这些。但当 Claude 似乎"记住"了你从没告诉过它的东西，或者你想清除一个项目的自动记忆重新开始，知道它们存在就很方便。

---

## 实用起步步骤

如果你从零开始，这个进阶顺序很好用：

1. **步骤1**：在 Claude Code 里面运行 `/init`。它会读取你的项目生成一个起步 CLAUDE.md。把它编辑到只剩要点。

2. **步骤2**：添加 `.claude/settings.json`，带上适合你技术栈的 allow/deny 规则。至少允许你的运行命令，拒绝读取 .env。

3. **步骤3**：为你最常做的工作流创建一两个命令。代码审查和修复 issue 是很好的起点。

4. **步骤4**：随着项目增长，你的 CLAUDE.md 变挤了，开始把指令拆分到 `.claude/rules/` 文件。在合理的地方按路径作用域。

5. **步骤5**：添加 `~/.claude/CLAUDE.md` 放你的个人偏好。这可能是"总是在实现前写类型"或者"偏好函数模式胜过类"。

对于 95% 的项目，这真的就是你需要的一切。当你有值得打包的重复复杂工作流时，才需要用到 skills 和 agents。

---

## 核心洞见

`.claude` 文件夹真的是一个协议，告诉 Claude 你是谁，你的项目做什么，它应该遵守什么规则。你定义得越清楚，你花在纠正 Claude 上的时间就越少，它花在做有用工作上的时间就越多。

**CLAUDE.md 是你最高杠杆的文件。先把它弄对。** 其他一切都是优化。

从小开始，边走边改进，把它当成你项目中另一段基础设施：一旦正确设置，它每天都会给你带来回报。

---

原文：[@akshay_pachaar](https://x.com/akshay_pachaar/status/2035341800739877091)

*本文翻译整理，不代表本站观点*
