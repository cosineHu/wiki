---
title: 飞书消息卡片与 CLI 发卡方案（Feishu Card + CLI）
created: 2026-06-05
updated: 2026-06-05
type: entity
tags: [feishu, cli, card, message, automation]
sources: [raw/2026-06-05-feishu-card-cli/]
confidence: high
---

# 飞书消息卡片与 CLI 发卡方案

飞书互动卡片（Interactive Message）的结构化消息方案，以及通过飞书 CLI（lark-cli）实现模板化、批量分发、AI 驱动的发卡工作流。

## 关键信息

| 维度 | 说明 |
|------|------|
| **来源** | 万涂幻象社区实践 |
| **工具** | lark-cli（飞书官方开源 CLI，`larksuite/cli`） |
| **卡片类型** | 互动消息（Interactive Message），v2 schema |
| **核心场景** | 单向推送 / 交互式工作流 / 动态刷新 |

## 卡片 vs 普通消息

| 维度 | 普通文本消息 | 飞书卡片 |
|------|------------|---------|
| 本质 | 一段文字 | 结构化 JSON 数据 |
| 排版 | 无 | 分栏/分块/分列 |
| 配图 | 仅附件 | 顶部横幅/内嵌图标/缩略图 |
| 按钮 | 无 | 可点击，打开 URL/提交表单/回调 |
| 表单 | 无 | 输入框/单选多选/文件上传 |
| 图表 | 无 | 柱状图/折线图/饼图 |
| 流式更新 | 不可 | 发后继续改内容 |

## 飞书 CLI 三件套

### 1. 卡片 JSON 模板（v2 schema）

```json
{
  "schema": "2.0",
  "header": {
    "template": "wathet",
    "title": {"tag": "plain_text", "content": "标题"},
    "subtitle": {"tag": "plain_text", "content": "{{DATE}}"}
  },
  "body": {
    "elements": [
      {"tag": "img", "img_key": "{{COVER_KEY}}"},
      {"tag": "column_set", "background_style": "grey", "columns": [...]},
      {"tag": "button", "type": "primary_filled", "text": {"content": "按钮"}, ...}
    ]
  }
}
```

### 2. image_key 上传

```bash
lark-cli im images create --as bot \
  --file image=@/path/to/cover.png \
  --data '{"image_type":"message"}'
```

### 3. messages-send 推送

```bash
lark-cli im +messages-send \
  --chat-id oc_xxx \
  --msg-type interactive \
  --content "$(cat card.json)" --as bot
```

## 模板变量设计模式

> 变量管"每次都要换的内容"，锚点管"品牌一致性"

| 变量 | 说明 |
|------|------|
| `{{TITLE}}` | 标题，可 AI 自动抓取 |
| `{{SUBTITLE}}` | 副标题/摘要 |
| `{{COVER_KEY}}` | 封面图 image_key |
| `{{URL}}` | 目标链接 |
| `{{DATE}}` | 日期 |
| 锚点 | header 颜色/按钮文案/品牌元素 → 写死不变 |

## 跨群分发

```bash
for CHAT in oc_群1 oc_群2 oc_群3; do
  lark-cli im +messages-send --chat-id "$CHAT" \
    --msg-type interactive --content "$(cat card.json)" --as bot
  sleep 1
done
```

## Claude Code Skill 封装

将发卡流程封装为 skill（`~/.claude/skills/`），实现自然语言触发：

```
"发公众号卡片，文章链接 https://..., 封面图 ./cover.png"
    ↓
Claude 自动：抓标题 → 上传封面 → 填变量 → dry-run 预览 → 发目标群
```

## 三类卡片场景

| 类型 | 用途 | 示例 |
|------|------|------|
| 单向推送 | 通知/预告/更新 | 公众号文章卡、直播预告卡 |
| 交互式工作流 | 审批/问卷/确认 | 审批卡、打车确认卡 |
| 动态刷新 | AI 流式回答 | 聊天机器人流式卡片 |

## 对 E3 AI 工作台的价值

本文方案直接支撑 [[e3-ai-workbench-blueprint-outline|E3 AI 工作台蓝图大纲]] 中飞书端的卡片交互设计：

| E3 AI 工作台场景 | 卡片类型 | 实现方式 |
|-----------------|---------|---------|
| 每日运营概览推送 | 单向推送 | 定时 lark-cli 发卡 |
| 库存预警通知 | 单向推送 | 事件触发 lark-cli 发卡 |
| 捡漏结果+一键确认 | 交互式工作流 | 按钮回调 |
| 预售审单+一键审核 | 交互式工作流 | 按钮回调 |
| AI 周报推送 | 单向推送 | 定时 lark-cli 发卡 |

## 相关

- [[e3-ai-workbench|E3 AI 工作台项目]]
- [[e3-ai-workbench-blueprint-outline|E3 AI 工作台业务蓝图大纲]]
- [[feishu-card-cli-analysis|飞书卡片 CLI 方案分析]]
