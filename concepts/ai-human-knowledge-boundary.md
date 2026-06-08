---
title: AI 与人知识边界（AI-Human Knowledge Boundary）
created: 2026-06-08
updated: 2026-06-08
type: concept
tags: [knowledge-base, llm, philosophy, comparison]
sources: [raw/articles/knowledge-management-llm-skills-2026.md]
confidence: high
---

# AI 与人知识边界

当 AI 深度参与知识管理时，如何划分人类思考与机器生成之间的边界？这是 [[kepano]] 在回应 [[andrej-karpathy]] 的 LLM 知识库方法论时提出的核心问题。

## 问题本质

如果人类写的和 AI 写的混在一起太多：

- 搜索会被 AI 生成内容淹没
- 反向链接不再局限于你真正思考过的内容
- 图谱视图变成 AI 的知识图谱，不再是你的
- 无法追溯哪些想法是自己的、哪些是机器生成的

## 两种方案

### 方案 A：物理隔离（kepano）

- **做法**：两个仓库——一个干净的给自己，一个「杂乱仓库」给 AI
- **原则**：只在 Agent 产出有用工件时，才手动搬到主仓库
- **适合**：把 Obsidian 当「思维延伸」的人，追求高信号噪声比
- **优点**：边界清晰，永不混淆
- **缺点**：需要手动搬运，人机协作效率降低

### 方案 B：溯源标记（老章）

- **做法**：所有 AI 生成内容带清晰来源标记
  - YAML frontmatter 的 author 字段
  - 文件路径本身（如 Video/口播稿/ 一看就知道是 AI 转的）
  - Skill 的执行日志
- **原则**：不需要物理隔离，但要知道每一段内容是谁写的、用什么工具写的、基于什么素材写的
- **适合**：人机协作频繁、产出量大的场景
- **优点**：灵活高效，自动化程度高
- **缺点**：依赖标记规范的严格执行

## 共同底线

两种方案都认同 kepano 的核心原则：

> **Don't delegate understanding（不要把理解力外包）**

你可以让 AI 帮你整理知识，但你不能让 AI 替你理解知识。如果你连自己知识库里的内容都没读过、没消化过，那这个知识库再大也不是你的。

## 与 llm-wiki 的关系

[[llm-wiki]] 模式通过以下机制天然支持溯源：

- raw/ 目录不可变——原始来源永远可追溯
- wiki 页面的 `sources:` frontmatter 记录来源
- Provenance markers（`^[raw/articles/source.md]`）标记段落级来源
- `confidence` 字段标记单源 vs 多源佐证

## IPO 建模

| 维度 | 内容 |
|------|------|
| **Input** | AI 生成的内容（文章、摘要、分析）+ 人类原创的笔记和思考 |
| **Process** | ① 识别内容来源（AI 生成 vs 人类原创） → ② 选择边界方案（物理隔离 或 溯源标记） → ③ 执行标记/隔离 → ④ 定期审查边界是否清晰 → ⑤ 人类确认「理解」而非「外包」 |
| **Output** | 边界清晰的知识库：每段内容可追溯到人类或 AI，信号噪声比可控 |
| **Tools** | YAML frontmatter, 文件路径约定, Skill 执行日志, Git 版本历史 |
| **Quality Check** | 能否快速区分人类原创和 AI 生成？搜索是否被 AI 内容淹没？图谱视图是否反映真实思考？ |

## 相关

- [[kepano]]
- [[karpathy-knowledge-base-method]]
- [[knowledge-management-pipeline]]
- [[content-production-pipeline]]
- [[llm-wiki]]
- [[second-brain]]
