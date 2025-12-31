# Workflow System

Modular, extensible role-based GitHub lifecycle workflow (5 roles currently implemented).

## Table of Contents

- [Structure](#structure)
- [Quick Navigation](#quick-navigation)
- [Naming Convention](#naming-convention)
- [Extensibility](#extensibility)
- [Version](#version)

---

## Structure

```
workflow-system/
├── rules/          # Modular workflow rules (extensible)
│   ├── 00-index.md
│   ├── 01-state-machine.md
│   ├── 02-definition-of-ready.md
│   ├── 03-definition-of-done.md
│   ├── 04-artifact-linking.md
│   ├── 05-pr-hygiene.md
│   └── 06-versioning.md
│
├── roles/          # Role definitions with prompts (extensible)
│   ├── 00-index.md
│   ├── 00-shared-context.md
│   ├── 01-sponsor-po.md
│   ├── 02-tech-lead.md
│   ├── 03-implementer.md
│   ├── 04-reviewer-qa.md
│   └── 05-release-devops.md
│
├── guides/         # User documentation
│   ├── 00-index.md
│   ├── 01-integration.md
│   ├── 02-quick-start.md
│   └── 03-customization.md
│
└── examples/       # Real artifact examples
    ├── 00-index.md
    ├── 01-epic-breakdown.md
    ├── 02-pr-with-evidence.md
    ├── 03-qa-review.md
    └── 04-release-notes.md
```

## Quick Navigation

| Need | Go To |
|------|-------|
| Understand the workflow | [rules/01-state-machine.md](rules/01-state-machine.md) |
| Check Definition of Ready | [rules/02-definition-of-ready.md](rules/02-definition-of-ready.md) |
| Check Definition of Done | [rules/03-definition-of-done.md](rules/03-definition-of-done.md) |
| Act as a specific role | [roles/00-index.md](roles/00-index.md) |
| See example PR | [examples/02-pr-with-evidence.md](examples/02-pr-with-evidence.md) |
| Get started quickly | [guides/02-quick-start.md](guides/02-quick-start.md) |

## Naming Convention

All files follow: `NN-descriptive-name.md`

- **NN** = Two-digit number for ordering (00, 01, 02...)
- **descriptive-name** = Kebab-case description
- **00-index.md** = Always the directory index
- **00-shared-*.md** = Shared/base content

## Extensibility

### Adding a New Rule

1. Create file: `rules/07-your-rule.md`
2. Follow template in [rules/00-index.md](rules/00-index.md)
3. Update rules/00-index.md table

### Adding a New Role

1. Create file: `roles/06-your-role.md`
2. Follow template in [roles/00-index.md](roles/00-index.md)
3. Update roles/00-index.md table

### Adding a New Guide

1. Create file: `guides/your-guide.md`
2. Update guides/00-index.md table

### Adding a New Example

1. Create file: `examples/05-your-example.md`
2. Update examples/00-index.md table

## Version

**System Version:** 1.0

---

**Parent:** [../.github/README.md](../README.md)
