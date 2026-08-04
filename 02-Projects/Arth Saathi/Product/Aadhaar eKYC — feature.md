---
created: 2026-05-24
type: product
status: built — mock provider live, real provider pending
tags: arth-saathi, features, compliance, kyc
repo_doc: /Users/shivanshgupta/projects/arth/docs/AADHAAR_KYC.md
---

# Aadhaar eKYC — feature

> **One-liner:** an employee proves who they are with a 60-second Aadhaar OTP, turning a phone-number signup into a *verified* workforce. The flagship trust feature — and our first step onto the [[Compliance integration is a structural moat|compliance moat]].

**Status:** built end-to-end (web + mobile + backend), running today on a **mock provider** (no credentials). Going live needs a licensed provider switched on by config. Repo truth: `docs/AADHAAR_KYC.md`.

## Why this matters
- It was already a named [[Arth Saathi|18-month milestone]] ("Aadhaar eKYC + UPI salary disbursement live"). Verified identity is the prerequisite for the fintech layer — you cannot disburse salary, extend EWA, or underwrite a micro-loan to an unverified person.
- Marketing edge: "every employee verified" is a claim the notebook and the WhatsApp group can never make.
- Ties to [[Vertical SMB-OS earns most revenue from embedded fintech]] — KYC is the on-ramp.

## What it is (and isn't)
- **Is:** one-time **identity verification** during employee onboarding, via a **UIDAI-licensed sub-AUA/KUA partner**. A permitted, defensible use of Aadhaar.
- **Is NOT:** a login or recurring auth factor. Aadhaar-as-login is *not* a permitted purpose under the Aadhaar Act — deliberately out of scope. (This decision is load-bearing; see the tech note.)

## The flow (employee app — web + mobile)
1. **Consent** — plain-language explanation + a required consent checkbox. No OTP is sent without it; consent is stored.
2. **Aadhaar number** — validated locally (12 digits + Verhoeff checksum) before anything leaves the device, so a typo never burns an OTP.
3. **OTP** — sent to the Aadhaar-linked mobile via the partner; 6-digit entry with a live expiry countdown and a start-over path.
4. **Verified** — green confirmation showing verified name + masked Aadhaar (`XXXX XXXX 1234`) + date.

Entry point: employee **Profile → "Verify your Aadhaar"**. A status badge on the profile reflects verified / not-verified.

## Privacy promise (what we say to users)
> "We never store your full Aadhaar number — only the last 4 digits and your verification status."

This is literally true in the data model (see tech note): the raw number is used transiently and discarded.

## Roadmap fit
- **Now:** mock-mode demo; pick + wire a real provider (Cashfree adapter scaffolded).
- **Next:** UPI salary disbursement (verified identity unlocks payouts) → EWA → micro-loans. See [[Feature catalog]] Q1/Q3.

## Related
- [[Aadhaar KYC — implementation]] — how it's built
- [[Feature catalog]]
- [[Architecture — Arth Saathi]]
- [[Compliance integration is a structural moat]]
- [[Source-of-truth pointers — Arth Saathi]]
