# I'm Stuck

Something's not working. Find your issue below.

## 🔍 By Error Type

### Tests Not Running
- **Python:** `ModuleNotFoundError` → Install dependencies: `pip install -r requirements.txt`
- **Node.js:** `Cannot find module` → Install: `npm install`
- **General:** Check CI logs for exact error

### Branch/Git Issues
- **Branch name rejected:** Must follow `feature/<id>-<slug>` pattern (no spaces)
- **Can't push:** Check remote: `git remote -v` | Authentication: `git auth login`
- **Merge conflicts:** Pull latest `main`, resolve conflicts, commit

### PR Not Merging
- **CI failing:** Check [GitHub Actions](https://github.com) → see error details
- **Missing approvals:** Need Reviewer approval (see [Im_a_Reviewer.md](Im_a_Reviewer.md))
- **Unlinked issue:** PR must have `Closes #<id>` in description
- **Template incomplete:** Fill all sections (no "TODO" placeholders)

### Success Criteria Not Clear
- Read the Story again carefully
- Ask Tech Lead in the issue (comment with @mention)
- Check for "Non-Goals" (what's NOT included?)

### Reviewer Wants Changes
1. Read feedback carefully
2. Ask clarifying question if unclear
3. Make the change
4. Push to same branch (don't create new PR)
5. Re-request review

### Story Marked "Not Ready"
- Tech Lead needs clarification
- Check: Are criteria measurable? Are non-goals clear? Is test plan defined?
- Comment on Epic asking what's needed
- Don't start coding until "Spec Ready" label is present

---

## 🎯 Common Scenarios

### Scenario 1: "Definition of Done not satisfied"
**What:** Reviewer blocked your PR

**Checklist:**
- [ ] All tests passing (locally + CI)
- [ ] README updated with example
- [ ] No "TODO" in PR template
- [ ] Each success criterion mapped to test evidence
- [ ] No regressions (existing tests still pass)

**Fix:** Review [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md) and update PR accordingly.

### Scenario 2: "CI is failing"
**What:** Automated tests not passing

**Steps:**
1. Look at GitHub Actions output for specific error
2. Run locally: `pytest tests/` or `npm test`
3. Fix the issue
4. Commit and push: `git add . && git commit -m "fix: address test failure"`
5. CI re-runs automatically

### Scenario 3: "I don't understand the success criteria"
**What:** Story is too vague

**Steps:**
1. Read the Story again (entire body)
2. Check "Non-Goals" section (what's NOT included?)
3. Comment on the Story: `@tech_lead, Can you clarify criterion #2?`
4. **Do not start coding** until it's clear

### Scenario 4: "My PR is too big"
**What:** Reviewer says >300 lines or multiple stories in one PR

**Why:** Large PRs are hard to review and understand

**Fix:**
- Split into smaller PRs (1 story per PR)
- Each PR should be <300 lines
- Ask Tech Lead if story is too large to fit

### Scenario 5: "I found a bug in the feature I just implemented"
**What:** Tests pass but there's a real issue

**Steps:**
1. Create a new Story describing the bug
2. In the original PR, reference the new bug issue
3. Don't try to fix in same PR (that's scope creep)
4. Fix in a follow-up PR

---

## 📞 Escalation Path

**Still stuck after troubleshooting above?**

1. **Ask in PR comments** (tag relevant people)
2. **Check space docs** (search by keyword)
3. **Ask Tech Lead** (via issue comment or chat)
4. **Ask team** (sync meeting if urgent)

---

## 💡 Pro Tips

**Prevent common issues:**
- ✅ Read the Story fully before coding
- ✅ Write tests first (or alongside code)
- ✅ Run CI locally before pushing: `pytest`, `npm test`
- ✅ Fill PR template completely (no placeholders)
- ✅ Keep PRs small (1 story, <300 lines)
- ✅ Map success criteria to test evidence explicitly

**Faster reviews:**
- Test names clearly show what's being tested
- PR template filled with real content
- Each section of template addresses a reviewer question
- Evidence obvious (test names, file paths, CI status)

---

## 📚 Related Docs

- [Im_an_Implementer.md](Im_an_Implementer.md) — Step-by-step feature implementation
- [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md) — Quality checklist
- [../Rules/Definition_of_Ready.md](../Rules/Definition_of_Ready.md) — Story readiness
- [Im_a_Reviewer.md](Im_a_Reviewer.md) — Understanding review feedback

---

**Can't find your issue here?** Comment in the GitHub Issue or space and describe the problem. Someone will help! 🤝
