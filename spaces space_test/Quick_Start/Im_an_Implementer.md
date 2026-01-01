# I'm an Implementer

You're here to **build features** following a Story. This is the most common path.

## 📍 You Are Here
Quick_Start → I'm an Implementer

---

## Step-by-Step: Implement a Feature

### ✅ Step 1: Read the Story
**Time: 10 min** | **What:** Understand what to build

1. Open the Story (GitHub Issue)
2. Read: "Success Criteria" section (this is your spec)
3. Read: "Non-Goals" section (what NOT to do)
4. Read: "Test Plan" section
5. Check: Is it labeled "Spec Ready"? If not, ask Tech Lead.

**Reference:** [../Rules/Definition_of_Ready.md](../Rules/Definition_of_Ready.md)

**Checklist:**
- [ ] Success criteria are clear and measurable
- [ ] Non-goals are defined
- [ ] I understand what tests to write
- [ ] Story has "Spec Ready" label

---

### ✅ Step 2: Create Feature Branch
**Time: 5 min** | **What:** Set up Git branch following naming convention

```bash
# Pattern: feature/<issue-id>-<short-description>
git checkout -b feature/42-login-endpoint

# Then push
git push -u origin feature/42-login-endpoint
```

**Reference:** [../Rules/](../Rules/) → Branching strategy

**Checklist:**
- [ ] Branch name follows `feature/<id>-<slug>` pattern
- [ ] Pushed to origin

---

### ✅ Step 3: Write Tests First (or Alongside)
**Time: 20-30 min** | **What:** Create tests that prove success criteria

For each success criterion:
1. Write a test that validates it
2. Test should be named clearly: `test_health_returns_200`
3. Keep tests small (1 criterion per test)

**Example:**
```python
def test_health_endpoint_returns_200():
    """Success Criterion 1: GET /health returns 200"""
    response = client.get("/health")
    assert response.status_code == 200

def test_health_includes_timestamp():
    """Success Criterion 2: Response includes ISO8601 timestamp"""
    response = client.get("/health")
    assert "timestamp" in response.json()
    # Validate ISO8601 format
```

**Checklist:**
- [ ] Tests written for each success criterion
- [ ] Tests are FAILING (not passing yet)
- [ ] Test names match criteria

---

### ✅ Step 4: Implement the Feature
**Time: varies** | **What:** Write code to make tests pass

1. Write only what's in success criteria (no extra features)
2. Keep implementation focused & clean
3. Run tests locally: `pytest tests/` or `npm test`
4. Ensure tests PASS

**Checklist:**
- [ ] Code written
- [ ] Tests passing locally
- [ ] No unrelated refactors ("drive-by changes")

---

### ✅ Step 5: Update Documentation
**Time: 10 min** | **What:** Update README with examples

Add to README:
- Endpoint/function signature
- Example request/response (if API)
- How to use it
- Any configuration needed

**Checklist:**
- [ ] README updated with examples
- [ ] Code comments added where needed
- [ ] No TODOs without issue references

---

### ✅ Step 6: Open a Pull Request
**Time: 15 min** | **What:** Submit code for review

1. Commit your changes (1-3 commits max)
2. Open PR with title: `feat: <description> (#<issue-id>)`
3. Fill PR template completely (no placeholders)
4. Map each success criterion to test evidence

**Reference:** [../Templates/PR_Template.md](../Templates/PR_Template.md)

**PR Template Checklist:**
- [ ] Title: `feat(...) (#<issue-id>)`
- [ ] Body: Links to issue via `Closes #<id>`
- [ ] Mapping: Each criterion → test name
- [ ] Tests: All passing (CI green)
- [ ] Docs: README updated
- [ ] No TODOs or incomplete sections

---

### ✅ Step 7: Get Review
**Time: 1-2 days** | **What:** Wait for approval

1. Request review from designated Reviewer
2. Address any feedback
3. Reviewer approves → PR merged automatically

**Reference:** [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md)

---

## 🎯 Definition of Done Checklist

Before asking for review, confirm:
- [ ] All success criteria from Story met
- [ ] Tests written and passing (local + CI)
- [ ] Documentation updated (README, examples)
- [ ] PR template fully filled (no "TODO")
- [ ] Branch follows naming convention
- [ ] PR linked to exactly one issue
- [ ] No failing tests
- [ ] No regressions (existing tests still pass)

---

## ❌ Common Mistakes

| Mistake | Why Bad | Fix |
|---------|--------|-----|
| Implementing features NOT in success criteria | Scope creep | Read story again, stick to spec |
| Opening PR without tests | Unproven | Write tests first |
| Filling PR template with placeholders | Incomplete | Fill every section with real content |
| Drive-by refactors | Scope confusion | Keep PRs focused on 1 story |
| Merging failing tests | Broken main | Run CI locally first |
| Not mapping criteria to evidence | Can't verify | List test names explicitly |

---

## 🆘 Stuck?

- **Tests won't run:** [Im_Stuck.md](Im_Stuck.md) → Testing section
- **Branch name wrong:** [../Rules/](../Rules/) → Branching strategy
- **Don't understand criteria:** Comment on Story, ask Tech Lead
- **PR feedback:** Address feedback & push updates (don't create new PR)

---

## ⏱️ Typical Timeline

- Steps 1-2: 15 min
- Steps 3-4: 1-4 hours (depends on complexity)
- Steps 5-6: 20 min
- Step 7: 1-2 days (waiting for review)

**Total: 1-5 days per story**

---

## 📚 Related Docs

**Before starting:**
- [../Rules/Definition_of_Ready.md](../Rules/Definition_of_Ready.md) — Verify story is ready
- [../Rules/State_Machine.md](../Rules/State_Machine.md) — Understand workflow states

**During implementation:**
- [../Templates/PR_Template.md](../Templates/PR_Template.md) — PR format
- [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md) — Quality checklist

**Getting unstuck:**
- [Im_Stuck.md](Im_Stuck.md) — Troubleshooting

---

**Ready?** Open your Story and follow Step 1 above. You've got this! 🚀
