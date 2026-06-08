---
title: obsidian-headless
created: 2026-06-02
updated: 2026-06-08
type: concept
tags: [devops, tool, sync]
sources: [raw/articles/hermes-llm-wiki-skill-2026.md]
confidence: high
---

# obsidian-headless

Obsidian 的无头（headless）模式工具，用于在无显示器的服务器上通过 Obsidian Sync 同步 vault。

## 用途

适合 Agent 在服务器上向 wiki 写入内容，同时在另一台设备上用 Obsidian 桌面端/移动端读取的场景。

## 前提条件

- Node.js 22+
- Obsidian 账号 + Sync 订阅（**付费**）

## 基本用法

```bash
npm install -g obsidian-headless
ob login --email <email> --password '<password>'
ob sync-create-remote --name "LLM Wiki"
cd ~/wiki
ob sync-setup --vault "<vault-id>"
ob sync --continuous
```

## systemd 后台运行

```ini
[Unit]
Description=Obsidian LLM Wiki Sync
After=network-online.target

[Service]
ExecStart=/path/to/ob sync --continuous
WorkingDirectory=/home/user/wiki
Restart=on-failure
RestartSec=10

[Install]
WantedBy=default.target
```

启用 linger 使同步在登出后继续：
```bash
sudo loginctl enable-linger $USER
```

## 免费替代方案

Obsidian Sync 需付费订阅。免费替代：
- **Git + Obsidian Git 插件**：通过 GitHub 私有仓库同步
- **Syncthing**：P2P 实时同步
- **rsync + cron**：定时 SSH 同步

## IPO 建模

| 阶段 | 内容 |
|------|------|
| **Input** | 需要在无显示器服务器上同步 Obsidian vault 的需求 |
| **Process** | ① 安装 Node.js 22+ 和 obsidian-headless → ② ob login 登录 Obsidian 账号 → ③ ob sync-create-remote 创建远程 vault → ④ ob sync-setup 关联本地目录 → ⑤ ob sync --continuous 持续同步（或配置 systemd 后台运行） |
| **Output** | 服务器端 vault 与 Obsidian 桌面端/移动端实时同步 |
| **Tools** | obsidian-headless (npm), systemd, Obsidian Sync（付费） |
| **Quality Check** | 同步是否持续运行（systemd status）？免费替代方案（Git/Syncthing/rsync）是否更适合当前场景？ |

## 相关

- [[LLM Wiki（Karpathy 模式）]]
- [[Hermes Agent]]
- [[Obsidian]]
- [[双向链接（Obsidian）]]
- [[Obsidian 标签系统与 Frontmatter]]
- [[Obsidian Dataview 查询]]
