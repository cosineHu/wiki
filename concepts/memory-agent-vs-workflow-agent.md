---
title: Memory Agent vs Workflow Agent
created: 2026-06-04
updated: 2026-06-05
type: concept
tags: [agent, llm, memory, comparison]
sources: [raw/articles/enterprise-second-brain-2026.md]
confidence: high
---

# Memory Agent vs Workflow Agent

AI Agent 的两条根本路线分歧：完成任务后，经验是消失还是积累？

## Workflow Agent

当前主流 Agent 平台（Dify、Coze、OpenClaw）的本质：

```
用户输入 → LLM推理 → 工具调用 → 输出结果
```

**任务结束后：**
- 经验消失
- 知识消失
- 能力不增长

Agent 不会因为完成了 100 个任务而变得更聪明。

## Memory Agent

Hermes 的路线：

```
执行任务 → 记录过程 → 总结经验 → 形成技能 → 下次复用
```

即：**Task → Memory → Reflection → Skill → Next Task**

Agent 具备**持续成长能力**。

## 核心差异

| | Workflow Agent | Memory Agent |
|---|---|---|
| **代表** | Dify、Coze、OpenClaw | Hermes Agent |
| **核心循环** | 输入→推理→输出 | 任务→记忆→反思→技能 |
| **任务后** | 经验消失 | 经验沉淀为 Skill |
| **长期价值** | 每次从零开始 | **复利式增长** |
| **知识积累** | 无 | 工作记忆→情景记忆→语义记忆→技能记忆 |
| **类比** | 每次查食谱做饭 | 练熟后形成肌肉记忆 |

## Hermes 的四层记忆

| 层级 | 内容 | 生命周期 |
|------|------|----------|
| **工作记忆** | 当前任务上下文 | 任务结束释放 |
| **情景记忆** | Agent 完成过的真实案例 | 持久化，可检索 |
| **语义记忆** | 沉淀的稳定知识（MCP协议、向量数据库等） | 长期积累 |
| **技能记忆** | 如何完成任务（部署Milvus、构建RAG等） | 持续进化 |

## Reflection 机制

Hermes 最大的创新之一——任务完成后不立即结束，而是执行反思：

```
本次任务成功了吗？
为什么成功？为什么失败？
下次如何优化？
```

形成可复用经验。

## Skill Evolution

```
原始经历：完成一次 RAG 项目
     ↓ 总结
标准流程：RAG 部署标准流程
     ↓ 抽象
技能：企业知识库建设
```

实现 **经验 → 技能** 转化。

## 相关

- [[Hermes Agent]]
- [[Hermes 记忆架构（Memory Architecture）]]
- [[Hermes 技能系统（Skills System）]]
- [[OpenClaw vs Hermes Agent]]
- [[企业级第二大脑架构（Enterprise Second Brain）]]
- [[分层记忆系统]] — 概念层面的 L0-L3 四层模型
- [[三种记忆模型对比（Three Memory Models）]] — 三模型映射关系与综合对比
