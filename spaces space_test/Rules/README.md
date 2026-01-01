# Rules: Quick Reference

Quick summaries of core governance rules. **Full versions in Git:** `workflow-system/rules/`

## State Machine

[State_Machine.md](State_Machine.md)

**Overview:** All work flows through 6 states. You cannot skip states.

**States:**
1. Intake → Epic created
2. Spec Ready → Stories created
3. In Progress → Implementation started
4. In Review → PR submitted
5. Done → PR merged
6. Released → Tagged & deployed

**Key rule:** Each state has entry/exit criteria. Automation blocks violations.

---

## Definition of Ready (DoR)

[Definition_of_Ready.md](Definition_of_Ready.md)

**Overview:** Before a Story becomes "In Progress", it must satisfy DoR.

**Checklist:**
- Success criteria are measurable & testable
- Non-goals explicitly stated
- Test plan documented
- Edge cases identified
- Owner assigned
- Tech Lead validated feasibility

**Key rule:** Can't start work without "Spec Ready" label (which verifies DoR).

---

## Definition of Done (DoD)

[Definition_of_Done.md](Definition_of_Done.md)

**Overview:** Before a PR is merged, it must satisfy DoD.

**Checklist:**
- All success criteria met (with test evidence)
- Tests passing (local + CI)
- Documentation updated
- PR template filled 100%
- Reviewer approved
- No regressions

**Key rule:** PR cannot merge without satisfying DoD.

---

## Artifact Linking

[Artifact_Linking.md](Artifact_Linking.md)

**Overview:** Complete traceability from idea → release. Links are ENFORCED.

**Requirements:**
- Epics have problem statements
- Stories linked to parent Epic (`Parent: #<id>`)
- PRs linked to Story (`Closes #<id>`)
- Branches follow naming (`feature/<id>-slug`)

**Key rule:** Unlinked artifacts cannot be created/merged. Automation enforces this.

---

## PR Hygiene

[PR_Hygiene.md](PR_Hygiene.md)

**Overview:** PRs must be small, focused, well-documented.

**Rules:**
- 1 story per PR
- <300 lines changed
- 1-3 commits
- No drive-by refactors
- No TODOs without issue references

**Key rule:** Large, unfocused PRs slow down review and increase risk.

---

## Branching Strategy

[Branching_Strategy.md](Branching_Strategy.md)

**Overview:** How branches are organized and managed.

**Pattern:** `<type>/<issue-id>-<slug>`
- `feature/42-login` (new features)
- `fix/99-null-pointer` (bug fixes)
- `chore/15-deps` (maintenance)
- `docs/8-readme` (documentation)

**Key rule:** Never commit directly to `main`. Always use PR from feature branch.

---

## Related Docs

**For implementation guidance:**
- [../Quick_Start/Im_an_Implementer.md](../Quick_Start/Im_an_Implementer.md) — Step-by-step feature development
- [../Quick_Start/Im_a_Tech_Lead.md](../Quick_Start/Im_a_Tech_Lead.md) — Creating Stories from Epics
- [../Quick_Start/Im_a_Reviewer.md](../Quick_Start/Im_a_Reviewer.md) — Reviewing PRs

**For templates:**
- [../Templates/](../Templates/) — All issue and PR templates

---

**Need more detail?** Full rules are in Git: `workflow-system/rules/<rule-name>.md`
