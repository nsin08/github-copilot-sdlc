# Templates

All issue and PR templates used in this project.

## Available Templates

| Template | Purpose | Use When |
|----------|---------|----------|
| [Epic_Template.md](Epic_Template.md) | Define large features | Starting a major initiative |
| [Story_Template.md](Story_Template.md) | Implementation tasks | Breaking down an Epic |
| [PR_Template.md](PR_Template.md) | Code submission | Opening a pull request |
| [Review_Checklist.md](Review_Checklist.md) | QA validation | Reviewing a PR |

---

## How to Use Templates

### Creating an Epic
1. Go to GitHub Issues → New Issue
2. Copy content from [Epic_Template.md](Epic_Template.md)
3. Fill in all sections (no placeholders)
4. Post issue
5. Share with Tech Lead for validation

### Creating a Story
1. Tech Lead (or PO) creates from an Epic
2. Use [Story_Template.md](Story_Template.md)
3. Link to parent Epic via `Parent: #<epic-id>`
4. Get "Spec Ready" approval from Tech Lead
5. Assign to Implementer

### Submitting Code (PR)
1. After implementing a Story
2. Use [PR_Template.md](PR_Template.md)
3. Fill template completely (no TODOs)
4. Map each success criterion to test evidence
5. Request review

### Reviewing Code
1. Use [Review_Checklist.md](Review_Checklist.md)
2. Verify each criterion is met
3. Approve or request changes
4. Don't approve incomplete work

---

## Template Standards

All templates:
- ✅ Include all required sections
- ✅ Have clear instructions (in bold)
- ✅ Use checkbox format for checklists
- ✅ Link to related rules/guides
- ✅ Enforce quality standards

**Never leave placeholders or "TODO" sections.** Template sections are REQUIRED to complete.

---

## Customization

These templates are tailored to this framework. Each adopting project can customize:
- Tech stack commands
- Repository structure
- Testing frameworks
- Deployment processes

**Core sections must remain** (Success Criteria, Non-Goals, Evidence, Definition of Done).

---

## Related Docs

- [../Rules/Definition_of_Ready.md](../Rules/Definition_of_Ready.md) — What makes a Story "ready"
- [../Rules/Definition_of_Done.md](../Rules/Definition_of_Done.md) — What makes a PR "done"
- [../Quick_Start/](../Quick_Start/) — Role-based guides (where to use these templates)

---

**Questions about templates?** See [../Quick_Start/Im_Stuck.md](../Quick_Start/Im_Stuck.md)
