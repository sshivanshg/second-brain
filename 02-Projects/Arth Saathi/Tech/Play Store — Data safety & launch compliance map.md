---
created: 2026-06-03
type: compliance
status: closed test (Alpha) submitted for review — build vc 14
tags: arth-saathi, tech, compliance, launch, play-store
repo_doc: /Users/shivanshgupta/projects/arth/docs/compliance/DATA_SAFETY.md
---

# Play Store — Data safety & launch compliance map

> Thinking + map. **Repo is truth:** `docs/compliance/DATA_SAFETY.md` is the canonical answer sheet; this note is the *why* behind the judgment calls. Built from a two-explorer code audit of what data actually leaves the device.

## Where this sits in the launch
First production Android build (`vc 14`, `in.arthsaathi.app`) is uploaded to **Closed testing → Alpha** and in review. Google's gate to production: **≥12 testers opted in × ≥14 days** on the closed track. So the binary is the easy part — the slow part is real humans opting in, and the *fragile* part is passing review. The audit existed to de-risk review.

## The four judgment calls that matter

**1. Shared = No, and that's defensible.** Every third party — Twilio (OTP/WhatsApp), Cashfree (eKYC), Razorpay (payments), Anthropic + Google/Gemini (AI Chat), Sentry, Cloudflare R2, Expo (push) — is a **processor acting on our behalf**, which Google explicitly *excludes* from the definition of "sharing." So "collected = yes, shared = no" across the board. The one string attached: it holds only while the AI vendors stay on **no-training API terms**. This is the everyday face of [[Compliance integration is a structural moat]] — the posture is structural, not a checkbox.

**2. Do NOT over-declare "Financial features."** The trap for a khata app: khata *records* debt, it does not *issue* it. We do bookkeeping + GST invoicing + subscription payments — **not** lending, deposits, or banking. Ticking loans/banking on the App-content declaration triggers license + documentation requirements we can't satisfy → instant rejection. Same family of discipline as the Aadhaar rule in [[Aadhaar KYC — implementation]]: *use the narrowest true category.*

**3. Aadhaar is the scariest data type and it's already handled right.** Because the raw 12-digit number is **never persisted** (HMAC hash + masked last-4 + verified name only — the never-store-raw invariant from [[Aadhaar KYC — implementation]]), it gets declared under *Personal info → Other info, processed ephemerally*, not as stored sensitive ID. The code's discipline directly shrinks the compliance surface.

**4. The real rejection risk isn't the form — it's App access.** The app is login- *and* role-gated (owner vs employee) with KYC behind it. A reviewer who can't log in auto-rejects. So the lever is a seeded **owner + employee** demo tenant (`prisma/seed-test-user.ts`: owner `1234567899/1234567899`, employee `9800000001/9800000001aarav`, no OTP on login). **Gotcha:** the seed writes to whatever `DATABASE_URL` points to — that tenant must be created in the **production** DB the release talks to, or the credentials are useless to the reviewer.

## Two honest gaps the audit surfaced (security, not form)
- **`s3.service.ts` is a stub.** KYC document storage (Aadhaar cards, ID proofs in R2) has no encryption-at-rest / access-control / presigned URLs yet. Must land before real users upload Aadhaar docs — this is the highest-sensitivity data we touch and it's currently unprotected at the storage layer. Cross-checks against the [[Architecture — Arth Saathi]] DPDP invariants.
- **Local WatermelonDB is unencrypted** — khata, salaries, employee PII sit in plaintext in app storage, relying on OS disk encryption. Tokens/PIN are correctly in encrypted SecureStore. SQLCipher on the financial tables is the fix.

## Privacy-policy alignment (Google cross-checks this)
The `/privacy` subprocessor list named only Vercel; the Data-safety form implies flows to eight more vendors. Expanded the list to name all of them (`apps/landing/app/privacy/page.tsx`) so the public policy and the form agree — a common silent reviewer flag, and expected under India's DPDP Act anyway. Keep it current when integrations change → [[Source-of-truth pointers — Arth Saathi]].

## Related
- [[Aadhaar KYC — implementation]]
- [[Architecture — Arth Saathi]]
- [[Compliance integration is a structural moat]]
- [[Source-of-truth pointers — Arth Saathi]]
