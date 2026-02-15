---
title: 🔧 OpenCode 调用第三方 API 指南：豆包 Seed Code 2.0 为例
date: 2026-02-15 21:36:26
tags:
  - OpenCode
  - 豆包
  - API
  - 配置
  - 技术教程
categories:
  - 技术教程
---

![OpenCode 第三方API配置](/images/2026-02-15/2026-02-15-21-36-00-opencode-custom-api.png)

## 发现

OpenCode 不仅仅支持官方的模型，还可以调用任何第三方的 **OpenAI 兼容 API**！

包括最近火热的**豆包 Seed Code 2.0**。

---

## 配置方法

### 第一步：创建配置文件

在 `/.config/opencode/` 目录下创建 `opencode.json`：

```bash
mkdir -p ~/.config/opencode
touch ~/.config/opencode/opencode.json
```

### 第二步：写入配置

参考官方配置示例：

```json
{
  "providers": {
    "doubao": {
      "api_key": "你的API密钥",
      "base_url": "https://ark.cn-beijing.volces.com/api/coding/v3",
      "models": ["doubao-seed-2.0-code"]
    }
  }
}
```

### 第三步：在 OpenCode 中使用

配置完成后，就可以在 OpenCode 中选择配置的第三方模型了。

---

## 支持的模型

根据 OpenCode 文档，理论上支持所有 **OpenAI 兼容 API**，包括：

- 豆包 Seed Code 2.0
- 火山引擎系列模型
- 任何第三方兼容 API

---

## 官方文档

- **配置示例**：https://gist.github.com/chenshaoju/af933465eee6dd18fa1c16716dcec2bc
- **OpenCode 文档**：https://opencode.ai/docs/providers/#custom-provider

---

## 应用场景

1. **使用国内模型**：绕过网络限制，调用豆包等国内模型
2. **成本优化**：选择性价比更高的模型
3. **模型对比**：同时测试多个模型的效果

---

## 我的测试

之前我已经成功使用 OpenCode 配置了 Kimi K2.5，详见之前的博客。

这次配置豆包 Seed Code 2.0 的流程类似，有兴趣的朋友可以试试！

---

*本文基于 @chenshaoju 的推文整理*

*参考：OpenCode 官方文档*
