---
title: OpenClaw vs Hermes Agent
created: 2026-06-03
updated: 2026-06-03
type: comparison
tags: [comparison, agent, llm, open-source]
sources: [raw/articles/openclaw-vs-hermes-2026.md, raw/articles/hermes-vs-openclaw-features-2026.md]
confidence: medium
---

# OpenClaw vs Hermes Agent

开源 AI Agent 领域最受关注的两条路线对比。基于 Jahangir 在 Medium 发布的文章。

## 核心分歧

| | [[OpenClaw]] | [[Hermes Agent]] |
|---|---|---|
| **核心问题** | 我能连接多少东西？ | 我能多快变得更懂你？ |
| **路线** | 平台（广度优先） | 伙伴（深度优先） |
| **比喻** | 多功能工具刀 | 专业助手 |
| **起源** | Peter Steinberger 个人实验（Clawdbot） | Nous Research 实验室 |
| **增长驱动** | 跨平台连接能力 | 持续学习用户工作方式 |

## 详细对比

### 平台连接

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 通信入口 | Telegram, Slack, Discord, iMessage, WeChat 等 | 相对集中 |
| 模型接入 | 多模型服务商 | 与 Nous Research 模型生态紧密 |
| 社区技能 | 最大社区技能生态 | 内置 skill 体系 |
| 跨渠道体验 | 桌面→手机→其他平台无缝切换 | 侧重服务器端持续运行 |

### 学习能力

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 学习机制 | 通用执行系统 | Closed Learning Loop（反思→识别模式→写入 skill） |
| 重复任务 | 每次重新执行 | 越跑越熟，自动积累经验 |
| 长期价值 | 广度覆盖 | 复利式增长 |

### 记忆系统

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 存储方式 | 可见文件，一条记忆一个文件 | SQLite + FTS + LLM 摘要 |
| 透明度 | 高（用户可直接查看/编辑/删除） | 中（可通过命令查看，但不如文件直观） |
| 维护成本 | 手动控制多 | 自动整理，省心 |
| 信任模型 | 控制权 → 信任感 | 便利性 → 省心 |

### 安全

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 已知风险 | 公开暴露实例多，被安全机构关注 | 公开报告问题较少 |
| 风险来源 | 增长速度、默认配置、开放端口 | 更年轻，审计机会少 |
| 建议 | 不要默认相信默认配置 | 同样适用 |

### 核心能力差异

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **自改进 Skill** | ❌ 无此机制 | ✅ 可复用流程保存为 SKILL.md，后续会话自动加载 |
| **持久记忆** | 文件级记忆 | SQLite + FTS + LLM 摘要，支持 Honcho/Mem0 等后端 |
| **多平台 Gateway** | 主要是 API Server 模式 | 16+ 平台（Telegram/Discord/Slack/飞书/钉钉/企业微信/Signal/Matrix/Email/Webhook） |
| **定时任务** | 需自行搭建 | 内置 cronjob 系统 |
| **定位** | "裸 Agent"，能力依赖自行搭建 | 完整 Agent 基础设施平台，开箱自带记忆/技能/多平台/定时任务 |

## 适合人群

### 选 OpenClaw
- 需要尽可能多的平台连接
- 想使用最大的社区技能生态
- 偏好透明、可手动编辑的记忆
- 做跨渠道、多 agent 的自动化调度

### 选 Hermes Agent
- 看重长期复利式学习
- 工作流重复性强，希望 agent 越跑越熟
- 想在服务器上运行更轻量的 agent
- 希望 agent 与模型实验室能力紧密结合

### 两者都用
- 有足够的基础设施能力
- OpenClaw 负责"广"（调度、规划、多平台路由）
- Hermes 负责"深"（重复执行、持续优化）
- 通过 Agent Communication Protocol 协作

## 深层意义

这场竞争不只是两个开源项目的对比，而是 **AI Agent 未来形态的分歧**：

- **OpenClaw = 平台**：连接入口，把 AI 接入各种系统、工具和渠道
- **Hermes = 伙伴**：随时间理解你、记住你、适应你的习惯

两个方向都很重要。未来的 agent 会常驻在工作系统里，记住上下文，学习流程，调用工具，并在多个场景中持续执行任务。

## 相关

- [[OpenClaw]]
- [[Hermes Agent]]
- [[第二大脑（Second Brain）]]
- [[LLM Wiki（Karpathy 模式）]]
