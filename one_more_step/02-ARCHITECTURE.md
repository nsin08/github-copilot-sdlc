# System Architecture
## GitHub-Native SDLC Governance Platform

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Architect  
**Status:** Approved  

---

## 1. Architecture Overview

### 1.1 High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        GITHUB-NATIVE SDLC PLATFORM                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                      PRESENTATION LAYER                              │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │  GitHub Issues    GitHub PRs    GitHub Projects    GitHub Releases  │  │
│  │  (Work Items)     (Changes)     (Boards/Views)     (Deployments)    │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                    ↕                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                      GOVERNANCE LAYER                                │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │  State Machine    Approval Gates    RBAC    Audit Trail    Metrics  │  │
│  │  (Workflows)      (Actions)         (CODEOWNERS)  (Logs)   (Data)   │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                    ↕                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                      AUTOMATION LAYER                                │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │  GitHub Actions (Enforcement, Validation, Metrics, Deployment)       │  │
│  │  ├── enforce-workflow.yml     (State transitions)                   │  │
│  │  ├── enforce-linking.yml      (Artifact traceability)               │  │
│  │  ├── enforce-approval.yml     (Role-based gates)                    │  │
│  │  ├── audit-logger.yml         (Immutable audit trail)               │  │
│  │  ├── metrics-collector.yml    (Performance metrics)                 │  │
│  │  └── security-gate.yml        (PenTest + scans)                     │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                    ↕                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                      CONTEXT LAYER                                   │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │  Copilot Spaces (Agent Context)                                      │  │
│  │  ├── space_framework (Governance, Rules, Role Guides)               │  │
│  │  └── space_project   (Architecture, Tech Stack, Runbooks)           │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                    ↕                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                      STORAGE LAYER                                   │  │
│  ├─────────────────────────────────────────────────────────────────────┤  │
│  │  Git Repository (Code, Docs, Config, Audit Logs, Spaces)            │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.2 Component Diagram

```
┌──────────────────────────────────────────────────────────────────────────┐
│                              GITHUB REPOSITORY                            │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐            │
│  │ .github/       │  │ spaces/        │  │ src/           │            │
│  ├────────────────┤  ├────────────────┤  ├────────────────┤            │
│  │ workflows/     │  │ space_framework│  │ (Project Code) │            │
│  │ ├── enforce-*  │  │ ├── Quick_Start│  │                │            │
│  │ ├── audit-*    │  │ ├── Rules/     │  └────────────────┘            │
│  │ └── metrics-*  │  │ ├── Roles/     │                                │
│  │                │  │ └── Templates/ │  ┌────────────────┐            │
│  │ ISSUE_TEMPLATE │  │                │  │ docs/          │            │
│  │ ├── idea.md    │  │ space_project/ │  ├────────────────┤            │
│  │ ├── epic.md    │  │ ├── Arch/      │  │ (Project Docs) │            │
│  │ ├── story.md   │  │ ├── Tech_Stack │  │                │            │
│  │ └── pr.md      │  │ └── Runbooks/  │  └────────────────┘            │
│  │                │  │                │                                │
│  │ CODEOWNERS     │  └────────────────┘  ┌────────────────┐            │
│  │ copilot-instr* │                      │ .audit/        │            │
│  └────────────────┘                      ├────────────────┤            │
│                                          │ transitions.log│            │
│  ┌────────────────┐  ┌────────────────┐  │ approvals.log  │            │
│  │ Branch         │  │ GitHub         │  │ merges.log     │            │
│  │ Protection     │  │ Projects       │  └────────────────┘            │
│  ├────────────────┤  ├────────────────┤                                │
│  │ main (locked)  │  │ Kanban View    │                                │
│  │ Require PR     │  │ Roadmap View   │                                │
│  │ Require review │  │ Backlog View   │                                │
│  │ CODEOWNER only │  │ Metrics View   │                                │
│  └────────────────┘  └────────────────┘                                │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Workflow State Machine

### 2.1 State Definitions

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           WORKFLOW STATE MACHINE                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌────────────┐           │
│  │   IDEA   │───▶│ APPROVED │───▶│  READY   │───▶│IN PROGRESS │           │
│  │          │    │          │    │          │    │            │           │
│  │ Client   │    │ Client + │    │ Architect│    │ PM assigns │           │
│  │ submits  │    │ PO review│    │ validates│    │ IC starts  │           │
│  └──────────┘    └──────────┘    └──────────┘    └────────────┘           │
│       │               │               │               │                    │
│       │               │               │               ▼                    │
│       │               │               │         ┌────────────┐            │
│       │               │               │         │ IN REVIEW  │            │
│       │               │               │         │            │            │
│       │               │               │         │ IC + Pen   │            │
│       │               │               │         │ Tester     │            │
│       │               │               │         └────────────┘            │
│       │               │               │               │                    │
│       ▼               ▼               ▼               ▼                    │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌────────────┐           │
│  │ REJECTED │    │ ON HOLD  │    │ BLOCKED  │    │    DONE    │           │
│  │          │    │          │    │          │    │            │           │
│  │ Terminal │    │ Paused   │    │ Waiting  │    │ Review     │           │
│  │ state    │    │ state    │    │ state    │    │ complete   │           │
│  └──────────┘    └──────────┘    └──────────┘    └────────────┘           │
│                                                       │                    │
│                                                       ▼                    │
│                                                 ┌────────────┐            │
│                                                 │  RELEASED  │            │
│                                                 │            │            │
│                                                 │ DevOps +   │            │
│                                                 │ CODEOWNER  │            │
│                                                 └────────────┘            │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2.2 State Transition Rules

| From State | To State | Required Approver(s) | Conditions | Automation |
|------------|----------|---------------------|------------|------------|
| - | Idea | Anyone | Idea template completed | Auto-label `state:idea` |
| Idea | Approved | Client | Business value confirmed | Label change triggers validation |
| Idea | Rejected | Client OR PO | Not aligned with goals | Terminal state |
| Approved | Ready | Architect | Technical feasibility validated, Epic/Story created | Check DoR criteria |
| Approved | On Hold | PM | Resource constraints | Preserve state for resumption |
| Ready | In Progress | PM | Resource assigned, IC picked up | Check capacity |
| In Progress | In Review | IC | PR opened with linked issue | Auto-transition on PR open |
| In Progress | Blocked | IC | External dependency | Requires blocker documentation |
| In Review | Done | IC (Reviewer) + PenTester | Code review passed, security approved | Check DoD + security gate |
| Done | Released | DevOps + CODEOWNER | All tests pass, deployment approved | Trigger release workflow |

### 2.3 Label Scheme

| Label | Purpose | Applied By |
|-------|---------|------------|
| `state:idea` | Initial submission | Automation |
| `state:approved` | Client approved | Client |
| `state:ready` | Ready for work | Architect |
| `state:in-progress` | Work started | PM/IC |
| `state:in-review` | PR under review | Automation |
| `state:done` | Review complete | Automation |
| `state:released` | In production | DevOps |
| `state:rejected` | Not proceeding | Client/PO |
| `state:on-hold` | Paused | PM |
| `state:blocked` | Waiting on external | IC |
| `type:idea` | Idea artifact | Template |
| `type:epic` | Epic artifact | Template |
| `type:story` | Story artifact | Template |
| `priority:critical` | P0 | PO |
| `priority:high` | P1 | PO |
| `priority:medium` | P2 | PO |
| `priority:low` | P3 | PO |
| `security:review-required` | Needs PenTester | Automation |

---

## 3. Artifact Linking Model

### 3.1 Artifact Hierarchy

```
┌──────────────────────────────────────────────────────────────────────────┐
│                          ARTIFACT HIERARCHY                              │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌─────────────┐                                                         │
│  │    IDEA     │  (GitHub Issue - type:idea)                            │
│  │  #100       │  Business need, success criteria                        │
│  └──────┬──────┘                                                         │
│         │ Parent: none                                                   │
│         ▼                                                                │
│  ┌─────────────┐                                                         │
│  │    EPIC     │  (GitHub Issue - type:epic)                            │
│  │  #101       │  Technical breakdown, architecture notes               │
│  │             │  Parent: #100                                           │
│  └──────┬──────┘                                                         │
│         │                                                                │
│    ┌────┴────┐                                                           │
│    ▼         ▼                                                           │
│  ┌─────────────┐  ┌─────────────┐                                       │
│  │   STORY     │  │   STORY     │  (GitHub Issue - type:story)          │
│  │  #102       │  │  #103       │  Testable acceptance criteria          │
│  │             │  │             │  Parent: #101                          │
│  └──────┬──────┘  └──────┬──────┘                                       │
│         │                │                                               │
│         ▼                ▼                                               │
│  ┌─────────────┐  ┌─────────────┐                                       │
│  │     PR      │  │     PR      │  (GitHub Pull Request)                │
│  │  #104       │  │  #105       │  Closes #102, Closes #103             │
│  │             │  │             │  Branch: feature/102-*, feature/103-* │
│  └──────┬──────┘  └──────┬──────┘                                       │
│         │                │                                               │
│         └────────┬───────┘                                               │
│                  ▼                                                       │
│           ┌─────────────┐                                                │
│           │   RELEASE   │  (GitHub Release)                             │
│           │  v1.2.0     │  Tag, changelog, deployed artifacts           │
│           │             │  References: #104, #105                        │
│           └─────────────┘                                                │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Linking Enforcement

| Link Type | Format | Validation | Consequence of Missing |
|-----------|--------|------------|------------------------|
| Epic → Idea | `Parent: #<idea-id>` in body | Automation checks | Cannot transition to Ready |
| Story → Epic | `Parent: #<epic-id>` in body | Automation checks | Cannot transition to Ready |
| PR → Story | `Closes #<story-id>` in title/body | Automation checks | Merge blocked |
| Branch → Story | `feature/<story-id>-slug` | Pre-commit hook | Commit rejected |
| Release → PRs | Auto-generated from merged PRs | Release workflow | Release notes incomplete |

---

## 4. Security Architecture

### 4.1 Access Control Model

```
┌──────────────────────────────────────────────────────────────────────────┐
│                         ACCESS CONTROL MODEL                             │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  CODEOWNERS File (.github/CODEOWNERS)                                    │
│  ────────────────────────────────────────────────────────────────────    │
│  # Default owners for everything                                         │
│  *                           @org/codeowners                            │
│                                                                          │
│  # Security-sensitive files require PenTester                            │
│  /src/auth/**                @org/codeowners @org/security              │
│  /src/crypto/**              @org/codeowners @org/security              │
│  *.pem                       @org/codeowners @org/security              │
│  *.key                       @org/codeowners @org/security              │
│                                                                          │
│  # Infrastructure requires DevOps                                        │
│  /infrastructure/**          @org/codeowners @org/devops                │
│  /.github/workflows/**       @org/codeowners @org/devops                │
│  Dockerfile                  @org/codeowners @org/devops                │
│                                                                          │
│  # Documentation can be broader                                          │
│  /docs/**                    @org/codeowners @org/tech-leads            │
│  /spaces/**                  @org/codeowners @org/tech-leads            │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Branch Protection Rules

```yaml
# Branch protection for 'main'
protection:
  required_pull_request_reviews:
    required_approving_review_count: 2
    require_code_owner_reviews: true
    dismiss_stale_reviews: true
    require_last_push_approval: true
  required_status_checks:
    strict: true
    contexts:
      - "enforce-workflow"
      - "enforce-linking"
      - "security-scan"
      - "tests"
  enforce_admins: true
  required_linear_history: true
  allow_force_pushes: false
  allow_deletions: false
  block_creations: false
  required_conversation_resolution: true
```

### 4.3 Security Gates

| Gate | Trigger | Required Approver | Blocks |
|------|---------|-------------------|--------|
| Secret Detection | Every commit | Automated | Merge if secrets found |
| Dependency Scan | Every PR | Automated | Merge if high/critical vulns |
| SAST (Static Analysis) | Every PR | Automated | Merge if critical issues |
| Security File Change | PR touches security paths | PenTester | Merge without PenTest approval |
| Production Deploy | Release workflow | CODEOWNER + DevOps | Deploy without dual approval |

---

## 5. Audit Trail Architecture

### 5.1 Audit Log Structure

```
┌──────────────────────────────────────────────────────────────────────────┐
│                          AUDIT LOG STRUCTURE                             │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  .audit/                                                                 │
│  ├── transitions/                                                        │
│  │   ├── 2026-01-01.jsonl     (Daily state transitions)                 │
│  │   └── 2026-01-02.jsonl                                               │
│  ├── approvals/                                                          │
│  │   ├── 2026-01-01.jsonl     (Daily approval decisions)                │
│  │   └── 2026-01-02.jsonl                                               │
│  ├── merges/                                                             │
│  │   ├── 2026-01-01.jsonl     (Daily merge events)                      │
│  │   └── 2026-01-02.jsonl                                               │
│  ├── releases/                                                           │
│  │   └── v1.2.0.json          (Per-release audit record)                │
│  └── index.json               (Searchable index)                         │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Audit Event Schema

```json
{
  "event_id": "uuid-v4",
  "timestamp": "2026-01-01T10:30:00Z",
  "event_type": "state_transition | approval | merge | release",
  "actor": {
    "id": "github-user-id",
    "login": "username",
    "role": "Client | PO | PM | Architect | IC | DevOps | PenTester"
  },
  "subject": {
    "type": "issue | pull_request | release",
    "id": 123,
    "url": "https://github.com/org/repo/issues/123"
  },
  "action": {
    "from_state": "idea",
    "to_state": "approved",
    "reason": "Business value confirmed"
  },
  "context": {
    "workflow_run_id": "github-actions-run-id",
    "commit_sha": "abc123",
    "linked_artifacts": ["#100", "#101"]
  },
  "signature": "sha256-hmac-of-event"
}
```

### 5.3 Immutability Guarantee

- Audit logs stored as append-only JSONL files
- Each entry signed with HMAC
- Commits to `.audit/` branch protected
- GitHub Actions workflow has write access only (no delete)
- Daily integrity check compares signatures

---

## 6. Metrics Architecture

### 6.1 Metrics Collection

| Metric | Source | Collection Frequency | Storage |
|--------|--------|---------------------|---------|
| Lead Time | PR created → merged | Per PR | `.metrics/lead-time.jsonl` |
| Cycle Time | Issue created → released | Per release | `.metrics/cycle-time.jsonl` |
| Rework Rate | PRs with revision requests | Per PR | `.metrics/rework.jsonl` |
| First-Pass Rate | PRs approved without changes | Per PR | `.metrics/first-pass.jsonl` |
| Security Gate Rate | PRs passing security first time | Per PR | `.metrics/security.jsonl` |
| State Duration | Time in each workflow state | Per issue | `.metrics/state-duration.jsonl` |
| Throughput | Stories completed per week | Weekly | `.metrics/throughput.jsonl` |

### 6.2 Dashboard Views (GitHub Projects)

| View | Purpose | Audience |
|------|---------|----------|
| Kanban Board | Current work by state | All |
| Backlog View | Prioritized upcoming work | PO, PM |
| Roadmap View | Timeline-based planning | Client, PM |
| Metrics Dashboard | Performance indicators | PM, Architect |
| Audit Trail View | Recent state changes | Compliance |

---

## 7. Integration Points

### 7.1 GitHub Native Integrations

| Integration | Purpose | Configuration |
|-------------|---------|---------------|
| GitHub Issues | Work item management | Issue templates |
| GitHub Pull Requests | Change management | PR template, branch protection |
| GitHub Actions | Automation & enforcement | Workflow files |
| GitHub Projects | Visual tracking | Project views |
| GitHub Releases | Deployment artifacts | Release workflow |
| GitHub Advanced Security | Vulnerability scanning | Enabled at repo level |
| GitHub CODEOWNERS | Access control | CODEOWNERS file |
| Copilot Spaces | Agent context | Space directories |

### 7.2 External Integration Points (Future)

| Integration | Purpose | Phase |
|-------------|---------|-------|
| SIEM (Splunk, etc.) | Audit log export | Phase 4 |
| ServiceNow | Change management ticket sync | Phase 4 |
| Jira | Legacy work item import | Phase 4 |
| Slack/Teams | Notifications | Phase 2 |

---

## 8. Deployment Architecture

### 8.1 Repository Structure

```
github-org/
├── sdlc-framework/                    (Framework Repository)
│   ├── .github/
│   │   ├── workflows/                 (Enforcement workflows)
│   │   ├── ISSUE_TEMPLATE/            (Standard templates)
│   │   ├── CODEOWNERS                 (Template CODEOWNERS)
│   │   └── copilot-instructions.md    (Framework instructions)
│   ├── spaces/
│   │   └── space_framework/           (Governance context)
│   └── workflow-system/               (Rules & guides)
│
└── project-x/                         (Project Repository)
    ├── .github/                       (Submodule → sdlc-framework)
    ├── spaces/
    │   └── space_project/             (Project-specific context)
    ├── src/                           (Project code)
    ├── .audit/                        (Audit logs)
    ├── .metrics/                      (Metrics data)
    └── CODEOWNERS                     (Project CODEOWNERS)
```

### 8.2 Adoption Flow

```
1. Organization creates sdlc-framework repo
2. Project repo adds sdlc-framework as submodule in .github/
3. Project configures CODEOWNERS
4. Project creates space_project/
5. Project enables GitHub Actions
6. Framework enforcement active
```

---

**Previous Document:** [01-REQUIREMENTS.md](01-REQUIREMENTS.md)  
**Next Document:** [03-ROLES-AND-PERMISSIONS.md](03-ROLES-AND-PERMISSIONS.md)
