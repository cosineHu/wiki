# 每日反向校验报告 — 2026-06-09

> 自动生成于 2026-06-09 20:04 UTC
> 5 维检查：场景 / 概念 / 实体 / 关系 / 死链

## 知识库统计

| 维度 | 数量 |
|------|------|
| 场景 (scenarios) | 21 |
| 原子概念 (meta-concepts) | 35 |
| 组合概念 (compose-concepts) | 22 |
| 实体 (entities, 含子实体) | 112 |
| Wiki 页面总数 | 43 |
| Wiki 概念页 | 27 |
| Wiki 实体页 | 10 |
| Wiki 对比页 | 6 |

## 最近 24 小时变更

```
681d81c create: DataMax 配补调退业务指标体系
720ecda fix: 智能问答 → 智能问数
49a3cd6 update: DataMax 商品智能体 — 重点建设商品运营
5b1b1ae audit: 每日知识审计 2026-06-09 — 297 处发现，自动修复 7 项
3e376d5 update: DataMax 定位升级 — 从库存管理平台到数据中台
cd366bb fix: DataMax 定位修正 — 通用平台而非广州尚睿专属系统
0ad7977 ingest: DataMax 商品配补调业务蓝图需求规格说明书
```

**关键观察**：过去 24 小时密集摄入 DataMax 相关内容（配补调退业务指标体系、商品智能体、定位升级），新增 3 个 wiki 页面（`datamax.md`、`replenishment-allocation-transfer-return.md`、`datamax-business-metrics.md`）。

---

## 🔴 严重：死链检查

### Meta 层死链

✅ **全部通过** — 所有 scenario 的 concepts/entities/related_scenarios 引用均有效，所有 compose_concept 的 decomposition meta 引用均有效，所有 entity→entity 关系目标均存在。

### Wiki 层死链

✅ **无真实死链** — 检测到的 `[[wikilinks]]`、`[[note]]`、`[[笔记名]]`、`[[项目A]]` 均为 Obsidian 文档页面中的语法示例代码，非实际断链。`[[knowledge-base-article-writing]]` 引用的是 compose_concept ID，属于 meta 模型引用而非 wiki 页面链接。

---

## 🟡 警告：新实体未注册

### 1 个新实体需要注册到 meta/entities/

| ID | 名称 | Wiki 页面 | 建议分类文件 |
|----|------|-----------|-------------|
| `datamax` | DataMax | `wiki/entities/datamax.md` | `knowledge-management.yaml` |

**DataMax 实体骨架**：
```yaml
- id: datamax
  name: DataMax
  level: 1
  type: platform
  description: "数据中台/商品智能体平台，覆盖配补调退全链路业务指标体系"
  source: wiki/entities/datamax.md
  relations:
    - target: hermes-agent
      type: uses
    - target: feishu
      type: uses
    - target: enterprise-second-brain
      type: related_to
    - target: llm-wiki
      type: uses
```

---

## 🔵 建议：Wiki 概念页 → Meta 覆盖缺口

以下 23 个 wiki 概念页在 meta/ 中无对应条目。其中大部分为**领域知识页**（非方法/动作概念），不需要注册到 meta-concepts.yaml。但以下页面值得关注：

### 建议注册为组合概念

| Wiki 页面 | 建议 ID | 理由 |
|-----------|---------|------|
| `scenario-driven-cognitive-loop` | `scenario-driven-cognitive-loop` | 场景驱动认知闭环是核心方法论，已在 scenarios 中大量引用 |
| `ipo-closed-loop` | `ipo-closed-loop` | IPO 闭环是原子概念 `abstract-modeling` 的具体化，可作为组合概念 |
| `atom-compose-concept-architecture` | `atom-compose-concept-architecture` | 原子+组合双层架构是 meta/ 的核心设计模式 |
| `yaml-reverse-validation` | `yaml-reverse-validation` | YAML 反向校验是 `yaml-reverse-check` 的 wiki 页面，可注册为组合概念 |

### 建议注册为实体（或实体子项）

| Wiki 页面 | 建议 | 理由 |
|-----------|------|------|
| `cognitive-closed-loop` | 已在 knowledge-management.yaml 中注册为 `cognitive-closed-loop` | ✅ 已覆盖 |
| `enterprise-second-brain-architecture` | 已在 knowledge-management.yaml 中注册为 `enterprise-second-brain` | ✅ 已覆盖 |
| `karpathy-knowledge-base-method` | 已在 knowledge-management.yaml 中注册 | ✅ 已覆盖 |
| `llm-wiki` | 已在 knowledge-management.yaml 中注册 | ✅ 已覆盖 |
| `second-brain` | 已在 knowledge-management.yaml 中注册 | ✅ 已覆盖 |
| `knowledge-ontology-three-layer-model` | 建议注册为实体 | 知识本体三层模型是 wiki 架构的核心理念 |
| `hermes-memory-architecture` | 已在 ai-agent.yaml 中注册为 `hermes-memory-system` 子项 | ✅ 已覆盖 |
| `hermes-skills-system` | 已在 ai-agent.yaml 中注册为 `hermes-skills-system` 子项 | ✅ 已覆盖 |
| `hermes-session-isolation` | 已在 ai-agent.yaml 中注册为 `hermes-session-isolation` 子项 | ✅ 已覆盖 |
| `hermes-terminal-backends` | 已在 ai-agent.yaml 中注册为 `hermes-terminal-backends` 子项 | ✅ 已覆盖 |
| `hermes-knowledge-reference` | 已在 ai-agent.yaml 中注册为 `hermes-knowledge-reference` 子项 | ✅ 已覆盖 |
| `obsidian-bidirectional-links` | 已在 knowledge-management.yaml 中注册为 `obsidian-bidirectional-links` 子项 | ✅ 已覆盖 |
| `obsidian-dataview` | 已在 knowledge-management.yaml 中注册为 `obsidian-dataview` 子项 | ✅ 已覆盖 |
| `obsidian-headless` | 已在 knowledge-management.yaml 中注册为 `obsidian-headless` 子项 | ✅ 已覆盖 |
| `obsidian-tag-system` | 已在 knowledge-management.yaml 中注册为 `obsidian-tag-system` 子项 | ✅ 已覆盖 |
| `layered-memory-system` | 已在 ai-agent.yaml 中注册为 `layered-memory` | ✅ 已覆盖 |
| `memory-agent-vs-workflow-agent` | 已在 ai-agent.yaml 中注册为 `memory-agent` + `workflow-agent` | ✅ 已覆盖 |

### 纯领域知识页（无需注册到 meta/）

| Wiki 页面 | 理由 |
|-----------|------|
| `datamax-business-metrics` | DataMax 业务指标体系 — 领域知识 |
| `replenishment-allocation-transfer-return` | 配补调退业务概念 — 领域知识 |

---

## 🔵 建议：新关系

### 3 个新关系（依赖 datamax 实体注册）

```yaml
new_relations:
  - source: datamax
    target: hermes-agent
    type: uses
    reason: "DataMax 通过 Hermes Agent 进行知识管理和自动化"
  - source: datamax
    target: feishu
    type: uses
    reason: "DataMax 使用飞书作为交互通道"
  - source: datamax
    target: enterprise-second-brain
    type: related_to
    reason: "DataMax 是企业级第二大脑架构的业务数据层"
```

---

## ✅ 正面指标

| 检查项 | 状态 |
|--------|------|
| Scenario concepts 引用完整性 | ✅ 0 死链 |
| Scenario entities 引用完整性 | ✅ 0 死链 |
| Scenario related_scenarios 完整性 | ✅ 0 死链 |
| Compose concept decomposition 完整性 | ✅ 0 死链 |
| Entity→Entity relations 完整性 | ✅ 0 死链 |
| Entity→Concept 跨层引用 | ✅ 全部有效（12 条） |
| Wiki 真实死链 | ✅ 0 条 |

---

## 审计元数据

- **时间戳**: 2026-06-09 20:04 UTC
- **工具**: `/tmp/reverse_check.py` + `/tmp/analyze_deadlinks.py`
- **场景**: `knowledge-evolution-cycle` (Phase 2: 缺口发现)
- **下次建议审计**: 2026-06-10
- **严重发现**: 0
- **警告发现**: 1 (datamax 实体未注册)
- **建议发现**: 7 (4 个概念注册建议 + 3 个新关系)
