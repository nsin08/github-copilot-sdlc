# Quick Start Guide

Test the entire workflow in 30 minutes.

## Setup (5 min)

```bash
# Navigate to your project with workflow system integrated
cd your-project

# Verify templates are in place
ls .github/ISSUE_TEMPLATE/
# Should show: 00-pull-request-template.md, 01-epic.md, 02-story-task.md, 03-review-checklist.md

# Verify CLI works
gh --version
```

## Step 1: Create Epic (5 min)

**Role: Sponsor/PO**

```bash
# Create Epic using template
gh issue create \
  --title "EPIC: Service health monitoring" \
  --label "epic" \
  --body "## Problem Statement
Users cannot check if the service is running.

## Success Criteria
- [ ] Health endpoint returns 200 when healthy
- [ ] Includes timestamp in response

## Non-Goals
- No authentication
- No detailed diagnostics

## Stories
- [ ] Story 1: Health endpoint
"
```

**Output:** Epic #1 created

## Step 2: Create Story (5 min)

**Role: Tech Lead**

```bash
# Create Story from Epic
gh issue create \
  --title "Story: Health endpoint" \
  --label "story" \
  --body "Parent: #1

## Success Criteria
1. GET /health returns 200
2. Response: {\"status\": \"ok\", \"timestamp\": \"ISO8601\"}
3. Unit tests for both criteria

## Test Plan
- test_health_returns_200
- test_health_response_shape
- test_health_timestamp_format

## DoR Checklist
- [x] Success criteria clear
- [x] Test plan defined
- [x] Non-goals in Epic
"
```

**Output:** Story #2 created

## Step 3: Implement (10 min)

**Role: Implementer**

```bash
# Create branch
git checkout -b feature/2-health-endpoint

# Create minimal implementation
# (Example: Python Flask)
cat > health.py << 'EOF'
from flask import jsonify
from datetime import datetime

def health():
    return jsonify({
        "status": "ok",
        "timestamp": datetime.utcnow().isoformat() + "Z"
    }), 200
EOF

# Create tests
cat > test_health.py << 'EOF'
def test_health_returns_200(client):
    response = client.get('/health')
    assert response.status_code == 200

def test_health_response_shape(client):
    response = client.get('/health')
    data = response.get_json()
    assert "status" in data
    assert "timestamp" in data

def test_health_timestamp_format(client):
    response = client.get('/health')
    data = response.get_json()
    assert data["timestamp"].endswith("Z")
EOF

# Commit
git add .
git commit -m "feat(health): add /health endpoint

Closes #2"

# Push
git push -u origin feature/2-health-endpoint

# Create PR
gh pr create \
  --title "feat: Add health endpoint (#2)" \
  --body "Closes #2
Epic #1

## Summary
Implements health endpoint per Story #2.

## Mapping to Success Criteria
| Criterion | Evidence |
|-----------|----------|
| Returns 200 | test_health_returns_200 ✅ |
| Response shape | test_health_response_shape ✅ |
| Timestamp format | test_health_timestamp_format ✅ |

## Test Evidence
\`\`\`
3 passed in 0.05s
\`\`\`

## DoD Checklist
- [x] All criteria met
- [x] Tests added
- [x] Docs updated
"
```

**Output:** PR #3 created

## Step 4: Review (3 min)

**Role: Reviewer/QA**

```bash
# Review the PR
gh pr view 3

# Check tests pass
gh pr checks 3

# Approve
gh pr review 3 --approve --body "## QA Review: Approved ✅

All 3 criteria validated with test evidence.
Tests passing. Ready to merge."
```

**Output:** PR approved

## Step 5: Release (2 min)

**Role: Release/DevOps**

```bash
# Merge PR
gh pr merge 3 --squash

# Pull latest
git checkout main
git pull

# Create release
gh release create v0.1.0 \
  --title "v0.1.0 - Health Monitoring" \
  --notes "## Features
- Health endpoint (#3, closes #2)

Closes Epic #1"

# Close Epic
gh issue close 1 --comment "Released in v0.1.0"
```

**Output:** v0.1.0 released, Epic closed

## Verification

```bash
# Check all artifacts
gh issue list --state closed  # #1, #2 should be closed
gh pr list --state merged     # #3 should be merged
gh release list               # v0.1.0 should exist

# Test the endpoint (if applicable)
curl http://localhost:5000/health
```

## What You've Demonstrated

- ✅ Role-based workflow (5 roles: PO → Tech Lead → Implementer → QA → Release)
- ✅ Template usage (Epic, Story, PR)
- ✅ Gate enforcement (DoR, DoD)
- ✅ Artifact traceability (Epic → Story → PR → Release)
- ✅ Evidence-based review

**Time:** ~30 minutes  
**Artifacts:** 1 Epic, 1 Story, 1 PR, 1 Release

---

**Next:** Try with a more complex Epic (multiple stories)
