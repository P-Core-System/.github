# P-Core Infrastructure — Core Node Runbook

> Sanitized on purpose. This document uses **role names only** — no hostnames,
> IPs, ports, key paths, or endpoints. Operators keep real values in their own
> secrets store; never commit them here.

## Topology (single core-node)

One production VM hosts every P-Core service. Repos live under a common
workspace directory (`~/pcore/<repo>`), each a plain git checkout of its
GitHub counterpart.

| Role | Repo | Runtime | Deploy trigger |
|------|------|---------|----------------|
| AI brain (opencode serve) | pcore-brain | systemd (`opencode-serve`) | manual |
| n8n bridge (brain API → workflow tools) | pcore-n8n-bridge | systemd, file synced from brain repo | brain deploy script |
| Workflow automation | pcore-n8n | docker stack (n8n + task runners + postgres) | manual / registry image |
| Trading bot core | pcore-trader | systemd (bot, observer, ops panel) | GitHub Actions SSH merge-deploy |
| Trader metrics | pcore-trader `deploy/` | docker (prometheus + grafana) | docker compose |
| Web AI panels (admin/chatgpt/gemini/control/deploy/worldcup) | pcore-webai | systemd per panel, shared venv | GitHub webhook → deploy-ops service |
| Telegram assistant bridge | junior-peter | systemd | manual |
| Orchestra tooling | pcore-orchestra | not runtime — installed into agent configs | install.sh |

Headless display for browser-automation panels: Xvfb service.

## Deploy flows

### 1. pcore-brain (GitHub Actions → SSH script)
- `Deploy` workflow (push to `main` or manual dispatch) SSHes to the core node
  and runs the brain deploy script.
- Script: fetch → `git reset --hard origin/main` → sync bridge entrypoint to
  the bridge workspace → restart bridge → health-check the brain HTTP endpoint.
- Requires the `DEPLOY_KEY` secret on the repo. Failure mode seen in the wild:
  files left root-owned by other processes block `git reset --hard` — fix by
  re-owning the workspace as the deploy user.

### 2. pcore-trader (Actions → SSH merge-deploy)
- Pushes touching bot/server code run an SSH deploy job: fetch, **merge**
  `origin/main` (never reset — the node carries its own auto-heal commits;
  conflicts fail loudly for manual resolution), restart all four trader units,
  then verify they are active.
- Requires the `DEPLOY_*` secrets; without a key the job warns and skips.
- Restarting those units may stop dependent brain-serve units — always re-check.

### 3. pcore-webai (webhook-driven)
- The deploy-ops panel receives GitHub webhooks and manages versions/panels.
- Server-local modifications exist on purpose (rebuilt icons, unit tweaks);
  keep them out of commits.

## Update runbook

1. `for d in ~/pcore/*/; do git -C $d fetch origin` — survey drift
   (`rev-list --left-right --count HEAD...@{upstream}`).
2. Local-only commits on the node: back them up first
   (`git push origin HEAD:refs/heads/server-backup-<date>`), then merge
   `origin/main` in, resolve, sanity-test.
3. Clean behind repos: `git pull --ff-only`.
4. Dirty repos: `git stash push -u`, pull, `git stash pop`.
5. Restart only the units that own changed code; verify with
   `systemctl is-active <units>` afterwards.

## CI/CD policy

See the org profile README. Summary: no scheduled workflows, PR/dispatch-only
CI, tag-driven releases, push-deploys reserved for production paths.

## Secrets inventory (names only)

- `DEPLOY_KEY` — read/write SSH deploy key for the core node (brain repo)
- Bridge/API keys — stored in root-owned env files on the node, never in git
- Registry tokens — scoped to GHCR pull/push where needed
