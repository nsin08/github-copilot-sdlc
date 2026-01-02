# Boards and Tracking
## SDLC Governance Control Plane — Operational Command Center

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Project Manager  
**Status:** Approved  
**Audience:** Product / sales / diligence  
**Classification:** Confidential — Competitive Strategy  

---

## Strategic Context

> **Visibility supports control, and control supports compliance.**

GitHub Projects becomes the single source of truth for work status, metrics, and reporting:

| Competitor Approach | Our Approach | Business Impact |
|---------------------|--------------|-----------------|
| Multiple dashboards across tools | Single GitHub Projects view | Zero context switching |
| Manual status updates | Automated from workflow state | Always accurate |
| Custom reporting builds | Built-in views + export | Instant audit response |
| Disconnected from code | Same platform as development | True traceability |

**The Boards Architecture enables:**
- Real-time workflow visibility
- Automated metrics collection
- One-click compliance reporting
- Executive dashboards without BI tools

---

## 1. GitHub Projects Overview

### 1.1 Project Structure — The Command Center

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    GITHUB PROJECTS — COMMAND CENTER                         │
│                    "Single Source of Truth"                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Project: "SDLC Governance Control Plane"                                   │
│  ════════════════════════════════════════                                   │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                           VIEWS                                      │  │
│  ├──────────────────┬──────────────────┬──────────────────┬────────────┤  │
│  │ 🗂️ Kanban        │ 📅 Roadmap       │ 📋 Backlog       │ 📊 Metrics │  │
│  │ (Current Work)   │ (Timeline)       │ (All Items)      │ (KPIs)     │  │
│  ├──────────────────┼──────────────────┼──────────────────┼────────────┤  │
│  │ Columns:         │ Grouped by:      │ Table with:      │ Charts:    │  │
│  │ • Idea           │ • Phase          │ • Type           │ • Lead Time│  │
│  │ • Approved       │ • Milestone      │ • State          │ • Cycle    │  │
│  │ • Ready          │                  │ • Assignee       │ • Rework   │  │
│  │ • In Progress    │ Timeline:        │ • Points         │ • Velocity │  │
│  │ • In Review      │ • Week/Month     │ • Priority       │ • Burndown │  │
│  │ • Done           │                  │ • Due Date       │            │  │
│  │ • Released       │                  │                  │            │  │
│  │ • BLOCKED ⚠️     │                  │                  │            │  │
│  └──────────────────┴──────────────────┴──────────────────┴────────────┘  │
│                                                                             │
│  Custom Fields (Synced with Labels):                                        │
│  ├── State (single select): Enforced workflow states                       │
│  ├── Type (single select): Idea, Epic, Story, Task                         │
│  ├── Priority (single select): P0 Critical, P1 High, P2 Medium, P3 Low    │
│  ├── Points (number): Story points for velocity                            │
│  ├── Phase (single select): Phase 1, Phase 2, Phase 3                     │
│  ├── Sprint (iteration): 2-week sprints                                    │
│  ├── Lead Time (calculated): PR open → merge (hours)                      │
│  └── Cycle Time (calculated): Issue open → release (days)                 │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.2 Views Configuration — Purpose-Built for Each Audience

#### Kanban View — Engineering Daily Operations

**Purpose:** Track current work by workflow state — the primary engineering view

**Columns (Enforced State Machine):**
| Column | Filter | WIP Limit | Color | Alert |
|--------|--------|-----------|-------|-------|
| Idea | `state:idea` | 10 | Gray | — |
| Approved | `state:approved` | 5 | Blue | — |
| Ready | `state:ready` | 10 | Green | — |
| In Progress | `state:in-progress` | **5** | Yellow | WIP exceeded |
| In Review | `state:in-review` | **5** | Orange | > 48h = stale |
| Done | `state:done` | ∞ | Purple | — |
| Released | `state:released` | ∞ | Teal | — |
| **Blocked** | `state:blocked` | ∞ | **Red** | **Immediate** |

**Swimlanes:** By Assignee (shows individual workload)

**Competitive Edge:** Single view matches state machine — no drift between board and reality

#### Roadmap View — Executive/Client View

**Purpose:** Timeline-based planning for leadership and client communication

**Configuration:**
- X-axis: Time (weeks or months)
- Grouping: Phase → Milestone
- Color: Priority
- Filter: type:epic OR type:story

**Layout:**
```
        Jan W1      Jan W2      Jan W3      Jan W4      Feb W1
Phase 1 ├── Epic 1: Core Workflow ────────────────────────────┤
        │   ├── Story 1.1: State Machine ────────┤
        │   ├── Story 1.2: Role Definitions ──────────────┤
        │   └── Story 1.3: Space Structure ────────────────────┤
Phase 2                                     ├── Epic 2: Enforcement ─...
```

**Competitive Edge:** Executives see progress without separate tools or BI

#### Backlog View — PO/PM Planning

**Purpose:** Full list of all work items with filtering

**Table Columns:**
| Column | Field | Sortable |
|--------|-------|----------|
| Title | Title | Yes |
| Type | type label | Yes |
| State | state label | Yes |
| Assignee | Assignees | Yes |
| Points | Points field | Yes |
| Priority | Priority field | Yes |
| Sprint | Sprint field | Yes |
| Due Date | Due Date field | Yes |
| Phase | Phase field | Yes |

**Default Sort:** Priority (descending), then Due Date (ascending)

**Saved Filters:**
- "My Items": `assignee:@me`
- "Blocked": `state:blocked`
- "Ready to Pick Up": `state:ready no:assignee`
- "Needs Review": `state:in-review`
- "This Sprint": `sprint:@current`

#### Metrics View

**Purpose:** Track KPIs and performance indicators

**Charts (Custom Build):**
1. **Lead Time Trend** (line chart)
   - X: Week
   - Y: Average days (PR open → merge)
   
2. **Cycle Time Trend** (line chart)
   - X: Week
   - Y: Average days (Issue open → release)
   
3. **Velocity** (bar chart)
   - X: Sprint
   - Y: Story points completed
   
4. **Burndown** (line chart)
   - X: Sprint day
   - Y: Remaining story points
   
5. **State Distribution** (pie chart)
   - Slices: Stories by current state

---

## 2. Custom Fields Configuration

### 2.1 Field Definitions

```yaml
# Project Custom Fields

fields:
  - name: State
    type: single_select
    options:
      - { name: "Idea", color: gray }
      - { name: "Approved", color: blue }
      - { name: "Ready", color: green }
      - { name: "In Progress", color: yellow }
      - { name: "In Review", color: orange }
      - { name: "Done", color: purple }
      - { name: "Released", color: teal }
      - { name: "Rejected", color: red }
      - { name: "On Hold", color: gray }
      - { name: "Blocked", color: red }
    
  - name: Type
    type: single_select
    options:
      - { name: "Idea", color: blue }
      - { name: "Epic", color: purple }
      - { name: "Story", color: green }
      - { name: "Task", color: gray }
      - { name: "Bug", color: red }
      
  - name: Priority
    type: single_select
    options:
      - { name: "P0 - Critical", color: red }
      - { name: "P1 - High", color: orange }
      - { name: "P2 - Medium", color: yellow }
      - { name: "P3 - Low", color: gray }
      
  - name: Points
    type: number
    
  - name: Phase
    type: single_select
    options:
      - { name: "Phase 1 - Foundation", color: blue }
      - { name: "Phase 2 - Enforcement", color: green }
      - { name: "Phase 3 - Production", color: purple }
      
  - name: Sprint
    type: iteration
    duration: 14  # 2 weeks
    
  - name: Due Date
    type: date
    
  - name: Lead Time (days)
    type: number
    description: "Calculated: PR open → merge"
    
  - name: Cycle Time (days)
    type: number
    description: "Calculated: Issue open → release"
```

### 2.2 Field Sync with Labels

| Project Field | Issue Label | Sync Direction |
|---------------|-------------|----------------|
| State | `state:*` | Bidirectional |
| Type | `type:*` | Bidirectional |
| Priority | `priority:*` | Bidirectional |
| Phase | `phase:*` | Label → Field |

**Automation (GitHub Action):**
```yaml
name: Sync Labels to Project Fields
on:
  issues:
    types: [labeled, unlabeled]

jobs:
  sync-to-project:
    runs-on: ubuntu-latest
    steps:
      - name: Update project field
        uses: actions/github-script@v7
        with:
          script: |
            const label = context.payload.label.name;
            
            // Parse label type (e.g., "state:ready" → field: State, value: Ready)
            const [fieldName, value] = label.split(':');
            if (!value) return;
            
            // Update project item
            // (Project GraphQL mutation)
```

---

## 3. Automation Rules

### 3.1 Project Automation

| Trigger | Action | Configuration |
|---------|--------|---------------|
| Issue created with `type:story` | Add to project | Auto-add to backlog |
| Label `state:ready` added | Move to Ready column | Kanban automation |
| PR opened | Move linked issue to In Review | Workflow automation |
| PR merged | Move linked issue to Done | Workflow automation |
| Release published | Move issues to Released | Release automation |
| Issue assigned | Update Assignee field | Field sync |
| Issue closed | Calculate cycle time | Metrics automation |

### 3.2 Built-in Workflows

**Enable these in Project Settings → Workflows:**

1. **Item added to project**
   - Set: Status = Idea (default)
   
2. **Item reopened**
   - Set: Status = In Progress
   
3. **Item closed**
   - Set: Status = Done
   
4. **Pull request merged**
   - Set: Status = Done
   
5. **Reviewers requested**
   - Set: Status = In Review

### 3.3 Custom Automation (GitHub Actions)

```yaml
name: Project Automation
on:
  issues:
    types: [opened, labeled, closed]
  pull_request:
    types: [opened, closed]
  release:
    types: [published]

jobs:
  update-project:
    runs-on: ubuntu-latest
    steps:
      - name: Add new issue to project
        if: github.event_name == 'issues' && github.event.action == 'opened'
        uses: actions/add-to-project@v0.5.0
        with:
          project-url: https://github.com/orgs/ORG/projects/1
          github-token: ${{ secrets.PROJECT_TOKEN }}
          
      - name: Calculate lead time on PR merge
        if: github.event_name == 'pull_request' && github.event.action == 'closed' && github.event.pull_request.merged
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;
            const createdAt = new Date(pr.created_at);
            const mergedAt = new Date(pr.merged_at);
            const leadTimeHours = (mergedAt - createdAt) / (1000 * 60 * 60);
            const leadTimeDays = (leadTimeHours / 24).toFixed(2);
            
            console.log(`Lead Time: ${leadTimeDays} days`);
            
            // Store metric
            // (append to .metrics/lead-time.jsonl)
```

---

## 4. Dashboard Queries

### 4.1 Status Report Query

```graphql
query GetProjectStatus {
  organization(login: "ORG") {
    projectV2(number: 1) {
      items(first: 100) {
        nodes {
          fieldValueByName(name: "State") {
            ... on ProjectV2ItemFieldSingleSelectValue {
              name
            }
          }
          fieldValueByName(name: "Points") {
            ... on ProjectV2ItemFieldNumberValue {
              number
            }
          }
          content {
            ... on Issue {
              title
              assignees(first: 5) {
                nodes { login }
              }
            }
          }
        }
      }
    }
  }
}
```

### 4.2 Velocity Calculation

```javascript
// Calculate velocity per sprint
const calculateVelocity = (items, sprintName) => {
  return items
    .filter(item => item.sprint === sprintName && item.state === 'Done')
    .reduce((sum, item) => sum + (item.points || 0), 0);
};
```

### 4.3 Lead Time Report

```javascript
// Generate lead time report
const leadTimeReport = async () => {
  const prs = await getPRsInRange(startDate, endDate);
  
  const leadTimes = prs.map(pr => ({
    pr: pr.number,
    created: pr.created_at,
    merged: pr.merged_at,
    leadTimeDays: daysBetween(pr.created_at, pr.merged_at)
  }));
  
  return {
    average: average(leadTimes.map(lt => lt.leadTimeDays)),
    median: median(leadTimes.map(lt => lt.leadTimeDays)),
    p90: percentile(leadTimes.map(lt => lt.leadTimeDays), 90),
    items: leadTimes
  };
};
```

---

## 5. Reporting Templates

### 5.1 Sprint Report

```markdown
# Sprint Report: Sprint [N]

**Period:** [Start Date] - [End Date]  
**Sprint Goal:** [Goal Description]  

## Summary

| Metric | Planned | Actual | Status |
|--------|---------|--------|--------|
| Stories | [X] | [Y] | ✅/⚠️/❌ |
| Points | [X] | [Y] | ✅/⚠️/❌ |
| Lead Time (avg) | [X] days | [Y] days | ✅/⚠️/❌ |
| Rework Rate | < 10% | [Y]% | ✅/⚠️/❌ |

## Completed Stories

| ID | Title | Points | Assignee |
|----|-------|--------|----------|
| #[N] | [Title] | [X] | @[user] |

## Carried Over

| ID | Title | Points | Reason |
|----|-------|--------|--------|
| #[N] | [Title] | [X] | [Reason] |

## Blockers Encountered

| Issue | Blocker | Resolution | Duration |
|-------|---------|------------|----------|
| #[N] | [Description] | [Resolution] | [X] days |

## Retrospective Actions

- [ ] [Action 1] - Owner: @[user]
- [ ] [Action 2] - Owner: @[user]

## Next Sprint Focus

- [Priority 1]
- [Priority 2]
```

### 5.2 Executive Summary

```markdown
# Executive Summary: [Month/Quarter]

**Reporting Period:** [Start] - [End]  
**Project Status:** 🟢 On Track / 🟡 At Risk / 🔴 Blocked  

## Key Metrics

| Metric | Target | Actual | Trend |
|--------|--------|--------|-------|
| Velocity (pts/sprint) | [X] | [Y] | ↑/→/↓ |
| Lead Time (days) | < [X] | [Y] | ↑/→/↓ |
| Cycle Time (days) | < [X] | [Y] | ↑/→/↓ |
| Rework Rate | < [X]% | [Y]% | ↑/→/↓ |
| Security Gate Pass | > [X]% | [Y]% | ↑/→/↓ |

## Phase Progress

| Phase | Status | Completion | Due |
|-------|--------|------------|-----|
| Phase 1: Foundation | ✅ Complete | 100% | Week 8 |
| Phase 2: Enforcement | 🔄 In Progress | [X]% | Week 16 |
| Phase 3: Production | ⏳ Planned | 0% | Week 24 |

## Milestone Status

- [x] M1: Framework Ready (Week 8)
- [ ] M2: Enforcement Active (Week 16)
- [ ] M3: Production Ready (Week 24)

## Risks & Issues

| ID | Description | Impact | Status |
|----|-------------|--------|--------|
| R1 | [Description] | [H/M/L] | [Mitigated/Open] |

## Decisions Required

1. [Decision needed] - Owner: [Role]
```

### 5.3 Compliance Report

```markdown
# Compliance Report: [Release Version]

**Report Date:** [Date]  
**Release:** [v1.0.0]  
**Auditor:** [Name]  

## Traceability Summary

| Artifact Type | Count | Linked | Unlinked | Compliance |
|---------------|-------|--------|----------|------------|
| Ideas | [X] | [Y] | [Z] | [%] |
| Epics | [X] | [Y] | [Z] | [%] |
| Stories | [X] | [Y] | [Z] | [%] |
| PRs | [X] | [Y] | [Z] | [%] |

## Traceability Chain

| Idea | Epic | Stories | PRs | Release |
|------|------|---------|-----|---------|
| #[N] | #[N] | #[N], #[N] | #[N], #[N] | v1.0.0 |

## Approval Audit

| State Transition | Count | Authorized | Unauthorized |
|------------------|-------|------------|--------------|
| Idea → Approved | [X] | [Y] | [Z] |
| Approved → Ready | [X] | [Y] | [Z] |
| In Review → Done | [X] | [Y] | [Z] |
| Done → Released | [X] | [Y] | [Z] |

## Security Gate Results

| Check | Passed | Failed | Exceptions |
|-------|--------|--------|------------|
| Secret Detection | [X] | [Y] | [Z] |
| Dependency Scan | [X] | [Y] | [Z] |
| SAST | [X] | [Y] | [Z] |
| PenTest Approval | [X] | [Y] | [Z] |

## Audit Trail Verification

- [x] All state transitions logged
- [x] Logs signed and verified
- [x] No unauthorized merges
- [x] All approvals documented

## Sign-Off

| Role | Name | Date | Signature |
|------|------|------|-----------|
| CODEOWNER | | | |
| PenTester | | | |
| Architect | | | |
```

---

## 6. Setup Instructions

### 6.1 Create GitHub Project

1. Navigate to Organization → Projects → New Project
2. Select "Board" template
3. Name: "SDLC Governance Platform"
4. Add custom fields (see Section 2.1)
5. Create views (see Section 1.2)
6. Enable workflows (see Section 3.2)
7. Link repository

### 6.2 Configure Automation

1. Create `PROJECT_TOKEN` secret (PAT with `project` scope)
2. Add workflow files to `.github/workflows/`
3. Enable Actions for repository
4. Test automation with sample issue

### 6.3 Team Access

| Role | Project Permission |
|------|-------------------|
| Client | Read |
| PO | Write |
| PM | Admin |
| Architect | Write |
| IC | Write |
| DevOps | Write |
| PenTester | Write |
| CODEOWNER | Admin |

---

**Previous Document:** [06-PHASED-DELIVERY-PLAN.md](06-PHASED-DELIVERY-PLAN.md)  
**Next Document:** [08-COMPLIANCE-MAPPING.md](08-COMPLIANCE-MAPPING.md)
