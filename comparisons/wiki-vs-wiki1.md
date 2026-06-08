---
title: 场景驱动的认知闭环 — meta 元模型
created: 2026-06-08
updated: 2026-06-08
type: comparison
tags: [architecture, methodology, comparison, knowledge-management]
sources: [raw/articles/cognitive-closed-loop-wiki1-2026.md]
confidence: high
---

# wiki/ vs meta/ — 从知识库到认知操作系统

## 对比维度

| 维度 | wiki/（知识库） | meta/（认知操作系统） |
|------|----------------|----------------------|
| 定位 | 图书馆 — 存储和检索知识 | 工厂 — 组装和执行知识 |
| 核心对象 | 知识页面（Markdown） | 元模型（YAML） |
| 组织方式 | 按类型分目录（entities/concepts/comparisons） | 三层架构（场景→概念→实体） |
| AI 使用方式 | 检索相关页面，由 AI 自行理解 | 按场景规则组装概念和实体 |
| 概念建模 | 叙述性描述（"是什么"） | IPO 流程建模（"怎么用"） |
| 实体组织 | 扁平列表 | 3 层父子结构 + 4 种关系 |
| 组装能力 | 无 — AI 需自行编排 | 有 — 场景定义 composition 规则 |
| 自我迭代 | 手动（新增/更新页面） | 自动（yaml 反向校验 → 骨架生成） |
| 存储内容 | 知识本身 | 结构、关系、组装规则 |
| 文件格式 | Markdown | YAML |

## 为什么需要 meta/

wiki/ 解决了"知识萃取"问题，但没有解决"知识组装"问题：
- AI 能复述知识，但无法面对新场景把知识编排成针对性方案
- 概念和实体混杂，AI 无法区分"方法"和"工具"
- 方法描述缺乏可执行规范（只有"是什么"，没有"怎么用"）
- 缺乏面向问题的组装机制

meta/ 通过三层架构 + IPO 闭环 + yaml 反向校验，让 AI 从"理解驱动"转向"组装驱动"。

## 两者关系

- meta/ 不替代 wiki/，而是 wiki/ 的元层，统一在 wiki/ 目录下
- meta/ 通过 `source:` 字段反向指回 wiki/ 中的原始页面
- wiki/ 继续作为知识内容的存储和萃取层
- meta/ 提供"如何调用和组装这些知识"的规则

## 参考

- [[cognitive-closed-loop]] — 认知闭环操作系统概念
- [[llm-wiki]] — LLM Wiki 基础模式
- [[rag-vs-wiki]] — RAG vs Wiki 对比
