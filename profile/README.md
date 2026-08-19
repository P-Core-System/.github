# P-Core System

Autonomous, self-healing AI systems built on a shared brain and orchestration core — maintained by Peter.

## Projects

| Project | Description | Language |
|---------|-------------|----------|
| [pcore-orchestra](https://github.com/P-Core-System/pcore-orchestra) | Ambient multi-agent orchestration for Cursor IDE and OpenCode CLI — plan → implement → verify → review | JavaScript |
| [pcore-brain](https://github.com/P-Core-System/pcore-brain) | Reusable AI brain client — opencode serve bridge with auto model pools per task/agent | Python |
| [pcore-webai](https://github.com/P-Core-System/pcore-webai) | Reusable multi-provider web AI server — Gemini & ChatGPT panels, crypto tools, ops bots | Python |
| [pcore-trader](https://github.com/P-Core-System/pcore-trader) | Automated crypto trading bot — signals, futures, margin, monitor, learn, ops panel | Python |
| [pcore-assistant](https://github.com/P-Core-System/pcore-assistant) | AI-powered Telegram chat assistant — English/Burmese offline message handling | JavaScript |
| [pcore-3x-ui](https://github.com/P-Core-System/pcore-3x-ui) | P-Core panel — Xray multi-protocol multi-user proxy panel (P-Core fork of 3x-ui) | Go |
| [pcore-n8n](https://github.com/P-Core-System/pcore-n8n) | Self-hosted n8n workflow automation - n8n 2.34.5 + Python/JS task runners on sg-ec2 | TypeScript |

## Meta

| Project | Description |
|---------|-------------|
| [.github](https://github.com/P-Core-System/.github) | Org profile, community health files, and reusable CI workflows |

## Automation

The P-Core org uses GitHub Actions for CI + maintenance automation:

- Org-wide reusable workflows in `.github/workflows/`
- Community health files (SECURITY, CONTRIBUTING, CODE_OF_CONDUCT, SUPPORT)
- Dependabot version updates configured per repo

## AI context methodology

Every repo ships a **six-file context** (`context/` + `AGENTS.md`) so AI agents
build with full project awareness — project overview, architecture, code
standards, UI context, AI workflow rules, and a progress tracker.