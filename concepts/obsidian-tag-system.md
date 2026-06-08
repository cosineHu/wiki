---
title: Obsidian 标签系统与 Frontmatter
created: 2026-06-03
updated: 2026-06-08
type: concept
tags: [knowledge-base, markdown, metadata]
sources: [raw/articles/hermes-obsidian-core-ops-2026.md]
confidence: high
---

# Obsidian 标签系统与 Frontmatter

标签和 Frontmatter 是 Obsidian 笔记的元数据层，让笔记可被程序化检索和管理。

## Frontmatter：笔记身份证

每篇笔记顶部的 YAML 元数据块，用 `---` 包裹：

```yaml
---
title: 笔记标题
date: 2026-04-26
tags: [ai, agent, obsidian]
status: 进行中
category: 技术
---
```

### 推荐字段

| 字段 | 说明 | 示例 |
|------|------|------|
| `title` | 笔记标题 | `"双向链接详解"` |
| `date` | 创建日期 | `2026-04-26` |
| `tags` | 标签列表 | `[ai, agent]` |
| `status` | 状态 | `草稿 / 进行中 / 已完成` |
| `category` | 分类 | `技术 / 生活 / 工作` |
| `aliases` | 别名 | `["别名1", "别名2"]` |

### 常用字段速查表

```yaml
# 基础
title: 笔记标题
date: 2026-04-26
tags: [ai, agent]
aliases: [别名1, 别名2]

# 状态
status: 草稿        # 草稿/进行中/已完成/已归档
priority: 高        # 高/中/低

# 来源
source: https://...
author: 作者名

# 项目
project: 项目名
deadline: 2026-05-01
```

## 标签规范

### 格式规范

```
格式: 领域/子领域/具体类型
规则:
  - 全小写
  - 用 / 分层
  - 不混用中英文
```

### 标签层级设计

```
# 按领域
工作/会议
工作/项目
工作/文档
学习/课程
学习/论文
生活/健康
生活/理财

# 按状态
状态/草稿
状态/进行中
状态/已完成
状态/已归档

# 按优先级
重要/紧急      ← 马上做
重要/不紧急    ← 计划做
不重要/紧急    ← 委托
不重要/不紧急  ← 删除
```

### 标签 vs 链接

| | 标签 `#tag` | 链接 `[[note]]` |
|---|---|---|
| 用途 | 分类、状态标记 | 建立笔记间关系 |
| 粒度 | 粗粒度分类 | 细粒度关联 |
| 示例 | `#工作/项目` | `[[项目A]]` |
| 数量 | 每篇 3-5 个 | 每篇 2+ 个 |

> 标签回答"这是什么类型"，链接回答"这和什么相关"。

### 状态标签系统

```markdown
# 笔记生命周期
#状态/种子      → 刚记下的想法
#状态/幼苗      → 开始整理
#状态/成长中    → 持续完善
#状态/常青      → 成熟笔记
#状态/枯萎      → 过时内容
```

## 踩坑提醒

- **标签不要太多层**：3-4 层足够，太深导致菜单难以使用
- **统一格式**：全小写、`/` 分层，不混用中英文
- **批量修改标签**：使用 Tag Wrangler 插件或全局搜索替换
- **标签和文件夹不冲突**：文件夹决定物理位置，标签决定逻辑分类

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | 需要分类和标记的 Obsidian 笔记 |
| **Process** | ① 设计标签层级：领域/子领域/具体类型（全小写，/ 分层） → ② 在笔记 Frontmatter 中声明 tags 字段 → ③ 按状态标签管理笔记生命周期（种子→幼苗→成长中→常青→枯萎） → ④ 用 Dataview 按标签查询和统计 |
| **Output** | 结构化的笔记元数据层，支持按标签检索、分组、统计 |
| **Tools** | Obsidian Frontmatter (YAML), Obsidian 标签系统, Dataview 插件, Tag Wrangler 插件 |
| **Quality Check** | 标签层级是否不超过 3-4 层？格式是否统一（全小写、/ 分层）？标签和文件夹是否各司其职？ |

## 相关

- [[obsidian]]
- [[obsidian-bidirectional-links]]
- [[obsidian-dataview]]
- [[second-brain]]
