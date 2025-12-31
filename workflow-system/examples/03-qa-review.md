# Example: QA Review

Shows a properly structured QA review approval comment.

## Context

- **PR:** #10 - Add health endpoint
- **Story:** #2
- **Epic:** #1

---

## QA Review Comment (Approval)

```markdown
## QA Review: Approved ✅

### Pre-Review Validation
- [x] PR linked to Issue #2 ✓
- [x] Issue #2 linked to Epic #1 ✓
- [x] PR template complete (no placeholders) ✓
- [x] CI checks passing (4/4 green) ✓
- [x] Branch naming correct (feature/2-health-endpoint) ✓

### Success Criteria Validation

| # | Criterion | Evidence | Validated |
|---|-----------|----------|-----------|
| 1 | GET /health returns 200 | test_health_returns_200 (line 13) - PASSING | ✅ |
| 2 | Response: {"status": "ok", "timestamp": "..."} | test_health_response_has_status_ok, test_health_response_has_timestamp | ✅ |
| 3 | Timestamp ISO8601 with Z | test_health_timestamp_is_iso8601 - checks .endswith('Z') | ✅ |
| 4 | Unit tests for all criteria | 4 tests added, all passing | ✅ |

**All 4 criteria validated with test evidence.**

### Test Coverage Review
- [x] All success criteria have tests (4/4)
- [x] Happy path: test_health_returns_200 ✓
- [x] Response validation: 2 tests for shape ✓
- [x] Format validation: test_health_timestamp_is_iso8601 ✓
- [x] Tests actually assert behavior (not just coverage) ✓

### Manual Testing
```bash
$ curl http://localhost:5000/health
{"status":"ok","timestamp":"2024-01-15T14:30:00.123456Z"}

# Verified:
# - Returns 200 ✓
# - JSON parseable ✓
# - status = "ok" ✓
# - timestamp ends with Z ✓
# - timestamp has T separator ✓
```

### Code Quality
- [x] No unrelated changes (only /health implementation)
- [x] Naming follows convention (health, not healthCheck)
- [x] No TODOs without issue refs
- [x] Error messages: N/A (no error cases in criterion)
- [x] Code readable (8 lines, clear intent)

### Documentation
- [x] README updated with /health example
- [x] Docstring in route function

### Integration Check
- [x] Existing tests still pass (12 total, 12 passing)
- [x] No new dependencies
- [x] No config changes

### CI Status
```
✓ Tests (12 passed in 0.15s)
✓ Lint (0 errors)
✓ Coverage (94%)
```

---

**Decision: APPROVED** ✅

All 4 success criteria met with evidence.
Tests comprehensive and passing.
Ready for merge.
```

---

## QA Review Comment (Changes Requested)

Example if issues were found:

```markdown
## QA Review: Changes Requested 🔄

### Pre-Review Validation
- [x] PR linked to Issue #2 ✓
- [x] Issue #2 linked to Epic #1 ✓
- [ ] PR template complete ❌ - Missing Test Evidence section
- [x] CI checks passing ✓
- [x] Branch naming correct ✓

### Success Criteria Validation

| # | Criterion | Evidence | Validated |
|---|-----------|----------|-----------|
| 1 | GET /health returns 200 | test_health_returns_200 | ✅ |
| 2 | Response: {"status": "ok", ...} | test_health_response_has_status_ok | ✅ |
| 3 | Timestamp ISO8601 with Z | **NO TEST** | ❌ |
| 4 | Unit tests for all criteria | Missing test for criterion 3 | ❌ |

### Issues Found

**Critical (must fix):**

1. **Missing test for timestamp format (Criterion 3)**
   - Story requires: "Timestamp in ISO8601 format with Z suffix"
   - No test verifies Z suffix
   - **Suggested fix:** Add test that asserts `timestamp.endswith('Z')`
   - File: tests/test_health.py

2. **PR template incomplete**
   - Test Evidence section empty
   - **Required:** Add pytest output showing test results

**Minor (should fix):**

1. **Docstring could be clearer**
   - Current: `"""Health endpoint."""`
   - Suggested: `"""Health endpoint for liveness probes. Returns 200 with status and timestamp."""`

### Next Steps
1. Add test for timestamp Z suffix
2. Fill Test Evidence section in PR description
3. Re-request review

**Blocking approval until critical issues resolved.**

I'm available if you have questions about the requested changes.
```

---

## Key Takeaways

### For Approvals
1. **Evidence for every criterion** - Show what validated it
2. **Manual testing documented** - Actual curl output
3. **CI status included** - Concrete numbers
4. **Clear decision** - "APPROVED" in bold

### For Changes Requested
1. **Specific issues** - File, line, what's wrong
2. **Suggested fixes** - Concrete, not vague
3. **Priority labeled** - Critical vs minor
4. **Actionable next steps** - Numbered list
5. **Available for questions** - Collaborative tone

---

**See also:** [04-release-notes.md](04-release-notes.md) for release after merge
