# Rule: Definition of Ready (DoR)

## Summary

Criteria that must be satisfied before work can begin ("In Progress").

## Rule Definition

### Checklist

Before marking an item "In Progress", verify:

- [ ] **Success Criteria:** Clear, measurable, testable outcomes defined
- [ ] **Non-Goals:** Explicitly stated what is NOT in scope
- [ ] **Test Plan:** At least minimal testing strategy documented
- [ ] **Edge Cases:** Known edge cases listed (if relevant)
- [ ] **Owner Assigned:** Single responsible person identified
- [ ] **Dependencies:** External dependencies identified and available
- [ ] **Acceptance:** PO/stakeholder confirms understanding

### By Item Type

#### Epic DoR
- [ ] Problem statement clear
- [ ] Business value articulated
- [ ] Success criteria measurable
- [ ] Breakdown into stories exists (or planned)

#### Story DoR
- [ ] Linked to parent Epic
- [ ] Success criteria specific and testable
- [ ] Test plan defined
- [ ] Effort estimated (optional but recommended)
- [ ] No blocking dependencies

#### Task DoR
- [ ] Linked to parent Story
- [ ] Clear deliverable defined
- [ ] Owner assigned

## Examples

### ✅ Ready

```markdown
## Success Criteria
1. GET /health returns 200 with {"status": "ok"}
2. Response includes ISO8601 timestamp
3. Unit tests cover happy path and error cases

## Non-Goals
- No authentication required
- No database dependency

## Test Plan
- Unit: test_health_returns_200, test_health_response_shape
- Integration: None needed
```

### ❌ Not Ready

```markdown
## Success Criteria
- Health endpoint should work  ❌ (vague, not testable)

## Non-Goals
(none specified)  ❌ (scope unclear)

## Test Plan
Will add tests later  ❌ (not defined)
```

## Enforcement

- **Template:** Issue templates include DoR checklist
- **Review:** Tech Lead validates DoR before marking "Spec Ready"
- **Block:** Cannot assign/start work until DoR satisfied

## Exceptions

- **Spike/Investigation:** May have looser DoR (goal is learning, not delivery)
- **Hotfix:** Can start with minimal DoR, document gaps afterward

---

**Rule ID:** DOR-001  
**Category:** Quality Gate  
**Severity:** Mandatory
