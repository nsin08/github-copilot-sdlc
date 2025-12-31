# Copilot Instructions: Role-Based Workflow System

**⚠️ IMPORTANT:** This `.github` directory must be copied to your repository root to function. GitHub only recognizes templates, workflows, and copilot-instructions when they are at `.github/` in the repo root, not in subdirectories.

## Project Overview
This is a **role-based workflow system** implementing a complete GitHub lifecycle (Intake → Spec → Implementation → Review → Release) with currently 5 defined roles. Customize the Implementation Patterns section below for your project's tech stack and patterns.

## Core Architecture

### Workflow State Machine
All work flows through: `Intake → Spec Ready → In Progress → In Review → Done → Released`  
**Critical:** Never skip states. Each transition has entry/exit criteria defined in [workflow-system/rules/01-state-machine.md](workflow-system/rules/01-state-machine.md).

### Roles & Handoffs (5 Roles Currently)
1. **Sponsor/PO** (Intake) — Creates Idea Issues with problem statement and success criteria
2. **Tech Lead/Architect** (Spec Ready) — Converts to Epic + Stories with API contracts and test plans
3. **Implementer** (In Progress → PR) — Builds features, tests, docs; opens PRs
4. **Reviewer/QA** (In Review) — Validates PRs against success criteria
5. **Release/DevOps** (Release) — Tags versions, generates release notes

### Artifact Linking (Non-negotiable)
- Every Story/Task **must** link to parent Epic via `Parent: #<id>` in body
- Every PR **must** link via `Closes #<id>` to exactly one Issue
- Maintain full traceability: Idea → Epic → Story → Branch → PR → Release
- See [workflow-system/rules/04-artifact-linking.md](workflow-system/rules/04-artifact-linking.md) for enforcement rules

## Implementation Patterns

**Customize this section for your project's tech stack and patterns.**

### Example: Service Code (src/service/)
- **Your framework here**: Describe your app structure
- **Data persistence**: Database or in-memory
- **Endpoint pattern**: Your API conventions
- **Request tracking**: Logging, metrics, tracing

### Example: Testing (tests/)
- **Test framework**: pytest, jest, junit, etc.
- **Test naming**: Your convention (e.g., `test_<feature>_<behavior>`)
- **Coverage target**: >90% per story acceptance criteria
- Run: `<your-test-command>`

### Branch Naming
- Features: `feature/<issue-id>-<slug>` (e.g., `feature/2-health-endpoint`)
- Chores: `chore/<issue-id>-<slug>`
- **Never** push to default branch directly—always PR from feature branch

## Critical Workflows

### When Acting as Implementer
1. Read Story success criteria line-by-line (in issue body under "Success Criteria")
2. Create branch following naming convention above
3. Implement **only** what's in success criteria (no extra features)
4. Add tests that directly prove each criterion
5. Update README with endpoint examples
6. Open PR using [00-pull-request-template.md](ISSUE_TEMPLATE/00-pull-request-template.md)—fill **every section** (no placeholders)
7. Map each success criterion to evidence (file paths, test names, CI output)

### When Opening PRs
- **Template compliance is mandatory**: See [00-pull-request-template.md](ISSUE_TEMPLATE/00-pull-request-template.md)
- Fill "Mapping to Success Criteria" section with concrete evidence (e.g., "✅ Criterion 1: Endpoint returns 200 → Evidence: `test_health_endpoint_returns_200` in tests/test_health.py:13-16")
- Include test output in "Test Evidence" section
- Keep PRs small: 1 story per PR, 1-3 commits, no drive-by refactors

### When Acting as Tech Lead
⚠️ **Tech Lead must challenge unclear requirements and collaborate - not just accept everything passively.**

**Before creating Epic/Stories:**
- Question the PO if requirements are unclear (comment on Idea Issue)
- Challenge assumptions: Is this needed? Is scope too large? Technical risks?
- Validate feasibility with implementers: effort estimate, blockers, dependencies
- Push back if success criteria are not measurable or edge cases missing
- Document Q&A with PO in Epic (what was clarified, what assumptions made)

**When creating Epic/Stories:**
- Split Epics into Stories that are independently testable
- Add "Architecture Note" comments to Epic with:
  - API contracts (endpoint, request/response shape, status codes)
  - Test strategy (unit/integration scope)
  - Dependencies between stories
  - Technical risks/unknowns
  - Assumptions validated with PO
- Example from Story #2: "GET /health → 200 {status:ok, timestamp:ISO8601} → Unit tests: status code, JSON shape, timestamp format"

**Do NOT mark "Spec Ready" unless:**
- [ ] Success criteria are measurable
- [ ] Edge cases identified
- [ ] Non-goals stated
- [ ] Technical feasibility validated
- [ ] PO confirms understanding
- [ ] DoR satisfied

### When Acting as Reviewer/QA
⚠️ **QA is the quality gate between implementation and release.**

- Follow comprehensive QA protocols in [workflow-system/roles/04-reviewer-qa.md](workflow-system/roles/04-reviewer-qa.md)
- Apply QA Review Checklist (pre-review, criteria validation, testing, code quality, docs, integration)
- Perform manual testing for frontend/API changes (browser testing, responsive layouts, error states)
- Provide concrete, actionable feedback with specific file/line references
- Block PRs that don't meet Definition of Done
- Escalate ambiguous requirements or scope creep

See [workflow-system/roles/04-reviewer-qa.md](workflow-system/roles/04-reviewer-qa.md) for:
- Step-by-step review process
- Review comment format
- Anti-patterns to avoid
- Escalation procedures

## Role Prompts
When invoking Copilot for lifecycle tasks, **always prefix** with role-specific prompt from [workflow-system/roles/00-index.md](workflow-system/roles/00-index.md). Example:
```
[Role: Implementer]
Pick Story #3 (metrics endpoint) and implement following the PR template.
```

See [workflow-system/roles/00-index.md](workflow-system/roles/00-index.md) for complete prompt templates for each role.

## Definition of Done
Before marking any PR "ready for review":
- [ ] All acceptance criteria from Story met
- [ ] Tests added/updated and passing locally
- [ ] README updated with examples
- [ ] PR template **fully filled** (no "TODO" or placeholder text)
- [ ] Branch follows naming convention
- [ ] Linked to exactly one Issue via `Closes #<id>`

See [workflow-system/rules/03-definition-of-done.md](workflow-system/rules/03-definition-of-done.md) for complete DoD checklist.

## Common Commands

**Customize for your project:**

```bash
# Run your service locally
<your-run-command>  # e.g., npm start, python -m src.app, etc.

# Run tests with coverage
<your-test-command>  # e.g., pytest tests/, npm test, mvn test

# Create issue via GH CLI
gh issue create --title "..." --label "story" --body-file .github/ISSUE_TEMPLATE/02-story-task.md

# Create PR from feature branch
gh pr create --title "..." --body "Closes #<id>, Epic #<epic-id>" --base main
```

## Anti-Patterns
- ❌ Creating PRs without filling template sections
- ❌ Adding features not in Story success criteria
- ❌ Breaking Issues/PRs into multiple unlinked artifacts
- ❌ Skipping tests ("will add later")
- ❌ Moving forward when DoR/DoD not satisfied
- ❌ Using generic commit messages ("fix", "update")—reference issue IDs

## Key Files to Reference
- [workflow-system/rules/00-index.md](workflow-system/rules/00-index.md) — All workflow rules
- [workflow-system/roles/00-index.md](workflow-system/roles/00-index.md) — Role definitions and prompts
- [workflow-system/guides/02-quick-start.md](workflow-system/guides/02-quick-start.md) — 30-min end-to-end test
- [workflow-system/examples/](workflow-system/examples/) — Real artifact examples
- [.github/ISSUE_TEMPLATE/](./ISSUE_TEMPLATE/) — Templates for Epic, Story, Review
