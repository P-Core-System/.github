# Progress Tracker

Update this file after every meaningful implementation change.

## Current Phase

- 2026-08-22 — Expanded org profile model routing: added per-platform model table, cost-pool rationale, and helper/docs links to `profile/README.md`. Impact: public org profile now clearly documents the Cursor vs OpenCode model policy and where to start.
- 2026-08-22 — Org docs sync for model-aware orchestration: added model routing section to `profile/README.md`, updated `SUPPORT.md` pcore-orchestra description, and added `Model routing` rule to `context/ai-workflow-rules.md`. Impact: org profile and workflow rules now document the Cursor `composer-2.5` default / `grok-4.6` escalation and OpenCode free-model policy.
- Complete — org profile + community health + reusable workflows live.

## Current Goal

- Keep org metadata and all member repo context in sync.

## Completed

- Org profile README with full project family table.
- Community health files (SECURITY, CONTRIBUTING, CODE_OF_CONDUCT, SUPPORT, FUNDING).
- Reusable workflows (`reusable-ci-py.yml`, `reusable-stale.yml`, dependabot consistency).
- Six-file context bootstrapped into every member repo + this repo.

## In Progress

- None.

## Next Up

- Ensure all member repos reference org reusable workflows.
- Verify dependabot consistency across repos.

## Open Questions

- None.

## Architecture Decisions

- Six-file context enforced org-wide for consistent AI-agent behavior.
- Reusable workflows tested in `demo-repository` before production rollout.

## Session Notes

- Org: github.com/P-Core-System · owner: peter.