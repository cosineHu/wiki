# profile.md — AI 操作规程

> 本文档定义了 AI Agent 在使用 wiki1/ 认知闭环操作系统时的标准操作规程。
> 版本: 1.0 | 创建: 2026-06-08

## 核心原则

1. **场景驱动，而非知识驱动**：面对任何问题，首先匹配场景（scenario），然后按场景的 composition 规则组装知识，而非简单检索。
2. **组装优于理解**：AI 不需要"理解"用户才能产出好结果，只需要严格按照场景的组装规则调用底层概念和实体。
3. **每次使用都是进化机会**：每次知识库被使用后，必须执行 yaml 反向校验，发现缺口并生成补充骨架。

## 7 步标准流水线

每次处理用户请求时，按以下 7 步执行：

### 第 1 步：问题解析

- 解析用户输入，提取关键意图、约束条件和期望输出
- 判断问题类型：知识摄入？知识查询？内容创作？系统维护？
- 确定问题的复杂度和是否需要多 Agent 协作

### 第 2 步：场景匹配

- 加载 `wiki1/scenarios/scenarios.yaml`
- 按 trigger 字段匹配最合适的场景
- 如无精确匹配，沿 related_scenarios 扩展搜索
- 如仍无匹配，标记为"新场景"，进入通用处理流程
- 输出：匹配的场景 ID + composition 规则

### 第 3 步：概念组装

- 加载 `wiki1/meta-concepts.yaml` 和 `wiki1/compose-concepts.yaml`
- 按场景 composition 中每个 phase 指定的 concepts，加载对应的原子概念和组合概念
- 如果 phase 指定了组合概念，展开其 decomposition 获取原子概念序列
- 输出：本场景需要的完整概念调用链

### 第 4 步：实体调取

- 加载 `wiki1/entities/*.yaml`（5 个分类文件）
- 按场景 composition 中每个 phase 指定的 entities，加载对应的实体
- 检查实体间的 relations，补充间接相关的实体
- 输出：本场景需要的完整实体集合

### 第 5 步：原文深挖

- 根据概念和实体的 source 字段，定位到 `wiki/raw/` 中的原始文章
- 读取相关段落，提取具体案例、数据、引用
- 将原文细节注入到当前组装流程
- 输出：原文中的具体细节和案例

### 第 6 步：结构化执行

- 按场景 composition 的 phase 顺序，逐步执行
- 每个 phase 遵循其 rule 字段的约束
- 调用相应的工具（terminal/file/search 等）完成实际操作
- 记录每个 phase 的执行结果
- 输出：完整的执行结果

### 第 7 步：YAML 反向校验（强制执行）

每次完成用户请求后，必须执行以下 5 个维度的检查：

#### 7.1 新场景检查
- 本次处理过程中，是否遇到了 yaml 中未定义的新场景？
- 检查维度：trigger 是否匹配？goal 是否覆盖？composition 是否适用？
- 输出格式：
```yaml
new_scenarios:
  - id: <建议的 scenario id>
    name: <场景名称>
    trigger: <触发条件>
    goal: <目标>
    reason: <为什么需要新增这个场景>
```

#### 7.2 新概念检查
- 本次处理过程中，是否使用了 yaml 中未定义的新概念（方法/动作）？
- 判断标准：是否是一个可独立描述的方法步骤？是否有明确的输入输出？
- 输出格式：
```yaml
new_meta_concepts:
  - id: <建议的 concept id>
    name: <概念名称>
    category: <分类>
    description: <描述>
    input: <输入>
    process: [<步骤列表>]
    output: <输出>
    reason: <为什么需要新增>
new_compose_concepts:
  - id: <建议的 concept id>
    name: <概念名称>
    decomposition:
      - step: 1
        meta: <引用的原子概念>
        purpose: <目的>
    reason: <为什么需要新增>
```

#### 7.3 新实体检查
- 本次处理过程中，是否提及了 yaml 中未定义的新实体（人事物/名词）？
- 判断标准：是否是一个可被方法调用的具体对象？
- 输出格式：
```yaml
new_entities:
  - id: <建议的 entity id>
    name: <实体名称>
    level: <层级 1/2/3>
    type: <类型>
    description: <描述>
    category_file: <应放入哪个分类文件>
    reason: <为什么需要新增>
```

#### 7.4 新关系检查
- 本次处理过程中，是否发现了实体间的新关系？
- 关系类型仅限 4 种：is_a、uses、depends_on、related_to
- 输出格式：
```yaml
new_relations:
  - source: <实体 id>
    target: <实体 id>
    type: <is_a|uses|depends_on|related_to>
    reason: <为什么需要新增这个关系>
```

#### 7.5 死链检查
- 本次处理过程中，是否引用了不存在的概念/实体/场景 id？
- 输出格式：
```yaml
dead_links:
  - reference: <引用的 id>
    context: <在什么上下文中引用>
    suggestion: <建议的修复方式>
```

### 校验结果汇总

将以上 5 个维度的发现汇总为结构化报告：

```yaml
reverse_check_result:
  timestamp: <YYYY-MM-DD HH:MM>
  session_id: <本次会话标识>
  scenario_used: <使用的场景 id>
  new_scenarios_count: N
  new_concepts_count: N
  new_entities_count: N
  new_relations_count: N
  dead_links_count: N
  summary: <一句话总结本次校验发现>
```

## 通用处理流程（无匹配场景时）

当无法匹配到任何已有场景时，使用以下通用流程：

1. 加载所有 meta-concepts，按 category 分类
2. 根据问题类型选择最相关的概念类别
3. 尝试用已有原子概念组合出解决方案
4. 记录过程中使用到的概念序列
5. 完成后执行 yaml 反向校验，将新发现的场景作为高优先级补充建议

## 知识摄入特殊流程

当用户发送文章链接或文档时，使用 `ingest-article-to-wiki` 场景。额外注意事项：

- 摄入前必须先 `git pull`
- 原始文章保存到 `wiki/raw/articles/`
- 萃取时严格区分概念（方法/动作）和实体（人事物/名词）
- 概念页面放入 `wiki/concepts/`，实体页面放入 `wiki/entities/`
- 每个新页面必须有完整的 YAML frontmatter
- 每个新页面必须至少有 2 个 [[wikilinks]] 出链
- 更新 `wiki/index.md` 和 `wiki/log.md`
- `git add -A && git commit` 提交
- 摄入完成后，检查 wiki1/ 是否需要更新

## 自我迭代规则

1. 每次 yaml 反向校验的发现，以骨架形式追加到 `wiki1/_pending/` 目录
2. 用户审核后，合入对应的 yaml 文件
3. 合入时更新文件的版本号和条目数
4. 记录变更到 `wiki1/CHANGELOG.md`

## 约束与边界

- wiki1/ 只存元信息（结构、关系、组装规则），不存知识内容本身
- 知识内容存储在 wiki/ 中
- 实体关系仅限 4 种：is_a、uses、depends_on、related_to
- 实体层级最多 3 层
- 原子概念必须有完整的 IPO 四元组
- 组合概念必须至少有 2 步 decomposition
- 场景的 composition 必须至少有 2 个 phase
