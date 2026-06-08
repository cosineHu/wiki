# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-06-08] ingest | 认知闭环操作系统 — meta/ 初始化
- 来源: https://mp.weixin.qq.com/s/ZMZHQVbtRFbQ22DgvY8YCw
- 创建文件:
  - raw/articles/cognitive-closed-loop-wiki1-2026.md (原始来源)
  - concepts/cognitive-closed-loop.md (认知闭环操作系统概念)
  - comparisons/wiki-vs-wiki1.md (wiki/ vs meta/ 对比)
  - meta/README.md (meta 架构说明)
  - meta/profile.md (AI 操作规程，7 步流水线 + yaml 反向校验)
  - meta/meta-concepts.yaml (35 个原子概念，IPO 建模)
  - meta/compose-concepts.yaml (20 个组合概念，decomposition 分解)
  - meta/entities/knowledge-management.yaml (20 个知识管理实体)
  - meta/entities/ai-agent.yaml (18 个 AI Agent 实体)
  - meta/entities/thinking-methods.yaml (12 个思维方法实体)
  - meta/entities/people.yaml (8 个人物实体)
  - meta/entities/infrastructure.yaml (12 个基础设施实体)
  - meta/scenarios/scenarios.yaml (20 个场景，完整 composition 组装规则)
  - meta/CHANGELOG.md (变更日志)
  - meta/_pending/ (待审核的 yaml 骨架目录)
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

## [2026-06-08] evolve | 三步落地：IPO 建模 + 场景层 + 反向校验接入 cron
- 核心动作: 将 meta/ 认知闭环系统正式接入 wiki 运营体系
- 第一步 — 概念页 IPO 建模:
  - concepts/hermes-skills-system.md: 新增 IPO 段（Input/Process/Output/Tools/Quality Check）
  - concepts/llm-wiki.md: 新增 IPO 段
  - concepts/scenario-driven-cognitive-loop.md: 新增 IPO 段
  - concepts/atom-compose-concept-architecture.md: 新增 IPO 段
  - concepts/yaml-reverse-validation.md: 新增 IPO 段
  - concepts/ipo-closed-loop.md: 新增 IPO 段
- 第二步 — 场景层已就位: meta/scenarios/scenarios.yaml（20 个场景，含 composition 组装规则）
- 第三步 — cron 任务升级:
  - 每日知识审计 (7e7b8f34c165): 增加 meta/ 层检查（yaml 一致性、交叉引用、死链）
  - 每周知识再进化 (431e61cc81b5): 增加场景覆盖度评估、基于 meta/ 的缺口发现
  - 每日反向校验 (962848e46455): 新增 cron，执行 5 维 yaml 反向校验（新场景/概念/实体/关系/死链）
- 修复: concepts/cognitive-closed-loop.md 中 3 处 wiki1/ 引用 → wiki/meta/
- 更新: SCHEMA.md 新增概念页 IPO 建模要求
- 删除: wiki1/ 目录（内容已移入 wiki/meta/）

## [2026-06-08] update | 三个 cron 任务投递方式改为 local
- 每日知识审计 (7e7b8f34c165): deliver=origin → local
- 每周知识再进化 (431e61cc81b5): deliver=origin → local
- 每日反向校验 (962848e46455): deliver=origin → local
- 原因: origin 模式下飞书投递目标无法解析，改为本地记录

## [2026-06-08] update | 全部 21 个概念页补充 IPO 建模段
- 新增 IPO 段的概念页（15个）:
  second-brain, enterprise-second-brain-architecture, hermes-memory-architecture,
  layered-memory-system, karpathy-knowledge-base-method, skills-authoring-guide,
  cognitive-closed-loop, memory-agent-vs-workflow-agent, hermes-terminal-backends,
  hermes-knowledge-reference, hermes-session-isolation, obsidian-headless,
  obsidian-dataview, obsidian-tag-system, obsidian-bidirectional-links
- 此前已有 IPO 段的概念页（6个）:
  hermes-skills-system, llm-wiki, scenario-driven-cognitive-loop,
  atom-compose-concept-architecture, yaml-reverse-validation, ipo-closed-loop
- IPO 覆盖率: 21/21 (100%)
- 每个 IPO 段包含: Input / Process / Output / Tools / Quality Check 五列

## [2026-06-08] update | web-pack v2.0 — 拆分输出到 raw/ + supplements/
- 改造 collect_web_pack.py：输出从单一目录拆分为 raw/（纯原始素材）+ supplements/（元数据参考）
- raw/ 只存正文 markdown + 本地化图片，保持 LLM Wiki 语义纯净
- supplements/ 存 research-brief.md、link-inventory.md、image-inventory.md、reading-map.md
- 文件名改用文章标题（不再使用 MAIN/LINKED 前缀）
- --out-root 改为 --base-dir，自动检测 $WIKI_PATH 或 ~/wiki
- 更新 SKILL.md 文档，版本号升至 2.0.0
- 更新 SCHEMA.md 增加 supplements/ 目录说明和关键规则
- 测试通过：CSDN 文章成功采集，raw/ 1 个 md + supplements/ 4 个元数据文件

## [2026-06-08] lint | 每日知识审计
- 审计范围: wiki/ 知识层 + wiki/meta/ 元信息层 + 交叉一致性
- 发现问题: 266 处（严重 31 / 警告 70 / 建议 165）
- 严重: 死链 23 处（wikilinks 占位符）+ 场景死链 8 处
- 警告: 标签不合规 64 处 + 孤立页面 2 个 + 索引缺失 1 个 + IPO 不完整 3 处
- 建议: meta 实体→wiki 缺失 86 处 + meta 概念→wiki 缺失 53 处 + wiki→meta 缺失 21 处
- 正面: 超大页面 0 / 低置信度 0 / 争议页面 0 / 所有页面有 confidence / 组合概念全合规
- 自动修复: 将 wiki-vs-wiki1 补入 index.md（Comparisons 部分），更新总页数为 36
- 审计报告: meta/_pending/audit-20260608.md

## [2026-06-08] ingest | 基于大模型、Skills 的知识管理 — 三位巨佬的知识管理哲学
- 来源: https://mp.weixin.qq.com/s/iRVOlhGZlirVRIilYiR8vg
- 作者: 老章（公众号「老章」）
- 核心主题: Karpathy/Lex Fridman/kepano 三人知识管理哲学对比 + 内容生产流水线
- 创建文件:
  - raw/articles/knowledge-management-llm-skills-2026.md (原始来源)
  - entities/lex-fridman.md (Lex Fridman 实体页)
  - entities/kepano.md (kepano/Steph Ango 实体页)
  - concepts/knowledge-management-pipeline.md (知识管理全链路：五环闭环模型)
  - concepts/content-production-pipeline.md (内容生产流水线：下游生产)
  - concepts/ai-human-knowledge-boundary.md (AI 与人知识边界：物理隔离 vs 溯源标记)
- 更新文件:
  - concepts/karpathy-knowledge-base-method.md (补充五大模块详解 + 三人对比表 + 终局愿景)
  - entities/obsidian.md (补充 kepano 创始哲学 + 交叉引用)
  - entities/andrej-karpathy.md (大幅扩充：五大模块 + 终局愿景 + 全链路定位)
  - concepts/second-brain.md (新增全链路/生产流水线/边界交叉引用)
  - index.md (新增 5 个页面条目，总页数 36→41)
