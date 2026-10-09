# P-Core System — Teams & Departments

> **Status**: Proposed structure — teams need to be created in GitHub org settings (requires `admin:org` scope)

---

## Departments & Teams

### 1. Platform Engineering
**Lead**: @peterlianpi  
**Description**: Core platform services, brain, orchestration, bridges

| Team | Slug | Repos | Description |
|------|------|-------|-------------|
| **Core Platform** | `core-platform` | `pcore-brain`, `pcore-n8n-bridge`, `pcore-orchestra`, `.github` | Core platform engineering - brain, bridge, orchestra, org meta |

---

### 2. AI Products
**Lead**: @peterlianpi  
**Description**: Customer-facing AI products and trading systems

| Team | Slug | Repos | Description |
|------|------|-------|-------------|
| **AI & ML** | `ai-ml` | `pcore-trader`, `pcore-assistant`, `pcore-webai` | AI/ML products - trading, assistant, webai |
| **Product & Design** | `product-design` | `pcore-webai`, `pcore-assistant`, `pcore-trader` | Product management, UX/UI, user research |

---

### 3. Infrastructure & Operations
**Lead**: @peterlianpi  
**Description**: Infrastructure, monitoring, automation, networking

| Team | Slug | Repos | Description |
|------|------|-------|-------------|
| **Infrastructure** | `infrastructure` | `pcore-vpn`, `pcore-monitor`, `pcore-n8n` | Infrastructure & DevOps - VPN, monitoring, n8n |
| **Security & Compliance** | `security-compliance` | *all repos* | Security auditing, compliance, secrets management |

---

## Repository → Team Mapping

| Repository | Primary Team | Department |
|------------|--------------|------------|
| `pcore-brain` | core-platform | Platform Engineering |
| `pcore-n8n-bridge` | core-platform | Platform Engineering |
| `pcore-orchestra` | core-platform | Platform Engineering |
| `.github` | core-platform | Platform Engineering |
| `pcore-trader` | ai-ml | AI Products |
| `pcore-assistant` | ai-ml | AI Products |
| `pcore-webai` | ai-ml | AI Products |
| `pcore-vpn` | infrastructure | Infrastructure & Operations |
| `pcore-monitor` | infrastructure | Infrastructure & Operations |
| `pcore-n8n` | infrastructure | Infrastructure & Operations |

---

## GitHub Team Creation Commands

Run these with a token that has `admin:org` scope:

```bash
# Core Platform
gh api orgs/P-Core-System/teams --method POST -f name="Core Platform" -f slug="core-platform" -f description="Core platform engineering - brain, bridge, orchestra" -f privacy=closed

# AI & ML
gh api orgs/P-Core-System/teams --method POST -f name="AI & ML" -f slug="ai-ml" -f description="AI/ML products - trading, assistant, webai" -f privacy=closed

# Infrastructure
gh api orgs/P-Core-System/teams --method POST -f name="Infrastructure" -f slug="infrastructure" -f description="Infrastructure & DevOps - VPN, monitoring, n8n" -f privacy=closed

# Security & Compliance
gh api orgs/P-Core-System/teams --method POST -f name="Security & Compliance" -f slug="security-compliance" -f description="Security auditing, compliance, secrets management" -f privacy=closed

# Product & Design
gh api orgs/P-Core-System/teams --method POST -f name="Product & Design" -f slug="product-design" -f description="Product management, UX/UI, user research" -f privacy=closed
```

---

## Team Repository Permissions

After creating teams, assign repository permissions:

```bash
# Core Platform → pcore-brain, pcore-n8n-bridge, pcore-orchestra, .github
for repo in pcore-brain pcore-n8n-bridge pcore-orchestra .github; do
  gh api orgs/P-Core-System/teams/core-platform/repos/P-Core-System/$repo --method PUT -f permission=admin
done

# AI & ML → pcore-trader, pcore-assistant, pcore-webai
for repo in pcore-trader pcore-assistant pcore-webai; do
  gh api orgs/P-Core-System/teams/ai-ml/repos/P-Core-System/$repo --method PUT -f permission=admin
done

# Infrastructure → pcore-vpn, pcore-monitor, pcore-n8n
for repo in pcore-vpn pcore-monitor pcore-n8n; do
  gh api orgs/P-Core-System/teams/infrastructure/repos/P-Core-System/$repo --method PUT -f permission=admin
done

# Security & Compliance → all repos (read for audit)
for repo in pcore-brain pcore-n8n-bridge pcore-orchestra .github pcore-trader pcore-assistant pcore-webai pcore-vpn pcore-monitor pcore-n8n; do
  gh api orgs/P-Core-System/teams/security-compliance/repos/P-Core-System/$repo --method PUT -f permission=read
done

# Product & Design → pcore-webai, pcore-assistant, pcore-trader
for repo in pcore-webai pcore-assistant pcore-trader; do
  gh api orgs/P-Core-System/teams/product-design/repos/P-Core-System/$repo --method PUT -f permission=write
done
```

---

## Adding Team Members

```bash
# Add maintainers to teams
for team in core-platform ai-ml infrastructure security-compliance product-design; do
  gh api orgs/P-Core-System/teams/$team/memberships/peterlianpi --method PUT -f role=maintainer
done
```

---

## Project.yaml Integration

Each repository's `project.yaml` now includes:

```yaml
team:
  lead: peter
  members:
    - name: peter
      role: maintainer
      contact: peterlianpi
  github_team: <team-slug>  # e.g., core-platform, ai-ml, infrastructure
```

The workspace `project.yaml` includes the full departments/teams structure for cross-repo visibility.

---

## Scaling Recommendations

See [SCALING.md](SCALING.md) for recommendations on handling more projects, teams, and organizational growth.

---

*Last updated: 2026-10-09*
