---
title: 🚀 gogcli 0.11.0 发布：Google Workspace 的命令行瑞士军刀
date: 2026-02-15 10:00:00
tags:
  - gogcli
  - Google Workspace
  - CLI
  - 生产力工具
  - 自动化
categories:
  - 技术探索
---

## 命令行管理 Google Workspace？

如果你重度依赖 Google Workspace（Gmail、Drive、Docs、Sheets），但厌倦了在浏览器里点来点去，**gogcli** 可能是你需要的工具。

这是由 @steipete 开发的 Google Workspace CLI 工具，让你可以用命令行完成几乎所有日常操作。

---

## ✨ 0.11.0 新功能一览

刚刚发布的 v0.11.0 带来了多项重磅更新：

### 📝 Apps Script 支持
```bash
# 创建 Apps Script 项目
gogcli apps-script create --name "My Automation"

# 直接运行函数
gogcli apps-script run myFunction
```

终于可以在终端里管理 Google Apps Script 了！告别浏览器里的 Script Editor。

### 📊 Google Forms 全功能
```bash
# 创建表单
gogcli forms create --title "客户满意度调查"

# 获取所有回复
gogcli forms responses get <form-id>

# 导出为 CSV
gogcli forms responses export --format csv > responses.csv
```

批量处理表单数据、自动化分析，不再需要手动点"导出"。

### 💬 文档评论和表格批注
```bash
# 获取 Docs 中的所有评论
gogcli docs comments list <doc-id>

# 给 Sheets 单元格添加批注
gogcli sheets notes add --cell "A1" --note "需要审核"
```

团队协作场景下超实用——批量处理评论、自动同步批注。

### 📧 Gmail 增强
```bash
# 带引用的回复
gogcli gmail reply --id <message-id> --quote --body "收到，我来处理"
```

保持邮件线程的上下文，回复格式规范。

### 📁 Drive 改进
- **默认移到垃圾桶**（更安全，可恢复）
- **全程支持共享云端硬盘**（Shared Drives）

```bash
# 删除文件（实际移到垃圾桶）
gogcli drive delete <file-id>

# 在共享云端硬盘中操作
gogcli drive list --shared-drive "团队资料"
```

### 🔐 更安全的 OAuth 流程
改进了认证流程，降低了权限泄露风险。

---

## 🛠️ 安装与配置

### 安装
```bash
# macOS (Homebrew)
brew install gogcli

# 或从 GitHub Releases 下载
curl -L https://github.com/steipete/gogcli/releases/latest/download/gogcli-$(uname -s)-$(uname -m) -o gogcli
chmod +x gogcli
```

### 初始化配置
```bash
# 第一次运行，会打开浏览器进行 OAuth 授权
gogcli auth login

# 验证连接
gogcli drive list
```

---

## 💡 实用场景

### 场景一：批量邮件处理
```bash
#!/bin/bash
# 自动归档已处理的客户邮件

SEARCH_QUERY="from:client@example.com is:unread"

# 获取邮件列表
gogcli gmail search "$SEARCH_QUERY" --format json | \
  jq -r '.[].id' | \
  while read msg_id; do
    # 自动回复
    gogcli gmail reply --id "$msg_id" \
      --body "感谢您的来信，我们已收到并会尽快处理。"
    
    # 添加标签并归档
    gogcli gmail label add --id "$msg_id" --label "客户邮件"
    gogcli gmail archive --id "$msg_id"
  done
```

### 场景二：自动备份 Drive 文件
```bash
#!/bin/bash
# 每周备份重要文件夹到本地

BACKUP_DIR="/backup/google-drive/$(date +%Y-%m-%d)"
mkdir -p "$BACKUP_DIR"

# 下载特定文件夹
gogcli drive download --folder "重要文档" --output "$BACKUP_DIR"

# 清理 30 天前的备份
find /backup/google-drive -type d -mtime +30 -exec rm -rf {} +
```

### 场景三：表单数据自动处理
```bash
#!/bin/bash
# 每小时检查新表单提交并发送通知

FORM_ID="1AbCdEfGhIjKlMnOpQrStUvWxYz"
LAST_CHECK_FILE="/tmp/last_form_check"

# 获取上次检查时间
LAST_CHECK=$(cat "$LAST_CHECK_FILE" 2>/dev/null || echo "1 hour ago")

# 获取新回复
gogcli forms responses get "$FORM_ID" --since "$LAST_CHECK" | \
  jq -c '.responses[]' | \
  while read response; do
    EMAIL=$(echo "$response" | jq -r '.respondentEmail')
    ANSWERS=$(echo "$response" | jq -r '.answers')
    
    # 发送通知
    gogcli gmail send \
      --to "admin@company.com" \
      --subject "新表单提交: $EMAIL" \
      --body "$ANSWERS"
  done

# 更新时间戳
date > "$LAST_CHECK_FILE"
```

---

## 🔧 进阶技巧

### 与其他 CLI 工具组合
```bash
# 用 fzf 交互式选择文件
gogcli drive list | fzf | xargs gogcli drive download

# 用 jq 处理 JSON 输出
gogcli sheets get <sheet-id> | jq '.values[] | select(.[0] == "完成")'

# 配合 cron 定时任务
0 9 * * * /usr/local/bin/gogcli gmail search "is:unread from:boss" --notify
```

### Shell 补全
```bash
# Bash
echo 'source <(gogcli completion bash)' >> ~/.bashrc

# Zsh
echo 'source <(gogcli completion zsh)' >> ~/.zshrc
```

---

## 📊 与同类工具对比

| 工具 | 特点 | 适用场景 |
|------|------|---------|
| **gogcli** | 功能全面，持续更新 | 重度 Google Workspace 用户 |
| gdrive | 专注 Drive，简单易用 | 仅需要 Drive 操作 |
| GAM | 专注 Google Workspace 管理 | 企业 IT 管理员 |
| rclone | 通用云存储同步 | 多平台备份 |

**gogcli 的优势：**
- 原生支持 Google Workspace 特有功能（Apps Script、Forms）
- 现代化 CLI 设计（子命令结构、JSON 输出）
- 活跃的开发和社区

---

## 🔗 资源链接

- **GitHub**: https://github.com/steipete/gogcli
- **Releases**: https://github.com/steipete/gogcli/releases
- **作者 Twitter**: @steipete

---

## 总结

如果你每天在 Google Workspace 里花费大量时间，gogcli 能帮你：

- ⚡ 批量处理重复操作
- 🤖 自动化工作流程  
- 🔄 与其他工具集成
- 💻 保持终端工作流不中断

尤其是那些需要定期处理表单数据、批量操作 Drive 文件、或者自动化邮件处理的同学，值得一试。

---

*本文基于 gogcli v0.11.0 发布信息整理。*  
*感谢 @steipete 和 Molty 的贡献！*
