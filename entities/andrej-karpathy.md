---
title: Andrej Karpathy
created: 2026-06-02
updated: 2026-06-08
type: entity
tags: [person, ai, open-source]
sources: [raw/articles/hermes-llm-wiki-skill-2026.md, raw/articles/knowledge-management-llm-skills-2026.md]
confidence: high
---

# Andrej Karpathy

AI 研究员，斯坦福 PhD，OpenAI 创始成员，前 Tesla AI 总监，CS231n（全球最火深度学习课程）缔造者。提出了 [[llm-wiki]] 知识管理方法论。

## 知识管理方法论

Karpathy 搭了一套 LLM 驱动的个人知识库系统，用 Obsidian 做前端，Markdown 做底层格式，LLM 负责一切脏活累活。核心理念：把大部分 token 预算花在「操作知识」上，而不是「操作代码」。

完整方法论见 [[karpathy-knowledge-base-method]]，五大模块：

1. 数据导入（Data Import）
2. 知识编译（Wiki Compilation）
3. 问答驱动（Q&A）
4. 输出呈现（Output）
5. 健康检查（Health Check）

## 终局愿景

「随着仓库的增长，自然地也想考虑合成数据生成 + 微调，让你的 LLM '知道'数据在其权重中，而不仅仅是上下文窗口。」——从上下文到权重，训练数字分身。

## 在知识管理全链路中的位置

Karpathy 占据 [[knowledge-management-pipeline]] 的「积累」环节——上游基建，raw/ → compile → wiki → query。

## 相关

- [[llm-wiki]]
- [[karpathy-knowledge-base-method]]
- [[hermes-agent]]
- [[lex-fridman]]
- [[kepano]]
- [[knowledge-management-pipeline]]
- [[second-brain]]
