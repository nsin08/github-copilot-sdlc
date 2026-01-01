# Copilot Instructions: GitHub Copilot SDLC Governance & Execution Control Plane

## 🚀 START HERE: Load Copilot Space for Agent Context

**All agents must load Copilot Space `space_test` for step-by-step guidance.**

The space contains everything agents need:
- 🎯 **Quick Start Guides** (by role: Implementer, Tech Lead, Reviewer, Release Manager)
- 📋 **Templates** (Epic, Story, PR, Review Checklist)
- 📖 **Rules Quick Reference** (State Machine, DoR, DoD, Artifact Linking)
- 🏃 **Runbooks** (Feature Development, Code Review, Deployment, Troubleshooting)
- 📚 **Reference** (Architecture Decisions, Process Evolution, FAQs)

**When you start a task:** Load `space_test` and follow the guide for your role.

| I want to... | Go to in Space |
|---|---|
| **Implement a feature** | Quick_Start → Im_an_Implementer.md |
| **Create a Story** | Quick_Start → Im_a_Tech_Lead.md |
| **Review code** | Quick_Start → Im_a_Reviewer.md |
| **Deploy** | Quick_Start → Im_Deploying.md |
| **Get unstuck** | Quick_Start → Im_Stuck.md |

---

## What This Codebase Does

This is a **governance and execution control plane for AI-assisted software delivery**. It integrates GitHub's native features (Issues, PRs, Actions, Releases) into an enforced state machine with 5 key roles (Sponsor/PO, Tech Lead, Implementer, Reviewer/QA, Release/DevOps).

**Critical difference:** This is NOT a workflow template. Rules and state transitions are **system-enforced**, not optional. Work cannot proceed unless predefined conditions are satisfied.

**How it works:** Every piece of work flows through a mandatory state machine:
```
Intake → Spec Ready → In Progress → In Review → Done → Released
```

You **cannot** skip states, merge without approval, or execute work from undefined states.

## Critical Architecture Patterns

### The Workflow State Machine (System-Enforced)
All work **must** follow this progression—skipping is blocked:
```
Intake → Spec Ready → In Progress → In Review → Done → Released
```

**State definitions and enforcement rules:**
- **Intake:** Sponsor creates Idea Issue with problem + success criteria (must be measurable)
  - *Enforcement:* Cannot transition without both fields filled
- **Spec Ready:** Tech Lead converts to Epic + Stories, adds architecture notes, validates feasibility
  - *Enforcement:* DoR checklist must be satisfied; Tech Lead approval required
- **In Progress:** Implementer builds, tests, opens PR with evidence mapping
  - *Enforcement:* Cannot start without "Spec Ready" label; story must have assigned owner
- **In Review:** Reviewer/QA validates against success criteria and DoD checklist
  - *Enforcement:* PR cannot merge without approval and passing CI
- **Done:** PR merged, CI green, docs updated
  - *Enforcement:* Merge blocked if any DoD criterion fails
- **Released:** Version tagged, release notes generated
  - *Enforcement:* Release cannot proceed if any "Done" stories have regressions

**Why enforcement matters:** Prevents unreviewed work, unsafe deployments, and ambiguous "ready" states. Violations are **blocked by automation**, not subject to judgment calls.

**Reference:** [workflow-system/rules/01-state-machine.md](workflow-system/rules/01-state-machine.md)

### Artifact Linking = Enforced Traceability
Traceability is **system-enforced**, not optional. Links must exist and be valid. Automation blocks creation of unlinked artifacts.

**Linking requirements:**
- **Epic** must exist; every Epic must have a problem statement
- **Stories** must have `Parent: #<epic-id>` in body; automation validates this field
- **PRs** must have `Closes #<story-id>` in title or body (exactly one); merge is blocked without valid link
- **Branches** must follow `feature/<issue-id>-slug` naming; non-compliant branch names are rejected by pre-commit hooks

**Why enforcement matters:** Complete traceability from business idea to production code. Without it, you cannot:
- Audit why code exists
- Trace regressions to requirements
- Calculate cycle time per feature
- Prove compliance to governance audits

**Example chain:** Epic #10 (Auth system) → Story #15 (Login endpoint) → Branch `feature/15-login` → PR links `Closes #15` → Merge triggers version bump and release notes

**Reference:** [workflow-system/rules/04-artifact-linking.md](workflow-system/rules/04-artifact-linking.md)

### Definition of Ready (DoR) Gates Entry
Before any work starts ("In Progress"), the issue MUST satisfy:
- [ ] Success criteria are specific and testable (not vague like "should work")
- [ ] Non-goals explicitly stated (scope boundaries)
- [ ] Test plan documented
- [ ] Edge cases identified
- [ ] Owner assigned
- [ ] Tech Lead validated feasibility

**Common DoR failure:** Acceptance criteria like "Add user authentication" (too vague → break into measurable steps)

**Reference:** [workflow-system/rules/02-definition-of-ready.md](workflow-system/rules/02-definition-of-ready.md)

### Definition of Done (DoD) Gates Release
Before marking "Done", the PR MUST satisfy:
- [ ] All acceptance criteria met **with evidence** (test names, file paths)
- [ ] Tests added and passing (local + CI)
- [ ] Documentation updated (README examples, API docs, comments)
- [ ] PR template 100% filled (no placeholders/TODOs)
- [ ] Reviewer approved
- [ ] CI green
- [ ] No regressions

**Evidence format example:**
```markdown
- ✅ Criterion: Returns 200 status
  - Evidence: test_health_returns_200 (tests/health_test.py:45-50) PASSING
- ✅ Criterion: JSON includes timestamp
  - Evidence: test_health_timestamp_present (tests/health_test.py:52-58) PASSING
```

**Reference:** [workflow-system/rules/03-definition-of-done.md](workflow-system/rules/03-definition-of-done.md)

## When You're an Implementer

**Full step-by-step guide:** Space → Quick_Start → Im_an_Implementer.md

**Quick summary:**
1. Pick a Story labeled "Spec Ready"
2. Create branch: `feature/<issue-id>-slug`
3. Read success criteria—implement ONLY what's listed
4. Write tests for each criterion
5. Update documentation
6. Open PR with success criteria mapped to test evidence
7. Get review and merge

**Golden rule:** Don't implement features NOT in success criteria.

## When You're a Tech Lead

**Full step-by-step guide:** Space → Quick_Start → Im_a_Tech_Lead.md

**Quick summary:**
1. Challenge unclear requirements (collaborate with PO)
2. Validate feasibility (check with implementers)
3. Create Stories from Epic with architecture notes
4. Mark "Spec Ready" only when Definition of Ready satisfied

**Key rule:** Don't mark ready until you've validated feasibility and collaboratively clarified requirements.

## Enforcement Mechanisms

### How Rules Are Enforced
This system is not advisory—violations are **blocked by automation**:

| Rule | Enforcement Mechanism | Consequence |
|------|----------------------|-------------|
| State transitions | GitHub labels, branch protection, Actions | Cannot proceed to next state without approvals/checks |
| Artifact linking | Pre-commit hooks, merge checks | PRs and issues cannot be created/merged without valid links |
| DoR validation | GitHub Actions workflow | Merge blocked if issue lacks DoR checklist completion |
| DoD validation | Status checks, PR approval gates | Merge blocked if tests fail, docs not updated, or criteria not met |
| Branch naming | Pre-commit hooks | Commits rejected if branch name violates pattern |
| PR template | Required status check | Merge blocked if template sections are incomplete or have placeholders |

### What Gets Blocked
- ❌ Starting work on issues without "Spec Ready" label
- ❌ Merging PRs without approval from designated reviewer
- ❌ Merging PRs with failing tests or CI errors
- ❌ Creating PRs unlinked to exactly one issue
- ❌ Pushing to `main` directly (only via PR)
- ❌ Merging PRs that don't map success criteria to evidence
- ❌ Releasing code with open regressions or test failures

### What Requires Human Approval
- Transitions from Intake → Spec Ready (Tech Lead validates requirements)
- Transitions from In Progress → In Review (reviewer enforces DoD)
- Transitions from Done → Released (Release/DevOps triggers deployment)
- Hotfixes or scope changes (documented exception with rationale)

**Reference:** [workflow-system/guides/04-enforcement-automation.md](workflow-system/guides/04-enforcement-automation.md)

## When You're a Reviewer/QA

**Full step-by-step guide:** Space → Quick_Start → Im_a_Reviewer.md

**Quick summary:**
1. Validate each success criterion is met with concrete evidence
2. Run manual tests if frontend/API changes
3. Check for regressions (existing functionality still works)
4. Verify PR template is fully filled
5. Confirm docs updated (README, API docs)
6. Ensure tests are passing locally and in CI
7. Approve or request changes

**Key rule:** Block PRs that don't meet Definition of Done. Don't approve incomplete work—it delays everyone.

**Provide specific, actionable feedback:**
- ❌ Bad: "This doesn't look right"
- ✅ Good: "Criterion 2 requires timestamp format ISO8601, but test uses 'YYYY-MM-DD'. Update to match Story #15:L8"

**Reference:** [workflow-system/roles/04-reviewer-qa.md](workflow-system/roles/04-reviewer-qa.md)

## Project Files & Their Purposes

| Path | Purpose |
|------|---------|
| `workflow-system/rules/` | Mandatory rules (state machine, DoR, DoD, linking, PR hygiene, branching) |
| `workflow-system/roles/` | Role-specific guides and prompts for each lifecycle role |
| `workflow-system/guides/` | Integration & customization guides for adopting into projects |
| `workflow-system/examples/` | Real-world examples of workflow in action |
| `ISSUE_TEMPLATE/` | Templates for Epic, Story, Task, PR, Review checklist |

## Branch Naming & Git Flow

**Pattern:** `<type>/<issue-id>-<slug>`

| Type | Purpose | Example |
|------|---------|---------|
| `feature/` | New functionality | `feature/42-login-endpoint` |
| `fix/` | Bug fixes | `fix/99-null-pointer` |
| `chore/` | Maintenance | `chore/15-update-deps` |
| `docs/` | Documentation | `docs/8-api-readme` |

**Golden rules:**
- Never push to `main` directly—always PR
- Main branch must always be deployable (CI green, tests passing)
- If using `develop` branch, merge feature branches there first, then to main
- Release branches (`release/*`) stabilize before merging to main

**Reference:** [workflow-system/rules/07-branching-strategy.md](workflow-system/rules/07-branching-strategy.md)

## Common Pitfalls & Anti-Patterns

❌ **Skipping Spec Ready:** Don't start implementation without Tech Lead validation  
❌ **Vague success criteria:** "Add authentication" is NOT testable; break into measurable steps  
❌ **PR without evidence:** Map every criterion to test/file—don't use placeholders  
❌ **Drive-by refactors:** Keep PRs focused on one story  
❌ **Merged PR with failing tests:** CI must be green before merge  
❌ **PRs without issue links:** Every PR must close exactly one issue via `Closes #<id>`  
❌ **Breaking existing tests:** Your changes must not fail existing tests  

---

## How to Use Role Prompts

When invoking Copilot for lifecycle tasks, prefix with the role context:

```
[Role: Implementer]
Story #15 is now in "In Progress". 
Read success criteria and implement following branch/PR patterns.
Create branch feature/15-health-endpoint.
```

See [workflow-system/roles/00-index.md](workflow-system/roles/00-index.md) for detailed prompts for each role.

## Key Integration Points

**This repository is a Git submodule** meant to be added to projects as `.github/`:
```bash
git submodule add https://github.com/nsin08/github-copilot-sdlc.git .github
```

GitHub automatically recognizes:
- Issue templates in `ISSUE_TEMPLATE/`
- GitHub Actions workflows in `workflows/`
- Copilot instructions from this file

**Customization:** Each adopting project customizes the `Implementation Patterns` section of this file and the role guides with project-specific tech stack, commands, and conventions.

## Quick Reference: When to Use Which Template

| Situation | Use | Location |
|-----------|-----|----------|
| New feature request | Epic template | [01-epic.md](ISSUE_TEMPLATE/01-epic.md) |
| Implementation task | Story/Task template | [02-story-task.md](ISSUE_TEMPLATE/02-story-task.md) |
| Code submission | PR template | [00-pull-request-template.md](ISSUE_TEMPLATE/00-pull-request-template.md) |
| Before PR approval | QA Review Checklist | [03-review-checklist.md](ISSUE_TEMPLATE/03-review-checklist.md) |

---

**Last updated:** January 2026  
**Framework version:** 1.0.0  
**Status:** Production-ready  
**Positioning:** Governance and execution control plane for AI-assisted software delivery

### Key Principles
- **Enforcement over guidance:** Rules are system-enforced, not advisory
- **State-driven execution:** All work flows through defined, controlled states
- **Touchless on happy path:** Once conditions are met, work executes without manual intervention
- **Measurable outcomes:** Track delivery quality, cycle time, and AI effectiveness
- **Risk reduction:** Prevent shortcuts and unsafe deployments through automation
