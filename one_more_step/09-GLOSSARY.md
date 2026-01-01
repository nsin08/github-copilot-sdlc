# Glossary
## GitHub-Native SDLC Governance Platform

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Technical Writer  
**Status:** Approved  

---

## A

### Acceptance Criteria
Specific, testable conditions that must be met for a Story to be considered complete. Written in Given/When/Then format or as a checklist of verifiable outcomes.

### Actor
The GitHub user or automation that performs an action. Recorded in audit logs for traceability.

### Agent
An AI assistant (e.g., GitHub Copilot) that performs tasks within the workflow. Operates under the permissions of the user who invokes it.

### Approval Gate
A checkpoint in the workflow where a specific role must approve before work can proceed. Enforced via GitHub branch protection and Actions.

### Architect
Role responsible for system design, technical standards, and feasibility validation. Approves transition from Approved → Ready.

### Artifact
Any work product created in the SDLC: Issues (Ideas, Epics, Stories), Pull Requests, Commits, Releases.

### Artifact Linking
The requirement that all artifacts reference their parent in the hierarchy. Enforced via automation (Closes #N syntax).

### Audit Trail
Immutable, timestamped record of all state transitions, approvals, and actions. Stored in JSONL format with HMAC signatures.

---

## B

### Backlog
The complete list of work items (Epics, Stories, Tasks) not yet started or in progress. Managed via GitHub Projects.

### Blocked State
An exception state indicating work cannot proceed due to external dependency or impediment. Requires explicit unblocking action.

### Branch Protection
GitHub feature that enforces rules on branches: required reviews, status checks, restrictions on who can push/merge.

### Branching Strategy
The defined patterns for creating and managing Git branches. This platform uses: `feature/<issue-id>-slug`, `fix/<issue-id>-slug`, `release/*`, `hotfix/*`.

---

## C

### CI/CD
Continuous Integration / Continuous Deployment. Automated building, testing, and deployment of code changes.

### Client
Role representing the external or internal customer who provides business requirements. Initiates Ideas.

### CMMC
Cybersecurity Maturity Model Certification. DoD framework for assessing contractor cybersecurity practices. See [08-COMPLIANCE-MAPPING.md](08-COMPLIANCE-MAPPING.md).

### CODEOWNER
Role with exclusive authority to merge Pull Requests to protected branches. Defined in CODEOWNERS file.

### CODEOWNERS File
GitHub file (`.github/CODEOWNERS`) that specifies individuals or teams responsible for code in specific paths. Used for automatic review requests.

### Compliance
Adherence to regulatory, legal, or organizational requirements. This platform targets HIPAA, SOX, PCI-DSS, NIST 800-171, CMMC, FedRAMP.

### Copilot Space
A GitHub Copilot feature for storing context documents that agents can reference. This platform uses two spaces: `space_framework` (governance) and `space_project` (project-specific).

### CUI
Controlled Unclassified Information. Government information requiring safeguarding (NIST 800-171).

### Cycle Time
Metric: Time from issue creation to release. Measures end-to-end delivery speed.

---

## D

### Definition of Done (DoD)
Checklist of criteria that must be satisfied before work can be marked complete and merged. Enforced via PR template and Actions.

### Definition of Ready (DoR)
Checklist of criteria that must be satisfied before work can begin (transition to In Progress). Enforced via issue template and Actions.

### DevOps
Role responsible for deployment, infrastructure, and release management. Approves transition from Done → Released.

### DPIA
Data Protection Impact Assessment. GDPR requirement for assessing privacy risks.

### Draft PR
A Pull Request marked as not ready for review. Used for early feedback before formal review.

---

## E

### Enforcement
System-level blocking of actions that violate workflow rules. Not advisory—violations are prevented by automation.

### Epic
A large work item representing a feature or initiative. Contains multiple Stories. Created by Tech Lead from Idea.

### Evidence
Concrete proof that acceptance criteria are met: test names, file paths, screenshots, logs. Required in PR description.

---

## F

### FDA 21 CFR Part 11
FDA regulation for electronic records and signatures. Requires validated systems, audit trails, and electronic signatures.

### FedRAMP
Federal Risk and Authorization Management Program. Security framework for cloud services used by federal agencies.

### First-Pass Rate
Metric: Percentage of PRs approved on first review without changes. Measures quality of submissions.

---

## G

### Gate
See Approval Gate.

### GDPR
General Data Protection Regulation. EU regulation for data privacy and protection.

### Governance
The framework of rules, practices, and processes that control how software is developed and delivered.

### Guard Rails
Automated constraints that prevent incorrect or risky actions. Examples: blocking merge without approval, requiring linked issues.

---

## H

### Happy Path
The standard workflow when all conditions are met and no exceptions occur. Designed to be "touchless" (minimal manual intervention).

### HIPAA
Health Insurance Portability and Accountability Act. US regulation for protecting health information (PHI).

### HMAC
Hash-based Message Authentication Code. Cryptographic signature used to verify audit log integrity.

### Hotfix
Urgent fix applied directly to production branch. Follows abbreviated workflow with post-hoc documentation.

---

## I

### IC (Implementer)
Individual Contributor role responsible for writing code, tests, and documentation. Creates PRs for Stories.

### IC (Reviewer)
Individual Contributor role responsible for code review. Validates PRs against DoD before CODEOWNER merge.

### Idea
The initial work item representing a business need or problem. Created by Client or PO. First state in workflow.

### Immutable
Cannot be changed after creation. Audit logs are immutable—entries are appended, never modified.

### In Progress
Workflow state indicating active development. Entered when IC begins work on a Story.

### In Review
Workflow state indicating PR is open and awaiting review. Entered when PR is created.

### Issue
GitHub feature for tracking work items. Used for Ideas, Epics, Stories, and Tasks in this platform.

### ITAR
International Traffic in Arms Regulations. US export control for defense articles.

---

## J

### JSONL
JSON Lines format. Each line is a valid JSON object. Used for audit logs for easy appending and parsing.

---

## K

### Kanban
Visual workflow management method. GitHub Projects Kanban view shows work items in columns by state.

---

## L

### Label
GitHub feature for categorizing issues/PRs. This platform uses prefixed labels: `state:*`, `type:*`, `priority:*`, `phase:*`.

### Lead Time
Metric: Time from PR creation to merge. Measures code review efficiency.

### Least Privilege
Security principle: users have only the minimum permissions needed for their role.

---

## M

### Merge
The action of incorporating PR changes into the target branch. Restricted to CODEOWNER in this platform.

### Metric
Quantitative measurement of process performance: Lead Time, Cycle Time, Rework Rate, Velocity, First-Pass Rate.

### Milestone
GitHub feature for grouping issues by target date or release. Used to track progress toward deliverables.

---

## N

### NIST 800-171
NIST Special Publication for protecting CUI. Required for DoD contractors.

### Non-Goal
Explicit statement of what is NOT in scope for a Story or Epic. Prevents scope creep.

---

## O

### On Hold
Exception state indicating work is paused but not blocked. Requires reason and expected resumption.

---

## P

### PAT
Personal Access Token. GitHub authentication credential with scoped permissions.

### PCI-DSS
Payment Card Industry Data Security Standard. Requirements for handling cardholder data.

### PenTester
Penetration Tester role. Approves security-related changes. Required for modifications to security-sensitive files.

### PHI
Protected Health Information. Health data protected under HIPAA.

### PM (Project Manager)
Role responsible for project coordination, timeline tracking, and stakeholder communication.

### PO (Product Owner)
Role responsible for product vision, requirements prioritization, and stakeholder representation. Approves Idea → Approved.

### PR
Pull Request. GitHub feature for proposing and reviewing code changes.

### Priority
Classification of work urgency: P0 (Critical), P1 (High), P2 (Medium), P3 (Low).

---

## Q

### QA
Quality Assurance. Validation activities to ensure deliverables meet requirements. Combined with Reviewer role in this platform.

---

## R

### RACI
Responsibility matrix: Responsible, Accountable, Consulted, Informed. Defines role involvement in activities.

### RBAC
Role-Based Access Control. Permissions assigned based on role, not individual user.

### Ready
Workflow state indicating Story meets DoR and can be picked up for implementation.

### Rejected
Exception state for Ideas/Stories that will not be implemented. Requires documented reason.

### Release
A versioned, deployable artifact. Created from Done Stories. Tagged with semantic version.

### Release Notes
Documentation accompanying a release describing changes, features, and fixes.

### Released
Final workflow state indicating code is deployed to production.

### Repository
Git storage for code and history. GitHub hosts repositories with additional features (Issues, PRs, Actions).

### Rework Rate
Metric: Percentage of PRs requiring changes after review. Measures first-pass quality.

### Roadmap
Visual timeline of planned work. GitHub Projects Roadmap view shows Epics/Stories on timeline.

### Role
A defined set of responsibilities and permissions. This platform defines: Client, PO, PM, Architect, IC (Impl), IC (Rev), DevOps, PenTester, CODEOWNER.

### Runbook
Step-by-step procedures for common operations. Stored in Copilot Space for agent reference.

---

## S

### SAST
Static Application Security Testing. Automated scanning of source code for vulnerabilities.

### SDLC
Software Development Life Cycle. The process of planning, developing, testing, and deploying software.

### Secret Scanning
GitHub feature that detects accidentally committed secrets (API keys, passwords).

### Security Gate
Approval checkpoint requiring PenTester sign-off for security-sensitive changes.

### Semantic Versioning
Version numbering: MAJOR.MINOR.PATCH. MAJOR = breaking changes, MINOR = new features, PATCH = fixes.

### Signed Commit
Git commit with cryptographic signature (GPG/SSH) proving author identity.

### SOC 2
Service Organization Control 2. Security audit framework for service providers.

### SOX
Sarbanes-Oxley Act. US regulation for financial reporting and internal controls.

### Space
See Copilot Space.

### Sprint
Time-boxed iteration (typically 2 weeks) for completing planned work.

### State
The current position of a work item in the workflow. States: Idea, Approved, Ready, In Progress, In Review, Done, Released, Rejected, On Hold, Blocked.

### State Machine
The defined states and valid transitions for work items. Enforced by automation.

### Story
A user-facing deliverable broken down from an Epic. Sized for completion in one sprint.

### Story Points
Relative measure of effort/complexity for a Story. Used for velocity calculation.

### Submodule
Git feature for including one repository inside another. Used to distribute governance framework.

### Success Criteria
See Acceptance Criteria.

---

## T

### Task
A sub-item of a Story for tracking internal work. Not user-facing.

### Tech Lead
Role responsible for technical design, Story breakdown, and feasibility validation. Creates Epics and Stories from Ideas.

### Template
Predefined structure for Issues or PRs. Ensures required information is captured.

### Touchless
Automation that proceeds without manual intervention when conditions are met.

### Traceability
The ability to trace any artifact to its origin and dependencies. Enabled by artifact linking.

### Transition
Movement from one workflow state to another. Requires specific conditions and approvals.

---

## U

### Unblocking
Action to resolve a blocked state and resume work.

---

## V

### Validation
Verification that requirements are correct and complete (DoR) or deliverables meet requirements (DoD).

### Velocity
Metric: Story points completed per sprint. Measures team capacity.

### Version
Identifier for a specific release. Follows semantic versioning (MAJOR.MINOR.PATCH).

---

## W

### WIP Limit
Work In Progress limit. Maximum items allowed in a workflow state to prevent overload.

### Workflow
The defined sequence of states and transitions for work items.

---

## Acronyms Quick Reference

| Acronym | Expansion |
|---------|-----------|
| AC | Access Control (NIST family) |
| API | Application Programming Interface |
| AU | Audit (NIST family) |
| CFR | Code of Federal Regulations |
| CHD | Cardholder Data |
| CI/CD | Continuous Integration / Continuous Deployment |
| CM | Configuration Management (NIST family) |
| CMMC | Cybersecurity Maturity Model Certification |
| CUI | Controlled Unclassified Information |
| DAST | Dynamic Application Security Testing |
| DoD | Definition of Done |
| DoR | Definition of Ready |
| DPO | Data Protection Officer |
| DPIA | Data Protection Impact Assessment |
| EU | European Union |
| FDA | Food and Drug Administration |
| FedRAMP | Federal Risk and Authorization Management Program |
| FISMA | Federal Information Security Modernization Act |
| GDPR | General Data Protection Regulation |
| GPG | GNU Privacy Guard |
| HHS | Health and Human Services |
| HIPAA | Health Insurance Portability and Accountability Act |
| HMAC | Hash-based Message Authentication Code |
| IC | Individual Contributor |
| IR | Incident Response (NIST family) |
| ITAR | International Traffic in Arms Regulations |
| JSON | JavaScript Object Notation |
| JSONL | JSON Lines |
| KPI | Key Performance Indicator |
| LMS | Learning Management System |
| MFA | Multi-Factor Authentication |
| NIST | National Institute of Standards and Technology |
| OCR | Office for Civil Rights |
| PAT | Personal Access Token |
| PCI-DSS | Payment Card Industry Data Security Standard |
| PHI | Protected Health Information |
| PM | Project Manager |
| PO | Product Owner |
| POA&M | Plan of Action and Milestones |
| PR | Pull Request |
| QA | Quality Assurance |
| QSA | Qualified Security Assessor |
| RA | Risk Assessment (NIST family) |
| RACI | Responsible, Accountable, Consulted, Informed |
| RBAC | Role-Based Access Control |
| SAST | Static Application Security Testing |
| SC | System and Communications Protection (NIST family) |
| SDLC | Software Development Life Cycle |
| SI | System and Information Integrity (NIST family) |
| SLA | Service Level Agreement |
| SOC | Service Organization Control |
| SOX | Sarbanes-Oxley Act |
| SSH | Secure Shell |
| SSO | Single Sign-On |
| TLS | Transport Layer Security |
| WIP | Work In Progress |

---

**Previous Document:** [08-COMPLIANCE-MAPPING.md](08-COMPLIANCE-MAPPING.md)  
**Index:** [00-EXECUTIVE-SUMMARY.md](00-EXECUTIVE-SUMMARY.md)
