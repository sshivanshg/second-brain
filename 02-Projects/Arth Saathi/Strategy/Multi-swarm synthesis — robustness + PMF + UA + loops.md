---
created: 2026-06-06
type: synthesis
tags: arth-saathi, strategy, robustness, pmf, ua, growth-loops
repo_doc: docs/audits/MASTER_PLAN_2026-06-06.md
---

# Multi-swarm synthesis — robustness + PMF + UA + loops

> The thinking behind the master plan. The spec lives in the repo at `docs/audits/MASTER_PLAN_2026-06-06.md`. This note holds **why** we believe the four things below — and what we're choosing not to do.

## What we ran

Four parallel deep-thinking sub-agents on Opus, coordinated by four ruflo swarms (hierarchical-mesh for robustness, mesh for growth, star for micro-fit, hierarchical-raft hive for cross-swarm consensus). Each produced a real deliverable in the repo:

| WS | Repo doc | Lines |
| -- | -------- | ----: |
| Robustness | `docs/audits/ROBUSTNESS_AUDIT.md` | 159 |
| PMF | `docs/strategy/MICRO_BUSINESS_PMF.md` | 867 |
| UA | `docs/strategy/USER_ACQUISITION_PLAYBOOK.md` | 1,276 |
| Loops | `docs/strategy/GROWTH_LOOPS.md` | 1,184 |
| Synthesis | `docs/audits/MASTER_PLAN_2026-06-06.md` | this is the plan |

Total: ~3,500 lines of new strategy + audit material, all repo-canonical, all citing real files where surfaces exist.

## The three findings that surprised me

### 1. We're building the wrong app for the user we want

The PRD reads like a staff-management SaaS, the vision doc leads with GST, the dashboard surfaces both. But the cohort we *say* we want — the ~30–40M solo kirana / dukaan / chai-shop / mandi-trader long tail — is not staff-heavy and not GST-registered. They are **udhaar-heavy and WhatsApp-native.**

The current ICP in the vault — [[ICP — Priya the boutique owner]] — is real, but she is **Archetype C** in WS-2's segmentation: 5–30 staff, GST-on, Tier-2 metro. She is who Vyapar already wins. She is not the Bharat scale unlock.

The Bharat unlock is **Raju the solo kirana** — Tier-3, ₹20–80k MRR, 0–2 staff, Hindi-only reader, ₹6–10k Android, post-Khatabook-trauma. He doesn't need an ERP. He needs to send today's udhaar reminders to 12 customers on WhatsApp without typing.

This isn't a pivot — it's a re-prioritisation. Priya stays in the funnel. Raju goes first in the wedge.

### 2. The codebase is in better shape than the surface suggested

The robustness audit found 7 CRITICAL items, but it also found that the Razorpay webhook is hardened, the graceful-shutdown trap in `server/main.ts:261–321` is textbook, PII redaction is always-on at the Winston sink, invoice numbering is atomic, and backend `console.log` count is **zero**. Those are not the artefacts of a careless codebase.

So the move is not a hardening pause. It's a 2-week CRITICAL sprint, then growth-product runs in parallel with the HIGH/MEDIUM backlog. We don't owe an "audit phase before growth phase" model.

The seven CRITICALs to clear first:
1. Plaintext phone-OTP store → hash + attempts counter (`EmailOtpChallenge` already does this right; phone must mirror it).
2. No `withIdempotency` on `routes/billing.ts` + `routes/payments.ts` → duplicate paid invoice on web-dashboard retry.
3. Recurring-invoice cron has no `(scheduleId, runDate)` UNIQUE → duplicates on partial-failure retry.
4. Fastify CVE GHSA-mrq3-vjjr-p77c (body-validation bypass) → upgrade ≥5.7.3.
5–7. Three more in the WS-1 doc.

### 3. The growth move is shared-spine code, not channel spend

This was the cleanest finding. **The single highest-leverage 2-engineer-week piece of code in the next 90 days is the growth spine** — `ShortLinkService` + `GrowthAttribution` Prisma model + canvas card renderer + public landing routes + central `share-footer.ts` band. It unlocks **7 of 12 loops**.

Once it ships, three loops on top of it (WhatsApp invoice share, employee invite, GST filing-status share card) compound to portfolio k ≈ 0.84 and cut blended CAC ~46% on top of whatever paid we run. That's not the same thing as a viral exponential — k < 1 means we still need a paid feeder. But it changes the unit economics of WS-D paid acquisition from "expensive" to "compounding."

The compounding partner is **WS-E (CA partner programme)**. Every CA's clients install with the CA's logo on their share artifacts → CA-vanity-loop → CA brings the next 20 clients. B2B-data preconsent (the [[B2B data perspective — Arth Saathi]] thesis) flips to ~70% opt-in on CA-channel installs vs ~30% on paid — turning UA into the feeder for the dataset business, not a separate motion from it.

## What we're choosing not to do

- **Not migrating Winston.** It's correct. Logging is a known win — don't disturb it.
- **Not white-labelling for CAs in Y1.** Revenue share + admin view only until 1,000-CA cohort proves stickiness.
- **Not paying Meta CAC before LOOP-2 ships.** Every ₹95 we spend without share-artifacts is one missed multiplier. WS-D gates on LOOP-2 live in prod.
- **Not adding more onboarding fields.** The B2B-data thesis says every onboarding field is a tax on the asset; passive capture + transactional inference + JIT prompts beat eager schemas. (This is already the load-bearing belief from the 06-04 work.)
- **Not building voice-first before defer-auth-first.** Voice is in the top-5 bets but it depends on Sarvam/Bhashini and adds 2–3 weeks. Defer-auth + udhaar quick-entry + WA 1-tap is the same 90-second activation lift without the dependency. Voice ships after the wedge proves.

## What kills this plan

The five risks in the master plan are not equal. The two that would actually kill it:

1. **WS-D paid ramping before LOOP-2 is live in production.** This burns money on traffic that doesn't generate share-artifacts. The whole "blended CAC drops 46%" math evaporates. We need a literal gate: WS-D scale spend requires `GrowthAttribution` events from LOOP-2 in prod telemetry.
2. **The wedge fails Hindi-first usability testing.** If Raju can't ship a WhatsApp reminder in 90s, defer-auth alone won't save us. The cure is unglamorous: two Tier-3 shopkeepers in the room every Friday on B1–B3. Not surveys. Not prototypes. Real usage.

## Related

- Source deliverables (repo):
  - `docs/audits/ROBUSTNESS_AUDIT.md`
  - `docs/strategy/MICRO_BUSINESS_PMF.md`
  - `docs/strategy/USER_ACQUISITION_PLAYBOOK.md`
  - `docs/strategy/GROWTH_LOOPS.md`
  - `docs/audits/MASTER_PLAN_2026-06-06.md`
- Vault context this re-prioritises:
  - [[ICP — Priya the boutique owner]] — stays in funnel, no longer the wedge
  - [[B2B data perspective — Arth Saathi]] — CA-channel preconsent makes the dataset thesis cheaper to feed
  - [[GTM strategy — PLG + WhatsApp viral + CA partner]] — the synthesis ratifies and operationalises the GTM
  - [[Competitors — landscape and gaps]] — the kirana long tail is where the Vyapar/Khatabook/myBillBook moat is thinnest
  - [[Ideology — what we believe that competitors don't]] — defer-auth + Hindi-first + WhatsApp-as-UI is the ideology, expressed as code
- New load-bearing beliefs to atom-ise (follow-ups):
  - [[The growth spine is a 2-engineer-week investment that unlocks seven of twelve loops]]
  - [[Below k=1 still pays — sub-1 viral coefficient cuts blended CAC ~46% on a paid feeder]]
  - [[CA-channel preconsent is the cheapest B2B-data acquisition motion we have]]
