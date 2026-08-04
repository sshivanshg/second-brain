---
created: 2026-08-04
updated: 2026-08-04
status: active
type: project
tags: project, sales, leads, outreach, freelance
categories: [backend]
audience: [freelance, startup]
repo: /Users/shivanshgupta/Projects/brixloop-leads
github: https://github.com/sshivanshg/brixloop-leads
---

# 🎯 Brixloop Leads

> **One-liner:** Geo lead pipeline for Brixloop studio — scrape Google Places via Apify → qualify (no site / rebuild / automation) → enrich → email/WhatsApp outreach.

**Repo:** `/Users/shivanshgupta/Projects/brixloop-leads` · **GitHub:** [sshivanshg/brixloop-leads](https://github.com/sshivanshg/brixloop-leads)

## Pipeline
1. **Scrape** — Apify `compass/crawler-google-places` (~$4/1k)
2. **Qualify** — missing website → easiest sell; Facebook-only; Lighthouse fail → rebuild; service biz → automation high ticket
3. **Enrich** — site crawl for owner email
4. **Outreach** — `python3 -m outreach.send` (Resend email / Twilio WhatsApp); dry-run default; log `outreach/sent-log.csv`

## 📝 Log
### 2026-06-10
- First commit; outreach sender built; geo scrape checkboxes in README

### 2026-08-04 — vault catch-up
- Studio GTM arm of [[Brixloop]]

## ✅ Open tasks
- [ ] Authenticate Apify MCP / run three geo scrapes
- [ ] Qualify + enrich → `leads/` CSVs
- [ ] Live outreach with domain-verified Resend + Meta WA templates

## 🔗 Connections
- [[Brixloop]] · [[Projects MOC]]
