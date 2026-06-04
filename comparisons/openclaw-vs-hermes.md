---
title: OpenClaw vs Hermes Agent
created: 2026-06-03
updated: 2026-06-03
type: comparison
tags: [comparison, agent, llm, open-source]
sources: [raw/articles/openclaw-vs-hermes-2026.md, raw/articles/hermes-vs-openclaw-features-2026.md]
confidence: medium
---

# OpenClaw vs Hermes Agent

开源 AI Agent 领域最受关注的两条路线对比。基于 Jahangir 在 Medium 发布的文章。

## 核心分歧

| | [[OpenClaw]] | [[Hermes Agent]] |
|---|---|---|
| **核心问题** | 我能连接多少东西？ | 我能多快变得更懂你？ |
| **路线** | 平台（广度优先） | 伙伴（深度优先） |
| **比喻** | 多功能工具刀 | 专业助手 |
| **起源** | Peter Steinberger 个人实验（Clawdbot） | Nous Research 实验室 |
| **增长驱动** | 跨平台连接能力 | 持续学习用户工作方式 |
- **OpenClaw：工具链式控制**  
    它坚持“写进文件的，才存在”的文件本位主义。它把所有的身份、规则和记忆都通过透明文件定义，赋予用户对Agent行为的绝对控制权。
- **Hermes：记忆增强型学习**  
    Hermes的战略规划从一开始就是打造一个会自我进化的AI Agent。它把记忆系统作为核心，通过持续积累和反思，让Agent能自主优化。

## 详细对比

### 平台连接

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 通信入口 | Telegram, Slack, Discord, iMessage, WeChat 等 | 相对集中 |
| 模型接入 | 多模型服务商 | 与 Nous Research 模型生态紧密 |
| 社区技能 | 最大社区技能生态 | 内置 skill 体系 |
| 跨渠道体验 | 桌面→手机→其他平台无缝切换 | 侧重服务器端持续运行 |

### 学习能力

| 维度   | OpenClaw | Hermes Agent                           |
| ---- | -------- | -------------------------------------- |
| 学习机制 | 通用执行系统   | Closed Learning Loop（反思→识别模式→写入 skill） |
| 重复任务 | 每次重新执行   | 越跑越熟，自动积累经验                            |
| 长期价值 | 广度覆盖     | 复利式增长                                  |
- **OpenClaw**：不生产技能，是“技能的使用者”。所有技能都需用户人工编写或从社区下载配置。
- **Hermes**：是“技能的生产者”，其**学习闭环**是它的灵魂。完成任务后，Agent会自主评估反思，将有效的方法提炼成可复用的标准化技能（Skill）或更新到`MEMORY.md`。

### 记忆系统

| 维度   | OpenClaw         | Hermes Agent          |
| ---- | ---------------- | --------------------- |
| 存储方式 | 可见文件，一条记忆一个文件    | SQLite + FTS + LLM 摘要 |
| 透明度  | 高（用户可直接查看/编辑/删除） | 中（可通过命令查看，但不如文件直观）    |
| 维护成本 | 手动控制多            | 自动整理，省心               |
| 信任模型 | 控制权 → 信任感        | 便利性 → 省心              |

这张表格清晰地展示了它们在各个设计维度的核心差异：

| 维度           | **OpenClaw**                                                              | **Hermes**                                                                          |
| ------------ | ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **🎯 核心理念**  | 规则驱动的确定性工具链[](https://developer.baidu.com/article/detail.html?id=6743966) | 记忆驱动的自我进化型智能体[](https://developer.baidu.com/article/detail.html?id=6743966)         |
| **💾 记忆载体**  | **Markdown文件为本**<br>一切皆文件，确保完全透明                                          | **多系统分层存储**  <br>SQLite + FTS + LLM 摘要                                              |
| **🏗️ 记忆架构** | 三层分级：短期/近端/长期记忆[](https://bbs.huaweicloud.com/blogs/477076)               | 四级矩阵：核心/用户画像/长时/技能库记忆[](https://developer.baidu.com/article/detail.html?id=6963837) |
| **🔄 记忆机制**  | 手动配置+后台自动提炼(Dreaming)                                                     | 全自动学习与反思(Learning Loop)[](https://36kr.com/p/3759278143587076)                      |
| **🕵️ 检索方式** | 被动检索(原生) + 主动预加载(插件)                                                      | **原生主动检索，对话时自动注入**                                                                  |
| **🎓 技能生成**  | **完全人工编写**，依靠社区市场[](https://developer.aliyun.com/article/1725145)         | **全自动生成与进化**，经验即技能[](https://developer.baidu.com/article/detail.html?id=6973734)    |
Hermes的核心理念是**按需注入而非全量加载**：
通过维持一个稳定、精简的“热记忆”（即`USER.md`和`MEMORY.md`）来保证对话流畅和成本优化；
至于海量历史数据，就交由SQLite + FTS5引擎管理，仅在需要时检索[](https://cloud.tencent.com.cn/developer/article/2668217)。
通过LLM摘要提升检索效率和准确性，最终实现对大模型长上下文窗口的有效补充和管理。

![对比图](https://developer.qcloudimg.com/http-save/yehe-5714147/d75de06ea067d84856f1014f22439ee5.png)

### 安全

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| 已知风险 | 公开暴露实例多，被安全机构关注 | 公开报告问题较少 |
| 风险来源 | 增长速度、默认配置、开放端口 | 更年轻，审计机会少 |
| 建议 | 不要默认相信默认配置 | 同样适用 |

### 核心能力差异

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **自改进 Skill** | ❌ 无此机制 | ✅ 可复用流程保存为 SKILL.md，后续会话自动加载 |
| **持久记忆** | 文件级记忆 | SQLite + FTS + LLM 摘要，支持 Honcho/Mem0 等后端 |
| **多平台 Gateway** | 主要是 API Server 模式 | 16+ 平台（Telegram/Discord/Slack/飞书/钉钉/企业微信/Signal/Matrix/Email/Webhook） |
| **定时任务** | 需自行搭建 | 内置 cronjob 系统 |
| **定位** | "裸 Agent"，能力依赖自行搭建 | 完整 Agent 基础设施平台，开箱自带记忆/技能/多平台/定时任务 |

### 记忆架构差异

| 维度 | OpenClaw | Hermes Agent |
|------|----------|-------------|
| **记忆哲学** | 记忆就是可搜索的存储知识 | 记忆是热工作集 + 冷检索层 |
| **Prompt 记忆** | Markdown-first，日志追加式 | 冻结快照，筛选状态，~1,300 tokens |
| **历史存储** | 笔记文件混合搜索 | SQLite + FTS5 独立存储 |
| **程序性记忆** | 无独立机制 | Skills 系统 |
| **用户建模** | 无 | Honcho（可选） |
| **缓存感知** | 弱 | 强（冻结快照、延迟更新、轮次级注入） |

详见 [[Hermes 记忆架构（Memory Architecture）]]

## 适合人群

### 选 OpenClaw
- 需要尽可能多的平台连接
- 想使用最大的社区技能生态
- 偏好透明、可手动编辑的记忆
- 做跨渠道、多 agent 的自动化调度

### 选 Hermes Agent
- 看重长期复利式学习
- 工作流重复性强，希望 agent 越跑越熟
- 想在服务器上运行更轻量的 agent
- 希望 agent 与模型实验室能力紧密结合

### 两者都用
- 有足够的基础设施能力
- OpenClaw 负责"广"（调度、规划、多平台路由）
- Hermes 负责"深"（重复执行、持续优化）
- 通过 Agent Communication Protocol 协作

### 企业第二大脑场景适配

| 维度 | Hermes | OpenClaw |
|------|--------|----------|
| 长期记忆 | ★★★★★ | ★★ |
| 知识沉淀 | ★★★★★ | ★★★ |
| 自我学习 | ★★★★★ | ★★ |
| 工作流编排 | ★★★ | ★★★★★ |
| 插件生态 | ★★ | ★★★★★ |
| 企业自动化 | ★★★ | ★★★★★ |
| **第二大脑适配度** | **★★★★★** | **★★★** |

如果目标是企业第二大脑/知识库/数字员工/组织记忆，推荐 Hermes——核心价值是 **Knowledge Growth（知识成长）**。

详见 [[企业级第二大脑架构（Enterprise Second Brain）]]

## 深层意义

这场竞争不只是两个开源项目的对比，而是 **AI Agent 未来形态的分歧**：

- **OpenClaw = 平台**：连接入口，把 AI 接入各种系统、工具和渠道
- **Hermes = 伙伴**：随时间理解你、记住你、适应你的习惯

两个方向都很重要。未来的 agent 会常驻在工作系统里，记住上下文，学习流程，调用工具，并在多个场景中持续执行任务。

## 相关

- [[OpenClaw]]
- [[Hermes Agent]]
- [[第二大脑（Second Brain）]]
- [[LLM Wiki（Karpathy 模式）]]
