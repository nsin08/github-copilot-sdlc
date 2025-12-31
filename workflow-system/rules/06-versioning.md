# Rule: Versioning

## Summary

Use Semantic Versioning (SemVer) for all releases.

## Rule Definition

### Version Format

```
v<MAJOR>.<MINOR>.<PATCH>

Examples:
- v0.1.0  (initial development)
- v1.0.0  (first stable release)
- v1.2.3  (stable with patches)
```

### Version Increment Rules

| Change Type | Increment | Example |
|-------------|-----------|---------|
| Breaking changes | MAJOR | v1.0.0 → v2.0.0 |
| New features (backward compatible) | MINOR | v1.0.0 → v1.1.0 |
| Bug fixes (backward compatible) | PATCH | v1.0.0 → v1.0.1 |

### Pre-release Versions

```
v0.x.x     - Initial development (anything may change)
v1.0.0-alpha.1  - Alpha release
v1.0.0-beta.1   - Beta release
v1.0.0-rc.1     - Release candidate
v1.0.0          - Stable release
```

### Release Process

1. **All PRs Merged:** Ensure all stories for this release are merged
2. **CI Green:** Verify main branch CI passes
3. **Create Tag:** `git tag v1.2.3`
4. **Push Tag:** `git push origin v1.2.3`
5. **Generate Notes:** Create GitHub release with notes
6. **Close Epic:** Mark Epic as Released

### Release Notes Format

```markdown
## v1.2.0 - 2025-12-31

### ✨ Features
- Add health endpoint (#10, PR #15)
- Add metrics endpoint (#11, PR #16)

### 🐛 Fixes
- Fix null pointer in calculator (#20, PR #21)

### 📚 Documentation
- Update API documentation (#25, PR #26)

### 🔧 Maintenance
- Update dependencies (#30, PR #31)
```

## Examples

### ✅ Good Release

```bash
# All stories merged, CI green
git tag v0.1.0
git push origin v0.1.0

# GitHub release created with:
# - Title: v0.1.0 - Service Readiness Baseline
# - Notes: List of features with PR/issue links
# - Epic #5 closed
```

### ❌ Bad Release

```bash
# Tagged without checking CI
git tag v0.1.0  # CI was failing ❌

# No release notes
# Just a tag with no documentation ❌

# Epic not closed
# Stories still marked "Done" not "Released" ❌
```

## Enforcement

- **CI Check:** Only release if main branch green
- **Release Template:** GitHub release template includes required sections
- **Automation:** GitHub Actions can auto-generate notes from PRs

## Exceptions

- **Hotfix:** May release patch version outside normal cycle
- **Canary:** Continuous deployment may use commit SHA instead of tags

---

**Rule ID:** VER-001  
**Category:** Release  
**Severity:** Mandatory
