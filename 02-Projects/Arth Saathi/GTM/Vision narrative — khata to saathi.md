---
created: 2026-05-28
type: product
tags: arth-saathi, gtm, vision, narrative
repo_doc: /Users/shivanshgupta/Documents/arth/docs/product/Arth_Saathi_Vision.md
---

# Vision narrative — khata to saathi

> **What this is:** the non-technical, business-angle telling of the *whole* product — written so a non-engineer (investor, partner, family, new joiner) gets *what we do and why it matters* in one read. Repo-canonical version: `docs/product/Arth_Saathi_Vision.md`.

## Why this doc exists
Every other internal doc ([[Feature catalog]], `docs/FEATURES_ECOSYSTEM.md`) is written for builders — it lists routes, gates, and dependencies. None of them hand cleanly to a **non-tech person** who needs the *business story*. This fills that gap: zero jargon, every feature translated into owner-felt value, framed end-to-end as a narrative.

## The spine: *khata → saathi*
The whole pitch hangs on one image the audience already owns — the shopkeeper's **bahi-khata** (handwritten register). It's the heart of the business *and* the source of every month-end fight. We don't describe "an HR SaaS"; we say **we replace the notebook**, and the upgrade is that you can now *talk* to it. That framing does two jobs:
1. It makes the problem instantly legible to someone who's never bought software.
2. It names the moat in the title — the notebook becomes a *saathi* (companion). That's the brand promise, not a tagline bolted on.

## The argument, compressed
- **Problem:** the register is opaque → salary disputes, no cost visibility, hours of manual maths, *and* money scattered across separate notebooks/apps (customer udhaar, supplier dues, expenses, stock, billing).
- **Solution:** one app where the owner marks attendance, approves leave, sees auto-calculated salary, raises GST invoices, **keeps the customer/supplier khata, tracks inventory + expenses + payments, and reads real P&L/cash-flow** — and can do *all of it by talking in plain English*, with a Confirm tap before anything is written.
- **Why we win:** *talk, don't learn* (the AI is the moat — no training needed); one shared source of truth kills disputes; built natively for India (₹, GST, Indian working-day logic); and it bundles **full Vyapar-class commerce *plus* the staff side they lack *plus* the AI none of them have** into one app.

## Scope correction (2026-05-28)
The first draft of the repo doc undersold the product — it was written off an older features reference and covered only staff/salary + GST billing. A code audit surfaced a whole **Vyapar-parity commerce layer** already built: khata (Party/LedgerEntry — customer & supplier udhaar), inventory (items/categories/batches/movements), payments in/out, expenses + other income, purchase bills, purchase/sales orders, e-invoicing + e-way bill (mock until NIC creds), accounting reports (day book, P&L, balance sheet, cash flow), Tally export, manufacturing/BOM, multi-firm, RBAC, bulk import, and a dues-reminder engine. Many have full backends with web UI **rolling out** — the vision doc now labels those honestly ("engine live, screens rolling out") rather than omitting them. Lesson for future docs: **trust the code, not an older summary doc** — `docs/FEATURES_ECOSYSTEM.md` itself still lags reality and needs the same treatment.

## Positioning calls baked in
- **Lead with the AI assistant as the headline differentiator**, not as a feature bullet. The whole "usable by someone who's never touched software" claim rests on conversation-as-interface. Reinforces [[Voice and Hinglish are the UX not a translation]].
- **Safety is a selling point, not fine print** — "the AI never writes without Confirm" is stated up front because trust is the adoption barrier for this audience. Reinforces [[Confirm before write — AI mutations need human approval]].
- **Free tier framed as "feel the value, no risk"**, Growth (₹499) as "what a serious shop needs." Mirrors [[Pricing tiers]] / [[Business model — subscription + embedded fintech]] — and the free-but-not-free-forever logic of [[Free + free = no revenue]].
- **GST billing pitched as "replaces separate software"** — a cost-saving the owner feels, not a compliance chore.
- **The vision close** points at the platform thesis — one dead-simple app the shop opens every morning — which is where [[Daily-active surfaces own monthly-active surfaces]] and [[Vertical SMB-OS earns most revenue from embedded fintech]] live.

## How to use it
Hand it to: prospective hires who aren't engineers, non-technical investors/advisors, partner CAs, family. **Not** a substitute for the PRD or the pitch deck — it's the "explain it to me like I run a shop, not a fund" layer that sits *under* both.

## Related
- [[Arth Saathi]]
- [[Feature catalog]]
- [[ICP — Priya the boutique owner]]
- [[Pricing tiers]]
- [[Business model — subscription + embedded fintech]]
- [[GTM strategy — PLG + WhatsApp viral + CA partner]]
- [[Voice and Hinglish are the UX not a translation]]
- [[Confirm before write — AI mutations need human approval]]
- [[Source-of-truth pointers — Arth Saathi]]
