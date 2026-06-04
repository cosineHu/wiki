---
title: Hermes 记忆架构（Memory Architecture）
created: 2026-06-04
updated: 2026-06-04
type: concept
tags: [llm, agent, memory, architecture]
sources: [raw/articles/hermes-memory-system-2026.md]
confidence: high
---

# Hermes 记忆架构（Memory Architecture）

Hermes Agent 的记忆系统不是单一存储，而是一个**分层连续性架构**——四层记忆各司其职，核心设计原则是：**保持 prompt 稳定以便缓存，把其他一切交给工具**。

> 不是记住更多，而是在正确的层级、以正确的成本，记住正确的东西。

## 四层记忆总览

```
┌─────────────────────────────────────────────┐
│              第四层：Honcho（可选）            │
│         深层用户建模 + AI 自我表征            │
├─────────────────────────────────────────────┤
│         第三层：Skills（程序性记忆）           │
│      可复用工作流，按需加载，索引注入          │
├─────────────────────────────────────────────┤
│      第二层：session_search（情景回忆）        │
│    SQLite + FTS5，按需搜索 + LLM 摘要         │
├─────────────────────────────────────────────┤
│    第一层：MEMORY.md + USER.md（热记忆）       │
│       冻结快照，~1,300 tokens，缓存友好        │
└─────────────────────────────────────────────┘
```

## 第一层：冻结的 Prompt 记忆

### 两个文件

| 文件 | 作用 | 限制 |
|------|------|------|
| `MEMORY.md` | 环境、约定、工具习惯、经验教训 | 2,200 字符 |
| `USER.md` | 用户画像：偏好、沟通风格、身份 | 1,375 字符 |

合计约 **1,300 tokens**，刻意保持极小。

### 关键设计

- **冻结快照**：会话开始时加载，整个会话期间不变。中途写入立即落盘，但只在新会话或压缩重建后才进入 prompt
- **字符限制而非 token 限制**：与模型无关，不需要依赖特定 tokenizer
- **纯文本 § 分隔**：无向量数据库，无自定义二进制存储
- **筛选而非日记**：只存高价值信息，不存任务进度/会话结果/TODO

### 写入规则

- ✅ 保存用户偏好、环境事实、反复纠正、稳定约定
- ❌ 不保存任务进度、会话结果、临时 TODO

### Memory 工具

三个动作：`add`、`replace`、`remove`。用子串匹配定位，无需内部 ID。内置安全扫描：拦截 prompt injection、凭证泄露、SSH 后门暗示、不可见 Unicode。

## 第二层：session_search（情景回忆）

### 存储

`~/.hermes/state.db` — SQLite 数据库：
- sessions 表 + messages 表
- FTS5 全文搜索索引
- parent_session_id 链路关系

### 召回流程

```
FTS5 搜索 → 按 session 分组 → 解析 lineage
→ 载入最匹配 sessions → 截断附近 transcript
→ 辅助模型总结 → 聚焦 recap 返回主模型
```

### 设计哲学

- 让始终注入的记忆保持小
- 把真正的历史存进 SQLite
- 只有需要时才搜索
- 搜索结果先总结，再交回主模型

> MEMORY.md = 持久事实，session_search = 情景回忆

## 第三层：压缩与 Memory Flush

长对话压缩前的关键步骤：

```
长对话 → flush 耐久事实到 memory → 压缩旧轮次
→ 重建 prompt → 用更小上下文 + 更新后的记忆继续
```

压缩前注入合成指令，让模型把值得跨越压缩期存活的信息写入 MEMORY.md/USER.md，然后才摘要掉中间对话。压缩后 prompt 缓存失效并重建，新记忆进入下一轮稳定快照。

## 第四层：Skills 作为程序性记忆

参见 [[Hermes 技能系统（Skills System）]]

Skills 存储可复用工作流。不会全部注入 prompt，只注入紧凑索引，需要时加载完整内容——程序性记忆随时可用但不消耗每轮 token。

## 第五层：Honcho（可选）

- 跨会话用户建模
- 跨机器、跨平台连续性
- 语义搜索用户上下文
- 为「用户」和「AI assistant」两个对象建模

### Prompt Caching 兼容

- 第一轮：Honcho 上下文烘进缓存 system prompt
- 后续轮次：不修改稳定 system prompt，只在当前轮 API 调用时附加召回结果
- 第 N 轮消费第 N-1 轮后台预取的上下文

## Hermes vs OpenClaw 记忆对比

| | OpenClaw | Hermes |
|---|---|---|
| 记忆方式 | Markdown-first 存储 | 热工作集 + 冷检索层 |
| 日志 | 追加式每日日志 | 筛选后的状态，非日记 |
| 历史存储 | 笔记文件混合搜索 | SQLite + FTS5 独立存储 |
| 程序性记忆 | 无独立机制 | Skills 系统 |
| 用户建模 | 无 | Honcho（可选） |
| 缓存感知 | 弱 | 强（冻结快照、延迟更新、轮次级注入） |

**关键洞察**：Hermes 比 OpenClaw 更「缓存感知」。OpenClaw 偏向「记忆就是可搜索的存储知识」，Hermes 偏向「记忆是热工作集 + 冷检索层」。

## Hermes 做对的三件事

1. **热记忆和冷召回分开** — 小型 prompt memory 只放永远重要的东西，需要时才搜索
2. **Prompt 稳定性是一等约束** — 冻结快照、延迟更新、压缩后重建，都指向同一原则
3. **承认记忆是复数** — 语义画像 + 情景回忆 + 程序性记忆 + 高阶用户建模

## 相关

- [[Hermes Agent]]
- [[Hermes 技能系统（Skills System）]]
- [[OpenClaw vs Hermes Agent]]
- [[Hermes 知识库参考方式]]
