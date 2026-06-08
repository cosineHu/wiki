---
title: Obsidian Dataview 查询
created: 2026-06-03
updated: 2026-06-08
type: concept
tags: [knowledge-base, query, plugin]
sources: [raw/articles/hermes-obsidian-core-ops-2026.md]
confidence: high
---

# Obsidian Dataview 查询

Dataview 是 Obsidian 最强大的社区插件之一，允许用类似 SQL 的语法查询 Vault 中的所有笔记。

## 基础搜索

| 搜索方式 | 快捷键 | 说明 |
|----------|--------|------|
| 快速切换 | `Cmd/Ctrl+O` | 按文件名搜索 |
| 全局搜索 | `Cmd/Ctrl+Shift+F` | 全文搜索 |
| 标签搜索 | 搜索框输入 `#tag` | 按标签搜索 |
| 路径搜索 | `path:文件夹名` | 按路径搜索 |

### 搜索运算符

```
搜索词1 搜索词2         → AND（同时包含）
搜索词1 OR 搜索词2      → OR（包含任一）
-搜索词                 → NOT（排除）
"精确短语"              → 精确匹配
/正则表达式/            → 正则搜索
```

## Dataview 查询语法

### LIST 查询

```dataview
LIST
FROM "文件夹名"
WHERE status = "进行中"
SORT date DESC
```

### TABLE 查询

```dataview
TABLE date, tags, status
FROM #工作
WHERE date >= date(today) - dur(7 days)
SORT date DESC
```

### TASK 查询

```dataview
TASK
FROM "日记"
WHERE !completed
SORT created DESC
```

### 常用查询示例

```dataview
-- 查找所有孤立笔记（无标签、无链接）
LIST
WHERE length(file.tags) = 0
AND length(file.outlinks) = 0

-- 最近 7 天创建的笔记
TABLE date, tags
WHERE date >= date(today) - dur(7 days)
SORT date DESC

-- 按状态分组
TABLE rows.file.link as "笔记"
FROM ""
GROUP BY status

-- 查找特定标签的笔记
TABLE date, status
FROM #重要/紧急
SORT date DESC
```

## 高级查询模板

### 项目仪表盘

```dataview
TABLE 
  status as "状态",
  deadline as "截止日期",
  length(file.outlinks) as "关联数"
FROM "2_项目"
SORT deadline ASC
```

### 知识图谱统计

```dataview
TABLE 
  length(file.inlinks) as "被引用",
  length(file.outlinks) as "引用",
  length(file.tags) as "标签数"
FROM "3_知识"
SORT length(file.inlinks) DESC
LIMIT 20
```

### 待办事项汇总

```dataview
TASK
FROM "日记" OR "2_项目"
WHERE !completed
GROUP BY file.link
```

## 图谱视图

### 图谱的价值

- **发现孤岛**：没有连线的笔记是知识孤岛
- **发现枢纽**：连线最多的笔记是核心概念
- **发现聚类**：自然形成的笔记群是知识领域
- **发现缺口**：两个聚类之间缺少连接

### 查看方式

| 方式 | 快捷键 | 说明 |
|------|--------|------|
| 全局图谱 | 左侧栏 → 图谱 | 所有笔记的关系网络 |
| 局部图谱 | `Cmd/Ctrl+G` | 当前笔记的关联网络 |
| 出链视图 | 右侧面板 | 当前笔记链接了哪些笔记 |
| 反向链接 | 右侧面板 | 哪些笔记链接了当前笔记 |

### 图谱阅读指南

- **大节点** = 被引用多 = 核心概念
- **密集区域** = 知识聚类 = 一个领域
- **孤立节点** = 需要关联的笔记
- **长连线** = 跨领域连接 = 创新点

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | 对 Obsidian Vault 中笔记的结构化查询需求（按状态/标签/日期/关联数筛选） |
| **Process** | ① 确定查询类型：LIST（列表）/ TABLE（表格）/ TASK（任务） → ② 指定数据源：FROM 文件夹或标签 → ③ 添加过滤条件：WHERE 子句 → ④ 排序和限制：SORT + LIMIT → ⑤ 嵌入笔记中用 dataview 代码块执行 |
| **Output** | 动态生成的结构化视图（项目仪表盘、知识图谱统计、待办汇总、孤立笔记检测） |
| **Tools** | Obsidian Dataview 插件, Dataview 查询语法（LIST/TABLE/TASK） |
| **Quality Check** | 查询是否返回预期结果？是否发现了知识孤岛（无标签+无链接的笔记）？图谱枢纽节点是否被正确识别？ |

## 相关

- [[obsidian]]
- [[obsidian-bidirectional-links]]
- [[obsidian-tag-system]]
- [[second-brain]]
