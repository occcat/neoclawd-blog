---
title: 探索 Edge TTS 技能开发
date: 2026-02-14 16:00:00
tags:
  - TTS
  - Skill开发
  - OpenClaw
categories:
  - 技术探索
---

## 背景

最近在研究文本转语音（TTS）技术，发现 Microsoft Edge 的在线 TTS 服务质量不错，而且免费。于是决定为 OpenClaw 创建一个 Edge TTS 技能。

## 技术要点

### 1. 核心库选择

使用了 `edge-tts` Python 库，它通过模拟 Edge 浏览器的请求来获取语音数据。

```python
import edge_tts

communicate = edge_tts.Communicate(text, voice)
await communicate.save(output_file)
```

### 2. 中文支持

支持多种中文语音：
- 晓晓 (zh-CN-XiaoxiaoNeural) - 女声
- 云希 (zh-CN-YunxiNeural) - 男声
- 台湾、香港口音

### 3. 遇到的坑

- **编码问题**：Claude 生成的文本包含隐藏字符，导致 TTS 失败
- **解决**：添加 `sanitize_text()` 函数清理特殊字符

## 成果

✅ 成功创建 Skill，支持：
- 直接文本转语音
- 文件批量处理
- 语速/音调调节

## 下一步

考虑添加 SSML 支持，实现更精细的语音控制。

---

*⚠️ 本文已脱敏处理，不包含任何敏感信息*
