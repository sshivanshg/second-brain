---
created: 2026-05-27
type: architecture
status: implemented
tags: arth-saathi, tech, auth, session, pwa, mobile, ux
repo_doc: /Users/shivanshgupta/Documents/arth/docs/SESSION_PERSISTENCE.md
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
2. **`SessionKeepAlive`** — a new side-effect-only `fetch('/api/auth/session')` on mount / visibility-regain / `online` (throttled 5 min). The server response re-issues the cookie with a fresh `expires`, so the rolling window stays alive — but it never reads the body or touches React state, so **a failed request can't log anyone out**. Resilience by construction, not by retry.
3. **Harden the boundary** — `shouldPurgeOnIdentityChange` (pure, unit-tested) only purges on a real switch to a *different signed-in account* (`A → B`). `A → null` is treated as transient and is a no-op; genuine sign-out still purges explicitly via `logoutAndCleanCaches`.
4. **90-day rolling window** (`SESSION_MAX_AGE_SECONDS`, was 30). Re-auth is OTP/Firebase — expensive on mobile — so a quarter-long rolling window means an active user effectively never re-logs-in.

Mental model: **the LLM-of-auth (NextAuth's client refetch) is unreliable on mobile, so don't let it make the logout decision. One source of truth for "you're out": the server cookie check, plus the already-guarded 401 path.**

## What still logs you out (correctly)
- Server layout `auth()` on navigation → genuinely expired/invalid cookie → `/login`.
- `fetchApiJson` verifies via `sessionConfirmedDead()` before redirecting on a `401`, so a transient backend 401 never logs you out.
- Explicit logout → `logoutAndCleanCaches` → `signOut()`.

## Residual limit (honest)
iOS evicts a home-screen PWA's storage after a long stretch of **non-use** (ITP); no server setting overrides that. We keep *active* users signed in indefinitely and we killed the flaky-network false logouts — we don't change OS eviction for an app left untouched for weeks. A future native wrapper would.

## Map
- Config: `lib/auth.ts` (`SESSION_MAX_AGE_SECONDS`, cookie options)
- Providers: `apps/web/app/providers.tsx` (`SessionProvider` props)
- Keep-alive: `apps/web/components/providers/SessionKeepAlive.tsx`
- Boundary: `apps/web/components/providers/SessionChangeBoundary.tsx` + `session-change.ts` (pure decision) + `session-change.test.ts`
- 401 guard: `lib/api-fetch.ts` (`sessionConfirmedDead`)
- Repo spec: `docs/SESSION_PERSISTENCE.md`

## Related
- [[Architecture — Arth Saathi]]
- [[Aadhaar KYC — implementation]]
- [[Daily-active surfaces own monthly-active surfaces]]
- [[Documentation protocol — repo ↔ vault]]
