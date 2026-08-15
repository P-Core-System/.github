# Contributing to P-Core

Thanks for contributing to the P-Core project family! All repos under the P-Core-System org share a common workflow.

## Before you start

Read the context docs first — they define the architecture, standards, and current progress:

- `context/project-overview.md` — product definition, goals, features, scope
- `context/architecture.md` — system structure, boundaries, storage, invariants
- `context/code-standards.md` — coding conventions
- `context/progress-tracker.md` — current phase, completed work, next steps

These files keep the whole org in sync, so keep them updated when your work changes scope or architecture.

## Development workflow

We follow **Test-Driven Development (TDD)**:

1. **Write the test first** — describe the expected behavior in a test function
2. **Run the test — confirm it fails** (RED)
3. **Write minimal code to make it pass** (GREEN)
4. **Refactor** — clean up while keeping tests green

## Before opening a PR

Run the full quality gate locally:

```bash
make test          # full test suite
make lint          # ruff check
make typecheck     # mypy static analysis
```

Or run everything at once:

```bash
make preflight     # test → lint → format-check → typecheck
```

Your PR should pass all of these before it is merged.

## Opening a pull request

- Open a PR with a **clear, descriptive title** and a description of what changed and why.
- **Reference the issue** your PR addresses (e.g. `Closes #12`).
- Keep changes focused — a small, reviewable PR is better than a large one.
- Ensure the description covers any behavior changes so reviewers and future maintainers understand the intent.

## Questions?

Check `SUPPORT.md` for where to get help.