---
title: Karpathy 式知识库方法（Karpathy Knowledge Base Method）
created: 2026-06-04
updated: 2026-06-08
type: concept
tags: [knowledge-base, llm, markdown, tutorial]
sources: [raw/articles/karpathy-knowledge-base-guide-2026.md]
confidence: high
---

# Karpathy 式知识库方法

Andrej Karpathy 提出的 AI 知识库方法论：**不依赖向量数据库，不搭建 RAG 架构，直接让 LLM 读写 Markdown 文件**。

> 知识管理最耗人的部分，从来不是「读」和「想」，而是整理。让 AI 代替你做这些。

## 核心理念

对于个人知识库规模（约 100 篇文章、40 万词），直接让 LLM 读取它自己维护的摘要文件，效果不比向量检索差，但简单得多。

## 四阶段流程

```
Phase 1: Ingest（摄入）  → 收集原始资料
Phase 2: Compile（编译） → LLM 自动生成摘要、提取概念、建立索引
Phase 3: Query（查询）   → 自然语言或搜索问答
Phase 4: Lint（维护）    → 定期检查和更新知识库
```

## 三层目录结构

```
knowledge-vault/
├── raw/                    # 原始资料（只进不改）
│   ├── articles/           # 网页文章
│   ├── podcasts/           # 播客转录
│   └── papers/             # 论文
│
├── wiki/                   # 编译产物（LLM 维护）
│   ├── indexes/            # All-Sources.md, All-Concepts.md
│   ├── concepts/           # 概念条目（一个概念一个文件）
│   └── summaries/          # 摘要文件
│
└── README.md               # 知识库导航
```

## 工具链

| 组件 | 作用 | 说明 |
|------|------|------|
| **Obsidian** | 知识库容器 | 本地优先，Markdown 原生，双向链接 |
| **Claude Code** | AI 引擎 | 命令行 AI，直接读写文件 |
| **Claudian** | 图形化桥梁 | Obsidian 插件，将 Claude Code 以侧边栏形式接入 |
| **Web Clipper** | 网页剪藏 | 一键保存网页为 Markdown |

### Claudian 插件

将 Claude Code 从命令行带入 Obsidian 图形界面：

| 特性 | 原生 Claude Code | Claudian 插件 |
|------|-----------------|---------------|
| 界面 | 命令行终端 | 图形化侧边栏 |
| 文件引用 | 手动输入路径 | 直接拖拽文件 |
| 上下文管理 | 复杂 | 可视化 |
| 学习曲线 | 陡峭 | 平缓 |

安装：通过 BRAT 插件安装 `obsidian-claudian/obsidian-claudian`。

## 与 Hermes + llm-wiki 的对比

| | Claude Code + Claudian | Hermes Agent + llm-wiki |
|---|---|---|
| AI 引擎 | Claude Code（仅 Claude 模型） | Hermes Agent（200+ 模型） |
| 操作方式 | 手动触发编译 | 自动摄入+整理 |
| 记忆系统 | CLAUDE.md | 四层记忆架构 |
| 多平台 | 仅 CLI/IDE | 16+ 平台 |
| 适用场景 | 个人知识库 | 个人+企业级 |

两者核心理念一致：**LLM 直接读写 Markdown，不依赖向量数据库**。Hermes 在此基础上增加了自动化、多平台和记忆系统。

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | 原始资料（网页文章、播客转录、论文 PDF） |
| **Process** | ① Phase 1 Ingest：收集原始资料存入 raw/ → ② Phase 2 Compile：LLM 自动生成摘要、提取概念、建立索引 → ③ Phase 3 Query：自然语言或搜索问答 → ④ Phase 4 Lint：定期检查和更新知识库 |
| **Output** | 三层目录的知识库（raw/ + wiki/ + README.md），LLM 直接读写 Markdown，不依赖向量数据库 |
| **Tools** | Claude Code / Hermes Agent, Obsidian, Claudian 插件, Web Clipper |
| **Quality Check** | raw/ 是否只进不改？wiki/ 索引是否覆盖所有概念？定期 lint 是否发现死链/过期内容？ |

## 相关

- [[LLM Wiki（Karpathy 模式）]]
- [[第二大脑（Second Brain）]]
- [[Obsidian]]
- [[Hermes Agent]]
- [[Andrej Karpathy]]
