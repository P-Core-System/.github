# P-Core System

Autonomous, self-healing AI systems built on a shared brain and orchestration core — maintained by Peter.

## Projects

| Project | Description | Language |
|---------|-------------|----------|
| [pcore-orchestra](https://github.com/P-Core-System/pcore-orchestra) | Ambient multi-agent orchestration for Cursor IDE and OpenCode CLI — plan → implement → verify → review | JavaScript |
| [pcore-brain](https://github.com/P-Core-System/pcore-brain) | Reusable AI brain client — opencode serve bridge with auto model pools per task/agent, multi-user auth, React SPA dashboard | Python |
| [pcore-webai](https://github.com/P-Core-System/pcore-webai) | Multi-provider LLM web-to-API gateway — Gemini & ChatGPT sessions as OpenAI-style APIs, crypto tools, ops bots | Python |
| [pcore-trader](https://github.com/P-Core-System/pcore-trader) | Automated crypto trading bot — signals, futures, margin, monitor, learn, ops panel | Python |
| [pcore-monitor](https://github.com/P-Core-System/pcore-monitor) | Self-hostable VPS monitoring panel with Telegram bot bridge, Go server + React SPA | Go / Python |
| [pcore-n8n-bridge](https://github.com/P-Core-System/pcore-n8n-bridge) | OpenAI-compatible HTTP bridge to pcore-brain for n8n integration | Python |
| [pcore-assistant](https://github.com/P-Core-System/pcore-assistant) | AI-powered Telegram chat assistant — English/Burmese offline message handling | JavaScript |
| [pcore-vpn](https://github.com/P-Core-System/pcore-vpn) | P Core-VPN — Xray multi-protocol proxy panel with reseller & brain integration (active fork of 3x-ui) | Go |
| [pcore-n8n](https://github.com/P-Core-System/pcore-n8n) | Self-hosted n8n workflow automation — n8n 2.35.7 + Python/JS task runners on the core node | TypeScript |

### Archived

| Project | Description | Status |
|---------|-------------|--------|
| [pcore-panel](https://github.com/P-Core-System/pcore-panel) | Xray multi-protocol multi-user panel (fork of 3x-ui) | Archived |

### Satellite repos

Maintained under [@peterlianpi](https://github.com/peterlianpi):

| Repo | Description |
|------|-------------|
| [junior-peter](https://github.com/peterlianpi/junior-peter) | AI-powered Telegram chat assistant — operates on Peter's personal Telegram account |
| [P-Core-System](https://github.com/peterlianpi/P-Core-System) | Monorepo — `p-core-backend`, `p-core-system`, `p-core-mobile`, zolai-dashboard plugin |
| [pcore-real-estate](https://github.com/peterlianpi/pcore-real-estate) | Listings CRM — real estate platform (Laravel + Inertia) |

## Meta

| Project | Description |
|---------|-------------|
| [.github](https://github.com/P-Core-System/.github) | Org profile, community health files, and reusable CI workflows |

## Architecture

Production runs on a single core VM (**sg-ec2**). Topology:

| Service | Port | Role |
|---------|------|------|
| opencode serve (brain) | `:41794` | Shared AI brain — model pools, session management |
| brain bridge | `:4099` | OpenAI-compatible HTTP bridge for n8n and external consumers |
| n8n | `:5678` | Workflow automation — MASTER pipeline, Telegram assistant, career navigator |
| crypto-trader ops panel | `:4030` | Trading bot control dashboard |
| pcore-monitor panel | `:8080` | VPS monitoring dashboard |
| pcore-agent | `:8081` | Lightweight Go agent for SSH-less metric collection |

All public endpoints are tunneled through **Cloudflare** (zero-trust, token-managed).

## AI context methodology

Every repo ships a **six-file context** (`context/` + `AGENTS.md`) so AI agents
build with full project awareness — project overview, architecture, code
standards, UI context, AI workflow rules, and a progress tracker.

## Model-aware orchestration

P-Core projects use the **pcore-orchestra** agent loop. It routes work to the
cheapest capable model per platform so daily tasks stay cheap and hard tasks
get the right horsepower without burning the limited "Other Models" pool.

| Platform | Task type | Default model | Escalation / notes |
|----------|-----------|---------------|--------------------|
| **Cursor** | Daily plan / implement / verify / review | `composer-2.5` standard | `grok-4.6` standard for hard or long-horizon tasks |
| **OpenCode** | All phases | `mimo-v2.5-free` | Free Zen tier only |

## Infrastructure

- **sg-ec2** (47.128.228.24) — 3.8GB RAM, opencode v1.18.24
- **Cloudflare tunnel** — public endpoints (pcore-brain, n8n, crypto-ops)
- **GitHub Actions** — org reusable workflows, PR-only CI (no scheduled crons)
- **systemd + Docker** — services on sg-ec2 managed via systemd units and docker-compose

## Automation policy

To stay within GitHub free-tier minutes:

- **No scheduled workflows** — all former daily/weekly crons removed or disabled
- CI runs on **pull requests** and **manual dispatch only**
- Release builds are **tag-driven** (`v*.*.*`)
- Production deploys keep their push triggers (`main` → deploy)