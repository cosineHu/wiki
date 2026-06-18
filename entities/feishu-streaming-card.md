---
title: 飞书流式更新卡片（Feishu Streaming Card — Official）
created: 2026-06-10
updated: 2026-06-10
type: entity
tags: [feishu, card, streaming, openapi, official-doc]
sources: [raw/2026-06-10-feishu-streaming-card/streaming-updates-openapi-overview.md]
confidence: high
---

# 飞书流式更新卡片（官方文档）

飞书开放平台官方文档，介绍卡片的流式更新能力——卡片内容以实时或准实时方式连续更新，实现逐步渲染效果。

## 关键信息

| 维度 | 说明 |
|------|------|
| **来源** | 飞书开放平台官方文档 |
| **URL** | `open.feishu.cn/document/cardkit-v1/streaming-updates-openapi-overview` |
| **核心能力** | 打字机效果文本输出 + 组件级局部更新 |
| **前提** | 卡片 JSON 2.0 结构 + 飞书客户端 ≥ 7.20 |

## 应用场景

| 场景 | 说明 |
|------|------|
| 场景一 | AI 大模型文本以"打字机效果"输出至飞书卡片 |
| 场景二 | 文本输出完毕后，追加交互组件（下拉选项/评价入口），用户反馈后更新组件 |

## 核心能力特性

| 特性 | 说明 |
|------|------|
| **打字机效果** | 持续向 `plain_text` 或 `markdown` 组件传入全量文本，平台自动计算增量逐字渲染 |
| **组件级局部更新** | 增加/删除/更新组件内容或属性（图表、按钮图标等） |
| **频率上限** | 10 次/秒（流式模式下不受 QPS 限制） |

## 操作步骤

### 步骤一：开启流式更新模式

**方式一：创建卡片时开启**

在卡片 JSON 的 `config` 中设置：

```json
{
  "config": {
    "streaming_mode": true,
    "streaming_config": {
      "print_frequency_ms": { "default": 70 },
      "print_step": { "default": 1 },
      "print_strategy": "fast"
    }
  }
}
```

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `print_frequency_ms` | 两次上屏间隔 | 70ms |
| `print_step` | 每次上屏增量字符数 | 1 |
| `print_strategy` | `fast`（快速上屏）/ `delay`（延迟上屏） | `fast` |

**方式二：更新已有卡片实体**

调用[更新卡片配置](https://open.feishu.cn/document/uAjLw4CM/ukTMukTMukTM/cardkit-v1/card/settings)接口，设置 `streaming_mode: true`。

### 步骤二：流式更新文本

调用[流式更新文本](https://open.feishu.cn/document/uAjLw4CM/ukTMukTMukTM/cardkit-v1/card-element/content)接口，传入全量文本。若新文本是旧文本的前缀子串，增量部分以打字机效果输出。

### 步骤三：持续更新卡片

文本输出完毕后，调用[卡片和组件接口](https://open.feishu.cn/document/uAjLw4CM/ukTMukTMukTM/cardkit-v1/feishu-card-resource-overview)进行增删改操作。

### 步骤四：处理交互回调

用户操作交互组件触发回调时，需先关闭流式模式（`streaming_mode: false`），再处理回调。

## 流式策略对比

| 策略 | 行为 |
|------|------|
| `fast`（默认） | 历史未上屏文本立即全部上屏，然后开始新内容上屏 |
| `delay` | 历史未上屏文本继续打字机输出完毕，再开始新内容上屏 |

## 注意事项

- 流式卡片无法转发（需先关闭流式模式）
- 流式模式下不支持直接响应交互回调更新卡片
- 流式模式 10 分钟后自动关闭，建议手动关闭
- 卡片实体仅支持发送一次，14 天有效期
- 需飞书客户端 ≥ 7.20

## 所需权限

- `im:message:send_as_bot` — 以应用身份发消息
- `im:message` — 获取与发送消息
- `cardkit:card:write` — 创建与更新卡片

## 相关

- [[feishu-card-overview|飞书卡片概述（官方文档）]]
- [[feishu-card-cli|飞书消息卡片与 CLI 发卡方案]]
- [[hermes-feishu-streaming-card|Hermes 飞书流式卡片]]
- [[e3-ai-workbench-blueprint-outline|E3 AI 工作台业务蓝图大纲]]
