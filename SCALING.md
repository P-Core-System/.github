# P-Core System — Scaling Strategy

> Recommendations for handling more projects, teams, and organizational growth.

---

## Current State (2026-10-09)

- **11 active repositories** across 3 departments
- **5 GitHub teams** (to be created)
- **1 maintainer** (@peterlianpi) wearing all hats
- **Single server** (sg-ec2) hosting all production services

---

## Growth Scenarios & Recommendations

### Scenario 1: Adding 2-3 New Projects (Near Term)

**Trigger**: New product ideas, client work, or spin-outs

**Actions**:
1. **Classify the project type** using `project.yaml` `type` field:
   - `devops` → Infrastructure team
   - `ai` / `library` → AI & ML team
   - `infrastructure` → Infrastructure team
   - `meta` → Core Platform team

2. **Assign to existing team** based on type — no new team needed

3. **Update workspace `project.yaml`**:
   ```yaml
   departments:
     - name: <Department>
       projects:
         - <new-repo>
   ```

4. **Create repo with six-file context** from template:
   ```bash
   # Use pcore-orchestra as template
   gh repo create P-Core-System/<new-repo> --private --template=P-Core-System/pcore-orchestra
   ```

---

### Scenario 2: Adding Team Members (Medium Term)

**Trigger**: Hiring, contractors, collaborators

**Current bottleneck**: Single maintainer (@peterlianpi) owns all teams

**Actions**:
1. **Create role-based access** in GitHub teams:
   - `maintainer` — full admin (current: peterlianpi)
   - `developer` — write access to assigned repos
   - `reviewer` — read + PR review access
   - `security-auditor` — read access to all (security-compliance team)

2. **Delegate team ownership**:
   ```yaml
   # In project.yaml
   team:
     lead: <new-lead>
     members:
       - name: <new-member>
         role: developer
         contact: <github-handle>
   ```

3. **Rotate on-call** for production services (sg-ec2)

---

### Scenario 3: Multiple Maintainers per Team (Medium Term)

**Trigger**: Team grows beyond 3-4 people

**Actions**:
1. **Split teams by domain**:
   - Core Platform → `platform-brain`, `platform-orchestra`, `platform-bridge`
   - AI & ML → `ai-trading`, `ai-assistant`, `ai-webai`
   - Infrastructure → `infra-network`, `infra-monitoring`, `infra-automation`

2. **Introduce team leads** with `maintainer` role

3. **Use CODEOWNERS** for automatic PR assignment:
   ```
   # .github/CODEOWNERS
   /pcore-brain/ @P-Core-System/platform-brain
   /pcore-orchestra/ @P-Core-System/platform-orchestra
   ```

---

### Scenario 4: Multi-Server / Multi-Region (Long Term)

**Trigger**: Production scaling, disaster recovery, latency requirements

**Current**: Single sg-ec2 (47.128.228.24)

**Actions**:
1. **Infrastructure as Code** (Terraform) for reproducible environments
2. **GitOps** (ArgoCD/Flux) for deployments
3. **Service mesh** for inter-service communication
4. **Centralized logging/metrics** (Loki + Prometheus + Grafana)

---

### Scenario 5: External Contributors / Open Source (Long Term)

**Trigger**: Community interest, client requests, standardization

**Actions**:
1. **Public repos** with `CONTRIBUTING.md`, `SECURITY.md`, `CODE_OF_CONDUCT.md`
2. **Contributor License Agreement (CLA)** via GitHub
3. **Triage team** for issue/PR management
4. **Release managers** for versioning/changelogs

---

## Organizational Patterns to Adopt

### 1. Team Topologies (Matthew Skelton & Manuel Pais)

| Pattern | When to Use |
|---------|-------------|
| **Stream-aligned team** | Product-focused (AI & ML team) |
| **Platform team** | Internal services (Core Platform team) |
| **Enabling team** | Specialized expertise (Security & Compliance) |
| **Complicated subsystem** | Deep technical domain (Trading engine) |

**Current mapping**:
- Core Platform = Platform team
- AI & ML = Stream-aligned teams (per product)
- Infrastructure = Platform team
- Security & Compliance = Enabling team

### 2. Conway's Law Alignment

> "Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations."

**Action**: Match repo boundaries to team boundaries. Each team owns their repos end-to-end.

### 3. Cognitive Load Management

**Rule**: A team should own ≤ 3-4 active repos to maintain context.

**Current**: 1 person owns 11 repos → **Overloaded**

**Fix**: 
- Hire/contract per team
- Or reduce active surface (archive stale repos)
- Or consolidate similar repos (e.g., pcore-vpn + pcore-panel)

---

## Recommended Next Steps (Priority Order)

### Immediate (This Week)
- [ ] Create 5 GitHub teams (requires admin:org token)
- [ ] Assign @peterlianpi as maintainer to all teams
- [ ] Add CODEOWNERS to each repo pointing to team slugs
- [ ] Set up branch protection rules per team

### Short Term (This Month)
- [ ] Document on-call rotation for sg-ec2 services
- [ ] Create runbooks for each production service
- [ ] Set up GitHub Projects for cross-repo work tracking
- [ ] Define "Definition of Done" per team

### Medium Term (This Quarter)
- [ ] Hire/contract 1-2 developers for AI & ML team
- [ ] Split Infrastructure team if >3 people
- [ ] Implement Terraform for sg-ec2 provisioning
- [ ] Add dependency tracking (Dependabot + Renovate)

### Long Term (6+ Months)
- [ ] Multi-region deployment capability
- [ ] Platform team builds internal developer platform (IDP)
- [ ] Formalize API contracts between services
- [ ] Invest in developer experience (dx) tooling

---

## Metrics to Track

| Metric | Target | Current |
|--------|--------|---------|
| Repos per maintainer | ≤ 4 | 11 |
| PR review time (p50) | < 4 hours | N/A |
| Deployment frequency | Daily | Weekly |
| Change failure rate | < 15% | Unknown |
| MTTR (production) | < 30 min | ~2 hours |
| Team cognitive load | Manageable | High |

---

## Decision Framework for New Projects

```
NEW PROJECT REQUEST
       │
       ▼
┌──────────────────┐
│ Classify Type    │── devops ──► Infrastructure Team
│ (project.yaml)   │── ai      ──► AI & ML Team
└──────────────────┘── infra   ──► Infrastructure Team
       │              ── lib    ──► Core Platform Team
       ▼              ── meta   ──► Core Platform Team
┌──────────────────┐
│ Team Capacity?   │── Yes ──► Assign to team
└──────────────────┘── No  ──► Hire/Contract OR
                              Defer OR
                              Consolidate
```

---

## Anti-Patterns to Avoid

| Anti-Pattern | Symptom | Fix |
|--------------|---------|-----|
| **Hero mode** | One person knows everything | Document, cross-train, hire |
| **Shared ownership** | No one owns anything | Clear CODEOWNERS, single team per repo |
| **Team sprawl** | Too many teams for people | Merge teams, use sub-teams |
| **Repo sprawl** | Too many repos for teams | Consolidate, archive, monorepo |
| **Process over people** | Heavy process, low output | Lightweight, trust-based, measure outcomes |

---

## Template: New Project Checklist

When creating a new P-Core repo:

- [ ] Choose `type` in `project.yaml` (devops/ai/library/infrastructure/meta)
- [ ] Assign to department/team in workspace `project.yaml`
- [ ] Create repo from appropriate template
- [ ] Add six-file context (`context/` + `AGENTS.md`)
- [ ] Configure branch protection (require PR review, status checks)
- [ ] Add CODEOWNERS pointing to GitHub team
- [ ] Enable Dependabot + secret scanning
- [ ] Add to sg-ec2 deploy pipeline (if production)
- [ ] Update org profile README (`.github` repo)
- [ ] Document in TEAMS.md

---

*Last updated: 2026-10-09*
