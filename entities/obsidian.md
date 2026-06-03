---
title: Obsidian
created: 2026-06-03
updated: 2026-06-03
type: entity
tags: [tool, knowledge-base, markdown]
sources: [raw/articles/hermes-obsidian-core-ops-2026.md]
confidence: high
---

# Obsidian

基于本地 Markdown 文件的笔记与知识管理工具，是 [[第二大脑（Second Brain）]] 系统的存储核心。

## 核心特性

- **本地优先**：所有数据存储在本地文件夹，数据完全属于自己
- **Markdown 编辑**：纯文本格式，可迁移，不锁定厂商
- **双向链接**：笔记之间可以相互引用，构建知识网络
- **图谱可视化**：直观展示知识网络，发现隐藏关联
- **插件生态**：丰富的社区插件，几乎可定制任何功能
- **永久免费**（离线使用）

## 与其他工具对比

| 工具 | 优点 | 缺点 |
|------|------|------|
| Notion | 好看、功能丰富 | 付费、云端、封闭 |
| Roam Research | 双向链接先驱 | 贵、云端 |
| Logseq | 开源、大纲笔记 | 生态较弱 |
| **Obsidian** | **本地、开源、插件多** | 需要自己配置 |

## 在第二大脑中的位置

```
用户界面层: Web UI / Obsidian Desktop / Mobile / CLI
    ↓
智能层: Hermes Agent（LLM推理 + 记忆系统 + 工作流 + 自动化）
    ↓
存储层: Obsidian Vault（Markdown 文件 + 双向链接 + 图谱）
    ↓
文件系统层: 本地磁盘 / iCloud / Syncthing
```

| 组件 | 职责 |
|------|------|
| Hermes Agent | 智能推理、自动化、上下文管理 |
| Obsidian | 持久存储、知识组织、图谱展示 |
| 文件系统 | 备份、同步 |

## 核心概念

- [[双向链接（Obsidian）]] — 笔记间相互引用，构建知识网络
- [[Obsidian 标签系统与 Frontmatter]] — 标签层级设计、Frontmatter 元数据
- [[Obsidian Dataview 查询]] — Dataview 插件进行高级查询
- [[obsidian-headless]] — 服务器端无头同步

## 常见踩坑

1. **文件夹分类综合症**：疯狂建文件夹分类 → 用标签+链接替代
2. **笔记越写越长**：一篇几万字 → 原子化原则，每条笔记一个想法
3. **只写不连**：笔记孤立无链接 → 强制每篇至少关联一篇已有笔记
4. **标签混乱**：格式不统一 → 建立严格标签规范（全小写、`/` 分层）
5. **追求完美结构**：花大量时间设计系统 → 先跑通流程，再优化细节

## 推荐文件夹结构

```
📁 0_临时/      ← 收集箱
📁 1_日志/      ← 每日日志
📁 2_项目/      ← 项目
📁 3_知识/      ← 沉淀知识
```

真正的分类靠标签和链接，而非文件夹。

## 相关

- [[第二大脑（Second Brain）]]
- [[Hermes Agent]]
- [[obsidian-headless]]
- [[LLM Wiki（Karpathy 模式）]]
