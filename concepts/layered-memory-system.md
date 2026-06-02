---
title: 分层记忆系统
created: 2026-06-02
updated: 2026-06-02
type: concept
tags: [llm, agent, architecture]
sources: [raw/articles/hermes-obsidian-second-brain-2026.md, raw/articles/hermes-second-brain-part1-2026.md]
confidence: medium
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

## 相关

- [[第二大脑（Second Brain）]]
- [[Hermes Agent]]
- [[LLM Wiki（Karpathy 模式）]]
