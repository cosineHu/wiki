---
title: 内容生产流水线（Content Production Pipeline）
created: 2026-06-08
updated: 2026-06-08
type: concept
tags: [knowledge-base, llm, automation, content-creation]
sources: [raw/articles/knowledge-management-llm-skills-2026.md]
confidence: high
---

# 内容生产流水线

老章（公众号「老章」）提出的知识管理下游——将结构化知识高效转化为多形态内容产品的自动化流水线。与 [[karpathy-knowledge-base-method]] 的上游积累形成互补。

## 定位

在 [[knowledge-management-pipeline]] 五环模型中，内容生产流水线占据「生产」环节：

```
积累（Karpathy）→ 消费（Lex）→ 守护（kepano）→ 生产（老章）→ 反哺积累
```

Karpathy 解决的是「大量散碎信息怎么变成结构化知识」，老章解决的是「有了知识和选题，怎么高效变成文章、视频、社交内容」。

## 核心架构

```
素材（Clippings/） → Skills 执行层（58 个 Skill） → 多形态输出
```

### 目录结构

```
zhangAI/
├── 1-Wechat/                     # 公众号文章（成品库）
│   ├── Archive/                  # 已发布文章
│   └── ing/                      # 正在写的稿子
├── .agent/skills/                # 58 个 AI Skill（执行层）
│   ├── 1-write_tech_article_pro/ # 技术文章撰写
│   ├── 1-title_generator/        # 标题生成（3种风格）
│   ├── 1-video-script-converter/ # 文章→口播稿
│   ├── 1-doubao-tts-voice-clone/ # 口播稿→克隆语音
│   ├── audio-to-video/           # 语音→竖版短视频
│   ├── 1-upload-images-to-picgo/ # 图片→个人图床
│   ├── design_svg_card/          # 知识卡片设计
│   └── ...（还有 50 多个）
├── Video/                        # 视频产物
│   ├── 口播稿/
│   ├── 口播音频/
│   └── 成品视频/
├── Clippings/                    # Karpathy 说的 raw/ 目录
├── CLAUDE.md                     # AI Agent 的「操作手册」
├── AGENTS.md                     # Codex 的上下文配置
└── GEMINI.md                     # Gemini 的指令文件
```

## 典型工作流

一条命令「帮我写篇技术文章」触发全流程：

| 步骤 | Skill | 输入 → 输出 | 耗时 |
|------|-------|------------|------|
| 1 | write_tech_article_pro | 选题/素材 → Markdown 成品文章 | ~3min |
| 2 | video-script-converter | 文章 → 口播稿 | ~1min |
| 3 | doubao-tts-voice-clone | 口播稿 → 克隆语音音频 | ~2min |
| 4 | audio-to-video | 音频 → 竖版短视频 | ~2min |

**全程约 8 分钟**，传统方式需要 2-3 小时。

## 与 Karpathy 系统的关系

| 维度 | Karpathy 系统 | 老章系统 |
|------|-------------|---------|
| 定位 | 知识上游（积累） | 知识下游（生产） |
| 流程 | raw/ → wiki/ → query | 素材 → skills/ → 文章 → 视频 |
| 核心能力 | 编译、索引、检索 | 写作、转换、分发 |
| 输出 | 结构化知识库 | 多形态内容产品 |
| 关系 | 上游输入 | 下游输出 |

两者是上下游互补关系，不是竞争。

## 溯源标记方案

与 [[kepano]] 的物理隔离不同，老章使用溯源标记来管理人机边界：

- YAML frontmatter 的 author 字段
- 文件路径本身（如 Video/口播稿/ 一看就知道是 AI 转的）
- Skill 的执行日志

详见 [[ai-human-knowledge-boundary]]。

## IPO 建模

| 维度 | 内容 |
|------|------|
| **Input** | 选题方向 + 素材库（Clippings/）+ 结构化知识（来自上游 Karpathy 系统） |
| **Process** | ① 技术文章撰写（write_tech_article_pro） → ② 标题生成（title_generator，3 种风格） → ③ 文章→口播稿转换（video-script-converter） → ④ 口播稿→克隆语音（doubao-tts-voice-clone） → ⑤ 语音→竖版短视频（audio-to-video） → ⑥ 图片上传图床（upload-images-to-picgo） → ⑦ 知识卡片设计（design_svg_card） |
| **Output** | Markdown 文章 + 口播稿 + 克隆语音音频 + 竖版短视频 + 社交媒体帖子 + 知识卡片 |
| **Tools** | Claude Code / AI Agent, 58 个 Skills, 豆包 TTS, 视频生成工具, PicGo 图床 |
| **Quality Check** | 文章是否通过标题生成器的 A/B 测试？口播稿是否自然流畅？视频是否适配竖版？全流程是否在 10 分钟内完成？ |

## 相关

- [[knowledge-management-pipeline]]
- [[karpathy-knowledge-base-method]]
- [[ai-human-knowledge-boundary]]
- [[hermes-skills-system]]
- [[skills-authoring-guide]]
- [[second-brain]]
