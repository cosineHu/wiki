---
title: Karpathy 式知识库方法（Karpathy Knowledge Base Method）
created: 2026-06-04
updated: 2026-06-08
type: concept
tags: [knowledge-base, llm, markdown, tutorial]
sources: [raw/articles/karpathy-knowledge-base-guide-2026.md, raw/articles/knowledge-management-llm-skills-2026.md]
confidence: high
---

# Karpathy 式知识库方法

Andrej Karpathy 提出的 AI 知识库方法论：**不依赖向量数据库，不搭建 RAG 架构，直接让 LLM 读写 Markdown 文件**。

> 知识管理最耗人的部分，从来不是「读」和「想」，而是整理。让 AI 代替你做这些。

## 核心理念

对于个人知识库规模（约 100 篇文章、40 万词），直接让 LLM 读取它自己维护的摘要文件，效果不比向量检索差，但简单得多。

## 四阶段流程

Karpathy 的完整系统可拆为五大模块（老章归纳）：

### 1. 数据导入（Data Import）
把各种原始素材——论文、文章、代码库、数据集、图片——统统丢进 raw/ 目录。用 Obsidian Web Clipper 浏览器插件一键裁剪网页文章为 .md 文件。核心思路：先不管三七二十一，把所有原始材料攒在一起。

### 2. 知识编译（Wiki Compilation）
整个系统最核心的一步——让 LLM 把 raw/ 目录里的散碎材料「编译」成结构化维基。为每份原始数据生成摘要、建立反向链接、将数据分类到不同概念、为每个概念撰写专题文章、将所有文章互相链接。

Karpathy 用的是编程的隐喻：原始数据是「源码」，维基是「编译产物」，LLM 就是那个「编译器」。

### 3. 问答驱动（Q&A）
当维基长到足够大（约 100 篇文章、40 万字），就可以向 LLM Agent 提各种复杂问题。关键发现：本以为需要复杂的 RAG，但实际上 LLM 自己维护的索引文件和摘要就够用了。40 万字对长上下文模型（Gemini 百万 token、Claude 200K）来说绰绰有余。

### 4. 输出呈现（Output）
Karpathy 不喜欢在终端里看答案。他更倾向于让 LLM 生成 Markdown 文件、Marp 格式幻灯片、matplotlib 图表，然后在 Obsidian 里直接查看。更聪明的是：把输出「归档」回维基，每次探索和查询都会「累积」在知识库里。

### 5. 健康检查（Health Check）
定期让 LLM 对维基做「体检」：查找数据不一致、用网络搜索填充缺失信息、发现有趣的关联、建议新的文章候选、提出下一步要调查的问题。LLM 在「建议进一步的问题」上做得相当好——这就是 Agent 自主探索。

## 四阶段流程（简化版）

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

## 三人观点对比

老章将 Karpathy、Lex Fridman、kepano 三人的知识管理哲学做了系统对比：

| 维度 | Karpathy | Lex Fridman | kepano |
|------|---------|------------|--------|
| 核心身份 | AI 研究者 / 教育者 | 播客主持人 / 研究者 | Obsidian CEO / 产品哲学家 |
| 知识库用途 | 研究探索 | 多领域播客准备 | 个人思维记录 |
| 对 LLM 的态度 | 全面拥抱，让 LLM 管理一切 | 拥抱但重视多模态输出 | 审慎，警惕过度依赖 |
| Obsidian 角色 | 只是「前端渲染器」 | 混合前端之一 | 思维的延伸 |
| 核心哲学 | 「编译知识」 | 「知识要可交互」 | 「文件优先于应用」 |
| 最大贡献 | 完整方法论框架 | 语音交互式学习 | 人机边界思考 |

## 终局愿景：从上下文到权重

Karpathy 最有远见的一句话：

> 「随着仓库的增长，自然地也想考虑合成数据生成 + 微调，让你的 LLM '知道'数据在其权重中，而不仅仅是上下文窗口。」

现在所有的知识管理方案本质上都在「上下文工程」范畴内打转——把知识塞进上下文窗口让 LLM 读取。但终局是：用自己积累的知识库微调专属小模型，让它从骨子里「理解」你的领域知识和思考方式。那就不是「你问它答」了，是训练了一个数字分身。

## 相关

- [[llm-wiki]]
- [[second-brain]]
- [[obsidian]]
- [[hermes-agent]]
- [[andrej-karpathy]]
- [[lex-fridman]]
- [[kepano]]
- [[knowledge-management-pipeline]]
- [[content-production-pipeline]]
- [[ai-human-knowledge-boundary]]
