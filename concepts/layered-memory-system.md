---
title: 分层记忆系统
created: 2026-06-02
updated: 2026-06-08
type: concept
tags: [llm, agent, architecture]
sources: [raw/articles/hermes-obsidian-second-brain-2026.md, raw/articles/hermes-second-brain-part1-2026.md, raw/articles/hermes-second-brain-part2-2026.md]
confidence: high
---

# 分层记忆系统

[[Hermes Agent]] + [[Obsidian]] 第二大脑方案中的核心记忆架构，将 AI 记忆分为四个层级。

## 四层架构

| 层级 | 名称 | 作用 | 生命周期 |
|------|------|------|----------|
| **L0** | 工作记忆 | 当前会话上下文 | 单次对话 |
| **L1** | 短期记忆 | 近期对话摘要 | 数天~数周 |
| **L2** | 长期记忆 | 跨会话知识提炼 | 持久化 |
| **L3** | 知识图谱 | 结构化知识网络 | 持久化 + 可视化 |

## 流转机制

```
L0 工作记忆 ──(会话结束)──→ L1 短期记忆
L1 短期记忆 ──(衰减/提炼)──→ L2 长期记忆
L2 长期记忆 ──(结构化)────→ L3 知识图谱（Obsidian 双向链接）
```

- **衰减机制**：不重要的记忆随时间自动降级或丢弃
- **提炼机制**：多次出现的知识点从 L1 提升到 L2
- **结构化**：L2 中的实体和关系自动映射为 Obsidian 的 `[[wikilinks]]`

## 设计优势

- **避免上下文污染**：不同频次的信息放不同层，热层保持精简
- **精准检索**：冷层内容通过标签和图谱可被精准定位
- **渐进积累**：越用越丰富，类似人类的记忆巩固过程

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | Agent 会话中产生的上下文、对话摘要、知识点、实体关系 |
| **Process** | ① L0 工作记忆：当前会话上下文（会话结束触发流转） → ② L1 短期记忆：近期对话摘要，不重要的随时间衰减 → ③ L2 长期记忆：多次出现的知识点提炼提升 → ④ L3 知识图谱：实体和关系映射为 Obsidian [[wikilinks]] |
| **Output** | 四层渐进积累的记忆系统，类似人类的记忆巩固过程 |
| **Tools** | Hermes memory 工具, session_search, Obsidian 双向链接 |
| **Quality Check** | L0→L1 流转是否正常？L1→L2 提炼是否准确？L3 知识图谱是否可被 Dataview 查询？ |

## 相关

- [[第二大脑（Second Brain）]]
- [[Hermes Agent]]
- [[LLM Wiki（Karpathy 模式）]]
- [[Hermes 记忆架构（Memory Architecture）]] — Hermes 的具体实现视角
- [[Memory Agent vs Workflow Agent]] — 认知心理学视角的四层记忆
- [[三种记忆模型对比（Three Memory Models）]] — 三模型映射关系与综合对比
