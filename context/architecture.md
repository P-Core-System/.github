# Architecture Context

## Stack

| Layer     | Technology                  | Role |
| --------- | --------------------------- | ---- |
| Profile   | Markdown                    | Org landing page (`profile/README.md`) |
| Health    | Markdown + YAML             | Community files (SECURITY, CONTRIBUTING, CODE_OF_CONDUCT, SUPPORT, FUNDING) |
| CI        | GitHub Actions              | Reusable workflows (`reusable-ci-py.yml`, `reusable-stale.yml`, dependabot consistency) |
| Context   | Markdown                    | Six-file context methodology for AI agents |

## System Boundaries

- `profile/` — org landing page rendered on github.com/P-Core-System
- `.github/workflows/` — reusable CI/stale/dependabot workflows shared org-wide
- `context/` — AI six-file context (this org's methodology)
- `AGENTS.md` — AI entry point

## Storage Model

- No application storage — org metadata only (files in git).

## Auth and Access Model

- Public org repo; maintainers (Peter) have write access.
- Community contributions via PRs using org templates.

## System Design & Infrastructure

| Concept | Service / Tech | Notes |
|---------|---------------|-------|
| **CI** | GitHub Actions | Reusable workflows referenced by member repos |
| **Automation** | Dependabot + stale bot | Version updates + issue/PR hygiene |
| **Hosting** | GitHub Pages / github.com | Profile + docs |

## Scaling & Performance Constraints

- Org-scale (8 repos), no traffic or latency targets.

## Invariants

1. Reusable workflows must stay backward-compatible with all member repos.
2. Community health files reflect current org security/contribution policy.
3. Every new P-Core repo ships the six-file context + AGENTS.md.