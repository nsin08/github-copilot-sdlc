# Integration Guide

How to integrate this workflow system into your project.

## Prerequisites

- Git repository on GitHub
- Admin access to repository settings
- GitHub CLI (`gh`) installed (optional but recommended)

## Integration Methods

### Method 1: Copy Directory (Recommended)

```bash
# Copy .github to your project
cp -r path/to/.github your-project/

# Customize for your project
# Edit: .github/copilot-instructions.md
# Edit: .github/workflows/test.yml

# Commit
git add .github
git commit -m "Add workflow system"
git push
```

### Method 2: Git Submodule (Recommended for Staying Updated)

```bash
# Add as submodule directly to .github
git submodule add https://github.com/nsin08/github-copilot-sdlc.git .github
git submodule update --init --recursive

# Commit
git add .github .gitmodules
git commit -m "Add SDLC workflow template system"
git push

# Update later
git submodule update --remote --merge .github
git add .github
git commit -m "Update SDLC workflow template system"
```

### Method 3: Selective Copy

```bash
# Just templates (minimal)
mkdir -p .github/ISSUE_TEMPLATE
cp path/to/.github/ISSUE_TEMPLATE/* .github/ISSUE_TEMPLATE/

# Commit
git add .github
git commit -m "Add issue and PR templates"
```

## Post-Integration Steps

### 1. Verify Templates Work

```bash
# Check issue templates
gh issue create
# Should show 3 templates: Epic, Story, Review

# Check PR template (optional: enable auto-fill first)
# For auto-fill: cp .github/ISSUE_TEMPLATE/00-pull-request-template.md .github/PULL_REQUEST_TEMPLATE.md

git checkout -b test/verify-templates
echo "test" > test.txt
git add test.txt
git commit -m "test: verify templates"
gh pr create
# If auto-fill enabled: Description should auto-fill with template
# If not: Manually copy template content
```

### 2. Customize for Your Project

Files to edit:

| File | What to Change |
|------|----------------|
| `copilot-instructions.md` | Your tech stack, patterns |
| `workflows/test.yml` | Your language, test commands |
| `ISSUE_TEMPLATE/*.md` | Project-specific labels (optional) |

### 3. Train Your Team

1. Share `workflow-system/guides/02-quick-start.md`
2. Walk through one Epic → Story → PR cycle
3. Print cheat sheet from `roles/00-index.md`

## Verification Checklist

- [ ] Issue templates appear (3 templates: Epic, Story, Review)
- [ ] PR template available (auto-fill optional: copy to `.github/PULL_REQUEST_TEMPLATE.md`)
- [ ] CI workflow runs on push
- [ ] Copilot aware of instructions (test in chat)
- [ ] Team understands templates

## Troubleshooting

### Templates not showing
- Ensure pushed to default branch (main/master)
- Wait 1-2 minutes (GitHub caches)
- Check file paths exactly match

### CI not running
- Check Settings → Actions → Allow all actions
- Verify YAML syntax valid

### Copilot not aware
- File must be exactly `.github/copilot-instructions.md`
- Reload VS Code window

---

**Time:** 15 minutes  
**Next:** [02-quick-start.md](02-quick-start.md)
