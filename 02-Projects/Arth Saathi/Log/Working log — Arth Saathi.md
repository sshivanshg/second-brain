---
created: 2026-05-23
type: log
tags: arth-saathi, log
---

# Working log — Arth Saathi

> Append-only log of decisions, blockers, and shipped work. Newest at top.

## 2026-05-28
- **Wrote: non-technical vision & feature story** — a business-angle, zero-jargon telling of the whole product for non-engineers (investors, partners, family, non-tech hires). Repo: `docs/product/Arth_Saathi_Vision.md` · vault: [[Vision narrative — khata to saathi]] (linked into hub under GTM & Business).
- **Spine:** *khata → saathi* — lead with the notebook the audience already owns, and the upgrade is you can now *talk* to it. AI assistant framed as the headline moat ("talk, don't learn"), Confirm-before-write framed as a selling point not fine print. Reinforces [[Voice and Hinglish are the UX not a translation]], [[Confirm before write — AI mutations need human approval]].
- **Caught a big scope gap:** first draft (and the `docs/FEATURES_ECOSYSTEM.md` it was based on) covered only staff/salary + GST billing. A code audit found a full **Vyapar-parity commerce layer already built**: khata (customer/supplier udhaar ledger), inventory, payments in/out, expenses + other income, purchase bills, purchase/sales orders, e-invoicing + e-way bill (mock until NIC creds), accounting reports (day book / P&L / balance sheet / cash flow), Tally export, manufacturing/BOM, multi-firm, RBAC, bulk import, dues-reminder engine. Many have complete backends with web UI **rolling out** — now in the vision doc, labelled honestly. Lesson: **trust the code, not an older summary doc.**
- **Follow-up flagged:** `docs/FEATURES_ECOSYSTEM.md` (the canonical features reference) still lags reality and needs the same audit-driven update.

## 2026-05-27
- **Shipped: first-run walkthrough (product tour)** — guided onboarding for new users across web (owner) + mobile (owner & employee). → [[First-run walkthrough — feature]]
- **Shape:** hybrid — *welcome modal → spotlight coachmarks on the real nav → dismissible getting-started checklist*. Skippable, re-launchable, shows once per user. Distinct from the business-setup wizard (that configures the account; this orients the human).
- **Hard part — responsive targeting:** desktop uses a sidebar, mobile a bottom nav. Each step carries **both** candidate selectors and the engine spotlights the *first visible one* (the hidden layout measures `0×0`), so one step definition lights up the correct element at every breakpoint with no branching. Mobile measures live nav tabs via `measureInWindow`; the spotlight is an absolute `zIndex:1000` overlay (not a Modal) so the real nav at `zIndex:40` shows through a transparent hole. Lesson: **reuse the real UI as the tour surface** — don't build a parallel screenshot slideshow that drifts from the app.
- **State:** per-user, device-local (`localStorage` / `AsyncStorage` keyed by userId) — shared shop phone shows it once per person, nothing leaks across accounts, nothing to wipe on logout. Reuses existing `ds-*` tokens + brand greens (no new colors), ≥44pt targets, light/dark. Persistence unit-tested. Spec: `docs/WALKTHROUGH.md`.
- **Fixed: mobile PWA "logs me out after a while"** — owners kept getting kicked to /login on phones, rarely on desktop. → [[Session persistence — mobile PWA stays logged in]]
- **Root cause:** not a dead session — a client-side illusion. The session is a stateless NextAuth JWT cookie (decoded in-process, no store), but `SessionProvider` refetches `/api/auth/session` on every `visibilitychange` (fires on *every* mobile app resume), and NextAuth's `fetchData` returns `null` on *any* transient failure (flaky network, 5xx, HTML error body) — indistinguishable from sign-out. That false-null flipped `useSession()` to unauthenticated, and `SessionChangeBoundary` then wiped React Query + service-worker caches on `userId X → null`, all while the cookie was still valid.
- **Fix (client-only; server `auth()` gating was already correct):** disabled NextAuth auto-refetch (`refetchOnWindowFocus={false}`, `refetchWhenOffline={false}`); added `SessionKeepAlive` — a side-effect-only `fetch('/api/auth/session')` on mount/visible/online that re-issues the rolling cookie but never touches React state, so a failed request *cannot* log anyone out; hardened the purge boundary (`shouldPurgeOnIdentityChange`, unit-tested) so `A → null` is a no-op and only a real account switch `A → B` purges; bumped the rolling window 30 → 90 days. Lesson: **"failed to fetch" ≠ "confirmed empty" — collapsing them manufactures phantom logouts.** Spec: `docs/SESSION_PERSISTENCE.md`.
- **Residual:** iOS evicts a home-screen PWA's storage after sustained *non-use* (ITP) — no server fix; active users now stay signed in indefinitely.

## 2026-05-25
- **Fixed: agent "Invalid arguments" loop on "mark everyone absent"** — chat returned *"Invalid arguments for mark_attendance_bulk."* four times in one bubble. → [[Agent attendance reliability — fix]]
- **Root cause:** whole-team tools existed for *present* and *holiday* (both `markDay`, no list) but **not absent**, so "everyone absent" fell to `mark_attendance_bulk`, whose schema needs an explicit `employees[]` the model couldn't conjure → Zod reject. The 4-loop retry + a fallback that *joins all tool summaries* turned one failure into four.
- **Fix (4 layers):** new `mark_all_absent`; `mark_attendance_bulk` expands whole-team tokens ("everyone"/"sabhi"/…) via `markDay`; prompt maps the command + bans "everyone" in bulk; loop guard dedupes identical failing calls so no duplicate-error bubbles. Lesson: a schema that forces the model to fabricate a value it can't produce turns a capability gap into a raw error — fail gracefully or close the gap. Reinforces [[Agent capability vision — do any manual task, dead-simple]].
- Audited employee/payroll/attendance agent tools: `mark_attendance_bulk` was the only fabricate-a-list trap; the rest fail gracefully with suggestions.
- **Full agent robustness sweep (all 19 tool categories):** added `executeToolSafely` — one central guard so a thrown service error degrades to a graceful result (4xx AppError messages surfaced, 5xx hidden) instead of crashing the turn; this single fix covered every inventory/leave "missing try-catch" finding at once. Typo-tolerant employee resolution (OSA edit distance: Rmesh→Ramesh, Pirya→Priya). `parseRelativeDate` now takes DD/MM/YYYY etc. + "aaj" and rejects impossible dates. Surfaced friendly summaries so the owner never sees "Invalid arguments" again. `lines.min(1)` on invoices/SOs is intentional (clarify, not empty-create). **186 agent tests green.** Spec: `docs/AI/AGENT_ATTENDANCE_RELIABILITY.md`.
- **Fixed: agent "confirmation loop"** — agent kept replying *"Would you like to mark attendance now?"* and never acted, even after "yes mark them" / "yes". → [[Agent confirmation flow — fix]]
- **Root cause:** LLM asked a *rhetorical* confirmation in prose (`finishReason="clarify"`) instead of emitting the tool call, so the real Confirm/Reject card never fired. The UI was fine all along; the agent never triggered it.
- **Fix:** prompt-level (`prompts/system.ts`) — forbid rhetorical confirmations, act on clear commands + affirmatives (incl. Hinglish "haan"/"kar do"), default date to today. Confirmation stays the structural `shouldConfirm → interrupt → /agent/confirm` gate.
- **Rejected** broadening the client's typed-"yes" matcher — it would false-confirm money/destructive actions. Tap-to-Confirm card stays the safe path. Reinforces [[Confirm before write — AI mutations need human approval]].
- 142 agent tests green; regression test in `prompts/system.test.ts`. Spec: `docs/AI/AGENT_CONFIRMATION_FLOW.md`.

## 2026-05-24
- **Shipped: Aadhaar eKYC** end-to-end (backend + web + mobile). Flagship trust feature; first step on the compliance moat. → [[Aadhaar eKYC — feature]], [[Aadhaar KYC — implementation]]
- **Decided:** KYC via a licensed sub-AUA/KUA provider behind a pluggable adapter; **Aadhaar is KYC-only, never login** (not a permitted purpose). Raw number never stored — masked + salted HMAC (one-per-business unique index) only. Explicit consent gated + persisted.
- Running on a **mock provider** today (OTP 123456, no creds); Cashfree adapter scaffolded. 31 tests green; repo spec at `docs/AADHAAR_KYC.md`.
- **Gated as "coming soon" in production** (greyed in UI) until a real provider is wired — mock must never verify real users in prod. Fully working in dev/test. Single flag `config.aadhaar.available`.
- **Set up the repo↔vault documentation protocol** so future AI sessions auto-document product/tech work here → [[Documentation protocol — repo ↔ vault]] (enforced from repo `CLAUDE.md`).
- **Blocked/next:** prod `prisma migrate deploy`; pick + wire real provider; confirm Cashfree field names; set dedicated `AADHAAR_HASH_SECRET`.

## 2026-05-23
- Set up second brain at `~/Documents/SecondBrain`
- Extracted full Arth Saathi knowledge graph into vault: ideology, enemy, market, competitors, GTM, business model, architecture, fundraising
- Source-of-truth pointers documented in [[Source-of-truth pointers — Arth Saathi]]
- Atomic notes created for 8 load-bearing beliefs (reusable across projects)

## Template entry
```
## YYYY-MM-DD
- What I shipped:
- What I decided:
- What's blocked:
- Next:
```

## Related
- [[Arth Saathi]]
