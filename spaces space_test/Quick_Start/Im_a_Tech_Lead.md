# I'm a Tech Lead

You're here to **validate requirements** and **create Stories** from Epics. This ensures implementers have clear, testable specs.

## 📍 You Are Here
Quick_Start → I'm a Tech Lead

---

## Your Responsibilities

1. **Challenge unclear requirements** (with PO)
2. **Validate feasibility** (with implementers)
3. **Create Stories** with architecture notes
4. **Mark "Spec Ready"** when DoR satisfied

---

## Step-by-Step: From Epic to Ready Stories

### ✅ Step 1: Read the Epic
**Time: 15 min** | **What:** Understand the business need

1. Open the Epic (GitHub Issue)
2. Read: "Problem Statement" (why we're doing this)
3. Read: "Success Criteria" (what success looks like)
4. Read: "Non-Goals" (scope boundaries)

**Checklist:**
- [ ] Problem is clear
- [ ] Success criteria are measurable
- [ ] Scope is bounded (not too large)

---

### ✅ Step 2: Challenge & Clarify (If Needed)
**Time: 10-30 min** | **What:** Ask hard questions

If anything is unclear, comment on the Epic:
- "Is this API-only or does UI need to change?"
- "Do we need backward compatibility?"
- "What's the performance target?"
- "What edge cases matter?"

**Collaboration approach:**
- Ask PO in comments on Epic
- Tag relevant stakeholders
- Document clarifications in Epic
- Update Epic body with agreed-upon answers

**Checklist:**
- [ ] All questions answered by PO
- [ ] Assumptions documented in Epic
- [ ] Scope locked (no moving targets)

---

### ✅ Step 3: Validate Feasibility
**Time: 20 min** | **What:** Confirm implementers can build this

1. Talk to implementers: "Can we do this? How long?"
2. Check: Do we have dependencies (APIs, libraries)?
3. Check: Are there technical risks?
4. Document blockers or risks in Epic

**Checklist:**
- [ ] Implementation team confirms feasibility
- [ ] Dependencies identified & available
- [ ] Technical risks documented
- [ ] Rough effort estimate (if needed)

---

### ✅ Step 4: Create Stories from Epic
**Time: 30 min - 1 hour** | **What:** Break Epic into independently testable Stories

**Rules for Story breakdown:**
- Each Story = 1 testable unit of work
- Story should take 1-4 days to implement
- Stories can be worked independently (no hard dependencies)
- Include API contracts & architecture notes

**Example Story structure:**
```markdown
# Story: Implement GET /health endpoint

**Parent Epic:** #10 (Health monitoring system)

## Success Criteria
1. GET /health returns 200 status code
2. Response includes timestamp in ISO8601 format
3. Response includes system status (ok/warning/error)

## Non-Goals
- No authentication required
- No database queries
- Not a health check for external services

## Test Plan
- Unit: Response shape & status codes
- Integration: None needed
- Manual: Test in local environment

## Architecture Note
GET /health → 200 {status:ok, timestamp:ISO8601, version:string}
```

**Reference:** [../Templates/Story_Template.md](../Templates/Story_Template.md)

**Checklist:**
- [ ] Each Story has measurable success criteria
- [ ] Each Story is independently testable
- [ ] Stories don't have hard dependencies (sequence OK)
- [ ] Architecture notes included (API contract, test strategy)
- [ ] Non-goals explicit (scope bounded)

---

### ✅ Step 5: Add "Spec Ready" Label
**Time: 5 min** | **What:** Mark Stories as ready for implementation

Only mark "Spec Ready" when:
- [ ] Success criteria are specific & testable (not vague)
- [ ] Non-goals are stated
- [ ] Test plan documented
- [ ] Edge cases identified
- [ ] Tech Lead validated feasibility
- [ ] DoR checklist satisfied

**Reference:** [../Rules/Definition_of_Ready.md](../Rules/Definition_of_Ready.md)

---

## 🎯 Definition of Ready Checklist

Before Stories go to implementers, confirm:

- [ ] **Success Criteria:** Measurable, testable, specific (not "should work")
- [ ] **Non-Goals:** Clear scope boundaries
- [ ] **Test Plan:** At least unit test strategy defined
- [ ] **Edge Cases:** Known edge cases identified
- [ ] **Owner Assigned:** Someone owns the story
- [ ] **Dependencies:** External dependencies identified
- [ ] **Acceptance:** PO confirms understanding
- [ ] **Feasibility:** Tech confirms it's doable

---

## ❌ Common Mistakes

| Mistake | Why Bad | Fix |
|---------|--------|-----|
| Vague criteria ("Add auth") | Not testable | Break down: "Login endpoint", "Password validation", "JWT token" |
| No architecture notes | Implementer guesses | Include API contracts, status codes, edge cases |
| Stories too large (>4 days) | Can't test independently | Split into smaller, sequential stories |
| Missing non-goals | Scope creep | Explicitly state what's NOT included |
| Marking ready too early | Implementers blocked | Verify DoR checklist 100% complete |
| Not collaborating with implementers | Infeasible specs | Talk to team about effort & risks first |

---

## 🆘 Stuck?

- **Don't know how to split Epic:** [../Runbooks/Create_Stories.md](../Runbooks/Create_Stories.md)
- **Success criteria not clear:** Ask PO to clarify in Epic comments
- **Feasibility uncertain:** Schedule sync with implementers
- **Technical risks high:** Document in Epic, flag for team discussion

---

## 📚 Related Docs

**Before starting:**
- [../Rules/Definition_of_Ready.md](../Rules/Definition_of_Ready.md) — DoR criteria
- [../Rules/State_Machine.md](../Rules/State_Machine.md) — Workflow overview

**Creating Stories:**
- [../Templates/Story_Template.md](../Templates/Story_Template.md) — Story template
- [../Rules/Artifact_Linking.md](../Rules/Artifact_Linking.md) — How to link stories to epics

**After Stories ready:**
- Implementer follows: [Im_an_Implementer.md](Im_an_Implementer.md)

---

**Key Principle:** A well-written Story makes implementation easy. A vague Story wastes everyone's time.

Take time to get this right. ✅
