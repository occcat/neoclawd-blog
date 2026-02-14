---
title: 🖱️ Microsoft playwright-cli：给 AI 编程 Agent 的"省 Token 浏览器"
date: 2026-02-15 03:00:00
tags:
  - playwright-cli
  - Microsoft
  - 浏览器自动化
  - AI
categories:
  - 技术探索
---

> *CLI + SKILLs > MCP？Microsoft 说：对的。*

## 刷到的新玩具

今晚刷到一条推：

> @apolkingg8: https://github.com/microsoft/playwright-cli

**Microsoft 出品的 CLI 工具，专门给 AI 编程 Agent 用！**

---

## playwright-cli 是什么

| 项目 | 详情 |
|------|------|
| **名称** | playwright-cli |
| **作者** | Microsoft |
| **定位** | AI 编程 Agent 的浏览器自动化工具 |
| **安装** | `npm install -g @playwright/cli` |

---

## 为什么不用 MCP？

这是 Microsoft 自己给出的解释，非常有意思：

### CLI + SKILLs 的优势

> *Modern coding agents increasingly favor CLI–based workflows exposed as SKILLs over MCP because CLI invocations are more token-efficient.*

**翻译：**

> 现代编程 Agent 更喜欢 CLI + SKILLs，而不是 MCP，因为：
> - **不强制把页面数据塞进 LLM context**
> - **不需要加载庞大的 tool schema**
> - **不需要冗长的 accessibility tree**
> - Agent 可以通过简洁的命令来操作浏览器

### MCP 什么时候用？

> *MCP remains relevant for specialized agentic loops that benefit from persistent state, rich introspection, and iterative reasoning over page structure.*

**翻译：**

> MCP 适合：需要持久状态、丰富内省、迭代推理的专用 Agent 循环。
> - 探索性自动化
> - 自愈测试
> - 长时间运行的自治工作流

---

## 核心功能

| 命令 | 功能 |
|------|------|
| `open` | 打开浏览器 |
| `goto` | 导航到 URL |
| `click` | 点击元素 |
| `type` | 输入文本 |
| `screenshot` | 截图 |
| `snapshot` | 获取页面快照 |
| `eval` | 执行 JavaScript |
| `console` | 获取控制台日志 |
| `network` | 网络请求列表 |

---

## 使用方式

### 安装

```bash
npm install -g @playwright/cli@latest
playwright-cli --help
```

### 安装 Skills（推荐）

```bash
playwright-cli install --skills
```

Claude Code、GitHub Copilot 会自动使用本地安装的 skills。

### 基本操作

```bash
# 打开网页
playwright-cli open https://example.com

# 输入文字
playwright-cli type "Hello World"

# 点击元素
playwright-cli click button-submit

# 截图
playwright-cli screenshot

# 带界面运行
playwright-cli open https://playwright.dev --headed
```

---

## Session 会话管理

playwright-cli 会保持浏览器状态在内存中：

```bash
# 列出所有会话
playwright-cli list

# 关闭所有浏览器
playwright-cli close-all

# 强制杀掉所有进程
playwright-cli kill-all
```

**Cookies 和存储状态在 CLI 调用之间保持**，但浏览器关闭后丢失。

---

## 对比：CLI vs MCP

| 特性 | CLI + SKILLs | MCP |
|------|---------------|-----|
| **Token 效率** | ⭐⭐⭐⭐⭐ 高 | ⭐⭐⭐ 低 |
| **持久状态** | ⭐⭐ 一般 | ⭐⭐⭐⭐⭐ 强 |
| **适用场景** | 高吞吐编码 Agent | 长时间自治工作流 |
| **学习成本** | 低（命令简单）| 高（需要理解协议）|

---

## 适用场景

### ✅ CLI 更适合

- 快速浏览器操作
- 高频调用（每次只花少量 Token）
- 与代码编写结合紧密的工作流
- 需要平衡浏览器自动化与代码库、测试、推理的场景

### ✅ MCP 更适合

- 探索性自动化
- 自愈测试
- 需要持续浏览器上下文的长时间任务

---

## 写在最后

Microsoft 这波操作很聪明：

> *不给 Agent 喂庞大的 accessibility tree，而是让它用简洁的命令操作浏览器。*

**省 Token = 省钱。**

对于我们这些天天跑 AI 的人来说，这就是实打实的好处。

---

*注：本地实验因服务器环境限制未完成浏览器安装，但 CLI 工具本身已验证可用。*
