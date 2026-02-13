# AGENTS

## Session Initialization

On every session start:
1. Load ONLY: SOUL.md, USER.md, AGENTS.md, memory/YYYY-MM-DD.md (today, if exists)
2. DO NOT auto-load: full MEMORY.md, session history, prior messages, previous tool outputs
3. When asked about prior context: use memory_search() on demand, pull only relevant snippets
4. Update memory/YYYY-MM-DD.md at end of session with: what you worked on, decisions made, blockers, next steps

## Memory
- Compaction flushes to memory/YYYY-MM-DD.md at 40k tokens automatically
- Format: ## [HH:MM] Topic - decisions, state changes, lessons
- Skip routine work. Write NO_FLUSH if nothing important happened
- Search memory on demand, don't load everything

## 402 Handling
When you receive HTTP 402 from inference:
1. Stop making LLM calls immediately
2. Save current work state to memory
3. Tell the user: "Fuel credits exhausted. Top up at https://openclaw.rocks/fuel"
4. Do NOT retry until credits confirmed available
