---
created: 2026-06-04
type: strategy
tags: arth-saathi, b2b-data, monetization, dpdp, onboarding
repo_doc: docs/strategy/b2b-data-perspective.md
---

# B2B data perspective — Arth Saathi

> The flip. We stopped thinking of Arth Saathi as a SaaS for micro-merchants
> and started thinking of the 60M-merchant cohort as **the dataset for
> India's informal economy**. The app is the acquisition channel; the
> dataset is the business.

## The load-bearing realisation

At 1M merchants we have a panel. At 10M we're the only source of truth. At
60M we ARE the dataset, and three buyer tiers (NBFC lenders, FMCG brands,
public-equity alt-data) will pay for it. Each merchant ARPU grows ~10×
from app subscription (₹0–₹200/yr) to data product economics
(₹1,000–₹3,000/yr at scale).

This is the structural moat. Anyone can build a billing app. Nobody can
leapfrog the dataset.

## What's different about Arth's data vs KhataBook / OkCredit

KhataBook raised $86M trying this exact play and stalled. The reason was
their data was *ledger-only* — names and amounts owed, nothing structured
about what was sold. Arth's data is **structurally richer**:

- HSN/SAC on every invoice line — FMCG sell-through inference becomes real,
  not a vibe.
- Inventory variants + batches — perishables visibility, waste rate.
- Purchase-side bills — buy/sell P&L reconstruction.
- B2B counterparty graph via `BillingCustomer.kind` (B2B_GSTIN vs B2C).
- Employees + Aadhaar KYC — first credible signal on informal labour.

KhataBook had names; we have a P&L. That's the wedge they didn't have.

## The onboarding inversion — "ask for nothing"

The instinct is "fields that matter for B2B → add to onboarding." Wrong.
Adding fields kills acquisition, which kills the dataset, which kills the
whole B2B play. The right frame:

> Onboarding is the trust event. Data capture is a 12-month flywheel.

Five channels do the work:

1. **GPS** at the natural moment (first "set up shop" tap, not signup).
2. **GSTIN cascade** — one optional field unlocks legalName, tradeName,
   pincode, NIC, registration date, constitutionOfBusiness, filing
   status.
3. **Transactional inference** — subCategory from HSN, turnover from
   invoices, hours from timestamps, B2B/B2C mix from
   `BillingCustomer.kind`.
4. **Brand inference** on existing inventory via a curated brand master.
5. **Just-in-time prompts** — PAN/bank/vintage/entityType only when the
   merchant taps "Apply for loan" (and only with their explicit consent).

The one thing we DO ask: a single skippable shop-type tile. Inference
takes 2-4 weeks to converge, and the merchant wants their app
personalised on day 1. But skip is always there.

## The DPDP-shaped hole

DPDP-2023 makes the difference between *aggregated* (sell freely) and
*named* (need consent) a 10× value gap. A loan API that requires named
merchant disclosure unlocks lender ARPU; without consent, you're stuck at
aggregated FMCG dashboards. Putting `ConsentGrant` in the schema **before**
the first B2B contract is the cheapest thing we can do — retrofitting
consent at 100K merchants is hell. Built it as purpose-scoped, revocable,
with `evidenceUrl` + `evidenceHash` so we can prove what the merchant saw
at grant time (a screenshot of the toggle / signed PDF). This is the cap
table for trust.

## What I'd push on next (loop candidates)

*(struck-through items shipped on the same-day autonomous loop pass — see
the working-log entry for that day.)*

- ~~**Market Pulse internal dashboard**~~ — shipped. Becomes the demo
  asset for the first design-partner pitch.
- ~~**GPS reverse-geocode**~~ — shipped. "Use my location" in mobile
  onboarding pre-fills pincode and persists lat/lng.
- ~~**Periodic job orchestration**~~ — shipped. Daily 03:30 IST cron
  refreshes HSN inference + turnover + brand backfill across 200 shops.
- ~~**Consent UX**~~ — shipped. Mobile + web Settings screens with one
  toggle per DPDP-2023 purpose; backend ConsentGrant with tamper-evident
  SHA-256 evidenceHash. (Evidence-URL screenshot upload still deferred.)

What's left:
- **GSP integration** behind `FilingStatusProvider` — the GSTR-1/3B
  schema, service, and quarterly cron are live with the `nullProvider`;
  swap it for a real GSP when the vendor is contracted.
- **Brand → sell-through join** — Market Pulse currently shows brand
  *shelf presence* (inventory rows). True sell-through needs an
  `InvoiceLineItem → InventoryItem` link or a `brandInferred` column on
  the line item itself.
- **First design-partner pitch** — take Market Pulse + the panel-coverage
  numbers to one FMCG category head and one NBFC credit head. Drives the
  external dashboard v0 spec.

## Where to look

- Repo spec: `docs/strategy/b2b-data-perspective.md`
- Migration: `apps/backend/prisma/migrations/20260604120000_b2b_data_foundation/`
- Branch: `explore/b2b-data-strategy`

## Related

- [[Ideology — what we believe that competitors don't]]
- [[Competitors — landscape and gaps]]
- [[Market — TAM SAM SOM and growth]]
- [[Inflection triggers — why now]]
- [[Mobile sync cost — manifest-gated fast path]] — why a local-first
  shape is what makes the data layer cost-feasible in the first place
