# Example: PR with Evidence

Shows a properly filled PR template with evidence mapping.

## PR Details

- **Title:** feat: Add health endpoint (#2)
- **Branch:** feature/2-health-endpoint
- **Base:** main

---

## PR Description (Filled Template)

```markdown
Closes #2
Epic #1

## Summary
Implements GET /health endpoint per Story #2 success criteria.

Returns JSON with status and ISO8601 timestamp for Kubernetes 
liveness probes and manual debugging.

## Mapping to Success Criteria

| # | Criterion | Evidence | Status |
|---|-----------|----------|--------|
| 1 | Returns 200 status | `test_health_returns_200` in tests/test_health.py:13 | ✅ |
| 2 | JSON: {"status": "ok", ...} | `test_health_response_has_status_ok` :18 | ✅ |
| 3 | Timestamp in ISO8601 | `test_health_timestamp_is_iso8601` :28 | ✅ |
| 4 | Unit tests for all | 4 tests added, see below | ✅ |

## Test Evidence

```
$ pytest tests/test_health.py -v

tests/test_health.py::test_health_returns_200 PASSED
tests/test_health.py::test_health_response_has_status_ok PASSED
tests/test_health.py::test_health_response_has_timestamp PASSED
tests/test_health.py::test_health_timestamp_is_iso8601 PASSED

==================== 4 passed in 0.05s ====================
```

## Files Changed

| File | Change |
|------|--------|
| src/service/app.py | Added /health route (lines 45-52) |
| tests/test_health.py | Added 4 tests |
| README.md | Added /health documentation |

## Risk Assessment

**Risk Level:** Low

**Rollback Plan:** 
- Revert commit
- No database changes
- No config changes

**Dependencies:** None

## DoD Checklist

- [x] All acceptance criteria met (4/4)
- [x] Tests added (4 new tests)
- [x] Tests passing (100%)
- [x] Docs updated (README)
- [x] PR template complete
- [x] Branch follows convention (feature/2-*)
- [x] Linked to Issue (Closes #2)
- [x] Linked to Epic (Epic #1)
- [x] No unrelated changes

## Additional Notes

Implementation uses `datetime.utcnow().isoformat() + 'Z'` for 
timestamp as required by ISO8601.
```

---

## Code Changes

### src/service/app.py (added)

```python
@app.route('/health', methods=['GET'])
def health():
    """Health endpoint for liveness probes."""
    return jsonify({
        'status': 'ok',
        'timestamp': datetime.utcnow().isoformat() + 'Z'
    }), 200
```

### tests/test_health.py (added)

```python
def test_health_returns_200(client):
    """Criterion 1: Returns 200 status code."""
    response = client.get('/health')
    assert response.status_code == 200


def test_health_response_has_status_ok(client):
    """Criterion 2: Response has status: ok."""
    response = client.get('/health')
    data = response.get_json()
    assert data['status'] == 'ok'


def test_health_response_has_timestamp(client):
    """Criterion 2: Response has timestamp."""
    response = client.get('/health')
    data = response.get_json()
    assert 'timestamp' in data


def test_health_timestamp_is_iso8601(client):
    """Criterion 3: Timestamp is ISO8601 with Z suffix."""
    response = client.get('/health')
    data = response.get_json()
    # ISO8601: YYYY-MM-DDTHH:MM:SS.sssZ
    assert data['timestamp'].endswith('Z')
    assert 'T' in data['timestamp']
```

---

## Key Takeaways

1. **Evidence table** - Each criterion maps to specific test
2. **Test output** - Actual output, not "tests pass"
3. **Files table** - Clear what changed and where
4. **Risk assessment** - Even for low-risk, state rollback plan
5. **Complete DoD** - Every checkbox checked
6. **No extras** - Only changes related to Story #2

---

**See also:** [03-qa-review.md](03-qa-review.md) for how QA reviewed this PR
