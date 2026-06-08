---
title: kepano (Steph Ango)
created: 2026-06-08
updated: 2026-06-08
type: entity
tags: [person, tool, knowledge-base, philosophy]
sources: [raw/articles/knowledge-management-llm-skills-2026.md]
confidence: high
---

# kepano (Steph Ango)

[[obsidian]] 的创始人兼 CEO，产品哲学家。真名 Steph Ango。在 Karpathy 提出 LLM 驱动的知识库方法论后，他提出了审慎的「人机边界」警告。

## 核心哲学

### File over app（文件优先于应用）

「如果你希望你的写作在 2060 年甚至 2160 年的电脑上仍然可读，那它最好在 1960 年代的电脑上也能被读取。」

核心主张：数据格式的长期可读性比应用功能更重要。这也是 Obsidian 选择纯 Markdown 格式的哲学根基。

### Don't delegate understanding（不要把理解力外包）

「你可以让 AI 帮你干活，但不能让 AI 替你思考。理解力本身就是你最大的资产。」

在个人 Obsidian 实践中，他甚至说：「有人问我能不能用 LLM 自动化笔记整理，但我不想这么做。我享受这个过程。做这种维护工作帮助我理解自己的模式。」

## 对 AI 知识库的「冷水」

当 Karpathy 提出让 LLM 全权管理知识库时，kepano 提出了关键警告：

### 核心观点

你的个人笔记库应该是「你」的思想的体现，而不是 AI 的。

### 具体建议

- **保持个人仓库干净** — 这是你的思维主场
- **给 Agent 一个单独的「杂乱仓库」** — 让 AI 在那里随便折腾
- **只在 Agent 产出有用工件时，才手动搬到主仓库**

### 担心的后果

如果人类写的和 AI 写的混在一起太多：

- 搜索会被 AI 生成内容淹没
- 反向链接不再局限于你真正思考过的内容
- 图谱视图变成 AI 的知识图谱，不再是你的
- 无法追溯哪些想法是自己的、哪些是机器生成的

## 物理隔离 vs 溯源标记

kepano 的方案是物理隔离（两个仓库），老章提出另一种方案：溯源标记（YAML frontmatter 的 author 字段、文件路径、Skill 执行日志）。详见 [[ai-human-knowledge-boundary]]。

## 在知识管理全链路中的位置

守护环节：[[knowledge-management-pipeline]] 中「守护」维度的代表，守住了最重要的底线——知识库的主人是你，不是 AI。

## 相关

- [[obsidian]]
- [[andrej-karpathy]]
- [[lex-fridman]]
- [[knowledge-management-pipeline]]
- [[ai-human-knowledge-boundary]]
- [[second-brain]]
