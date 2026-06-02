---
source_url: https://hermes-agent.nousresearch.com/docs/zh-Hans/user-guide/skills/bundled/research/research-llm-wiki
ingested: 2026-06-02
sha256: 8a3f2c1d4e5b6a7f8e9d0c1b2a3f4e5d6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d
---

# Llm Wiki — Karpathy 的 LLM Wiki

Hermes Agent 内置的 research/llm-wiki skill（v2.1.0，MIT 许可证），基于 Andrej Karpathy 的 LLM Wiki 模式。

## 核心思想

与传统 RAG（每次查询都从头重新发现知识）不同，wiki 只编译一次知识并保持更新。交叉引用已就位，矛盾已被标记，综合分析反映了所有已摄入的内容。

分工：人类负责筛选来源并指导分析。Agent 负责摘要、交叉引用、归档和维护一致性。

## 三层架构

- Layer 1 (raw/): 不可变原始来源 — articles/, papers/, transcripts/, assets/
- Layer 2: Agent 拥有的 Markdown 文件 — entities/, concepts/, comparisons/, queries/
- Layer 3: SCHEMA.md 定义结构、约定和标签分类体系

## 核心操作

### 摄入 (Ingest)
1. 捕获原始来源（URL→raw/articles/, PDF→raw/papers/）
2. 添加 raw frontmatter（source_url, ingested, sha256）
3. 检查已有内容避免重复
4. 创建/更新 wiki 页面（含交叉引用、标签、来源溯源）
5. 更新 index.md 和 log.md

### 查询 (Query)
1. 读 index.md 定位相关页面
2. 综合答案，引用来源页面
3. 有价值答案归档到 queries/ 或 comparisons/

### Lint
12 项检查：孤立页面、断开链接、索引完整性、frontmatter 验证、过时内容、矛盾、质量信号、来源漂移、页面大小、标签审计、日志轮转。

## Obsidian 集成

Wiki 目录开箱即用作为 Obsidian vault。[[wikilinks]] 渲染为可点击链接，图谱视图可视化知识网络，YAML frontmatter 支持 Dataview 查询。

### obsidian-headless
服务器无显示器环境使用，通过 Obsidian Sync 同步（需付费订阅）。也可通过 systemd 实现持续后台同步。

## 关键规则
- 不修改 raw/ 文件
- 每次会话先定位（读 SCHEMA + index + log）
- 每个页面至少 2 个交叉引用
- 标签必须来自分类体系
- 页面超过 200 行应拆分
- 显式处理矛盾，不静默覆盖

## 相关工具
- llm-wiki-compiler: Node.js CLI，批量编译来源目录为概念 wiki
