---
source_url: https://ruizhehou.github.io/2026/05/01/Hermes-Agent%E8%A7%A3%E6%9E%90/
ingested: 2026-06-03
sha256: c8f6be47528127cd2c4571da41c2f48198b3dcf95450064a8b1f9319c7da9b10
---

# Hermes Agent 深度解析：会自我进化的开源 AI Agent

05-01
    
  
  
  
## 
      Hermes Agent 深度解析：会自我进化的开源 AI Agent
  

  

  

大多数 AI Agent 框架都在解决同一个问题：如何让 LLM 调用工具、完成任务。但 **Hermes Agent** 想解决的是一个更难的问题：**如何让 Agent 从每次对话中学习，越用越聪明，而不是每次都从零开始**。

Hermes Agent 是 Nous Research 开源的 AI Agent 框架（MIT 协议），它最核心的设计理念是一个**闭合的学习循环**：Agent 在完成复杂任务后自动生成「技能」，技能在使用中持续改进，同时跨会话积累对用户的理解。这让它与 AutoGPT、CrewAI 等框架有本质上的不同。

## 一、核心设计理念：学习闭环

绝大多数 Agent 框架的记忆是「静态」的——你告诉它一些背景信息，它记住；你不告诉它，它不知道。Hermes 的不同之处在于它有一套**主动的、自驱的知识积累机制**。

学习闭环由四个部分构成：

- **持久记忆（Persistent Memory）**：Agent 会主动将重要信息写入 `MEMORY.md`，并在对话中定期「nudge」自己检视和更新记忆，而不是等用户主动告知

- **用户建模（User Modeling）**：集成 Honcho 的 dialectic 用户建模，跨会话逐渐建立对用户偏好、习惯、工作方式的理解——类似一个越来越了解你的助手

- **技能系统（Skills System）**：完成复杂任务后，Agent 自动将解决方案提炼成可复用的「技能」（Skill），下次遇到同类问题直接调用，不再重复推理

- **会话搜索（Session Search）**：基于 SQLite FTS5 的全文检索，配合 LLM 摘要，可以跨历史会话检索相关内容，让 Agent 能「想起」很久之前做过的事

这个设计的本质是：**把 Agent 的经验转化为可调用的程序性记忆，而不只是陈述性记忆**。一个类比：人类不只是记住「做饭的步骤」，而是练熟了之后形成肌肉记忆，不需要每次查食谱。Hermes 的技能系统就是 Agent 的「肌肉记忆」。

## 二、技能系统（Skills System）

技能是 Hermes 最有特色的设计。每个技能本质上是一个 Markdown 文件，描述了完成某类任务的步骤、注意事项和最佳实践。

## 技能的生命周期

- **自动创建**：Agent 完成一个复杂任务后，会判断这个任务值不值得抽象成技能。如果值得，自动生成技能文件

- **使用中改进**：每次调用某个技能时，如果发现步骤有误或有更好的做法，技能文件会被自动修订

- **用户触发**：在对话中用 `/skill-name` 命令可以手动触发某个技能

- **社区共享**：技能遵循 agentskills.io 开放标准，可以在社区 Skills Hub 上发布和下载

`# 查看已有技能列表
/skills

# 触发某个技能
/deploy-to-production

# 技能文件存储位置
~/.hermes/skills/
├── system/          # 内置技能
├── user/            # 用户创建的技能
└── openclaw-imports/ # 从 OpenClaw 迁移的技能
`
```

技能文件的格式是普通的 Markdown，任何人都可以手动编写和修改：

`# Deploy to Production

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
- Always deploy during low-traffic windows (avoid 9am-11am, 7pm-9pm)
- Notify #deployments channel before starting
- Use `hermes cron` to schedule off-hours deploys if needed
`
```

## 三、工具系统：40+ 内置工具

Hermes 内置了 40 多个工具，覆盖开发、系统、文件、网络等各类场景，并支持通过 MCP（Model Context Protocol）接入任意外部工具。

## 六种终端后端

Hermes 最独特的工具设计是支持**六种不同的代码执行后端**，让 Agent 可以在不同的隔离环境中运行代码：

  
    后端适用场景特点
  
  
    **Local**本机直接执行最快，无隔离
    **Docker**本地容器化执行隔离好，需要 Docker
    **SSH**远程服务器适合在云服务器上运行
    **Daytona**Serverless 开发环境闲置时自动休眠，近乎零成本
    **Singularity**HPC 集群科研计算环境
    **Modal**Serverless GPU需要 GPU 计算时按需唤醒
  

这个设计的亮点是 **Daytona 和 Modal 后端**：Agent 的执行环境可以在云端持久化，闲置时自动休眠（几乎零成本），有任务时唤醒。这意味着你可以在 Telegram 上给 Agent 发一条消息，它在云端虚拟机上工作，完成后通知你，全程不需要你的笔记本开着。

## 并行子 Agent

Hermes 支持派生隔离的子 Agent 并行处理多个工作流：

`# 主 Agent 可以同时派生多个子 Agent 处理独立任务
# 子 Agent 通过 RPC 调用工具，结果汇总回主 Agent
# 每个子 Agent 有独立的上下文窗口，不消耗主 Agent 的 context

# 也可以用 Python 脚本直接调用工具（零 context cost）
python3 

- **Telegram**：最成熟的集成，支持语音消息转录

- **Discord / Slack**：团队协作场景

- **WhatsApp / Signal**：个人消息

- **Home Assistant**：智能家居集成

- **CLI**：完整的 TUI，支持多行编辑、slash 命令自动补全、流式工具输出

所有平台共享同一份记忆和技能系统，对话历史跨平台互通。

## 五、模型无关：不锁定任何 LLM

Hermes 的另一个核心设计原则是**模型无关（Model-Agnostic）**。切换模型只需一条命令：

`hermes model
# 交互式选择：
# > Nous Portal (Hermes 3, Hermes 2 Pro...)
#   OpenRouter (200+ models)
#   NVIDIA NIM (Nemotron...)
#   OpenAI (GPT-4o, o3...)
#   Anthropic (Claude...)
#   Hugging Face
#   Custom endpoint

# 也可以直接指定
hermes config set model openrouter:anthropic/claude-opus-4
hermes config set model nous:hermes-3-70b
`
```

支持的提供商覆盖了市面上几乎所有主流模型服务：Nous Portal、OpenRouter（200+ 模型）、NVIDIA NIM、Xiaomi MiMo、Kimi/Moonshot、MiniMax、Hugging Face、OpenAI、Anthropic，以及任意自定义 OpenAI 兼容端点。

不同任务用不同模型是一个很实用的场景：日常对话用便宜的小模型，复杂代码生成切换到 Claude Opus 或 Hermes 3 70B，批量数据处理用本地部署的模型。Hermes 让这种切换毫无成本。

## 六、定时任务：用自然语言写 Cron

Hermes 内置了 Cron 调度器，可以用自然语言描述定时任务，结果推送到任意平台：

`# 每天早上 8 点生成日报，推送到 Telegram
"每天早上 8 点，整理昨天的 GitHub PR 合并情况和待处理 issue，发给我的 Telegram"

# 每周一生成周报
"每周一 9 点，分析上周的项目进展，对比 OKR，发到 Slack 的 #weekly-report 频道"

# 每晚自动备份
"每天凌晨 2 点，备份数据库到 S3，如果失败发告警邮件"
`
```

这些任务运行在云端，完全无人值守，结果通过消息平台通知你。

## 七、安全设计

给 Agent 执行命令的权限是一把双刃剑。Hermes 的安全设计：

- **命令审批（Command Approval）**：可以配置哪些命令需要人工确认才能执行，哪些可以自动放行。白名单里的 `git status`、`ls` 等只读命令自动放行，`rm -rf` 类操作需要确认

- **DM 配对（DM Pairing）**：消息平台上的 Agent 只响应授权用户，通过配对码绑定

- **容器隔离**：使用 Docker 后端时，Agent 的代码执行在隔离容器内，不影响宿主机

## 八、RL 训练集成：为下一代模型生成数据

这是 Hermes 最「研究向」的功能，也揭示了 Nous Research 的长期目标。

Hermes 内置了与 Atropos（Nous Research 的 RL 训练框架）的集成：

- **批量轨迹生成**：可以批量运行任务，收集 Agent 的完整决策轨迹（observation → action → result）

- **Atropos RL 环境**：将 Hermes 的工具环境包装成标准 RL 环境，用于强化学习训练

- **轨迹压缩**：将高质量的 Agent 轨迹压缩成训练数据，用于训练下一代 tool-calling 模型

这形成了一个完整的飞轮：**Hermes Agent 在真实任务中积累的高质量轨迹 → 训练更强的工具调用模型 → 更强的模型驱动更好的 Agent 表现**。这正是 Nous Research 作为 AI 研究机构的核心布局。

## 九、快速上手

`# 一键安装（Linux / macOS / WSL2）
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

source ~/.bashrc

# 运行配置向导
hermes setup

# 开始对话
hermes

# 常用命令
hermes model        # 切换模型
hermes tools        # 配置工具
hermes gateway      # 启动消息网关
hermes doctor       # 诊断问题
hermes update       # 更新版本
`
```

从 OpenClaw（Hermes 的前身）迁移：

`hermes claw migrate          # 迁移设置、记忆、技能、API Key
hermes claw migrate --dry-run # 预览迁移内容
`
```

## 十、与其他框架的对比

  
    
      特性
      Hermes Agent
      AutoGPT
      CrewAI
      Claude Code
    
  
  
    
      **学习闭环**
      ✅ 技能自动生成+改进
      ❌
      ❌
      ✅ CLAUDE.md
    
    
      **跨会话记忆**
      ✅ 主动积累+用户建模
      部分
      ❌
      ✅ Memory files
    
    
      **多平台接入**
      ✅ 6 个平台
      ❌
      ❌
      ❌ 仅 CLI/IDE
    
    
      **模型无关**
      ✅ 200+ 模型
      部分
      ✅
      ❌ 仅 Claude
    
    
      **Serverless 部署**
      ✅ Daytona / Modal
      ❌
      ❌
      ❌
    
    
      **RL 训练集成**
      ✅ Atropos
      ❌
      ❌
      ❌
    
    
      **定时任务**
      ✅ 内置 Cron
      ❌
      ❌
      ✅ CronCreate
    
  

## 十一、总结

Hermes Agent 的核心价值主张很清晰：**一个真正能自我成长的 Agent，而不只是一个有工具的聊天机器人**。

它的技能系统把「解决问题的方法」沉淀为可复用的程序性知识；它的用户建模让 Agent 随着时间推移越来越了解你；它的多平台接入和 Serverless 部署让 Agent 真正从「桌面工具」变成「随时可用的助手」。

对于研究者来说，Atropos 集成让它同时是一个优质的 RL 训练数据生成工具。对于开发者来说，模型无关的设计和 MCP 支持让它可以轻松嵌入已有工作流。

Hermes Agent 目前仍在快速迭代中，感兴趣可以从 GitHub 直接体验：

- GitHub：github.com/NousResearch/hermes-agent

- 文档：hermes-agent.nousresearch.com/docs

- 技能社区：agentskills.io

  

  
    
      
           AI Agent
           LLM
           NousResearch
           开源
      
      
        
        
        
      
    
  
  

  

  
    
    
    
  

  
    
      
        
- 文章目录
        
- 站点概览
      
      
      
        

- 1. 一、核心设计理念：学习闭环

- 2. 二、技能系统（Skills System）

- 3. 三、工具系统：40+ 内置工具

- 4. 四、多平台接入

- 5. 五、模型无关设计

- 6. 六、定时任务

- 7. 七、安全设计

- 8. 八、RL 训练集成

- 9. 九、快速上手

- 10. 十、与其他框架的对比

- 11. 十一、总结

      
      
      
        
          
              
侯瑞哲

              
          
          
            
              221日志
            
            
              0分类
            
            
              39标签
