# YAML 反向校验: 知识本体构建 — 基于 LLM Wiki 的大模型知识库
# 日期: 2026-06-05
# 触发: web-pack 补全 + ingest 完成

## 1. 新场景

### 方案文档写作场景 (NEW)
本次文章核心应用场景是"方案类文档写作"（立项报告、采购文件、招标文件、标准规范文档），
现有 scenarios.yaml 中 `write-article-from-kb` 偏通用技术文章，未覆盖方案文档的特殊需求
（合规性检查、结构化模板、多章节长文）。

建议新增:
```yaml
- id: proposal-document-writing
  name: 方案文档写作
  trigger: "需要撰写方案类文档（立项报告/采购文件/招标文件/标准规范）"
  goal: "基于知识库的场景-概念-实体三层模型，组装结构化方案文档"
  composition:
    - phase: 1
      name: 需求解析
      concepts: ["task-decomposition"]
      entities: []
      rule: "解析文档类型、受众、合规要求、篇幅预期"
    - phase: 2
      name: 场景匹配
      concepts: ["scene-matching"]
      entities: ["meta-scenario-layer"]
      rule: "匹配方案文档写作场景，确定章节模板"
    - phase: 3
      name: 概念组装
      concepts: ["knowledge-assembly"]
      entities: ["meta-concept-layer", "meta-entity-layer"]
      rule: "按 composition 规则逐章组装概念和实体"
    - phase: 4
      name: 原文深挖
      concepts: ["source-deep-dive"]
      entities: ["llm-wiki", "wiki-raw-layer"]
      rule: "回溯原始文章获取具体案例、数据、引用"
    - phase: 5
      name: 结构化写作
      concepts: ["structured-writing"]
      entities: ["obsidian"]
      rule: "按方案文档规范写作，确保合规性和完整性"
    - phase: 6
      name: 合规检查
      concepts: ["comparative-analysis"]
      entities: []
      rule: "检查文档是否符合对应类型的格式和内容要求"
    - phase: 7
      name: 反向校验
      concepts: ["yaml-reverse-check"]
      entities: ["meta-meta-layer"]
      rule: "检查是否发现新的概念/实体/关系"
  related_scenarios:
    - write-article-from-kb
    - query-wiki-knowledge
```

## 2. 新概念

### 2.1 知识本体建模 (META)
文章核心方法论，但现有 meta-concepts 中无此概念。建议新增原子概念:
```yaml
- id: knowledge-ontology-modeling
  name: 知识本体建模
  category: knowledge-management
  description: 从历史资料中萃取概念和实体，构建场景驱动的三层知识本体
  input: "历史资料文档集合"
  process:
    - "从文档中萃取核心概念（方法/动作）和实体（人事物/名词）"
    - "为概念建模 IPO 四元组（Input/Process/Output/Tools）"
    - "为实体建立父子层级结构"
    - "构建场景层：定义场景的阶段、概念引用、实体引用、组装流程"
    - "建立概念-实体-场景之间的引用关系"
  output: "三层知识本体模型（场景→概念→实体）"
  tools: ["llm-wiki", "meta"]
  quality_check: "三层是否完整？概念是否有 IPO？实体是否有层级？场景是否有 composition？"
```

### 2.2 知识图谱可视化 (META)
文章大量展示了可视化方案，现有概念中无此条目:
```yaml
- id: knowledge-graph-visualization
  name: 知识图谱可视化
  category: knowledge-management
  description: 将三层知识本体模型可视化为交互式图表
  input: "三层知识本体模型（场景/概念/实体 + 关系）"
  process:
    - "选择可视化类型（分层网络图/场景支撑图/泳道图/力导向图）"
    - "提取节点（场景/概念/实体）和边（组装/引用/父子关系）"
    - "生成可视化图表"
    - "支持交互式浏览（点击展开/筛选/搜索）"
  output: "交互式知识图谱可视化"
  tools: ["ai-programming", "d3js", "mermaid"]
  quality_check: "三层结构是否清晰可见？交互是否流畅？"
```

## 3. 新实体

### 3.1 web-pack (NEW)
本次使用的 web-pack skill 在 entity yaml 中未注册:
```yaml
- id: web-pack
  name: Web Pack
  level: 1
  type: tool
  description: "网页素材包采集工具，将同主题链接整理为 raw/ + meta/supplements/"
  source: hermes-skill:research/web-pack
  relations:
    - target: llm-wiki
      type: uses
    - target: meta-meta-layer
      type: related_to
```

### 3.2 knowledge-ontology-three-layer-model (NEW)
新创建的 wiki 概念页:
```yaml
- id: knowledge-ontology-three-layer-model
  name: 知识本体三层模型
  level: 1
  type: methodology
  description: "基于 LLM Wiki 的知识本体建模方法，场景→概念→实体三层结构"
  source: wiki/concepts/knowledge-ontology-three-layer-model.md
  relations:
    - target: scenario-driven-cognitive-loop
      type: related_to
    - target: cognitive-closed-loop
      type: related_to
    - target: llm-wiki
      type: is_a
```

## 4. 关系补全

建议补充以下关系:

```yaml
# 在 knowledge-ontology-three-layer-model 实体中
relations:
  - target: scenario-driven-cognitive-loop
    type: related_to
    reason: "三层模型是认知闭环的理论基础"
  - target: atom-compose-concept-architecture
    type: uses
    reason: "概念层采用原子-组合双层架构"
  - target: ipo-closed-loop
    type: uses
    reason: "原子概念采用 IPO 建模"
  - target: yaml-reverse-validation
    type: uses
    reason: "自我进化依赖 YAML 反向校验"

# 在 web-pack 实体中
relations:
  - target: meta-supplements-layer
    type: produces
    reason: "web-pack 产出 meta/supplements/ 元数据"
```

## 5. 死链

### 5.1 wiki 页面 source 引用过期
`concepts/knowledge-ontology-three-layer-model.md` 的 sources 字段引用:
- `raw/articles/knowledge-ontology-llm-wiki-2026.md` → 应更新为 `raw/2026-06-05-knowledge-ontology/knowledge-ontology-llm-wiki.md`

### 5.2 无其他死链
- 所有 [[wikilinks]] 指向的页面均存在
- meta/ yaml 中的 entity/concept 引用均有效

## 总结

| 维度 | 发现 | 操作 |
|------|------|------|
| 新场景 | 1 (proposal-document-writing) | 建议新增 |
| 新概念 | 2 (knowledge-ontology-modeling, knowledge-graph-visualization) | 建议新增为 META |
| 新实体 | 2 (web-pack, knowledge-ontology-three-layer-model) | 建议新增到 knowledge-management.yaml |
| 关系补全 | 4 条 | 建议补充 |
| 死链 | 1 (source 引用过期) | 需修复 |
