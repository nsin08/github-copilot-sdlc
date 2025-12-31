# Rule: PR Hygiene

## Summary

Pull requests must be small, focused, and well-documented.

## Rule Definition

### Size Guidelines

| Metric | Target | Maximum |
|--------|--------|---------|
| Lines changed | <300 | 500 |
| Files changed | <10 | 20 |
| Stories per PR | 1 | 1 |
| Commits | 1-3 | 5 |

### Content Rules

- [ ] **Single Focus:** One story/feature per PR
- [ ] **No Drive-by Refactors:** Unrelated changes go in separate PR
- [ ] **No TODOs Without Issues:** Every TODO references an issue `// TODO(#123): ...`
- [ ] **Requirements in Issues:** No requirements in code comments
- [ ] **Template Complete:** All PR template sections filled (no placeholders)
- [ ] **Evidence Mapped:** Each success criterion linked to test/code

### Commit Messages

**Format:**
```
<type>(<scope>): <description>

[optional body]

[optional footer: Closes #<id>]
```

**Types:** `feat`, `fix`, `docs`, `test`, `chore`, `refactor`

**Examples:**
```
feat(health): add /health endpoint

- Returns 200 with status:ok
- Includes ISO8601 timestamp
- Tests added

Closes #15
```

### Branch Naming

**Pattern:** `<type>/<issue-id>-<short-description>`

| Type | Use Case | Example |
|------|----------|---------|
| `feature/` | New functionality | `feature/15-health-endpoint` |
| `fix/` | Bug fixes | `fix/42-null-pointer` |
| `chore/` | Maintenance | `chore/50-update-deps` |
| `docs/` | Documentation | `docs/55-api-readme` |

## Examples

### ✅ Good PR

```markdown
Title: feat(health): Add health endpoint (#15)

## Summary
Implements GET /health as specified in Story #15.

## Changes
- src/app.py: Added /health route
- tests/test_health.py: Added 3 tests

## Evidence
- Criterion 1: ✅ → test_health_returns_200
- Criterion 2: ✅ → test_health_json_shape

Lines: +45 -0 | Files: 2 | Commits: 1
```

### ❌ Bad PR

```markdown
Title: Updates

## Summary
Made some changes.

## Changes
- Fixed health endpoint
- Also refactored database
- And updated some docs
- Plus fixed typos

Lines: +1,247 -834 | Files: 47 | Commits: 23
❌ Too large, unfocused, vague description
```

## Enforcement

- **Template:** PR template requires all sections
- **Review:** Reviewer can reject oversized PRs
- **CI:** Optionally add size check action

## Exceptions

- **Generated Code:** May exceed size limits (note in PR)
- **Initial Setup:** First PR may be larger (setting up project)
- **Migrations:** Database migrations may touch many files

---

**Rule ID:** PR-001  
**Category:** Code Quality  
**Severity:** Mandatory
