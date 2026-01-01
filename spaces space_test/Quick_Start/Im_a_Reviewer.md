# I'm a Reviewer

You're the **quality gate** between implementation and release. Your job is to enforce standards and ensure code meets success criteria.

## 📍 You Are Here
Quick_Start → I'm a Reviewer

---

## Your Responsibilities

1. **Validate success criteria** are met (with evidence)
2. **Check code quality** (no regressions, tests passing)
3. **Verify documentation** (README, comments updated)
4. **Block incomplete work** (enforce Definition of Done)

---

## Step-by-Step: Review a PR

### ✅ Step 1: Pre-Review Checklist
**Time: 5 min** | **What:** Is the PR ready for review?

Check:
- [ ] PR linked to exactly one issue (via `Closes #<id>`)
- [ ] PR template filled completely (no placeholders)
- [ ] CI is green (all automated checks passed)
- [ ] No merge conflicts

If any fail → Request changes before reviewing code.

---

### ✅ Step 2: Read the Story
**Time: 10 min** | **What:** Understand what success looks like

1. Open the linked Story (GitHub Issue)
2. Read: "Success Criteria" section
3. Read: "Non-Goals" section
4. Keep this open while reviewing

**Why:** You're checking if PR *proves* each criterion.

---

### ✅ Step 3: Check Evidence Mapping
**Time: 10 min** | **What:** Are success criteria proven?

In the PR, look for "Mapping to Success Criteria" section:

```markdown
## Mapping to Success Criteria

- [ ] Criterion 1: GET /health returns 200
  - Evidence: test_health_returns_200 (tests/health.py:45-50) PASSING

- [ ] Criterion 2: Response includes timestamp
  - Evidence: test_health_timestamp (tests/health.py:52-58) PASSING
```

**Check:**
- [ ] Each criterion has a test name
- [ ] Test file + line numbers provided
- [ ] All tests listed are PASSING in CI
- [ ] Tests actually prove the criterion (not just named loosely)

If evidence is missing or unclear → Request changes.

---

### ✅ Step 4: Review Code Quality
**Time: 15-30 min** | **What:** Is the code production-ready?

Check:
- [ ] Code is clean & understandable (no "why?" comments needed)
- [ ] No TODO comments without issue references
- [ ] No unrelated refactors (scope is focused)
- [ ] No breaking existing tests (run `pytest tests/` or equivalent locally)

**Pay attention to:**
- Error handling (graceful failures)
- Edge cases (what if input is null/empty/huge?)
- Security (no hardcoded secrets, SQL injection risks, etc.)
- Performance (not obviously inefficient)

If issues found → Request specific, actionable changes.

**Good feedback:**
```
Line 45: This condition checks for null, but what if the input is an 
empty string? Consider: if not value.strip(): ...

Alternatively, add a test case: test_health_with_empty_string()
```

**Bad feedback:**
```
This doesn't look right
```

---

### ✅ Step 5: Verify Documentation
**Time: 5 min** | **What:** Is README updated?

Check:
- [ ] README updated with new endpoint/feature (if applicable)
- [ ] Examples included (how to use it?)
- [ ] API docs updated (parameters, response format)
- [ ] Breaking changes noted (if any)

Missing docs → Request changes.

---

### ✅ Step 6: Check Definition of Done
**Time: 5 min** | **What:** Does this meet our quality bar?

Verify:
- [ ] All acceptance criteria met (from step 3)
- [ ] Tests added AND passing
- [ ] Documentation updated
- [ ] PR template fully filled
- [ ] No regressions (existing tests still pass)
- [ ] Code review feedback addressed

**Reference:** [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md)

---

### ✅ Step 7: Approve or Request Changes

**Approve if:**
- All criteria above are satisfied
- No outstanding questions
- Ready to merge

**Request changes if:**
- Evidence incomplete or unclear
- Tests failing
- Code quality issues
- Documentation missing
- Success criteria not actually met

**Don't approve incomplete work.** It delays the whole team.

---

## 🎯 Review Checklist

Use this every time:

### Pre-Review
- [ ] PR linked to issue (Closes #X)
- [ ] PR template filled
- [ ] CI green
- [ ] No conflicts

### Evidence & Testing
- [ ] Each success criterion mapped to test(s)
- [ ] All mapped tests PASSING
- [ ] Tests actually prove criteria (not just named well)
- [ ] No failing tests in CI

### Code Quality
- [ ] Code is clean & maintainable
- [ ] No unrelated refactors
- [ ] Error handling present
- [ ] Edge cases considered
- [ ] No TODOs without issue references
- [ ] Existing tests still pass (no regressions)

### Documentation
- [ ] README updated (if user-facing)
- [ ] Examples included (if applicable)
- [ ] Code comments explain "why" not "what"
- [ ] API docs updated (if applicable)

### Definition of Done
- [ ] All above items satisfied
- [ ] PR template 100% complete
- [ ] Ready to merge

---

## ❌ Common Mistakes

| Mistake | Why Bad | Fix |
|---------|--------|-----|
| Approving without evidence | Can't verify criteria met | Request test names & passing proof |
| Approving failing tests | Broken main branch | CI must be 100% green before approval |
| Approving incomplete docs | User confusion | Check README has examples |
| Overlooking scope creep | Unplanned features | Flag if PR does more than story requests |
| Being vague in feedback | Implementer confused | Name specific line, suggest fix, explain why |
| Not blocking incomplete PRs | Quality erosion | Enforce Definition of Done—don't compromise |

---

## 📋 Review Comment Template

When requesting changes:

```markdown
**Issue:** [Describe the problem]

**Why it matters:** [Explain impact]

**Suggested fix:** [Concrete suggestion]

**Reference:** [Link to relevant rule/template]

**Examples:**
- ✅ Good: "Test test_health_timestamp validates ISO8601 format"
- ❌ Bad: "Timestamp is missing format check"
```

---

## 🆘 Questions to Ask

- "How does this prove criterion #X?"
- "What if input is null/empty/huge?"
- "Why did you choose this approach over Y?"
- "Are there edge cases we're missing?"
- "Is this backward compatible?"
- "Do we need migration code?"

---

## 📚 Related Docs

**Before reviewing:**
- [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md) — Quality standards
- [../Templates/Review_Checklist.md](../Templates/Review_Checklist.md) — Detailed checklist

**During review:**
- [../Rules/PR_Hygiene.md](../Rules/PR_Hygiene.md) — PR standards
- [../Rules/State_Machine.md](../Rules/State_Machine.md) — Workflow context

**Giving feedback:**
- Be specific (file/line references)
- Be actionable (suggest a fix)
- Be kind (code review is collaboration)

---

**Remember:** You're the quality gate. Don't approve incomplete work—it's a disservice to everyone downstream. 🛡️
