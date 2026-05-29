---
created: 2026-05-27
type: product
status: built — web (owner) + mobile (owner + employee)
tags: arth-saathi, features, onboarding, ux
repo_doc: /Users/shivanshgupta/Documents/arth/docs/WALKTHROUGH.md
---

# First-run walkthrough — feature

> **One-liner:** a 60-second guided tour that takes a brand-new user by the hand on first login — *welcome → spotlight each feature → leave behind a getting-started checklist* — so a shop owner who has never seen the app knows what it does before they bounce.

**Status:** built end-to-end. Web covers the Owner dashboard; mobile covers both Owner and Employee. Skippable, re-launchable, shows once per user. Repo truth: `docs/WALKTHROUGH.md`.

## Why this matters
- Our user is a non-technical [[Bharat shop owner IS the HR department|Bharat shop owner]] on a 6-inch phone. The cost of "I opened it, didn't get it, closed it" is a churned pilot. The first session has to *teach itself*.
- It is **not** the [[Feature catalog|business-setup wizard]] (business type, working days, leave policy) — that configures the account. This orients the human. Two different jobs; both happen early, easy to conflate. Keeping them separate means setup stays a form and discovery stays delightful.
- Reinforces [[UX principles — Arth Saathi]]: the app should be legible without a manual. A guided tour is the manual collapsing into the product.

## The shape, and why hybrid
Offered four styles; chose **all three combined** (welcome + spotlight + checklist) deliberately:
- **Welcome modal** sets context and gives an honest *Skip* — respect the user who already knows what they want.
- **Spotlight** is the part that earns the name "walkthrough mode" — it points at the *real* nav, not a slideshow of screenshots, so muscle memory forms on the actual buttons.
- **Checklist** is the long tail: a one-shot tour is forgotten in a day, but a dashboard card that says "2 of 5 done" pulls the user back to the features they skipped. Nudge over time, not one blocking gate.

## The one hard design problem: responsive targeting
Desktop and mobile are *different navigations* — a left **sidebar** on web-desktop, a **bottom nav** on web-mobile and on the app. A naïve tour hard-codes one and breaks the other.

The trick (web): each step carries **both** candidate selectors and the engine spotlights the *first visible one*. The hidden layout's element measures `0×0`, so "first non-zero box wins" picks the right target at every breakpoint with zero branching. One step definition, both layouts. (Mobile measures the live nav tabs via `measureInWindow` into a store.)

This is the load-bearing idea — see [[Aesthetic is sacred — improve, don't disrupt]]: the tour reuses the real UI and existing `ds-*` tokens / brand greens, so it never invents a parallel visual world.

## Personas
- **Owner** (web + mobile): dashboard → team → shop (billing/khata/inventory/salary) → AI assistant → settings/help.
- **Employee** (mobile): home → mark attendance → salary → profile/leaves. The employee's whole relationship with the app is 4 taps; the tour makes all 4 obvious on day one.

## State & honesty
Completion is **per-user, device-local** (`localStorage` / `AsyncStorage`, keyed by user id) — a shared shop phone shows the tour once per person, nothing leaks across accounts, nothing to wipe on logout. It is a UI nudge, not business data; promote to a server flag only if cross-device sync ever matters. Ties to the standing rule in [[PWA session — never re-enable NextAuth client refetch|session handling]]: client-only state that can't manufacture a wrong server conclusion.

## Open threads / next
- Web employee tour (employees are currently routed to the native app).
- Localise tour copy (Hindi/Hinglish) — currently English; should ride the existing i18n catalogue. Connects to [[Voice and Hinglish are the UX not a translation]].
- Consider a voice-narrated tour once the voice agent lands.

## Related
- [[Arth Saathi]]
- [[UX principles — Arth Saathi]]
- [[Feature catalog]]
- [[Aesthetic is sacred — improve, don't disrupt]]
- [[Bharat shop owner IS the HR department]]
