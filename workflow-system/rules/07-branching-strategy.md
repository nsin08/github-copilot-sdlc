# Branching Strategy

Trunk-based development with feature branches and optional develop branch for staged releases.

## Overview

**Philosophy:** Keep branches short-lived, integrate frequently, release from stable branches.

**Model:** Git Flow inspired with trunk-based flexibility

```
main (production-ready, tagged releases)
  ↑
  ├── hotfix/* (emergency fixes)
  └── release/* (release stabilization) [optional]
        ↑
      develop (integration/staging) [optional]
        ↑
      feature/*, fix/*, chore/*, docs/* (short-lived)
```

**Branch Hierarchy:**
- **Long-lived:** `main`, `develop` (optional)
- **Medium-lived:** `release/*` (days to weeks, optional)
- **Short-lived:** `feature/*`, `fix/*`, `chore/*`, `docs/*` (hours to days)
- **Emergency:** `hotfix/*` (hours, fast-track)

---

## Branch Types

### 1. Main Branch (`main` or `master`)

**Purpose:** Production-ready code  
**Protection:** Required  
**Merges:** Only from feature branches or develop (via PR)  
**Tags:** All releases (`v1.0.0`, `v1.1.0`, etc.)

**Rules:**
- ✅ Always deployable
- ✅ All tests passing
- ✅ Requires PR approval
- ❌ No direct commits
- ❌ No force push

**Settings:**
```yaml
Protected: true
Required approvals: 1+
Required status checks: CI/tests
Restrict force push: true
Restrict deletions: true
```

### 2. Develop Branch (`develop`) [Optional]

**Purpose:** Integration branch for staged releases  
**Use When:**
- Multiple features need coordinated release
- QA needs stable integration environment
- You release on schedule (not continuous)

**Use Direct to Main When:**
- Continuous deployment
- Single feature releases
- Small team (<5 people)

**Rules:**
- ✅ Merge feature branches here first
- ✅ Test integration before promoting to main
- ✅ Can be reset if needed
- ❌ Don't commit directly

**Flow:**
```bash
feature/123-new-thing → develop → main
```

### 3. Release Branches (`release/*`) [Optional]

**Purpose:** Prepare and stabilize release candidates  
**Pattern:** `release/<version>` or `release/v<version>`  
**Example:** `release/v1.2.0`, `release/1.2.0`

**Use When:**
- You need release testing/QA cycle
- Multiple features bundled in versioned releases
- You have staging/pre-production environment
- Release requires final hardening (version bumps, changelog, etc.)

**Don't Use When:**
- Continuous deployment (use develop → main)
- Hotfixes (use dedicated hotfix branches)

**Lifecycle:**
1. Branch from `develop` when feature-complete
2. Stabilize: only bug fixes, no new features
3. Merge to both `main` AND back to `develop`
4. Tag release on `main`
5. Delete release branch

**Rules:**
- ✅ Branch from `develop` when ready for release
- ✅ Only bug fixes allowed (no new features)
- ✅ Must merge to both `main` AND `develop`
- ✅ Delete after successful release
- ❌ No feature work
- ❌ Don't branch other features from release branch

**Flow:**
```bash
develop ──→ release/v1.2.0 ──→ main (tag v1.2.0)
                    └──────→ develop (backport fixes)
```

### 4. Hotfix Branches (`hotfix/*`)

**Purpose:** Emergency fixes for production  
**Pattern:** `hotfix/<issue-id>-<description>` or `hotfix/v<version>`  
**Example:** `hotfix/999-security-patch`, `hotfix/v1.2.1`

**Use When:**
- Critical production bug
- Security vulnerability
- Data corruption issue
- Cannot wait for normal release cycle

**Lifecycle:**
1. Branch from `main` (production)
2. Fix the issue
3. Merge to both `main` AND `develop`
4. Tag patch release on `main`
5. Delete hotfix branch

**Rules:**
- ✅ Branch from `main` (not develop)
- ✅ Fast-track review and merge
- ✅ Must merge to both `main` AND `develop`
- ✅ Create patch version (1.2.0 → 1.2.1)
- ❌ Only critical fixes
- ❌ No feature work

**Flow:**
```bash
main ──→ hotfix/v1.2.1 ──→ main (tag v1.2.1)
                  └────→ develop (backport fix)
```

### 5. Feature Branches (Short-lived)

**Pattern:** `<type>/<issue-id>-<short-description>`

| Type | Purpose | Example |
|------|---------|---------|
| `feature/` | New functionality | `feature/15-health-endpoint` |
| `fix/` | Bug fixes (non-critical) | `fix/42-null-pointer` |
| `chore/` | Maintenance | `chore/50-update-deps` |
| `docs/` | Documentation | `docs/55-api-readme` |
| `refactor/` | Code restructuring | `refactor/60-service-layer` |
| `test/` | Test improvements | `test/65-integration-tests` |

**Lifecycle:**
1. **Create:** Branch from `develop` (or `main` if no develop)
2. **Develop:** Commit changes, push regularly
3. **PR:** Open pull request when ready
4. **Review:** Address feedback
5. **Merge:** Squash or merge to target branch
6. **Delete:** Automatically after merge

**Rules:**
- ✅ One feature/fix per branch
- ✅ Link to issue in branch name
- ✅ Keep under 5 days old
- ✅ Rebase/merge from target regularly
- ❌ Don't branch from other features

---

## Workflows

### Workflow 1: Trunk-Based (No Develop)

**Best for:** Continuous deployment, small teams

```bash
# Create feature
git checkout main
git pull
git checkout -b feature/123-add-widget

# Develop
# ... make changes ...
git add .
git commit -m "feat: add widget component"
git push -u origin feature/123-add-widget

# Create PR to main
gh pr create --base main

# After merge, main is deployed immediately
```

### Workflow 2: Develop Branch (Staged Releases)

**Best for:** Scheduled releases, integration testing

```bash
# Create feature from develop
git checkout develop
git pull
git checkout -b feature/123-add-widget

# Develop
# ... make changes ...
git commit -m "feat: add widget component"
git push -u origin feature/123-add-widget

# PR to develop first
gh pr create --base develop

# After merge to develop, test integration
# When ready for release:
git checkout main
git pull
git merge develop
git tag v1.2.0
git push origin main --tags
```

### Workflow 3: Release Branch (Staged Release)

**For coordinated releases requiring stabilization**

```bash
# When develop is feature-complete for v1.2.0
git checkout develop
git pull
git checkout -b release/v1.2.0

# Stabilization: bump version, update changelog
echo "1.2.0" > VERSION
git add VERSION
git commit -m "chore: bump version to 1.2.0"

# Only bug fixes allowed during stabilization
git commit -m "fix: resolve edge case in validation"

# Push release branch
git push -u origin release/v1.2.0

# After QA approval, merge to main
git checkout main
git pull
git merge release/v1.2.0
git tag v1.2.0
git push origin main --tags

# Backport to develop
git checkout develop
git merge release/v1.2.0
git push

# Delete release branch
git branch -d releasType | Branch Name | PR Title |
|----------|-------------|-------------|----------|
| Story #15 | Feature | `feature/15-health-endpoint` | `feat(health): Add health endpoint (#15)` |
| Bug #42 | Fix | `fix/42-null-pointer` | `fix(auth): Handle null user (#42)` |
| Release v1.2.0 | Release | `release/v1.2.0` | `release: Version 1.2.0` |
| Hotfix #999 | Hotfix | `hotfix/999-security-patch` | `fix: Critical security patch (#999)` |
| Dependencies | Chore | `chore/50-update-deps` | `chore: Update dependencies (#50)` |
| API docs | Docs | `docs/55-api-readme` | `docs: Add API usage guide (#55)` |
| Refactoring | Refactorl bugs requiring immediate fix**

```bash
# Branch from main (production code)
git checkout main
git pull
git checkout -b hotfix/999-critical-security

# Fix the issue
# ... make the fix ...
git commit -m "fix: patch critical security vulnerability"
git push -u origin hotfix/999-critical-security

# Fast-track PR to main
gh pr create --base main --label "hotfix" --label "priority:critical"

# After approval and merge to main
git checkout main
git pull

# Create patch release
git tag v1.2.1
git push origin main --tags

# Backport to develop
git checkout develop
git pull
git merge main
git push

# Delete hotfix branch
git branch -d hotfix/999-critical-security
git push origin --delete hotfix/999-critical-security
```

---

## Branch Naming Examples

| Scenario | Branch Name | PR Title |
|----------|-------------|----------|
| Story #15 | `feature/15-health-endpoint` | `feat(health): Add health endpoint (#15)` |
| Bug #42 | `fix/42-null-pointer` | `fix(auth): Handle null user (#42)` |
| Dependencies | `chore/50-update-deps` | `chore: Update dependencies (#50)` |
| API docs | `docs/55-api-readme` | `docs: Add API usage guide (#55)` |
| Refactoring | `refactor/60-service-layer` | `refactor(api): Extract service layer (#60)` |

---

## Best Practices

### ✅ Do

- **Branch per Story/Task:** One issue = one branch
- **Pull frequently:** `git pull origin develop` daily
- **Push regularly:** Backup work, enable collaboration
- **Small PRs:** Easier to review, faster to merge
- **Delete after merge:** Keep repository clean
- **Descriptive names:** Include issue number + summary
- **Commit often:** Small, logical commits
- **Rebase before PR:** Clean up history

### ❌ Don't

- **Long-lived feature branches:** Merge within 1-5 days
- **Branch from branches:** Feature → develop/main only (not from other features)
- **Hoard changes:** Don't wait for "perfect"
- **Force push:** After others have pulled (use `--force-with-lease`)
- **Bypass PRs:** Even for "small" changes
- **Work on main/develop:** Always use feature branches
- **Features in release branches:** Only bug fixes during stabilization
- **Forget to backport:** Hotfixes and release fixes must go to both main AND develop

---

## Merge Strategies

### 1. Squash and Merge (Recommended)

**Use for:** Most feature branches

**Pros:**
- Clean linear history
- One commit per feature
- Easy to revert
- Good for small/medium PRs

**Result:**
```
main: A ← B ← C ← D (squashed feature)
```

**Command:**
```bash
gh pr merge --squash --delete-branch
```

### 2. Merge Commit

**Use for:** Large features, complex history you want to preserve

**Pros:**
- Preserves all commits
- Shows branch history
- Good for auditing

**Result:**
```
main: A ← B ← F (merge commit)
           ↖ C ← D ← E (feature)
```

**Command:**
```bash
gh pr merge --merge --delete-branch
```

### 3. Rebase and Merge

**Use for:** When you want clean commits without squashing

**Pros:**
- Linear history
- Preserves individual commits
- No merge commits

**Result:**
```
main: A ← B ← C ← D ← E (feature commits)
```

**Command:**
```bash
gh pr merge --rebase --delete-branch
```

---

## Integration with Issue Workflow

### Story Issue → Feature Branch

```markdown
Story Issue #15: Add health endpoint
State: Spec Ready

↓ Create branch
feature/15-health-endpoint
State: In Progress

↓ Open PR
PR #20: feat(health): Add health endpoint (#15)
State: In Review

↓ Merge to develop (or main)
State: Done

↓ Tag release
v1.2.0
State: Released
Issue #15 closed
```

---

## Configuration

### GitHub Branch Protection

**For `main`:**
```yaml
Settings → Branches → Add rule

Branch name pattern: main

Protections:
☑ Require pull request before merging
  ☑ Require approvals (1)
  ☑ Dismiss stale approvals
  ☑ Require review from Code Owners
☑ Require status checks to pass
  ☑ Require branches to be up to date
  ☑ Status checks: CI, tests
☑ Require conversation resolution
☑ Require signed commits (optional)
☑ Require linear history (optional)
☑ Do not allow bypassing the above settings
☑ Restrict who can push to matching branches
```

**For `develop` (if used):**
```yaml
Same as main, but:
- Optional: Allow force push with lease (for cleanup)
- Optional: Lower approval count (0-1)
```

### .gitignore Branch Patterns

```gitignore
# No need to ignore branches, but document abandoned patterns
# Old pattern: feature-* (if you used feature-15-name)
# New pattern: feature/* (enforced by PR template)
```

---

## Troubleshooting

### Branch Out of Sync

```bash
# Update your branch with latest from develop
git checkout feature/123-my-feature
git fetch origin
git rebase origin/develop

# If conflicts
git status  # resolve conflicts
git add .
git rebase --continue
git push --force-with-lease
```

### Wrong Base Branch

```bash
# Created from main, need to target develop
git rebase --onto develop main feature/123-my-feature
git push --force-with-lease

# Update PR base on GitHub
gh pr edit 123 --base develop
```

### Abandoned Feature

```bash
# Close PR
gh pr close 123

# Delete branch
git branch -d feature/123-abandoned
git push origin --delete feature/123-abandoned

# Close/comment on issue
gh issue comment 123 --body "Abandoned, see #124 for new approach"
gh issue close 123
```Start release** | `git checkout develop && git pull && git checkout -b release/v1.2.0` |
| **Start hotfix** | `git checkout main && git pull && git checkout -b hotfix/999-name` |
| **Update from base** | `git fetch && git rebase origin/develop` |
| **Create PR** | `gh pr create --base develop` |
| **Merge PR** | `gh pr merge --squash --delete-branch` |
| **Tag release** | `git tag v1.2.0 && git push origin main --tags
## Quick Reference

| Action | Command |
|--------|---------|
| **Start feature** | `git checkout develop && git pull && git checkout -b feature/123-name` |
| **Update from base** | `git fetch && git rebase origin/develop` |
| **Create PR** | `gh pr create --base develop` |
| **Merge PR** | `gh pr merge --squash --delete-branch` |
| **Hotfix** | `git checkout main && git checkout -b fix/999-name` |
| **Delete local** | `git branch -d feature/123-name` |
| **Delete remote** | `git push origin --delete feature/123-name` |

---

**Related:**
- [04-artifact-linking.md](04-artifact-linking.md) - Branch naming requirements
- [05-pr-hygiene.md](05-pr-hygiene.md) - PR standards
- [06-versioning.md](06-versioning.md) - Release tagging

**Enforcement:** See [guides/04-enforcement-automation.md](../guides/04-enforcement-automation.md) for GitHub Actions
