---
title: Hermes 技能系统（Skills System）
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [llm, agent, skill, self-improving]
sources: [raw/articles/hermes-agent-deep-dive-2026.md]
confidence: high
---

# Hermes 技能系统（Skills System）

Hermes Agent 最有特色的设计——把 Agent 的经验转化为可调用的**程序性记忆**，而不只是陈述性记忆。

> 人类不只是记住「做饭的步骤」，而是练熟了之后形成肌肉记忆，不需要每次查食谱。Hermes 的技能系统就是 Agent 的「肌肉记忆」。

## 学习闭环

Hermes 的核心设计理念是一个**闭合的学习循环**，由四个部分构成：

| 组件 | 功能 |
|------|------|
| **持久记忆** | 主动将重要信息写入 `MEMORY.md`，定期检视和更新 |
| **用户建模** | 集成 Honcho dialectic 建模，跨会话建立用户偏好理解 |
| **技能系统** | 完成任务后自动提炼为可复用 Skill |
| **会话搜索** | SQLite FTS5 + LLM 摘要，跨历史会话检索 |

## 技能生命周期

```
完成任务 → 判断是否值得抽象 → 自动生成 SKILL.md
                                    ↓
下次同类任务 ← 自动加载技能 ← 存储在 ~/.hermes/skills/
                                    ↓
使用中发现改进点 → 自动修订技能文件
```

### 四个阶段

1. **自动创建**：Agent 完成复杂任务后，判断是否值得抽象成技能，自动生成技能文件
2. **使用中改进**：每次调用技能时，发现步骤有误或有更好做法，自动修订
3. **用户触发**：用 `/skill-name` 命令手动触发
4. **社区共享**：遵循 agentskills.io 开放标准，可在 Skills Hub 发布和下载

## 技能存储结构

```
~/.hermes/skills/
├── system/           # 内置技能
├── user/             # 用户创建的技能
└── openclaw-imports/ # 从 OpenClaw 迁移的技能
```

## 技能文件格式

每个技能是普通 Markdown 文件，任何人都可以手动编写和修改：

```markdown
# Deploy to Production

## When to use
Use this skill when asked to deploy a service to production.

## Steps
1. Run the test suite: `make test`
2. Build the Docker image: `docker build -t {service}:{version} .`
3. Push to registry: `docker push registry.example.com/{service}:{version}`
4. Update the Helm chart values and open a PR
5. After PR merge, monitor the canary deployment for 10 minutes
6. Check error rate — if > 1%, roll back immediately

## Notes
- Always deploy during low-traffic windows
- Notify #deployments channel before starting
```

## 本质

**把 Agent 的经验转化为可调用的程序性记忆**。这与其他 Agent 框架的「静态记忆」有本质区别——Hermes 是主动的、自驱的知识积累，而非被动等待用户告知。

## Reflection 机制

Hermes 最大的创新之一——任务完成后不立即结束，而是执行反思：

```
本次任务成功了吗？
为什么成功？为什么失败？
下次如何优化？
```

形成可复用经验。例如：

```
用户需求：搭建 RAG 系统
执行结果：成功
经验：先完成数据清洗，再进行 Embedding，效果最佳
```

## Skill Evolution

```
原始经历：完成一次 RAG 项目
     ↓ 总结
标准流程：RAG 部署标准流程
     ↓ 抽象
技能：企业知识库建设
```

实现 **经验 → 技能** 转化。这也是 Hermes 区别于 Workflow Agent（Dify/Coze/OpenClaw）的核心能力。

详见 [[Memory Agent vs Workflow Agent]]

## 相关

- [[Hermes Agent]]
- [[OpenClaw vs Hermes Agent]]
- [[Hermes 知识库参考方式]]
- [[Hermes 记忆架构（Memory Architecture）]]
- [[Memory Agent vs Workflow Agent]]
