# meta/ — 场景驱动的认知闭环操作系统

> 从静态知识图谱到可执行认知操作系统。wiki/ 是"图书馆"，meta/ 是"工厂"。

## 三层架构

```
meta/
├── README.md              # 本文件
├── profile.md             # AI 操作规程（7步流水线 + 反向校验）
├── meta-concepts.yaml     # 原子概念：不可再分的最小方法单元（IPO 建模）
├── compose-concepts.yaml  # 组合概念：由原子概念串接而成的复合方法
├── entities/              # 实体层：人事物，5 个分类文件
│   ├── ai-agent.yaml               # AI Agent 相关实体
│   ├── infrastructure.yaml         # 基础设施实体
│   ├── knowledge-management.yaml   # 知识库与知识管理
│   ├── people.yaml                 # 人物实体
│   └── thinking-methods.yaml       # 思维方法实体
└── scenarios/             # 场景层：问题驱动的组装规则
    └── scenarios.yaml     # 20 个场景，每个包含 trigger/goal/composition/related_scenarios
```

## 设计原则

1. **分离关注点**：概念（动词/方法）与实体（名词/工具）严格分离
2. **原子-组合双层**：原子概念有完整 IPO，组合概念通过 decomposition 引用原子概念
3. **场景驱动**：知识库的价值在于"组装"而非"存储"，场景层定义组装规则
4. **自我迭代**：每次使用后执行 yaml 反向校验，发现新场景/概念/实体/关系
5. **最小关系集**：实体间仅 4 种关系：is_a、uses、depends_on、related_to

## 与 wiki/ 的关系

- wiki/ = 资料库 + 知识库（萃取后的结构化文章）
- meta/ = 模式库（元信息：结构、关系、组装规则）
- meta/ 通过 `source:` 字段反向指回 wiki/ 中的原始页面
- meta/ 不存储知识内容本身，只存储"如何调用和组装知识"

## 版本

- 创建: 2026-06-08
- 灵感来源: 人月聊IT《进一步优化LLM-Wiki大模型知识库，构建场景驱动的认知闭环》
- 适配领域: Hermes Agent + Obsidian 第二大脑 + AI Agent 知识体系
