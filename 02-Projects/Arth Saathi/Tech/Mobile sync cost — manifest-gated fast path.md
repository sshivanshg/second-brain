---
created: 2026-06-04
type: architecture
status: implemented
tags: arth-saathi, tech, mobile, sync, offline, performance, cost
repo_doc: /Users/shivanshgupta/projects/arth/docs/architecture/mobile-sync-manifest.md
---

# Mobile sync cost — manifest-gated fast path

> Thinking + map. **Repo is truth:** `docs/architecture/mobile-sync-manifest.md` is the canonical spec; this is the orientation layer. Sits on top of the local-first engine described in [[Architecture — Arth Saathi]] and built on [[Offline-first sync needs tombstones, not hard deletes]].

## The question that started it
"We sync the mobile app with our database — that feels cost-inefficient and bad for the DB. Should we store invoices on-device differently, or stop syncing?"

Two worries were bundled together, and they needed splitting before anything sensible could be said:
1. **"Too much data on the device."**
2. **"The sync is expensive / bad for the database."**

## What the code actually showed (premise correction)
- **Worry 1 is mostly a non-issue at our scale.** A shop's khata + invoices + customers are a few **MB of text rows**; SQLite eats that. And crucially — **binary files are not in SQLite and not in the sync payloads.** Receipt photos, invoice PDFs, attendance selfies all live in object storage (S3/R2) and travel as **URL strings** (`photo_url`, `bill_photo_url`, `attachment_url`, `pdf_storage_key`). So the thing that *would* bloat a cheap Android phone isn't being synced in the first place.
- **Worry 2 was real but mis-located.** The cost driver isn't row volume, it's **empty-sync frequency**. `syncAll()` fired on every foreground/boot and pulled **all 8 entities every time, even when nothing changed**. Open the app 30×/day → ~240 near-empty round trips/device/day.
- **The tempting "fix" (go online-only / thin client) is a trap** — it would *increase* DB load (every screen read hits Postgres, no local cache) and break offline, which is the whole point for a patchy-connectivity Bharat user. [[Daily-active surfaces own monthly-active surfaces]] — and the daily surface here is often offline.

## The non-obvious lever
Don't touch the architecture. Cut the **frequency of empty work**:
- **Throttle** the high-frequency foreground path (≤1 sync / 60s).
- **Gate** each pull behind a single cheap probe — `GET /mobile/sync/manifest` returns `MAX(updatedAt)` per entity (index-tip read, tenant-scoped). Pull only the entities whose max moved past the local cursor. **Nothing changed → one probe, zero pulls** (was 8 pulls).

## The design decision worth remembering
**Manifest = fast path; periodic full sweep = correctness backstop.** I deliberately kept the probe *cheap* by excluding low-churn reference tables that lack a `(businessId, updatedAt)` index (categories, payment instruments, line items). That risks a reference-table edit not propagating instantly — so the **hourly background full sweep** (which ignores the manifest) catches it within the hour. This buys the whole optimisation **without a production index migration**, which matters because the cost was *"high bill, gut feeling — not measured."* You don't ship a risky prod migration to fix an unmeasured problem.

That's also why I added **instrumentation** (Sentry breadcrumb + `lastRun` in the sync store): the next decision — is the bigger multiplex refactor or an index migration worth it? — should be made against numbers, not vibes.

## Load-bearing lesson
**A local-first sync that feels expensive is usually a *frequency* problem, not a *storage* or *architecture* problem.** The instinct to "store data differently" or "stop syncing" attacks the wrong layer. Measure empty round-trips first; the cheapest win is almost always "ask *whether* to sync before *doing* the sync."

## Related
- [[Architecture — Arth Saathi]]
- [[Offline-first sync needs tombstones, not hard deletes]]
- [[Monorepo layout — Arth Saathi]]
- [[Daily-active surfaces own monthly-active surfaces]]
- [[Session persistence — mobile PWA stays logged in]]
