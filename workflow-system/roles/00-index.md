# Roles Index

Role definitions are modular and extensible. Each file defines one role.

## Active Roles

| # | Role | File | Workflow Stage |
|---|------|------|----------------|
| 00 | Shared Context | [00-shared-context.md](00-shared-context.md) | All (prefix for all roles) |
| 01 | Sponsor/PO | [01-sponsor-po.md](01-sponsor-po.md) | Intake |
| 02 | Tech Lead | [02-tech-lead.md](02-tech-lead.md) | Spec Ready |
| 03 | Implementer | [03-implementer.md](03-implementer.md) | In Progress |
| 04 | Reviewer/QA | [04-reviewer-qa.md](04-reviewer-qa.md) | In Review |
| 05 | Release/DevOps | [05-release-devops.md](05-release-devops.md) | Released |

## Role Workflow

```
01-Sponsor/PO ──→ 02-Tech Lead ──→ 03-Implementer ──→ 04-Reviewer/QA ──→ 05-Release
   (Intake)       (Spec Ready)     (In Progress)      (In Review)       (Released)
```

## Adding New Roles

1. Create file: `NN-role-name.md` (NN = next number)
2. Follow role template structure
3. Update this index
4. Add collaboration touchpoints to related roles

## Role File Structure

```markdown
# Role: <Role Name>

## Summary
One-line role description.

## Responsibilities
- Key responsibility 1
- Key responsibility 2

## Prompt
The exact prompt to use when acting as this role.

## Deliverables
What this role produces.

## Collaboration
How this role interacts with other roles.

## Anti-Patterns
What NOT to do in this role.
```

## Using Role Prompts

Prefix any Copilot/AI interaction with:

```
[Role: <Role Name>]

<Your request here>
```

Example:
```
[Role: Implementer]

Pick Story #15 and implement the health endpoint.
```

---

**Version:** 1.0  
**Last Updated:** 2025-12-31
