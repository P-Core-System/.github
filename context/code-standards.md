# Code Standards

## General

- Org metadata in Markdown/YAML — keep files concise and consistent across repos.
- Every repo must ship `context/` + `AGENTS.md` (six-file methodology).
- Reusable workflows must be parameterized for reuse by member repos.

## Markdown

- Use GitHub-flavored markdown, tables for structured data.
- Keep org profile table rows aligned (Project | Description | Language).

## GitHub Actions

- Reusable workflows live in `.github/workflows/` with `workflow_call`.
- Pin actions to version tags; document required inputs/secrets.
- Test workflow changes in `demo-repository` before production rollout.

## File Organization

- `profile/` — org landing content
- `.github/workflows/` — reusable workflows
- `context/` — six-file context
- `AGENTS.md` — AI entry point

## Community Health

- SECURITY, CONTRIBUTING, CODE_OF_CONDUCT, SUPPORT, FUNDING at repo root.
- Issue/PR templates under `.github/`.

## Testing

- Validate workflow YAML via `actionlint` or GitHub lint in CI.
- Dependabot consistency checked by `dependabot-consistency.yml`.