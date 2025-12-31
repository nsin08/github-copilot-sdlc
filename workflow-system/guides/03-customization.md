# Customization Guide

How to adapt the workflow system for your project.

## Required Customizations

### 1. Copilot Instructions

**File:** `.github/copilot-instructions.md`

**Change these sections:**

```markdown
# Project Overview
- Change: Project description
- Change: Tech stack
- Change: Architecture overview

# Implementation Patterns
- Change: Your service patterns
- Change: Your testing patterns
- Change: Your file structure

# Branch Naming
- Change: If different from feature/<id>-<slug>
```

**Example (Node.js project):**

```markdown
## Implementation Patterns

### Service Code (src/)
- **Express app**: Modular route structure
- **TypeScript**: Strict mode enabled
- **Prisma**: Database ORM

### Testing (tests/)
- **Jest + Supertest**: Integration tests
- **Test naming**: describe/it pattern
- **Coverage target**: >90%
- Run: `npm test`
```

### 2. CI Workflow

**File:** `.github/workflows/test.yml`

**Change:**

```yaml
# Before (Python)
- name: Set up Python 3.11
  uses: actions/setup-python@v4

- name: Run tests
  run: pytest tests/

# After (Node.js)
- name: Set up Node.js
  uses: actions/setup-node@v3
  with:
    node-version: '18'

- name: Run tests
  run: npm test
```

## Optional Customizations

### Issue Template Labels

**Files:** `.github/ISSUE_TEMPLATE/*.md`

```yaml
---
name: Epic
labels: ['epic', 'your-project-label']  # Add your labels
---
```

### PR Template Sections

**File:** `.github/ISSUE_TEMPLATE/00-pull-request-template.md`

Add project-specific sections:

```markdown
## Security Checklist
- [ ] No secrets in code
- [ ] Input validation added
- [ ] SQL injection prevented

## Performance
- [ ] No N+1 queries
- [ ] Caching considered
```

### Additional Rules

**Add to:** `.github/workflow-system/rules/`

Create new rule file:

```markdown
# Rule: <Your Rule Name>

## Summary
One-line description.

## Rule Definition
Your rule details.

## Examples
Good/bad examples.
```

Update index:
```markdown
| 07 | Your Rule | [07-your-rule.md](07-your-rule.md) | Description |
```

### Additional Roles

**Add to:** `.github/workflow-system/roles/`

Create new role file following template in `00-index.md`.

## Customization Checklist

### Must Do
- [ ] Update `copilot-instructions.md` with your tech stack
- [ ] Update `workflows/test.yml` with your test commands

### Should Do
- [ ] Add project-specific labels to templates
- [ ] Review DoR/DoD rules for your context
- [ ] Customize PR template sections

### Optional
- [ ] Add new rules specific to your project
- [ ] Add new roles if team structure differs
- [ ] Add more example files

## Tech Stack Examples

### Python/Flask
```yaml
# workflows/test.yml
- uses: actions/setup-python@v4
  with:
    python-version: '3.11'
- run: pip install -r requirements.txt
- run: pytest tests/ -v --cov
```

### Node.js/Express
```yaml
# workflows/test.yml
- uses: actions/setup-node@v3
  with:
    node-version: '18'
- run: npm ci
- run: npm test -- --coverage
```

### Java/Spring
```yaml
# workflows/test.yml
- uses: actions/setup-java@v3
  with:
    java-version: '17'
    distribution: 'temurin'
- run: mvn test
```

### Go
```yaml
# workflows/test.yml
- uses: actions/setup-go@v4
  with:
    go-version: '1.21'
- run: go test ./...
```

## Keeping Customizations

### If Using Submodule

When using as a submodule, you have two options:

**Option 1: Use directly (simplest)**
```
.github/                        # Submodule (stays in sync)
├── copilot-instructions.md     # From submodule
├── workflows/
│   └── test.yml                # From submodule
├── ISSUE_TEMPLATE/             # From submodule
└── workflow-system/            # From submodule
```

**Option 2: Fork & customize (for extensive changes)**
- Fork the repository
- Add your fork as the submodule
- Customize freely while keeping git tracking

### If Copied

Just edit files directly. Track changes in version control.

---

**Time:** 20 minutes  
**Next:** Run [02-quick-start.md](02-quick-start.md) with your customized setup
