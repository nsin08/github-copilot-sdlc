# Rule: Definition of Done (DoD)

## Summary

Criteria that must be satisfied before work is considered complete ("Done").

## Rule Definition

### Checklist

Before marking an item "Done", verify:

- [ ] **Acceptance Criteria Met:** All success criteria from story satisfied
- [ ] **Tests Added/Updated:** New functionality has corresponding tests
- [ ] **Tests Passing:** All tests pass (local and CI)
- [ ] **Documentation Updated:** README, API docs, comments as needed
- [ ] **PR Template Complete:** All sections filled with evidence
- [ ] **Reviewer Approved:** At least one approval from qualified reviewer
- [ ] **CI Green:** All automated checks pass
- [ ] **No Regressions:** Existing functionality still works

### By Artifact Type

#### Story DoD
- [ ] All acceptance criteria met with evidence
- [ ] Tests added (coverage target met)
- [ ] Documentation updated
- [ ] PR approved and merged
- [ ] No TODOs without issue references

#### Epic DoD
- [ ] All child stories completed
- [ ] Integration testing passed
- [ ] Documentation complete
- [ ] Ready for release

#### PR DoD
- [ ] Linked to exactly one issue
- [ ] Template fully filled
- [ ] Evidence for each criterion
- [ ] Reviewer approved
- [ ] CI green
- [ ] Conflicts resolved

## Examples

### ✅ Done

```markdown
## Evidence
- Criterion 1: ✅ Endpoint returns 200 → test_health_returns_200 (PASSING)
- Criterion 2: ✅ JSON shape correct → test_health_response_shape (PASSING)
- Criterion 3: ✅ Timestamp format → test_health_timestamp_iso8601 (PASSING)

## Tests
- 5 new tests added
- Coverage: 94% (+3%)
- All 123 tests passing

## Documentation
- README updated with endpoint example
- API section added
```

### ❌ Not Done

```markdown
## Evidence
- Criterion 1: Implemented  ❌ (no test evidence)
- Criterion 2: Should work  ❌ (vague)

## Tests
- Will add later  ❌ (not done)
- 2 tests failing  ❌ (CI not green)

## Documentation
- TODO  ❌ (incomplete)
```

## Enforcement

- **PR Template:** Requires evidence mapping
- **CI Checks:** Block merge if tests fail
- **Review:** Reviewer validates against DoD
- **Automation:** GitHub Actions can check template completion

## Exceptions

- **WIP PR:** Draft PRs may be incomplete (mark as draft)
- **Partial Merge:** Large features may merge incrementally behind feature flag

---

**Rule ID:** DOD-001  
**Category:** Quality Gate  
**Severity:** Mandatory
