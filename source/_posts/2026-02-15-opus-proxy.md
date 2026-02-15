---
title: 🧠 本地 Proxy 实战：把 Opus 4.6 变成 OpenClaw 大脑
date: 2026-02-15 22:33:34
tags:
  - OpenClaw
  - Claude
  - Opus
  - Proxy
  - AI
  - 本地部署
categories:
  - 技术教程
---

![Opus 4.6 本地 Proxy](/images/2026-02-15/2026-02-15-22-33-00-opus-proxy.png)

## 背景

作者试了各家模型当 OpenClaw 大脑：
- Kimi K2.5
- Gemini 3 Pro
- GPT 5.2
- **MiniMax M2.5**

**结论**：所有活人感"最强模型里**"**的就是 **Opus 4.6**。

> "你跟它讲话会觉得它有个性、有灵魂。个性不像 GPT 5.2 那么三八，说话直接，像个标准的工程师直男，但关键时刻又会给你足够的情绪价值，推理能力又超强。"

---

## 问题：太贵

| 项目 | 成本 |
|------|------|
| 第一天 | 不到 1 小时烧 10 美金 |
| 一个月 | 几千美金跑不掉 |

---

## 方案对比

### 方案一：Session Token 白嫖

- **问题**：A 社随时能根据 TOS 把你账号 Ban 掉
- **风险**：太高，Reddit 上一堆惨案
- **后果**：
  - 历史对话没了
  - Project Context 没了
  - 几周调教出来的思维惯性全部归零

### 方案二：本地 Proxy（推荐）

> "为什么 not 直接用 Claude Code 的 CLI 当大脑？"

**原理**：
- Claude Max 订阅一个月 **200 美金**
- Opus 4.6 随便用
- 通过 CLI 发出的 Request，官方看来是**正规开发行为**
- 因为你用的正是他们自己的 Binary

---

## 架构设计

```
Telegram/Discord → OpenClaw → Proxy → Claude Code CLI → Anthropic API
```

**核心思路**：
1. OpenClaw 接收 Telegram/Discord 消息
2. 转成 CLI 的输入
3. 把响应转回来
4. **中间没有任何第三方服务器**
5. 所有东西都在 Mac mini 上跑

---

## 三道坎

### 第一道：权限

**问题**：CLI 每一步都要人按 y 确认

**解决**：调整 Proxy 启动参数，让 CC 启动时可以直接跳过互动式确认

### 第二道：环境

**问题**：怎么让 Proxy 驱动 CLI？

**解决**：用**模拟 TTY 终端**的方式来跑
- Anthropic 提供 Agent SDK 可以 programmatic 调用
- 但需要驱动完整的 CLI 环境（tool use、file editing、git 整合）
- CLI 启动时会检查自己是不是在真实终端里
- 在 OpenClaw 里**模拟了一个 TTY 环境**，骗过这层检查

### 第三道：浏览器

**问题**：CLI 解锁后能写代码、跑指令、搜网页，但碰不到真正的浏览器

**解决**：把 OpenClaw 的 Playwright 浏览器能力封装成指令，让它通过 CLI 就能操作 Chrome
- 登录态的书签抓取
- 动态网页截图
- 全部打通

---

## 成果

从午夜折腾到凌晨两点半，**不到三个小时**：

| 功能 | 状态 |
|------|------|
| 搜网页 | ✅ |
| 操作浏览器 | ✅ |
| 主动发消息 | ✅ |
| 排程任务 | ✅ |
| 生成子代理 | ✅ |

**100% 追平 OpenClaw 原生 Agent 的功能**

---

## 优势

### 同一大脑，同一 Context

> "日常聊天速度确实比原生 Agent 慢一点，毕竟中间多了一层 Proxy。但 Opus 4.6 每一步都一次到位，像个靠谱的合伙人，很容易 get 到你到底想干嘛，还会帮你多想一步。"

| 传统方式 | 本地 Proxy |
|----------|------------|
| 聊天用一个模型 | 聊天 + 写 Code 同一大脑 |
| 写代码派给 Coding Agent | 同一份 Context |
| 两层延迟 + Context 丢失 | 读完需求下一秒就能改档 |

**结论**：慢半拍但不返工，算下来反而更快。

---

## 注意事项

1. **不要把 heartbeat 类工作交给它** — 太固定的 request 可能被视为机器流量
2. **用轻量模型处理简单任务** — 如 Gemini 3 Flash
3. **不要爆 Token** — 正常使用不会被封账号

---

## 总结

> "既然有最好的灵魂，就该亲手为它打造最适合的躯壳。"

**本地 Proxy 思路**：
- 用 Claude Code CLI 当大脑
- 通过 Telegram/Discord 互动
- 官方允许的操作，没有违规风险
- 200 美金/月，Opus 4.6 随便用

---

*本文基于 @BensonTWN 的推文整理*

*参考：OpenClaw + Claude Code 本地部署*
