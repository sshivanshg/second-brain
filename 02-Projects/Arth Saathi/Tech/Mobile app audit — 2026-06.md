---
created: 2026-06-09
type: tech
tags: arth-saathi, mobile, audit, sync, security
repo_doc: docs/audits/MOBILE_APP_AUDIT_2026-06-09.md
---

# Mobile app audit — 2026-06

Seven parallel principal-engineer reviews over `apps/mobile` (architecture, security, offline sync, data layer, performance, reliability, build/release), every finding verified against code with file:line citations. The full spec lives in the repo doc; this note is the *shape* of what was found.

## The three themes that matter

**1. One path can destroy a shopkeeper's unsynced data.** A network blip during token refresh is treated as auth failure → forced logout → `unsafeResetDatabase()` wipes the mutation queue of offline writes. On 2G/4G this is routine, not edge-case, and it defeats the whole offline-first bet. This is the single highest-priority fix in the codebase (C1).

**2. The sync protocol has quiet correctness holes.** A cross-tenant write hole in the attendance push endpoint (no tenancy check — khata and leaves do it right, attendance forgot); a multi-table cursor bug that advances to MAX across tables and permanently skips rows on the initial sync of any established shop; `serverTime`-as-cursor races; equal-timestamp page boundaries. The orchestrator itself is genuinely production-grade — the holes are in the protocol details, which is the classic sync failure mode: the scaffolding looks done, so nobody re-derives the cursor math.

**3. The release pipeline would make a prod incident undiagnosable.** Sentry source-map upload is disabled in every EAS profile; no release/dist tags from expo-updates; and `eas update` doesn't pass `--environment`, so an OTA publish from a laptop bakes the laptop's `.env` into production (empty Sentry DSN, missing feature flags). None of this hurts until the first bad OTA — then all of it hurts at once.

Also load-bearing: the signed-out udhaar wedge has **no path to sign-in** (fresh installs and reinstalls are strandable), and offline mutations have no local echo outside billing — saved attendance visibly reverts on screen while showing "saved."

## What the audit refused to regress

Token storage (SecureStore-only, single-flight refresh dedup), the three-layer logout purge, the manifest-gated sync fast path ([[Mobile sync cost — manifest-gated fast path]]), zod-on-every-response via contracts, zero circular deps, `tsc` clean under strict. The foundations are right; the findings are about the seams.

## Beliefs this reinforces

- A sync engine's risk lives in the cursor math and tenancy checks, not the orchestration — the parts that look like plumbing.
- Offline-first dies by its error-handling defaults: any code path that equates "network failed" with "user is unauthorized" will eventually delete data.
- Observability gaps are invisible until the exact moment they're unaffordable — source maps and release tags are pre-paid insurance.

## Housekeeping done in the same pass

Deleted 30 stale docs (executed plans, superseded audits, dead design docs like the never-built Mongo chat offload, duplicate .docx exports) and fixed every inbound link, including two code comments. `docs/audits/MOBILE_APP_AUDIT_2026-06-09.md` is now the mobile audit of record; it supersedes `MOBILE_OFFLINE_SYNC_AUDIT.md` for sync findings.

## Related
- [[Mobile sync cost — manifest-gated fast path]]
- [[Mobile ↔ web UI parity — sweep]]
- [[Architecture — Arth Saathi]]
- [[Play Store — Data safety & launch compliance map]]
- [[A green deploy is not a live deploy — verify the running revision]]
