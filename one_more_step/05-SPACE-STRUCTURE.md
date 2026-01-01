# Space Structure
## GitHub-Native SDLC Governance Platform

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Architect  
**Status:** Approved  

---

## 1. Two-Space Architecture

### 1.1 Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        TWO-SPACE ARCHITECTURE                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────┐  ┌─────────────────────────────────┐  │
│  │      space_framework            │  │       space_project             │  │
│  │   (Reusable Governance)         │  │   (Project-Specific)            │  │
│  ├─────────────────────────────────┤  ├─────────────────────────────────┤  │
│  │                                 │  │                                 │  │
│  │  • Workflow rules               │  │  • Project architecture        │  │
│  │  • Role definitions             │  │  • Tech stack guides           │  │
│  │  • State machine                │  │  • Local runbooks              │  │
│  │  • DoR/DoD templates            │  │  • Domain patterns             │  │
│  │  • Issue/PR templates           │  │  • External references         │  │
│  │  • Quick start guides           │  │  • Team-specific context       │  │
│  │  • Compliance standards         │  │  • Service documentation       │  │
│  │                                 │  │                                 │  │
│  │  Distribution: Submodule        │  │  Distribution: Per-project     │  │
│  │  Updates: Framework team        │  │  Updates: Project team         │  │
│  │  Audience: All agents           │  │  Audience: Project agents      │  │
│  │                                 │  │                                 │  │
│  └─────────────────────────────────┘  └─────────────────────────────────┘  │
│                                                                             │
│  Agent loads: space_framework (governance) → space_project (context)       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.2 Design Principles

| Principle | Description |
|-----------|-------------|
| **Separation of Concerns** | Governance rules in framework; project specifics in project space |
| **Single Source of Truth** | Framework rules are authoritative; project extends, not overrides |
| **Role-Based Navigation** | Entry points by role; agents load relevant context |
| **Minimal Duplication** | Space summarizes and links; full docs in git source |
| **Scalability** | Structure supports 100+ files without overwhelming agents |

---

## 2. space_framework Structure

### 2.1 Directory Layout

```
spaces/space_framework/
├── README.md                          (Master entry point)
├── Quick_Start/
│   ├── README.md                      (Role picker)
│   ├── Im_a_Client.md                 (Client guide)
│   ├── Im_a_PO.md                     (Product Owner guide)
│   ├── Im_a_PM.md                     (Project Manager guide)
│   ├── Im_an_Architect.md             (Architect guide)
│   ├── Im_an_Implementer.md           (IC Implementer guide)
│   ├── Im_a_Reviewer.md               (IC Reviewer guide)
│   ├── Im_DevOps.md                   (DevOps guide)
│   ├── Im_a_PenTester.md              (Security guide)
│   ├── Im_a_CODEOWNER.md              (CODEOWNER guide)
│   └── Im_Stuck.md                    (Troubleshooting)
├── Rules/
│   ├── README.md                      (Rules index)
│   ├── State_Machine.md               (Workflow states & transitions)
│   ├── Definition_of_Ready.md         (DoR checklist)
│   ├── Definition_of_Done.md          (DoD checklist)
│   ├── Artifact_Linking.md            (Traceability rules)
│   ├── Approval_Gates.md              (Who approves what)
│   └── Branch_Naming.md               (Git conventions)
├── Roles/
│   ├── README.md                      (Role matrix)
│   ├── Client.md                      (Role definition)
│   ├── Product_Owner.md
│   ├── Project_Manager.md
│   ├── Architect.md
│   ├── IC_Implementer.md
│   ├── IC_Reviewer.md
│   ├── DevOps.md
│   ├── PenTester.md
│   └── CODEOWNER.md
├── Templates/
│   ├── README.md                      (Template index)
│   ├── Idea_Template.md               (Links to ISSUE_TEMPLATE)
│   ├── Epic_Template.md
│   ├── Story_Template.md
│   ├── PR_Template.md
│   └── Review_Checklist.md
├── Compliance/
│   ├── README.md                      (Compliance overview)
│   ├── Audit_Trail.md                 (Audit requirements)
│   ├── Security_Standards.md          (OWASP, etc.)
│   └── Regulatory_Mapping.md          (HIPAA, SOX, etc.)
└── _Internal/
    ├── README.md                      (Internal notes - not for agents)
    ├── Framework_Decisions.md
    └── Evolution_Log.md
```

### 2.2 Role-Based Entry Points

#### Quick_Start/Im_an_Implementer.md

```markdown
# Quick Start: I'm an Implementer

**Your Role:** Build features with high quality, security, and test coverage.

## Load Context
1. ✅ You're in space_framework (governance rules)
2. 📂 Also load: space_project (tech stack, patterns)

## Your Workflow

### Step 1: Pick a Story
- Find stories with label `state:ready`
- Check success criteria are clear
- Assign yourself

### Step 2: Create Branch
```bash
git checkout -b feature/<story-id>-<slug>
# Example: feature/123-user-authentication
```

### Step 3: Implement
- Read success criteria carefully
- Follow coding standards (see space_project/Tech_Stack/)
- Apply security guidelines (see Compliance/Security_Standards.md)

### Step 4: Write Tests
- Unit tests for each criterion
- Integration tests for workflows
- All tests must pass locally

### Step 5: Open PR
- Use PR template (Templates/PR_Template.md)
- Include `Closes #<story-id>`
- Map evidence to criteria

### Step 6: Address Feedback
- Respond to all comments
- Push fixes
- Re-request review

### Step 7: Done
- CODEOWNER merges when approved
- Issue auto-closes
- Celebrate! 🎉

## Key Rules
- ❌ Cannot self-approve PRs
- ❌ Cannot merge (CODEOWNER only)
- ✅ Can request reviews
- ✅ Can approve peer PRs

## Related Docs
- [Definition of Done](../Rules/Definition_of_Done.md)
- [PR Template](../Templates/PR_Template.md)
- [Security Standards](../Compliance/Security_Standards.md)
```

---

## 3. space_project Structure

### 3.1 Directory Layout

```
spaces/space_project/
├── README.md                          (Project overview)
├── Architecture/
│   ├── README.md                      (Architecture index)
│   ├── System_Overview.md             (High-level design)
│   ├── Component_Diagram.md           (Service boundaries)
│   ├── Data_Model.md                  (Database schema)
│   ├── API_Contracts.md               (API specifications)
│   └── ADR/                           (Architecture Decision Records)
│       ├── 001_Database_Selection.md
│       ├── 002_Auth_Strategy.md
│       └── Template.md
├── Tech_Stack/
│   ├── README.md                      (Tech stack overview)
│   ├── Backend/
│   │   ├── Language_Guide.md          (Python/Go/etc.)
│   │   ├── Framework_Patterns.md      (FastAPI/Django/etc.)
│   │   ├── Testing_Patterns.md
│   │   └── Database_Guide.md
│   ├── Frontend/
│   │   ├── Framework_Guide.md         (React/Vue/etc.)
│   │   ├── Component_Patterns.md
│   │   ├── State_Management.md
│   │   └── Testing_Guide.md
│   └── Infrastructure/
│       ├── Cloud_Provider.md          (AWS/Azure/GCP)
│       ├── Container_Guide.md         (Docker/K8s)
│       └── CI_CD_Pipeline.md
├── Runbooks/
│   ├── README.md                      (Runbook index)
│   ├── Local_Development.md           (Dev environment setup)
│   ├── Build_and_Test.md              (Build commands)
│   ├── Deployment.md                  (Deploy procedures)
│   ├── Troubleshooting.md             (Common issues)
│   └── Incident_Response.md           (Production issues)
├── External_References/
│   ├── README.md                      (Reference index)
│   ├── Industry_Standards.md          (OWASP, etc.)
│   ├── OSS_Patterns.md                (Open source references)
│   └── Regulatory_Context.md          (Sector-specific)
└── _Team/
    ├── README.md                      (Team notes - internal)
    ├── Onboarding.md
    └── Contact_List.md
```

### 3.2 Project-Specific Entry Point

#### README.md (Project Overview)

```markdown
# Project: [Project Name]

**Version:** 1.0.0  
**Status:** Active Development  
**Framework:** github-copilot-sdlc v1.0.0  

## Quick Links

| I want to... | Go to |
|--------------|-------|
| Understand the architecture | [Architecture/System_Overview.md](Architecture/System_Overview.md) |
| Set up local environment | [Runbooks/Local_Development.md](Runbooks/Local_Development.md) |
| Learn the tech stack | [Tech_Stack/README.md](Tech_Stack/README.md) |
| Deploy code | [Runbooks/Deployment.md](Runbooks/Deployment.md) |
| Fix a production issue | [Runbooks/Incident_Response.md](Runbooks/Incident_Response.md) |

## Tech Stack Summary

| Layer | Technology | Guide |
|-------|------------|-------|
| Backend | Python 3.11 + FastAPI | [Backend Guide](Tech_Stack/Backend/Framework_Patterns.md) |
| Frontend | React 18 + TypeScript | [Frontend Guide](Tech_Stack/Frontend/Framework_Guide.md) |
| Database | PostgreSQL 15 | [Database Guide](Tech_Stack/Backend/Database_Guide.md) |
| Infrastructure | AWS + Kubernetes | [Infra Guide](Tech_Stack/Infrastructure/Cloud_Provider.md) |
| CI/CD | GitHub Actions | [Pipeline Guide](Tech_Stack/Infrastructure/CI_CD_Pipeline.md) |

## Governance

This project follows the **github-copilot-sdlc** framework.

**Workflow:** Idea → Approved → Ready → In Progress → In Review → Done → Released

**Rules:** See [space_framework/Rules/](../space_framework/Rules/)

## Team

| Role | Contact | Responsibility |
|------|---------|----------------|
| CODEOWNER | @alice | Merge authority |
| Architect | @bob | Technical decisions |
| DevOps | @charlie | Deployments |
| PM | @diana | Coordination |
```

---

## 4. Navigation Strategy

### 4.1 Agent Entry Flow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           AGENT NAVIGATION FLOW                             │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. Agent starts task                                                       │
│     ↓                                                                       │
│  2. Load space_framework                                                    │
│     ├── Read: README.md (compass)                                          │
│     ├── Navigate to: Quick_Start/Im_an_<Role>.md                           │
│     └── Learn: Workflow rules, DoR, DoD                                    │
│     ↓                                                                       │
│  3. Load space_project                                                      │
│     ├── Read: README.md (project context)                                  │
│     ├── Navigate to: Tech_Stack/<relevant>                                 │
│     └── Learn: Patterns, standards, tools                                  │
│     ↓                                                                       │
│  4. Execute task                                                            │
│     ├── Follow: Rules from framework                                       │
│     ├── Apply: Patterns from project                                       │
│     └── Create: Artifacts per templates                                    │
│     ↓                                                                       │
│  5. (Optional) Update space                                                 │
│     └── Add: Discoveries, tips, improvements                               │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Cross-Space Navigation

| From | To | Navigation |
|------|-----|------------|
| space_framework | space_project | `[Project Details](../space_project/README.md)` |
| space_project | space_framework | `[Workflow Rules](../space_framework/Rules/)` |
| Quick Start | Full Role Doc | `[Full Guide](../Roles/<Role>.md)` |
| Summary Rule | Full Rule Doc | `[Full Details](../../workflow-system/rules/<rule>.md)` |

### 4.3 Breadcrumb Pattern

Each document includes navigation breadcrumb:

```markdown
---
**Navigation:** [Home](../README.md) → [Quick Start](README.md) → Im an Implementer
**Related:** [DoD](../Rules/Definition_of_Done.md) | [PR Template](../Templates/PR_Template.md)
---
```

---

## 5. Content Guidelines

### 5.1 What Goes Where

| Content Type | space_framework | space_project | Git Source |
|--------------|-----------------|---------------|------------|
| Workflow rules | ✅ Summary + link | ❌ | ✅ Full details |
| Role guides | ✅ Quick start | ❌ | ✅ Full definition |
| Templates | ✅ Index + link | ❌ | ✅ Templates |
| Tech stack | ❌ | ✅ Full guides | ❌ |
| Architecture | ❌ | ✅ Full design | Optional |
| Runbooks | ❌ | ✅ Full procedures | ❌ |
| Compliance | ✅ Standards | ✅ Sector-specific | ✅ Mapping |
| ADRs | ❌ | ✅ Project ADRs | Optional |

### 5.2 Document Format

**Header:**
```markdown
# Document Title

**Purpose:** One-line description  
**Audience:** Role(s) who use this  
**Last Updated:** YYYY-MM-DD  

---
```

**Body:**
- Use headers (##, ###) for navigation
- Use tables for reference data
- Use checklists for procedures
- Include code examples where relevant
- Link to related docs

**Footer:**
```markdown
---
**Previous:** [Previous Doc](previous.md)  
**Next:** [Next Doc](next.md)  
**Related:** [Related 1](related1.md) | [Related 2](related2.md)
```

### 5.3 Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Folder | PascalCase | `Quick_Start/` |
| File | PascalCase + Underscores | `Im_an_Implementer.md` |
| ADR | NNN_Title | `001_Database_Selection.md` |
| No spaces | Always use underscore | `State_Machine.md` |

---

## 6. Maintenance

### 6.1 Update Responsibilities

| Space | Owner | Update Frequency | Review |
|-------|-------|-----------------|--------|
| space_framework | Framework team | Per release | CODEOWNER |
| space_project | Project team | Continuous | Tech Lead |

### 6.2 Version Alignment

| Framework Version | Space Structure | Compatibility |
|-------------------|-----------------|---------------|
| v1.0.x | Initial structure | Current |
| v1.1.x | May add folders | Backward compatible |
| v2.0.x | May restructure | Migration guide |

### 6.3 Quality Checklist

- [ ] All links valid
- [ ] No placeholder text
- [ ] Breadcrumbs accurate
- [ ] Code examples tested
- [ ] Tables formatted correctly
- [ ] No PII or secrets

---

**Previous Document:** [04-WORKFLOW-SPECIFICATION.md](04-WORKFLOW-SPECIFICATION.md)  
**Next Document:** [06-PHASED-DELIVERY-PLAN.md](06-PHASED-DELIVERY-PLAN.md)
