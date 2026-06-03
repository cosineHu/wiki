---
source_url: user-uploaded-document
ingested: 2026-06-03
sha256: 8d788a8bad100e34ae385985d25d86b2f5f9d036bba0b8f1847efcb8b681de52
---

# Hermes vs OpenClaw — 核心差异、知识库参考、会话隔离、第二大脑

### **Hermes vs OpenClaw**

两者都属于"自主编码和任务执行 Agent"，用工具调用来操作系统。Hermes 的核心差异点：

1. **自改进 Skill 体系** — 把可复用的流程保存为 SKILL.md 文件，后续会话自动加载，越用越强。OpenClaw 没有这个机制。

2. **跨会话持久记忆** — 内置 memory 系统（还支持 Honcho、Mem0 等后端），上下文不会随会话结束丢失。

3. **多平台 Gateway** — 支持 16+ 平台（Telegram、Discord、Slack、飞书、钉钉、企业微信、Signal、Matrix、Email、Webhook 等），每个平台都有完整工具访问能力。OpenClaw 主要是 API Server 模式。

**简单说：** OpenClaw 更像一个"裸 Agent"，能力依赖你自己搭。Hermes 是一套完整的 Agent 基础设施平台，开箱自带记忆、技能、多平台、定时任务等能力。

### Hermes运行时，在会话中如何参考知识库回答？

1. **放在 AGENTS.md / MEMORY.md 里（自动注入，最省心）**
这些文件放在工作目录下，每次对话都会自动注入到上下文中。适合放精炼的核心知识（比如项目规范、常用API、决策记录）。
2. **做成 Skill（按需加载，按主题分类）**
用 skill_manage 创建知识型 skill，在识别到相关话题时会自动加载。适合按领域分块的知识（比如某个框架的用法、特定流程）。
3. **写入 Memory（每次都看，但容量有限）**
用 memory 工具存入关键事实，每轮对话都会注入。适合少量、持久、高价值的参考信息。容量约 2200 字符，不适合大段知识。
4. **Obsidian 知识库（主动搜索）**
创建相应的 Obsidian vault 目录，使用 obsidian skill 实时搜索和读取笔记。但这需要在对话中主动调用工具去查，不是自动注入的。适合知识量大、需要按需检索的场景。
**推荐组合方案：**  
- 精炼的核心知识 → 写入 `AGENT.md`（自动注入）
- 按主题分块的领域知识 → 做成 skill
- 大量笔记/文档 → 放在 Obsidian vault 里，需要时搜索

### Hermes的会话隔离机制
1. **Profile 隔离（最接近 dmScope 的概念）**  
每个 profile 有独立的 config、sessions、skills、memory。可以用 `hermes profile create` 为不同用户/场景创建完全隔离的实例。比如给不同 Telegram 群组分配不同 profile，各自互不干扰。
2. **Gateway 平台级隔离**  
Gateway 为每个平台的每个用户/群组自动创建独立 session。不同 Telegram 用户、不同 Discord 频道、不同 Slack channel 各自有独立的对话上下文，不会串。
3. **pairing（DM 授权）**  
`hermes pairing list/approve/revoke` 控制哪些用户/DM 可以与 Agent 交互，类似 dmScope 的准入控制。
4. **终端/文件隔离**  
每个 `delegate_task` 子代理有独立的终端会话和工作目录；cronjob 可以设置 `workdir` 限制工作范围。

### "第二大脑"（Second Brain ）
这个概念由 Tiago Forte 在他的书 Building a Second Brain 中提出。核心理念是：
	你的第一大脑负责思考、创造、决策
	你的第二大脑负责存储、组织、检索

Obsidian = 本地存储 + 双向链接 + 知识图谱
Hermes Agent = 智能收集 + 自动整理 + 分层记忆

[Llm Wiki — Karpathy 的 LLM Wiki：构建/查询互联 Markdown 知识库 | Hermes Agent](https://hermes-agent.nousresearch.com/docs/zh-Hans/user-guide/skills/bundled/research/research-llm-wiki)
在没有显示器的机器上，使用 `obsidian-headless` 代替桌面应用。它通过 Obsidian Sync 同步 vault，无需 GUI——非常适合在服务器上运行、向 wiki 写入内容，同时在另一台设备上用 Obsidian 桌面端读取的 Agent。

