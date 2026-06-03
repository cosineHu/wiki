---
title: "Hermes Agent + Obsidian 打造第二大脑（二）：我的踩坑经验与完整部署方案——从零到生产环境的血泪史！"
source: https://blog.csdn.net/sgr011215/article/details/160530638
author: sgr011215 (安逸 i)
date_published: 2026-04-26
date_ingested: 2026-06-03
type: article
tags: [hermes, obsidian, second-brain, deployment, docker, knowledge-base]
series: "Hermes Agent + Obsidian 打造第二大脑"
part: 2
---

# Hermes Agent + Obsidian 打造第二大脑（二）：我的踩坑经验与完整部署方案——从零到生产环境的血泪史！

> **来源**: [CSDN](https://blog.csdn.net/sgr011215/article/details/160530638) | 作者: 安逸 i | 发布: 2026-04-26

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/14801bb863b847fc90b1ba669fcb6a63.gif#pic_center)


### 1. 前言：为什么部署环境配置值得单独写 
#### 1.1 我的部署噩梦开场 

部署 Hermes Agent + Obsidian 这套系统，表面上是一个"跑 `docker compose up`"的事情，实际上涉及：


```
环境复杂度矩阵:
├── 操作系统层: macOS / Linux / Windows WSL2
├── 虚拟化层: Docker Engine / Docker Desktop
├── 数据层: PostgreSQL + Redis
├── 应用层: Hermes API + Hermes Worker + Hermes Web
├── LLM 层: OpenAI / Anthropic / 本地模型
├── 同步层: Obsidian Vault + Hermes Sync
└── 网络层: 端口映射 / 域名 / 防火墙 / 反向代理
```


任何一个环节出问题，都会导致系统无法正常运行。而这些问题往往不是"报错——百度——复制粘贴"能解决的，因为很多坑是多个因素叠加导致的。


我花了整整三周时间，踩过了你能想到和想不到的坑。以下是我整理的完整踩坑复盘和解决方案。

#### 1.2 这篇文章能帮你解决什么 

```
┌─────────────────────────────────────────────────────────────────┐
│ 本文能帮你解决 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ ✅ 内存不足导致的 OOM 问题 │
│ ✅ Docker 镜像拉取失败的国内网络困境 │
│ ✅ 端口冲突导致的服务无法启动 │
│ ✅ API Key 泄露的安全隐患 │
│ ✅ Obsidian 同步丢文件的数据安全问题 │
│ ✅ 数据库连接超时的启动顺序问题 │
│ ✅ Redis 内存溢出的配置问题 │
│ ✅ 时区配置导致的任务调度混乱 │
│ ✅ 文件锁导致的同步死锁 │
│ ✅ 网络分区后的数据不一致 │
│ │
└─────────────────────────────────────────────────────────────────┘
```

#### 1.3 技术栈全景图 

```
┌────────────────────────────────────────────────────────────────────────────┐
│ Hermes Agent 技术栈全景图 │
├────────────────────────────────────────────────────────────────────────────┤
│ │
│ ┌────────────────────────────────────────────────────────────────────┐ │
│ │ 用户交互层 │ │
│ │ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │ │
│ │ │ 浏览器 │ │ Obsidian │ │ API │ │ │
│ │ │ Web UI │ │ 客户端 │ │ 客户端 │ │ │
│ │ └─────────────┘ └─────────────┘ └─────────────┘ │ │
│ └────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌────────────────────────────────────────────────────────────────────┐ │
│ │ 反向代理层 │ │
│ │ ┌─────────────────────────────────────────────────────────────┐ │ │
│ │ │ Nginx / Caddy │ │ │
│ │ │ (HTTPS + 负载均衡 + 静态资源) │ │ │
│ │ └─────────────────────────────────────────────────────────────┘ │ │
│ └────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ▼ │
│ ┌────────────────────────────────────────────────────────────────────┐ │
│ │ 应用服务层 │ │
│ │ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │ │
│ │ │ Hermes │ │ Hermes │ │ Hermes │ │ │
│ │ │ Web │ │ API │ │ Worker │ │ │
│ │ │ :3000 │ │ :8080 │ │ (任务队列) │ │ │
│ │ └─────────────┘ └─────────────┘ └─────────────┘ │ │
│ └────────────────────────────────────────────────────────────────────┘ │
│ │ │
│ ┌─────────────────────────┼─────────────────────────┐ │
│ │ │ │ │
│ ▼ ▼ ▼ │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ LLM │ │ PostgreSQL │ │ Redis │ │
│ │ Provider │ │ 数据库 │ │ 缓存/队列 │ │
│ │ OpenAI/Ant │ │ :5432 │ │ :6379 │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ │
│ │ │
│ ▼ │
│ ┌────────────────────────────────────────────────────────────────────┐ │
│ │ 持久化层 │ │
│ │ ┌─────────────────────────────────────────────────────────────┐ │ │
│ │ │ Obsidian Vault │ │ │
│ │ │ (Markdown 文件系统) │ │ │
│ │ └─────────────────────────────────────────────────────────────┘ │ │
│ └────────────────────────────────────────────────────────────────────┘ │
│ │
│ ┌────────────────────────────────────────────────────────────────────┐ │
│ │ 基础设施层 │ │
│ │ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │ │
│ │ │ Docker │ │ Linux │ │ Systemd │ │ │
│ │ │ Engine │ │ Kernel │ │ Init │ │ │
│ │ └─────────────┘ └─────────────┘ └─────────────┘ │ │
│ └────────────────────────────────────────────────────────────────────┘ │
│ │
└────────────────────────────────────────────────────────────────────────────┘
```


---

### 2. 我的踩坑经历全记录 
#### 2.1 坑一：内存不够导致 OOM 

**症状表现**：


```
容器启动后：
├── 页面加载极慢（超过 30 秒）
├── API 响应超时
├── Docker 日志显示: "worker killed: out of memory"
├── 系统整体变卡，其他应用受影响
└── dmesg 显示: "Out of memory: Kill process xxx score xxx"
```


**我的经历**：


我一开始用的是 8GB 内存的 VPS。Hermes Agent 的组件包括：


- PostgreSQL（建议 512MB+，实际峰值 1GB+）
- Redis（建议 256MB+，实际峰值 512MB+）
- Hermes API（建议 512MB+，实际峰值 1GB+）
- Hermes Worker（建议 256MB+，实际峰值 512MB+）
- Hermes Web（建议 256MB+，实际峰值 512MB+）


总计：约 2GB+ 基础运行，但实际运行时因为：


- 并发请求
- 大文本处理
- LLM API 响应的巨大 JSON
- 搜索索引构建


峰值可能到 4-6GB。8GB 内存的 VPS 在其他服务（SSH、Nginx 等）运行的情况下，内存直接爆炸。


**问题分析流程图**：


```
┌─────────────────────────────────────────────────────────────────┐
│ OOM 问题排查流程 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ 1. 检查系统内存使用 │
│ ├─> free -h │
│ └─> cat /proc/meminfo │
│ │
│ 2. 检查 Docker 容器内存使用 │
│ ├─> docker stats │
│ └─> docker compose ps │
│ │
│ 3. 检查容器日志 │
│ ├─> docker compose logs api | grep -i memory │
│ └─> dmesg | grep -i oom │
│ │
│ 4. 分析内存使用构成 │
│ ├─> docker compose exec api top │
│ └─> docker compose exec postgres ps aux | sort -rn -k 3 │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**解决方案**：


```yaml
# docker-compose.yml 添加资源限制
services:
api:
deploy:
resources:
limits:
memory: 1G # 硬限制
reservations:
memory: 512M # 预留内存
# 其他配置...

worker:
deploy:
resources:
limits:
memory: 512M
reservations:
memory: 256M
# 其他配置...

postgres:
deploy:
resources:
limits:
memory: 1G
reservations:
memory: 512M
# 其他配置...

redis:
deploy:
resources:
limits:
memory: 512M
reservations:
memory: 256M
# 其他配置...
```


**根本解决方案**：

方案内存适用场景成本升级到 16GB VPS16GB生产环境¥100-200/月Apple Silicon Mac16-64GB开发/小团队¥10,000-30,000 一次性增加 SWAP+8GB临时应急免费精简组件4GB尝鲜体验免费

**增加 SWAP 应急方案**：


```bash
# 检查当前 SWAP
swapon -s

# 创建 8GB SWAP 文件
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# 永久启用
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# 优化 SWAP 使用（降低 swappiness）
sysctl vm.swappiness=10
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
```

#### 2.2 坑二：Docker 镜像拉取失败 

**症状表现**：


```bash
$ docker compose pull
Using default tag: latest
Error response from daemon: Get "https://registry-1.docker.io/v2/": 
dial tcp: lookup registry-1.docker.io: server misbehaving
```


或者：


```bash
Error response from daemon: 
head https://registry-1.docker.io/v2/hermes/api/manifests/latest: 
net/http: request canceled while waiting for connection
(Client.Timeout exceeded while awaiting headers)
```


**原因分析**：


```
┌─────────────────────────────────────────────────────────────────┐
│ Docker Hub 访问问题 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ 国内用户 ──✗──> registry-1.docker.io (超时/失败) │
│ │ │
│ ├── DNS 污染 │
│ ├── 链路不稳定 │
│ ├── 限流 (429 Too Many Requests) │
│ └── 被墙 │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**解决方案一：配置国内镜像加速**：


```bash
# 创建 /etc/docker/daemon.json
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json ~/.docker/config.json hermes-api.tar.gz

# 传输到目标机器
scp hermes-api.tar.gz user@target-server:~/

# 在目标机器导入
docker load 端口服务常见来源检测命令3000Hermes WebApache, Airplay`lsof -i :3000`8080Hermes APITomcat, Java`lsof -i :8080`5432PostgreSQL本地 PostgreSQL`lsof -i :5432`6379Redis本地 Redis`lsof -i :6379`27017MongoDB本地 MongoDB`lsof -i :27017`

**排查流程图**：


```
┌─────────────────────────────────────────────────────────────────┐
│ 端口冲突排查流程 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ $ lsof -i :3000 │
│ COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME │
│ httpd 1234 root 4u IPv4 0t0 TCP *:3000 (LISTEN)│
│ │
│ 1. 确认进程来源 │
│ ├─> ps aux | grep 1234 │
│ └─> ps -ef | grep httpd │
│ │
│ 2. 决策 │
│ ├─> 停止冲突服务（如本地 PostgreSQL） │
│ ├─> 改 Docker 映射端口（推荐） │
│ └─> 卸载冲突软件（不推荐） │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**解决方案：修改 docker-compose.yml 端口**：


```yaml
services:
web:
ports:
- "3001:3000" # 外部 3001 -> 容器 3000
- "3002:3001" # 第二个实例

api:
ports:
- "8081:8080" # 外部 8081 -> 容器 8080

postgres:
ports:
- "5433:5432" # 外部 5433 -> 容器 5432

redis:
ports:
- "6380:6379" # 外部 6380 -> 容器 6379
```

#### 2.4 坑四：API Key 泄露 

**症状表现**：


```bash
# 收到 OpenAI 账单（异常高额）
OpenAI API - $150.00
Usage in current period: 2,847,800 tokens
# 你的预期: 最多 $20
```


**原因分析**：


```
GitHub 搜索 "OPENAI_API_KEY=sk-":
https://github.com/search?q=OPENAI_API_KEY%3Dsk-&type=code

结果: Thousands of exposed keys
每年因此损失数百万美元
```


**立即止损**：


```bash
# 1. 立即在 OpenAI Console 重新生成 Key
# 访问: https://platform.openai.com/api-keys
# 点击 "Create new secret key" 
# 旧的 Key 立即失效

# 2. 从 Git 历史中清除（如果已提交）
git filter-branch --force --index-filter \
'git rm --cached --ignore-unmatch .env' \
--prune-empty --tag-name-filter cat -- --all

# 或者使用 git filter-repo（推荐，更快）
# pip install git-filter-repo
# git filter-repo --path .env --invert-paths

# 3. 强制推送到远程
git push origin --force --all
git push origin --force --tags

# 4. 确保 .gitignore 包含 .env
echo ".env" >> .gitignore
echo ".env.local" >> .gitignore
echo ".env.*.local" >> .gitignore
echo "*.log" >> .gitignore
git add .gitignore
git commit -m "Add .env to gitignore"
git push origin main
```


**预防措施**：


```bash
# .gitignore 完整内容
cat > .gitignore 同时编辑同一文件 │
│ ├─> 冲突时直接覆盖（无备份） │
│ └─> 没有版本控制 │
│ │
│ 2. 同步时机问题 │
│ ├─> 写入一半就被同步 │
│ ├─> 文件锁未正确释放 │
│ └─> 网络中断导致的部分写入 │
│ │
│ 3. 权限问题 │
│ ├─> 容器内用户与宿主机用户不一致 │
│ └─> 文件权限被错误修改 │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**解决方案**：


```yaml
# docker-compose.yml 添加冲突处理
hermes:
environment:
# 同步策略
SYNC_STRATEGY: "keep_both" # 保留双方（推荐）
# SYNC_STRATEGY: "newest_wins" # 最新优先（可能丢数据）
# SYNC_STRATEGY: "manual" # 手动处理（最安全但麻烦）

# 冲突文件保留时间
CONFLICT_BACKUP_DAYS: 30

# 备份配置
BACKUP_BEFORE_OVERWRITE: "true"
MAX_BACKUPS_PER_FILE: 5

# 同步间隔（秒）
SYNC_INTERVAL: 30

# 文件完整性检查
ENABLE_CHECKSUM: "true"
```


**Obsidian 端配置**：


```json
// 在 vault 根目录创建 .obsidian/sync-config.json
{
"conflictStrategy": "keepBoth",
"backupBeforeOverwrite": true,
"maxBackupsPerFile": 5,
"syncInterval": 30,
"enableVersionControl": true,
"ignorePatterns": [
".obsidian/**",
".git/**",
"*.tmp",
"*.swp",
".trash/**"
],
"fileHashAlgorithm": "sha256"
}
```

#### 2.6 坑六：数据库连接超时 

**症状表现**：


```bash
# API 日志
Error: connection refused: postgres:5432
FATAL: the database system is starting up

# 或者
Error: could not connect to server: Connection timed out
Is the server running on host "postgres" and accepting
TCP/IP connections on port 5432?

# 或者
Error: password authentication failed for user "hermes"
```


**排查流程图**：


```
┌─────────────────────────────────────────────────────────────────┐
│ 数据库连接问题排查流程 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ Step 1: 检查容器状态 │
│ $ docker compose ps postgres │
│ → 如果 Status 不是 "Up"，检查日志 │
│ │
│ Step 2: 检查网络连通性 │
│ $ docker compose exec api ping postgres │
│ $ docker compose exec api nc -zv postgres 5432 │
│ │
│ Step 3: 检查认证配置 │
│ $ docker compose logs postgres | grep -i "password" │
│ → 确认 POSTGRES_USER/POSTGRES_PASSWORD 正确 │
│ │
│ Step 4: 检查连接字符串 │
│ $ docker compose exec api env | grep DATABASE_URL │
│ → 格式: postgresql://user:password@host:port/dbname │
│ │
│ Step 5: 测试手动连接 │
│ $ docker compose exec postgres psql -U hermes -d hermes │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**完整解决方案**：


```yaml
# docker-compose.yml 添加 depends_on 和健康检查
services:
api:
depends_on:
postgres:
condition: service_healthy # 等待 postgres 健康后再启动
redis:
condition: service_healthy # 等待 redis 健康后再启动
environment:
- DATABASE_URL=postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@postgres:5432/${POSTGRES_DB}
- DB_POOL_SIZE=20
- DB_POOL_TIMEOUT=30
- DB_CONNECTION_TIMEOUT=10

postgres:
healthcheck:
test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER} -d ${POSTGRES_DB}"]
interval: 5s
timeout: 5s
retries: 10
start_period: 30s
environment:
- POSTGRES_DB=${POSTGRES_DB}
- POSTGRES_USER=${POSTGRES_USER}
- POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
- POSTGRES_INITDB_ARGS="--encoding=UTF8 --locale=C"
command: >
postgres
-c max_connections=200
-c shared_buffers=256MB
-c effective_cache_size=512MB

redis:
healthcheck:
test: ["CMD", "redis-cli", "-a", "${REDIS_PASSWORD}", "ping"]
interval: 5s
timeout: 3s
retries: 5
```

#### 2.7 坑七：Redis 内存溢出 

**症状表现**：


```bash
# Redis 日志
# WARNING: Memory overcommit is enabled
# WARNING: You have a memory limit on your container

# 或者
# MISCONF Redis is configured to save RDB snapshots, but is unable to it.
# 原因是磁盘空间不足或 fork 失败

# 或者
# OOM in VM builder while computing the memory map
```


**排查命令**：


```bash
# 查看 Redis 内存使用
docker compose exec redis redis-cli -a $REDIS_PASSWORD INFO memory

# 查看 key 数量
docker compose exec redis redis-cli -a $REDIS_PASSWORD DBSIZE

# 查看大 key
docker compose exec redis redis-cli -a $REDIS_PASSWORD --bigkeys

# 查看内存碎片
docker compose exec redis redis-cli -a $REDIS_PASSWORD INFO memory | grep mem_fragmentation
```


**完整解决方案**：


```yaml
redis:
image: redis:7-alpine
container_name: hermes-redis
restart: unless-stopped
command: >
redis-server
--requirepass ${REDIS_PASSWORD}
--maxmemory 512mb
--maxmemory-policy allkeys-lru
--maxmemory-samples 5
--save ""
--appendonly no
--tcp-backlog 511
--timeout 0
--tcp-keepalive 300
--lazyfree-lazy-eviction yes
--lazyfree-lazy-expansion yes
volumes:
- redis-data:/data
deploy:
resources:
limits:
memory: 512M
```

#### 2.8 坑八：时区配置导致的任务调度混乱 

**症状表现**：


- 定时备份在错误的时间执行
- 显示的时间和实际时间差 8 小时
- Cron 任务不按预期执行
- 日志时间与系统时间不一致


**原因分析**：


```
┌─────────────────────────────────────────────────────────────────┐
│ 时区问题示意 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ 服务器时区: UTC │
│ 用户预期: Asia/Shanghai (UTC+8) │
│ │
│ 定时任务配置: 每天 03:00 执行 │
│ │
│ 实际情况: │
│ ├─> UTC 03:00 = 北京时间 11:00 │
│ └─> 用户以为 03:00（北京）但实际 UTC 03:00 │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**解决方案**：


```yaml
# docker-compose.yml 统一时区配置
services:
api:
environment:
- TZ=Asia/Shanghai
- TIMEZONE=Asia/Shanghai

worker:
environment:
- TZ=Asia/Shanghai
- TIMEZONE=Asia/Shanghai

postgres:
environment:
- TZ=Asia/Shanghai
- PGTZ=Asia/Shanghai

redis:
# Redis 不支持时区，但日志时间会受容器时区影响

# docker-compose.yml 挂载时区文件
services:
api:
volumes:
- /etc/localtime:/etc/localtime:ro
- /etc/timezone:/etc/timezone:ro
```


```bash
# 宿主机确保时区正确
# Ubuntu/Debian
sudo timedatectl set-timezone Asia/Shanghai

# macOS
sudo systemsetup -settimezone Asia/Shanghai

# 验证
timedatectl
# Local time: Sat 2026-04-26 17:30:00 CST
# Universal time: Sat 2026-04-26 09:30:00 UTC
# RTC time: Sat 2026-04-26 09:30:00
# Time zone: Asia/Shanghai (CST, +0800)
```

#### 2.9 坑九：文件锁导致的同步死锁 

**症状表现**：


- 笔记文件无法保存
- Obsidian 显示 “File is currently open”
- Hermes 日志显示 “Could not acquire lock”
- 强制关闭后文件损坏


**原因分析**：


```
┌─────────────────────────────────────────────────────────────────┐
│ 文件锁死锁场景 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ 场景1: Obsidian 正在保存 + Hermes 正在同步 │
│ ┌─────────┐ ┌─────────┐ │
│ │ Obsidian│ │ Hermes │ │
│ │ 写入 │ │ 读取 │ │
│ └────┬────┘ └────┬────┘ │
│ │ │ │
│ ▼ ▼ │
│ ┌─────────┐ ┌─────────┐ │
│ │ 创建锁 │ │ 等待锁 │ │
│ │ 写入中 │ │ 超时 │ │
│ └─────────┘ └─────────┘ │
│ │
│ 场景2: 多实例 Hermes 同时同步 │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Worker 1 │ │Worker 2 │ │Worker 3 │ │
│ │ 写入A │ │ 写入B │ │ 读取A │ │
│ └────┬────┘ └────┬────┘ └────┬────┘ │
│ │ │ │ │
│ └────────────────┴────────────────┘ │
│ 文件冲突 │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**解决方案**：


```yaml
# docker-compose.yml
hermes:
environment:
# 文件锁配置
ENABLE_FILE_LOCK: "true"
LOCK_TIMEOUT: 30
LOCK_RETRY_INTERVAL: 1

# 分布式锁（使用 Redis）
USE_DISTRIBUTED_LOCK: "true"
LOCK_KEY_PREFIX: "hermes:file_lock:"

# 乐观锁（不阻塞，检测冲突）
USE_OPTIMISTIC_LOCK: "false"

# 冲突检测
ENABLE_CONFLICT_DETECTION: "true"
CONFLICT_RESOLUTION: "keep_both"
```


```python
# 文件锁实现示例（Python）
import fcntl
import time
from contextlib import contextmanager

@contextmanager
def file_lock(path, timeout=30, retry_interval=1):
"""文件锁上下文管理器"""
lock_path = f"{path}.lock"
start_time = time.time()

while True:
try:
with open(lock_path, 'w') as f:
fcntl.flock(f.fileno(), fcntl.LOCK_EX | fcntl.LOCK_NB)
f.write(str(os.getpid()))
f.flush()
yield
return
except BlockingIOError:
if time.time() - start_time >= timeout:
raise TimeoutError(f"Could not acquire lock for {path}")
time.sleep(retry_interval)
finally:
try:
fcntl.flock(f.fileno(), fcntl.LOCK_UN)
os.unlink(lock_path)
except:
pass
```

#### 2.10 坑十：网络分区后的数据不一致 

**症状表现**：


- 断网恢复后部分笔记丢失
- 同步显示成功但实际未同步
- 数据库和 Vault 内容不一致
- 出现重复的笔记文件


**原因分析**：


```
┌─────────────────────────────────────────────────────────────────┐
│ 网络分区场景分析 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ 正常情况: │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Obsidian│───▶│ Hermes │───▶│ Postgres│ │
│ │ Vault │ │ API │ │ DB │ │
│ └─────────┘ └─────────┘ └─────────┘ │
│ │
│ 网络分区时: │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Obsidian│───▶│ Hermes │ ✗───▶ │ Postgres│ (断开) │
│ │ Vault │ │ API │ │ DB │ │
│ └─────────┘ └─────────┘ └─────────┘ │
│ │ │ │
│ │ 本地缓存 │
│ │ │ │
│ └──────────────┘ │
│ │
│ 恢复后: │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Obsidian│───▶│ Hermes │───▶│ Postgres│ │
│ │ Vault │ │ API │ │ DB │ │
│ └─────────┘ └─────────┘ └─────────┘ │
│ │ │ │ │
│ └──────────────┴──────────────┘ │
│ 需要合并冲突 │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**解决方案**：


```yaml
# docker-compose.yml
hermes:
environment:
# 离线队列配置
OFFLINE_QUEUE_ENABLED: "true"
OFFLINE_QUEUE_MAX_SIZE: 1000
OFFLINE_QUEUE_TTL: 86400 # 24小时

# 冲突解决策略
CONFLICT_RESOLUTION: "keep_both"
CONFLICT_MERGE_STRATEGY: "three_way_merge" # 三方合并

# 一致性校验
ENABLE_CONSISTENCY_CHECK: "true"
CONSISTENCY_CHECK_INTERVAL: 3600 # 每小时

# 最终一致性保证
SYNC_VERIFICATION: "true"
MAX_SYNC_RETRIES: 3
```


---

### 3. 完整环境准备 
#### 3.1 硬件配置要求与选型指南 

**配置要求对比表**：

配置级别CPU内存存储网络适用场景成本估算入门2 核4GB20GB SSD10Mbps尝鲜/开发¥30-50/月最低4 核8GB50GB SSD50Mbps个人使用¥80-150/月推荐8 核16GB100GB SSD100Mbps小团队/生产¥200-400/月最佳16 核32GB500GB NVMe500Mbps企业/重度使用¥500-1000/月

**我的实际配置**：


```
┌─────────────────────────────────────────────────────────────────┐
│ 我的配置方案 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ MacBook Pro M2 Pro 16GB（主力开发机） │
│ ├─ 统一内存架构 │
│ ├─ Docker Desktop + Kubernetes │
│ ├─ 内网: 1000Mbps │
│ └─ 外网: 100Mbps │
│ │
│ 外接 Samsung T7 1TB SSD（Obsidian Vault） │
│ ├─ USB 3.2 Gen 2 │
│ ├─ 文件级加密 │
│ └─ Time Machine 备份 │
│ │
│ 阿里云 ECS 8核16G 100G SSD（生产服务器） │
│ ├─ 2-3 台用于高可用 │
│ └─ 快照备份 │
│ │
└─────────────────────────────────────────────────────────────────┘
```


**Apple Silicon vs Intel vs 云服务器对比**：

维度Apple SiliconIntel Mac/PC云服务器性能原生支持，效率极高需要 Rosetta 翻译取决于实例类型内存统一内存，无上限受限于物理内存可弹性扩展功耗极低（<30W）高（100W+）按需计费便携性笔记本形态台式/笔记本无（远程）成本（长期）一次性投入一次性投入持续付费初始化开箱即用配置繁琐快速创建适用场景开发/小规模生产开发/小规模生产大规模/高可用
#### 3.2 macOS 环境配置详解 

**Homebrew 安装与配置**：


```bash
# 安装 Homebrew（如果没装）
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 配置 Homebrew 镜像（国内用户必需）
# 替换 brew.git
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# 替换 core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

# 替换 cask.git（GUI 应用）
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-cask"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git

# 刷新
brew update
```


**核心依赖安装**：


```bash
# Docker Desktop（必需）
brew install --cask docker

# Git（必需）
brew install git

# Node.js（用于前端开发、pnpm）
brew install node

# Python（用于脚本）
brew install python@3.11

# Watchman（Obsidian 插件需要）
brew install watchman

# 其他工具
brew install jq yq httpie tmux zsh-autosuggestions
```


**Docker Desktop 配置**（macOS 必需）：


```json
// ~/Library/Application Support/docker/docker-desktop/settings.json
{
"diskSpaceMB": 256000,
"memoryMB": 8192,
"cpus": 4,

"displayedOnboarding": true,
"fileSharingAndVolumes": [
"/Users/anyi/Obsidian Vault"
],
"kubernetesEnabled": false,
"registry-mirrors": [
"https://docker.mirrors.ustc.edu.cn"
]
}
```


**验证安装**：


```bash
# Docker
docker --version
# Docker version 26.1.1, build 4cf5a9b
docker compose version
# Docker Compose version v2.24.0

# Git
git --version
# git version 2.44.0

# Node.js
node --version
# v20.14.0
npm --version
# 10.7.0

# Python
python3 --version
# Python 3.11.9

# Watchman
watchman --version
# 2024.04.22.0
```

#### 3.3 Linux 环境配置 Ubuntu 22.04 

**系统更新与基础工具**：


```bash
# 更新系统
sudo apt update && sudo apt upgrade -y

# 安装基础工具
sudo apt install -y curl wget git build-essential
sudo apt install -y unzip zip htop atop net-tools
sudo apt install -y nfs-common cifs-utils

# 安装 Docker
curl -fsSL https://get.docker.com | sudo sh

# 添加当前用户到 docker 组（免 sudo）
sudo usermod -aG docker $USER
newgrp docker

# 启用 Docker 服务
sudo systemctl enable docker
sudo systemctl start docker

# 验证
docker --version
docker compose version
```


**Docker Compose V2 安装**：


```bash
# 方法1: apt 安装
sudo apt install docker-compose-v

# 方法2: 手动安装最新版
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# 验证
docker compose version
```


**Node.js 20 安装**：


```bash
# 使用 NodeSource 安装
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# 验证
node --version
npm --version
```


**Python 3.11 安装**：


```bash
sudo apt install -y python3.11 python3-pip python3-venv
sudo apt install -y python3.11-dev python3.11-venv

# 创建软链接
sudo ln -sf /usr/bin/python3.11 /usr/bin/python3

# 验证
python3 --version
pip3 --version
```

#### 3.4 Windows WSL2 配置 

**安装 WSL2**：


```powershell
# 管理员 PowerShell
wsl --install -d Ubuntu-22.04

# 重启后进入 WSL
wsl -d Ubuntu-22.04

# 设置默认用户
ubuntu2204 config --default-user your_username
```


**WSL2 内安装 Docker**：


```bash
# 更新
sudo apt update && sudo apt upgrade -y

# 安装 Docker
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER

# 安装 Docker Compose
sudo apt install docker-compose-v

# 重要: 在 WSL2 中使用 Docker Desktop
# Windows 上需要安装 Docker Desktop 并启用 WSL2 integration
```


**Docker Desktop WSL2 集成配置**：


```json
// %USERPROFILE%\.docker\daemon.json
{
"features": {
"general": true
},
"wslEngineVersions": {
"distro": "Ubuntu-22.04"
},
"registry-mirrors": [
"https://docker.mirrors.ustc.edu.cn"
],
"builder": {
"gc": {
"enabled": true,
"defaultKeepStorage": "20GB"
}
}
}
```


**WSL2 内存限制调整**：


```powershell
# 创建 .wslconfig 文件
notepad $env:USERPROFILE\.wslconfig
```


```ini
[wsl2]
memory=8GB
processors=4
swap=4GB
localhostForwarding=true
nestedVirtualization=true
```


```powershell
# 重启 WSL
wsl --shutdown
```

#### 3.5 云服务器部署方案 

**阿里云 ECS 部署**：

配置项推荐值说明实例规格ecs.g7.large2核4G 起操作系统Ubuntu 22.04 LTS推荐存储40GB SSD 起系统盘网络公网带宽 5Mbps按量计费更划算安全组开放 22/80/443SSH + Web

```bash
# 连接服务器
ssh root@your-server-ip

# 执行环境配置（参考 Ubuntu 22.04 部分）

# 配置防火墙
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

# 安装 Docker
curl -fsSL https://get.docker.com | sh
```


**腾讯云 CVM 部署**：


```bash
# 连接服务器
ssh root@your-server-ip

# 配置镜像源（国内必需）
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

# 配置 Docker 镜像
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json 组件职责依赖Web用户界面，处理前端请求APIAPI业务逻辑，REST APIPostgreSQL, Redis, LLMWorker异步任务处理，搜索索引构建PostgreSQL, Redis, VaultPostgreSQL主数据存储-Redis缓存、队列、分布式锁-VaultObsidian 文件系统-
#### 4.2 创建项目目录 

```bash
# 创建项目目录
mkdir -p ~/hermes-agent
cd ~/hermes-agent

# 创建数据目录
mkdir -p data/vault
mkdir -p data/postgres
mkdir -p data/redis

# 设置权限（Docker 需要写入）
chmod -R 755 data/

# 创建日志目录
mkdir -p logs
```

#### 4.3 获取源码 

```bash
# 如果有 Git 仓库
git clone https://your-repo/hermes-agent.git .

# 如果是官方仓库
git clone https://github.com/hermes-agent/hermes-agent.git .

# 查看目录结构
ls -la
```


**完整目录结构**：


```
hermes-agent/
├── docker-compose.yml # 容器编排配置
├── .env # 环境变量（不提交 Git）
├── .env.example # 环境变量模板
├── data/ # 数据目录
│ ├── vault/ # Obsidian Vault 挂载点
│ ├── postgres/ # PostgreSQL 数据
│ └── redis/ # Redis 数据
├── logs/ # 日志目录
├── backups/ # 备份目录
├── scripts/ # 运维脚本
│ ├── backup.sh # 备份脚本
│ ├── restore.sh # 恢复脚本
│ └── healthcheck.sh # 健康检查脚本
├── README.md # 项目说明
└── CHANGELOG.md # 变更日志
```

#### 4.4 配置文件深度解析 

**创建 `.env` 文件**：


```bash
cat > .env 
postgres
-c max_connections=200
-c shared_buffers=256MB
-c effective_cache_size=512MB
-c maintenance_work_mem=128MB
-c checkpoint_completion_target=0.9
-c wal_buffers=16MB
-c default_statistics_target=100
-c random_page_cost=1.1
-c effective_io_concurrency=200
-c wal_compression=on
-c ptrack_pages_per_page=4

# ============================================
# Redis 缓存
# ============================================
redis:
image: redis:7-alpine
container_name: hermes-redis
restart: unless-stopped
command: >
redis-server
--requirepass ${REDIS_PASSWORD}
--maxmemory 512mb
--maxmemory-policy allkeys-lru
--maxmemory-samples 5
--save ""
--appendonly no
--tcp-backlog 511
--timeout 0
--tcp-keepalive 300
--hz 10
--lazyfree-lazy-eviction yes
--lazyfree-lazy-expansion yes
volumes:
- redis-data:/data
networks:
- hermes-network
healthcheck:
test: ["CMD", "redis-cli", "-a", "${REDIS_PASSWORD}", "ping"]
interval: 10s
timeout: 3s
retries: 5
deploy:
resources:
limits:
memory: 512M
reservations:
memory: 256M
logging:
driver: "json-file"
options:
max-size: "10m"
max-file: "3"

# ============================================
# 网络配置
# ============================================
networks:
hermes-network:
driver: bridge
ipam:
config:
- subnet: 172.28.0.0/16

# ============================================
# 持久化存储
# ============================================
volumes:
postgres-data:
driver: local
redis-data:
driver: local
vault-data:
driver: local
```

#### 4.6 启动服务与验证 

**启动服务**：


```bash
# 拉取镜像（国内建议配置镜像加速）
docker compose pull

# 后台启动
docker compose up -d

# 查看状态
docker compose ps

# 查看日志（实时）
docker compose logs -f

# 查看特定服务日志
docker compose logs -f api
docker compose logs -f worker
docker compose logs -f postgres
```


**预期输出**：


```
$ docker compose ps
NAME STATUS PORTS
hermes-web Up (healthy) 0.0.0.0:3000->3000/tcp
hermes-api Up (healthy) 0.0.0.0:8080->8080/tcp
hermes-worker Up 0.0.0.0:8080
hermes-postgres Up (healthy) 5432/tcp
hermes-redis Up (healthy) 6379/tcp
```


**健康检查**：


```bash
# API 健康检查
curl http://localhost:8080/health

# 预期输出
{
"status": "ok",
"version": "1.0.0",
"uptime": 1234,
"services": {
"database": "connected",
"redis": "connected"
}
}

# Web UI 健康检查
curl http://localhost:3000/health

# 预期输出
{"status": "ok"}
```


**访问 Web UI**：


```
浏览器打开: http://localhost:3000
```


首次访问会要求创建管理员账号。


**API 文档**：


```
访问: http://localhost:8080/api/docs
```


---

### 5. Obsidian 配置 
#### 5.1 创建 Obsidian 库 

```bash
# 创建 Obsidian 库目录
mkdir -p ~/Obsidian\ Vault

# 创建示例文件夹结构
cd ~/Obsidian\ Vault

# 创建主要分类
mkdir -p "0_临时" "1_日志" "2_项目" "3_知识" "4_归档"
mkdir -p "1_日志/每日" "1_日志/周报" "1_日志/月报"
mkdir -p "2_项目/进行中" "2_项目/已完成" "2_项目/已归档"
mkdir -p "3_知识/技术" "3_知识/产品" "3_知识/运营"

# 创建模板目录
mkdir -p .obsidian templates

# 初始化 Git（强烈推荐）
git init
git add .
git commit -m "Initial commit"
```

#### 5.2 安装 Obsidian 插件 

**必须安装的插件**：

插件名称用途安装方式Templater模板系统社区插件搜索Dataview数据查询社区插件搜索QuickAdd快速创建社区插件搜索Local REST APIAPI 访问社区插件搜索

**推荐安装的插件**：

插件名称用途Minimal Theme极简主题Style Settings主题自定义Auto Link Title自动补全链接标题Tag Wrangler标签管理Git版本控制
#### 5.3 配置 Hermes Sync 插件 

**通过 BRAT 安装**：


- 安装 BRAT 插件（社区插件搜索）
- 启用 BRAT
- 添加仓库：`https://github.com/hermes-agent/obsidian-sync`
- 点击 “Add and enable plugin”


**配置 Hermes Sync**：


```json
// .obsidian/plugins/hermes-sync/config.json
{
"vaultPath": "/data/vault",
"syncInterval": 30,
"direction": "bidirectional",
"conflictStrategy": "keepBoth",
"ignorePatterns": [
".obsidian/**",
".git/**",
"*.tmp",
"*.swp",
".trash/**"
],
"enableFileLock": true,
"lockTimeout": 30,
"enableChecksum": true
}
```


---

### 6. 常见问题排查 
#### 6.1 服务启动后无法访问 Web UI 

**排查步骤**：


```bash
# 1. 检查容器状态
docker compose ps

# 2. 查看 web 服务日志
docker compose logs web

# 3. 检查端口占用
lsof -i :3000

# 4. 检查网络连接
docker compose exec web curl -v http://api:8080/health

# 5. 检查容器健康状态
docker inspect hermes-web | grep -A 10 "Health"
```

#### 6.2 API 返回 401 Unauthorized 

**排查步骤**：


```bash
# 1. 检查 .env 配置
grep -E "JWT_SECRET|API_SECRET_KEY" .env

# 2. 验证配置是否注入到容器
docker compose exec api env | grep -E "JWT|API"

# 3. 重新生成密钥
openssl rand -base64 32 # 新 JWT_SECRET
openssl rand -hex 32 # 新 API_SECRET_KEY
```

#### 6.3 笔记同步后内容丢失 

**排查步骤**：


```bash
# 1. 检查是否有冲突文件
ls -la data/vault/*conflict*

# 2. 查看同步日志
docker compose logs worker | grep -i sync

# 3. 检查文件权限
ls -la data/vault/

# 4. 检查 Git 历史
cd ~/Obsidian\ Vault
git log --oneline -10
```

#### 6.4 数据库连接失败 

**排查步骤**：


```bash
# 1. 检查 PostgreSQL 状态
docker compose ps postgres
docker compose logs postgres

# 2. 测试连接
docker compose exec api nc -zv postgres 5432

# 3. 检查 DATABASE_URL 格式
docker compose exec api env | grep DATABASE_URL
```

#### 6.5 LLM API 调用失败 

**排查步骤**：


```bash
# 1. 检查 API Key
docker compose exec api env | grep -E "OPENAI|ANTHROPIC"

# 2. 测试 API Key 有效性
curl https://api.openai.com/v1/models \
-H "Authorization: Bearer $OPENAI_API_KEY"

# 3. 查看 API 日志
docker compose logs api | grep -i "openai\|anthropic\|error"
```

#### 6.6 Worker 任务队列阻塞 

**排查步骤**：


```bash
# 1. 查看队列状态
docker compose exec redis redis-cli -a $REDIS_PASSWORD LLEN hermes_tasks

# 2. 查看 worker 日志
docker compose logs worker | tail -100

# 3. 重启 worker
docker compose restart worker
```

#### 6.7 磁盘空间不足 

**排查命令**：


```bash
# 检查磁盘空间
df -h

# 检查 Docker 占用
docker system df

# 清理未使用的资源
docker system prune -a
docker volume prune
```


---

### 7. 性能优化 
#### 7.1 Docker 资源限制 

```yaml
# docker-compose.yml 添加资源限制
services:
api:
deploy:
resources:
limits:
memory: 2G
cpus: '2'
reservations:
memory: 1G
cpus: '1'
```

#### 7.2 PostgreSQL 深度优化 

```yaml
postgres:
command: >
postgres
-c max_connections=200
-c shared_buffers=256MB
-c effective_cache_size=512MB
-c maintenance_work_mem=128MB
-c checkpoint_completion_target=0.9
-c wal_buffers=16MB
-c default_statistics_target=100
-c random_page_cost=1.1
-c effective_io_concurrency=200
-c wal_compression=on
```

#### 7.3 Redis 深度优化 

```yaml
redis:
command: >
redis-server
--requirepass ${REDIS_PASSWORD}
--maxmemory 512mb
--maxmemory-policy allkeys-lru
--tcp-backlog 511
--timeout 0
--tcp-keepalive 300
```

#### 7.4 系统级优化 

```bash
# Linux: 增加文件描述符限制
echo "* soft nofile 65536" | sudo tee -a /etc/security/limits.conf
echo "* hard nofile 65536" | sudo tee -a /etc/security/limits.conf

# Linux: 优化内核参数
sudo sysctl -w vm.swappiness=10
sudo sysctl -w vm.dirty_ratio=15
sudo sysctl -w vm.dirty_background_ratio=5
```

#### 7.5 应用层优化 

```yaml
# API 服务优化
api:
environment:
- DB_POOL_SIZE=20
- DB_POOL_TIMEOUT=30
- WORKER_CONCURRENCY=10
- CACHE_TTL=3600
```


---

### 8. 备份策略与灾难恢复 
#### 8.1 三层备份架构 

```
┌─────────────────────────────────────────────────────────────────┐
│ 三层备份架构图 │
├─────────────────────────────────────────────────────────────────┤
│ │
│ 第一层: 实时同步 │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Obsidian│ ──▶ │Hermes │ ──▶ │Git Repo │ │
│ │ Vault │ │ Server │ │ (每日) │ │
│ └─────────┘ └─────────┘ └─────────┘ │
│ │
│ 第二层: 定时备份 │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Postgres │ ──▶ │ NAS │ ──▶ │ 云存储 │ │
│ │ DB │ │ (本地) │ │ (异地) │ │
│ └─────────┘ └─────────┘ └─────────┘ │
│ │
│ 第三层: 快照备份 │
│ ┌─────────┐ ┌─────────┐ │
│ │云服务器│ ──▶ │快照服务│ │
│ │ VPS │ │ (每日) │ │
│ └─────────┘ └─────────┘ │
│ │
└─────────────────────────────────────────────────────────────────┘
```

#### 8.2 Obsidian 笔记备份 

```bash
# Git 备份（适合有 Git 仓库的用户）
cd ~/Obsidian\ Vault
git init
git add .
git commit -m "Initial commit"

# 添加远程仓库
git remote add origin https://github.com/your-username/hermes-kb.git
git push -u origin main

# 自动化提交
cat > ~/Scripts/git-backup.sh backup_$(date +%Y%m%d).sql

# 备份 Redis
docker compose exec redis redis-cli -a $REDIS_PASSWORD SAVE
docker cp $(docker compose ps -q redis):/data/dump.rdb ./redis_backup_$(date +%Y%m%d).rdb

# 备份整个 vault
tar -czf vault_backup_$(date +%Y%m%d).tar.gz data/vault/
```

#### 8.4 定时备份脚本 

```bash
cat > ~/Scripts/backup-hermes.sh /dev/null || true
git push origin main 2>/dev/null || true

# 备份 PostgreSQL
docker compose exec postgres pg_dump -U ${POSTGRES_USER} ${POSTGRES_DB} > $BACKUP_DIR/postgres_$DATE.sql

# 备份 Redis
docker compose exec redis redis-cli -a $REDIS_PASSWORD SAVE 2>/dev/null
docker cp $(docker compose ps -q redis):/data/dump.rdb $BACKUP_DIR/redis_$DATE.rdb 2>/dev/null || true

# 清理旧备份
find $BACKUP_DIR -name "*.sql" -mtime +$RETENTION_DAYS -delete
find $BACKUP_DIR -name "*.rdb" -mtime +$RETENTION_DAYS -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +$RETENTION_DAYS -delete

echo "Backup completed: $DATE"
EOF

chmod +x ~/Scripts/backup-hermes.sh

# 添加到 crontab（每天凌晨 3 点执行）
(crontab -l 2>/dev/null; echo "0 3 * * * ~/Scripts/backup-hermes.sh >> ~/logs/backup.log 2>&1") | crontab -
```

#### 8.5 灾难恢复演练 

```bash
# 恢复步骤
# 1. 恢复 PostgreSQL
docker compose exec -T postgres psql -U ${POSTGRES_USER} ${POSTGRES_DB} 

# 常见原因：
# - 配置错误（.env 格式）
# - 端口占用
# - 权限问题
# - 依赖服务未就绪
```


**Q2: 如何查看容器资源使用？**


```bash
docker stats

# 输出示例：
# CONTAINER ID NAME CPU % MEM USAGE / LIMIT MEM %
# abc123 hermes-api 2.34% 512MiB / 1GiB 50.0%
```


**Q3: 如何更新到新版本？**


```bash
# 拉取最新镜像
docker compose pull

# 重启服务
docker compose up -d

# 如果有数据库迁移
docker compose run --rm api migrate
```


**Q4: 如何完全卸载？**


```bash
# 停止服务
docker compose down

# 删除数据（谨慎！）
docker compose down -v

# 删除镜像
docker compose down --rmi all
```


**Q5: 如何查看容器内部进程？**


```bash
# 进入容器 bash
docker compose exec api /bin/bash

# 进入 PostgreSQL
docker compose exec postgres psql -U hermes -d hermes

# 进入 Redis
docker compose exec redis redis-cli -a $REDIS_PASSWORD
```


**Q6: 如何修改已运行容器的配置？**


```bash
# 修改 .env 文件后，重启服务
docker compose up -d

# 如果需要重新创建容器
docker compose up -d --force-recreate

# 只重启特定服务
docker compose restart api
```

#### 11.2 性能相关问题 

**Q7: 系统变慢怎么办？**


```bash
# 1. 检查资源使用
docker stats

# 2. 检查磁盘 IO
docker compose exec postgres iostat -x 1 5

# 3. 检查网络
docker network inspect hermes-network

# 4. 查看慢查询日志
docker compose exec postgres cat /var/log/postgresql/postgresql.log
```


**Q8: 如何优化 PostgreSQL 性能？**


```sql
-- 查看慢查询
SELECT query, calls, mean_time 
FROM pg_stat_statements 
ORDER BY mean_time DESC 
LIMIT 10;

-- 查看索引使用情况
SELECT indexrelname, idx_scan, idx_tup_read 
FROM pg_stat_user_indexes 
ORDER BY idx_scan ASC;

-- 查看缓存命中率
SELECT 
sum(heap_blks_read) as heap_read,
sum(heap_blks_hit) as heap_hit,
sum(heap_blks_hit) / (sum(heap_blks_hit) + sum(heap_blks_read)) as ratio
FROM pg_statio_user_tables;
```


**Q9: Redis 内存使用率过高怎么办？**


```bash
# 查看 Redis 内存详情
docker compose exec redis redis-cli -a $REDIS_PASSWORD INFO memory

# 查看大 key
docker compose exec redis redis-cli -a $REDIS_PASSWORD --bigkeys

# 查看 key 过期情况
docker compose exec redis redis-cli -a $REDIS_PASSWORD --scan | head -100 | while read key; do
ttl=$(docker compose exec redis redis-cli -a $REDIS_PASSWORD TTL "$key")
if [ "$ttl" -eq -1 ]; then
echo "Key without TTL: $key"
fi
done
```


**Q10: 如何减少 Docker 磁盘占用？**


```bash
# 清理未使用的镜像、容器、网络
docker system prune -a

# 清理构建缓存
docker builder prune -a

# 清理未使用的卷
docker volume prune

# 查看占用
docker system df
```

#### 11.3 数据安全相关问题 

**Q11: 如何防止数据丢失？**


- 开启 Git 自动提交
- 配置定时备份
- 使用 `keep_both` 同步策略
- 定期测试恢复流程


**Q12: API Key 泄露了怎么办？**


- 立即在官网重新生成 Key
- 从 Git 历史中清除
- 检查是否有异常使用
- 启用 API 密钥轮换


**Q13: 如何加密敏感文件？**


```bash
# 使用 gpg 加密
gpg --symmetric --cipher-algo AES256 .env

# 解密
gpg --decrypt .env.gpg > .env
```


**Q14: Obsidian Vault 被误删如何恢复？**


```bash
# 如果有 Git
cd ~/Obsidian\ Vault
git checkout -- .

# 如果有备份
tar -xzf vault_backup_20260426.tar.gz

# 如果有快照（云服务器）
# 在云控制台找到快照，创建新磁盘或恢复
```

#### 11.4 网络相关问题 

**Q15: 国内拉取 Docker 镜像太慢怎么办？**


配置国内镜像加速：


```bash
sudo tee /etc/docker/daemon.json 场景最佳实践优先级内存配置至少 16GB，避免 OOM🔴 必须Docker 镜像配置国内镜像加速🔴 必须端口管理使用非标准端口，避免冲突🟡 推荐安全API Key 不提交 Git，定期轮换🔴 必须同步策略keep_both 避免数据丢失🔴 必须备份Git + 定时自动备份🔴 必须健康检查配置 healthcheck 避免启动竞态🟡 推荐资源限制设置内存限制，防止系统崩溃🟡 推荐时区统一配置 Asia/Shanghai🟡 推荐日志配置日志轮转，防止磁盘爆满🟢 建议防火墙仅开放必要端口🔴 必须容器安全使用非 root 用户，限制权限🟡 推荐密钥管理使用 Docker Secrets🟢 建议监控配置资源监控和告警🟢 建议文档记录部署配置和变更🟢 建议

---

### 13. 参考文献 

- **Docker 官方文档** - https://docs.docker.com/
- **PostgreSQL 官方文档** - https://www.postgresql.org/docs/
- **Redis 官方文档** - https://redis.io/documentation
- **Docker Compose 文件参考** - https://docs.docker.com/compose/compose-file/
- **Hermes Agent GitHub 仓库** - https://github.com/hermes-agent/hermes-agent
- **Obsidian 官方文档** - https://help.obsidian.md/
- **Nginx 入门与实战** - 《 Nginx 高性能 Web 服务器详解》
- **Docker 安全最佳实践** - https://docs.docker.com/engine/security/
- **PostgreSQL 性能优化指南** - https://www.postgresql.org/docs/current/performance-tips.html
- **Redis 内存优化** - https://redis.io/docs/management/optimization/


---


- 
- 


$(function() {
setTimeout(function () {
var mathcodeList = document.querySelectorAll('.htmledit_views img.mathcode');
if (mathcodeList.length > 0) {
for (let i = 0; i ');
curSpan.text(alt);
$(mathcodeList[i]).before(curSpan);
$(mathcodeList[i]).remove();
}
} else {
mathcodeList[i].onerror = function() {
var alt = mathcodeList[i].alt;
alt = '\\(' + alt + '\\)';
var curSpan = $('');
curSpan.text(alt);
$(mathcodeList[i]).before(curSpan);
$(mathcodeList[i]).remove();
};
}
}
MathJax.Hub.Queue(["Typeset",MathJax.Hub]);
}
}, 500)
});


关注博主即可阅读全文
![](https://csdnimg.cn/release/blogv2/dist/pc/img/arrowDownAttend.png)


![](https://csdnimg.cn/release/blogv2/dist/pc/img/vip-limited-close-newWhite.png)


确定要放弃本次机会？


福利倒计时


:

:


![](https://csdnimg.cn/release/blogv2/dist/pc/img/vip-limited-close-roup.png)
立减 ¥


普通VIP年卡可用


[立即使用](https://mall.csdn.net/vip)


<div class="
