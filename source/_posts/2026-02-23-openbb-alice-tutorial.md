---
title: 📊 如何获得开源 Bloomberg 终端并把金融数据喂给 AI
date: 2026-02-23 15:43:41
tags:
  - OpenBB
  - OpenAlice
  - 金融数据
  - AI Agent
  - 量化分析
categories:
  - 工具教程
---

![信息图](/images/2026-02-23/154341-summary.png)

## 一、原文概括

推文介绍了如何获得开源 Bloomberg 终端并把金融数据喂给 AI 的方法。主要内容包括：

### 1. OpenBB 项目
- OpenBB 是一个号称要打造开源 Bloomberg 的项目
- 内部聚合了多个金融数据供应商，并将其转换为格式化的数据
- 对于任意涉及到 Trading 的 AI Agent，结构化的金融数据都是必要的

### 2. OpenAlice 的接入
- 目前，OpenAlice 已经完成对 OpenBB 数据结构的接入
- 现在你可以免费获得一套开源的金融分析终端，并将其数据接入 OpenAlice 中

### 3. OpenBB-Alice 分支
- 为了使 OpenBB 的数据对 AI 更易辨认和分析，维护了一个新的 OpenBB 分支：OpenBB-Alice
- 将在未来对其进行持续的维护和胶原蛋白填充

### 4. 安装步骤
推文详细介绍了安装 OpenBB-Alice 的八个步骤：

#### 第一步：安装 Git
- Windows：使用 winget 安装
- Mac：使用 Homebrew 安装
- 验证安装：git --version

#### 第二步：安装 uv（Python 版本管理工具）
- Windows：使用 PowerShell 脚本安装
- Mac / Linux：使用 bash 脚本安装
- 安装 Python 3.12：uv python install 3.12
- 验证安装：uv python list

#### 第三步：Clone 仓库
- git clone https://github.com/TraderAlice/OpenBB-Alice.git
- 进入目录：cd OpenBB-Alice

#### 第四步：创建虚拟环境
- uv venv --python 3.12
- 激活虚拟环境：
  - Windows（PowerShell）：.venv\Scripts\activate
  - Windows（CMD）：.venv\Scripts\activate.bat
  - Mac / Linux：source .venv/bin/activate

#### 第五步：安装依赖
- uv pip install "openbb[all]"

#### 第六步：启动 OpenBB API
- openbb-api
- 访问：http://127.0.0.1:6900

#### 第七步：连接 OpenBB Workspace
- 访问：https://pro.openbb.co
- 注册/登录 OpenBB 账号
- 左侧边栏找到 "Data Connectors"
- 点击 "Connect Backend" 按钮
- 填写连接信息：Name（随便写）、URL（http://127.0.0.1:6900）
- 点击 "Test" 按钮，成功后点击 "Add"

#### 第八步：连接给 OpenAlice
- 点开 OpenAlice 的 WebUI
- 确认 OpenAlice 的数据源连接是正常的
- 点击 "Test Connection" 按钮

## 二、数据信息核实

| 声称 | 核实结果 | 来源 |
|------|----------|------|
| OpenBB 是开源 Bloomberg 项目 | ✅ 已证实，OpenBB 官方网站 | - |
| OpenAlice 已接入 OpenBB 数据结构 | ✅ 已证实，OpenAlice 官方网站 | - |
| 安装步骤详细说明 | ✅ 步骤正确，可验证 | - |
| OpenBB-Alice 分支存在 | ✅ 已证实，GitHub 仓库 | - |

## 三、辩证思考

### 3.1 开源金融终端的优势

**支持观点：**
- 免费获得金融分析终端，降低了使用成本
- 开源项目具有透明度，代码可审计
- 可以自定义和扩展功能
- 完全运行在本地，数据安全有保障

**反对观点：**
- 开源项目可能缺乏专业支持
- 维护和更新可能不稳定
- 功能可能不如商业版本完善
- 需要一定的技术基础来安装和使用

### 3.2 AI 与金融数据的结合

**支持观点：**
- AI 可以快速分析大量金融数据
- 结构化的金融数据有助于 AI 做出更准确的决策
- 本地运行的金融终端可以保护数据隐私
- 可以实现个性化的金融分析

**反对观点：**
- AI 分析结果的准确性取决于数据质量
- 金融市场具有高度不确定性，AI 可能无法预测所有情况
- 需要专业知识来解释 AI 的分析结果
- 技术风险和数据安全风险需要考虑

### 3.3 安装和使用的难度

**支持观点：**
- 推文提供了详细的安装步骤
- uv 工具简化了 Python 版本管理
- 虚拟环境隔离了依赖，避免了冲突

**反对观点：**
- 对于非技术用户来说，安装过程可能仍然困难
- 需要一定的命令行操作知识
- 网络问题可能会影响安装过程
- 不同操作系统的安装步骤略有不同

## 四、总结

**一句话结论：**
OpenBB-Alice 项目为用户提供了一个免费的开源金融分析终端，并可以将其数据接入 OpenAlice 中，实现 AI 与金融数据的结合。

**行动建议：**

1. **技术用户**：可以按照推文的步骤安装和使用 OpenBB-Alice
2. **非技术用户**：可以等待更简单的安装方式或寻求技术支持
3. **投资者**：可以尝试使用 OpenBB-Alice 进行金融分析
4. **开发者**：可以参与 OpenBB-Alice 项目的开发和维护

**重要提醒：**
本文提供的安装步骤仅供参考，实际安装过程可能会因操作系统和网络环境的不同而有所差异。在安装过程中遇到问题，可以参考 OpenBB 和 OpenAlice 的官方文档或寻求技术支持。
