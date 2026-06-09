# Wiki Schema

## Domain
综合性知识库 — 技术、研究、个人笔记，无所不包。

## 双层架构

本仓库包含两层知识体系，统一在 wiki/ 目录下：

- **wiki/**（顶层）— 知识库：萃取后的结构化文章（Markdown），存储知识内容本身
- **wiki/meta/** — 模式库（认知闭环操作系统）：元信息（YAML），存储结构、关系、组装规则
  - 三层架构：场景层 → 概念层（原子+组合） → 实体层
  - IPO 闭环：Input → Process → Output
  - 4 种实体关系：is_a / uses / depends_on / related_to
  - 每次使用后强制执行 yaml 反向校验

meta/ 通过 `source:` 字段反向指回 wiki/ 中的原始页面。详见 `meta/README.md` 和 `meta/profile.md`。

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `transformer-architecture.md`)
- Every wiki page starts with YAML frontmatter (see below)
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`
- **Provenance markers:** On pages that synthesize 3+ sources, append `^[raw/articles/source-file.md]`
  at the end of paragraphs whose claims come from a specific source.

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
confidence: high | medium | low
contested: true
contradictions: [other-page-slug]
---
```

## Tag Taxonomy
- **Tech:** programming, devops, architecture, security, database, cloud, serverless, markdown, automation, tool, platform, sync, plugin, query
- **AI/ML:** model, benchmark, training, inference, alignment, llm, ai, agent, memory, skill, self-improving, multi-tenant, agentskills
- **People/Orgs:** person, company, lab, open-source
- **Knowledge:** knowledge-base, knowledge-management, methodology, ontology, metadata, second-brain, enterprise, content-creation, business-process
- **Domain:** retail, inventory, erp, fashion, podcast, philosophy
- **Meta:** comparison, timeline, tutorial, reference, opinion

Rule: every tag on a page must appear in this taxonomy. Add new tags here BEFORE using them.

## Page Thresholds
- **Create a page** when an entity/concept appears in 2+ sources OR is central to one source
- **Add to existing page** when a source mentions something already covered
- **DON'T create a page** for passing mentions, minor details, or things outside the domain
- **Split a page** when it exceeds ~200 lines — break into sub-topics with cross-links
- **Archive a page** when its content is fully superseded — move to `_archive/`, remove from index

## Entity Pages
One page per notable entity. Include:
- Overview / what it is
- Key facts and dates
- Relationships to other entities ([[wikilinks]])
- Source references

## Concept Pages
One page per concept or topic. Include:
- Definition / explanation
- Current state of knowledge
- Open questions or debates
- Related concepts ([[wikilinks]])
- **IPO 建模** (required for core concepts): Input / Process / Output / Tools / Quality Check table, following the meta/meta-concepts.yaml IPO format. This transforms the page from "descriptive knowledge" to "executable specification" that AI can assemble into solutions.

## Comparison Pages
Side-by-side analyses. Include:
- What is being compared and why
- Dimensions of comparison (table format preferred)
- Verdict or synthesis
- Sources

## raw/ 目录的两种模式

raw/ 支持两种素材组织模式共存：

### 单篇文章模式（raw/articles/）
扁平结构，每篇独立文章一个 .md 文件。适用于零散摄入。

### 多源研究包模式（raw/<date>-<topic>/）
web-pack 采集产出的研究包目录。适用于围绕同一主题的多源深度研究。

```
raw/
├── articles/                           # 单篇文章（扁平）
│   └── hermes-llm-wiki-skill-2026.md
└── 2026-06-08-transformer/             # 多源研究包（目录）
    ├── attention-is-all-you-need.md    # 语义化文件名
    ├── flash-attention-blog.md
    └── assets/                         # 本地化图片
```

每个 .md 文件带 llm-wiki 标准 raw frontmatter（source_url/ingested/sha256）。

## meta/supplements/ 目录

`meta/supplements/` 存放 web-pack 采集时生成的元数据文件，作为 ingest 的辅助参考：

```
meta/supplements/
└── YYYY-MM-DD-主题名/
    ├── research-brief.md     # 采集概览（入口URL、页面数、失败项）
    ├── link-inventory.md     # URL → 文件映射表
    ├── image-inventory.md    # 图片下载状态清单
    └── reading-map.md        # 阅读地图（入口→关联页面关系）
```

关键规则：

- supplements/ 中的文件是**元数据**，不是知识来源。ingest 时可以参考 reading-map.md 理解素材关联，但提取知识的来源必须是 raw/ 中的原始文件
- supplements/ 文件不参与 wikilink 交叉引用，不列入 index.md
- 与 raw/ 不同，supplements/ 不是 immutable——可以在 ingest 完成后删除或归档
- 保持 LLM Wiki 语义纯净：raw/ 只有纯原始正文 + 本地化图片
- supplements/ 放在 meta/ 下，与 meta-concepts.yaml、scenarios/ 等元模型文件属于同一层——都是"关于知识的知识"

## web-pack → llm-wiki 工作流

web-pack 采集与 llm-wiki ingest 的衔接流程：

```
1. web-pack 采集 → raw/<date>-<topic>/（纯正文）+ meta/supplements/<date>-<topic>/（元数据）
2. llm-wiki ingest 读取 raw/ 中的原始文件，参考 supplements/ 中的阅读地图理解关联
3. 场景驱动认知闭环介入：按 scenarios.yaml 中的 composition 规则组装知识
4. 创建/更新 wiki 页面，建立交叉引用
5. YAML 反向校验：检查是否有新概念/实体/关系需要补充到 meta/
6. 更新 index.md 和 log.md，git 提交
```

raw frontmatter 中的 sha256 让后续 re-ingest 可以跳过未变化的源文件。

## Update Policy
When new information conflicts with existing content:
1. Check the dates — newer sources generally supersede older ones
2. If genuinely contradictory, note both positions with dates and sources
3. Mark the contradiction in frontmatter: `contradictions: [page-name]`
4. Flag for user review in the lint report
