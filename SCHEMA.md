# Wiki Schema

## Domain
综合性知识库 — 技术、研究、个人笔记，无所不包。

## 双层架构

本仓库包含两层知识体系：

- **wiki/** — 资料库 + 知识库：萃取后的结构化文章（Markdown），存储知识内容本身
- **wiki1/** — 模式库（认知闭环操作系统）：元信息（YAML），存储结构、关系、组装规则
  - 三层架构：场景层 → 概念层（原子+组合） → 实体层
  - IPO 闭环：Input → Process → Output
  - 4 种实体关系：is_a / uses / depends_on / related_to
  - 每次使用后强制执行 yaml 反向校验

wiki1/ 通过 `source:` 字段反向指回 wiki/ 中的原始页面。详见 `wiki1/README.md` 和 `wiki1/profile.md`。

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
- **Tech:** programming, devops, architecture, security, database, cloud
- **AI/ML:** model, benchmark, training, inference, alignment, llm
- **People/Orgs:** person, company, lab, open-source
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

## Comparison Pages
Side-by-side analyses. Include:
- What is being compared and why
- Dimensions of comparison (table format preferred)
- Verdict or synthesis
- Sources

## Update Policy
When new information conflicts with existing content:
1. Check the dates — newer sources generally supersede older ones
2. If genuinely contradictory, note both positions with dates and sources
3. Mark the contradiction in frontmatter: `contradictions: [page-name]`
4. Flag for user review in the lint report
