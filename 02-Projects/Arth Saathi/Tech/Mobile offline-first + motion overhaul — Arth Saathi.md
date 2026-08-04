---
created: 2026-06-11
type: architecture
status: implemented
tags: arth-saathi, tech, mobile, offline-first, motion, design-system, a11y
repo_doc: /Users/shivanshgupta/projects/arth/docs/audits/MOBILE_UIUX_OVERHAUL_2026-06-10.md
---

# Mobile offline-first + motion overhaul — Arth Saathi

> Thinking + map. **Repo is truth:** `docs/audits/MOBILE_UIUX_OVERHAUL_2026-06-10.md` is the canonical audit + 5-wave plan + outcome. This note is the orientation layer — *why* it was sequenced this way and the non-obvious calls. Builds directly on the [[Mobile app audit — 2026-06]] (data-correctness fixes) and [[Mobile ↔ web UI parity — sweep]].

## What was actually wrong
The founder's ask was sweeping — "fix the complete UI/UX end-to-end, make it work offline-first, nothing hidden or dysfunctional, good animations." Underneath that, four distinct problem classes, and they are **not** equally important:

1. **Offline-first had a hole exactly where money lives.** Sync/data correctness (the C1–C8 fixes from the June audit) was already solid, but the single most common payroll actions — record a salary **advance**, add a **bonus**, edit/delete them — went straight to `api.post` and were *hard-disabled when offline*. A shopkeeper on flaky village 4G literally could not record the advance they just handed out. Everything else (khata, billing, attendance) already queued; payroll was the gap.
2. **Complete features were unreachable.** A working App-lock (PIN) screen with no navigation to it; a real Help centre rendered as an engineer placeholder; notification taps pushing raw *web* hrefs into non-existent Expo routes; a Today/Roster toggle that was a dead control on legacy routes.
3. **No systematic motion or haptics**, and shared primitives still emitted **synthetic fake-bold** (set `fontWeight` on `Inter_400Regular` instead of using the real `Inter_600` face) — screens had been migrated, the primitives hadn't.
4. **No user control over sync** — no pause, no Wi-Fi-only, no way to see or recover a write that permanently failed.

## The sequencing decision (why offline-first FIRST)
Five waves, explicitly ordered so **correctness precedes polish**: W1 offline payroll → W2 offline controls → W3 dysfunction → W4 motion foundation → W5 rollout + parity/a11y. The founder confirmed this order. The logic: a beautiful animation on a payroll write that silently drops offline is worse than no animation — fix the money path first, make it controllable, *then* make it feel good.

## The non-obvious calls
- **Optimistic payroll writes can't naively match by `server_id`.** The pull engine reconciles rows by `server_id` only. An offline-created advance has no server id yet, so a naive optimistic create would *duplicate* the moment the server row arrives. The fix: write the optimistic row with `server_id = clientId` + `localStatus:"pending"`, enqueue a profile mutation, and on push success **reconcile `clientId → real id`** so the pending row *becomes* the server row instead of colliding. Advance dedups by `clientId`; bonus is natural-key idempotent `(employee, month, year)`; deletes are idempotent. → backend `upsertEmployeeBonus` had to start *returning* the row so the push could reconcile it.
- **List entrance animations are a trap on virtualized lists.** Every long list is a FlashList (v2), which **recycles** rows — so per-row `entering={FadeInDown}` re-fires every time a row scrolls into a recycled view, which reads as a bug, not polish. So per-row stagger was applied **only** to the bounded, non-virtualized dashboard activity feed (≤8 items in a `.map`); the big screens get a screen-level slide transition and skeletons instead. This is the kind of thing that *needs* on-device eyes — which is why it wasn't blanket-applied.
- **Haptics must never throw or block.** `lib/haptics.ts` wraps `expo-haptics` so a device with no haptic engine (or web) silently no-ops, and nothing is awaited — the visual fires on the same frame regardless.
- **Everything motion gates on `useReducedMotion()`.** One hook reading the OS "reduce motion" setting; press-scale falls back to opacity, count-ups jump to the value, transitions cut. A real-money tool must respect that accessibility preference.
- **`font-heading` is already Plus Jakarta *Bold*** — so `font-heading font-bold` was redundant fake-bold-on-top; heading combos collapse to `font-heading` / `font-heading-semibold`, body weights move to the real `font-sans-{medium,semibold,bold}` Inter faces.

## What the user actually gets
- **Offline payroll** that saves on-device and syncs on reconnect, with a "Not synced yet" pill so the owner can *see* what's still pending. → [[Salary advance — feature]]
- A **Sync & backup screen** (More → Sync & backup): pause all sync, Wi-Fi-only (save mobile data), 15-min / hourly / 6-hr / manual cadence, per-data-type opt-out, manual "Sync now", last-synced, and a **failed-changes recovery** list (retry / discard dead-lettered mutations). This is genuinely new product surface, not just plumbing.
- Reachable App-lock, a real in-app Help centre (SSO hand-off to web), working notification deep-links, loading skeletons instead of "empty ledger" flashes, honest offline copy.
- A **motion layer** — press-scale + haptics on every button/card, count-up money totals, toasts, staggered/faded entrances — all reduced-motion aware.

## Deliberately deferred (not regressions)
List-entrance on the virtualized screens (Stack slide transition instead), full skeleton rollout of the remaining `ActivityIndicator`s, per-mutation haptics on save/delete/payment/pull-to-refresh, and AnimatedNumber on cashbook/expense balances — all low-risk follow-ons best done with on-device screenshot verification (the verification path that was set up but not run this session).

## Getting it onto the store (two build bugs the cloud hid)
The first production build (vc19) **errored on EAS cloud** with an opaque `EAS_BUILD_UNKNOWN_GRADLE_ERROR`. Building **locally** (founder's call) surfaced the real gradle output and two genuine repo bugs — both now fixed on `main` and both of which would have bitten the cloud too:
- **`brace-expansion` override broke RN codegen.** An unbounded pnpm override `>=5.0.6` (for ReDoS CVE-2025-5889) rode glob's v2 dependency up to a breaking major (v5 is ESM-restructured), so glob's `__importDefault(...).default` was `undefined` → `generateCodegenSchemaFromJavaScript` failed for every native module. Fixed by scoping per-major (`@1: ^1.1.12`, `@2: ^2.0.2`). *Lesson: an unbounded security `>=` silently climbs to the next breaking major — pin per-major.*
- **Release build OOMed on JVM Metaspace** (`kspReleaseKotlin` + lint). The Expo template's `MaxMetaspaceSize=512m` is too small for this app's new-arch/KSP/native surface — and it's a JVM limit independent of host RAM. Because `android/` is managed (CNG, regenerated by `expo prebuild`), the fix is a **config plugin** (`plugins/with-android-build-memory.js`): 1 GB Metaspace for *both* the Gradle and Kotlin daemons (KSP runs in the Kotlin daemon), serialized workers, lint-off-on-release. *Lesson: CNG gradle memory must live in a config plugin, never a tracked `gradle.properties`.*

Outcome: `BUILD SUCCESSFUL`, a 70 MB **versionCode-22** AAB built locally. Play upload succeeded; the production submit then hit `Precondition check failed` — root cause (per Console, not the API): this **new personal developer account** is policy-gated out of production until a **≥12-tester closed test runs ≥14 continuous days** + an access review (was at day 7/14). So vc22 shipped to the **Alpha closed-testing track** instead — gets the overhaul to the 12 testers *and* keeps the 14-day clock running. Promote to production at ~day 14 (~2026-06-18) using `docs/compliance/DATA_SAFETY.md` + `marketing/creatives/playstore/`. *Lesson: a new Play developer account cannot ship to production on day one — the 14-day/12-tester closed-test gate is the real long pole, not the build.*

## Shape of the work
14 commits on `feat/mobile-uiux-overhaul` → fast-forwarded into `main` and pushed. Each commit: mobile `tsc` clean, eslint clean, 46 vitest tests green (the engine's pause/manual/throttle/force matrix is locked by a pure `preNetworkSkipReason` unit test, mirroring how `role-steps` isolates the role gate). Reusable motion primitives live under `components/ui/motion/`; the sync-preferences store (`lib/db/sync/preferences.ts`) mirrors the `local-shop` cached-singleton pattern so the engine reads prefs synchronously outside React.

## Related
- [[Arth Saathi]]
- [[Mobile app audit — 2026-06]]
- [[Mobile ↔ web UI parity — sweep]]
- [[Mobile sync cost — manifest-gated fast path]]
- [[Salary advance — feature]]
- [[Architecture — Arth Saathi]]
- [[Working log — Arth Saathi]]
