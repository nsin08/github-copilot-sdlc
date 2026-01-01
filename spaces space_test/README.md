# space_test: Agent Context Hub

Welcome! This Copilot Space is your **navigation hub** for all governance rules, templates, role guides, and runbooks.

---

## 🎯 Where Do You Want to Go?

### 🚀 **Getting Started**
New to this project? Start here.
- [Quick_Start/](Quick_Start/) — Step-by-step guides by role
- [Quick_Start/Im_an_Implementer.md](Quick_Start/Im_an_Implementer.md) — Build features
- [Quick_Start/Im_a_Tech_Lead.md](Quick_Start/Im_a_Tech_Lead.md) — Validate requirements
- [Quick_Start/Im_a_Reviewer.md](Quick_Start/Im_a_Reviewer.md) — Approve code

### 📋 **Templates**
Need a template? Find them here.
- [Templates/](Templates/) — All templates (Epic, Story, PR, Review)
- Link to Git source: `.github/ISSUE_TEMPLATE/`

### 👥 **Role Guides**
Detailed guidance for your role.
- [Role_Guides/](Role_Guides/) — Pick your role
- Step-by-step checklists
- Common issues & solutions

### 📖 **Rules & Governance**
Understand the system.
- [Rules/](Rules/) — Quick reference summaries
- [Rules/State_Machine.md](Rules/State_Machine.md) — Workflow states & transitions
- [Rules/Definition_of_Ready.md](Rules/Definition_of_Ready.md) — Entry criteria
- [Rules/Definition_of_Done.md](Rules/Definition_of_Done.md) — Exit criteria
- Full versions in Git: `workflow-system/rules/`

### 🏃 **Runbooks & How-Tos**
Task-focused guides (numbered steps).
- [Runbooks/](Runbooks/) — Pick your task
- Feature Development (steps 1-5)
- Code Review
- Deployment

### 🔍 **Stuck?**
Something not working?
- [Quick_Start/Im_Stuck.md](Quick_Start/Im_Stuck.md) — Troubleshooting guide
- Search by error/keyword
- Escalation procedures

### 📚 **Reference**
Historical decisions & context.
- [Reference/](Reference/) — Architecture decisions, process evolution
- Why things work the way they do

---

## 🔑 Quick Navigation by Task

| I want to... | Go to... |
|---|---|
| **Implement a feature** | [Quick_Start/Im_an_Implementer.md](Quick_Start/Im_an_Implementer.md) |
| **Create a Story** | [Templates/Story_Template.md](Templates/Story_Template.md) + [Rules/Definition_of_Ready.md](Rules/Definition_of_Ready.md) |
| **Open a PR** | [Templates/PR_Template.md](Templates/PR_Template.md) + [Rules/Definition_of_Done.md](Rules/Definition_of_Done.md) |
| **Review code** | [Quick_Start/Im_a_Reviewer.md](Quick_Start/Im_a_Reviewer.md) |
| **Deploy** | [Runbooks/Deployment_Runbook.md](Runbooks/Deployment_Runbook.md) |
| **Understand the state machine** | [Rules/State_Machine.md](Rules/State_Machine.md) |
| **Find a template** | [Templates/](Templates/) |
| **Debug an issue** | [Quick_Start/Im_Stuck.md](Quick_Start/Im_Stuck.md) |

---

## 📍 How This Space Works

**Three-layer structure:**

1. **Entry Points** (Quick_Start/) — "I am X, what do I do?"
2. **References** (Templates/, Rules/, Role_Guides/) — Detailed info by topic
3. **Procedures** (Runbooks/) — Step-by-step tasks
4. **Context** (Reference/) — Why & how decisions were made

**For Agents:**
- Load space at start of task
- Follow link chains (breadcrumbs at bottom of each doc)
- Update space when you discover gaps
- Search by keyword if lost

---

## 📖 File Naming Convention

All files use underscores (no spaces):
- `Im_an_Implementer.md` ✅
- `I'm an Implementer.md` ❌
- `State_Machine.md` ✅
- `State Machine.md` ❌

This ensures agents can reliably reference files.

---

## 🔗 Linked Resources

- **Git Source Truth:** `workflow-system/` directory
- **Governance Rules:** `.github/copilot-instructions.md`
- **Issue Templates:** `.github/ISSUE_TEMPLATE/`

This space **summarizes and indexes** these resources for agent navigation.

---

## 📝 Last Updated
January 2026 | Framework v1.0.0

---

**Questions?** Check [Quick_Start/Im_Stuck.md](Quick_Start/Im_Stuck.md) or refer to the Git source docs.
