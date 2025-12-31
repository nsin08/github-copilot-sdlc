# Rule: Artifact Linking

## Summary

All artifacts must maintain traceability from idea to release.

## Rule Definition

### Linking Requirements

```
Idea Issue
    ↓ (converted to)
Epic Issue
    ↓ (broken into)
Story/Task Issues ──→ "Parent: #<epic-id>" in body
    ↓ (implemented in)
Branch ──→ "feature/<issue-id>-<slug>"
    ↓ (reviewed via)
Pull Request ──→ "Closes #<story-id>" in body
    ↓ (merged to)
Main Branch
    ↓ (released as)
Release/Tag ──→ Notes reference PRs and issues
```

### Specific Rules

#### Issues
- **Epic → Stories:** Each story body contains `Parent: #<epic-id>`
- **Story → Tasks:** Each task body contains `Parent: #<story-id>`

#### Branches
- **Pattern:** `<type>/<issue-id>-<description>`
- **Types:** `feature/`, `fix/`, `chore/`, `docs/`
- **Example:** `feature/42-health-endpoint`

#### Pull Requests
- **Must link:** `Closes #<issue-id>` or `Fixes #<issue-id>` in body
- **One-to-one:** Each PR closes exactly one issue (preferably)
- **Epic reference:** Also mention Epic: `Epic #<epic-id>`

#### Releases
- **Tag format:** `v<major>.<minor>.<patch>`
- **Notes:** Reference all merged PRs and closed issues
- **Epic closure:** Close Epic when all child stories released

## Examples

### ✅ Correct Linking

**Story Issue Body:**
```markdown
Parent: #10

## Success Criteria
...
```

**PR Body:**
```markdown
Closes #15
Epic #10

## Summary
Implements health endpoint as specified in Story #15.
```

**Branch Name:**
```
feature/15-health-endpoint
```

### ❌ Incorrect Linking

```markdown
# PR body missing link
## Summary
Added health endpoint.
❌ No "Closes #" reference
```

```markdown
# Multiple issues in one PR
Closes #15, Closes #16, Closes #17
❌ Too many issues (hard to review, rollback)
```

```markdown
# Branch name missing issue
feature/add-health-endpoint
❌ No issue ID
```

## Enforcement

- **Templates:** Issue and PR templates include linking fields
- **CI Check:** GitHub Action can validate PR has `Closes #` reference
- **Review:** Reviewer checks linking before approval

## Exceptions

- **Pure Chore:** Chore PRs (formatting, deps) may not have story, but still need issue
- **Hotfix:** May bypass normal linking but must document in PR

---

**Rule ID:** LINK-001  
**Category:** Traceability  
**Severity:** Mandatory
