---
title: Hermes 知识库参考方式
created: 2026-06-03
updated: 2026-06-08
type: concept
tags: [llm, agent, knowledge-base, reference]
sources: [raw/articles/hermes-vs-openclaw-features-2026.md]
confidence: high
---

# Hermes 知识库参考方式

Hermes Agent 在会话中参考知识库回答的四种方式，按自动化程度和适用场景排列。

## 四种方式对比

| 方式 | 加载机制 | 容量 | 适用场景 |
|------|----------|------|----------|
| **AGENTS.md / MEMORY.md** | 每次对话自动注入 | 不限（但影响 token 消耗） | 精炼的核心知识（项目规范、常用 API、决策记录） |
| **Skill** | 按需加载，识别到相关话题时自动触发 | 按主题分块 | 按领域分块的知识（框架用法、特定流程） |
| **Memory 工具** | 每轮对话自动注入 | ~2200 字符 | 少量、持久、高价值的参考信息 |
| **Obsidian Vault** | 主动搜索（需调用工具） | 不限 | 大量笔记/文档，按需检索 |

## 方式详解

### 1. AGENTS.md / MEMORY.md（自动注入，最省心）

放在工作目录下，每次对话自动注入到上下文中。

```bash
# 项目根目录
~/project/AGENTS.md     # 项目规范、架构决策
~/project/MEMORY.md     # 持久上下文
```

**适合**：精炼的核心知识，如项目规范、常用 API、决策记录。

### 2. Skill（按需加载，按主题分类）

用 `skill_manage` 创建知识型 skill，识别到相关话题时自动加载。

```bash
hermes skill create --name "react-patterns" --category "frontend"
```

**适合**：按领域分块的知识，如某个框架的用法、特定流程。

### 3. Memory 工具（每次都看，容量有限）

用 `memory` 工具存入关键事实，每轮对话都会注入。

```
容量约 2200 字符，不适合大段知识。
适合少量、持久、高价值的参考信息。
```

### 4. Obsidian 知识库（主动搜索）

创建 Obsidian vault 目录，使用 obsidian skill 实时搜索和读取笔记。

> ⚠️ 需要在对话中主动调用工具去查，不是自动注入的。

**适合**：知识量大、需要按需检索的场景。

## 推荐组合方案

```
精炼的核心知识     → AGENTS.md（自动注入）
按主题分块的领域知识 → Skill（按需加载）
大量笔记/文档       → Obsidian Vault（按需搜索）
关键事实/偏好       → Memory 工具（自动注入）
```

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | Agent 在会话中需要参考的知识（项目规范、领域知识、关键事实、大量文档） |
| **Process** | ① 精炼核心知识 → AGENTS.md/MEMORY.md（自动注入，每轮可见） → ② 按主题分块 → Skill（按需加载，识别话题自动触发） → ③ 关键事实/偏好 → Memory 工具（自动注入，~2200字符） → ④ 大量笔记/文档 → Obsidian Vault（主动搜索，按需检索） |
| **Output** | 四层知识参考体系，从自动注入到按需检索，覆盖不同粒度和容量的知识需求 |
| **Tools** | AGENTS.md, MEMORY.md, skill_manage, memory 工具, Obsidian skill |
| **Quality Check** | 核心知识是否精炼（不浪费 token）？Skill 是否按领域合理分块？Memory 是否只存高价值持久事实？ |

## 相关

- [[Hermes Agent]]
- [[第二大脑（Second Brain）]]
- [[LLM Wiki（Karpathy 模式）]]
- [[Obsidian]]
