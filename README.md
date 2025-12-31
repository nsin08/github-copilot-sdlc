# .github - Role-Based Workflow System

**Version:** 1.0  
**Status:** ✅ Production Ready

> **🎯 Designed for Submodule Use:** This repository contains a complete SDLC workflow template system that can be integrated into any GitHub project as a submodule. See [Quick Start](#quick-start) for integration instructions.

---

## Table of Contents

- [Using as Submodule](#using-as-submodule)

- [Using as Submodule](#using-as-submodule)
- [What This Is](#what-this-is)
- [Workflow Flowchart](#workflow-flowchart)
- [Directory Structure](#directory-structure)
- [Quick Start](#quick-start)
- [Quick Navigation](#quick-navigation)
- [Extending the System](#extending-the-system)
- [File Purposes](#file-purposes)
- [Naming Conventions](#naming-conventions)

---

## Using as Submodule

**Add this workflow system to your project:**

```bash
# Navigate to your project root
cd your-project/

# Add as submodule directly to .github directory
git submodule add https://github.com/nsin08/github-copilot-sdlc.git .github
git submodule update --init --recursive

# Commit
git add .github .gitmodules
git commit -m "Add SDLC workflow template system"
git push
```

**Update submodule to latest version:**

```bash
cd your-project/
git submodule update --remote --merge .github

# Review changes, then commit
git add .github
git commit -m "Update SDLC workflow template system"
git push
```

**Benefits of using as submodule:**
- ✅ Stay up-to-date with workflow improvements
- ✅ Consistent workflow across multiple projects
- ✅ Easy to customize locally while tracking upstream changes
- ✅ Revert to previous versions if needed

---

## What This Is

A **standalone, reusable `.github` directory** implementing a role-based software development lifecycle.

**Currently Implemented (5 Roles):**
1. **Sponsor/PO** → Creates Ideas with success criteria
2. **Tech Lead** → Breaks down into Epic + Stories
3. **Implementer** → Builds features, opens PRs
4. **Reviewer/QA** → Validates against criteria
5. **Release/DevOps** → Tags, releases, closes Epics

---

## Workflow Flowchart

<div align="center">

```mermaid
flowchart TD
    Start([New Idea]) --> PO["<b>Role: Sponsor/PO</b><br/>Create Idea Issue<br/>Define success criteria"]
    PO --> |State: Intake| TLReview["<b>Role: Tech Lead</b><br/>Review Idea Issue"]
    TLReview --> TLClear{"Requirements<br/>Clear?"}
    TLClear -->|No| POClarify["Request Clarification<br/>Ask specific questions"]
    POClarify --> PO
    TLClear -->|Yes| TLFeasible["Validate Feasibility<br/>with Implementers"]
    TLFeasible --> Feasible{"Feasible?"}
    Feasible -->|No - Too Risky| POClarify
    Feasible -->|Yes| TLCreate["Create Epic + Stories<br/>Add architecture notes"]
    TLCreate --> POValidate["Request PO Validation"]
    POValidate --> POApprove{"PO<br/>Approves?"}
    POApprove -->|No - Needs Adjustment| TLCreate
    POApprove -->|Yes + DoR Met| SpecReady[/"State: Spec Ready"/]
    SpecReady --> Impl["<b>Role: Implementer</b><br/>Create branch<br/>Implement + Tests + Docs"]
    Impl --> |State: In Progress| PR["Open PR<br/>Link to Issue & Epic<br/>Fill template"]
    PR --> QA["<b>Role: Reviewer/QA</b><br/>Validate against criteria<br/>Check tests & docs"]
    QA --> |State: In Review| Approve{"Approved?"}
    Approve -->|No| Changes["Request Changes<br/>Specific feedback"]
    Changes --> Impl
    Approve -->|Yes| Merge["Merge PR"]
    Merge --> |State: Done| Release["<b>Role: Release/DevOps</b><br/>Tag version<br/>Generate release notes"]
    Release --> |State: Released| End([Epic Closed])
    
    style PO fill:#e1f5ff
    style TLReview fill:#fff4e1
    style TLCreate fill:#fff4e1
    style TLFeasible fill:#fff4e1
    style POValidate fill:#e1f5ff
    style SpecReady fill:#d4edda
    style Impl fill:#e8f5e1
    style QA fill:#ffe1f5
    style Release fill:#f5e1ff
    style TLClear fill:#fff9e6
    style Feasible fill:#fff9e6
    style POApprove fill:#f0f0f0
    style Approve fill:#f0f0f0
```

</div>

**Key Workflow States:**
- 🟦 **Intake** → Idea exists, needs breakdown
- 🟨 **Spec Ready** → Epic & Stories defined, ready to implement
- 🟩 **In Progress** → Active development
- 🟪 **In Review** → PR open, awaiting validation
- ✅ **Done** → PR merged
- 🎉 **Released** → Version tagged, Epic closed

---

## Directory Structure

```
.github/
├── README.md                     # This file
├── copilot-instructions.md       # AI assistant context
│
├── ISSUE_TEMPLATE/               # Templates (GitHub required location)
│   ├── 00-pull-request-template.md  # (Copy to .github/PULL_REQUEST_TEMPLATE.md for auto-fill)
│   ├── 01-epic.md
│   ├── 02-story-task.md
│   └── 03-review-checklist.md
│
├── workflows/                    # CI/CD
│   └── test.yml
│
└── workflow-system/              # Modular documentation (extensible)
    ├── README.md                 # System overview
    ├── rules/                    # Workflow rules (7 files)
    ├── roles/                    # Role definitions (7 files)
    ├── guides/                   # User guides (5 files)
    └── examples/                 # Real examples (5 files)
```

---

## Quick Start

### 1. Add to Your Project

**Option A: Via Submodule (Recommended)**

```bash
# Add as submodule directly to .github
git submodule add https://github.com/nsin08/github-copilot-sdlc.git .github
git submodule update --init --recursive
```

**Option B: Direct Copy (Fork & Modify)**

```bash
# Copy directory
cp -r path/to/downloaded/.github your-project/
```

### 2. Customize

1. Edit `copilot-instructions.md` for your tech stack
2. Edit `workflows/test.yml` for your language
3. **Optional:** For PR template auto-fill, copy `ISSUE_TEMPLATE/00-pull-request-template.md` to `.github/PULL_REQUEST_TEMPLATE.md` (GitHub requires this specific location/name for auto-fill)

### 3. Test

```bash
# Create test issue
gh issue create
# Should show 3 templates (Epic, Story, Review)

# Create test PR
gh pr create
# Manually copy PR template content OR enable auto-fill (see step 2.3 above)
```

---

## Quick Navigation

| Need | Location |
|------|----------|
| **Rules** | |
| State machine (workflow states) | [workflow-system/rules/01-state-machine.md](workflow-system/rules/01-state-machine.md) |
| Definition of Ready | [workflow-system/rules/02-definition-of-ready.md](workflow-system/rules/02-definition-of-ready.md) |
| Definition of Done | [workflow-system/rules/03-definition-of-done.md](workflow-system/rules/03-definition-of-done.md) |
| PR hygiene | [workflow-system/rules/05-pr-hygiene.md](workflow-system/rules/05-pr-hygiene.md) |
| **Roles** | |
| All roles overview | [workflow-system/roles/00-index.md](workflow-system/roles/00-index.md) |
| PO prompt | [workflow-system/roles/01-sponsor-po.md](workflow-system/roles/01-sponsor-po.md) |
| Tech Lead prompt | [workflow-system/roles/02-tech-lead.md](workflow-system/roles/02-tech-lead.md) |
| Implementer prompt | [workflow-system/roles/03-implementer.md](workflow-system/roles/03-implementer.md) |
| QA prompt | [workflow-system/roles/04-reviewer-qa.md](workflow-system/roles/04-reviewer-qa.md) |
| Release prompt | [workflow-system/roles/05-release-devops.md](workflow-system/roles/05-release-devops.md) |
| **Guides** | |
| Integration guide | [workflow-system/guides/01-integration.md](workflow-system/guides/01-integration.md) |
| 30-min quick start | [workflow-system/guides/02-quick-start.md](workflow-system/guides/02-quick-start.md) |
| Customization | [workflow-system/guides/03-customization.md](workflow-system/guides/03-customization.md) |
| Enforcement & automation | [workflow-system/guides/04-enforcement-automation.md](workflow-system/guides/04-enforcement-automation.md) |
| **Examples** | |
| Epic breakdown | [workflow-system/examples/01-epic-breakdown.md](workflow-system/examples/01-epic-breakdown.md) |
| PR with evidence | [workflow-system/examples/02-pr-with-evidence.md](workflow-system/examples/02-pr-with-evidence.md) |
| QA review | [workflow-system/examples/03-qa-review.md](workflow-system/examples/03-qa-review.md) |
| Release notes | [workflow-system/examples/04-release-notes.md](workflow-system/examples/04-release-notes.md) |

---

## Extending the System

### Add a New Rule
1. Create `workflow-system/rules/07-your-rule.md`
2. Follow template in [workflow-system/rules/00-index.md](workflow-system/rules/00-index.md)

### Add a New Role
1. Create `workflow-system/roles/06-your-role.md`
2. Follow template in [workflow-system/roles/00-index.md](workflow-system/roles/00-index.md)

### Add a New Example
1. Create `workflow-system/examples/05-your-example.md`
2. Update index

---

## File Purposes

| File | Purpose | Required By |
|------|---------|-------------|
| `copilot-instructions.md` | AI context for Copilot | GitHub Copilot |
| `ISSUE_TEMPLATE/*.md` | Issue/PR templates | GitHub |
| `workflows/*.yml` | CI/CD automation | GitHub Actions |
| `workflow-system/` | Documentation (modular) | Humans & AI |

---

## Naming Conventions

| Pattern | Example | Usage |
|---------|---------|-------|
| `NN-name.md` | `01-state-machine.md` | Numbered files (ordered) |
| `00-index.md` | `rules/00-index.md` | Directory index |
| `00-shared-*.md` | `00-shared-context.md` | Shared/base content |
| `kebab-case` | `02-quick-start.md` | Multi-word with numbers |

---

**Full documentation:** [workflow-system/README.md](workflow-system/README.md)
