# Space Structure
## SDLC Governance Control Plane — AI Context Architecture

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Architect  
**Status:** Approved  
**Audience:** Product / sales / diligence  
**Classification:** Confidential — Competitive Strategy  

---

## Strategic Context

> **AI governance is still emerging; we design for role-scoped, auditable AI assistance.**

The two-space architecture enables AI agents to operate within governed boundaries:

| Common challenge | Our approach | Why it helps |
|-----------------------|--------------|-------------|
| AI agents lack context | Role-specific spaces with workflow rules | Agents follow same rules as humans |
| AI actions are uncontrolled | Permission boundaries in space docs | Agents can't merge, can't skip gates |
| AI suggestions violate compliance | Embedded compliance rules in context | AI suggests compliant approaches |
| Multiple projects, inconsistent governance | Shared framework space + project space | Governance scales across org |

**The Two-Space Architecture is our AI governance pattern:**
- **space_framework** — Reusable governance (distributed to all projects)
- **space_project** — Project-specific context (tech stack, architecture)

---

## 1. Two-Space Architecture

### 1.1 Overview — The AI Context Hub

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    TWO-SPACE ARCHITECTURE                                   │
│                    "AI Governance at Scale"                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────┐  ┌─────────────────────────────────┐  │
│  │      space_framework            │  │       space_project             │  │
│  │   (Reusable Governance)         │  │   (Project-Specific)            │  │
│  ├─────────────────────────────────┤  ├─────────────────────────────────┤  │
│  │                                 │  │                                 │  │
│  │  • Workflow rules (ENFORCED)    │  │  • Project architecture        │  │
│  │  • Role definitions             │  │  • Tech stack guides           │  │
│  │  • State machine (NO SKIP)      │  │  • Local runbooks              │  │
│  │  • DoR/DoD templates            │  │  • Domain patterns             │  │
│  │  • Issue/PR templates           │  │  • External references         │  │
│  │  • Quick start guides           │  │  • Team-specific context       │  │
│  │  • Compliance standards         │  │  • Service documentation       │  │
│  │                                 │  │                                 │  │
│  │  Distribution: Submodule        │  │  Distribution: Per-project     │  │
│  │  Updates: Governance team       │  │  Updates: Project team         │  │
│  │  Audience: ALL agents           │  │  Audience: Project agents      │  │
│  │                                 │  │                                 │  │
│  │   AUTHORITATIVE               │  │   CONTEXTUAL                 │  │
│  │  Rules cannot be overridden     │  │  Extends, never overrides      │  │
│  │                                 │  │                                 │  │
│  └─────────────────────────────────┘  └─────────────────────────────────┘  │
│                                                                             │
│  Agent Load Order: space_framework -> space_project                          │
│  If conflict: space_framework wins (governance is authoritative)            │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.2 Design Principles — Built for Scale

| Principle | Description | Competitive Edge |
|-----------|-------------|------------------|
| **Separation of Concerns** | Governance in framework; project specifics in project space | Clean scaling |
| **Single Source of Truth** | Framework rules are authoritative; project extends, never overrides | No governance drift |
| **Role-Based Navigation** | Entry points by role; agents load relevant context | Least privilege for AI |
| **Minimal Duplication** | Space summarizes and links; full docs in git source | Maintainable at scale |
| **Scalability** | Structure supports 100+ files without overwhelming agents | Enterprise-ready |
| **Compliance-First** | Embedded regulatory mappings (HIPAA, SOX, CMMC) | Auditors love it |

---

## 2. space_framework Structure

### 2.1 Directory Layout - The Governance Library

```
spaces/space_framework/
|-- README.md                          (Master entry point - START HERE)
|-- VISION.md                          (Vision context)
|-- Quick_Start/                       (Role-based entry points)
|   |-- README.md                      (Role picker - "I am a...")
|   |-- Im_a_Client.md                 (Client: Submit ideas, approve work)
|   |-- Im_a_PO.md                     (PO: Define requirements, prioritize)
|   |-- Im_a_PM.md                     (PM: Coordinate, assign, track)
|   |-- Im_an_Architect.md             (Architect: Design, validate, guide)
|   |-- Im_an_Implementer.md           (IC: Build, test, PR)
|   |-- Im_a_Reviewer.md               (IC: Review, approve, block)
|   |-- Im_DevOps.md                   (DevOps: Deploy, release, monitor)
|   |-- Im_a_PenTester.md              (Security: Scan, approve, document)
|   |-- Im_a_CODEOWNER.md              (CODEOWNER: final merge authority)
|   `-- Im_Stuck.md                    (Troubleshooting - common blockers)
|-- Rules/                             (ENFORCED governance rules)
|   |-- README.md                      (Rules index - all rules linked)
|   |-- State_Machine.md               (Workflow states & transitions)
|   |-- Definition_of_Ready.md         (DoR checklist - enforced)
|   |-- Definition_of_Done.md          (DoD checklist - enforced)
|   |-- Artifact_Linking.md            (Traceability rules - enforced)
|   |-- Approval_Gates.md              (Who approves what - enforced)
|   |-- Branch_Naming.md               (Git conventions - enforced)
|   |-- CODEOWNER_Merge_Only.md        (critical rule)
|   `-- AI_Agent_Boundaries.md         (What AI can/cannot do)
|-- Roles/                             (Detailed role definitions)
|   |-- README.md                      (Role matrix)
|   |-- Client.md
|   |-- Product_Owner.md
|   |-- Project_Manager.md
|   |-- Architect.md
|   |-- IC_Implementer.md
|   |-- IC_Reviewer.md
|   |-- DevOps.md
|   |-- PenTester.md
|   `-- CODEOWNER.md
|-- Compliance/                        (Compliance mappings & standards)
|   |-- README.md
|   |-- Security_Standards.md
|   |-- HIPAA.md
|   |-- SOX.md
|   |-- CMMC.md
|   |-- FedRAMP.md
|   `-- FDA_Part11.md
|-- Templates/                         (Issue and PR templates)
|   |-- README.md
|   |-- Idea_Template.md
|   |-- Epic_Template.md
|   |-- Story_Template.md
|   |-- PR_Template.md
|   |-- Definition_of_Ready.md
|   `-- Definition_of_Done.md
`-- Navigation/                        (Cross-links and entry points)
    |-- README.md
    |-- Role_Index.md
    `-- Compliance_Index.md
```

### 2.2 Role-Based Entry Points

#### Quick_Start/Im_an_Implementer.md

```markdown
# Quick Start: I'm an Implementer

**Your Role:** Build features with high quality, security, and test coverage.

## Load Context
1. You are in `space_framework/` (governance rules)
2. Also load `space_project/` (tech stack, patterns)

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
- Celebrate (optional)

## Key Rules
- Cannot self-approve PRs
- Cannot merge (CODEOWNER only)
- Can request reviews
- Can approve peer PRs

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
|-- README.md                          (Project overview)
|-- Architecture/
|   |-- README.md                      (Architecture index)
|   |-- System_Overview.md             (High-level design)
|   |-- Component_Diagram.md           (Service boundaries)
|   |-- Data_Model.md                  (Database schema)
|   |-- API_Contracts.md               (API specifications)
|   `-- ADR/                           (Architecture Decision Records)
|       |-- 001_Database_Selection.md
|       |-- 002_Auth_Strategy.md
|       `-- Template.md
|-- Tech_Stack/
|   |-- README.md                      (Tech stack overview)
|   |-- Backend/
|   |   |-- Language_Guide.md          (Python/Go/etc.)
|   |   |-- Framework_Patterns.md      (FastAPI/Django/etc.)
|   |   |-- Testing_Patterns.md
|   |   `-- Database_Guide.md
|   |-- Frontend/
|   |   |-- Framework_Guide.md         (React/Vue/etc.)
|   |   |-- Component_Patterns.md
|   |   |-- State_Management.md
|   |   `-- Testing_Guide.md
|   `-- Infrastructure/
|       |-- Cloud_Provider.md          (AWS/Azure/GCP)
|       |-- Container_Guide.md         (Docker/K8s)
|       `-- CI_CD_Pipeline.md
|-- Runbooks/
|   |-- README.md                      (Runbook index)
|   |-- Local_Development.md           (Dev environment setup)
|   |-- Build_and_Test.md              (Build commands)
|   |-- Deployment.md                  (Deploy procedures)
|   |-- Troubleshooting.md             (Common issues)
|   `-- Incident_Response.md           (Production issues)
|-- External_References/
|   |-- README.md                      (Reference index)
|   |-- Industry_Standards.md          (OWASP, etc.)
|   |-- OSS_Patterns.md                (Open source references)
|   `-- Regulatory_Context.md          (Sector-specific)
`-- _Team/
    |-- README.md                      (Team notes - internal)
    |-- Onboarding.md
    `-- Contact_List.md
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

**Workflow:** Idea -> Approved -> Ready -> In Progress -> In Review -> Done -> Released

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
| Workflow rules | Yes Summary + link | No | Yes Full details |
| Role guides | Yes Quick start | No | Yes Full definition |
| Templates | Yes Index + link | No | Yes Templates |
| Tech stack | No | Yes Full guides | No |
| Architecture | No | Yes Full design | Optional |
| Runbooks | No | Yes Full procedures | No |
| Compliance | Yes Standards | Yes Sector-specific | Yes Mapping |
| ADRs | No | Yes Project ADRs | Optional |

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



