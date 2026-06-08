---
title: OpenClaw
created: 2026-06-03
updated: 2026-06-05
type: entity
tags: [llm, open-source, agent, platform]
sources: [raw/articles/openclaw-vs-hermes-2026.md]
confidence: high
---

# OpenClaw

开源 AI Agent 平台，强调跨平台连接与社区技能生态。前身为 Clawdbot，由 Peter Steinberger 创建。

## 关键信息

- **创始人**：Peter Steinberger
- **前身**：Clawdbot（后因商标争议更名）
- **治理**：已迁移至独立基金会
- **关联**：创始人与 OpenAI 有关系，社区对其开放性和中立性有讨论

## 核心哲学

OpenClaw 的核心问题是：**"我能连接多少东西？"**

它代表 AI Agent 的**"平台"路线**——成为一个连接入口，把 AI 接入用户数字生活中的各种系统、工具和渠道。

## 核心优势

- **跨平台连接**：Telegram、Slack、Discord、iMessage、WeChat 等多通信入口
- **跨模型支持**：可接入不同模型服务商
- **社区技能生态**：最大的社区技能生态
- **透明记忆**：记忆以可见文件形式存在，用户可直接查看、编辑、删除
- **多 agent 编排**：适合跨渠道、多 agent 的自动化调度

## 记忆系统

OpenClaw 的记忆更**透明**。很多记忆内容以可见文件形式存在，用户可以直接查看、编辑或删除。这种方式对开发者很有吸引力，因为它提供了明确的控制权——你知道 agent 记住了什么，也知道该如何纠正它。

## 安全风险

安全研究人员曾发现大量公开暴露的 OpenClaw 实例。风险来源包括：开放端口、弱认证、恶意社区技能、API key 泄露。增长速度带来了安全挑战。

## 与 Hermes Agent 对比

参见 [[openclaw-vs-hermes]]

| 维度 | OpenClaw | [[hermes-agent]] |
|------|----------|-------------------|
| 核心问题 | 能连接多少东西？ | 能多快变得更懂你？ |
| 路线 | 平台（广度） | 伙伴（深度） |
| 学习方式 | 通用执行系统 | Closed Learning Loop（反思+skill 化） |
| 记忆 | 透明、手动控制 | 自动整理、SQLite+FTS+LLM |
| 适合场景 | 跨平台调度、多 agent 编排 | 重复性工作流、长期复利学习 |

## 组合使用

高级用户常将两者组合：OpenClaw 负责调度、规划、任务拆解和多平台路由；Hermes 负责重复、稳定、可持续优化的执行环节。两者通过 Agent Communication Protocol 等方式协作。

## 相关

- [[hermes-agent]]
- [[openclaw-vs-hermes]]
- [[second-brain]]
