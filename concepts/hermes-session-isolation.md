---
title: Hermes 会话隔离机制
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [llm, agent, security, multi-tenant]
sources: [raw/articles/hermes-vs-openclaw-features-2026.md]
confidence: high
---

# Hermes 会话隔离机制

Hermes Agent 提供四层会话隔离，确保不同用户、群组、场景之间的对话上下文互不干扰。

## 四层隔离

### 1. Profile 隔离（最接近 dmScope）

每个 profile 有独立的 config、sessions、skills、memory。

```bash
# 为不同用户/场景创建完全隔离的实例
hermes profile create --name "team-a"
hermes profile create --name "team-b"
```

**用途**：给不同 Telegram 群组分配不同 profile，各自互不干扰。

### 2. Gateway 平台级隔离

Gateway 为每个平台的每个用户/群组自动创建独立 session。

| 平台 | 隔离粒度 |
|------|----------|
| Telegram | 每个用户独立 session |
| Discord | 每个频道独立 session |
| Slack | 每个 channel 独立 session |
| 飞书 | 每个用户/群组独立 session |

不同用户、不同频道各自有独立的对话上下文，不会串。

### 3. Pairing（DM 授权）

准入控制，类似 dmScope。

```bash
hermes pairing list      # 查看待审批的配对请求
hermes pairing approve   # 批准用户/DM 交互
hermes pairing revoke    # 撤销授权
```

控制哪些用户/DM 可以与 Agent 交互。

### 4. 终端/文件隔离

- **delegate_task 子代理**：每个子代理有独立的终端会话和工作目录
- **cronjob**：可以设置 `workdir` 限制工作范围

## 隔离层次总览

```
┌─────────────────────────────────────────┐
│           Profile 隔离（最外层）          │
│  独立 config / sessions / skills / memory │
├─────────────────────────────────────────┤
│         Gateway 平台级隔离               │
│   每个用户/群组独立 session              │
├─────────────────────────────────────────┤
│         Pairing 准入控制                 │
│   控制谁能与 Agent 交互                  │
├─────────────────────────────────────────┤
│         终端/文件隔离（最内层）           │
│   子代理独立终端，cronjob 限制 workdir    │
└─────────────────────────────────────────┘
```

## 相关

- [[Hermes Agent]]
- [[OpenClaw vs Hermes Agent]]
- [[第二大脑（Second Brain）]]
