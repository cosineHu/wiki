---
title: Skills 编写指南（Skills Authoring Guide）
created: 2026-06-04
updated: 2026-06-08
type: concept
tags: [agent, skill, tutorial, agentskills]
sources: [raw/articles/skills-creation-tutorial-2026.md]
confidence: high
---

# Skills 编写指南

Skills 是 Anthropic 牵头发起、被 Claude Code、OpenClaw、Hermes Agent 等广泛采用的开放标准——把「教 AI 怎么做某件事」打包成一个文件夹，需要时按需加载、不用时不占上下文。

> 你反复粘贴同一段指令、同一份 checklist、同一套多步流程到对话框时——这就是该写成 Skill 的信号。

## Skills 是什么

**官方定义**（agentskills.io）：A standardized way to give AI agents new capabilities and expertise——一种给 AI Agent 增加能力和专业知识的标准化方式。

### 核心结构

```
my-skill/
├── SKILL.md          # 必需：metadata + 指令
├── scripts/          # 可选：可执行代码
├── references/       # 可选：参考文档
├── assets/           # 可选：模板、资源
└── ...               # 其他任意文件
```

## 三阶段加载机制（Progressive Disclosure）

| 阶段 | Agent 看到的内容 | 触发条件 |
|------|-----------------|----------|
| **Discovery（发现）** | 仅 name + description | Agent 启动时 |
| **Activation（激活）** | 完整 SKILL.md 内容 | 用户任务匹配 description |
| **Execution（执行）** | 跟随指令，按需调用 scripts/references | Skill 被触发后 |

**好处**：装 50 个 Skills 也不会拖慢启动——平时只占少量上下文，用到才加载。

**对编写者的启示**：
- `description` 是核心触发信号——写得不好，Skill 永远不会被自动激活
- SKILL.md 主体可以放心写——平时不进 context window
- scripts 与 references 进一步延后加载——重资源无负担

## 5 分钟创建第一个 Skill

### 第一步：创建目录

```bash
mkdir -p ~/.hermes/skills/user/hello-world
```

### 第二步：写 SKILL.md

```markdown
---
name: hello-world
description: A simple skill that says hello.
---

# Hello World Skill

When the user asks for a greeting, use the `echo` tool to say
"Hello from your custom skill!".
```

**两个关键约定**：
- 文件夹名和 `name` 字段保持一致
- 使用 hyphen-case：小写字母 + 数字 + 连字符

### 第三步：加载 Skill

```bash
# Hermes：重启会话或 gateway
hermes gateway restart

# 确认加载成功
hermes skills list
```

### 第四步：测试

```bash
hermes agent --message "give me a greeting"
# 或直接 /hello-world 手动触发
```

所有 Skill 名字都会自动注册成 slash 命令。

## SKILL.md Frontmatter 完整字段

### 通用字段（所有 agentskills 兼容工具）

| 字段 | 必需 | 说明 |
|------|------|------|
| `name` | ✅ | 全局标识符，hyphen-case |
| `description` | ✅ | 一行描述，告诉 Agent 什么时候用 |

### Claude Code 扩展

| 字段 | 说明 |
|------|------|
| 动态命令注入 | `!`命令`` 在加载前替换为命令输出 |
| Tool 预批准 | 声明需要的工具，避免运行时反复授权 |
| 参数传递 | `/skill-name 参数` 传给 Skill |

### OpenClaw 扩展

| 字段 | 说明 |
|------|------|
| `metadata.openclaw.os` | OS 过滤，如 `["darwin"]`、`["linux"]` |
| `metadata.openclaw.requires.bins` | 必需的 PATH 二进制工具 |
| `metadata.openclaw.requires.config` | 必需的 config 键 |

## 编写最佳实践

1. **description 决定触发率** — 写清楚「什么时候用」，而非「这是什么」
2. **保持原子化** — 一个 Skill 只做一件事
3. **包含验证步骤** — 告诉 Agent 如何确认任务完成
4. **记录踩坑经验** — 把已知问题和解决方案写进 Notes
5. **版本管理** — Skills 文件夹放入 Git，可追溯、可协作

## 跨工具兼容性

Skills 遵循 agentskills.io 开放标准，在 Claude Code、OpenClaw、Hermes Agent 之间基本互通。差异主要在扩展字段和加载机制（Claude Code 支持 Live Change Detection，OpenClaw/Hermes 需重启）。

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | 用户反复粘贴的同一段指令、checklist、多步流程（即"该写成 Skill 的信号"） |
| **Process** | ① 创建目录 ~/.hermes/skills/user/<name>/ → ② 编写 SKILL.md（name + description + 指令体） → ③ 按需添加 scripts/references/assets → ④ 重启 gateway 加载 → ⑤ 测试验证 |
| **Output** | 符合 agentskills.io 标准的可复用技能文件，支持三阶段渐进加载（Discovery → Activation → Execution） |
| **Tools** | skill_manage, skill_view, terminal, hermes gateway restart |
| **Quality Check** | description 是否写清了"什么时候用"而非"这是什么"？Skill 是否保持原子化？是否包含验证步骤？ |

## 相关

- [[hermes-skills-system]]
- [[hermes-agent]]
- [[openclaw-vs-hermes]]
