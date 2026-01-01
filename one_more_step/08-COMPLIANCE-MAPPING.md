# Compliance Mapping
## GitHub-Native SDLC Governance Platform

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Author:** Architect + Product Owner  
**Status:** Approved  

---

## 1. Compliance Framework Overview

### 1.1 Target Regulatory Frameworks

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                     COMPLIANCE TARGET MATRIX                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  SECTOR            REGULATION              FOCUS AREA                      │
│  ═══════════════════════════════════════════════════════════════════════   │
│                                                                             │
│  Healthcare        HIPAA                   PHI protection, audit trail      │
│                    FDA 21 CFR Part 11      Electronic records, signatures   │
│                    SOC 2 Type II           Security controls                │
│                                                                             │
│  Financial         SOX                     Financial controls, audit        │
│                    PCI-DSS                 Cardholder data protection       │
│                    GDPR                    Data privacy (EU)                │
│                    SOC 2 Type II           Security controls                │
│                                                                             │
│  Defense           NIST 800-171            CUI protection                   │
│                    CMMC Level 2-3          Cybersecurity maturity           │
│                    ITAR                    Export control                   │
│                                                                             │
│  Government        FedRAMP                 Cloud security                   │
│                    FISMA                   Federal IT security              │
│                    StateRAMP               State cloud security             │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 1.2 How Platform Addresses Compliance

| Platform Feature | Compliance Need | Implementation |
|------------------|-----------------|----------------|
| State machine | Authorized workflow | Label-based transitions with role approval |
| Audit trail | Immutable records | JSONL logs with HMAC signatures |
| RBAC | Least privilege | CODEOWNERS + branch protection |
| Traceability | Evidence chain | Artifact linking (Idea→Epic→Story→PR→Release) |
| Change control | Controlled changes | PR-only merges, CODEOWNER approval |
| Security gates | Vulnerability mgmt | Automated scanning + PenTester approval |
| Version control | Record management | Git history + semantic versioning |

---

## 2. Healthcare Compliance

### 2.1 HIPAA (Health Insurance Portability and Accountability Act)

**Applicability:** Organizations handling Protected Health Information (PHI)

| HIPAA Requirement | Section | Platform Control | Evidence |
|-------------------|---------|------------------|----------|
| Access controls | §164.312(a)(1) | GitHub RBAC, CODEOWNERS | Branch protection rules, team membership |
| Audit controls | §164.312(b) | Audit trail (JSONL + HMAC) | `.audit/` logs, nightly verification |
| Integrity controls | §164.312(c)(1) | HMAC signing, immutable logs | Hash chain verification |
| Transmission security | §164.312(e)(1) | GitHub TLS, branch protection | HTTPS-only access |
| Unique user ID | §164.312(a)(2)(i) | GitHub user accounts | Assignee/author fields in logs |
| Automatic logoff | §164.312(a)(2)(iii) | GitHub session timeout | GitHub Enterprise settings |
| Encryption | §164.312(a)(2)(iv) | GitHub at-rest encryption | GitHub infrastructure |

**Implementation Notes:**
- PHI should NEVER be stored in GitHub issues or PRs
- Reference PHI by external secure ID only
- Audit logs capture who/what/when without PHI content

### 2.2 FDA 21 CFR Part 11

**Applicability:** Electronic records and signatures for FDA-regulated products

| Part 11 Requirement | Section | Platform Control | Evidence |
|---------------------|---------|------------------|----------|
| Validated system | §11.10(a) | Documented validation | Validation protocol in `.compliance/` |
| Accurate copies | §11.10(b) | Git history, immutable | `git log`, signed commits |
| Record protection | §11.10(c) | Branch protection | CODEOWNER-only merge |
| Access limitation | §11.10(d) | RBAC, CODEOWNERS | Team permissions audit |
| Audit trail | §11.10(e) | JSONL + HMAC | `.audit/` with timestamps |
| Sequence control | §11.10(f) | State machine | Label transitions logged |
| Authority checks | §11.10(g) | Role-based approvals | Approval gates by role |
| Device checks | §11.10(h) | GitHub 2FA requirement | Org security settings |
| Training | §11.10(i) | Documented training | Training records |
| Electronic signatures | §11.50 | GitHub signed commits | GPG/SSH commit signing |
| Signature manifestations | §11.100 | Approver identity in logs | GitHub username, timestamp |
| Signature linking | §11.200 | Commit signing | Signed commits linked to identity |

**Required Procedures:**
```markdown
## Part 11 Validation Protocol

### 1. System Description
GitHub-native SDLC platform using Issues, PRs, Actions, and Projects
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
```

---

## 3. Financial Services Compliance

### 3.1 SOX (Sarbanes-Oxley Act)

**Applicability:** Public companies, financial reporting systems

| SOX Requirement | Section | Platform Control | Evidence |
|-----------------|---------|------------------|----------|
| Internal controls | §302/404 | State machine, approvals | Workflow logs, approval records |
| Segregation of duties | §404 | Role-based restrictions | RACI matrix, CODEOWNERS |
| Change management | §404 | PR-only changes | Merge policy, branch protection |
| Audit trail | §404 | Immutable logs | `.audit/` with HMAC |
| Access controls | §404 | RBAC, least privilege | Team permissions |
| Documentation | §302 | DoR/DoD checklists | Issue templates |

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
| Level 2 | Access Control (AC) | RBAC, CODEOWNERS | ✅ Implemented |
| Level 2 | Audit (AU) | Audit trail | ✅ Implemented |
| Level 2 | Config Mgmt (CM) | State machine, PR-only | ✅ Implemented |
| Level 2 | Identification (IA) | GitHub auth, 2FA | ✅ Implemented |
| Level 2 | Incident Response (IR) | Security workflow | ✅ Implemented |
| Level 2 | Media Protection (MP) | GitHub encryption | ✅ GitHub-managed |
| Level 2 | Risk Assessment (RA) | Security scanning | ✅ Implemented |
| Level 2 | Security Assessment (CA) | Automated validation | ✅ Implemented |
| Level 3 | All above + enhanced | Additional controls | ⚠️ Requires Enterprise |

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
| State machine | ✓ | ✓ | ✓ | ✓ | 3.4 | CM | CM |
| RBAC/CODEOWNERS | ✓ | ✓ | ✓ | ✓ | 3.1 | AC | AC |
| Audit trail | ✓ | ✓ | ✓ | ✓ | 3.3 | AU | AU |
| HMAC signing | ✓ | ✓ | ✓ | - | 3.14 | SI | SI |
| PR-only merge | ✓ | ✓ | ✓ | ✓ | 3.4 | CM | CM |
| Security scanning | - | - | - | ✓ | 3.11 | RA | RA |
| Signed commits | - | ✓ | ✓ | - | 3.5 | IA | IA |
| DoR/DoD checklists | - | ✓ | ✓ | ✓ | 3.4 | CM | CM |
| Artifact linking | ✓ | ✓ | ✓ | ✓ | 3.3 | AU | AU |
| Release approval | ✓ | ✓ | ✓ | ✓ | 3.4 | CM | CM |

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
**Next Document:** [09-GLOSSARY.md](09-GLOSSARY.md)
