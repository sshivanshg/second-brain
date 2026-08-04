---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, freelance, airtable, softr, creator
categories: [fullstack, frontend]
audience: [freelance]
repo: /Users/shivanshgupta/Projects/ditchla-creator-portal
github: https://github.com/sshivanshg/ditchla-creator-portal
---

# 👖 Ditch LA Creator Portal

> **One-liner:** Multi-brand creator portal workspace (DITCH LA + Baggy) — Airtable schema/scripts, Softr UX, Make automation, and **session-logged** ops work.

**Repo:** `/Users/shivanshgupta/Projects/ditchla-creator-portal` · **GitHub:** [sshivanshg/ditchla-creator-portal](https://github.com/sshivanshg/ditchla-creator-portal)

## 🎯 Goal
Let creators manage profiles, PR packages, submissions, and auto-calculated payouts; admins stay in Airtable; portals via Softr. Scale to sister brands without mixing data.

## 🧱 Stack / shape
```
brands/ditch-la/   # live portal + full history + session logs
brands/baggy/      # sister brand (sessions from Jul 17–28)
.cursor/hooks      # active-brand switch + session logging
```
- **Airtable** base(s) + Node setup/verify scripts  
- **Softr** portal pages  
- **Make** blueprints (per brand)  
- Cursor hooks write `brands/<brand>/docs/sessions/*.md`

## 📝 Log (from session files)
### 2026-07-10 — Auto payout rates
- Formula-driven **Calculated Payout**; `setup_payout_rates.mjs` + `sync-payouts`
- Export uses Calculated Payout as Amount Earned
- Verified test creator: $5k earned / $0 paid / $5k outstanding
- Human follow-up: rates on ~14 creators still missing

### 2026-07-14 — Creator profiles + PR packages
- Live base: Creators profile fields + **PR Packages** table
- CLI `log_pr_package.mjs`; BUILD guides + decisions log #10
- Human: Soft `/profile` + `/packages`; Email/Name lookups on PR Packages

### 2026-07-16–17 — James product feedback
- Sweatpants ≠ denim sizes (letter sizes vs waist×inseam)
- Shipping address last in form order
- Multi-brand **repo reorg** (`a121d8e`, `42ec100`)

### 2026-07-17 → 2026-07-28 — Baggy brand
- Baggy session logs under `brands/baggy/docs/sessions/` (portal setup sessions)

### 2026-08-04 — vault catch-up
- Last push: multi-brand reorg session log. Local: README/AGENTS dirty.

## ✅ Open tasks
- [ ] Soft field swaps for denim/sweatpants if not published everywhere
- [ ] Baggy Airtable + Softr parity with DITCH patterns
- [ ] Finish rates backfill for remaining creators

## 📚 Sources
- Session dir: `brands/ditch-la/docs/sessions/`
- Repo README multi-brand model

## 🔗 Connections
- [[Boutique web projects]] · [[Projects MOC]]
- Freelance / client delivery pattern
