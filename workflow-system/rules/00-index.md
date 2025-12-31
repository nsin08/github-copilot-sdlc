# Workflow Rules Index

Rules are modular and extensible. Each file defines one rule category.

## Active Rules

| # | Rule | File | Description |
|---|------|------|-------------|
| 01 | State Machine | [01-state-machine.md](01-state-machine.md) | Workflow states and transitions |
| 02 | Definition of Ready | [02-definition-of-ready.md](02-definition-of-ready.md) | Entry criteria for "In Progress" |
| 03 | Definition of Done | [03-definition-of-done.md](03-definition-of-done.md) | Exit criteria for "Done" |
| 04 | Artifact Linking | [04-artifact-linking.md](04-artifact-linking.md) | Traceability requirements |
| 05 | PR Hygiene | [05-pr-hygiene.md](05-pr-hygiene.md) | Pull request standards |
| 06 | Versioning | [06-versioning.md](06-versioning.md) | SemVer and release process |
| 07 | Branching Strategy | [07-branching-strategy.md](07-branching-strategy.md) | Branch types, naming, and workflows |

## Adding New Rules

1. Create file: `NN-rule-name.md` (NN = next number)
2. Follow template structure (see any existing rule)
3. Update this index
4. Reference in `copilot-instructions.md` if AI needs awareness

## Rule File Structure

```markdown
# Rule: <Rule Name>

## Summary
One-line description.

## Rule Definition
Detailed rule with checklists.

## Examples
Good/bad examples.

## Enforcement
How this rule is enforced.

## Exceptions
When this rule can be bypassed.
```

---

**Version:** 1.0  
**Last Updated:** 2025-12-31
