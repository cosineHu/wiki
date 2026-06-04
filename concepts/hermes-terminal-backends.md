---
title: Hermes 终端后端（Terminal Backends）
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [llm, agent, devops, serverless]
sources: [raw/articles/hermes-agent-deep-dive-2026.md]
confidence: high
---

# Hermes 终端后端（Terminal Backends）

Hermes Agent 支持**六种不同的代码执行后端**，让 Agent 可以在不同的隔离环境中运行代码。

## 六种后端

| 后端 | 适用场景 | 特点 |
|------|----------|------|
| **Local** | 本机直接执行 | 最快，无隔离 |
| **Docker** | 本地容器化执行 | 隔离好，需要 Docker |
| **SSH** | 远程服务器 | 适合在云服务器上运行 |
| **Daytona** | Serverless 开发环境 | 闲置时自动休眠，近乎零成本 |
| **Singularity** | HPC 集群 | 科研计算环境 |
| **Modal** | Serverless GPU | 需要 GPU 计算时按需唤醒 |

## 亮点：Daytona 和 Modal

Agent 的执行环境可以在云端持久化：

- **闲置时自动休眠**（几乎零成本）
- **有任务时唤醒**

这意味着你可以在 Telegram 上给 Agent 发一条消息，它在云端虚拟机上工作，完成后通知你，**全程不需要你的笔记本开着**。

## 并行子 Agent

Hermes 支持派生隔离的子 Agent 并行处理多个工作流：

- 每个子 Agent 通过 RPC 调用工具
- 结果汇总回主 Agent
- 每个子 Agent 有独立的上下文窗口，不消耗主 Agent 的 context
- 也可以用 Python 脚本直接调用工具（零 context cost）

## 相关

- [[Hermes Agent]]
- [[Hermes 技能系统（Skills System）]]
- [[Hermes 会话隔离机制]]
