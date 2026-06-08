---
title: Obsidian
created: 2026-06-03
updated: 2026-06-08
type: entity
tags: [tool, knowledge-base, markdown]
sources: [raw/articles/hermes-obsidian-core-ops-2026.md, raw/articles/knowledge-management-llm-skills-2026.md]
confidence: high
---

# Obsidian

基于本地 Markdown 文件的笔记与知识管理工具，是 [[second-brain]] 系统的存储核心。

## 创始人的哲学

[[kepano]]（Steph Ango）是 Obsidian 的创始人兼 CEO，他的产品哲学深刻影响了 Obsidian 的设计：

- **File over app（文件优先于应用）**：「如果你希望你的写作在 2060 年甚至 2160 年的电脑上仍然可读，那它最好在 1960 年代的电脑上也能被读取。」
- **Don't delegate understanding（不要把理解力外包）**：你可以让 AI 帮你干活，但不能让 AI 替你思考。理解力本身就是你最大的资产。
- **个人笔记库应该是「你」的思想的体现**，而不是 AI 的。他建议给 Agent 单独的「杂乱仓库」，只在产出有用工件时才手动搬到主仓库。

关于 AI 与人知识边界的讨论，详见 [[ai-human-knowledge-boundary]]。

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

- [[obsidian-bidirectional-links]] — 笔记间相互引用，构建知识网络
- [[obsidian-tag-system]] — 标签层级设计、Frontmatter 元数据
- [[obsidian-dataview]] — Dataview 插件进行高级查询
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

- [[second-brain]]
- [[hermes-agent]]
- [[obsidian-headless]]
- [[llm-wiki]]
- [[kepano]]
- [[ai-human-knowledge-boundary]]
- [[knowledge-management-pipeline]]
