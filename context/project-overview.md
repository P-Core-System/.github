# P-Core System Org (.github) — Project Overview

## Overview

The `.github` repository is the **P-Core System** organization's meta-repo. It
holds the org profile README (landing page), community health files (SECURITY,
CONTRIBUTING, CODE_OF_CONDUCT, SUPPORT, FUNDING), and reusable CI workflows
shared across all P-Core repos.

## Goals

1. Present the P-Core project family on the org landing page.
2. Provide org-wide community health defaults.
3. Share reusable GitHub Actions workflows (`reusable-ci-py.yml`,
   `reusable-stale.yml`, dependabot consistency).
4. Enforce the six-file context methodology org-wide.

## Core User Flow

1. A visitor opens the org page and sees the project family table.
2. Contributors opening issues/PRs get org templates + health guidance.
3. P-Core repos reference reusable workflows from this repo.
4. Dependabot config stays consistent across repos.

## Features

### Profile & Health

- `profile/README.md` — org landing page.
- Community health files — SECURITY, CONTRIBUTING, CODE_OF_CONDUCT, SUPPORT, FUNDING.

### Workflows

- `reusable-ci-py.yml` — reusable Python CI.
- `reusable-stale.yml` — stale issue/PR automation.
- `dependabot-consistency.yml` — dependabot config check.

## Scope

### In Scope

- Org profile, community health, reusable workflows, org automation.

### Out of Scope

- Application code — this repo is org infrastructure only.

## Success Criteria

1. Org page renders the full project family.
2. All reusable workflows are used by member repos.
3. New repos inherit org health files automatically.