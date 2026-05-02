---
name: multi-agent-automation
description: Use when setting up or using the multi-agent office automation system on Feishu/Lark. Covers invoice processing, web-to-document extraction, and group chat knowledge distillation via a four-layer closed-loop architecture (Perception→Reasoning→Execution→Sedimentation). Triggers include: 发票处理, 网页转文档, 群聊知识沉淀, 多智能体系统配置.
version: 2.0.0
author: youxiao240388
license: MIT
metadata:
  hermes:
    tags: [feishu, automation, multi-agent, invoice, document, knowledge-management, productivity]
    related_skills: [invoice-pdf-renamer, web-article-to-word, ocr-and-documents]
---

# Multi-Agent Office Automation System

A four-layer closed-loop automation system running on Feishu/Lark. Deploys a heterogeneous LLM cluster with self-orchestrating toolchains to fully automate unstructured information processing in daily office workflows.

## Architecture

```
Perception → Reasoning → Execution → Sedimentation
    ↑___________________________________↓
            Feedback Loop · Knowledge-Driven Decisions
```

| Layer | Role | Key Capability |
|-------|------|---------------|
| **Perception** | Multi-modal input gateway | Auto format detection, adaptive extraction (PDF/image/web), dependency-free fallback |
| **Reasoning** | Long-chain task planning + dynamic routing | 3-tier failover (primary→backup→free), sub-second switching |
| **Execution** | Reusable skill library + cross-session memory | Toolchain orchestration, skill solidification, persistent preference memory |
| **Sedimentation** | Group chat knowledge distillation engine | Semantic clustering, auto de-noise/summarize, monthly full-history backtracking |

**Core philosophy:** Each layer handles its own failures — problems don't bubble up.

## When to Use

### Triggers
- **发票处理** — User sends an invoice image/PDF: "处理这张发票" / "rename this invoice"
- **网页转文档** — User shares a URL: "把这个页面转成Word" / "save this article as docx"
- **群聊知识沉淀** — User asks about knowledge distillation or group chat analysis
- **系统配置** — Setting up the Feishu gateway, model providers, or toolchains
- **演示查看** — "show me the architecture" / "演示四层闭环"

### Don't Use For
- Simple Q&A or one-shot tasks — this is for multi-step automation pipelines
- Non-Chinese invoice processing (naming conventions are China-specific)
- Static HTML pages that `curl` can fetch (use web-article-to-word directly)

## Setup

### Prerequisites

```bash
# Python 3.10+
python3 --version

# Core dependencies
pip install rapidocr-onnxruntime python-docx img2pdf playwright
playwright install chromium
```

### Step 1: Feishu Gateway

Create a Feishu app at [Feishu Open Platform](https://open.feishu.cn/), then configure:

```bash
# ~/.hermes/.env
FEISHU_APP_ID=cli_xxxxxxxxxxxx
FEISHU_APP_SECRET=xxxxxxxxxxxxxxxxxxxxxxxxxx
FEISHU_DOMAIN=https://open.feishu.cn
FEISHU_CONNECTION_MODE=websocket
FEISHU_ALLOW_ALL_USERS=true
FEISHU_GROUP_POLICY=open
```

### Step 2: Model Providers

```yaml
# ~/.hermes/config.yaml
model:
  default: deepseek-v4-pro
  provider: deepseek
  base_url: https://api.deepseek.com
  api_key: sk-xxxxxxxxxxxxxxxxxxxxxxxxxx

fallback_providers:
  - provider: siliconflow
    model: deepseek-ai/DeepSeek-V4-Flash
  - provider: zenmux
    model: deepseek/deepseek-v4-flash-free
  - provider: siliconflow
    model: Qwen/Qwen3-8B
```

### Step 3: Install Sub-Skills

```bash
# Clone sub-skills to ~/.hermes/skills/productivity/
git clone https://github.com/youxiao240388/invoice-pdf-renamer.git \
  ~/.hermes/skills/productivity/invoice-pdf-renamer
git clone https://github.com/youxiao240388/web-article-to-word.git \
  ~/.hermes/skills/productivity/web-article-to-word
```

Or copy the bundled skill files from `config/skills/`.

## Workflows

### Workflow 1: Invoice Processing

**Input:** JPG/PNG/PDF invoice image  
**Output:** Renamed PDF + Feishu delivery

Pipeline: `Upload → Format Detection → OCR (rapidocr) → Field Extraction → Category Inference → Naming → Archive → Push`

**Naming convention:**
```
{购买方(去"上海"取前两字)}_{销售方}_{消费类型}_{金额}元_发票号{发票号}.pdf
```

Example: `于外_上海翰文麻辣烫店_午餐费_140.00元_发票号25314000000004646020.pdf`

Delegate to `invoice-pdf-renamer` skill for detailed processing.

### Workflow 2: Web-to-Document Extraction

**Input:** URL of a JS-rendered knowledge base page  
**Output:** Formatted Word (.docx) with preserved structure

Pipeline: `URL → Playwright Render → DOM Structure Extract → OOXML Numbering Isolation → docx Export`

**Key innovation:** Independent `numId` registration per list block — prevents ordered list numbering from chaining across sections. Reverse-engineered OOXML `numbering.xml` specification.

Delegate to `web-article-to-word` skill for detailed processing.

### Workflow 3: Group Chat Knowledge Distillation

**Input:** Real-time Feishu group chat message stream (WebSocket)  
**Output:** Structured knowledge entries + monthly review reports

Pipeline: `WebSocket Ingest → Semantic Clustering → De-noise & Summarize → Structured Rewrite → Archive → Monthly Backtrack`

**Key innovation:** Long-context window (1M tokens) enables full-history backtracking across weeks of chat data, extracting cross-event correlation patterns.

## Model Failover Chain

```
Primary (DeepSeek-V4) → Backup (Qwen3-8B) → Fallback (V4-Free 1M ctx)
         ↓                      ↓                      ↓
    Full capability        Reduced quality        Free tier, high latency
```

Sub-second automatic switching. Zero manual intervention in 30+ days of continuous operation.

## Performance Metrics

| Metric | Before | After |
|--------|--------|-------|
| Invoice processing | 3-5 min | ~30 sec |
| Format compliance | Manual | 100% |
| Document conversion rework | Frequent | Zero |
| Knowledge retention rate | ~0 | 10+ entries/day |
| Daily token consumption | - | 3-5 million |
| System uptime | - | 30+ days, zero downtime |

## Engineering Highlights

### Document Structure Fidelity Engine
Reverse-engineered OOXML `numbering.xml` specification. Independent `numId` registration per list segment ensures 100% structural fidelity across format sources.

### 3-Tier Model Failover
Millisecond-level automatic switching. Nighttime tasks never stall due to provider outages.

### Sensitive Data Local Sanitization
Financial amounts and personal information are sanitized locally before entering cloud inference. Architecture reserves the possibility of fully private deployment.

## Demo

Online interactive demos (PPT-style, ← → keys to navigate):

- **Architecture Overview (10 slides):** [Live Demo](https://youxiao240388.github.io/agent-workflow-demo/assets/demos/index.html)
- **Invoice Pipeline (5 slides):** [Live Demo](https://youxiao240388.github.io/agent-workflow-demo/assets/demos/workflow_01.html)
- **Web Extraction (5 slides):** [Live Demo](https://youxiao240388.github.io/agent-workflow-demo/assets/demos/workflow_02.html)
- **Knowledge Distillation (6 slides):** [Live Demo](https://youxiao240388.github.io/agent-workflow-demo/assets/demos/workflow_03.html)

## Common Pitfalls

1. **Feishu file delivery** — Don't use MEDIA: syntax for Feishu files. Use `im/v1/files` API: upload then send as `msg_type=file`.
2. **OCR dependency** — `rapidocr-onnxruntime` is pure Python (no system deps), but ~80MB first install. Don't fall back to asking users for text.
3. **Invoice naming** — Remove "上海" BEFORE taking first 2 characters, not after. Personal names (e.g., "张海霞") keep full name.
4. **OOXML numbering** — Each `<ol>` block MUST use a different `numId` registered in `numbering.xml`, or numbering chains across sections.
5. **SPA pages** — Synology KB and similar JS-rendered pages require Playwright, not `curl`. Cookie banners may block clicks — use `evaluate()` to extract DOM directly.

## Verification Checklist

- [ ] Feishu gateway responds to test messages
- [ ] Invoice processing: upload a test JPG, verify PDF naming convention
- [ ] Web extraction: convert a Synology KB page, verify no numbering chain issues
- [ ] Knowledge distillation: verify semantic clustering produces distinct units
- [ ] Model failover: kill primary provider, verify automatic fallback
- [ ] Demo pages: open `assets/demos/index.html`, verify ← → navigation and animations
