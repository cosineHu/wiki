---
title: 原子-组合概念双层架构
created: 2026-06-08
updated: 2026-06-08
type: concept
tags: [architecture, knowledge-base, llm]
sources: [raw/articles/scenario-driven-cognitive-loop-2026.md]
confidence: high
---

# 原子-组合概念双层架构

## 定义

原子-组合概念双层架构（Atom-Compose Concept Architecture）是场景驱动认知闭环中概念层的核心设计模式。它将所有"方法"（概念）拆分为两个层级：不可再分的原子概念和由原子概念串接而成的组合概念。这种设计对应软件工程中"原子函数"和"组合函数"的关系，是实现知识复用和组装的基础。

## 两层结构

### 原子概念 (Meta-Concepts)

- 不可再分的最小方法单元
- 必须有完整的 IPO 四元组（Input → Process → Output + Tools）
- 不依赖其他概念组装
- 示例：归纳、演绎、抽象、MECE 法则、第一性原理、因果链分析

### 组合概念 (Compose-Concepts)

- 由若干原子概念串接而成的复合方法
- 必须包含至少两步的 decomposition
- 每一步引用一个 meta 概念，并写明"为什么在这一步需要它"
- 示例：深度思考、系统思考、批判性思考、快速学习

## 判别标准

| 维度 | 原子概念 | 组合概念 |
|------|---------|---------|
| 可分解性 | 不可再分 | 可分解为多个原子步骤 |
| IPO 四元组 | 必须有 | 不直接有，通过 decomposition 间接定义 |
| 依赖关系 | 不依赖其他概念 | 依赖多个原子概念 |
| 复用方式 | 被组合概念引用 | 被场景引用 |

## 示例：深度思考（组合概念）

decomposition：
1. 问题定义 → 调用 meta:problem-definition
2. 第一性原理拆解 → 调用 meta:first-principles
3. SBR 模型推演 → 调用 meta:sbr-model
4. 因果链分析 → 调用 meta:causal-chain
5. 结论复盘 → 调用 meta:retrospective

## 示例：第一性原理（原子概念）

- **输入**: 待分析的复杂问题
- **处理步骤**: 识别表面现象 → 剥离类比假设 → 追问底层不可再分要素 → 重新基于第一原理推导
- **输出**: 基于底层逻辑的解决方案
- **工具实体**: SBR 模型、归纳-演绎闭环

## 核心价值

### 复用性

同一个原子概念被多个组合概念引用，同一个组合概念被多个场景引用。新增场景时，大概率只需重新编排已有概念，不必从零定义新方法。

### 边际扩展成本递减

知识库的边际扩展成本随着积累越来越低——这是任何可持续生长的体系都必须具备的特性。

### 颗粒度最优

在足够精细以保证可执行性、和足够抽象以保证可复用性之间，找到最优平衡点。这个平衡点不是一次性找到的，而是在反复使用中校准的。

## 与软件工程的类比

| 知识体系 | 软件工程 |
|---------|---------|
| 原子概念 | 原子函数 / 基础组件 |
| 组合概念 | 组合函数 / 高阶组件 |
| 场景 | 业务流程 / Use Case |
| 实体 | 数据模型 / 外部依赖 |
| IPO 闭环 | 函数签名 (Input → Output) |

## 相关概念

- [[scenario-driven-cognitive-loop]] — 原子-组合架构是认知闭环概念层的核心设计
- [[ipo-closed-loop]] — IPO 闭环是原子概念的建模方式
- [[llm-wiki]] — LLM Wiki 的概念层可以升级为原子-组合双层架构
- [[hermes-skills-system]] — Hermes 技能的原子-组合模式与本架构一致

## IPO 建模

| 维度 | 内容 |
|------|------|
| **Input** | 需要建模的方法论集合（概念清单 + 使用场景） |
| **Process** | ① 识别所有方法，判断可分解性 → ② 不可再分的归为原子概念，填充 IPO 四元组 → ③ 可分解的归为组合概念，写出 decomposition（每步引用原子概念 + 目的说明） → ④ 建立概念间的引用关系 → ⑤ 写入 meta-concepts.yaml 和 compose-concepts.yaml |
| **Output** | 两层概念体系（原子概念 yaml + 组合概念 yaml），支持场景层按需组装 |
| **Tools** | read_file, write_file, patch, meta/ yaml 文件 |
| **Quality Check** | 原子概念是否有完整 IPO？组合概念是否有至少 2 步 decomposition？引用关系是否有效？ |

> 原子-组合双层架构的本质是"颗粒度管理"——在可执行性和可复用性之间找到最优平衡。
