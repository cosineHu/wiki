# Reading Map: 知识本体构建 — 基于 LLM Wiki 的大模型知识库

## 入口文章

**知识本体构建：基于 LLM Wiki 的大模型知识库**
→ `raw/2026-06-05-knowledge-ontology/knowledge-ontology-llm-wiki.md`

## 文章结构

```
1. 知识库分层结构概览 (img_01, img_02)
   └── 三层结构：场景 → 概念 → 实体

2. 知识本体建模的两个核心
   ├── 核心一：基于历史资料萃取概念和实体，构建知识图谱
   └── 核心二：场景问题层模型构建（关键创新）

3. 场景层详解 (img_03)
   ├── 5 大类 47 个场景
   ├── 每个场景：阶段划分 + 概念/实体引用 + 组装流程
   └── 场景 = 概念和实体按步骤组装的过程

4. 概念层详解 (img_04)
   ├── 原子概念：IPO 四元组（Input/Process/Output）
   ├── 组合概念：decomposition 分解结构
   └── 动态逻辑引入概念方法

5. 实体层详解 (img_05)
   ├── 父实体/子实体分层
   └── 实体分类：工具/产品、方法论、人物/技术

6. 知识图谱可视化 (img_06~img_11)
   ├── 分层网络图（场景层→概念层→实体层）
   ├── 场景支撑图（选中场景，动态列出支撑概念和实体）
   ├── 泳道图（分阶段展示）
   └── 力导向图（完整知识图谱）

7. 7 步写作流水线
   ├── Step 1: 问题解析
   ├── Step 2: 场景匹配（scenario-method.yaml）
   ├── Step 3: 概念组装（meta-concepts.yaml + compose-concepts.yaml）
   ├── Step 4: 实体调取（entity-*.yaml）
   ├── Step 5: 原文深挖（wiki/concepts/*.md）
   ├── Step 6: 结构化写作
   └── Step 7: YAML 反向校验（5 维检查）

8. 自我进化能力 (img_12)
   └── 每次写作后 AI 分析 → 新场景/概念/实体 → 死链修正

9. 应用场景 (img_13, img_14)
   ├── AI 知识库应用：复杂场景写作方案
   ├── 10 万字完整文章输出
   └── 方案类文档：立项报告、采购文件、招标文件、标准规范
```

## 与 wiki 现有内容的关联

| 本文概念 | wiki 已有页面 |
|---------|-------------|
| 三层架构 | `concepts/scenario-driven-cognitive-loop.md` |
| 认知闭环 | `concepts/cognitive-closed-loop.md` |
| 原子-组合概念 | `concepts/atom-compose-concept-architecture.md` |
| IPO 闭环 | `concepts/ipo-closed-loop.md` |
| YAML 反向校验 | `concepts/yaml-reverse-validation.md` |
| 7 步流水线 | `meta/profile.md` |
| LLM Wiki | `concepts/llm-wiki.md` |
| RAG vs Wiki | `comparisons/rag-vs-wiki.md` |
| **新增** 知识本体三层模型 | `concepts/knowledge-ontology-three-layer-model.md` |

## 阅读建议

1. 先读本文了解"场景→概念→实体"三层模型的**动机和来源**（为什么需要场景层）
2. 再读 `concepts/scenario-driven-cognitive-loop.md` 了解**技术实现**（YAML 结构、composition 规则）
3. 对照 `meta/profile.md` 理解 7 步流水线的**实际操作**
4. 最后读 `concepts/knowledge-ontology-three-layer-model.md` 了解与传统本体建模的**对比视角**
