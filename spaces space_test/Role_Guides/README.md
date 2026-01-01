# Role Guides

Detailed guidance for each role in the workflow.

## Pick Your Role

- **[Implementer](Implementer_Guide.md)** — Building features (code)
- **[Tech_Lead](Tech_Lead_Guide.md)** — Validating requirements & creating stories
- **[Reviewer](Reviewer_Guide.md)** — Approving code & enforcing standards
- **[Release_Manager](Release_Manager_Guide.md)** — Deploying to production

---

## Role Responsibilities Overview

| Role | Owns | Key Activity | Time Investment |
|------|------|--------------|-----------------|
| **Implementer** | Code | Writing features & tests | Most of day |
| **Tech Lead** | Requirements | Validating & creating stories | ~1 day per epic |
| **Reviewer** | Quality | Approving PRs | 30 min per PR |
| **Release Manager** | Deployment | Tagging & releasing | 30 min per release |

---

## How Roles Work Together

```
1. Sponsor (PO) creates Epic issue
    ↓
2. Tech Lead validates & creates Stories (links to Epic)
    ↓
3. Implementer picks Story (must be "Spec Ready")
    ↓
4. Implementer opens PR (links to Story)
    ↓
5. Reviewer approves (verifies DoD)
    ↓
6. PR merged (automation)
    ↓
7. Release Manager deploys (tags & releases)
```

---

## Quick Reference

### Implementer Starting Work
→ [Implementer_Guide.md](Implementer_Guide.md) | Also see: [../Quick_Start/Im_an_Implementer.md](../Quick_Start/Im_an_Implementer.md)

### Tech Lead Validating Requirements
→ [Tech_Lead_Guide.md](Tech_Lead_Guide.md) | Also see: [../Quick_Start/Im_a_Tech_Lead.md](../Quick_Start/Im_a_Tech_Lead.md)

### Reviewer Approving Code
→ [Reviewer_Guide.md](Reviewer_Guide.md) | Also see: [../Quick_Start/Im_a_Reviewer.md](../Quick_Start/Im_a_Reviewer.md)

### Release Manager Deploying
→ [Release_Manager_Guide.md](Release_Manager_Guide.md) | Also see: [../Quick_Start/Im_Deploying.md](../Quick_Start/Im_Deploying.md)

---

## Role Matrix: Who Does What?

| Task | Sponsor/PO | Tech Lead | Implementer | Reviewer | Release |
|------|----------|-----------|-------------|----------|---------|
| Create Epic | ✅ | - | - | - | - |
| Validate requirements | - | ✅ | - | - | - |
| Create Stories | - | ✅ | - | - | - |
| Implement | - | - | ✅ | - | - |
| Write tests | - | - | ✅ | - | - |
| Review code | - | - | - | ✅ | - |
| Approve PR | - | - | - | ✅ | - |
| Merge | - | - | - | (auto) | - |
| Deploy | - | - | - | - | ✅ |
| Monitor | - | - | - | - | ✅ |

---

## Anti-Patterns by Role

### Implementer
- ❌ Starting work before "Spec Ready" label
- ❌ Implementing features not in success criteria
- ❌ Opening PR without mapped evidence
- ❌ Adding unrelated refactors

### Tech Lead
- ❌ Creating vague success criteria
- ❌ Marking "Spec Ready" without DoR validation
- ❌ Not collaborating with implementers on feasibility
- ❌ Creating stories too large (>4 days)

### Reviewer
- ❌ Approving without evidence
- ❌ Approving failing tests
- ❌ Approving incomplete documentation
- ❌ Approving without reading success criteria

### Release Manager
- ❌ Deploying without verified CI
- ❌ Deploying incomplete stories
- ❌ Skipping smoke tests
- ❌ No rollback plan

---

## Related Docs

- [../Quick_Start/](../Quick_Start/) — Quick entry points by role
- [../Rules/](../Rules/) — Governance rules (DoR, DoD, State Machine)
- [../Templates/](../Templates/) — Templates for each artifact type

---

**Find your role above and follow the guide!** 👆
