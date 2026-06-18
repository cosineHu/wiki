---
title: Hermes 飞书流式卡片（Hermes Feishu Streaming Card）
created: 2026-06-10
updated: 2026-06-10
type: entity
tags: [hermes, feishu, streaming, card, plugin]
sources: [raw/2026-06-10-hermes-feishu-streaming-card/]
confidence: high
---

# Hermes 飞书流式卡片

Hermes Agent 的飞书流式卡片插件（hermes-feishu-streaming-card），将 Hermes 的所有事件（思考、答案、工具调用、授权确认、运行统计）聚合到同一张飞书卡片里实时更新，解决 Agent 刷屏问题。

## 关键信息

| 维度 | 说明 |
|------|------|
| **来源** | AI松鼠派 公众号 |
| **作者** | baileyh8 |
| **仓库** | `github.com/baileyh8/hermes-feishu-streaming-card` |
| **架构** | Sidecar-only，对 Hermes 侵入极小 |
| **安装** | 一行命令 curl 安装脚本 |

## 解决的痛点

| 痛点 | 解决方案 |
|------|---------|
| 刷屏 | 思考/答案/工具调用聚合到同一张卡片 |
| 漏字乱序 | 流式 delta 持续更新同一卡片 |
| 长内容变乱码 | 表格按结构边界拆分、代码块保持完整 fence |
| 交互靠手打 | approval/clarify 渲染成飞书按钮 |
| 排查靠猜 | doctor 诊断、start/stop/status 进程管理 |

## 核心能力

| 能力 | 说明 |
|------|------|
| 流式卡片 | `thinking.delta`、`answer.delta`、`tool.updated` 持续更新同一张卡片 |
| 卡片内交互 | approval / clarify 渲染成飞书按钮，点击后原任务继续执行 |
| 长内容保护 | 表格按结构边界拆分、代码块保持完整 fence |
| 多 Bot / 多 Profile | 支持多飞书机器人、群聊绑定、per-bot 标题和路由诊断 |
| 故障隔离 | hook fail-open，sidecar 挂了不影响 Hermes 原生文本 |
| 运维工具 | `doctor` 诊断、`start/stop/status` 进程管理、`restore/uninstall` 安全回退 |

## 架构：Sidecar-only

```
Hermes Gateway
  └─ gateway/run.py 中的轻量 hook（fail-open）
       └─ HTTP POST /events ──→ Sidecar Server
                                    ├─ CardSession 状态机
                                    ├─ render_card() 卡片渲染
                                    ├─ Feishu CardKit HTTP Client
                                    ├─ 节流、合并、重试、锁
                                    └─ /health 指标端点
```

Hermes hook 只负责把事件转发给 sidecar，所有飞书发送、更新、重试逻辑都在 sidecar 内独立运行。sidecar 挂了？hook 自动跳过，Hermes 原生文本照常投递。

## 安装

```bash
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/baileyh8/hermes-feishu-streaming-card/main/install.sh | bash

# 验证
python3 -m hermes_feishu_card.cli status --config ~/.hermes/config.yaml
python3 -m hermes_feishu_card.cli smoke-feishu-card --config ~/.hermes/config.yaml --chat-id oc_xxx
python3 -m hermes_feishu_card.cli doctor --hermes-dir ~/.hermes/hermes-agent
```

需确保 Hermes `config.yaml` 中启用流式：
```yaml
streaming:
  enabled: true
  transport: edit
```

## 相关

- [[feishu-card-cli|飞书消息卡片与 CLI 发卡方案]]
- [[feishu-card-cli-analysis|飞书卡片 CLI 方案分析]]
- [[e3-ai-workbench|E3 AI 工作台项目]]
- [[hermes-agent|Hermes Agent]]
