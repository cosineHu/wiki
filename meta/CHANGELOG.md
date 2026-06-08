# meta/ CHANGELOG

> 记录 meta/ 元模型的所有变更
> 格式: `## [YYYY-MM-DD] type | description`

## [2026-06-08] create | meta/ 认知闭环操作系统初始化

- 灵感来源: 人月聊IT《进一步优化LLM-Wiki大模型知识库，构建场景驱动的认知闭环》
- 创建文件:
  - README.md (架构说明)
  - profile.md (AI 操作规程，7 步流水线 + yaml 反向校验)
  - meta-concepts.yaml (35 个原子概念，含完整 IPO 建模)
  - compose-concepts.yaml (20 个组合概念，含 decomposition 分解)
  - entities/knowledge-management.yaml (20 个知识管理实体)
  - entities/ai-agent.yaml (18 个 AI Agent 实体)
  - entities/thinking-methods.yaml (12 个思维方法实体)
  - entities/people.yaml (8 个人物实体)
  - entities/infrastructure.yaml (12 个基础设施实体)
  - scenarios/scenarios.yaml (20 个场景，含完整 composition 组装规则)
- 设计原则:
  - 三层架构: 场景层 → 概念层（原子+组合） → 实体层
  - IPO 闭环: Input → Process → Output
  - 4 种实体关系: is_a / uses / depends_on / related_to
  - 3 层实体层级上限
  - 每次使用后强制执行 yaml 反向校验
