# OpenClaw Config

**Official workspace template for [OpenClaw](https://openclaw.ai) pro instances, powered by [Fuel](https://openclaw.rocks/fuel).**

Clone this repo, add your Fuel virtual key, and you have a production-ready workspace with optimized inference, context management, and cost controls — no config to figure out.

This template combines best practices from the [OpenClaw Token Optimization Guide](https://docs.google.com/document/d/1ffmZEfT7aenfAz2lkjyHsQIlYRWFpGcM/edit?tab=t.0) by [@mattganzak](https://github.com/mattganzak) and the [OpenClaw Runbook](https://github.com/digitalknk/openclaw-runbook) by [@digitalknk](https://github.com/digitalknk). The key difference: this config uses **Fuel** as the model provider, so you get semantic role routing (worker/reasoning/heartbeat) with automatic cheapest-provider selection instead of managing provider keys and model IDs directly.

## Quick Start

```bash
# 1. Clone the template
git clone https://github.com/openclaw-rocks/openclaw-config.git my-workspace
cd my-workspace

# 2. Replace the placeholder with your Fuel virtual key
#    Get one at https://openclaw.rocks/fuel
sed -i '' 's/<YOUR_FUEL_VK>/vk-your-key-here/' openclaw.json

# 3. Customize your agent
#    Edit SOUL.md (personality), USER.md (your context)

# 4. Run your agent
#    Point your OpenClaw instance at this directory
```

## What's in each file

| File | Purpose |
|------|---------|
| `openclaw.json` | Main config — Fuel models, concurrency limits, context pruning, compaction, caching |
| `SOUL.md` | Agent personality and principles (template — customize this) |
| `USER.md` | Your context: name, timezone, mission (template — customize this) |
| `AGENTS.md` | Shared agent instructions: session initialization, memory rules, 402 handling |
| `HEARTBEAT.md` | Heartbeat check definitions (system health, task status) |
| `memory/` | Memory directory for compaction flush (auto-populated) |
| `.gitignore` | Ignores sessions, logs, credentials |

## What this config saves you

| Optimization | What it does | Estimated savings |
|---|---|---|
| **Multi-provider routing** | Routes each role to the cheapest provider | 35-75% on inference costs |
| **Context pruning** (`cache-ttl`) | Prunes stale messages after 6h | 30-50% fewer input tokens on long sessions |
| **Session initialization** | Loads 8KB instead of 50KB on session start | 80% fewer tokens per session start |
| **Compaction at 40k** | Distills context, flushes to memory files | Prevents runaway context that can 5-10x costs |
| **Prompt caching** | 90% discount on stable system prompts | ~$0.01/session instead of ~$0.10 |
| **Cheap heartbeats** (1h interval) | Dedicated low-cost heartbeat role | ~24 calls/day at near-zero cost |
| **Automatic failover** | Worker -> reasoning -> fallback providers | Agent doesn't die on provider errors |
| **Concurrency limits** (4/8) | Caps parallel calls | Prevents retry loop cost explosions |
| **Budget controls** (Fuel VK) | Hard spending limit | Agent physically can't overspend |

**Typical result:** An autonomous agent running 8+ hours/day costs **$0.30-1.00/day** with Fuel vs **$3-5/day** with default config.

## How Fuel works

Your agent asks for `worker`, `reasoning`, or `heartbeat`. Fuel resolves that to the best real model at the cheapest provider and rewrites both request and response. Your agent never sees a provider-specific model ID.

| Role | Cost (per M tokens) | Use case |
|---|---|---|
| `fuel/worker` | ~$0.67 input / ~$2.02 output | Routine coding, subagents, file operations |
| `fuel/reasoning` | ~$0.72 input / ~$3.60 output | Architecture, complex reasoning, research |
| `fuel/heartbeat` | ~$0.06 input / ~$0.24 output | Heartbeats, status checks |

Costs are approximate — we continuously optimize which providers and models back each role. Current model details are always visible at [openclaw.rocks/fuel](https://openclaw.rocks/fuel).

## Region filtering

By default, Fuel routes to the best model globally. If you have data sovereignty or compliance requirements, change the `baseUrl` in `openclaw.json`:

```
https://inference.openclaw.rocks/v1              # Default: best globally
https://inference.openclaw.rocks/v1/~eu-eu       # GDPR: EU origin + EU providers
https://inference.openclaw.rocks/v1/~all-us      # Any origin, US providers only
https://inference.openclaw.rocks/v1/~us-us       # US origin + US providers
```

**Two dimensions:**
- **Provider region** (after the `-`): Where the API is physically hosted. `eu` = data only goes to EU-hosted APIs.
- **Model origin** (before the `-`): Where the model company is based. `eu` = only models from EU companies.

Every region filter has full role coverage — no gaps:

| Filter | Worker | Reasoning | Heartbeat |
|--------|--------|-----------|-----------|
| `~all-all` (default) | DeepSeek V3.1 @ Fireworks | Kimi K2.5 @ Fireworks | GPT-OSS 20B @ Fireworks |
| `~all-us` | DeepSeek V3.1 @ Fireworks | Kimi K2.5 @ Fireworks | GPT-OSS 20B @ Fireworks |
| `~us-us` | Llama 4 Maverick @ Together AI | gpt-oss-120b @ Together AI | GPT-OSS 20B @ Fireworks |
| `~eu-eu` | Devstral 2 (123B) @ Mistral | Magistral Medium @ Mistral | Mistral Small 3.2 @ OVHcloud |

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `401 Unauthorized` | Check your virtual key in `openclaw.json`. It should start with `vk-`. |
| `402 Budget Exceeded` | Credits exhausted. Top up at [openclaw.rocks/fuel](https://openclaw.rocks/fuel). |
| `429 Too Many Requests` | Hit rate limits. Wait a moment or check concurrency settings. |
| Agent not using Fuel | Verify model IDs start with `fuel/` in your config. |
| Context growing too fast | Verify `contextPruning` is set in `openclaw.json`. Check AGENTS.md session init rules. |
| Still loading full history | Session init rules missing from agent prompt. See AGENTS.md. |
| Heartbeats too expensive | Verify `heartbeat.model` points to `fuel/heartbeat`. |

## Credits

This template builds on the work of:

- **[@mattganzak](https://github.com/mattganzak)** — [OpenClaw Token Optimization Guide](https://docs.google.com/document/d/1ffmZEfT7aenfAz2lkjyHsQIlYRWFpGcM/edit?tab=t.0): session initialization, context pruning, rate limiting, model routing patterns
- **[@digitalknk](https://github.com/digitalknk)** — [OpenClaw Runbook](https://github.com/digitalknk/openclaw-runbook): compaction, memory management, heartbeat tuning, security defaults

## Links

- [Fuel managed service](https://openclaw.rocks/fuel) — get your virtual key
- [Fuel repo](https://github.com/openclaw-rocks/fuel) — proxy source, model matrix, skill
- [OpenClaw](https://openclaw.ai) — your AI agent, live in seconds

## License

MIT
