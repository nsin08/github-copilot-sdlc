# Market Viability & Sizing (Evidence-Based)

**Document Version:** 1.0.0  
**Date:** 2026-01-01  
**Status:** Draft (evidence-backed)  
**Audience:** Product / sales / diligence  

---

## 1. Purpose

This document replaces unsourced market-size claims with source-backed ranges and explicit definitions, so we can speak credibly about market viability during due diligence.

---

## 2. Executive Summary (What We Can Defend Today)

### 2.1 Market Size Anchors (2024)

There is no widely reported, standardized analyst category called "SDLC governance control plane". The safest way to size the opportunity is to anchor on adjacent, better-defined markets and be explicit about what we mean.

**Defensible 2024 anchors (with citations):**
- **GRC / eGRC market (broad): ~USD 49B to ~USD 63B in 2024** depending on definition (platform vs enterprise GRC) and whether services are included.
- **DevSecOps market (adjacent to SDLC-integrated security/governance): ~USD 8.84B in 2024**.

### 2.2 Why the "$47B+ compliance software spend" Claim Is Not Validated

As written, the "$47B+ in compliance-related software spend across Healthcare, FinTech, Defense, and Government" claim is **not currently verifiable** because it:
- Mixes categories (software vs services vs internal headcount vs audit fees vs consulting).
- Implies a sector roll-up without a method, a definition of included spend, or citations.

**Recommended correction:** Use the cited ranges above and label them as TAM anchors (not SAM/SOM), then separately estimate a constrained SAM from buyer/budget validation.

---

## 3. Definitions (Prevent TAM Inflation)

### 3.1 GRC / eGRC

Enterprise governance, risk, and compliance platforms (often includes **software + services** such as consulting/integration).

### 3.2 DevSecOps

Development + security + operations tooling that integrates security across the SDLC (typically **developer/security toolchain** budgets).

### 3.3 "Governed SDLC" (This Product)

Enforcement + evidence generation inside the developer workflow (GitHub). In practice, budgets typically come from a mix of:
- DevSecOps / AppSec budgets (pipeline controls, security gates, policy-as-code)
- GRC/compliance budgets (evidence, controls, audit response)
- Engineering productivity budgets (workflow, traceability, reporting)

---

## 4. Market Size Benchmarks (Cited)

These benchmarks are not additive; they overlap. They are provided to establish credible "order of magnitude" anchors for investor conversations and to avoid unsourced claims.

| Market Category | 2024 Market Size | Notes / Caveats | Source |
|---|---:|---|---|
| GRC platform market | USD 49.2B | Platform framing; may include services depending on methodology | IMARC Group: https://www.imarcgroup.com/governance-risk-compliance-platform-market |
| Enterprise GRC (eGRC) market | USD 62.92B | Broad enterprise framing; explicitly includes components/services in report structure | Grand View Research: https://www.grandviewresearch.com/industry-analysis/enterprise-governance-risk-compliance-egrc-market |
| DevSecOps market | USD 8,841.8M (USD 8.84B) | Adjacent market focused on SDLC tooling; closer to where this product could land initially | Grand View Research: https://www.grandviewresearch.com/industry-analysis/development-security-operation-market-report |

### 4.1 Optional: "Software Spend" Approximation (Derived, Not a Reported Market Total)

Some claims are specifically about "software spend" (excluding services). If you need a software-only approximation, be explicit that it is derived and depends on the analyst's segmentation.

- Grand View Research states the eGRC **software segment** accounted for "nearly 65.0%" of revenue in 2024. Applying that share to the stated 2024 market size (USD 62.92B) yields an approximate software-only figure of **~USD 41B** (0.65 * 62.92B).  
  Source: https://www.grandviewresearch.com/industry-analysis/enterprise-governance-risk-compliance-egrc-market

- Grand View Research also breaks out DevSecOps **software revenue** in 2024 as USD 6,552.4M (USD 6.55B).  
  Source: https://www.grandviewresearch.com/industry-analysis/development-security-operation-market-report

---

## 5. What Is Actually Addressable (TAM vs SAM vs SOM)

### 5.1 TAM (Defensible, Broad)

If we define the broad opportunity as "GRC/eGRC platforms and adjacent SDLC-integrated security/governance", a reasonable, sourced 2024 TAM anchor is:
- **~USD 49B to ~USD 63B** for GRC/eGRC market definitions; plus
- **~USD 8.8B** for DevSecOps (adjacent).

This is still a broad TAM statement. It does not imply we can sell to the whole market.

### 5.2 SAM (Needs Validation)

Our serviceable market depends on constraints not validated in this repo yet:
- GitHub adoption in regulated orgs for production SDLC (and which GitHub features are actually available/enabled)
- Who owns the budget (engineering vs security vs compliance) and what line-item we replace
- Whether the buyer wants "GitHub-native" (no new UI) or requires integration into an existing GRC suite

Until we have buyer interviews and pricing validation, SAM should be presented as a hypothesis, not a fact.

---

## 6. Recommended External-Facing Market Statement (Replace "$47B+")

Use language like this (and keep it consistent across docs):

> "Third-party analysts estimate the global GRC/eGRC market at roughly USD 49B to USD 63B in 2024 (definition-dependent), with the adjacent DevSecOps market estimated around USD 8.8B in 2024. Our initial wedge is governed SDLC enforcement and audit-ready evidence generation inside GitHub workflows."

---

## 7. Minimum Diligence Checklist (To Convert SAM From Guess to Fact)

- Define the exact category for pricing: "DevSecOps control plane", "SDLC governance", or "compliance evidence automation".
- Interview 10-15 buyers across engineering, security, compliance in regulated orgs; record:
  - current toolchain (GitHub/Jira/ServiceNow/Archer/etc.)
  - who signs the check and typical contract sizes
  - current pain (audit prep time, change-control failure modes, evidence gaps)
- Build a bottom-up model from:
  - target buyer segments (e.g., regulated software teams on GitHub Enterprise)
  - estimated spend per team/app/year (validated via interviews)

---

## 8. Sources (Direct Links)

- IMARC Group - Governance, Risk and Compliance Platform Market (states "USD 49.2 Billion in 2024"): https://www.imarcgroup.com/governance-risk-compliance-platform-market  
- Grand View Research - Enterprise GRC (states "USD 62.92 billion in 2024"): https://www.grandviewresearch.com/industry-analysis/enterprise-governance-risk-compliance-egrc-market  
- Grand View Research - DevSecOps (states "USD 8,841.8 million in 2024"): https://www.grandviewresearch.com/industry-analysis/development-security-operation-market-report  
