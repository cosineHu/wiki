---
title: 三种记忆模型对比（Three Memory Models）
created: 2026-06-05
updated: 2026-06-05
type: comparison
tags: [comparison, llm, agent, memory, architecture]
sources: [raw/articles/hermes-obsidian-second-brain-2026.md, raw/articles/hermes-memory-system-2026.md, raw/articles/enterprise-second-brain-2026.md]
confidence: high
---

# 三种记忆模型对比

Wiki 中出现了三种不同的「四层记忆」模型，它们从不同视角描述同一件事——AI Agent 的记忆系统应该如何分层。

## 三模型总览

| 模型 | 来源 | 视角 | 层级命名 |
|------|------|------|----------|
| **分层记忆系统** | 第二大脑系列文章 | 概念设计视角 | L0 工作记忆 / L1 短期记忆 / L2 长期记忆 / L3 知识图谱 |
| **Hermes 记忆架构** | Hermes 源码分析 | 工程实现视角 | 热记忆(MEMORY.md) / 情景回忆(session_search) / 程序性记忆(Skills) / 用户建模(Honcho) |
| **认知心理学模型** | 企业第二大脑论文 | 认知科学视角 | 工作记忆 / 情景记忆 / 语义记忆 / 技能记忆 |

## 映射关系

| 层级 | 分层记忆系统 | Hermes 记忆架构 | 认知心理学模型 |
|------|-------------|-----------------|---------------|
| **第一层** | L0 工作记忆（单次对话） | MEMORY.md + USER.md（冻结快照） | 工作记忆（任务结束释放） |
| **第二层** | L1 短期记忆（数天~数周） | session_search（SQLite+FTS5） | 情景记忆（真实案例） |
| **第三层** | L2 长期记忆（持久化） | Skills（程序性记忆） | 语义记忆（稳定知识） |
| **第四层** | L3 知识图谱（结构化） | Honcho（用户建模） | 技能记忆（持续进化） |

## 关键洞察

### 1. 三模型互补而非矛盾

- **分层记忆系统** 提供了最直观的概念框架，适合向非技术人员解释
- **Hermes 记忆架构** 提供了具体的工程实现细节，适合开发者理解源码
- **认知心理学模型** 提供了学术严谨的分类，适合论文和研究场景

### 2. 共同的核心理念

三个模型都指向同一个设计原则：**热记忆和冷召回分开**。

- 热层（第一层）：始终在上下文中，保持极小（~1,300 tokens）
- 冷层（第二~四层）：按需检索，不消耗每轮 token

这正是 Hermes 区别于 OpenClaw 的关键——OpenClaw 把所有记忆都当作「可搜索的存储知识」，Hermes 则区分「热工作集」和「冷检索层」。

### 3. 缓存感知设计

三个模型都隐含了缓存感知的设计理念：
- 冻结快照确保 prompt 稳定可缓存
- 延迟更新避免缓存失效
- 按需加载减少上下文消耗

## 阅读建议

- 先读 [[layered-memory-system]] — 建立概念框架
- 再读 [[hermes-memory-architecture]] — 理解工程实现
- 最后读 [[memory-agent-vs-workflow-agent]] — 理解为什么这很重要

## 相关

- [[layered-memory-system]]
- [[hermes-memory-architecture]]
- [[memory-agent-vs-workflow-agent]]
- [[second-brain]]
- [[hermes-agent]]
- [[openclaw-vs-hermes]]
