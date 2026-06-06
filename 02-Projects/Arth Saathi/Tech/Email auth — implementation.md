---
created: 2026-06-04
type: architecture
status: implemented
tags: arth-saathi, tech, auth, otp, email, nextauth
repo_doc: /Users/shivanshgupta/Documents/arth/docs/features/EMAIL_AUTH.md
---

# Email auth — implementation

> Thinking + map. **Repo is truth:** `docs/features/EMAIL_AUTH.md`. Product framing in [[Email auth — feature]]. Deploy/ops story in [[Backend deploy & revision health — Arth Saathi]].

## Shape (two stacks, one User table)
Arth Saathi has **two independent auth stacks** on one Prisma `User` table: **web** = NextAuth v5 (root `lib/auth.ts`, JWT cookies), **mobile** = the standalone Fastify backend (`apps/backend`, Bearer access+refresh JWT). Email auth had to be added to **both**, reusing the existing unique/nullable `User.email`. Additive only — two new tables, no `User` change:
- `EmailOtpChallenge` — code stored as **HMAC-SHA256 keyed by AUTH_SECRET** (never plaintext, unlike the legacy phone `OtpChallenge`), with `purpose` (register|login), `attempts` (burns after 5), short TTL.
- `RegistrationEmailSend` — per-email send cap.

## Flow
`send-email-code` → `verify-email-code` → mints an HMAC **registration proof** → `register-business` (already took an email). Login: NextAuth `email-password` + `email-otp` credential providers (web); `/api/mobile/auth/email-login` + `email-login-otp` (mobile). Email delivery = **Resend via plain `fetch`** (no SDK). Dev with no key logs the code; prod with no key fails loud (503).

## The gotcha that cost the most: NextAuth swallows `/api/auth/*`
The web send/verify endpoints live under `/api/auth/…`. The app has a generic backend proxy at `app/api/[...path]/route.ts` whose allowlist *mentions* these paths — **but that proxy never sees them.** Next.js route precedence makes `/api/auth/[...nextauth]` (NextAuth's catch-all) **out-specify** the generic `/api/[...path]` catch-all, so every `/api/auth/X` is matched by NextAuth, which returns **`400 "Bad request"`** for unknown actions. Symptom: `x-matched-path: /api/auth/[...nextauth]` in the Network tab. (The old phone `send-otp` had the same latent issue — it just isn't on a live path.)

**The working pattern (how `register-business` always worked):** a **dedicated route file** at the exact path out-specifies the catch-all. So each email endpoint needs its own file that forwards to the backend with the proxy secret:
- `apps/web/app/api/auth/login/send-email-code/route.ts`
- `apps/web/app/api/auth/register/send-email-code/route.ts`
- `apps/web/app/api/auth/register/verify-email-code/route.ts`
- shared forwarder: `apps/web/lib/forward-to-backend.ts` (mirrors `register-business/route.ts`).

**Rule for future agents:** any new web endpoint under `/api/auth/*` must be its own route file — adding it to the `[...path]` proxy allowlist does nothing. Mobile is unaffected (it calls the backend FQDN directly, not through Next).

## Cross-surface invariant: same AUTH_SECRET
Email-OTP **login** is the one path where crypto crosses surfaces: the backend **hashes** the code on send; the web NextAuth `email-otp` provider **verifies** it. Both use `AUTH_SECRET` for the HMAC (`lib/email-otp-code.ts`, shared). Web (Vercel) and backend (Azure) **must carry the same AUTH_SECRET** — they already do for NextAuth JWTs, but it's a load-bearing assumption.

## Gating
`emailAuthEnabled = NEXT_PUBLIC_EMAIL_AUTH_ENABLED === "true" || NODE_ENV === "development"` (web `lib/env.ts`); `EXPO_PUBLIC_EMAIL_AUTH_ENABLED === "true" || __DEV__` (mobile). On in dev, opt-in in prod. `EXPO_PUBLIC_*` is **baked at build time** → mobile needs a new build (or `eas update`) to flip it; web `NEXT_PUBLIC_*` needs a prod rebuild (not just an env set).

## Map
- Shared crypto + email validation: `lib/email-otp-code.ts`
- Backend: `apps/backend/src/services/{email,email-otp,registration-email-rate-limit}.service.ts`; routes in `apps/backend/src/server/routes/auth-routes.ts` + `mobile.ts`; proof `registration-proof.service.ts` (`mintEmail`)
- Web: providers in `lib/auth.ts` (`email-password`, `email-otp`); dedicated routes above; UI in `app/(auth)/login/page.tsx` + `register/page.tsx` + `register/email/page.tsx`
- Mobile: `apps/mobile/lib/auth.ts` store methods; `app/(auth)/login.tsx` + `register.tsx`
- Schema: `EmailOtpChallenge`, `RegistrationEmailSend` (migration `20260603100000_email_otp_auth`)

## Related
- [[Email auth — feature]]
- [[Backend deploy & revision health — Arth Saathi]]
- [[Mobile Google sign-in — invalid_client fix]]
- [[Architecture — Arth Saathi]]
- [[Monorepo layout — Arth Saathi]]
