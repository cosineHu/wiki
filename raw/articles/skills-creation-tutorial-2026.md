---
source_url: https://www.cnblogs.com/qiniushanghai/p/20027864
ingested: 2026-06-04
sha256: 31617235a7b7a5e8f77690cdea9e6aa870f75fb0d2d40c1fd6b3ce0a78c195a6
---

# Skills 从 0 到 1 怎么写：AI Agent Skills 完整创建教程（2026）

Skills 是 Anthropic 牵头发起、被 Claude Code、OpenClaw、Hermes Agent 等 AI Agent 工具广泛采用的开放标准——把"教 AI 怎么做某件事"打包成一个文件夹，需要时按需加载、不用时不占上下文。本文基于 2026 年 5 月 agentskills.io 官方标准 与 Claude Code Skills 文档、OpenClaw Skills 创建文档 真实数据，整理一套从 0 到 1 写出第一个可用 Skill 的完整教程。

## Skills 是什么：让 AI 学会一项新本领

**Skills 的官方定义**（来自 agentskills.io）：A standardized way to give AI agents new capabilities and expertise——一种给 AI Agent 增加能力和专业知识的标准化方式。

**核心结构非常简单**：一个 Skill 就是一个文件夹，里面有一个必须的 `SKILL.md` 文件，加上可选的脚本、参考资料、模板等：

`my-skill/
├── SKILL.md          # 必需：metadata + 指令
├── scripts/          # 可选：可执行代码
├── references/       # 可选：参考文档
├── assets/           # 可选：模板、资源
└── ...               # 其他任意文件
`
```

**Skills 解决的核心问题**：你反复粘贴同一段指令、同一份 checklist、同一套多步流程到对话框时——这就是该写成 Skill 的信号。Skills 把这些过程性知识打包成可版本管理、可复用、跨工具兼容的文件夹。

数据来源：agentskills.io 官方文档 2026 年 5 月。

## 一、Skills 的三阶段加载机制（Progressive Disclosure）

**理解 Skills 的工作原理是写好 Skills 的前提**。官方文档明确把 Skills 加载分为三阶段：

阶段
Agent 看到的内容
触发条件

Discovery（发现）
仅读取每个 Skill 的 name + description
Agent 启动时

Activation（激活）
把完整 SKILL.md 内容读入上下文
用户任务匹配某个 Skill 的 description

Execution（执行）
跟随指令，按需调用 scripts 或加载参考文件
Skill 被触发后

**这个机制带来的好处**：你可以装很多 Skills，平时只占少量上下文（仅 name + description），只有真正用到时才加载完整内容。一个仓库装 50 个 Skills 也不会拖慢启动。

**对写 Skill 的人意味着什么**：

- **description 是核心触发信号**——写得不好，Skill 永远不会被自动激活

- **SKILL.md 主体的长度可以放心写**——平时不会进 context window

- **scripts 与 references 进一步延后加载**——重资源放进去也没负担

## 二、5 分钟创建第一个 Skill：实战示例

**官方推荐的 hello-world Skill 创建路径**（以 OpenClaw 为例，Claude Code / Hermes 流程类似）：

## 第一步：创建 Skill 目录

`mkdir -p ~/.openclaw/workspace/skills/hello-world
`
```

Claude Code 等价命令：

`mkdir -p ~/.claude/skills/hello-world
`
```

## 第二步：写 SKILL.md

在 Skill 目录里创建 `SKILL.md`，包含 YAML frontmatter 和 Markdown 指令两部分：

`---
name: hello-world
description: A simple skill that says hello.
---

# Hello World Skill

When the user asks for a greeting, use the `echo` tool to say
"Hello from your custom skill!".
`
```

**两个关键约定**：

- 文件夹名和 frontmatter 的 `name` 字段保持一致

- 使用 hyphen-case：小写字母 + 数字 + 连字符

## 第三步：让 Agent 加载 Skill

OpenClaw：

`# 在会话中重开新会话
/new

# 或重启 gateway
openclaw gateway restart

# 确认加载成功
openclaw skills list
`
```

Claude Code 支持 **Live Change Detection**——文件改完无需重启，当前会话内即生效。OpenClaw 需要 `/new` 或 `gateway restart`。

## 第四步：测试

`# 命令行测试
openclaw agent --message "give me a greeting"

# 或在对话中说："给我打个招呼"
`
```

或者直接 `/hello-world` 手动触发——所有 Skill 名字都会自动注册成 slash 命令。

## 三、SKILL.md frontmatter 完整字段

**Skills 的 YAML frontmatter 是 Agent 识别 Skill 的关键元数据**。

## 通用字段（所有兼容 agentskills 的工具支持）

字段
必需
说明

name
是
全局标识符，hyphen-case（小写字母 + 数字 + 连字符）

description
是
一行描述，告诉 Agent "什么时候用这个 Skill"

## Claude Code 扩展字段

字段
说明

动态命令注入
在 SKILL.md 正文里写 `!`命令`` 会在加载前被替换为命令输出

Tool 预批准
可声明该 Skill 需要的工具，避免运行时反复授权

参数传递
调用 `/skill-name 参数` 时把参数传给 Skill

## OpenClaw 扩展字段

字段
说明

metadata.openclaw.os
OS 过滤，例如 `["darwin"]`、`["linux"]`

metadata.openclaw.requires.bins
必需的 PATH 二进制工具

metadata.openclaw.requires.config
必需的 config 键

数据来源：Claude Code Skills 文档与 OpenClaw Creating Skills 文档 2026-05-13。

## 四、四种 Skill 内容类型

**Skill 主体内容按使用方式可以分为四类**：

## 1. Reference 内容（参考知识）

提供 Agent 在当前工作中应用的知识：约定、模式、风格指南、领域知识。

`---
name: api-conventions
description: API design patterns for this codebase
---

When writing API endpoints:
- Use RESTful naming conventions
- Return consistent error formats
- Include request validation
`
```

## 2. Task 内容（任务步骤）

给 Agent 具体动作的逐步指令：部署、提交、代码生成等。

`---
name: deploy
description: Deploy the application to production
---

1. Run `pnpm test` and confirm all pass
2. Run `pnpm build` and check for warnings
3. Tag the release: `git tag v$(cat package.json | jq -r .version)`
4. Push tags: `git push --tags`
5. Trigger the deploy workflow on GitHub
`
```

## 3. Template 内容（模板填充）

让 Agent 按既定结构填空——例如周报、邮件、PR 描述。可以引用同目录下的 `template.md`。

## 4. Workflow 内容（多步流程 + 子 Agent）

Claude Code 支持把 Skill 跑在子 Agent 里，做隔离的研究 / 探索任务，避免污染主对话上下文。

## 五、Skills 存储位置与优先级

**不同位置的 Skill 有不同的作用域和优先级**。

## Claude Code 存储位置

位置
路径
作用域

Enterprise
受管设置
组织内所有用户

Personal
`~/.claude/skills/<name>/SKILL.md`
你的所有项目

Project
`.claude/skills/<name>/SKILL.md`
当前项目

Plugin
`<plugin>/skills/<name>/SKILL.md`
Plugin 启用的地方

**优先级规则**：同名时 enterprise 覆盖 personal、personal 覆盖 project。Plugin Skill 用 `plugin-name:skill-name` 命名空间，不会冲突。

## OpenClaw 存储位置（按优先级从高到低）

位置
优先级
作用域

`<workspace>/skills/`
最高
Per-agent

`<workspace>/.agents/skills/`
高
Per-workspace agent

`~/.agents/skills/`
中
共享 agent profile

`~/.openclaw/skills/`
中
所有 agent 共享

Bundled（OpenClaw 自带）
低
全局

`skills.load.extraDirs`
较低
自定义共享文件夹

## Hermes Agent 存储位置（已知优先级）

`<workspace>/skills/` > `<workspace>/.agents/skills/` > `~/.agents/skills/` > `~/.hermes/skills/` > Bundled > `skills.load.extraDirs`。

数据来源：三个工具的官方文档 2026 年 5 月。

## 六、写出"AI 真的会触发"的 description

**description 写得好不好，决定了 Skill 是否被自动激活**。这是 Skills 写作中最关键的一步。

**反例（很难触发）**：

`description: A skill for code stuff
`
```

**正例（高触发率）**：

`description: Summarizes uncommitted changes and flags anything risky. Use when the user asks what changed, wants a commit message, or asks to review their diff.
`
```

**写好 description 的 4 条规则**：

- **写出 Agent 应该看到什么样的请求时使用这个 Skill**——明确"用户问什么"，不要只描述 Skill 本身做什么

- **列出多种触发关键词**——"asks what changed / wants a commit message / asks to review their diff"，覆盖用户可能的多种说法

- **保持单行简洁**——多数 Agent 在 Discovery 阶段会**截断过长的 description**

- **避免歧义**——如果两个 Skill 的 description 太相似，Agent 会随机激活其中一个

## 七、动态上下文注入：让 Skill 在加载时获取实时数据

**Claude Code 的杀手锏**——`!` 语法让 Skill 在加载时执行命令并把输出注入正文：

`---
description: Summarizes uncommitted changes and flags anything risky. Use when the user asks what changed, wants a commit message, or asks to review their diff.
---

## Current changes

!`git diff HEAD`

## Instructions

Summarize the changes above in two or three bullet points, then list any risks you notice such as missing error handling, hardcoded values, or tests that need updating. If the diff is empty, say there are no uncommitted changes.
`
```

**这一行 `!`git diff HEAD`` 让 Claude Code 在 Skill 内容到达 Claude 之前先执行 `git diff HEAD` 命令，把输出替换进来**——意味着 Claude 看到的不是"请帮我看下我改了什么"，而是已经包含完整 diff 的具体提示。

**适合用动态注入的场景**：

- `!`git diff HEAD`​`：当前未提交修改

- `!`git log --oneline -10`​`：最近 10 个 commit

- `!`pnpm test 2>&1 | tail -50`​`：最近一次测试结果

- `!`ls -la dist/`​`：构建产物清单

- `!`cat package.json`​`：项目配置

## 八、安全与实践规范

**Skills 跑在 Agent 工具链里，安全性问题需要前置考虑**：

- **避免命令注入**：如果你的 Skill 用 `exec` 工具，不要把用户输入直接拼进 shell 命令——优先用工具的参数化接口

- **声明依赖**：用 `metadata.openclaw.requires.bins` 之类的字段说明 Skill 需要哪些二进制工具（git、pnpm、jq 等），缺了会跳过加载而不是运行报错

- **本地测试**：写完先用 `openclaw agent --message "..."` 或 Claude Code 直接调用测试，再分享出去

- **保持简洁**：description 一行、SKILL.md 主体聚焦——指示 AI 做什么，不要写"你是一个有帮助的助手"这类废话

- **版本管理**：Skill 是文件夹，可以丢 git 仓库做团队共享 / 版本历史

- **共享生态**：Claude 的 Skills 可上传到 Anthropic Skills Marketplace；OpenClaw 用 ClawHub；Hermes 用 agentskills.io 标准的注册中心

## 九、Skills 的常见组合用法

**重度用户会把 Skills 与其他能力组合**，提升 Agent 工作流：

## 与 MCP Server 组合

Skill 提供"知识 / 流程"，MCP 提供"外部工具"。例如做"GitHub PR 审查 Skill"时让 Skill 指令调用 `@modelcontextprotocol/server-github` 提供的 PR 接口。

## 与子 Agent / 子任务组合

Claude Code 支持把 Skill 跑在子 Agent 里。研究类 Skill（如 `/research`）可以走子 Agent 的隔离上下文，避免主对话被研究内容塞满。

## 与定时任务组合

OpenClaw 与 Hermes 都支持 `cron create` 时绑定 `--skill <name>`，让定时任务自动加载 Skill。例如每周一早上 9 点跑 `/weekly-report` Skill 生成周报。

## 跨工具复用同一个 Skill

agentskills.io 标准被 Claude Code、OpenClaw、Hermes Agent 共同采用——一个 SKILL.md 可以同时用在三个工具里，不需要重复维护。差别只在某些扩展字段（动态注入语法、metadata 字段）的支持范围。

## 十、调试 Skills：触发不到怎么办

**写完 Skill 测试不通过，按这个顺序排查**：

症状
可能原因
处理

Skill 不触发
description 写得不够明确
用更具体的"用户问什么"句式重写；列多个触发关键词

Skill 不触发
文件夹名和 name 字段不一致
检查目录名 == frontmatter 的 name

Skill 不触发
文件没被发现
跑 `openclaw skills list` 或 `claude` 启动时确认列表

Skill 不触发
description 被截断
缩短描述（多数工具有长度限制）

Skill 触发太频繁
description 太宽泛
加上具体上下文限制（"only when X"）

Skill 加载失败
缺少依赖
用 `requires.bins` 声明依赖；本地装好对应工具

Live change 不生效
OpenClaw / Hermes 的会话需要重新加载
`/new` 或 `gateway restart`

Skill 执行报错
scripts 没有执行权限
`chmod +x scripts/run.sh`

## 常见问题

**Q：Skills 和 CLAUDE.md / AGENTS.md 这种"项目记忆"有什么区别？**

两者解决不同问题。CLAUDE.md / AGENTS.md 是**始终加载**的项目级记忆——任何对话都会把它读进 system prompt，适合放"全局事实"（项目用的技术栈、风格约定、关键决策）。Skills 是**按需加载**——只有触发时才进上下文，适合放"过程性指令"（多步流程、checklist、复杂任务模板）。如果你的 CLAUDE.md 越长越像一份操作手册，就该把里面的过程性内容拆成 Skills。

**Q：Skill 名应该用动词还是名词？**

推荐**动作型短语**（动词或动名词）："summarize-changes"、"deploy-staging"、"review-pr"、"create-jira-ticket"——因为 `/skill-name` 调用时是命令式语境。状态型名称（"api-docs"、"team-info"）也可以，但触发率往往不如动作型——Agent 在自动激活时更容易匹配"用户想做某事"。

**Q：一个 Skill 多大算大？什么时候应该拆分？**

SKILL.md 主体一般几十到几百行没问题（不进 Discovery 阶段）。**应该拆分的信号**：1）一个 Skill 描述超过 3 个不相关任务；2）触发条件过于宽泛，激活后用户还要进一步澄清做哪一步；3）SKILL.md 超过 1000 行——这时建议拆成多个小 Skill 用 description 区分触发场景。

**Q：Skills 能引用其他 Skills 吗？**

不能直接 import，但可以在 SKILL.md 正文里说"此任务建议先调用 /other-skill"——Agent 会理解这是建议然后自己决定是否调用。这种弱组合比硬编码依赖更灵活，避免循环依赖。

**Q：Skills 的 description 能写中文吗？**

可以。description 是给 LLM 读的，中英文都能识别。但有几个细节：1）多数 AI Agent 平台的"Discovery 阶段截断长度"按 token 计算，中文 token 密度比英文高，留意别写超长；2）混合中英文 query 场景下，英文 description 触发率往往更稳定；3）你的用户是中文用户、对话多用中文 → 写中文反而精准；用户多用英文 → 写英文。

**Q：Skills 跑出错了，agent 怎么处理？**

取决于工具实现。Claude Code 默认会显示错误并把控制权交回给 Claude（让 Claude 决定要不要重试 / 改方案）；OpenClaw 与 Hermes 类似。你可以在 SKILL.md 里明确写"如果遇到 X 错误，告诉用户 Y"——给 Agent 错误处理的明确指令。

**Q：什么时候应该写 Skill，什么时候应该写 Plugin？**

Skill 是**纯 prompt + 静态资源**——不需要写代码，几分钟就能产出。Plugin 是**带可执行代码的扩展**——能注册新工具、加 MCP Server、监听事件、注入新逻辑。**简单流程 / 知识 / 模板写 Skill；需要新工具 / 新 API / 新事件机制就写 Plugin**。OpenClaw 的 Plugin 内部往往也带 Skill，是组合关系。

**Q：Skill 写好了如何分享给团队？**

团队仓库做法：把 Skill 放到项目级 `.claude/skills/` 或 OpenClaw 项目 workspace 下，commit 进 git——队友 clone 仓库后自动加载。Personal Skill 想共享：用 ClawHub（OpenClaw 生态）或 Anthropic Skills Marketplace（Claude Code）；同时遵循 agentskills.io 开放标准的 Skill 在多个工具间可以直接复用。

**Q：Skills 能传参数吗？**

可以。Claude Code 支持 `/skill-name arg1 arg2`，arguments 会被注入 Skill 上下文。OpenClaw 与 Hermes 类似——通过 slash 命令后跟参数即可。在 SKILL.md 里用占位符（如 `{{arg}}` 或自然语言"用户提供的参数"）让 Agent 知道怎么处理参数。

## 总结

写 Agent Skill 的核心路径是 "**找出你反复做的流程 → 创建 SKILL.md 文件夹 → 写好 frontmatter（name + description）和 Markdown 指令 → 测试触发是否准确 → 用 ClawHub / agentskills.io 共享给生态**"。Skills 的关键技术红利是 Progressive Disclosure 三阶段加载——你可以装很多，平时不占上下文，只在需要时进入。**写 description 比写主体更重要**——主体是 Agent 看到的指令，description 是 Agent 决定要不要看的判断依据。多家 AI Agent 工具（Claude Code、OpenClaw、Hermes Agent）共同采用 agentskills.io 开放标准，意味着同一个 SKILL.md 可以跨工具复用。

本文内容基于 agentskills.io 开放标准与 Claude Code、OpenClaw、Hermes Agent 三家官方 Skills 文档 2026 年 5 月版本整理，扩展字段与高级特性各家略有差异，建议接入前以最新官方文档为准。

**参考资料**

- Skills 列表：https://linskills.qiniu.com

- Claude Code Skills 文档：https://docs.claude.com/en/docs/claude-code/skills

- OpenClaw Creating Skills 教程：https://docs.openclaw.ai/tools/creating-skills

- Hermes Agent Skills 文档：https://hermes-agent.nousresearch.com/docs/user-guide/features/skills

- 多模型 API 聚合参考：https://www.qiniu.com/ai/models
