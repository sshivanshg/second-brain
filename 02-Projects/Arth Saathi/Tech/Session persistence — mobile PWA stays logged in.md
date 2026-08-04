---
created: 2026-05-27
updated: 2026-05-31
type: architecture
status: implemented
tags: arth-saathi, tech, auth, session, pwa, mobile, ux
repo_doc: /Users/shivanshgupta/projects/arth/docs/features/SESSION_PERSISTENCE.md
---

# Session persistence — mobile PWA stays logged in

> Thinking + map. **Repo is truth:** `docs/SESSION_PERSISTENCE.md` is the canonical spec; this note is the orientation layer. Auth sibling to [[Aadhaar KYC — implementation]] (KYC, never login) and [[Architecture — Arth Saathi]].

## What broke
Owners using the installed web PWA kept getting "logged out after a while" — noticeably on **phones**, rarely on desktop. For a daily-use bookkeeping app where re-login is an OTP dance, that's a retention wound on the most-used surface. ([[Daily-active surfaces own monthly-active surfaces]] — and nothing kills a daily habit like being kicked to a login screen.)

## The non-obvious cause
The session was never actually dead. It's a **stateless NextAuth JWT in an httpOnly cookie**, decoded in-process on every server `auth()` — no session store, so backend scale-to-zero and multi-replica can't touch it. The logout was a **client-side illusion** that turned destructive.

Two compounding traps, both in the *client* session layer:

1. **NextAuth nulls the session on any transient fetch failure.** `SessionProvider` refetches `/api/auth/session` on every `visibilitychange` — which on a phone fires on *every app resume*. NextAuth's `fetchData` returns `null` on **any** failure (flaky mobile network, a 5xx/504, an HTML error body all look identical), and it can't tell that apart from a real sign-out. One unlucky fetch → `useSession()` flips to `unauthenticated`.
2. **That false-null was wired to destruction.** `SessionChangeBoundary` saw `userId: X → null` and **wiped the React Query + service-worker caches**, and UI gated on auth status rendered its logged-out shell — all while the cookie on disk was still valid.

The general lesson, worth carrying everywhere: **"failed to fetch" and "confirmed empty" are different facts, and collapsing them is dangerous.** A library that treats a network blip as a sign-out, feeding a boundary that treats a sign-out as "nuke everything", manufactures the exact bug the user reported. Mobile just exposes it because mobile backgrounds the webview constantly.

## The fix — distrust the fragile path, refresh resiliently
Server gating (`auth()` in the dashboard/employee layouts) was already correct — it only logs out on a genuinely expired cookie. So the fix is entirely client-side:

1. **Stop trusting NextAuth's auto-refetch** — `refetchOnWindowFocus={false}`, `refetchWhenOffline={false}`. A transient fetch can no longer null the in-memory session.
2. **`SessionKeepAlive`** — a side-effect-only `fetch('/api/auth/session')` on mount / a steady 5-min heartbeat / visibility-regain / `pageshow` (mobile bfcache restore) / `online` (throttled 5 min). The server response re-issues the cookie with a fresh `expires`, so the rolling window stays alive — but it never reads the body or touches React state, so **a failed request can't log anyone out**. Resilience by construction, not by retry. *(Verified against `@auth/core` source: a JWT session GET re-signs the token and re-sets the cookie with `now + maxAge` **unconditionally** every read — so each keep-alive genuinely rolls the window, `updateAge` notwithstanding.)*
3. **Harden the boundary** — `shouldPurgeOnIdentityChange` (pure, unit-tested) only purges on a real switch to a *different signed-in account* (`A → B`). `A → null` is treated as transient and is a no-op; genuine sign-out still purges explicitly via `logoutAndCleanCaches`.
4. **365-day rolling window** (`SESSION_MAX_AGE_SECONDS`, was 30 → 90 → 365 on 2026-05-30). Re-auth is OTP/Firebase — expensive on mobile — so a one-year rolling window means an active user effectively never re-logs-in. 365 is the practical ceiling: Chrome caps any cookie at 400 days, so there's no point reaching higher.

Mental model: **the LLM-of-auth (NextAuth's client refetch) is unreliable on mobile, so don't let it make the logout decision. One source of truth for "you're out": the server cookie check, plus the already-guarded 401 path.**

## Sibling fix — registration must land *inside* the app (2026-05-30)
Same felt symptom ("it logged me out / kicked me back to login"), different mechanism: finishing **business registration** dropped the new owner back at the sign-up form instead of opening the dashboard.

New-business signup is **Google-OAuth-first**. During the OAuth round-trip the `jwt` callback stamps the token *registration-in-progress* (`oauthProvider`/`oauthEmail`, no Prisma `id`/`role`/`businessId`) and routes to `/register/details`. The catch: that linking branch only runs **while an OAuth `account` is present** — i.e. during the sign-in round-trip. After the form POST creates the OWNER row, the client calls `getSession()`, a plain read with **no `account`**, so nothing upgraded the token. The dashboard layout saw `role !== "OWNER"` and bounced them back into signup. A capability gap masquerading as a logout.

Fix — **auto-promote on read**: the `jwt` callback now links a registration-in-progress token to the freshly-created OWNER on *any* session read, keyed off the Google-**verified** email (`isPromotableGoogleRegistrationToken`, pure + unit-tested in `lib/auth-token-promotion.ts`). One indexed `user.findFirst`; skipped once linked; a failed lookup leaves the token untouched (next keep-alive retries) so it can never log anyone out. `register/details` then confirms `getSession()` returns a full OWNER session, falling back to an explicit `owner-phone-password` sign-in only if promotion somehow didn't take — and routes to `/dashboard`, **never `/login`**.

Mental model carried over from the logout fix: **the same surface lesson — don't let a transient/ambiguous read drive a destructive route.** Here the "ambiguity" was a token that *will* be linkable the instant the OWNER row exists; promoting on read closes the window instead of stranding the user.

## Third trap — the launch target itself ignored the session (2026-05-31)
Same felt symptom again ("it logs me out every time I open the app, I have to tap Google"), but the *opposite* of the flaky case: **100% reproducible, every cold launch.** That reproducibility is the tell — a network blip is intermittent; this was deterministic, so the session was never the variable.

Cause: the installed PWA's manifest `start_url` is **`/login`**. It has to be — the app host's `/` is redirected to the marketing site by middleware, so `/` can't be the launch target. But `/login` was a pure client form whose route layout only redirected panel-only founders. So every home-screen launch dropped a *fully signed-in* owner/employee onto the login form, and they re-tapped "Continue with Google" — which round-trips instantly (their Google session persists), making it feel like a logout-and-reauth loop. The cookie was valid the entire time.

Fix: `app/(auth)/login/layout.tsx` now gates on server `auth()` *before* the form renders, mirroring `app/page.tsx` + the group layouts — OWNER→`/dashboard`, EMPLOYEE→`/employee/dashboard`, Google reg-in-progress (`oauthEmail`, no app role)→`/register/details`, founder→admin app. A genuinely logged-out user, or a failed OAuth round-trip (`/login?error=…` creates *no* session), still gets the form with its error/`?registered=1` messaging.

Surface lesson, third time it shows up here: **if your PWA `start_url` is an auth page, that page must honor an existing session — otherwise the launch icon itself manufactures a logout on every open.** The whole "stay logged in" story only holds if the *entry point* checks the cookie, not just the protected pages behind it.

## What still logs you out (correctly)
- Server layout `auth()` on navigation → genuinely expired/invalid cookie → `/login`.
- `fetchApiJson` verifies via `sessionConfirmedDead()` before redirecting on a `401`, so a transient backend 401 never logs you out.
- Explicit logout → `logoutAndCleanCaches` → `signOut()`.

## Residual limit (honest)
iOS evicts a home-screen PWA's storage after a long stretch of **non-use** (ITP); no server setting overrides that. We keep *active* users signed in indefinitely and we killed the flaky-network false logouts — we don't change OS eviction for an app left untouched for weeks. A future native wrapper would.

## Map
- Config: `lib/auth.ts` (`SESSION_MAX_AGE_SECONDS`, cookie options, `jwt` promotion branch)
- Registration promotion (pure gate): `lib/auth-token-promotion.ts` + `lib/auth-token-promotion.test.ts`
- Registration UI: `apps/web/app/(auth)/register/details/page.tsx` (`completeRegistration` → `/dashboard`)
- Providers: `apps/web/app/providers.tsx` (`SessionProvider` props)
- Keep-alive: `apps/web/components/providers/SessionKeepAlive.tsx` (mount / 5-min heartbeat / visibility / `pageshow` / `online`)
- Boundary: `apps/web/components/providers/SessionChangeBoundary.tsx` + `session-change.ts` (pure decision) + `session-change.test.ts`
- 401 guard: `lib/api-fetch.ts` (`sessionConfirmedDead`)
- Launch gate: `apps/web/app/(auth)/login/layout.tsx` (authenticated-session redirect) + `apps/web/app/manifest.ts` (`start_url: "/login"`)
- Repo spec: `docs/features/SESSION_PERSISTENCE.md`

## Related
- [[Architecture — Arth Saathi]]
- [[Aadhaar KYC — implementation]]
- [[Daily-active surfaces own monthly-active surfaces]]
- [[Documentation protocol — repo ↔ vault]]
