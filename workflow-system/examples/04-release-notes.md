# Example: Release Notes

Shows properly formatted release documentation.

## GitHub Release

**Tag:** v0.1.0  
**Title:** v0.1.0 - Service Monitoring Baseline

---

## Release Notes Body

```markdown
## v0.1.0 - Service Monitoring Baseline

**Release Date:** 2024-01-15

### Summary
Initial release with health and metrics endpoints for service monitoring.
Enables Kubernetes liveness probes and basic observability.

### Features
- **Health endpoint** - GET /health returns 200 with status and timestamp
  - PR #10, closes #2
  - For: Kubernetes liveness probes, debugging
  
- **Metrics endpoint** - GET /metrics returns request count and uptime
  - PR #11, closes #3
  - For: Basic observability, monitoring dashboards

### API Reference

#### GET /health
```json
{
  "status": "ok",
  "timestamp": "2024-01-15T12:00:00.123456Z"
}
```

#### GET /metrics
```json
{
  "requests_total": 42,
  "uptime_seconds": 3600
}
```

### Upgrade Guide
New installation - no migration needed.

### Breaking Changes
None (initial release).

### Known Issues
None.

### Contributors
- @implementer - Health endpoint, Metrics endpoint

### Traceability
- **Epic:** #1 (Service Monitoring Baseline)
- **Stories:** #2 (Health), #3 (Metrics)
- **PRs:** #10, #11
- **Tests:** 8 new tests, 100% coverage on new code

---

Full changelog: https://github.com/org/repo/compare/v0.0.0...v0.1.0
```

---

## CHANGELOG.md Entry

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2024-01-15

### Added
- Health endpoint (`GET /health`) for liveness probes (#2)
- Metrics endpoint (`GET /metrics`) for observability (#3)
- Unit tests for health endpoint (4 tests)
- Unit tests for metrics endpoint (4 tests)
- Documentation for new endpoints

### Changed
- Nothing

### Deprecated
- Nothing

### Removed
- Nothing

### Fixed
- Nothing

### Security
- Nothing

---

## [0.0.0] - 2024-01-01

### Added
- Initial project structure
- Basic Flask app skeleton
```

---

## Epic Closure Comment

```markdown
## Epic Completed ✅

### Delivered
All success criteria from Epic #1 met:

| Criterion | Delivered | Evidence |
|-----------|-----------|----------|
| Health endpoint | ✅ | PR #10, Story #2 |
| Metrics endpoint | ✅ | PR #11, Story #3 |
| Tests for all | ✅ | 8 tests, 100% coverage |
| Docs updated | ✅ | README updated in both PRs |

### Artifacts Created
- Stories: #2, #3
- PRs: #10, #11
- Release: v0.1.0
- Tag: v0.1.0

### Traceability Map
```
Epic #1 (Service Monitoring Baseline)
├── Story #2 (Health) → PR #10 → merged
├── Story #3 (Metrics) → PR #11 → merged
└── Release v0.1.0
```

### PO Validation
@PO - Please validate:
- [ ] /health works as expected
- [ ] /metrics works as expected
- [ ] Docs sufficient

Closing Epic after PO confirmation.
```

---

## PO Validation Response

```markdown
@release - Validated ✅

Tested both endpoints:
- /health returns expected format ✓
- /metrics shows correct counters ✓
- Docs clear and helpful ✓

**Approved for closure.**

Thanks team for clean delivery! 🎉
```

---

## Key Takeaways

### Release Notes
1. **Summary first** - What this release does
2. **Feature list** - With PR/Issue links
3. **API docs** - Actual examples
4. **Traceability** - Link back to Epic/Stories
5. **Upgrade guide** - Even if "none needed"

### CHANGELOG
1. **Keep a Changelog format** - Standard structure
2. **Issue references** - (#2, #3)
3. **All sections** - Even if empty

### Epic Closure
1. **Evidence table** - Criterion → Evidence
2. **Artifact list** - Everything created
3. **Traceability map** - Visual hierarchy
4. **PO validation** - Explicit confirmation request

---

**This completes the full workflow cycle:**
Epic → Stories → PRs → Reviews → Release → Close
