# 多智能体自动化系统 — Agent 工作流

> 🧠 Hermes Agent Skill · 四层闭环架构 · 一键安装调用

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Hermes Agent](https://img.shields.io/badge/Hermes-Skill-green)](https://github.com/youxiao240388/agent-workflow)
[![HTML5 Demo](https://img.shields.io/badge/HTML5-Demo-orange)](https://youxiao240388.github.io/agent-workflow/)

---

## 这是什么

一套跑在**飞书（Lark）**上的多智能体自动化系统，可作为 **Hermes Agent Skill** 安装使用。底层对接 DeepSeek/Qwen 等异构大模型集群，上层是可自编排的工具链。

**三个核心能力：**

| 能力 | 输入 | 输出 |
|------|------|------|
| 🧾 **发票智能处理** | JPG/PNG/PDF 发票 | 规范命名 PDF + 飞书推送 |
| 🌐 **网页知识提取** | JS 渲染页面 URL | 结构化 Word 文档 |
| 💬 **群聊知识蒸馏** | 飞书群消息流 | 知识条目 + 月度复盘 |

---

## 🚀 快速安装

### 前置条件

- Hermes Agent 已安装
- Python 3.10+
- 飞书应用凭证（[申请地址](https://open.feishu.cn/)）

### 安装 Skill

```bash
# 1. 克隆仓库
git clone https://github.com/youxiao240388/agent-workflow.git \
  ~/.hermes/skills/productivity/multi-agent-automation

# 2. 安装 Python 依赖
pip install rapidocr-onnxruntime python-docx img2pdf playwright
playwright install chromium

# 3. 配置环境变量
cp ~/.hermes/skills/productivity/multi-agent-automation/config/.env.example \
   ~/.hermes/.env
# 编辑 ~/.hermes/.env，填入飞书凭证和模型 API Key

# 4. 重启 Hermes Agent
```

### 安装子技能

```bash
# 发票处理
cp -r ~/.hermes/skills/productivity/multi-agent-automation/config/skills/invoice-pdf-renamer \
  ~/.hermes/skills/productivity/

# 网页转文档
cp -r ~/.hermes/skills/productivity/multi-agent-automation/config/skills/web-article-to-word \
  ~/.hermes/skills/productivity/
```

---

## 🎬 在线演示

点击链接在浏览器中体验（← → 键翻页）：

| 演示 | 说明 |
|------|------|
| 🏗 [架构总览](https://youxiao240388.github.io/agent-workflow/assets/demos/index.html) | 10 页 PPT，四层闭环架构全览 |
| 🧾 [发票处理](https://youxiao240388.github.io/agent-workflow/assets/demos/workflow_01.html) | 3-5分钟→30秒，格式100%合规 |
| 🌐 [网页提取](https://youxiao240388.github.io/agent-workflow/assets/demos/workflow_02.html) | OOXML保真引擎，零返工 |
| 💬 [知识蒸馏](https://youxiao240388.github.io/agent-workflow/assets/demos/workflow_03.html) | 用后即焚→组织神经中枢 |

---

## 📂 目录结构

```
agent-workflow/
├── SKILL.md                     # Skill 定义（触发条件、工作流、配置）
├── README.md                    # 项目说明（本文件）
├── LICENSE                      # MIT
├── config/
│   └── .env.example             # 环境变量模板
├── references/
│   └── architecture.md          # 架构设计参考
└── assets/
    ├── demos/                   # 在线演示 HTML
    │   ├── index.html           #   架构总览（10页PPT动画）
    │   ├── workflow_01.html     #   发票处理管线
    │   ├── workflow_02.html     #   网页知识提取
    │   └── workflow_03.html     #   群聊知识蒸馏
    └── videos/                  # 录屏演示 MP4
        ├── 架构总览_四层闭环.mp4
        ├── workflow_01_发票处理.mp4
        ├── workflow_02_网页知识提取.mp4
        └── workflow_03_群聊知识蒸馏.mp4
```

---

## 🏗 架构

```
感知层 → 推理层 → 执行层 → 沉淀层
  ↑____________________________↓
        闭环反馈 · 知识反哺决策
```

详见 [references/architecture.md](references/architecture.md)

---

## 🔧 工程亮点

### 文档结构保真引擎
逆向 OOXML `numbering.xml` 规范，独立编号注册机制，确保跨格式源到标准 Word 文档的结构 **100% 保真**。

### 三级模型容错链
```
DeepSeek-V4 → Qwen3-8B → V4-Free (1M ctx)
```
毫秒级自动切换，30 天+零中断。

### 敏感数据本地清洗
涉及金额、个人信息的原始数据在本地完成脱敏后才进入云端推理链路，架构层面预留私有化部署可能。

---

## 📊 实际效果

| 指标 | 优化前 | 优化后 |
|------|--------|--------|
| 单张发票处理 | 3-5 分钟 | **~30 秒** |
| 格式合规率 | 人工校对 | **100%** |
| 文档转换返工 | 频繁 | **零返工** |
| 群聊知识沉淀 | 近乎 0 | **日均 10+ 条** |
| 日均 Token 消耗 | - | 300-500 万 |

---

## 🛠 技术栈

- **Agent 框架**: Hermes Agent
- **推理引擎**: DeepSeek-V4-Flash / Qwen3-8B / Zenmux
- **OCR**: rapidocr-onnxruntime（纯 Python，零系统依赖）
- **文档生成**: python-docx + OOXML 逆向
- **Web 抓取**: Playwright (headless Chromium)
- **消息通道**: 飞书 WebSocket 长连接
- **前端演示**: HTML5 + CSS Animation + Lucide Icons

---

## 📄 开源协议

MIT License

---

*多智能体自动化系统 · V2.0 · 2026年5月*
