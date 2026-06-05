# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-06-02] create | Wiki initialized
- Domain: General knowledge base (technology, research, personal notes)
- Structure created with SCHEMA.md, index.md, log.md
- Directory layout: raw/, entities/, concepts/, comparisons/, queries/, _archive/
## [2026-06-02] config | post-commit hook enabled

## [2026-06-02] ingest | Hermes Agent LLM Wiki Skill 文档
- 来源: https://hermes-agent.nousresearch.com/docs/zh-Hans/user-guide/skills/bundled/research/research-llm-wiki
- 创建文件:
  - raw/articles/hermes-llm-wiki-skill-2026.md (原始来源)
  - concepts/llm-wiki.md (LLM Wiki 概念)
  - entities/andrej-karpathy.md (Andrej Karpathy)
  - entities/hermes-agent.md (Hermes Agent)
  - concepts/obsidian-headless.md (obsidian-headless)
  - comparisons/rag-vs-wiki.md (RAG vs Wiki 对比)
- 更新文件:
  - index.md (新增 5 个页面条目)

## [2026-06-02] ingest | Hermes Agent + Obsidian 打造第二大脑系列
- 来源: https://blog.csdn.net/sgr011215/article/details/160530313
- 创建文件:
  - raw/articles/hermes-obsidian-second-brain-2026.md (原始来源)
  - concepts/second-brain.md (第二大脑概念)
  - concepts/layered-memory-system.md (分层记忆系统)
  - entities/dify.md (Dify)
  - entities/n8n.md (n8n)
  - entities/ollama.md (Ollama)
- 更新文件:
  - entities/hermes-agent.md (补充第二大脑集成交叉引用)
  - index.md (新增 5 个页面条目)

## [2026-06-02] ingest | Hermes Agent + Obsidian 打造第二大脑（一）：为什么需要第二大脑？
- 来源: https://blog.csdn.net/sgr011215/article/details/160530542
- 创建文件:
  - raw/articles/hermes-second-brain-part1-2026.md (原始来源)
- 更新文件:
  - concepts/second-brain.md (补充三个核心特征、四大痛点表格)
  - concepts/layered-memory-system.md (补充来源引用)
  - entities/hermes-agent.md (补充来源引用)

## [2026-06-03] ingest | Hermes Agent + Obsidian 打造第二大脑（二）：我的踩坑经验与完整部署方案
- 来源: https://blog.csdn.net/sgr011215/article/details/160530638
- 创建文件:
  - raw/articles/hermes-second-brain-part2-2026.md (原始来源)
- 更新文件:
  - concepts/second-brain.md (补充部署踩坑要点表格、技术架构层次)
  - concepts/layered-memory-system.md (补充来源引用)
  - entities/hermes-agent.md (补充部署架构、来源引用)

## [2026-06-03] ingest | Hermes Agent + Obsidian 打造第二大脑（四）：Obsidian 核心操作与踩坑
- 来源: https://blog.csdn.net/sgr011215/article/details/160530681
- 创建文件:
  - raw/articles/hermes-obsidian-core-ops-2026.md (原始来源)
  - entities/obsidian.md (Obsidian 实体页)
  - concepts/obsidian-bidirectional-links.md (双向链接)
  - concepts/obsidian-tag-system.md (标签系统与 Frontmatter)
  - concepts/obsidian-dataview.md (Dataview 查询)
- 更新文件:
  - concepts/second-brain.md (新增 Obsidian 交叉引用)
  - entities/hermes-agent.md (新增 Obsidian 交叉引用)
  - concepts/obsidian-headless.md (新增 Obsidian 交叉引用)
  - index.md (新增 4 个页面条目，总页数 14)

## [2026-06-03] ingest | OpenClaw vs Hermes：AI Agent 未来之争
- 来源: https://www.drpang.ai/openclaw-vs-hermes-ai-agent-future/
- 创建文件:
  - raw/articles/openclaw-vs-hermes-2026.md (原始来源)
  - entities/openclaw.md (OpenClaw 实体页)
  - comparisons/openclaw-vs-hermes.md (OpenClaw vs Hermes 对比)
- 更新文件:
  - entities/hermes-agent.md (新增 OpenClaw 交叉引用)
  - index.md (新增 2 个页面条目，总页数 16)

## [2026-06-03] ingest | Hermes vs OpenClaw — 核心差异、知识库参考、会话隔离、第二大脑
- 来源: 用户上传文档 (Hermes vs OpenClaw.md)
- 创建文件:
  - raw/articles/hermes-vs-openclaw-features-2026.md (原始来源)
  - concepts/hermes-knowledge-reference.md (知识库参考方式)
  - concepts/hermes-session-isolation.md (会话隔离机制)
- 更新文件:
  - comparisons/openclaw-vs-hermes.md (补充 Skill/Gateway 核心能力差异表)
  - concepts/second-brain.md (补充 Tiago Forte 起源)
  - entities/hermes-agent.md (新增交叉引用)
  - index.md (新增 2 个页面条目，总页数 18)

## [2026-06-03] ingest | Hermes Agent 深度解析：会自我进化的开源 AI Agent
- 来源: https://ruizhehou.github.io/2026/05/01/Hermes-Agent%E8%A7%A3%E6%9E%90/
- 创建文件:
  - raw/articles/hermes-agent-deep-dive-2026.md (原始来源)
  - concepts/hermes-skills-system.md (技能系统详解)
  - concepts/hermes-terminal-backends.md (六种终端后端)
  - comparisons/hermes-vs-other-agents.md (vs AutoGPT/CrewAI/Claude Code)
- 更新文件:
  - entities/hermes-agent.md (补充核心特性、RL 训练、对比引用)
  - index.md (新增 3 个页面条目，总页数 21)

## [2026-06-04] ingest | Hermes Agent 的记忆系统：为什么它修正了 OpenClaw 的错误
- 来源: https://cloud.tencent.com.cn/developer/article/2668217
- 原文: Manthan Gupta（@manthanguptaa）
- 创建文件:
  - raw/articles/hermes-memory-system-2026.md (原始来源)
  - concepts/hermes-memory-architecture.md (四层记忆架构)
- 更新文件:
  - comparisons/openclaw-vs-hermes.md (补充记忆架构差异表)
  - entities/hermes-agent.md (新增交叉引用)
  - index.md (新增 1 个页面条目，总页数 22)

## [2026-06-04] ingest | 基于 Hermes Agent + Obsidian 的企业级第二大脑知识库体系研究
- 来源: 用户上传文档
- 创建文件:
  - raw/articles/enterprise-second-brain-2026.md (原始来源)
  - concepts/enterprise-second-brain-architecture.md (企业级架构)
  - concepts/memory-agent-vs-workflow-agent.md (Memory Agent vs Workflow Agent)
- 更新文件:
  - concepts/hermes-memory-architecture.md (补充工作/情景/语义/技能四层视角)
  - concepts/hermes-skills-system.md (补充 Reflection + Skill Evolution 机制)
  - concepts/second-brain.md (新增企业架构交叉引用)
  - comparisons/openclaw-vs-hermes.md (补充企业第二大脑场景适配表)
  - index.md (新增 2 个页面条目，总页数 24)

## [2026-06-04] ingest | Skills 从 0 到 1 怎么写：AI Agent Skills 完整创建教程（2026）
- 来源: https://www.cnblogs.com/qiniushanghai/p/20027864
- 创建文件:
  - raw/articles/skills-creation-tutorial-2026.md (原始来源)
  - concepts/skills-authoring-guide.md (Skills 编写指南)
- 更新文件:
  - concepts/hermes-skills-system.md (新增交叉引用)
  - index.md (新增 1 个页面条目，总页数 25)

## [2026-06-04] ingest | Karpathy 式 AI 知识库搭建指南：让 Claude Code + Obsidian 成为你的第二大脑
- 来源: https://segmentfault.com/a/1190000047707371
- 创建文件:
  - raw/articles/karpathy-knowledge-base-guide-2026.md (原始来源)
  - concepts/karpathy-knowledge-base-method.md (Karpathy 式知识库方法)
- 更新文件:
  - entities/hermes-agent.md (新增交叉引用)
  - index.md (新增 1 个页面条目，总页数 26)

## [2026-06-05] lint | 155 个问题发现
- 严重 (109): 109 个断链（wikilinks 使用了中文别名/占位符，未匹配实际页面 slug）
- 警告 (35): 16 个孤立页面（入链为 0，因断链导致）+ 19 个未分类标签
- 提示 (11): 11 个 raw/ 源文件 sha256 漂移（内容曾被修改或重新抓取）
- 正常: frontmatter 完整、索引无遗漏/僵尸、无过期内容、无超大页面、日志无需轮转

## [2026-06-05] evolve | 每周知识再进化
- 置信度提升 (8): second-brain (medium→high, 4 sources), layered-memory-system (medium→high, 3 sources), openclaw-vs-hermes (medium→high, 2 sources+跨页佐证), hermes-vs-other-agents (medium→high, 跨页佐证), rag-vs-wiki (medium→high, 跨页佐证), enterprise-second-brain-architecture (medium→high, 跨页佐证), openclaw (medium→high, 跨页佐证), obsidian-headless (medium→high, 跨页佐证)
- 跨页面合成: 发现三种「四层记忆」模型（分层记忆系统/ Hermes记忆架构/ 认知心理学模型），创建 comparisons/three-memory-models.md 进行映射对比
- 交叉引用增强: layered-memory-system ↔ hermes-memory-architecture ↔ memory-agent-vs-workflow-agent 三向链接完成
- 矛盾检测: 无新矛盾发现（页面内容互补无冲突）
- 知识缺口: 109 个断链（中文别名问题）已在 6/5 lint 中报告，待人工处理
- 创建文件: comparisons/three-memory-models.md
- 更新文件: concepts/second-brain.md, concepts/layered-memory-system.md, concepts/hermes-memory-architecture.md, concepts/memory-agent-vs-workflow-agent.md, concepts/enterprise-second-brain-architecture.md, concepts/obsidian-headless.md, comparisons/openclaw-vs-hermes.md, comparisons/hermes-vs-other-agents.md, comparisons/rag-vs-wiki.md, entities/openclaw.md, index.md

## [2026-06-05] lint | 229 个问题发现
- 严重 (183): 183 个断链 — 所有 wikilinks 使用中文别名（如 `[[Hermes Agent]]`、`[[第二大脑（Second Brain）]]`），但实际页面 slug 为英文（`hermes-agent`、`second-brain`），导致全部断链。这是同一根因，非 183 个独立问题。
- 警告 (45): 25 个孤立页面（因断链导致入链为 0）+ 20 个未分类标签（agent, agentskills, ai, automation, enterprise, knowledge-base, markdown, memory, metadata, multi-tenant, obsidian, platform, plugin, query, second-brain, self-improving, serverless, skill, sync, tool）
- 提示 (11): 11 个 raw/ 源文件 sha256 漂移（与上次 lint 相同的 11 个文件）
- 正常: frontmatter 完整、索引无遗漏/僵尸、无过期内容、无矛盾标记、无低置信度页面、无超大页面、日志无需轮转（16 条）
