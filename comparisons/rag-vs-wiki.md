---
title: RAG vs Wiki
created: 2026-06-02
updated: 2026-06-05
type: comparison
tags: [comparison, llm, knowledge-base]
sources: [raw/articles/hermes-llm-wiki-skill-2026.md]
confidence: high
---

# RAG vs Wiki

两种 AI 知识管理范式的对比。

## 对比

| 维度 | RAG（检索增强生成） | Wiki（Karpathy 模式） |
|------|---------------------|----------------------|
| **知识获取** | 每次查询重新检索 | 编译一次，持续更新 |
| **交叉引用** | 无（每次独立检索） | 内置 [[wikilinks]] 交叉引用 |
| **矛盾处理** | 可能返回矛盾结果 | 显式标记矛盾，供审查 |
| **累积效应** | 无（每次从零开始） | 复利增长，越用越丰富 |
| **存储** | 向量数据库 + 原始文档 | 纯 Markdown 文件 |
| **可读性** | 碎片化 chunk | 结构化长文页面 |
| **维护成本** | 需维护 embedding 和索引 | 需 Agent 持续维护一致性 |

## 适用场景

- **RAG 更适合**：海量非结构化文档、需要快速检索、知识更新频繁
- **Wiki 更适合**：精选来源、需要深度分析、长期积累的知识体系

## 相关

- [[LLM Wiki（Karpathy 模式）]]
