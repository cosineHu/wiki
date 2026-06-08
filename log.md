# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-06-08] ingest | 认知闭环操作系统 — wiki1/ 初始化
- 来源: https://mp.weixin.qq.com/s/ZMZHQVbtRFbQ22DgvY8YCw
- 创建文件:
  - raw/articles/cognitive-closed-loop-wiki1-2026.md (原始来源)
  - concepts/cognitive-closed-loop.md (认知闭环操作系统概念)
  - comparisons/wiki-vs-wiki1.md (wiki/ vs wiki1/ 对比)
  - wiki1/README.md (wiki1 架构说明)
  - wiki1/profile.md (AI 操作规程，7 步流水线 + yaml 反向校验)
  - wiki1/meta-concepts.yaml (35 个原子概念，IPO 建模)
  - wiki1/compose-concepts.yaml (20 个组合概念，decomposition 分解)
  - wiki1/entities/knowledge-management.yaml (20 个知识管理实体)
  - wiki1/entities/ai-agent.yaml (18 个 AI Agent 实体)
  - wiki1/entities/thinking-methods.yaml (12 个思维方法实体)
  - wiki1/entities/people.yaml (8 个人物实体)
  - wiki1/entities/infrastructure.yaml (12 个基础设施实体)
  - wiki1/scenarios/scenarios.yaml (20 个场景，完整 composition 组装规则)
  - wiki1/CHANGELOG.md (变更日志)
  - wiki1/_pending/ (待审核的 yaml 骨架目录)
- 更新文件:
  - SCHEMA.md (新增双层架构说明)
  - index.md (新增 2 个页面条目，总计 34 页)
- 核心设计:
  - 三层架构: 场景层 → 概念层（原子+组合） → 实体层
  - IPO 闭环贯穿三层
  - 4 种实体关系: is_a / uses / depends_on / related_to
  - 每次使用后强制执行 yaml 反向校验（5 个维度）
  - 7 步标准流水线: 问题解析→场景匹配→概念组装→实体调取→原文深挖→结构化执行→反向校验

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

## [2026-06-06] lint | 221 个问题发现
- 严重 (184): 184 个断链（191 个 wikilinks 中仅 7 个有效）— 所有页面使用中文别名（如 `[[Hermes Agent]]`、`[[第二大脑（Second Brain）]]`），但实际页面 slug 为英文（`hermes-agent`、`second-brain`），导致 96% 断链。与 6/5 lint 同一根因，未变化。
- 警告 (25): 25 个孤立页面（全部页面入链为 0，因断链导致）+ 0 个未分类标签（所有标签已在 6/5 lint 中报告，无新增）
- 提示 (12): 12 个 raw/ 源文件 sha256 漂移（全部 12 个 raw 文件，与上次 lint 相同的已知问题）
- 正常: frontmatter 完整（27/27）、索引无遗漏/僵尸（27/27 匹配）、无过期内容（全部 updated 在 6 月）、无矛盾标记、无低置信度页面、无超大页面（最大 158 行）、日志无需轮转（17 条）
- 根因分析: 184 个断链是同一根因 — 页面 slug 使用英文命名（如 `hermes-agent`），但所有 wikilinks 使用中文 title（如 `[[Hermes Agent]]`）。修复方案：二选一 — (A) 将所有 wikilinks 改为英文 slug 格式，或 (B) 将所有页面文件重命名为中文 slug。推荐方案 A（保持 slug 英文，批量替换 wikilinks）。

## [2026-06-07] lint | 199 个问题发现
- 严重 (159): 159 个断链 — 与 6/5、6/6 同一根因，所有 wikilinks 使用中文别名（如 `[[Hermes Agent]]`、`[[第二大脑（Second Brain）]]`），但实际页面 slug 为英文（`hermes-agent`、`second-brain`）。27 个页面中仅 6 个 wikilinks 有效（`obsidian`、`obsidian-headless`、`dify`、`n8n`、`ollama`、`openclaw` 等直接匹配英文 slug 的链接）。索引无遗漏、无僵尸条目。
- 警告 (40): 21 个孤立页面（因断链导致所有页面入链为 0）+ 19 个未分类标签（agent, agentskills, ai, automation, enterprise, knowledge-base, markdown, memory, metadata, multi-tenant, platform, plugin, query, second-brain, self-improving, serverless, skill, sync, tool — 与 6/5 相同，未修复）
- 提示 (0): 无过期内容、无矛盾标记、无低置信度页面、无超大页面
- 正常: frontmatter 完整（27/27）、索引无遗漏/僵尸（27/27 匹配）、日志无需轮转（18 条）
- 趋势: 断链数从 184→184→159（减少 25 个，因部分页面出链去重后计数变化），根因未变。已连续 3 天报告同一问题，建议尽快执行方案 A 批量修复。

## [2026-06-08] ingest | 场景驱动认知闭环 — LLM-Wiki 从知识萃取到认知操作系统
- 来源: https://blog.csdn.net/m0_59235945/article/details/161753234
- 核心主题: 将 LLM-Wiki 从静态知识库升级为场景驱动的认知闭环操作系统
- 创建文件:
  - raw/articles/scenario-driven-cognitive-loop-2026.md (原始来源)
  - concepts/scenario-driven-cognitive-loop.md (场景驱动认知闭环：三层架构 + IPO + 自我迭代)
  - concepts/atom-compose-concept-architecture.md (原子-组合概念双层架构)
  - concepts/ipo-closed-loop.md (IPO 闭环：输入→处理→输出→工具)
  - concepts/yaml-reverse-validation.md (YAML 反向校验：知识库自我迭代机制)
  - comparisons/static-kb-vs-cognitive-os.md (静态知识库 vs 认知操作系统)
- 更新文件: index.md (新增 5 个页面条目，总页数 27→32)
