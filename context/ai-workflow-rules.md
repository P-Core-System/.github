# AI Workflow Rules

## Approach

Maintain the P-Core org metadata incrementally. Context files define the org
structure, community health, and reusable workflows. Implement against these
specs — keep every member repo in sync.

## Scoping Rules

- Work on one org concern at a time (profile, workflows, health, context).
- Prefer small, verifiable increments over large speculative changes.
- Do not change reusable workflows without a test repo rollout.

## When to Split Work

Split an implementation step if it combines:

- Workflow changes and health-file changes
- Profile changes and CI changes
- Behavior not clearly defined in the context files

## System Design Triggers

| Trigger | Consider |
|---------|----------|
| New member repo | Six-file context + dependabot + reusable workflow wiring |
| Workflow change | Backward compatibility with all member repos |
| Security policy change | Update SECURITY.md + docs in member repos |

## Handling Missing Requirements

- Do not invent org behavior not defined in context files.
- If ambiguous, resolve in the relevant context file before implementing.
- If missing, add as an open question in `progress-tracker.md`.

## Protected Files

- Do not modify reusable workflows without a `demo-repository` rollout.

## Keeping Docs in Sync

Update the relevant context file whenever implementation changes:

- Org structure or boundaries
- Workflow / automation decisions
- Code conventions
- Feature scope

## Model routing

For projects using pcore-orchestra:

- **Cursor:** default to `composer-2.5` standard; escalate to `grok-4.6` standard only for hard / long-horizon tasks. Avoid Fast variants and Other Models as defaults.
- **OpenCode:** use free models only. Validate with `bash scripts/update-free-models.sh --check`.

See `pcore-orchestra/docs/token-optimization.md` and `pcore-orchestra/scripts/setup-cursor-model.sh`.

## Before Moving to the Next Unit

1. The current unit works end to end within its scope.
2. No invariant defined in `architecture.md` was violated.
3. `progress-tracker.md` reflects the completed work.
4. Workflows validate in CI.