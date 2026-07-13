---
title: Matt Pocock 的 18 个 Claude Code Skill（Matt Pocock Skills）
created: 2026-06-10
updated: 2026-07-13
type: entity
tags: [claude-code, skill, engineering, tdd, debugging]
sources: [raw/2026-06-10-matt-pocock-skills/]
confidence: high
---

# Matt Pocock 的 18 个 Claude Code Skill

Matt Pocock（Total TypeScript 作者）开源的 Claude Code Skill 仓库（`mattpocock/skills`），85800+ 星，7400+ fork。解决 AI 编程的四种失败模式。

## 关键信息

| 维度 | 说明 |
|------|------|
| **作者** | Matt Pocock，TypeScript 布道者 |
| **仓库** | `mattpocock/skills` |
| **Skill 数** | 18 个 |
| **安装** | `npx skills@latest add mattpocock/skills` |
| **规范** | Anthropic SKILL.md 标准，无厂商锁定 |

## 四种失败模式

| 模式 | 问题 | 对应 Skill |
|------|------|-----------|
| 意图对齐失败 | 你和 AI 各想各的设计 | `/grill-me` `/to-prd` |
| 缺乏领域语言 | 每次重新解释概念 | `CONTEXT.md` `/grill-with-docs` |
| 没有反馈回路 | 没有测试，凭直觉走 | `/tdd` `/diagnose` |
| 架构腐化 | AI 加速代码也加速技术债 | `/improve-codebase-architecture` `/zoom-out` |

## 18 个 Skill 分类

### 工程类（10 个）— 核心

| Skill | 功能 |
|-------|------|
| `/tdd` | 强制红绿重构循环，一次只写一个测试 |
| `/diagnose` | 六阶段调试法：复现→假设→工具化→修复→清理→复盘 |
| `/grill-with-docs` | 拷问设计，同步更新 CONTEXT.md 和 ADR |
| `/grill-me` | 穷举决策树，编码前逼你想清楚 |
| `/to-prd` | 对话上下文转带 User Story 的 PRD |
| `/to-issues` | PRD 拆成可独立执行的竖切 GitHub Issue |
| `/triage` | 状态机驱动的 Issue 分类 |
| `/improve-codebase-architecture` | 定期识别模块边界腐化 |
| `/zoom-out` | 在陌生代码里先建立系统级理解 |
| `/prototype` | 快速脏原型 |

### 效率类（4 个）

| Skill | 功能 |
|-------|------|
| `/caveman` | 极度压缩通信，token 减少约 75% |
| `/handoff` | agent 换窗口时生成紧凑上下文摘要 |
| `/write-a-skill` | 写新 Skill 的 Skill（元层） |

### 杂项（4 个）

`git-guardrails`（拦截危险 git 操作）、`migrate-to-shoehorn`、`scaffold-exercises`、`setup-pre-commit`

## 核心 Skill 详解

### `/grill-me` — 编码前最重要的一步

把计划告诉 agent，它穷举追问直到双方对需求达成精确共识。Matt 自己第一次用时被问了 38 个问题，其中一半是从没认真想过的边界情况。

**最佳实践**：在「已有第一版方案」之后用，带具体设计去让它质疑。

### `/tdd` — 一次只前进一步

核心约束：一次只写一个测试，写完了才能动实现代码。三条规则：
- 测试只验证公共接口行为，不验证内部实现
- 重构只在所有测试绿了之后
- 代码只写刚好让当前测试过的最少量

### `/diagnose` — 六阶段调试法

```
建立反馈回路 → 复现 → 形成假设(3-5个可证伪) → 工具化(唯一tag日志)
    → 修复+回归测试 → 清理+事后复盘
```

### `/to-issues` — 竖切不横切

| 横切（不推荐） | 竖切（推荐） |
|--------------|------------|
| Schema Issue → API Issue → 前端 Issue | 一个 Issue 穿透所有层，端到端可验证 |
| 串行依赖，一层卡全停 | 薄但完整的功能切片 |

标签分类：`HITL`（需人决策）/ `AFK`（agent 独立完成）

## 设计哲学

每个 Skill 背后都是经典工程原则的 AI 化：

| Skill | 来源 |
|-------|------|
| `/grill-me` | 需求澄清会议 |
| `/tdd` | Kent Beck 极限编程 |
| `/diagnose` | 科学方法论 + Google SRE 事后分析 |
| `/to-issues` | Deep Module 理论 + 敏捷竖切 |
| `CONTEXT.md` | DDD 统一语言（Ubiquitous Language） |

## 局限

- 最适合已有工程化基础的项目，原型阶段强行套用会痛苦
- 深度绑定 GitHub Issues（Linear/Jira 需改造）
- `/grill-me` 成本高，建议只对高风险模块使用
- Skill 本身不等于能力，需要工程基础

## 相关

- [[skills-authoring-guide|Skills 编写指南]]
- [[hermes-skills-system|Hermes 技能系统]]
