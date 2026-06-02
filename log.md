# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete
> When this file exceeds 500 entries, rotate: rename to log-YYYY.md, start fresh.

## [2026-06-02] create | Wiki initialized
- Domain: General knowledge base (technology, research, personal notes)
- Structure created with SCHEMA.md, index.md, log.md
- Directory layout: raw/, entities/, concepts/, comparisons/, queries/, _archive/
## [2026-06-02] config | post-commit hook enabled

## [2026-06-02] ingest | Hermes Agent LLM Wiki Skill 文档
- 来源: https://hermes-agent.nousresearch.com/docs/zh-Hans/user-guide/skills/bundled/research/research-llm-wiki
- 创建文件:
  - raw/articles/hermes-llm-wiki-skill-2026.md (原始来源)
  - concepts/llm-wiki.md (LLM Wiki 概念)
  - entities/andrej-karpathy.md (Andrej Karpathy)
  - entities/hermes-agent.md (Hermes Agent)
  - concepts/obsidian-headless.md (obsidian-headless)
  - comparisons/rag-vs-wiki.md (RAG vs Wiki 对比)
- 更新文件:
  - index.md (新增 5 个页面条目)

## [2026-06-02] ingest | Hermes Agent + Obsidian 打造第二大脑系列
- 来源: https://blog.csdn.net/sgr011215/article/details/160530313
- 创建文件:
  - raw/articles/hermes-obsidian-second-brain-2026.md (原始来源)
  - concepts/second-brain.md (第二大脑概念)
  - concepts/layered-memory-system.md (分层记忆系统)
  - entities/dify.md (Dify)
  - entities/n8n.md (n8n)
  - entities/ollama.md (Ollama)
- 更新文件:
  - entities/hermes-agent.md (补充第二大脑集成交叉引用)
  - index.md (新增 5 个页面条目)
