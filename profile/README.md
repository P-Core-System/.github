# P-Core System

Autonomous, self-healing AI systems built on a shared brain and orchestration core — maintained by Peter.

## Projects

| Project | Description | Language |
|---------|-------------|----------|
| [pcore-orchestra](https://github.com/P-Core-System/pcore-orchestra) | Ambient multi-agent orchestration for Cursor IDE and OpenCode CLI — plan → implement → verify → review | JavaScript |
| [pcore-brain](https://github.com/P-Core-System/pcore-brain) | Reusable AI brain client — opencode serve bridge with auto model pools per task/agent | Python |
| [pcore-webai](https://github.com/P-Core-System/pcore-webai) | Multi-provider LLM web-to-API gateway — Gemini & ChatGPT sessions as OpenAI-style APIs | Python |
| [pcore-trader](https://github.com/P-Core-System/pcore-trader) | Automated crypto trading bot — signals, futures, margin, monitor, learn, ops panel | Python |
| [pcore-assistant](https://github.com/P-Core-System/pcore-assistant) | AI-powered Telegram chat assistant — English/Burmese offline message handling | JavaScript |
| [pcore-vpn](https://github.com/P-Core-System/pcore-vpn) | P Core-VPN — Xray multi-protocol proxy panel with reseller & brain integration (active fork of 3x-ui) | Go | Go |
| [pcore-n8n](https://github.com/P-Core-System/pcore-n8n) | Self-hosted n8n workflow automation - n8n 2.34.5 + Python/JS task runners on the core node | TypeScript |

## Meta

| Project | Description |
|---------|-------------|
| [.github](https://github.com/P-Core-System/.github) | Org profile, community health files, and reusable CI workflows |

## Satellite repos

Maintained under [@peterlianpi](https://github.com/peterlianpi):

| Repo | Description |
|------|-------------|
| [P-Core-System](https://github.com/peterlianpi/P-Core-System) | Monorepo — `p-core-backend`, `p-core-system`, `p-core-mobile`, zolai-dashboard plugin |
| [pcore-real-estate](https://github.com/peterlianpi/pcore-real-estate) | Listings CRM — real estate platform (Laravel + Inertia) |

## Automation policy

To stay within GitHub free-tier minutes:

- **No scheduled workflows** — all former daily/weekly crons (stale bot, mutation testing, cache cleanup, health scans, CodeQL schedule) were removed or disabled
- CI runs on **pull requests** and **manual dispatch only** — no automatic test runs on every push
- Release builds are **tag-driven** (`v*.*.*`) — branch pushes no longer build release artifacts
- Production deploys keep their push triggers intentionally (`main` → deploy)

## Infrastructure

Production runs on a single core VM ("**core-node**" role). Topology, deploy
flows, and the update runbook: [docs/INFRASTRUCTURE.md](../docs/INFRASTRUCTURE.md)
(sanitized — no hosts, IPs, keys, or endpoints).

## AI context methodology

Every repo ships a **six-file context** (`context/` + `AGENTS.md`) so AI agents
build with full project awareness — project overview, architecture, code
standards, UI context, AI workflow rules, and a progress tracker.

## Model-aware orchestration

P-Core projects use the **pcore-orchestra** agent loop. It routes work to the
cheapest capable model per platform:

- **Cursor:** `composer-2.5` standard for daily work, `grok-4.6` standard for
  hard / long-horizon tasks. Avoid Fast variants and Other Models as defaults.
- **OpenCode:** free models only (e.g., `nemotron-3-ultra-free`,
  `nemotron-3.5-lightning-free`, `x-preview-f-free`).

Helpers: `pcore-orchestra/scripts/setup-cursor-model.sh` (Cursor) and
`pcore-orchestra/scripts/update-free-models.sh --check` (OpenCode).