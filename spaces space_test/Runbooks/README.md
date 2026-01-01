# Runbooks

Task-focused, step-by-step guides for common procedures.

## Available Runbooks

| Runbook | Purpose | Time |
|---------|---------|------|
| [Feature_Development.md](Feature_Development.md) | Build & ship a feature | 1-5 days |
| [Code_Review.md](Code_Review.md) | Review & approve a PR | 30 min |
| [Deployment_Runbook.md](Deployment_Runbook.md) | Release to production | 30 min |
| [Troubleshooting.md](Troubleshooting.md) | Debug & fix issues | 15-60 min |

---

## How to Use Runbooks

**Runbooks are numbered steps.** Follow them in order:

```
Step 1: Do X
Step 2: Check Y
Step 3: Do Z
```

**Each step includes:**
- What you're doing (the goal)
- How to do it (commands/actions)
- How to verify success (checklist)

**Example:**
```markdown
### ✅ Step 1: Create Branch
1. Run: git checkout -b feature/42-login
2. Verify: git branch (shows new branch)
3. Push: git push -u origin feature/42-login
```

---

## Quick Navigation

### I'm building a feature
→ [Feature_Development.md](Feature_Development.md)

Steps:
1. Read the Story
2. Create branch
3. Write tests
4. Implement
5. Update docs
6. Open PR
7. Get review

### I'm reviewing code
→ [Code_Review.md](Code_Review.md)

Steps:
1. Pre-review checklist
2. Read story & criteria
3. Check evidence
4. Review code
5. Verify docs
6. Check DoD
7. Approve/request changes

### I'm deploying
→ [Deployment_Runbook.md](Deployment_Runbook.md)

Steps:
1. Verify readiness
2. Create tag
3. Generate release notes
4. Deploy
5. Monitor

### I'm stuck
→ [Troubleshooting.md](Troubleshooting.md)

Common issues & solutions:
- Tests not running
- Branch/Git issues
- PR won't merge
- Success criteria unclear
- Reviewer feedback

---

## Runbook Standards

All runbooks:
- ✅ Have numbered steps
- ✅ Are task-focused (not reference docs)
- ✅ Include checklists
- ✅ Show expected outcomes
- ✅ Link to related docs

---

## Customization

These runbooks are framework-independent. Each project can customize:
- **Commands:** Based on your tech stack
- **Tools:** Your CI/CD, deployment, monitoring
- **Timelines:** Adjust estimates for your team
- **Escalation:** Add project-specific escalation paths

**Core structure must remain** (steps, checklists, verification).

---

## Related Docs

- [../Quick_Start/](../Quick_Start/) — Role-based entry points (links to runbooks)
- [../Rules/](../Rules/) — Governance (why we do these steps)
- [../Templates/](../Templates/) — Artifact templates

---

**Pick a runbook above and follow the numbered steps!** 👆
