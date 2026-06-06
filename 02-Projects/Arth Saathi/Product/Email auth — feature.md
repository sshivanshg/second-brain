---
created: 2026-06-04
type: product
status: live
tags: arth-saathi, product, auth, onboarding
repo_doc: /Users/shivanshgupta/Documents/arth/docs/features/EMAIL_AUTH.md
---

# Email auth — feature

> Product thinking + map. **Repo is truth:** `docs/features/EMAIL_AUTH.md` is the spec; this is the *why*. Implementation map in [[Email auth — implementation]].

## What it is
A **third way in** alongside Google (owners) and phone+password (owners + employees): sign up / sign in with an **email + password**, where the email is verified by a **6-digit one-time code**, plus an **"email me a code"** passwordless login. Live on `app.arthsaathi.co.in` since 2026-06-04; mobile ships with the next Play build.

## Why we added it
- **Google native sign-in is operationally fragile.** It depends on per-build signing-SHA-1 registration in Google Cloud (see [[Mobile Google sign-in — invalid_client fix]]) — every keystore/track needs a console step, and a mismatch dead-ends the user. Email works on every platform with zero per-build config.
- **Phone OTP costs SMS credits** (Twilio) and has deliverability quirks; email delivery is near-free and reliable once the domain is verified.
- It opens registration to owners who think in email-first terms and gives everyone a **self-serve recovery-ish path** (code to inbox) without us building a full password-reset flow yet.

## The identity rule that makes it safe to add
`User.email` is **globally unique** and is the single account key, so the three doors converge on **one account**: Google (same verified email), email+password, and email-OTP all resolve to the same `User` row. Registering with Google for an email that already exists just signs you in (no duplicate). The one caveat: matching is **by exact lowercased email** — a different Google email is a different identity (no phone-based linking). This is why the feature is *purely additive* — it reuses the column that already exists and never migrates or merges existing users (the founder's hard constraint).

## How a user experiences it
- **Login** (Business Owner tab): subtle toggle — "Use email & password instead →" and "Email me a code →" — so phone-first users are undisturbed; Google stays the top button. Mirrored on mobile.
- **Register**: a prominent "Sign up with email" CTA → email → 6-digit code → business name/password/phone/T&C → in. Equal visual weight to Google (founder asked for parity, not a buried link).

## Rollout posture (deliberately dark until ready)
Gated behind a flag on each surface (`EMAIL_AUTH_ENABLED` backend, `NEXT_PUBLIC_EMAIL_AUTH_ENABLED` web, `EXPO_PUBLIC_EMAIL_AUTH_ENABLED` mobile): **on in dev, opt-in in prod**, so the UI stayed invisible until the migration + Resend (email provider) + backend were verified live. This is what let us merge and deploy the code safely before flipping it on.

## Related
- [[Email auth — implementation]]
- [[Backend deploy & revision health — Arth Saathi]]
- [[Mobile Google sign-in — invalid_client fix]]
- [[Session persistence — mobile PWA stays logged in]]
- [[Feature catalog]]
- [[Arth Saathi]]
