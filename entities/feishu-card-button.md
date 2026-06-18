---
title: 飞书卡片按钮组件（Feishu Card Button Component — Official）
created: 2026-06-10
updated: 2026-06-10
type: entity
tags: [feishu, card, component, button, official-doc]
sources: [raw/2026-06-10-feishu-card-components/button.md]
confidence: high
---

# 飞书卡片按钮组件（官方文档）

飞书开放平台官方文档，按钮组件的 JSON 2.0 结构、属性说明、回调示例。

## 关键信息

| 维度 | 说明 |
|------|------|
| **来源** | 飞书开放平台官方文档 |
| **组件标签** | `"tag": "button"` |
| **版本** | JSON 2.0（不支持旧版 `action` 模块） |

## 按钮类型（type）

| 类型 | 效果 |
|------|------|
| `default` | 黑色字体，有边框 |
| `primary` | 蓝色字体，有边框 |
| `danger` | 红色字体，有边框 |
| `text` | 黑色字体，无边框 |
| `primary_text` | 蓝色字体，无边框 |
| `danger_text` | 红色字体，无边框 |
| `primary_filled` | 蓝底白字 |
| `danger_filled` | 红底白字 |
| `laser` | 镭射按钮 |

## 按钮尺寸（size）

| 尺寸 | PC 端 | 移动端 |
|------|-------|--------|
| `tiny` | 24px | 28px |
| `small` | 28px | 28px |
| `medium` | 32px | 36px |
| `large` | 40px | 48px |

## 核心属性

| 属性 | 说明 |
|------|------|
| `text` | 按钮文本，最多 100 字符 |
| `icon` | 前缀图标（`standard_icon` 图标库 / `custom_icon` 自定义图片） |
| `behaviors` | 交互行为：`open_url`（跳转链接）/ `callback`（回传数据） |
| `confirm` | 二次确认弹窗配置 |
| `disabled` | 是否禁用 |
| `hover_tips` | PC 端悬浮提示 |
| `width` | `default` / `fill` / 自定义 px |

## 交互行为（behaviors）

```json
{
  "behaviors": [
    {
      "type": "open_url",
      "default_url": "https://...",
      "android_url": "...",
      "ios_url": "...",
      "pc_url": "..."
    },
    {
      "type": "callback",
      "value": { "key": "value" }
    }
  ]
}
```

支持同时配置跳转链接和回传交互。

## 回调结构

用户点击按钮后触发 `card.action.trigger` 回调：

```json
{
  "event": {
    "action": {
      "tag": "button",
      "value": { "key_1": "value_1" }
    },
    "token": "c-295e...",
    "context": {
      "open_message_id": "om_...",
      "open_chat_id": "oc_..."
    }
  }
}
```

`token` 有效期 30 分钟，最多可更新卡片 2 次。

## 对 E3 AI 工作台的价值

按钮组件直接支撑蓝图大纲中的交互卡片：

| 场景 | 按钮类型 | 行为 |
|------|---------|------|
| 捡漏确认 | `primary_filled` | callback 回传确认 |
| 预售审核 | `primary_filled` | callback 回传审核 |
| 查看详情 | `default` | open_url 跳转 E3OMS |
| 取消操作 | `danger` | callback 回传取消 |
| 一键操作 | `laser` | callback 回传执行 |

## 相关

- [[feishu-card-overview|飞书卡片概述]]
- [[feishu-streaming-card|飞书流式更新卡片]]
- [[feishu-card-cli|飞书 CLI 发卡方案]]
- [[e3-ai-workbench-blueprint-outline|E3 AI 工作台蓝图大纲]]
