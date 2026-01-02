# Compliance Catalog (Software Development)

**Document Version:** 1.0.0  
**Date:** 2026-01-02  
**Status:** Draft (source-linked)  
**Audience:** Product / sales / diligence  

---

## 1. Notes (read before using)

- This is not legal advice. It is a practical catalog of common compliance drivers that impact software development.
- **Ease (SDLC evidence)** means: "how much of the required evidence can be produced directly from SDLC workflow controls" (e.g., PR approvals, change traceability, CI logs, signed releases, audit logs) in a GitHub-first workflow.
- Many items below still require **non-SDLC controls** (vendor risk, HR/training, security ops, physical security, legal review, external audits). "Easy" does not mean "instant certification".

---

## 2. High-leverage starters (easier to demonstrate with SDLC evidence)

These are typically the fastest to demo credibly because the evidence is mostly SDLC-native:

- **OWASP Top 10** (secure coding baseline)
- **NIST SSDF (SP 800-218)** (secure development practices)
- **SLSA + SBOM (SPDX)** (software supply chain / provenance)
- **CIS Controls (v8)** (baseline security program controls)
- **SOC 2 readiness** (change management, access control, audit trails)
- **ISO/IEC 27001 readiness** (ISMS + control evidence; certification later)

---

## 3. Catalog (50 common regimes that affect the SDLC)

| # | Compliance / standard | Typical scope | SDLC evidence you can produce (examples) | Ease (SDLC evidence) | Primary source |
|---:|---|---|---|---|---|
| 1 | SOC 2 (Trust Services Criteria) | SaaS / service organizations | Change control, approvals, access logs, incident evidence, retention | Medium | https://www.aicpa-cima.com/resources/landing/system-and-organization-controls-soc-suite-of-services |
| 2 | ISO/IEC 27001 | ISMS certification (security management) | Policies + control evidence, access controls, change management, audit logs | Medium | https://www.iso.org/standard/27001 |
| 3 | ISO/IEC 27002 | Security controls guidance (not a certification by itself) | Control mappings, secure SDLC evidence, logging/monitoring evidence | Medium | https://www.iso.org/standard/75652.html |
| 4 | ISO/IEC 27017 | Cloud security controls (extension to 27002) | Cloud/shared-responsibility evidence, change control, access logs | Medium | https://www.iso.org/standard/43757.html |
| 5 | ISO/IEC 27018 | PII protection in public cloud | Privacy controls evidence, data handling procedures, access logs | Medium | https://www.iso.org/standard/76559.html |
| 6 | ISO/IEC 27701 | Privacy information management (PIMS) | Privacy-by-design evidence, DPIA artifacts, access and audit trails | Medium | https://www.iso.org/standard/71670.html |
| 7 | ISO 22301 | Business continuity management (BCMS) | Release/rollback evidence, RTO/RPO-related runbooks, incident evidence | Medium | https://www.iso.org/standard/75106.html |
| 8 | PCI DSS | Cardholder data security standard | Secure SDLC gates, secrets scanning, change control, access restriction evidence | Hard | https://www.pcisecuritystandards.org/standards/pci-dss/ |
| 9 | CIS Controls v8 | Baseline cybersecurity controls | Control evidence (asset inventory, access mgmt, logging, secure config, SDLC) | Medium | https://www.cisecurity.org/controls/cis-controls-list |
| 10 | NIST Cybersecurity Framework (CSF) | Cybersecurity program framework | Policy/control mapping evidence, risk register links, control ownership | Medium | https://www.nist.gov/cyberframework |
| 11 | NIST SP 800-53 Rev. 5 | Security control catalog (commonly used in gov/high assurance) | Control implementation + continuous evidence (logging, access control, change mgmt) | Hard | https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final |
| 12 | NIST SP 800-171 Rev. 3 | CUI protection requirements (DIB / contractors) | Access control, configuration/change management, audit logging, evidence exports | Hard | https://csrc.nist.gov/pubs/sp/800/171/r3/final |
| 13 | NIST SP 800-218 (SSDF) | Secure Software Development Framework | SDLC requirements, code review, testing, supply chain controls, evidence capture | Easy | https://csrc.nist.gov/pubs/sp/800/218/final |
| 14 | NIST SP 800-63-3 | Digital identity guidelines | Authentication/identity requirements, MFA evidence, access logs | Medium | https://pages.nist.gov/800-63-3/ |
| 15 | NIST SP 800-61 Rev. 2 | Incident handling guide | Incident playbooks, postmortems, response evidence, logging evidence | Medium | https://csrc.nist.gov/pubs/sp/800/61/r2/final |
| 16 | OWASP ASVS | App security verification standard | Security requirements + test evidence, secure coding checklists, gating | Medium | https://owasp.org/www-project-application-security-verification-standard/ |
| 17 | OWASP SAMM | Software assurance maturity model | Maturity evidence across governance/design/impl/verification/ops | Medium | https://owasp.org/www-project-samm/ |
| 18 | OWASP Top 10 | Common web app risks baseline | Secure coding training + scanning evidence, issue tracking, remediation SLAs | Easy | https://owasp.org/www-project-top-ten/ |
| 19 | SLSA | Supply chain levels for software artifacts | Build provenance, hermetic builds, signed attestations, release integrity | Medium | https://slsa.dev/ |
| 20 | SPDX (SBOM format) | Software bill of materials standard | SBOM generation + storage, dependency inventory, traceability to releases | Easy | https://spdx.dev/ |
| 21 | FDA 21 CFR Part 11 | Electronic records & signatures (life sciences) | Audit trails, e-signature evidence, validation artifacts, traceability | Hard | https://www.govinfo.gov/content/pkg/CFR-2024-title21-vol1/pdf/CFR-2024-title21-vol1-part11.pdf |
| 22 | HIPAA (45 CFR Part 164) | US healthcare privacy/security | Access controls, audit logs, change control evidence for systems handling PHI | Hard | https://www.law.cornell.edu/cfr/text/45/part-164 |
| 23 | HITECH Act (ARRA 2009 Title XIII) | US healthcare security/privacy enforcement | Breach notification process evidence, audit trails, risk program evidence | Hard | https://www.govinfo.gov/content/pkg/PLAW-111publ5/pdf/PLAW-111publ5.pdf |
| 24 | HITRUST CSF | Healthcare-focused assurance framework | Control mappings + evidence pack (inherits from HIPAA/ISO/NIST) | Hard | https://hitrustalliance.net/hitrust-framework |
| 25 | FDA 21 CFR Part 820 (QSR) | Quality System Regulation (medical devices) | Design controls traceability, CAPA artifacts, change history, testing evidence | Hard | https://www.law.cornell.edu/cfr/text/21/part-820 |
| 26 | IEC 62304 | Medical device software lifecycle | Lifecycle documentation, risk-based testing evidence, traceability to requirements | Hard | https://webstore.iec.ch/en/publication/2607 |
| 27 | ISO 13485 | Medical devices QMS certification | QMS evidence, design controls, document control, change control evidence | Hard | https://www.iso.org/standard/59752.html |
| 28 | ISO 14971 | Medical device risk management | Risk analysis/controls traceability, verification evidence, change impact evidence | Hard | https://www.iso.org/standard/72704.html |
| 29 | EU MDR (Regulation (EU) 2017/745) | EU medical devices regulation | Technical documentation, traceability, post-market evidence, change control | Hard | https://eur-lex.europa.eu/eli/reg/2017/745/oj |
| 30 | Sarbanes-Oxley Act (SOX) | Public companies (financial reporting controls) | Segregation of duties, approvals, change control, audit trails for relevant systems | Hard | https://www.govinfo.gov/content/pkg/PLAW-107publ204/pdf/PLAW-107publ204.pdf |
| 31 | Gramm-Leach-Bliley Act (GLBA) | US financial institutions (customer info) | Access control, audit logging, secure SDLC evidence for customer data systems | Hard | https://www.law.cornell.edu/uscode/text/15/6801 |
| 32 | FTC Safeguards Rule (16 CFR Part 314) | US financial institutions (information security program) | Risk assessment artifacts, access controls, logging, secure SDLC evidence | Hard | https://www.law.cornell.edu/cfr/text/16/part-314 |
| 33 | NYDFS Cybersecurity Regulation (23 NYCRR 500) | New York regulated financial entities | Policies, logging, incident reporting evidence, secure SDLC evidence | Hard | https://www.dfs.ny.gov/industry_guidance/cybersecurity |
| 34 | GDPR (Regulation (EU) 2016/679) | EU personal data protection | Privacy by design evidence, security of processing, access logs, DPIAs | Hard | https://eur-lex.europa.eu/eli/reg/2016/679/oj |
| 35 | UK Data Protection Act 2018 | UK data protection regime | Similar evidence to GDPR; adds local requirements and enforcement context | Hard | https://www.legislation.gov.uk/ukpga/2018/12/contents/enacted |
| 36 | ePrivacy Directive (2002/58/EC) | EU electronic communications privacy | Cookie/consent handling evidence, data minimization, privacy controls | Hard | https://eur-lex.europa.eu/eli/dir/2002/58/oj |
| 37 | CCPA (California Consumer Privacy Act) | California consumer privacy | Data mapping, deletion/DSAR process evidence, access logs, retention evidence | Hard | https://oag.ca.gov/privacy/ccpa |
| 38 | COPPA (15 U.S.C. 6501 et seq.) | US childrens privacy | Consent handling evidence, data minimization, access logs, deletion evidence | Hard | https://www.law.cornell.edu/uscode/text/15/6501 |
| 39 | FERPA (20 U.S.C. 1232g) | US student education records privacy | Access control, audit logging, data handling policies, change control evidence | Hard | https://www.law.cornell.edu/uscode/text/20/1232g |
| 40 | PIPEDA | Canada private-sector privacy | Privacy program evidence, data handling procedures, access logs, DSAR process | Hard | https://laws-lois.justice.gc.ca/eng/acts/P-8.6/ |
| 41 | Australia Privacy Act 1988 | Australia privacy law | Privacy program evidence, access logs, data breach response evidence | Hard | https://www.legislation.gov.au/C2004A03712/latest/text |
| 42 | Japan APPI | Japan personal data protection | Privacy/security program evidence, access logs, incident response evidence | Hard | https://www.ppc.go.jp/en/legal/ |
| 43 | EU DORA (Regulation (EU) 2022/2554) | EU financial operational resilience | ICT risk management evidence, incident evidence, third-party risk artifacts | Hard | https://eur-lex.europa.eu/eli/reg/2022/2554/oj |
| 44 | PSD2 (Directive (EU) 2015/2366) | EU payment services | Strong auth/security controls evidence, logging, change control for payment systems | Hard | https://eur-lex.europa.eu/eli/dir/2015/2366/oj |
| 45 | NIS2 (Directive (EU) 2022/2555) | EU cybersecurity obligations for essential/important entities | Security measures evidence, incident reporting evidence, risk management artifacts | Hard | https://eur-lex.europa.eu/eli/dir/2022/2555/oj |
| 46 | FedRAMP | US federal cloud authorization program | Control inheritance + continuous monitoring evidence (often mapped to NIST 800-53) | Hard | https://www.fedramp.gov/ |
| 47 | FISMA (Public Law 113-283) | US federal information security program | Program evidence, risk management, control assessment evidence, reporting | Hard | https://www.govinfo.gov/content/pkg/PLAW-113publ283/pdf/PLAW-113publ283.pdf |
| 48 | CMMC 2.0 | US DoD contractor cybersecurity program | Evidence aligned heavily to NIST 800-171; audit artifacts; control mapping | Hard | https://www.cisa.gov/resources-tools/resources/cybersecurity-maturity-model-certification-20-program |
| 49 | IEC 62443-4-1 (Industrial secure development lifecycle) | Industrial automation / product security | Secure development lifecycle requirements, vulnerability handling evidence | Hard | https://webstore.iec.ch/en/iec-search/result?q=62443-4-1 |
| 50 | ETSI EN 303 645 | Consumer IoT security baseline | Secure dev practices, vuln disclosure, update policy evidence, testing evidence | Medium | https://www.etsi.org/deliver/etsi_en/303600_303699/303645/02.01.01_60/en_303645v020101p.pdf |

