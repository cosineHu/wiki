---
title: 认知闭环操作系统
created: 2026-06-08
updated: 2026-06-08
type: concept
tags: [architecture, methodology, knowledge-management, llm]
sources: [raw/articles/cognitive-closed-loop-wiki1-2026.md]
confidence: high
---

# 认知闭环操作系统

从静态知识图谱升级为场景驱动的认知闭环操作系统。核心理念：知识管理的终点不是知识图谱，而是认知组装能力。

## 三层架构

```
场景层 (Scenarios)  →  我要解决什么问题？（组装规则）
概念层 (Concepts)   →  用什么方法解决？（原子+组合双层）
实体层 (Entities)   →  调用哪些工具/对象？（5分类+3层父子）
```

贯穿三层的 IPO 闭环（Input → Process → Output）让系统从"描述性知识"变为"可执行规范"。

## 核心设计原则

1. **分离关注点**：概念（动词/方法）与实体（名词/工具）严格分离
2. **原子-组合双层**：原子概念有完整 IPO，组合概念通过 decomposition 引用原子概念
3. **场景驱动**：知识库的价值在于"组装"而非"存储"，场景层定义组装规则
4. **自我迭代**：每次使用后执行 yaml 反向校验，发现新场景/概念/实体/关系
5. **最小关系集**：实体间仅 4 种关系：is_a、uses、depends_on、related_to

## 演进路径

资料库（raw/） → 知识库（wiki/） → 模式库（wiki/meta/）

每一次迭代，知识的可用性都向前推进：从"我能看"到"我能查"，再到"AI 能调用、能组装、能自我迭代"。

## 与 LLM Wiki 的关系

- wiki/ = 资料库 + 知识库（萃取后的结构化文章）
- wiki/meta/ = 模式库（元信息：结构、关系、组装规则）
- wiki/meta/ 通过 `source:` 字段反向指回 wiki/ 中的原始页面

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | 用户的知识管理需求（查询、写作、分析、决策） |
| **Process** | ① 场景匹配：从 meta/scenarios/ 匹配当前任务场景 → ② 概念组装：按场景的 composition 规则编排原子概念和组合概念 → ③ 实体调取：按场景的 entities 字段加载所需工具/对象 → ④ 原文深挖：通过 source 字段回溯 wiki/ 原始页面 → ⑤ 结构化执行：按 IPO 规范逐步执行 → ⑥ 反向校验：任务完成后执行 yaml 反向校验，发现缺口 |
| **Output** | 可执行的认知方案 + 自动发现的知识缺口（写入 _pending/） |
| **Tools** | meta/scenarios/scenarios.yaml, meta/meta-concepts.yaml, meta/compose-concepts.yaml, meta/entities/*.yaml, wiki/ 原始页面 |
| **Quality Check** | 场景是否被正确匹配？组装规则是否完整执行？反向校验是否发现了新场景/概念/实体/关系？ |

## 参考

- [[llm-wiki]] — LLM Wiki 基础模式
- [[enterprise-second-brain-architecture]] — 企业级第二大脑架构
- [[hermes-skills-system]] — Hermes 技能系统（类比：概念 = 技能，场景 = 编排）
- [[rag-vs-wiki]] — RAG vs Wiki 对比（meta/ 是 Wiki 的下一阶段进化）
