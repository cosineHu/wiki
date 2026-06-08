---
title: YAML 反向校验
created: 2026-06-08
updated: 2026-06-08
type: concept
tags: [knowledge-base, llm, architecture]
sources: [raw/articles/scenario-driven-cognitive-loop-2026.md]
confidence: high
---

# YAML 反向校验

## 定义

YAML 反向校验（YAML Reverse Validation）是场景驱动认知闭环中的自我迭代机制。每次 AI 基于知识库完成任务后，强制执行一次反向检视：检查任务过程中是否出现了知识库中未定义的新场景、新概念、新实体、新关系或死链，并将发现以 yaml 条目骨架形式返回，由人决定是否合入主库。

## 核心流程

```
AI 完成任务
    ↓
反向检视（5 个检查维度）
    ↓
生成 yaml 条目骨架
    ↓
人工审核 → 合入主库
    ↓
知识库生长
```

## 五个检查维度

### 1. 新场景检测

- 本次任务是否涉及 yaml 中找不到的新场景？
- 如果有，提取 trigger、goal、composition 骨架

### 2. 新概念检测

- 是否用到了 yaml 里没定义的新概念（原子或组合）？
- 如果有，提取 IPO 四元组骨架

### 3. 新实体检测

- 是否提及了 yaml 里没有的新实体（工具、产品、人物等）？
- 如果有，提取实体属性和关系骨架

### 4. 新关系检测

- 发现了哪些新的语义关系应该补充进现有条目？
- 包括概念间关系、实体间关系、概念-实体间关系

### 5. 死链检测

- 是否引用了不存在的 id？
- 是否有概念引用了已被删除或重命名的实体？

## 输出格式

每次反向校验产出一个结构化的 yaml 骨架：

```yaml
# 反向校验报告
timestamp: 2026-06-08T12:00:00
task: "基于知识库进行文章写作"

new_scenarios:
  - trigger: "..."
    goal: "..."
    composition: [...]

new_concepts:
  - name: "..."
    type: meta | compose
    ipo: {input, process, output, tools}

new_entities:
  - name: "..."
    type: tool | product | person | ...
    relations: [...]

new_relations:
  - from: "..."
    to: "..."
    type: is_a | uses | depends_on | related_to

dead_links:
  - reference: "..."
    reason: "目标不存在"
```

## 为什么需要反向校验

### 让知识库在使用中生长

传统知识库的更新依赖人工主动维护，而反向校验让每一次使用都成为知识库的一次压力测试和生长机会。知识库从"我手动定义的样子"慢慢演化成"被真实使用场景塑造的样子"。

### 闭环哲学

任何可持续生长的系统都必须包含一个反馈闭环。学习-实践-复盘是个人成长的闭环，PDCA 是组织管理的闭环，场景-组装-校验是知识库自身演化的闭环。

### 降低维护成本

人工维护知识库需要持续的意志力和时间投入，而反向校验将维护动作嵌入到使用流程中，让维护成为使用的自然副产品。

## 与 7 步写作流水线的关系

反向校验是 7 步标准写作流水线的第 7 步：

1. 问题解析
2. 场景匹配
3. 概念组装
4. 实体调取
5. 原文深挖
6. 结构化写作
7. **YAML 反向校验** ← 这一步让系统具备生长能力

## 与 Wiki Lint 的区别

| 维度 | Wiki Lint | YAML 反向校验 |
|------|-----------|-------------|
| 触发时机 | 定期（手动触发） | 每次任务完成后 |
| 检查范围 | 全量（所有页面） | 增量（本次任务涉及的范围） |
| 检查内容 | 断链、孤立页、过期内容等 | 新场景/概念/实体/关系/死链 |
| 产出 | 问题报告 | yaml 条目骨架（可直接合入） |
| 目的 | 质量保障 | 知识生长 |

## 相关概念

- [[scenario-driven-cognitive-loop]] — YAML 反向校验是认知闭环的自我迭代机制
- [[ipo-closed-loop]] — 反向校验会检查 IPO 的完整性和有效性
- [[atom-compose-concept-architecture]] — 反向校验会发现新的原子/组合概念
- [[llm-wiki]] — LLM Wiki 的 lint 功能是反向校验的前身

## IPO 建模

| 维度 | 内容 |
|------|------|
| **Input** | 一次完整的知识库使用会话（场景执行记录 + 工具调用日志 + 产出内容） |
| **Process** | ① 新场景检查（是否有未定义的 trigger/goal/composition） → ② 新概念检查（是否有未收录的方法/动作） → ③ 新实体检查（是否有未收录的人事物/名词） → ④ 新关系检查（是否有未记录的 is_a/uses/depends_on/related_to） → ⑤ 死链检查（yaml 和 wiki 中的引用是否有效） → ⑥ 汇总为结构化报告 → ⑦ 生成补充骨架写入 meta/_pending/ |
| **Output** | 反向校验报告（yaml 格式）+ 补充骨架文件（可合入 meta/ 的 yaml 片段） |
| **Tools** | search_files, read_file, write_file, meta/ yaml 文件 |
| **Quality Check** | 5 个维度是否全部覆盖？发现的缺口是否有对应的补充骨架？报告是否可操作？ |

> YAML 反向校验是认知闭环的"反馈回路"——每次使用都是一次进化机会，让知识库在使用中持续生长。
