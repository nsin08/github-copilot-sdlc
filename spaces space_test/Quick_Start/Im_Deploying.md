# I'm Deploying

You're releasing code to production. This must be done carefully.

## 📍 You Are Here
Quick_Start → I'm Deploying

---

## Your Responsibilities

1. **Verify all stories are "Done"** (merged, tested)
2. **Create release tag** with version
3. **Generate release notes**
4. **Deploy to production**
5. **Monitor for issues**

---

## Step-by-Step: Deploy to Production

### ✅ Step 1: Verify Readiness
**Time: 10 min** | **What:** Are we ready to release?

Checklist:
- [ ] All stories for this release are merged (no pending PRs)
- [ ] CI is green on `main` branch
- [ ] No known regressions or critical bugs
- [ ] Release notes are prepared
- [ ] Stakeholders are notified

If any fail → **Do not deploy**. Investigate first.

---

### ✅ Step 2: Create Release Tag
**Time: 5 min** | **What:** Mark the release point in Git

```bash
# Determine version (semantic versioning: MAJOR.MINOR.PATCH)
# Examples: v1.0.0, v1.1.0, v1.1.1

git tag -a v1.1.0 -m "Release v1.1.0: Feature X, Bug fix Y"
git push origin v1.1.0
```

**Reference:** [../Rules/](../Rules/) → Versioning

**Checklist:**
- [ ] Version number follows semantic versioning
- [ ] Tag message describes major changes
- [ ] Tag is pushed to origin

---

### ✅ Step 3: Generate Release Notes
**Time: 10 min** | **What:** Document what changed

Release notes should include:
- Features added (link to stories)
- Bug fixes (link to issues)
- Breaking changes (if any)
- Migration guide (if needed)
- Contributors (optional)

**Template:**
```markdown
# Release v1.1.0

## ✨ New Features
- Feature #42: Login endpoint (#15)
- Feature #43: User profile page (#20)

## 🐛 Bug Fixes
- Fixed null pointer in health endpoint (#25)

## 🚨 Breaking Changes
- Authentication now required for all endpoints
- See migration guide below

## Migration Guide
[steps to upgrade]

## Contributors
- @implementer1
- @implementer2
```

---

### ✅ Step 4: Deploy to Production
**Time: varies** | **What:** Push code to production environment

Deployment process (specific to your infrastructure):
- [ ] Merge to `main` (if not already)
- [ ] Run deployment command: [your command here]
- [ ] Verify deployment completed
- [ ] Smoke test (quick manual check)

**Checklist:**
- [ ] Deployment logs show success
- [ ] No errors during startup
- [ ] Basic functionality works (health check, key features)

---

### ✅ Step 5: Monitor for Issues
**Time: 30 min - ongoing** | **What:** Watch for problems

Monitor:
- [ ] Error rates (logs, monitoring dashboard)
- [ ] Performance (response times, throughput)
- [ ] User reports (support channels, monitoring tools)

**First 30 minutes:** Stay alert, ready to rollback if critical issues.

---

## 🚨 Rollback Procedure (If Something Goes Wrong)

**If critical issue detected:**

```bash
# Identify the previous stable version
git tag -l | sort -V | tail -5

# Rollback to previous version
git checkout v1.0.0
git push origin main  # (or however you deploy)

# Document the incident
# Create issue with rollback reason
```

**After rollback:**
1. Investigate root cause
2. Create fix PR
3. Test thoroughly
4. Re-deploy

---

## 📋 Release Checklist

Before deploying:
- [ ] All stories merged
- [ ] CI green on main
- [ ] Release tag created
- [ ] Release notes generated
- [ ] Stakeholders notified

During deployment:
- [ ] Deployment command runs successfully
- [ ] Smoke tests pass
- [ ] No critical errors in logs

After deployment:
- [ ] Monitor for 30 minutes
- [ ] Document in release notes
- [ ] Close related stories/issues
- [ ] Celebrate! 🎉

---

## ❌ Common Mistakes

| Mistake | Why Bad | Fix |
|---------|--------|-----|
| Deploying failing tests | Broken production | Verify CI green first |
| No release notes | Unclear what changed | Document features & fixes |
| Skipping smoke tests | Silent failures | Test key functionality after deploy |
| No rollback plan | Can't recover from disasters | Know how to rollback before deploying |
| Deploying unreviewed code | Untested code in production | Ensure all PRs approved & merged |

---

## 📚 Related Docs

**Before deploying:**
- [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md) — Verify work is complete
- [../Rules/State_Machine.md](../Rules/State_Machine.md) — Understand workflow states

**During deployment:**
- [../Runbooks/Deployment_Runbook.md](../Runbooks/Deployment_Runbook.md) — Detailed steps

---

**Key Principle:** Slow down before deploying. A broken deploy is far worse than a delayed one.

Take your time, verify everything, monitor after. ✅
