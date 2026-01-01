# Workflow Specification
## GitHub-Native SDLC Governance Platform

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Architect  
**Status:** Approved  

---

## 1. Workflow Overview

### 1.1 Core Workflow

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         GOVERNED WORKFLOW LIFECYCLE                         │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   CLIENT          PO/ARCHITECT         PM/IC            REVIEW           RELEASE
│     │                  │                 │                │                │
│     ▼                  ▼                 ▼                ▼                ▼
│  ┌──────┐         ┌─────────┐       ┌─────────┐      ┌─────────┐      ┌─────────┐
│  │ IDEA │────────▶│APPROVED │──────▶│  READY  │─────▶│IN PROG  │─────▶│IN REVIEW│
│  └──────┘         └─────────┘       └─────────┘      └─────────┘      └─────────┘
│     │                  │                 │                │                │
│     │                  │                 │                │                ▼
│     │                  │                 │                │           ┌─────────┐
│     │                  │                 │                │           │  DONE   │
│     │                  │                 │                │           └─────────┘
│     │                  │                 │                │                │
│     │                  │                 │                │                ▼
│     │                  │                 │                │           ┌─────────┐
│     │                  │                 │                │           │RELEASED │
│     │                  │                 │                │           └─────────┘
│     │                  │                 │                │
│     ▼                  ▼                 ▼                ▼
│  ┌──────┐         ┌─────────┐       ┌─────────┐      ┌─────────┐
│  │REJECT│         │ ON HOLD │       │ BLOCKED │      │ REWORK  │
│  └──────┘         └─────────┘       └─────────┘      └─────────┘
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.2 State Definitions

| State | Label | Description | Entry Criteria | Exit Criteria |
|-------|-------|-------------|----------------|---------------|
| **Idea** | `state:idea` | Initial submission | Idea template complete | Client approval or rejection |
| **Approved** | `state:approved` | Business value confirmed | Client approved | Architect validates feasibility |
| **Ready** | `state:ready` | Ready for implementation | DoR satisfied, Epic/Story linked | PM assigns, IC picks up |
| **In Progress** | `state:in-progress` | Active development | Resource assigned | PR opened |
| **In Review** | `state:in-review` | Code review active | PR linked to story | Reviews complete, tests pass |
| **Done** | `state:done` | Implementation complete | CODEOWNER merged | Release authorized |
| **Released** | `state:released` | In production | DevOps deployed | N/A (terminal) |
| **Rejected** | `state:rejected` | Not proceeding | Client rejected | N/A (terminal) |
| **On Hold** | `state:on-hold` | Paused | PM paused | PM resumes |
| **Blocked** | `state:blocked` | External dependency | IC flagged blocker | Blocker resolved |

---

## 2. State Transition Details

### 2.1 Idea → Approved

**Trigger:** Client adds label `state:approved`

**Preconditions:**
- [ ] Issue has `type:idea` label
- [ ] Issue has `state:idea` label
- [ ] Required fields completed (Business Value, Success Criteria)
- [ ] Client is member of @org/clients team

**Postconditions:**
- [ ] Label changed from `state:idea` to `state:approved`
- [ ] Audit log entry created
- [ ] PO notified for epic creation

**Automation (GitHub Action):**
```yaml
name: Idea Approval
on:
  issues:
    types: [labeled]

jobs:
  validate-approval:
    if: github.event.label.name == 'state:approved'
    runs-on: ubuntu-latest
    steps:
      - name: Check approver is Client
        uses: actions/github-script@v7
        with:
          script: |
            const { data: teams } = await github.rest.teams.listMembersInOrg({
              org: context.repo.owner,
              team_slug: 'clients'
            });
            const isClient = teams.some(m => m.login === context.actor);
            if (!isClient) {
              core.setFailed('Only Clients can approve ideas');
              // Remove label
              await github.rest.issues.removeLabel({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: context.issue.number,
                name: 'state:approved'
              });
            }
      
      - name: Validate required fields
        uses: actions/github-script@v7
        with:
          script: |
            const body = context.payload.issue.body;
            const hasBusinessValue = body.includes('## Business Value') && 
                                     !body.includes('[Describe the business value]');
            const hasSuccessCriteria = body.includes('## Success Criteria') &&
                                       !body.includes('[Define measurable criteria]');
            if (!hasBusinessValue || !hasSuccessCriteria) {
              core.setFailed('Required fields not completed');
            }
      
      - name: Log audit entry
        run: |
          echo '{"event":"state_transition","from":"idea","to":"approved","actor":"${{ github.actor }}","timestamp":"${{ github.event.created_at }}"}' >> .audit/transitions.jsonl
      
      - name: Notify PO
        uses: actions/github-script@v7
        with:
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: '✅ Idea approved by @' + context.actor + '. @org/product please create Epic.'
            });
```

### 2.2 Approved → Ready

**Trigger:** Architect adds label `state:ready`

**Preconditions:**
- [ ] Issue has `state:approved` label
- [ ] Epic created and linked to Idea (Parent: #idea-id)
- [ ] Stories created and linked to Epic
- [ ] Architecture notes documented
- [ ] Definition of Ready (DoR) checklist complete
- [ ] Architect is member of @org/architecture team

**DoR Checklist:**
- [ ] Success criteria are specific and testable
- [ ] Non-goals explicitly stated
- [ ] Technical approach documented
- [ ] Dependencies identified
- [ ] Risks assessed
- [ ] Estimated effort provided

**Postconditions:**
- [ ] Label changed to `state:ready`
- [ ] Audit log entry created
- [ ] PM notified for resource assignment

### 2.3 Ready → In Progress

**Trigger:** IC self-assigns and adds label `state:in-progress`

**Preconditions:**
- [ ] Issue has `state:ready` label
- [ ] Issue has assignee
- [ ] PM has confirmed resource availability

**Postconditions:**
- [ ] Label changed to `state:in-progress`
- [ ] Branch created following naming convention
- [ ] Audit log entry created

**Branch Naming Convention:**
```
feature/<issue-id>-<slug>
fix/<issue-id>-<slug>
chore/<issue-id>-<slug>
docs/<issue-id>-<slug>

Example: feature/123-user-authentication
```

### 2.4 In Progress → In Review

**Trigger:** PR opened with linked issue

**Preconditions:**
- [ ] Issue has `state:in-progress` label
- [ ] PR title or body contains `Closes #<issue-id>`
- [ ] Branch follows naming convention
- [ ] PR template completed

**Postconditions:**
- [ ] Issue label changed to `state:in-review`
- [ ] PR linked to issue
- [ ] Reviewers auto-assigned based on CODEOWNERS
- [ ] Security team notified if security files changed
- [ ] Audit log entry created

**Automation (GitHub Action):**
```yaml
name: PR Opened - Link Validation
on:
  pull_request:
    types: [opened, edited]

jobs:
  validate-linking:
    runs-on: ubuntu-latest
    steps:
      - name: Check issue link exists
        uses: actions/github-script@v7
        with:
          script: |
            const prBody = context.payload.pull_request.body || '';
            const prTitle = context.payload.pull_request.title || '';
            const combined = prBody + ' ' + prTitle;
            
            const closesMatch = combined.match(/[Cc]loses #(\d+)/);
            if (!closesMatch) {
              core.setFailed('PR must include "Closes #<issue-id>" linking to a story');
              return;
            }
            
            const issueNumber = closesMatch[1];
            const { data: issue } = await github.rest.issues.get({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: issueNumber
            });
            
            // Check issue is in correct state
            const hasInProgress = issue.labels.some(l => l.name === 'state:in-progress');
            if (!hasInProgress) {
              core.setFailed(`Issue #${issueNumber} must be in 'In Progress' state`);
              return;
            }
            
            // Transition issue to in-review
            await github.rest.issues.removeLabel({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: issueNumber,
              name: 'state:in-progress'
            });
            await github.rest.issues.addLabels({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: issueNumber,
              labels: ['state:in-review']
            });
      
      - name: Check branch naming
        run: |
          BRANCH="${{ github.head_ref }}"
          if [[ ! "$BRANCH" =~ ^(feature|fix|chore|docs)/[0-9]+-[a-z0-9-]+$ ]]; then
            echo "::error::Branch name must follow pattern: type/<issue-id>-slug"
            exit 1
          fi
      
      - name: Check for security files
        uses: actions/github-script@v7
        with:
          script: |
            const { data: files } = await github.rest.pulls.listFiles({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: context.payload.pull_request.number
            });
            
            const securityPatterns = [
              /^src\/auth\//,
              /^src\/crypto\//,
              /^src\/security\//,
              /\.pem$/,
              /\.key$/
            ];
            
            const securityFilesChanged = files.some(f => 
              securityPatterns.some(p => p.test(f.filename))
            );
            
            if (securityFilesChanged) {
              await github.rest.issues.addLabels({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: context.payload.pull_request.number,
                labels: ['security:review-required']
              });
            }
```

### 2.5 In Review → Done

**Trigger:** CODEOWNER merges PR

**Preconditions:**
- [ ] PR has required approvals (IC Reviewer + CODEOWNER)
- [ ] If `security:review-required` label, PenTester approved
- [ ] All status checks passing
- [ ] No unresolved conversations
- [ ] Definition of Done (DoD) checklist complete

**DoD Checklist:**
- [ ] All acceptance criteria met with evidence
- [ ] Tests added and passing
- [ ] Documentation updated
- [ ] PR template fully completed
- [ ] No security vulnerabilities
- [ ] Code review feedback addressed

**Postconditions:**
- [ ] PR merged to main
- [ ] Linked issue(s) closed
- [ ] Issue label changed to `state:done`
- [ ] Audit log entry created

### 2.6 Done → Released

**Trigger:** Release workflow executed

**Preconditions:**
- [ ] All linked stories in `state:done`
- [ ] Release checklist complete
- [ ] DevOps approval
- [ ] CODEOWNER approval

**Postconditions:**
- [ ] GitHub Release created
- [ ] Tag created (semantic versioning)
- [ ] Release notes generated
- [ ] Linked stories labeled `state:released`
- [ ] Deployment triggered
- [ ] Audit log entry created

---

## 3. Definition of Ready (DoR)

### 3.1 Checklist Template

```markdown
## Definition of Ready Checklist

### Business Requirements
- [ ] Success criteria are specific and measurable
- [ ] Non-goals explicitly stated
- [ ] Acceptance criteria testable
- [ ] Business value articulated

### Technical Requirements
- [ ] Technical approach documented
- [ ] Architecture notes provided (for complex work)
- [ ] API contracts defined (if applicable)
- [ ] Database schema changes identified (if applicable)

### Dependencies
- [ ] External dependencies identified
- [ ] Internal dependencies identified
- [ ] Blockers documented

### Planning
- [ ] Effort estimated (story points or hours)
- [ ] Risks identified and mitigated
- [ ] Owner assigned

### Approval
- [ ] PO validated requirements
- [ ] Architect validated feasibility
```

### 3.2 DoR Enforcement

```yaml
name: Validate DoR
on:
  issues:
    types: [labeled]

jobs:
  validate-dor:
    if: github.event.label.name == 'state:ready'
    runs-on: ubuntu-latest
    steps:
      - name: Check DoR completion
        uses: actions/github-script@v7
        with:
          script: |
            const body = context.payload.issue.body || '';
            
            const dorItems = [
              'Success criteria are specific',
              'Non-goals explicitly stated',
              'Technical approach documented',
              'Owner assigned'
            ];
            
            const unchecked = dorItems.filter(item => {
              const regex = new RegExp(`- \\[ \\].*${item}`, 'i');
              return regex.test(body);
            });
            
            if (unchecked.length > 0) {
              core.setFailed(`DoR not complete. Missing: ${unchecked.join(', ')}`);
              await github.rest.issues.removeLabel({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: context.issue.number,
                name: 'state:ready'
              });
            }
```

---

## 4. Definition of Done (DoD)

### 4.1 Checklist Template

```markdown
## Definition of Done Checklist

### Implementation
- [ ] All acceptance criteria met
- [ ] Code follows project standards
- [ ] No hardcoded secrets or credentials
- [ ] Error handling implemented

### Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated (if applicable)
- [ ] All tests passing locally
- [ ] All tests passing in CI

### Documentation
- [ ] README updated (if applicable)
- [ ] API documentation updated (if applicable)
- [ ] Code comments for complex logic
- [ ] Changelog updated

### Review
- [ ] Self-review completed
- [ ] PR template fully completed
- [ ] No placeholder text or TODOs
- [ ] All review feedback addressed

### Security
- [ ] No security vulnerabilities introduced
- [ ] Dependency scan clean
- [ ] PenTester approved (if security files changed)

### Evidence Mapping
| Criterion | Evidence | Status |
|-----------|----------|--------|
| [Criterion 1] | [Test file:line or screenshot] | ✅/❌ |
| [Criterion 2] | [Test file:line or screenshot] | ✅/❌ |
```

### 4.2 DoD Enforcement

```yaml
name: Validate DoD
on:
  pull_request_review:
    types: [submitted]

jobs:
  validate-dod:
    if: github.event.review.state == 'approved'
    runs-on: ubuntu-latest
    steps:
      - name: Check DoD completion
        uses: actions/github-script@v7
        with:
          script: |
            const body = context.payload.pull_request.body || '';
            
            // Check for unchecked items
            const uncheckedCount = (body.match(/- \[ \]/g) || []).length;
            const checkedCount = (body.match(/- \[x\]/gi) || []).length;
            
            if (uncheckedCount > 0 && checkedCount === 0) {
              core.setFailed('DoD checklist not completed');
            }
            
            // Check for placeholder text
            const placeholders = ['TODO', '[Describe', '[Add', '[Update'];
            const hasPlaceholders = placeholders.some(p => body.includes(p));
            
            if (hasPlaceholders) {
              core.setFailed('PR contains placeholder text');
            }
            
            // Check evidence mapping
            if (!body.includes('| Criterion |') || !body.includes('| ✅')) {
              core.setFailed('Evidence mapping required');
            }
```

---

## 5. Approval Flow

### 5.1 PR Approval Matrix

| PR Type | Required Approvers | Minimum Count |
|---------|-------------------|---------------|
| Standard code change | IC Reviewer + CODEOWNER | 2 |
| Security-sensitive | IC Reviewer + PenTester + CODEOWNER | 3 |
| Infrastructure | IC Reviewer + DevOps + CODEOWNER | 3 |
| Documentation only | Tech Lead + CODEOWNER | 2 |
| Hotfix | CODEOWNER + DevOps | 2 (expedited) |

### 5.2 Approval Validation

```yaml
name: Validate Approvals
on:
  pull_request:
    types: [synchronize]
  pull_request_review:
    types: [submitted, dismissed]

jobs:
  validate-approvals:
    runs-on: ubuntu-latest
    steps:
      - name: Check required approvals
        uses: actions/github-script@v7
        with:
          script: |
            const { data: reviews } = await github.rest.pulls.listReviews({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: context.payload.pull_request.number
            });
            
            const approvals = reviews.filter(r => r.state === 'APPROVED');
            const approverLogins = [...new Set(approvals.map(a => a.user.login))];
            
            // Check CODEOWNER approved
            const { data: codeowners } = await github.rest.teams.listMembersInOrg({
              org: context.repo.owner,
              team_slug: 'codeowners'
            });
            const codeownerLogins = codeowners.map(c => c.login);
            const hasCodeownerApproval = approverLogins.some(a => codeownerLogins.includes(a));
            
            if (!hasCodeownerApproval) {
              core.info('Waiting for CODEOWNER approval');
            }
            
            // Check security approval if needed
            const { data: labels } = await github.rest.issues.listLabelsOnIssue({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.payload.pull_request.number
            });
            
            const needsSecurityReview = labels.some(l => l.name === 'security:review-required');
            if (needsSecurityReview) {
              const { data: security } = await github.rest.teams.listMembersInOrg({
                org: context.repo.owner,
                team_slug: 'security'
              });
              const securityLogins = security.map(s => s.login);
              const hasSecurityApproval = approverLogins.some(a => securityLogins.includes(a));
              
              if (!hasSecurityApproval) {
                core.setFailed('Security review required but not approved');
              }
            }
```

---

## 6. Blocked State Handling

### 6.1 Blocker Documentation

When marking an issue as blocked:

```markdown
## Blocker Details

**Blocked On:** [External system / Team / Decision]
**Blocker Description:** [What is preventing progress]
**Expected Resolution:** [Date or condition]
**Escalation Contact:** [@username]
**Impact:** [High / Medium / Low]

### Mitigation
- [ ] Alternative approach identified
- [ ] Stakeholders notified
- [ ] Timeline adjusted
```

### 6.2 Blocker Resolution

When blocker is resolved:
1. Remove `state:blocked` label
2. Add `state:in-progress` or previous state
3. Document resolution in comment
4. Update timeline if needed

---

## 7. Rollback Procedures

### 7.1 Code Rollback

| Scenario | Procedure | Approvers Required |
|----------|-----------|-------------------|
| Bug in production | Revert PR, create fix PR | CODEOWNER |
| Security vulnerability | Emergency revert, hotfix | CODEOWNER + PenTester |
| Performance issue | Revert PR, investigate | CODEOWNER + DevOps |

### 7.2 State Rollback

| From State | To State | Allowed | Approver | Documentation |
|------------|----------|---------|----------|---------------|
| Released | Done | Yes (revert) | CODEOWNER | Incident report |
| Done | In Review | Yes (rework) | CODEOWNER | Reason documented |
| In Review | In Progress | Yes (feedback) | Reviewer | Review comments |
| Ready | Approved | Yes (scope change) | Architect | Scope change log |
| Approved | Idea | No | - | Create new Idea |

---

**Previous Document:** [03-ROLES-AND-PERMISSIONS.md](03-ROLES-AND-PERMISSIONS.md)  
**Next Document:** [05-SPACE-STRUCTURE.md](05-SPACE-STRUCTURE.md)
