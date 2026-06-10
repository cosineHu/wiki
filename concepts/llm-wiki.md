---
title: LLM Wiki（Karpathy 模式）
created: 2026-06-02
updated: 2026-06-08
type: concept
tags: [llm, knowledge-base, reference]
sources: [raw/articles/hermes-llm-wiki-skill-2026.md]
confidence: high
---

# LLM Wiki（Karpathy 模式）

由 [[andrej-karpathy]] 提出的一种知识管理方法：将知识库构建为互联的 Markdown 文件，持续积累、复利增长。

## 核心理念

与传统 [[rag-vs-wiki]] 不同，wiki **只编译一次知识并保持更新**，而非每次查询都从头重新发现。

- 交叉引用已就位
- 矛盾已被标记
- 综合分析反映所有已摄入内容

## 分工模式

| 角色 | 职责 |
|------|------|
| **人类** | 筛选来源、指导分析方向 |
| **Agent** | 摘要、交叉引用、归档、维护一致性 |

## 三层架构

```
wiki/
├── SCHEMA.md           # Layer 3: 约定、结构规则、领域配置
├── index.md            # 内容目录（分节 + 单行摘要）
├── log.md              # 追加式操作日志（按年轮转）
├── raw/                # Layer 1: 不可变原始来源
│   ├── articles/       # 网页文章
│   ├── papers/         # PDF、论文
│   ├── transcripts/    # 会议记录、访谈
│   └── assets/         # 图片、图表
├── entities/           # Layer 2: 实体页面
├── concepts/           # Layer 2: 概念页面
├── comparisons/        # Layer 2: 对比分析
└── queries/            # Layer 2: 归档的查询结果
```

## 核心操作

### 摄入（Ingest）
1. 捕获原始来源 → `raw/` 目录（含 `source_url`、`ingested`、`sha256` frontmatter）
2. 检查已有内容避免重复
3. 创建/更新 wiki 页面（含交叉引用、标签、来源溯源）
4. 更新 `index.md` 和 `log.md`

### 查询（Query）
1. 读 `index.md` 定位相关页面
2. 综合答案，引用来源
3. 有价值答案归档到 `queries/` 或 `comparisons/`

### Lint（健康检查）
12 项检查：孤立页面、断开链接、索引完整性、frontmatter 验证、过时内容、矛盾标记、质量信号、来源漂移、页面大小、标签审计、日志轮转。

## 关键规则

- **不修改 `raw/`** — 来源不可变，更正写入 wiki 页面
- **每次会话先定位** — 读 SCHEMA + index + 近期 log
- **每个页面 ≥2 个交叉引用** — 避免孤立页面
- **标签来自分类体系** — 先在 SCHEMA.md 添加再使用
- **页面 >200 行应拆分** — 保持 30 秒可读完
- **显式处理矛盾** — 不静默覆盖，标记 `contested: true`

## 与 Obsidian 集成

Wiki 目录天然兼容 [[obsidian]]，`[[wikilinks]]` 可点击，图谱视图可视化知识网络，YAML frontmatter 支持 Dataview 查询。

服务器端可通过 [[obsidian-headless]] 实现无 GUI 同步。

## IPO 建模

| 维度 | 内容 |
|------|------|
| **Input** | 外部来源（文章 URL、论文 PDF、对话记录）；用户的知识查询 |
| **Process** | ① 捕获原始来源 → raw/ 目录（含 source_url/ingested/sha256） → ② 检查已有内容避免重复 → ③ 萃取实体（名词）和概念（动词） → ④ 创建/更新 wiki 页面（含 frontmatter、交叉引用、标签） → ⑤ 更新 index.md 和 log.md → ⑥ git commit 提交 |
| **Output** | 结构化的互联知识库（markdown 页面 + 索引 + 日志 + Git 版本历史） |
| **Tools** | read_file, write_file, search_files, terminal(git), llm-wiki skill |
| **Quality Check** | 每个页面是否有完整 frontmatter？是否有至少 2 个 [[wikilinks]]？是否避免了重复创建？ |

> LLM Wiki 的本质是"编译一次，持续复用"。IPO 建模明确了从原始信息到结构化知识的完整转化链路。

## 相关工具

- [llm-wiki-compiler](https://github.com/atomicmemory/llm-wiki-compiler)：Node.js CLI，批量编译来源目录为概念 wiki

## 进阶应用

- [[knowledge-ontology-three-layer-model|知识本体三层模型]] — 场景→概念→实体 三层知识架构
- [[scenario-driven-cognitive-loop]] — 场景驱动的知识组装与问题解决
- 知识库文章写作 — 基于三层模型的标准化 AI 写作流水线（页面待创建）
