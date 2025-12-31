# Rule: State Machine

## Summary

All work items flow through defined states. No state can be skipped.

## Rule Definition

### States

```
Intake → Spec Ready → In Progress → In Review → Done → Released
```

### State Descriptions

| State | Owner | Entry Criteria | Exit Criteria |
|-------|-------|----------------|---------------|
| **Intake** | Sponsor/PO | Idea exists | Problem & success criteria defined |
| **Spec Ready** | Tech Lead | DoR satisfied | Stories created with arch notes |
| **In Progress** | Implementer | Assigned owner | PR opened with tests |
| **In Review** | Reviewer/QA | PR ready | Approved or changes requested |
| **Done** | - | PR merged | CI green, docs updated |
| **Released** | Release/DevOps | All stories merged | Tagged, notes generated |

### Transition Rules

1. **Forward Only:** Items move forward through states (no skipping)
2. **Gate Required:** Each transition requires entry criteria satisfaction
3. **Backward Allowed:** Items can move backward (e.g., Review → In Progress for changes)
4. **Blocking:** Missing information = STOP and request details

## Examples

### ✅ Valid Flow
```
Issue #5: Intake → Spec Ready → In Progress → In Review → Done → Released
```

### ❌ Invalid Flow
```
Issue #5: Intake → In Progress  ❌ (skipped Spec Ready)
Issue #5: Spec Ready → Done     ❌ (skipped In Progress, In Review)
```

## Enforcement

- **Labels:** Use GitHub labels matching state names
- **Project Board:** Columns match states
- **Automation:** GitHub Actions can enforce transitions (optional)

## Exceptions

- **Hotfix:** Can fast-track from Intake → In Progress (must document why)
- **Chore:** Documentation-only changes may skip Spec Ready

---

**Rule ID:** STATE-001  
**Category:** Process  
**Severity:** Mandatory
