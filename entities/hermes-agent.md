---
title: Hermes Agent
created: 2026-06-02
updated: 2026-06-03
type: entity
tags: [llm, open-source, agent]
sources: [raw/articles/hermes-llm-wiki-skill-2026.md, raw/articles/hermes-second-brain-part1-2026.md, raw/articles/hermes-second-brain-part2-2026.md, raw/articles/openclaw-vs-hermes-2026.md, raw/articles/hermes-agent-deep-dive-2026.md]
confidence: high
---

# Hermes Agent

由 [Nous Research](https://nousresearch.com) 开发的 AI Agent 平台，MIT 许可证开源。

## 关键信息

- **开发者**：Nous Research
- **许可证**：MIT
- **官网**：<https://hermes-agent.nousresearch.com>
- **仓库**：<https://github.com/NousResearch/hermes-agent>

## 内置 Skill 体系

Hermes 拥有丰富的内置 skill 生态，涵盖研究、笔记、开发、媒体等多个领域。

### research 类别
- [[LLM Wiki（Karpathy 模式）]]（`research/llm-wiki` v2.1.0）— 互联 Markdown 知识库
- Arxiv — 论文检索
- Blogwatcher — 博客监控
- Polymarket — 预测市场

### 其他类别
- note-taking/obsidian — Obsidian 集成
- github/codebase-inspection — 代码库分析
- email/himalaya — 邮件管理

## 第二大脑集成

[[Hermes Agent]] + [[Obsidian]] 可构建 [[第二大脑（Second Brain）]]，核心包括：
- [[分层记忆系统]]（L0-L3）
- [[LLM Wiki（Karpathy 模式）]] 知识摄入
- 与 [[Dify]]、[[n8n]]、[[Ollama]] 等工具集成


## 部署架构

基于 Docker Compose 的生产级部署，核心组件：
- **Hermes Web** — Web UI (:3000)
- **Hermes API** — API 服务 (:8080)
- **Hermes Worker** — 任务队列处理器
- **PostgreSQL** — 持久化数据存储
- **Redis** — 缓存 + 任务队列
- **Nginx/Caddy** — 反向代理 + HTTPS

部署注意事项：至少 4GB RAM、配置 Docker 镜像加速器、使用 healthcheck 确保启动顺序、时区设置 TZ=Asia/Shanghai。

## 核心特性

### 学习闭环
- [[Hermes 技能系统（Skills System）]] — 自动生成+改进的 Skill 机制，程序性记忆
- 持久记忆 + 用户建模（Honcho）+ 会话搜索（SQLite FTS5）

### 工具系统
- 40+ 内置工具，支持 MCP 协议接入外部工具
- [[Hermes 终端后端（Terminal Backends）]] — 六种执行后端（Local/Docker/SSH/Daytona/Singularity/Modal）
- 并行子 Agent，独立上下文窗口

### 多平台
- Telegram / Discord / Slack / WhatsApp / Signal / Home Assistant / CLI
- 所有平台共享记忆和技能系统

### 模型无关
- 200+ 模型（OpenRouter），支持 OpenAI/Anthropic/NVIDIA/HuggingFace 等
- 一条命令切换模型

### RL 训练集成
- 与 Atropos（Nous Research 的 RL 框架）集成
- 批量轨迹生成 → 训练更强的 tool-calling 模型 → 驱动更好的 Agent

### 安全
- 命令审批（白名单自动放行，危险命令需确认）
- DM 配对（只响应授权用户）
- 容器隔离（Docker 后端）

## 与其他框架对比

参见 [[Hermes Agent vs 其他 Agent 框架]]

## 相关

- [[LLM Wiki（Karpathy 模式）]]
- [[Andrej Karpathy]]
- [[Karpathy 式知识库方法（Karpathy Knowledge Base Method）]]
- [[obsidian-headless]]
- [[第二大脑（Second Brain）]]
- [[分层记忆系统]]
- [[Obsidian]]
- [[双向链接（Obsidian）]]
- [[Obsidian 标签系统与 Frontmatter]]
- [[Obsidian Dataview 查询]]
- [[OpenClaw]]
- [[OpenClaw vs Hermes Agent]]
- [[Hermes 知识库参考方式]]
- [[Hermes 会话隔离机制]]
- [[Hermes 技能系统（Skills System）]]
- [[Hermes 终端后端（Terminal Backends）]]
- [[Hermes Agent vs 其他 Agent 框架]]
- [[Hermes 记忆架构（Memory Architecture）]]
