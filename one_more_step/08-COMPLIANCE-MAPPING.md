# Compliance Mapping
## SDLC Governance Control Plane — Compliance Mapping

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Architect + Product Owner  
**Status:** Approved  
**Audience:** Product / sales / diligence  
**Classification:** Confidential — Competitive Strategy  

---

## Strategic Context

> **Compliance is a core buying driver in regulated SDLC.**

Third-party analysts estimate the global GRC/eGRC market at roughly USD 49B to 63B in 2024 (definition-dependent). See [10-MARKET-VIABILITY.md](10-MARKET-VIABILITY.md) for sources and definitions. We make it automatic:

| Competitor Reality | Our Approach | Business Impact |
|--------------------|--------------|-----------------|
| Audit prep often takes weeks | Reduce to hours (target) | Major time savings |
| Manual evidence collection | Automatic from workflow | Reduced audit burden |
| Spreadsheet control mappings | Embedded in platform | Always current |
| Different tools per regulation | Modular compliance packs | One platform, all frameworks |
| Consultants explain controls | Self-service documentation | Reduced professional services |

**This document is our sales collateral for compliance-heavy sectors.** Every mapping shows:
1. **Requirement** — What the regulation demands
2. **Platform Control** — How we address it
3. **Evidence** — What auditors see
4. **Competitor Gap** — Why we win

---

## 1. Compliance Framework Overview

### 1.1 Target Regulatory Frameworks — The Addressable Market

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    COMPLIANCE TARGET MATRIX                                 │
│                    "GRC/eGRC Market (2024)"                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  SECTOR            REGULATION / STANDARD               OUR EDGE             │
│  ═══════════════════════════════════════════════════════════════════════   │
│                                                                             │
│  Healthcare        HIPAA                              Automatic PHI audit   │
│                    FDA 21 CFR Part 11                 Part 11 evidence gen  │
│                    SOC 2 Type II                      Control mapping       │
│                                                                             │
│  Financial         SOX                               One-click SOX report  │
│                    PCI-DSS                            Zero CHD exposure     │
│                    GDPR                               Privacy by design     │
│                    SOC 2 Type II                      Control inheritance   │
│                                                                             │
│  Defense           NIST 800-171                       CUI handling built-in │
│                    CMMC Level 2-3                     Pre-cert ready        │
│                    ITAR                               Geographic controls   │
│                                                                             │
│  Government        FedRAMP                            Authorization-ready   │
│                    FISMA                              Continuous ATO        │
│                    StateRAMP                          State compliance      │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.2 How Platform Addresses Compliance — The Control Framework

| Platform Feature | Compliance Need | Implementation | Competitor Gap |
|------------------|-----------------|----------------|----------------|
| **Enforced State Machine** | Authorized workflow | Label-based + Actions | Competitors advise; we enforce |
| **HMAC-Signed Audit Trail** | Immutable records | JSONL + cryptographic signature | Database logs can be edited |
| **CODEOWNER Merge** | Segregation of duties | Branch protection | Most allow any write access |
| **Artifact Linking** | Evidence chain | Idea→Epic→Story→PR→Release | End-to-end traceability focus |
| **Role-Based Gates** | Least privilege | 9 roles with approval flows | Typically 3-4 roles |
| **Security Gates** | Vulnerability mgmt | PenTester approval required | Usually optional scanning |
| **AI Agent Governance** | Emerging requirement | Same rules, logged actions | Governed AI usage with audit trail |

---

## 2. Healthcare Compliance

### 2.1 HIPAA (Health Insurance Portability and Accountability Act)

**Applicability:** Organizations handling Protected Health Information (PHI)

**Sales Positioning:** "Automatic HIPAA audit evidence from your workflow"

| HIPAA Requirement | Section | Platform Control | Evidence | Competitor Gap |
|-------------------|---------|------------------|----------|----------------|
| Access controls | §164.312(a)(1) | RBAC + CODEOWNERS | Team membership audit | Manual access reviews |
| Audit controls | §164.312(b) | HMAC-signed audit trail | `.audit/` logs | Database logs editable |
| Integrity controls | §164.312(c)(1) | Cryptographic signatures | Hash chain verification | Trust-based |
| Transmission security | §164.312(e)(1) | GitHub TLS + branch protection | HTTPS enforcement | Hope users use VPN |
| Unique user ID | §164.312(a)(2)(i) | GitHub accounts | Immutable actor in logs | Shared accounts common |
| Automatic logoff | §164.312(a)(2)(iii) | GitHub session timeout | Enterprise settings | Often disabled |
| Encryption | §164.312(a)(2)(iv) | GitHub at-rest encryption | GitHub SOC 2 report | Self-managed varies |

**Key Differentiator:** PHI never enters GitHub — we log who/what/when without content.

### 2.2 FDA 21 CFR Part 11 — Medical Device Software

**Applicability:** Electronic records and signatures for FDA-regulated products

**Sales Positioning:** "Pass Part 11 audits on first try"

| Part 11 Requirement | Section | Platform Control | Evidence | Competitor Gap |
|---------------------|---------|------------------|----------|----------------|
| Validated system | §11.10(a) | Validation protocol | `.compliance/validation.md` | Ad-hoc documentation |
| Accurate copies | §11.10(b) | Git history | `git log` immutable | Database backups |
| Record protection | §11.10(c) | Branch protection | CODEOWNER-only merge | Anyone can modify |
| Access limitation | §11.10(d) | 9-role RBAC | Quarterly permission audit | Coarse permissions |
| Audit trail | §11.10(e) | HMAC-signed JSONL | Timestamped, verified | Deletable logs |
| Sequence control | §11.10(f) | State machine | Transition log | Manual tracking |
| Authority checks | §11.10(g) | Role-based gates | Approval audit | Policy-only |
| Device checks | §11.10(h) | GitHub 2FA required | Org security settings | Optional MFA |
| Training | §11.10(i) | Training records | LMS integration | Tribal knowledge |
| Electronic signatures | §11.50 | Signed commits | GPG/SSH verification | Unsigned commits |
| Signature manifestations | §11.100 | Approver in logs | Username + timestamp | Just username |
| Signature linking | §11.200 | Commit signing | Signature chain | No linking |

**Key Differentiator:** We generate Part 11 validation evidence automatically.

---

## 3. Financial Services Compliance

### 3.1 SOX (Sarbanes-Oxley Act)

**Applicability:** Public companies, financial reporting systems

**Sales Positioning:** "Reduce SOX audit prep from weeks to hours (target)"

| SOX Requirement | Section | Platform Control | Evidence | Competitor Gap |
|-----------------|---------|------------------|----------|----------------|
| Internal controls | §302/404 | Enforced state machine | Workflow logs | Manual checklists |
| Segregation of duties | §404 | CODEOWNER ≠ author | RACI matrix, merge logs | Same person can approve |
| Change management | §404 | PR-only changes | Branch protection | Direct commits allowed |
| Audit trail | §404 | HMAC-signed logs | Immutable evidence | Editable databases |
| Access controls | §404 | 9-role RBAC | Permission audits | Broad access |
| Documentation | §302 | DoR/DoD checklists | Template enforcement | Inconsistent docs |

**SOX Control Matrix — Ready for Auditors:**

| Control ID | Control Description | Platform Implementation | Test Procedure | Pass Criteria |
|------------|---------------------|------------------------|----------------|---------------|
| SOX-CM-01 | Changes require approval | CODEOWNER merge required | Attempt merge without approval | Blocked |
| SOX-CM-02 | Changes traceable | Artifact linking enforced | Verify PR → Issue | 100% linked |
| SOX-CM-03 | Changes tested | DoD checklist + CI | Review PR status | All green |
| SOX-SD-01 | Developer ≠ approver | Branch protection | Check merge logs | Never same |
| SOX-AU-01 | All changes logged | Audit automation | Verify log completeness | 100% logged |

**Key Differentiator:** Pre-built SOX control mapping — auditors can verify in minutes.

### 1. System Description
GitHub-first SDLC platform (GitHub-native core) using Issues, PRs, Actions, and Projects
for workflow management with enforced state machine.

### 2. Validation Scope
- State machine enforcement (GitHub Actions)
- Audit trail integrity (HMAC verification)
- Access controls (CODEOWNERS enforcement)
- Electronic signatures (signed commits)

### 3. Acceptance Criteria
- [ ] All state transitions logged with timestamp and actor
- [ ] HMAC verification passes for all audit logs
- [ ] Unauthorized merge attempts blocked
- [ ] Signed commits verified for production branches

### 4. Evidence Requirements
- Audit log samples
- Branch protection rule screenshots
- CODEOWNERS file
- HMAC verification script output

---

## 3. Financial Services Compliance

### 3.1 SOX (Sarbanes-Oxley Act)

**Applicability:** Public companies, financial reporting systems

| SOX Requirement | Section | Platform Control | Evidence |
|-----------------|---------|------------------|----------|
| Internal controls | 302/404 | State machine, approvals | Workflow logs, approval records |
| Segregation of duties | 404 | Role-based restrictions | RACI matrix, CODEOWNERS |
| Change management | 404 | PR-only changes | Merge policy, branch protection |
| Audit trail | 404 | Immutable logs | `.audit/` with HMAC |
| Access controls | 404 | RBAC, least privilege | Team permissions |
| Documentation | 302 | DoR/DoD checklists | Issue templates |

**SOX Control Matrix:**

| Control ID | Control Description | Platform Implementation | Test Procedure |
|------------|---------------------|------------------------|----------------|
| SOX-CM-01 | Changes require approval | CODEOWNER merge requirement | Attempt merge without approval |
| SOX-CM-02 | Changes traceable to request | Artifact linking enforcement | Verify PR links to Issue |
| SOX-CM-03 | Changes tested before deploy | DoD checklist + CI | Review PR check status |
| SOX-SD-01 | Developer cannot approve own code | CODEOWNER ≠ PR author | Review approval logs |
| SOX-AU-01 | All changes logged | Audit trail automation | Verify log completeness |

### 3.2 PCI-DSS (Payment Card Industry Data Security Standard)

**Applicability:** Systems handling cardholder data

| PCI-DSS Requirement | Section | Platform Control | Evidence |
|---------------------|---------|------------------|----------|
| Install/maintain firewall | 1.x | GitHub network controls | GitHub Enterprise settings |
| Change default passwords | 2.x | Personal access tokens | Token rotation policy |
| Protect cardholder data | 3.x | No CHD in repos | Secret scanning |
| Encrypt transmission | 4.x | GitHub TLS | HTTPS enforcement |
| Anti-virus/malware | 5.x | GitHub dependency scanning | Dependabot alerts |
| Secure systems | 6.x | SDLC controls, code review | State machine, DoR/DoD |
| Restrict access | 7.x | RBAC, CODEOWNERS | Permission audit |
| Unique IDs | 8.x | GitHub user accounts | User audit |
| Restrict physical access | 9.x | GitHub infrastructure | SOC 2 report |
| Track/monitor access | 10.x | Audit logs | `.audit/` logs |
| Test security systems | 11.x | Security scanning | SAST/DAST results |
| Security policy | 12.x | Documented policy | `SECURITY.md` |

**Critical Note:**
- NEVER store PAN, CVV, or cardholder data in GitHub
- Use tokenized references only
- Secret scanning must be enabled and monitored

### 3.3 GDPR (General Data Protection Regulation)

**Applicability:** Processing EU personal data

| GDPR Article | Requirement | Platform Control | Evidence |
|--------------|-------------|------------------|----------|
| Art. 5(2) | Accountability | Documented processes | Workflow documentation |
| Art. 25 | Privacy by design | Security gates | PenTester approval gate |
| Art. 30 | Records of processing | Audit trail | `.audit/` logs |
| Art. 32 | Security of processing | Access controls, encryption | RBAC, GitHub encryption |
| Art. 33 | Breach notification | Incident workflow | Security issue template |
| Art. 35 | Impact assessment | DPIA in workflow | Required for `type:security` |

---

## 4. Defense Sector Compliance

### 4.1 NIST 800-171

**Applicability:** Controlled Unclassified Information (CUI)

| NIST Family | Platform Controls | Implementation |
|-------------|-------------------|----------------|
| 3.1 Access Control | RBAC, CODEOWNERS | GitHub teams, branch protection |
| 3.3 Audit | Audit trail | JSONL logs, HMAC signing |
| 3.4 Config Mgmt | Change control | PR-only, state machine |
| 3.5 Identification | GitHub accounts | User authentication |
| 3.6 Incident Response | Security workflow | Issue templates, escalation |
| 3.7 Maintenance | Version control | Git history, releases |
| 3.8 Media Protection | GitHub storage | Encryption at rest |
| 3.9 Personnel Security | Access review | Quarterly permission audit |
| 3.10 Physical Protection | GitHub infrastructure | SOC 2 compliance |
| 3.11 Risk Assessment | Security gates | SAST/DAST, PenTester review |
| 3.12 Security Assessment | Continuous validation | Automated checks |
| 3.13 SC Protection | TLS, encryption | GitHub infrastructure |
| 3.14 SI Integrity | HMAC verification | Hash chain validation |

**CUI Marking Requirements:**
```markdown
## CUI Handling in Platform

### Marking
- Issues handling CUI: Label `cui:controlled`
- Files with CUI: Header banner required
- Commits: Reference CUI policy

### Access
- CUI repositories: Private, restricted team
- CUI issues: Limited visibility (Enterprise)
- CUI branches: Additional protection rules

### Transmission
- Always over TLS (GitHub default)
- No CUI in public repos
- No CUI in commit messages (use references)
```

### 4.2 CMMC (Cybersecurity Maturity Model Certification)

**Applicability:** DoD contractors

| CMMC Level | Domain | Platform Control | Status |
|------------|--------|------------------|--------|
| Level 2 | Access Control (AC) | RBAC, CODEOWNERS | Implemented |
| Level 2 | Audit (AU) | Audit trail | Implemented |
| Level 2 | Config Mgmt (CM) | State machine, PR-only | Implemented |
| Level 2 | Identification (IA) | GitHub auth, 2FA | Implemented |
| Level 2 | Incident Response (IR) | Security workflow | Implemented |
| Level 2 | Media Protection (MP) | GitHub encryption | GitHub-managed |
| Level 2 | Risk Assessment (RA) | Security scanning | Implemented |
| Level 2 | Security Assessment (CA) | Automated validation | Implemented |
| Level 3 | All above + enhanced | Additional controls | Requires Enterprise |

### 4.3 ITAR (International Traffic in Arms Regulations)

**Applicability:** Defense articles and services

| ITAR Requirement | Platform Control | Implementation |
|------------------|------------------|----------------|
| Access restriction | Geographic restrictions | GitHub Enterprise IP allow list |
| US person only | Team membership audit | Citizenship verification (external) |
| Export control | Repository visibility | Private repos only |
| Technical data protection | Encryption, access control | Branch protection, CODEOWNERS |
| Record keeping | Audit trail | `.audit/` logs (7+ year retention) |

**Critical Warning:**
- ITAR-controlled data requires GitHub Enterprise with geographic restrictions
- Non-US person access is an ITAR violation
- Maintain separate access verification process

---

## 5. Government Compliance

### 5.1 FedRAMP (Federal Risk and Authorization Management Program)

**Applicability:** Cloud services for federal agencies

| FedRAMP Control | Platform Mapping | Evidence |
|-----------------|------------------|----------|
| AC (Access Control) | RBAC, CODEOWNERS | Permission audit |
| AU (Audit) | Audit trail | `.audit/` logs |
| CM (Config Mgmt) | State machine, GitOps | Workflow automation |
| CP (Contingency) | GitHub backup | GitHub SLA |
| IA (Identification) | GitHub auth | SSO integration |
| IR (Incident Response) | Security workflow | Response playbooks |
| SC (System/Comm) | TLS, encryption | GitHub infrastructure |
| SI (System Integrity) | HMAC verification | Hash chain |

**Note:** Full FedRAMP compliance requires GitHub Enterprise Cloud with FedRAMP authorization or GitHub Enterprise Server in authorized environment.

### 5.2 FISMA (Federal Information Security Modernization Act)

**Applicability:** Federal information systems

| FISMA Requirement | Platform Control | Implementation |
|-------------------|------------------|----------------|
| Risk categorization | System classification | Document in README |
| Security controls | NIST 800-53 mapping | Control inheritance |
| Continuous monitoring | Automated scanning | GitHub security features |
| POA&M | Issue tracking | Security issue labels |
| Authorization | Documented approval | Release approval gate |

---

## 6. Control Mapping Matrix

### 6.1 Cross-Framework Control Mapping

| Platform Control | HIPAA | Part 11 | SOX | PCI | NIST 171 | CMMC | FedRAMP |
|------------------|-------|---------|-----|-----|----------|------|---------|
| State machine | Yes | Yes | Yes | Yes | 3.4 | CM | CM |
| RBAC/CODEOWNERS | Yes | Yes | Yes | Yes | 3.1 | AC | AC |
| Audit trail | Yes | Yes | Yes | Yes | 3.3 | AU | AU |
| HMAC signing | Yes | Yes | Yes | - | 3.14 | SI | SI |
| PR-only merge | Yes | Yes | Yes | Yes | 3.4 | CM | CM |
| Security scanning | - | - | - | Yes | 3.11 | RA | RA |
| Signed commits | - | Yes | Yes | - | 3.5 | IA | IA |
| DoR/DoD checklists | - | Yes | Yes | Yes | 3.4 | CM | CM |
| Artifact linking | Yes | Yes | Yes | Yes | 3.3 | AU | AU |
| Release approval | Yes | Yes | Yes | Yes | 3.4 | CM | CM |

### 6.2 Gap Analysis

| Gap | Frameworks Affected | Remediation | Priority |
|-----|---------------------|-------------|----------|
| Signed commits not enforced | Part 11, SOX | Enable commit signing requirement | High |
| No geographic restriction | ITAR, CMMC L3 | Requires GitHub Enterprise | Medium |
| Training not tracked | All | External LMS integration | Medium |
| Retention policy not automated | HIPAA, ITAR | Implement log retention automation | Medium |
| DPIA not integrated | GDPR | Add DPIA template for security issues | Low |

---

## 7. Evidence Collection

### 7.1 Audit Evidence Artifacts

| Evidence Type | Location | Collection Method |
|---------------|----------|-------------------|
| State transitions | `.audit/transitions/` | Automated logging |
| Approvals | `.audit/approvals/` | Automated logging |
| PR reviews | GitHub PR history | GraphQL export |
| Permission changes | GitHub audit log | API export |
| Security scan results | `.audit/security/` | CI pipeline output |
| Release approvals | `.audit/releases/` | Automated logging |
| HMAC verifications | `.audit/verification/` | Nightly job output |

### 7.2 Evidence Retention

| Regulation | Retention Period | Implementation |
|------------|------------------|----------------|
| HIPAA | 6 years | Archive to cold storage |
| Part 11 | Duration of record | Git history + audit logs |
| SOX | 7 years | Archive to cold storage |
| PCI-DSS | 1 year online, 1 year archive | Log rotation + archive |
| ITAR | 5 years after export | Long-term archive |
| GDPR | Purpose-based | Data lifecycle automation |

### 7.3 Audit Preparation Checklist

```markdown
## Regulatory Audit Preparation

### Pre-Audit (2 weeks before)
- [ ] Export audit logs for audit period
- [ ] Verify HMAC signatures for all logs
- [ ] Generate compliance report (traceability, approvals)
- [ ] Collect permission audit evidence
- [ ] Prepare control mapping documentation

### During Audit
- [ ] Provide read-only access to audit repository
- [ ] Demo state machine enforcement (live)
- [ ] Show blocked unauthorized merge attempt
- [ ] Present audit log with signature verification
- [ ] Walk through artifact linking chain

### Post-Audit
- [ ] Address findings in tracked issues
- [ ] Update control implementation as needed
- [ ] Archive audit evidence package
- [ ] Schedule remediation follow-up
```

---

## 8. Compliance Automation

### 8.1 Compliance Check Workflow

```yaml
name: Compliance Validation
on:
  schedule:
    - cron: '0 6 * * 1'  # Weekly Monday 6 AM
  workflow_dispatch:

jobs:
  compliance-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Verify audit log integrity
        run: |
          # Verify HMAC signatures for all logs
          for log in .audit/**/*.jsonl; do
            ./scripts/verify-hmac.sh "$log"
          done
          
      - name: Check artifact linking
        run: |
          # Verify all PRs link to issues
          gh pr list --state merged --json number,body | \
            jq '.[] | select(.body | test("Closes #[0-9]+") | not) | .number'
          
      - name: Audit permission changes
        run: |
          # Export and review permission changes
          gh api /orgs/{org}/audit-log?phrase=action:team \
            --paginate > .audit/permissions/$(date +%Y%m%d).json
          
      - name: Generate compliance report
        run: |
          ./scripts/generate-compliance-report.sh
          
      - name: Upload report
        uses: actions/upload-artifact@v4
        with:
          name: compliance-report-${{ github.run_number }}
          path: .audit/reports/
```

### 8.2 Automated Evidence Collection

```yaml
name: Evidence Collection
on:
  release:
    types: [published]

jobs:
  collect-evidence:
    runs-on: ubuntu-latest
    steps:
      - name: Export release evidence
        run: |
          VERSION=${{ github.event.release.tag_name }}
          
          # Collect all linked issues
          gh api graphql -f query='
            query($tag: String!) {
              repository(owner: "{owner}", name: "{repo}") {
                release(tagName: $tag) {
                  releaseAssets(first: 10) { nodes { name } }
                }
              }
            }
          ' -f tag="$VERSION" > evidence/release-$VERSION.json
          
          # Export PR evidence
          gh pr list --state merged --search "label:release-$VERSION" \
            --json number,title,author,mergedAt,reviews \
            > evidence/prs-$VERSION.json
            
          # Export approval chain
          # (custom script to trace Idea → Epic → Story → PR → Release)
          ./scripts/trace-artifacts.sh $VERSION > evidence/traceability-$VERSION.json
          
      - name: Sign evidence package
        run: |
          # Create evidence bundle and sign
          tar -czf evidence-$VERSION.tar.gz evidence/
          openssl dgst -sha256 -sign private.pem \
            -out evidence-$VERSION.sig evidence-$VERSION.tar.gz
            
      - name: Archive evidence
        uses: actions/upload-artifact@v4
        with:
          name: compliance-evidence-${{ github.event.release.tag_name }}
          path: |
            evidence-*.tar.gz
            evidence-*.sig
          retention-days: 2555  # 7 years
```

---

## 9. Compliance Contacts

| Regulation | Internal Owner | External Contact |
|------------|----------------|------------------|
| HIPAA | Privacy Officer | HHS OCR |
| FDA Part 11 | Quality Assurance | FDA CDER/CBER |
| SOX | Internal Audit | External Auditor |
| PCI-DSS | Security Team | QSA |
| NIST/CMMC | Security Officer | DIBCAC |
| FedRAMP | System Owner | FedRAMP PMO |
| GDPR | DPO | Supervisory Authority |

---

**Previous Document:** [07-BOARDS-AND-TRACKING.md](07-BOARDS-AND-TRACKING.md)  
**Next Document:** [98-ACRONYM-TERM-GLOSSARY.md](98-ACRONYM-TERM-GLOSSARY.md)
