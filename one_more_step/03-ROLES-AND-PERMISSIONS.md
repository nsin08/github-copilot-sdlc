# Roles and Permissions
## GitHub-Native SDLC Governance Platform

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Authors:** Project Manager + Architect  
**Status:** Approved  

---

## 1. Role Definitions

### 1.1 Role Summary

| Role | Primary Responsibility | Reports To | GitHub Team |
|------|----------------------|------------|-------------|
| **Client** | Define business needs, approve ideas | External | @org/clients |
| **Product Owner (PO)** | Define requirements, prioritize backlog | Client | @org/product |
| **Project Manager (PM)** | Coordinate delivery, manage resources | PO | @org/project |
| **Architect** | Design systems, validate feasibility | PM | @org/architecture |
| **IC (Implementer)** | Build & test features | Architect | @org/engineering |
| **IC (Reviewer)** | Review code, ensure quality | Architect | @org/engineering |
| **DevOps** | Manage deployments, infrastructure | PM | @org/devops |
| **PenTester** | Security review, vulnerability assessment | Architect | @org/security |
| **CODEOWNER** | Final merge authority, governance | Architect | @org/codeowners |

### 1.2 Detailed Role Descriptions

#### 1.2.1 Client

**Purpose:** External stakeholder who defines business needs and approves work.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Business outcomes, ROI, user value |
| **Key Activities** | Submit ideas, approve/reject ideas, validate delivered value |
| **Workflow States Owned** | Idea → Approved, Idea → Rejected |
| **Approvals Given** | Idea approval |
| **Approvals Needed From** | None (initiator) |
| **GitHub Permissions** | Triage (can create/label issues) |
| **Space Context** | space_framework/Quick_Start/Im_a_Client.md |

#### 1.2.2 Product Owner (PO)

**Purpose:** Translate business needs into technical requirements and manage backlog.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Requirements clarity, backlog prioritization, acceptance criteria |
| **Key Activities** | Create Epics from Ideas, define Stories, validate acceptance |
| **Workflow States Owned** | Approved → Ready (with Architect) |
| **Approvals Given** | Epic/Story approval, acceptance validation |
| **Approvals Needed From** | Client (for idea approval) |
| **GitHub Permissions** | Write (can create/edit issues, PRs) |
| **Space Context** | space_framework/Quick_Start/Im_a_PO.md |

#### 1.2.3 Project Manager (PM)

**Purpose:** Coordinate delivery, allocate resources, track progress.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Timeline, resources, risks, team coordination |
| **Key Activities** | Assign work, track progress, remove blockers, report status |
| **Workflow States Owned** | Ready → In Progress (resource assignment) |
| **Approvals Given** | Resource allocation, timeline decisions |
| **Approvals Needed From** | PO (priority), Architect (feasibility) |
| **GitHub Permissions** | Write (can manage projects, assign issues) |
| **Space Context** | space_framework/Quick_Start/Im_a_PM.md |

#### 1.2.4 Architect

**Purpose:** Design systems, validate technical feasibility, ensure quality.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | System design, scalability, maintainability, technical standards |
| **Key Activities** | Review architecture, validate feasibility, guide technical decisions |
| **Workflow States Owned** | Approved → Ready (technical validation) |
| **Approvals Given** | Technical feasibility, architecture decisions |
| **Approvals Needed From** | PO (requirements clarity) |
| **GitHub Permissions** | Write + CODEOWNER for architecture files |
| **Space Context** | space_framework/Quick_Start/Im_an_Architect.md |

#### 1.2.5 IC - Implementer

**Purpose:** Build features according to specifications, write tests.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Code quality, test coverage, security, standards compliance |
| **Key Activities** | Implement stories, write tests, open PRs, address review feedback |
| **Workflow States Owned** | In Progress → In Review (PR opened) |
| **Approvals Given** | None (implementer cannot self-approve) |
| **Approvals Needed From** | IC (Reviewer), PenTester (security), CODEOWNER (merge) |
| **GitHub Permissions** | Write (can create branches, PRs) |
| **Space Context** | space_framework/Quick_Start/Im_an_Implementer.md + space_project/Tech_Stack/ |

#### 1.2.6 IC - Reviewer

**Purpose:** Review code, ensure quality and standards compliance.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Code quality, correctness, test coverage, standards adherence |
| **Key Activities** | Review PRs, provide feedback, approve/request changes |
| **Workflow States Owned** | In Review → Done (with PenTester) |
| **Approvals Given** | Code review approval |
| **Approvals Needed From** | None for review; CODEOWNER for merge |
| **GitHub Permissions** | Write (can approve PRs) |
| **Space Context** | space_framework/Quick_Start/Im_a_Reviewer.md |

#### 1.2.7 DevOps

**Purpose:** Manage deployments, infrastructure, and release process.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Deployment reliability, infrastructure stability, release automation |
| **Key Activities** | Configure pipelines, manage releases, monitor deployments |
| **Workflow States Owned** | Done → Released |
| **Approvals Given** | Deployment authorization |
| **Approvals Needed From** | CODEOWNER (release approval) |
| **GitHub Permissions** | Write + CODEOWNER for infrastructure files |
| **Space Context** | space_framework/Quick_Start/Im_DevOps.md |

#### 1.2.8 PenTester

**Purpose:** Security review, vulnerability assessment, compliance validation.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Security vulnerabilities, OWASP compliance, threat modeling |
| **Key Activities** | Review security-sensitive PRs, validate security controls, report findings |
| **Workflow States Owned** | In Review (security approval required for security files) |
| **Approvals Given** | Security approval |
| **Approvals Needed From** | None for security review |
| **GitHub Permissions** | Write + CODEOWNER for security-sensitive files |
| **Space Context** | space_framework/Quick_Start/Im_a_PenTester.md |

#### 1.2.9 CODEOWNER

**Purpose:** Final merge authority, governance enforcement.

| Attribute | Description |
|-----------|-------------|
| **Primary Focus** | Code integrity, governance compliance, merge authorization |
| **Key Activities** | Final review, merge authorization, governance oversight |
| **Workflow States Owned** | In Review → Done (final approval), Done → Released |
| **Approvals Given** | Merge approval (required for all merges) |
| **Approvals Needed From** | IC (Reviewer) + PenTester (for security files) |
| **GitHub Permissions** | Admin or Maintain + CODEOWNER |
| **Space Context** | space_framework/Quick_Start/Im_a_CODEOWNER.md |

---

## 2. RACI Matrix

### 2.1 Workflow State Transitions

| Transition | Client | PO | PM | Architect | IC (Impl) | IC (Rev) | DevOps | PenTest | CODEOWNER |
|------------|--------|----|----|-----------|-----------|----------|--------|---------|-----------|
| Submit Idea | **R** | I | I | I | - | - | - | - | - |
| Idea → Approved | **A** | C | I | C | - | - | - | - | - |
| Idea → Rejected | **A** | C | I | - | - | - | - | - | - |
| Approved → Ready | I | **R** | C | **A** | - | - | - | - | - |
| Ready → In Progress | I | I | **A** | C | **R** | - | - | - | - |
| In Progress → In Review | - | I | I | I | **R** | - | - | - | - |
| In Review → Done | - | I | I | C | - | **R** | - | **R*** | **A** |
| Done → Released | I | I | C | I | - | - | **R** | - | **A** |

**Legend:** R = Responsible, A = Accountable, C = Consulted, I = Informed, * = Required for security-sensitive files

### 2.2 Artifact Creation

| Artifact | Client | PO | PM | Architect | IC (Impl) | IC (Rev) | DevOps | PenTest | CODEOWNER |
|----------|--------|----|----|-----------|-----------|----------|--------|---------|-----------|
| Idea | **R/A** | C | I | - | - | - | - | - | - |
| Epic | I | **R/A** | C | **C** | - | - | - | - | - |
| Story | I | **R/A** | C | C | - | - | - | - | - |
| PR | - | I | I | C | **R** | **A** | - | C* | **A** |
| Release | I | I | C | I | - | - | **R** | - | **A** |
| ADR | - | C | I | **R/A** | C | - | - | C | - |

### 2.3 Approval Decisions

| Decision | Client | PO | PM | Architect | IC (Impl) | IC (Rev) | DevOps | PenTest | CODEOWNER |
|----------|--------|----|----|-----------|-----------|----------|--------|---------|-----------|
| Idea Approval | **A** | C | - | - | - | - | - | - | - |
| Technical Feasibility | - | C | - | **A** | - | - | - | - | - |
| Resource Allocation | - | C | **A** | C | - | - | - | - | - |
| Code Review | - | - | - | C | - | **A** | - | - | - |
| Security Review | - | - | - | - | - | - | - | **A** | - |
| Merge | - | - | - | - | - | C | - | C | **A** |
| Deploy | - | - | C | - | - | - | **R** | - | **A** |

---

## 3. Permission Matrix

### 3.1 GitHub Repository Permissions

| Permission | Client | PO | PM | Architect | IC | DevOps | PenTest | CODEOWNER |
|------------|--------|----|----|-----------|-------|--------|---------|-----------|
| Read repo | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Create issues | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Label issues | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Assign issues | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Create branches | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Create PRs | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Approve PRs | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Merge PRs | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Manage Projects | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ✅ |
| Create Releases | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ |
| Admin settings | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |

### 3.2 Workflow State Permissions

| State Transition | Who Can Trigger | Enforcement |
|------------------|-----------------|-------------|
| → Idea | Anyone | Template validation |
| Idea → Approved | Client only | Label requires Client team membership |
| Idea → Rejected | Client, PO | Label requires team membership |
| Approved → Ready | Architect only | Automation checks Architect approval |
| Ready → In Progress | PM, IC | Requires assignment |
| In Progress → In Review | Automated | Triggered by PR open |
| In Review → Done | Automated | Triggered by approvals + merge |
| Done → Released | DevOps + CODEOWNER | Release workflow requires dual approval |

### 3.3 CODEOWNERS Configuration

```
# .github/CODEOWNERS
# Default: CODEOWNER approval required for all files
*                                   @org/codeowners

# Architecture files: Architect + CODEOWNER
/docs/architecture/**               @org/architecture @org/codeowners
/spaces/*/Architecture/**           @org/architecture @org/codeowners

# Security-sensitive files: PenTester + CODEOWNER
/src/auth/**                        @org/security @org/codeowners
/src/crypto/**                      @org/security @org/codeowners
/src/security/**                    @org/security @org/codeowners
*.pem                               @org/security @org/codeowners
*.key                               @org/security @org/codeowners
*.secret                            @org/security @org/codeowners

# Infrastructure files: DevOps + CODEOWNER
/infrastructure/**                  @org/devops @org/codeowners
/.github/workflows/**               @org/devops @org/codeowners
/Dockerfile                         @org/devops @org/codeowners
/docker-compose*.yml                @org/devops @org/codeowners
/k8s/**                             @org/devops @org/codeowners

# Governance files: CODEOWNER only
/.github/CODEOWNERS                 @org/codeowners
/.github/copilot-instructions*.md   @org/codeowners
/spaces/space_framework/**          @org/codeowners

# Documentation: Tech leads + CODEOWNER
/docs/**                            @org/tech-leads @org/codeowners
/README.md                          @org/tech-leads @org/codeowners

# Spaces: Broader contribution
/spaces/space_project/**            @org/engineering @org/codeowners
```

---

## 4. Approval Gates

### 4.1 Gate Definitions

| Gate | Trigger | Required Approvers | Timeout | Escalation |
|------|---------|-------------------|---------|------------|
| **Idea Approval** | Idea submitted | Client (1) | 5 business days | PO follows up |
| **Technical Feasibility** | Epic created | Architect (1) | 3 business days | PM escalates |
| **Resource Allocation** | Story ready | PM (1) | 2 business days | PO escalates |
| **Code Review** | PR opened | IC Reviewer (1) | 2 business days | Architect assigns |
| **Security Review** | PR touches security files | PenTester (1) | 3 business days | Architect escalates |
| **Merge Approval** | Reviews complete | CODEOWNER (1) | 1 business day | PM escalates |
| **Release Approval** | Release requested | DevOps (1) + CODEOWNER (1) | 4 hours | PM escalates |

### 4.2 Gate Bypass Rules

| Scenario | Allowed Bypass | Approver | Audit Requirement |
|----------|---------------|----------|-------------------|
| Critical production fix | Yes | CODEOWNER + DevOps | Documented justification + post-mortem |
| Security vulnerability | Yes | PenTester + CODEOWNER | Immediate + follow-up PR within 24h |
| All other scenarios | No | N/A | N/A |

### 4.3 Self-Approval Prevention

| Rule | Enforcement | Consequence |
|------|-------------|-------------|
| PR author cannot be sole approver | Branch protection | PR cannot merge |
| PR author cannot be CODEOWNER approver | Automation check | PR flagged, requires different CODEOWNER |
| Self-approval attempt logged | Audit workflow | Security alert generated |

---

## 5. AI Agent Permissions

### 5.1 Agent Role Mapping

| Agent Role | Equivalent Human Role | Context Loaded | Restrictions |
|------------|----------------------|----------------|--------------|
| Implementer Agent | IC (Implementer) | space_framework + space_project/Tech_Stack | Cannot approve, cannot merge |
| Reviewer Agent | IC (Reviewer) | space_framework + space_project | Cannot merge (human CODEOWNER required) |
| Architect Agent | Architect | space_framework + space_project/Architecture | Cannot bypass feasibility check |
| DevOps Agent | DevOps | space_framework + space_project/Infrastructure | Cannot deploy without human approval |

### 5.2 Agent Constraints

| Constraint | Enforcement | Rationale |
|------------|-------------|-----------|
| Agents cannot merge PRs | Branch protection (human required) | Compliance requirement |
| Agents cannot approve own work | Same as human IC | Quality assurance |
| Agent actions are logged | Audit workflow | Traceability |
| Agents follow same workflow | State machine enforcement | Consistency |
| Agents load role-appropriate context | Space design | Separation of concerns |

---

## 6. GitHub Team Structure

### 6.1 Recommended Team Hierarchy

```
@org/
├── clients                    (External stakeholders)
├── product                    (Product Owners)
├── project                    (Project Managers)
├── architecture               (Architects)
├── engineering                (ICs - Implementers & Reviewers)
│   ├── backend
│   ├── frontend
│   └── platform
├── devops                     (DevOps Engineers)
├── security                   (PenTesters, Security Engineers)
├── tech-leads                 (Senior engineers with doc access)
└── codeowners                 (Merge authority - small group)
```

### 6.2 Team Membership Guidelines

| Team | Membership Criteria | Typical Size |
|------|---------------------|--------------|
| @org/clients | External stakeholders with business need | 1-5 per project |
| @org/product | Product management role | 1-2 per project |
| @org/project | Project management role | 1-2 per project |
| @org/architecture | Senior technical role, system design responsibility | 1-3 per project |
| @org/engineering | All developers | 5-50 per project |
| @org/devops | Infrastructure/deployment responsibility | 2-5 per project |
| @org/security | Security-trained engineers | 1-3 per org |
| @org/codeowners | Highest trust, governance responsibility | 2-5 per project |

---

## 7. Conflict Resolution

### 7.1 Escalation Path

| Conflict Type | Resolution Authority | Escalation Path |
|---------------|---------------------|-----------------|
| Priority disagreement | PO | PO → Client |
| Resource conflict | PM | PM → PO → Client |
| Technical disagreement | Architect | Architect → CODEOWNER |
| Security exception | PenTester | PenTester → CODEOWNER → Client |
| Process exception | CODEOWNER | CODEOWNER → PM → Client |

### 7.2 Deadlock Resolution

If approval is blocked for > timeout period:
1. PM notifies escalation authority
2. Escalation authority has 1 business day to decide
3. Decision is logged with justification
4. If still unresolved, CODEOWNER makes final call

---

**Previous Document:** [02-ARCHITECTURE.md](02-ARCHITECTURE.md)  
**Next Document:** [04-WORKFLOW-SPECIFICATION.md](04-WORKFLOW-SPECIFICATION.md)
