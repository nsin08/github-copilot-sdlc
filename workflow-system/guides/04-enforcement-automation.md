# Enforcement & Automation Guide

How to enforce workflow rules with GitHub features and automation.

## Overview

The base workflow system relies on **discipline and templates**. This guide adds **technical enforcement** via GitHub settings and Actions.

## Current State

**What's Included (Base System):**
- ✅ Templates (Epic, Story, Review, PR)
- ✅ Documentation (rules, roles, examples)
- ✅ Basic CI (test runner example)

**What's NOT Included (Optional Enforcement):**
- ❌ CODEOWNERS for required reviewers
- ❌ Branch protection rules
- ❌ Automated validation (PR links, branch naming, etc.)
- ❌ Advanced security scanning
- ❌ Deployment pipelines

**Why Not Included:** These are repository-specific settings that require admin access and vary by organization policy.

---

## Enforcement Options

### 1. Branch Protection Rules

**Purpose:** Prevent force pushes, require PR reviews, enforce CI

**Setup (Repository Admin):**

```bash
# Via GitHub CLI
gh api repos/:owner/:repo/branches/main/protection -X PUT --input - <<EOF
{
  "required_status_checks": {
    "strict": true,
    "contexts": ["test"]
  },
  "enforce_admins": true,
  "required_pull_request_reviews": {
    "dismiss_stale_reviews": true,
    "require_code_owner_reviews": false,
    "required_approving_review_count": 1
  },
  "restrictions": null,
  "required_linear_history": false,
  "allow_force_pushes": false,
  "allow_deletions": false
}
EOF
```

**Or via Web UI:**
1. Settings → Branches → Add rule
2. Branch name pattern: `main`
3. Enable:
   - ✅ Require a pull request before merging
   - ✅ Require approvals (1)
   - ✅ Require status checks to pass (select "test")
   - ✅ Require branches to be up to date
   - ✅ Require linear history

---

### 2. CODEOWNERS File

**Purpose:** Auto-assign reviewers, enforce role-based review

**Create:** `.github/CODEOWNERS`

```plaintext
# Default: All files require review from QA role
* @your-org/qa-team

# Epic/Story templates: Tech Lead reviews
.github/ISSUE_TEMPLATE/*.md @your-org/tech-leads

# Code: Implementers can self-review, but QA required for merge
/src/ @your-org/implementers
/tests/ @your-org/implementers

# Release/DevOps: Only release team can modify workflows
.github/workflows/ @your-org/release-team
```

**Note:** Requires GitHub Team setup.

---

### 3. PR Template Validation (GitHub Action)

**Purpose:** Ensure PR template is filled, links are present

**Create:** `.github/workflows/pr-validation.yml`

```yaml
name: PR Validation

on:
  pull_request:
    types: [opened, edited, synchronize]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Check PR links Issue
        run: |
          PR_BODY="${{ github.event.pull_request.body }}"
          
          # Check for "Closes #X"
          if ! echo "$PR_BODY" | grep -qE "Closes #[0-9]+"; then
            echo "❌ PR must link to an issue: 'Closes #X'"
            exit 1
          fi
          
          # Check for "Epic: #X"
          if ! echo "$PR_BODY" | grep -qE "Epic: #[0-9]+"; then
            echo "⚠️ Warning: PR should reference Epic: 'Epic: #X'"
          fi
          
          echo "✅ PR links validated"
      
      - name: Check PR template sections
        run: |
          PR_BODY="${{ github.event.pull_request.body }}"
          
          REQUIRED_SECTIONS=(
            "What changed"
            "Why"
            "Mapping to Success Criteria"
            "Test Evidence"
            "Checklist"
          )
          
          for section in "${REQUIRED_SECTIONS[@]}"; do
            if ! echo "$PR_BODY" | grep -q "$section"; then
              echo "❌ Missing section: $section"
              exit 1
            fi
          done
          
          echo "✅ PR template sections present"
      
      - name: Check branch naming
        run: |
          BRANCH="${{ github.head_ref }}"
          
          if ! echo "$BRANCH" | grep -qE "^(feature|chore|hotfix)/[0-9]+-"; then
            echo "❌ Branch must follow pattern: feature/<issue-id>-<slug>"
            echo "   Current: $BRANCH"
            exit 1
          fi
          
          echo "✅ Branch naming validated"
```

---

### 4. Issue Linking Validation

**Purpose:** Ensure Stories link to Epic with "Parent: #X"

**Create:** `.github/workflows/issue-validation.yml`

```yaml
name: Issue Validation

on:
  issues:
    types: [opened, edited]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - name: Check Story has Parent
        if: contains(github.event.issue.labels.*.name, 'story')
        run: |
          BODY="${{ github.event.issue.body }}"
          
          if ! echo "$BODY" | grep -qE "Parent: #[0-9]+"; then
            echo "❌ Story must link to Epic: 'Parent: #X'"
            gh issue comment ${{ github.event.issue.number }} --body "⚠️ **Missing Parent Link**: This Story must reference its parent Epic with \`Parent: #X\` in the body."
            exit 1
          fi
          
          echo "✅ Story links to parent Epic"
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

### 5. Security & Compliance

**Purpose:** Automated security scanning

**Setup:**

1. **Enable Dependabot** (Settings → Security → Code security and analysis):
   - ✅ Dependabot alerts
   - ✅ Dependabot security updates
   - ✅ Grouped security updates

2. **Enable CodeQL** (Security → Code scanning):
   ```bash
   # Create .github/workflows/codeql.yml
   gh workflow create codeql-analysis
   ```

3. **Create:** `.github/dependabot.yml`
   ```yaml
   version: 2
   updates:
     - package-ecosystem: "pip"
       directory: "/"
       schedule:
         interval: "weekly"
       reviewers:
         - "your-org/release-team"
   ```

---

### 6. Release Automation

**Purpose:** Auto-generate release notes from merged PRs

**Create:** `.github/workflows/release.yml`

```yaml
name: Release

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      
      - name: Generate Release Notes
        id: notes
        run: |
          # Get previous tag
          PREV_TAG=$(git tag --sort=-v:refname | sed -n '2p')
          CURRENT_TAG=${GITHUB_REF#refs/tags/}
          
          # Get commits between tags
          COMMITS=$(git log ${PREV_TAG}..${CURRENT_TAG} --pretty=format:"- %s (%h)" --no-merges)
          
          # Extract PR numbers and fetch linked issues
          echo "## Changes in $CURRENT_TAG" > notes.md
          echo "" >> notes.md
          echo "$COMMITS" >> notes.md
          
          cat notes.md
      
      - name: Create GitHub Release
        uses: softprops/action-gh-release@v1
        with:
          body_path: notes.md
          draft: false
          prerelease: false
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## Enforcement Checklist

Use this to determine what enforcement to add:

### Basic Enforcement (Recommended for All)
- [ ] Branch protection on `main` (no direct pushes)
- [ ] Require 1 approval before merge
- [ ] Require CI to pass before merge
- [ ] Enable Dependabot alerts

### Moderate Enforcement (Team Projects)
- [ ] CODEOWNERS for role-based review
- [ ] PR template validation workflow
- [ ] Branch naming validation
- [ ] Issue parent linking validation

### Advanced Enforcement (Enterprise)
- [ ] CodeQL security scanning
- [ ] SBOM generation
- [ ] Signed commits required
- [ ] Deployment environments with approvals
- [ ] Automated rollback workflows

---

## Trade-offs

| Enforcement Level | Pros | Cons |
|-------------------|------|------|
| **None (Base System)** | Flexible, easy to start | Relies on discipline |
| **Basic** | Prevents common mistakes | Minimal overhead |
| **Moderate** | Strong workflow compliance | Requires team coordination |
| **Advanced** | Enterprise-grade security | Complex, may slow velocity |

**Recommendation:** Start with **Basic**, add **Moderate** when team grows beyond 5 people, add **Advanced** only if required by compliance.

---

## Implementation Plan

**Week 1:**
1. Enable branch protection on `main`
2. Add CODEOWNERS file
3. Test with 1-2 PRs

**Week 2:**
4. Add PR validation workflow
5. Add issue validation workflow
6. Document for team

**Month 2:**
7. Enable Dependabot
8. Add security scanning (if needed)

**As Needed:**
9. Deployment pipelines
10. Release automation

---

**Version:** 1.0  
**Status:** Optional Enhancement

